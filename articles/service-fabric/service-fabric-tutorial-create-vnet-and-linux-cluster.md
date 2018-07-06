---
title: Create a Linux Service Fabric cluster in Azure | Microsoft Docs
description: In this tutorial, you learn how to deploy a Linux Service Fabric cluster into an existing Azure virtual network using Azure CLI.
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: ''

ms.assetid:
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/22/2018
ms.author: ryanwi
ms.custom: mvc

---
# Tutorial: Deploy a Linux Service Fabric cluster into an Azure virtual network

This tutorial is part one of a series. You will learn how to deploy a Linux Service Fabric cluster into an [Azure virtual network (VNET)](../virtual-network/virtual-networks-overview.md) and [network security group (NSG)](../virtual-network/virtual-networks-nsg.md) using Azure CLI and a template. When you're finished, you have a cluster running in the cloud that you can deploy applications to. To create a Windows cluster using PowerShell, see [Create a secure Windows cluster on Azure](service-fabric-tutorial-create-vnet-and-windows-cluster.md).

In this tutorial, you learn how to:

> [!div class="checklist"]
> * Create a VNET in Azure using Azure CLI
> * Create a secure Service Fabric cluster in Azure using Azure CLI
> * Secure the cluster with an X.509 certificate
> * Connect to the cluster using Service Fabric CLI
> * Remove a cluster

In this tutorial series you learn how to:
> [!div class="checklist"]
> * Create a secure cluster on Azure
> * [Scale a cluster in or out](service-fabric-tutorial-scale-cluster.md)
> * [Upgrade the runtime of a cluster](service-fabric-tutorial-upgrade-cluster.md)
> * [Deploy API Management with Service Fabric](service-fabric-tutorial-deploy-api-management.md)

## Prerequisites

Before you begin this tutorial:

* If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
* Install the [Service Fabric CLI](service-fabric-cli.md)
* Install the [Azure CLI 2.0](/cli/azure/install-azure-cli)

