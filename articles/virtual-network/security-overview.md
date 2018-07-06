---
title: Azure network security overview | Microsoft Docs
description: Learn about security options for controlling the flow of network traffic between Azure resources.
services: virtual-network
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: ''

ms.assetid: 
ms.service: virtual-network
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/19/2017
ms.author: jdial

---
# Network security

You can limit network traffic to resources in a virtual network using a network security group. A network security group contains a list of security rules that allow or deny inbound or outbound network traffic based on source or destination IP address, port, and protocol. 

## Network security groups

Each network interface has zero, or one, associated network security group. Each network interface exists in a [virtual network](virtual-networks-overview.md) subnet. A subnet can also have zero, or one, associated network security group. 

When applied to a subnet, security rules are applied to all resources in the subnet. In addition to network interfaces, you may have instances of other Azure services such as HDInsight, Virtual Machine Scale Sets, and Application Service Environments deployed in the subnet.

How network security groups are applied when a network security group is associated to both a network interface, and the subnet the network interface is in, is as follows:

- **Inbound traffic**: The network security group associated to the subnet the network interface is in is evaluated first. Any traffic allowed through the network security group associated to the subnet is then evaluated by the network security group associated to the network interface. For example, you might require inbound access to a virtual machine over port 80 from the Internet. If you associate a network security group to both the network interface, and the subnet the network interface is in, the network security group associated to the subnet and the network interface must allow port 80. If you only allowed port 80 through the network security group associated to the subnet or the network interface the subnet is in, communication fails due to default security rules. See [default security rules](#default-security-rules) for details. If you only applied a network security group to either the subnet or the network interface, and the network security group contained a rule that allowed inbound port 80 traffic, for example, the communication succeeds. 
- **Outbound traffic**: The network security group associated to the network interface is evaluated first. Any traffic allowed through the network security group associated to the network interface is then evaluated by the network security group associated to the subnet.

You may not always be aware when network security groups are applied to both a network interface and a subnet. You can easily view the aggregate rules applied to a network interface by viewing the [effective security rules](virtual-network-network-interface.md#view-effective-security-rules) for a network interface. You can also use the [IP flow verify](../network-watcher/diagnose-vm-network-traffic-filtering-problem.md?toc=%2fazure%2fvirtual-network%2ftoc.json) capability in Azure Network Watcher to determine whether communication is allowed to or from a network interface. The tool tells you whether communication is allowed, and which network security rule allows or denies traffic.
 
> [!NOTE]
> Network security groups are associated to subnets or to virtual machines and cloud services deployed the classic deployment model, rather than to network interfaces in the Resource Manager deployment model. To learn more about Azure deployment models, see [Understand Azure deployment models](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

The same network security group can be applied to as many individual network interfaces and subnets as you choose.

## Security rules

A network security group contains zero, or as many rules as desired, within Azure subscription [limits](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits). Each rule specifies the following properties:

|Property  |Explanation  |
|---------|---------|
|Name|A unique name within the network security group.|
|Priority | A number between 100 and 4096. Rules are processed in priority order, with lower numbers processed before higher numbers, because lower numbers have higher priority. Once traffic matches a rule, processing stops. As a result, any rules that exist with lower priorities (higher numbers) that have the same attributes as rules with higher priorities are not processed.|
|Source or destination| Any, or an individual IP address, CIDR block (example 10.0.0.0/24, for example), service tag, or application security group. Learn more about [service tags](#service-tags) and [application security groups](#application-security-groups). Specifying a range, a service tag, or application security group, enables you to create fewer security rules. The ability to specify multiple individual IP addresses and ranges (you cannot specify multiple service tags or application groups) in a rule is referred to as augmented security rules. Learn more about [augmented security rules](#augmented-security-rules). Augmented security rules can only be created in network security groups created through the Resource Manager deployment model. You cannot specify multiple IP addresses and IP address ranges in network security groups created through the classic deployment model.|
|Protocol     | TCP, UDP, or Any, which includes TCP, UDP, and ICMP. You cannot specify ICMP alone, so if you require ICMP, you must use Any. |
|Direction| Whether the rule applies to inbound, or outbound traffic.|
|Port range     |You can specify an individual or range of ports. For example, you could specify 80 or 10000-10005. Specifying ranges enables you to create fewer security rules. Augmented security rules can only be created in network security groups created through the Resource Manager deployment model. You cannot specify multiple ports or port ranges in the same security rule in network security groups created through the classic deployment model.   |
|Action     | Allow or deny        |

NSG security rules are evaluated by priority using the 5 tuple information (source, source port, destination, destination port and protocol) to allow or deny the traffic. A Flow record is created for existing connections, communication is allowed or denied based on the connection state of the flow records, this allows NSG to be stateful. If you specify an outbound security rule to any address over port 80, for example, it's not necessary to specify an inbound security rule for the response to the outbound traffic. You only need to specify an inbound security rule if communication is initiated externally. The opposite is also true. If inbound traffic is allowed over a port, it's not necessary to specify an outbound security rule to respond to traffic over the port.
An existing connections may not be interrupted when you remove a security rule that enabled the flow. Traffic flows are interrupted when connections are stopped and no traffic is flowing on either direction for at least a few minutes.

To learn about limits when creating security rules, see [Azure limits](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits).

## Augmented security rules

Augmented rules simplify security definition for virtual networks, allowing you to define larger and complex network security policies, with fewer rules. You can combine multiple ports, multiple explicit IP addresses, Service tags, and Application security groups into a single, easily understood security rule. Use augmented rules in the source, destination, and port fields of a rule. When creating a rule, you can specify multiple explicit IP addresses, CIDR ranges, and ports. To simplify maintenance of your security rule definition, combine augmented security rules with service tags or application security groups. 

To learn about limits when creating augmented security rules, see [Azure limits](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits).

## Default security rules

If a network security group is not associated to a subnet or network interface, all traffic is allowed inbound to, or outbound from, the subnet, or network interface. As soon as a network security group is created, Azure creates the following default rules within the network security group:

### Inbound

#### AllowVNetInBound

|Priority|Source|Source ports|Destination|Destination ports|Protocol|Access|
|---|---|---|---|---|---|---|
|65000|VirtualNetwork|0-65535|VirtualNetwork|0-65535|All|Allow|

#### AllowAzureLoadBalancerInBound

|Priority|Source|Source ports|Destination|Destination ports|Protocol|Access|
|---|---|---|---|---|---|---|
|65001|AzureLoadBalancer|0-65535|0.0.0.0/0|0-65535|All|Allow|

#### DenyAllInbound

|Priority|Source|Source ports|Destination|Destination ports|Protocol|Access|
|---|---|---|---|---|---|---|
|65500|0.0.0.0/0|0-65535|0.0.0.0/0|0-65535|All|Deny|

### Outbound

#### AllowVnetOutBound

|Priority|Source|Source ports| Destination | Destination ports | Protocol | Access |
|---|---|---|---|---|---|---|
| 65000 | VirtualNetwork | 0-65535 | VirtualNetwork | 0-65535 | All | Allow |

#### AllowInternetOutBound

|Priority|Source|Source ports| Destination | Destination ports | Protocol | Access |
|---|---|---|---|---|---|---|
| 65001 | 0.0.0.0/0 | 0-65535 | Internet | 0-65535 | All | Allow |

#### DenyAllOutBound

|Priority|Source|Source ports| Destination | Destination ports | Protocol | Access |
|---|---|---|---|---|---|---|
| 65500 | 0.0.0.0/0 | 0-65535 | 0.0.0.0/0 | 0-65535 | All | Deny |

In the **Source** and **Destination** columns, *VirtualNetwork*, *AzureLoadBalancer*, and *Internet* are [service tags](#tags), rather than IP addresses. In the protocol column, **All** encompasses TCP, UDP, and ICMP. When creating a rule, you can specify TCP, UDP, or All, but you cannot specify ICMP alone. Therefore, if your rule requires ICMP, you must select *All* for protocol. *0.0.0.0/0* in the **Source** and **Destination** columns represents all addresses.
 
You cannot remove the default rules, but you can override them by creating rules with higher priorities.

## Service tags

 A service tag represents a group of IP address prefixes to help minimize complexity for security rule creation. You cannot create your own service tag, nor specify which IP addresses are included within a tag. Microsoft manages the address prefixes encompassed by the service tag, and automatically updates the service tag as addresses change. You can use service tags in place of specific IP addresses when creating security rules. The following service tags are available for use in security rule definition. Their names vary slightly between [Azure deployment models](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

* **VirtualNetwork** (Resource Manager) (**VIRTUAL_NETWORK** for classic): This tag includes the virtual network address space (all CIDR ranges defined for the virtual network), all connected on-premises address spaces, and [peered](virtual-network-peering-overview.md) virtual networks or virtual network connected to a [virtual network gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
* **AzureLoadBalancer** (Resource Manager) (**AZURE_LOADBALANCER** for classic): This tag denotes Azure's infrastructure load balancer. The tag translates to an [Azure datacenter IP address](https://www.microsoft.com/download/details.aspx?id=41653) where Azure's health probes originate. If you are not using the Azure load balancer, you can override this rule.
* **Internet** (Resource Manager) (**INTERNET** for classic): This tag denotes the IP address space that is outside the virtual network and reachable by the public Internet. The address range includes the [Azure owned public IP address space](https://www.microsoft.com/download/details.aspx?id=41653).
* **AzureTrafficManager** (Resource Manager only): This tag denotes the IP address space for the Azure Traffic Manager probe IPs. More information on Traffic Manager probe IPs can be found in the [Azure Traffic Manager FAQ](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs).
* **Storage** (Resource Manager only): This tag denotes the IP address space for the Azure Storage service. If you specify *Storage* for the value, traffic is allowed or denied to storage. If you only want to allow access to storage in a specific [region](https://azure.microsoft.com/regions), you can specify the region. For example, if you want to allow access only to Azure Storage in the East US region, you could specify *Storage.EastUS* as a service tag. The tag represents the service, but not specific instances of the service. For example, the tag represents the Azure Storage service, but not a specific Azure Storage account. All address prefixes represented by this tag are also represented by the **Internet** tag.
* **Sql** (Resource Manager only): This tag denotes the address prefixes of the Azure SQL Database and Azure SQL Data Warehouse services. If you specify *Sql* for the value, traffic is allowed or denied to Sql. If you only want to allow access to Sql in a specific [region](https://azure.microsoft.com/regions), you can specify the region. For example, if you want to allow access only to Azure SQL Database in the East US region, you could specify *Sql.EastUS* as a service tag. The tag represents the service, but not specific instances of the service. For example, the tag represents the Azure SQL Database service, but not a specific SQL database or server. All address prefixes represented by this tag are also represented by the **Internet** tag.
* **AzureCosmosDB** (Resource Manager only): This tag denotes the address prefixes of the Azure Cosmos Database service. If you specify *AzureCosmosDB* for the value, traffic is allowed or denied to AzureCosmosDB. If you only want to allow access to AzureCosmosDB in a specific [region](https://azure.microsoft.com/regions), you can specify the region in the following format AzureCosmosDB.[region name].
* **AzureKeyVault** (Resource Manager only): This tag denotes the address prefixes of the Azure KeyVault service. If you specify *AzureKeyVault* for the value, traffic is allowed or denied to AzureKeyVault. If you only want to allow access to AzureKeyVault in a specific [region](https://azure.microsoft.com/regions), you can specify the region in the following format AzureKeyVault.[region name].

> [!NOTE]
> Service tags of azure services denotes the address prefixes from the specific cloud being used. Regional service tags are not supported on national clouds, only in global format, e.g. Storage, Sql.

> [!NOTE]
> If you implement a [virtual network service endpoint](virtual-network-service-endpoints-overview.md) for a service, such as Azure Storage or Azure SQL Database, Azure adds a route to a virtual network subnet for the service. The address prefixes in the route are the same address prefixes, or CIDR ranges, as the corresponding service tag.

## Application security groups

Application security groups enable you to configure network security as a natural extension of an application's structure, allowing you to group virtual machines and define network security policies based on those groups. This feature allows you to reuse your security policy at scale without manual maintenance of explicit IP addresses. The platform handles the complexity of explicit IP addresses and multiple rule sets, allowing you to focus on your business logic.

You can specify one application security group as the source and destination in a security rule. You cannot specify multiple application security groups in the source and destination. Once your security policy is defined, you can create virtual machines and assign the network interfaces in the virtual machine to an application security group. The policy is applied based on the application security group membership of each network interface within a virtual machine. The following example illustrates how you might use an application security group for all web servers in your subscription:

1. Create an application security group named *WebServers*.
2. Create a network security group named *MyNSG*.
3. Create an inbound security rule in the network security group, specifying the *Internet* service tag for source address and the *WebServers* application security group as the destination address, and allow ports 80 and 443.
4. Deploy a virtual machine running a web server application. Specify that the network interface in the virtual machine is a member of the *WebServers* application security group. Ports 80 and 443 are then allowed to the virtual machine. The ports are also allowed to any subsequent web servers you create that you make members of the *WebServers* application security group. 

If you create other rules, specifying other application security groups as the destination, those rules aren't applied to the web servers, in the previous example. Rules specifying an application security group are only applied to network interfaces that are members of the application security group. Application security groups, combined with augmented security rules and service tags, make it possible to create a minimal number of network security groups to manage network security within your subscription.
 
To learn about limits when creating application security groups and specifying them in security rules, see [Azure limits](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits).

Application security groups have the following constraints:

-	All network interfaces assigned to an application security group must exist in the same virtual network that the first network interface assigned to the application security group is in. For example, if the first network interface assigned to an application security group named *ASG1* is in the virtual network named *VNet1*, then all subsequent network interfaces assigned to *ASG1* must exist in *VNet1*. You cannot add network interfaces from different virtual networks to the same application security group.
- If you specify an application security group as the source and destination in a security rule, the network interfaces in both application security groups must exist in the same virtual network. For example, if ASG1 contained network interfaces from VNet1, and ASG2 contained network interfaces from VNet2, you could not assign ASG1 as the source and ASG2 as the destination in a rule. All network interfaces need to exist in VNet1.

## Azure platform considerations

- **Virtual IP of the host node**: Basic infrastructure services such as DHCP, DNS, and health monitoring are provided through the virtualized host IP addresses 168.63.129.16 and 169.254.169.254. These public IP addresses belong to Microsoft and are the only virtualized IP addresses used in all regions for this purpose. The addresses map to the physical IP address of the server machine (host node) hosting the virtual machine. The host node acts as the DHCP relay, the DNS recursive resolver, and the probe source for the load balancer health probe and the machine health probe. Communication to these IP addresses is not an attack. If you block traffic to or from these IP addresses, a virtual machine may not function properly.
- **Licensing (Key Management Service)**: Windows images running in virtual machines must be licensed. To ensure licensing, a request is sent to the Key Management Service host servers that handle such queries. The request is made outbound through port 1688. For deployments using [default route 0.0.0.0/0](virtual-networks-udr-overview.md#default-route) configuration, this platform rule will be disabled.
- **Virtual machines in load-balanced pools**: The source port and address range applied are from the originating computer, not the load balancer. The destination port and address range are for the destination computer, not the load balancer.
- **Azure service instances**: Instances of several Azure services, such as HDInsight, Application Service Environments, and Virtual Machine Scale Sets are deployed in virtual network subnets. For a complete list of services you can deploy into virtual networks, see [Virtual network for Azure services](virtual-network-for-azure-services.md#services-that-can-be-deployed-into-a-virtual-network). Ensure you familiarize yourself with the port requirements for each service before applying a network security group to the subnet the resource is deployed in. If you deny ports required by the service, the service doesn't function properly.
- **Sending outbound email**: Microsoft recommends that you utilize authenticated SMTP relay services (typically connected via TCP port 587, but often others, as well) to send email from Azure Virtual Machines. SMTP relay services specialize in sender reputation, to minimize the possibility that third-party email providers reject messages. Such SMTP relay services include, but are not limited to, Exchange Online Protection and SendGrid. Use of SMTP relay services is in no way restricted in Azure, regardless of your subscription type. 

  If you created your Azure subscription prior to November 15, 2017, in addition to being able to use SMTP relay services, you can send email directly over TCP port 25. If you created your subscription after November 15, 2017, you may not be able to send email directly over port 25. The behavior of outbound communication over port 25 depends on the type of subscription you have, as follows:

     - **Enterprise Agreement**: Outbound port 25 communication is allowed. You are able to send outbound email directly from virtual machines to external email providers, with no restrictions from the Azure platform. 
     - **Pay-as-you-go:** Outbound port 25 communication is blocked from all resources. If you need to send email from a virtual machine directly to external email providers (not using an authenticated SMTP relay), you can make a request to remove the restriction. Requests are reviewed and approved at Microsoft's discretion and are only granted after anti-fraud checks are performed. To make a request, open a support case with the issue type *Technical*, *Virtual Network Connectivity*, *Cannot send e-mail (SMTP/Port 25)*. In your support case, include details about why your subscription needs to send email directly to mail providers, instead of going through an authenticated SMTP relay. If your subscription is exempted, only virtual machines created after the exemption date are able to communicate outbound over port 25.
     - **MSDN, Azure Pass, Azure in Open, Education, BizSpark, and Free trial**: Outbound port 25 communication is blocked from all resources. No requests to remove the restriction can be made, because requests are not granted. If you must send email from your virtual machine, you must use an SMTP relay service.
     - **Cloud service provider**: Customers that are consuming Azure resources via a cloud service provider can create a support case with their cloud service provider, and request that the provider create an unblock case on their behalf, if a secure SMTP relay cannot be used.

  If Azure allows you to send email over port 25, Microsoft cannot guarantee email providers will accept inbound email from your virtual machine. If a specific provider rejects mail from your virtual machine, you must work directly with the provider to resolve any message delivery or spam filtering issues, or use an authenticated SMTP relay service.

## Next steps

* Learn how to [Create a network security group](tutorial-filter-network-traffic.md).