The following procedures create a five-node Service Fabric cluster. To calculate cost incurred by running a Service Fabric cluster in Azure use the [Azure Pricing Calculator](https://azure.microsoft.com/pricing/calculator/).

## Key concepts

A [Service Fabric cluster](service-fabric-deploy-anywhere.md) is a network-connected set of virtual or physical machines into which your microservices are deployed and managed. Clusters can scale to thousands of machines. A machine or VM that is part of a cluster is called a node. Each node is assigned a node name (a string). Nodes have characteristics such as placement properties.

A node type defines the size, number, and properties for a set of virtual machines in the cluster. Every defined node type is set up as a [virtual machine scale set](/azure/virtual-machine-scale-sets/), an Azure compute resource you use to deploy and manage a collection of virtual machines as a set. Each node type can then be scaled up or down independently, have different sets of ports open, and can have different capacity metrics. Node types are used to define roles for a set of cluster nodes, such as "front end" or "back end".  Your cluster can have more than one node type, but the primary node type must have at least five VMs for production clusters (or at least three VMs for test clusters).  [Service Fabric system services](service-fabric-technical-overview.md#system-services) are placed on the nodes of the primary node type.

The cluster is secured with a cluster certificate. A cluster certificate is an X.509 certificate used to secure node-to-node communication and authenticate the cluster management endpoints to a management client.  The cluster certificate also provides an SSL for the HTTPS management API and for Service Fabric Explorer over HTTPS. Self signed certificates are useful for test clusters.  For production clusters, use a certificate from a certificate authority (CA) as the cluster certificate.

The cluster certificate must:

* contain a private key.
* be created for key exchange, which is exportable to a Personal Information Exchange (.pfx) file.
* have a subject name that matches the domain that you use to access the Service Fabric cluster. This matching is required to provide SSL for the cluster's HTTPS management endpoints and Service Fabric Explorer. You cannot obtain an SSL certificate from a certificate authority (CA) for the .cloudapp.azure.com domain. You must obtain a custom domain name for your cluster. When you request a certificate from a CA, the certificate's subject name must match the custom domain name that you use for your cluster.

Azure Key Vault is used to manage certificates for Service Fabric clusters in Azure.  When a cluster is deployed in Azure, the Azure resource provider responsible for creating Service Fabric clusters pulls certificates from Key Vault and installs them on the cluster VMs.

This tutorial deploys a cluster with five nodes in a single node type. For any production cluster deployment, however, [capacity planning](service-fabric-cluster-capacity.md) is an important step. Here are some things to consider as a part of that process.

* The number of nodes and node types that your cluster needs
* The properties of each of node type (for example size, primary, internet facing, and number of VMs)
* The reliability and durability characteristics of the cluster

## Download and explore the template

Download the following Resource Manager template files:

* [vnet-linuxcluster.json][template]
* [vnet-linuxcluster.parameters.json][parameters]

The [vnet-linuxcluster.json][template] deploys a number resources, including the following.

### Service Fabric cluster

A Linux cluster is deployed with the following characteristics:

* a single node type
* five nodes in the primary node type (configurable in the template parameters)
* OS: Ubuntu 16.04 LTS (configurable in the template parameters)
* certificate secured (configurable in the template parameters)
* [DNS service](service-fabric-dnsservice.md) is enabled
* [Durability level](service-fabric-cluster-capacity.md#the-durability-characteristics-of-the-cluster) of Bronze (configurable in the template parameters)
* [Reliability level](service-fabric-cluster-capacity.md#the-reliability-characteristics-of-the-cluster) of Silver (configurable in the template parameters)
* client connection endpoint: 19000 (configurable in the template parameters)
* HTTP gateway endpoint: 19080 (configurable in the template parameters)

### Azure load balancer

A load balancer is deployed and probes and rules setup for the following ports:

* client connection endpoint: 19000
* HTTP gateway endpoint: 19080
* application port: 80
* application port: 443

### Virtual network, subnet, and network security group

The names of the virtual network, subnet, and network security group are declared in the template parameters.  Address spaces of the virtual network and subnet are also declared in the template parameters:

* virtual network address space: 10.0.0.0/16
* Service Fabric subnet address space: 10.0.2.0/24

The following inbound traffic rules are enabled in the network security group. You can change the port values by changing the template variables.

* ClientConnectionEndpoint (TCP): 19000
* HttpGatewayEndpoint (HTTP/TCP): 19080
* SMB : 445
* Internodecommunication - 1025, 1026, 1027
* Ephemeral port range – 49152 to 65534 (need a min of 256 ports )
* Ports for application use: 80 and 443
* Application port range – 49152 to 65534 (used for service to service communication and unlike are not opened on the Load balancer )
* Block all other ports

If any other application ports are needed, then you will need to adjust the Microsoft.Network/loadBalancers resource and the Microsoft.Network/networkSecurityGroups resource to allow the traffic in.

## Set template parameters

The [vnet-cluster.parameters.json][parameters] parameters file declares many values used to deploy the cluster and associated resources. Some of the parameters that you might need to modify for your deployment:

|Parameter|Example value|Notes|
|---|---||
|adminUserName|vmadmin| Admin username for the cluster VMs. |
|adminPassword|Password#1234| Admin password for the cluster VMs.|
|clusterName|mysfcluster123| Name of the cluster. |
|location|southcentralus| Location of the cluster. |
|certificateThumbprint|| <p>Value should be empty if creating a self-signed certificate or providing a certificate file.</p><p>To use an existing certificate previously uploaded to a key vault, fill in the certificate thumbprint value. For example, "6190390162C988701DB5676EB81083EA608DCCF3". </p>|
|certificateUrlValue|| <p>Value should be empty if creating a self-signed certificate or providing a certificate file.</p><p>To use an existing certificate previously uploaded to a key vault, fill in the certificate URL. For example, "https://mykeyvault.vault.azure.net:443/secrets/mycertificate/02bea722c9ef4009a76c5052bcbf8346".</p>|
|sourceVaultValue||<p>Value should be empty if creating a self-signed certificate or providing a certificate file.</p><p>To use an existing certificate previously uploaded to a key vault, fill in the source vault value. For example, "/subscriptions/333cc2c84-12fa-5778-bd71-c71c07bf873f/resourceGroups/MyTestRG/providers/Microsoft.KeyVault/vaults/MYKEYVAULT".</p>|

<a id="createvaultandcert" name="createvaultandcert_anchor"></a>

## Deploy the virtual network and cluster

Next, set up the network topology and deploy the Service Fabric cluster. The [vnet-linuxcluster.json][template] Resource Manager template creates a virtual network (VNET) and also a subnet and network security group (NSG) for Service Fabric. The template also deploys a cluster with certificate security enabled.  For production clusters, use a certificate from a certificate authority (CA) as the cluster certificate. A self-signed certificate can be used to secure test clusters.

The following script uses the [az sf cluster create](/cli/azure/sf/cluster?view=azure-cli-latest#az_sf_cluster_create) command and template to deploy a new cluster secured with an existing certificate. The command also creates a new key vault in Azure and uploads your certificate.

```azurecli
ResourceGroupName="sflinuxclustergroup"
Location="southcentralus"
Password="q6D7nN%6ck@6"
VaultName="linuxclusterkeyvault"
VaultGroupName="linuxclusterkeyvaultgroup"
CertPath="C:\MyCertificates\MyCertificate.pem"

# sign in to your Azure account and select your subscription
az login
az account set --subscription <guid>

# Create a new resource group for your deployment and give it a name and a location.
az group create --name $ResourceGroupName --location $Location

# Create the Service Fabric cluster.
az sf cluster create --resource-group $ResourceGroupName --location $Location \
   --certificate-password $Password --certificate-file $CertPath \
   --vault-name $VaultName --vault-resource-group $ResourceGroupName  \
   --template-file vnet-linuxcluster.json --parameter-file vnet-linuxcluster.parameters.json
```

## Connect to the secure cluster

Connect to the cluster using the Service Fabric CLI command `sfctl cluster select` with your key.  Note, only use the **--no-verify** option for a self-signed certificate.

```azurecli
sfctl cluster select --endpoint https://aztestcluster.southcentralus.cloudapp.azure.com:19080 \
--pem ./aztestcluster201709151446.pem --no-verify
```

Check that you are connected and the cluster is healthy using the `sfctl cluster health` command.

```azurecli
sfctl cluster health
```

## Clean up resources

The other articles in this tutorial series use the cluster you just created. If you're not immediately moving on to the next article, you might want to delete the cluster to avoid incurring charges. The simplest way to delete the cluster and all the resources it consumes is to delete the resource group.

Log in to Azure and select the subscription ID with which you want to remove the cluster.  You can find your subscription ID by logging in to the [Azure portal](http://portal.azure.com). Delete the resource group and all the cluster resources using the [az group delete](/cli/azure/group?view=azure-cli-latest#az_group_delete) command.

```azurecli
az group delete --name $ResourceGroupName
```

## Next steps

In this tutorial, you learned how to:

> [!div class="checklist"]
> * Create a VNET in Azure using Azure CLI
> * Create a secure Service Fabric cluster in Azure using Azure CLI
> * Secure the cluster with an X.509 certificate
> * Connect to the cluster using Service Fabric CLI
> * Remove a cluster

Next, advance to the following tutorial to learn how to scale your cluster.
> [!div class="nextstepaction"]
> [Scale a Cluster](service-fabric-tutorial-scale-cluster.md)

[template]:https://github.com/Azure/service-fabric-scripts-and-templates/blob/master/templates/cluster-tutorial/vnet-linuxcluster.json
[parameters]:https://github.com/Azure/service-fabric-scripts-and-templates/blob/master/templates/cluster-tutorial/vnet-linuxcluster.parameters.json