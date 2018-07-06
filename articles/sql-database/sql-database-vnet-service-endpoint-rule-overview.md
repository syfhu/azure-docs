---
title: "Virtual Network service endpoints and rules for Azure SQL Database | Microsoft Docs"
description: "Mark a subnet as a Virtual Network service endpoint. Then the endpoint as a virtual network rule to the ACL your Azure SQL Database. You SQL Database then accepts communication from all virtual machines and other nodes on the subnet."
services: sql-database
ms.service: sql-database
ms.prod_service: sql-database, sql-data-warehouse
author: DhruvMsft
manager: craigg
ms.custom: "VNet Service endpoints"
ms.topic: conceptual
ms.date: 06/13/2018
ms.reviewer: genemi
ms.author: dmalik
ms.reviewer: carlrab
---
# Use Virtual Network service endpoints and rules for Azure SQL Database and SQL Data Warehouse

*Virtual network rules* are one firewall security feature that controls whether your Azure [SQL Database](sql-database-technical-overview.md) or [SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md) server accepts communications that are sent from particular subnets in virtual networks. This article explains why the virtual network rule feature is sometimes your best option for securely allowing communication to your Azure SQL Database.

> [!NOTE]
> This topic applies to Azure SQL server, and to both SQL Database and SQL Data Warehouse databases that are created on the Azure SQL server. For simplicity, SQL Database is used when referring to both SQL Database and SQL Data Warehouse.

To create a virtual network rule, there must first be a [virtual network service endpoint][vm-virtual-network-service-endpoints-overview-649d] for the rule to reference.

#### How to create a virtual network rule

If you only create a virtual network rule, you can skip ahead to the steps and explanation [later in this article](#anchor-how-to-by-using-firewall-portal-59j).






<a name="anch-terminology-and-description-82f" />

## Terminology and description

**Virtual network:** You can have virtual networks associated with your Azure subscription.

**Subnet:** A virtual network contains **subnets**. Any Azure virtual machines (VMs) that you have are assigned to subnets. One subnet can contain multiple VMs or other compute nodes. Compute nodes that are outside of your virtual network cannot access your virtual network unless you configure your security to allow access.

**Virtual Network service endpoint:** A [Virtual Network service endpoint][vm-virtual-network-service-endpoints-overview-649d] is a subnet whose property values include one or more formal Azure service type names. In this article we are interested in the type name of **Microsoft.Sql**, which refers to the Azure service named SQL Database.

**Virtual network rule:** A virtual network rule for your SQL Database server is a subnet that is listed in the access control list (ACL) of your SQL Database server. To be in the ACL for your SQL Database, the subnet must contain the **Microsoft.Sql** type name.

A virtual network rule tells your SQL Database server to accept communications from every node that is on the subnet.







<a name="anch-benefits-of-a-vnet-rule-68b" />

## Benefits of a virtual network rule

Until you take action, the VMs on your subnets cannot communicate with your SQL Database. One action that establishes the communication is the creation of a virtual network rule. The rationale for choosing the VNet rule approach requires a compare-and-contrast discussion involving the competing security options offered by the firewall.

#### A. Allow access to Azure services

The firewall pane has an **ON/OFF** button that is labeled **Allow access to Azure services**. The **ON** setting allows communications from all Azure IP addresses and all Azure subnets. These Azure IPs or subnets might not be owned by you. This **ON** setting is probably more open than you want your SQL Database to be. The virtual network rule feature offers much finer granular control.

#### B. IP rules

The SQL Database firewall allows you to specify IP address ranges from which communications are accepted into SQL Database. This approach is fine for stable IP addresses that are outside the Azure private network. But many nodes inside the Azure private network are configured with *dynamic* IP addresses. Dynamic IP addresses might change, such as when your VM is restarted. It would be folly to specify a dynamic IP address in a firewall rule, in a production environment.

You can salvage the IP option by obtaining a *static* IP address for your VM. For details, see [Configure private IP addresses for a virtual machine by using the Azure portal][vm-configure-private-ip-addresses-for-a-virtual-machine-using-the-azure-portal-321w].

However, the static IP approach can become difficult to manage, and it is costly when done at scale. Virtual network rules are easier to establish and to manage.

#### C. Cannot yet have SQL Database on a subnet

If your Azure SQL Database server was a node on a subnet in your virtual network, all nodes within the virtual network could communicate with your SQL Database. In this case, your VMs could communicate with SQL Database without needing any virtual network rules or IP rules.

However as of September 2017, the Azure SQL Database service is not yet among the services that can be assigned to a subnet.






<a name="anch-details-about-vnet-rules-38q" />

## Details about virtual network rules

This section describes several details about virtual network rules.

#### Only one geographic region

Each Virtual Network service endpoint applies to only one Azure region. The endpoint does not enable other regions to accept communication from the subnet.

Any virtual network rule is limited to the region that its underlying endpoint applies to.

#### Server-level, not database-level

Each virtual network rule applies to your whole Azure SQL Database server, not just to one particular database on the server. In other words, virtual network rule applies at the server-level, not at the database-level.

- In contrast, IP rules can apply at either level.

#### Security administration roles

There is a separation of security roles in the administration of Virtual Network service endpoints. Action is required from each of the following roles:

- **Network Admin:** &nbsp; Turn on the endpoint.
- **Database Admin:** &nbsp; Update the access control list (ACL) to add the given subnet to the SQL Database server.

*RBAC alternative:*

The roles of Network Admin and Database Admin have more capabilities than are needed to manage virtual network rules. Only a subset of their capabilities is needed.

You have the option of using [role-based access control (RBAC)][rbac-what-is-813s] in Azure to create a single custom role that has only the necessary subset of capabilities. The custom role could be used instead of involving either the Network Admin or the Database Admin. The surface area of your security exposure is lower if you add a user to a custom role, versus adding the user to the other two major administrator roles.

> [!NOTE]
> In some cases the Azure SQL Database and the VNet-subnet are in different subscriptions. In these cases you must ensure the following configurations:
> - Both subscriptions must be in the same Azure Active Directory tenant.
> - The user has the required permissions to initiate operations, such as enabling service endpoints and adding a VNet-subnet to the given Server.

## Limitations

For Azure SQL Database, the virtual network rules feature has the following limitations:

- A Web App can be mapped to a private IP in a VNet/subnet. Even if service endpoints are turned ON from the given VNet/subnet, connections from the Web App to the server will have an Azure public IP source, not a VNet/subnet source. To enable connectivity from a Web App to a server that has VNet firewall rules, you must **Allow all Azure services** on the server.

- In the firewall for your SQL Database, each virtual network rule references a subnet. All these referenced subnets must be hosted in the same geographic region that hosts the SQL Database.

- Each Azure SQL Database server can have up to 128 ACL entries for any given virtual network.

- Virtual network rules apply only to Azure Resource Manager virtual networks; and not to [classic deployment model][arm-deployment-model-568f] networks.

- Turning ON virtual network service endpoints to Azure SQL Database also enables the endpoints for the MySQL and PostgreSQL Azure services. However, with endpoints ON, attempts to connect from the endpoints to your MySQL or PostgreSQL instances will fail.
    - The underlying reason is that MySQL and PostgreSQL do not presently support ACLing.

- On the firewall, IP address ranges do apply to the following networking items, but virtual network rules do not:
    - [Site-to-Site (S2S) virtual private network (VPN)][vpn-gateway-indexmd-608y]
    - On-premises via [ExpressRoute][expressroute-indexmd-744v]

#### Considerations when using Service Endpoints
When using service endpoints for Azure SQL Database, review the following considerations:

- **Outbound to Azure SQL Database Public IPs is required**: Network Security Groups (NSGs) must be opened to Azure SQL Database IPs to allow connectivity. You can do this by using NSG [Service Tags](../virtual-network/security-overview.md#service-tags) for Azure SQL Database.
- **Azure Database for PostgreSQL and MySQL are unsupported**: Service endpoints are not supported for Azure Database for PostgreSQL or MySQL. Enabling service endpoints to SQL Database will break connectivity to these services. We have a mitigation for this, and you may contact *dmalik@microsoft.com* for more information.

#### ExpressRoute

If your network is connected to the Azure network through use of [ExpressRoute][expressroute-indexmd-744v], each circuit is configured with two public IP addresses at the Microsoft Edge. The two IP addresses are used to connect to Microsoft Services, such as to Azure Storage, by using Azure Public Peering.

To allow communication from your circuit to Azure SQL Database, you must create IP network rules for the public IP addresses of your circuits. In order to find the public IP addresses of your ExpressRoute circuit, open a support ticket with ExpressRoute by using the Azure portal.


<!--
FYI: Re ARM, 'Azure Service Management (ASM)' was the old name of 'classic deployment model'.
When searching for blogs about ASM, you probably need to use this old and now-forbidden name.
-->

## Impact of removing 'Allow all Azure Services'

Many users want to remove **Allow all Azure Services** from their Azure SQL Servers and replace it with a VNet Firewall Rule.
However removing this affects the following Azure SQLDB features:

#### Import Export Service
Azure SQLDB Import Export Service runs on VMs in Azure. These VMs are not in your VNet and hence get an Azure IP when connecting to your
database. On removing **Allow all Azure Services** these VMs will not be able to access your databases.
You can work around the problem. Run the BACPAC import or export directly in your code by using the DACFx API. Ensure that this is deployed in a VM that is in the VNet-subnet for which you have set the firewall rule.

#### SQL Database Query Editor
The Azure SQL Database Query Editor is deployed on VMs in Azure. These VMs are not in your VNet. Therefore the VMs get an Azure IP when connecting to your database. On removing **Allow all Azure Services**, these VMs will not be able to access your databases.

#### Table Auditing
At present there are two ways to enable auditing on your SQL Database. Table auditing fails after you have enabled service endpoints on your Azure SQL Server. Mitigation here is to move to Blob auditing.

#### Impact on Data Sync
Azure SQLDB has the Data Sync feature that connects to your databases using Azure IPs. When using service endpoints, it is likely that you will turn off **Allow all Azure Services** access to your logical server. This will break the Data Sync feature.

## Impact of using VNet Service Endpoints with Azure storage

Azure Storage has implemented the same feature that allows you to limit connectivity to your Storage account.
If you choose to use this feature with a Storage account that is being used by an Azure SQL Server, you can run into issues. Next is a list and discussion of Azure SQLDB features that are impacted by this.

#### Azure SQLDW PolyBase
PolyBase is commonly used to load data into Azure SQLDW from Storage accounts. If the Storage account that you are loading data from limits access only to a set of VNet-subnets, connectivity from PolyBase to the Account will break. There is a mitigation for this, and you may contact *dmalik@microsoft.com* for more information.

#### Azure SQLDB Blob Auditing
Blob auditing pushes audit logs to your own storage account. If this storage account uses the VENT Service endpoints feature then connectivity from Azure SQLDB to the storage account will break.


## Adding a VNET Firewall rule to your server without turning On VNET Service Endpoints

Long ago, before this feature was enhanced, you were required to turn VNet service endpoints On before you could implement a live VNet rule in the Firewall. The endpoints related a given VNet-subnet to an Azure SQL Database. But now as of January 2018, you can circumvent this requirement by setting the **IgnoreMissingServiceEndpoint** flag.

Merely setting a Firewall rule does not help secure the server. You must also turn VNet service endpoints On for the security to take effect. When you turn service endpoints On, your VNet-subnet experiences downtime until it completes the transition from Off to On. This is especially true in the context of large VNets. You can use the **IgnoreMissingServiceEndpoint** flag to reduce or eliminate the downtime during transition.

You can set the **IgnoreMissingServiceEndpoint** flag by using PowerShell. For details, see [PowerShell to create a Virtual Network service endpoint and rule for Azure SQL Database][sql-db-vnet-service-endpoint-rule-powershell-md-52d].


## Errors 40914 and 40615

Connection error 40914 relates to *virtual network rules*, as specified on the Firewall pane in the Azure portal. Error 40615 is similar, except it relates to *IP address rules* on the Firewall.

#### Error 40914

*Message text:* Cannot open server '*[server-name]*' requested by the login. Client is not allowed to access the server.

*Error description:* The client is in a subnet that has virtual network server endpoints. But the Azure SQL Database server has no virtual network rule that grants to the subnet the right to communicate with the SQL Database.

*Error resolution:* On the Firewall pane of the Azure portal, use the virtual network rules control to [add a virtual network rule](#anchor-how-to-by-using-firewall-portal-59j) for the subnet.

#### Error 40615

*Message text:* Cannot open server '{0}' requested by the login. Client with IP address '{1}' is not allowed to access the server.

*Error description:* The client is trying to connect from an IP address that is not authorized to connect to the Azure SQL Database server. The server firewall has no IP address rule that allows a client to communicate from the given IP address to the SQL Database.

*Error resolution:* Enter the client's IP address as an IP rule. Do this by using the Firewall pane in the Azure portal.


A list of several SQL Database error messages is documented [here][sql-database-develop-error-messages-419g].





<a name="anchor-how-to-by-using-firewall-portal-59j" />

## Portal can create a virtual network rule

This section illustrates how you can use the [Azure portal][http-azure-portal-link-ref-477t] to create a *virtual network rule* in your Azure SQL Database. The rule tells your SQL Database to accept communication from a particular subnet that has been tagged as being a *Virtual Network service endpoint*.

> [!NOTE]
> If you intend to add a service endpoint to the VNet firewall rules of your Azure SQL Database server, first ensure that service endpoints are turned On for the subnet.
>
> If service endpoints are not turned on for the subnet, the portal asks you to enable them. Click the **Enable** button on the same blade on which you add the rule.

#### PowerShell alternative

A PowerShell script can also create virtual network rules. The crucial cmdlet **New-AzureRmSqlServerVirtualNetworkRule**. If interested, see [PowerShell to create a Virtual Network service endpoint and rule for Azure SQL Database][sql-db-vnet-service-endpoint-rule-powershell-md-52d].

#### REST API alternative

Internally, the PowerShell cmdlets for SQL VNet actions call REST APIs. You can call the REST APIs directly.

- [Virtual Network Rules: Operations][rest-api-virtual-network-rules-operations-862r]

#### Prerequisites

You must already have a subnet that is tagged with the particular Virtual Network service endpoint *type name* relevant to Azure SQL Database.

- The relevant endpoint type name is **Microsoft.Sql**.
- If your subnet might not be tagged with the type name, see [Verify your subnet is an endpoint][sql-db-vnet-service-endpoint-rule-powershell-md-a-verify-subnet-is-endpoint-ps-100].

<a name="a-portal-steps-for-vnet-rule-200" />

### Azure portal steps

1. Log in to the [Azure portal][http-azure-portal-link-ref-477t].

2. Then navigate the portal to **SQL servers** &gt; **Firewall / Virtual Networks**.

3. Set the **Allow access to Azure services** control to OFF.

    > [!IMPORTANT]
    > If you leave the control set to ON, your Azure SQL Database server accepts communication from any subnet. Leaving the control set to ON might be excessive access from a security point of view. The Microsoft Azure Virtual Network service endpoint feature, in coordination with the virtual network rule feature of SQL Database, together can reduce your security surface area.

4. Click the **+ Add existing** control, in the **Virtual networks** section.

    ![Click add existing (subnet endpoint, as a SQL rule).][image-portal-firewall-vnet-add-existing-10-png]

5. In the new **Create/Update** pane, fill in the controls with the names of your Azure resources.

    > [!TIP]
    > You must include the correct **Address prefix** for your subnet. You can find the value in the portal.
    > Navigate **All resources** &gt; **All types** &gt; **Virtual networks**. The filter displays your virtual networks. Click your virtual network, and then click **Subnets**. The **ADDRESS RANGE** column has the Address prefix you need.

    ![Fill in fields for new rule.][image-portal-firewall-create-update-vnet-rule-20-png]

6. Click the **OK** button near the bottom of the pane.

7.  See the resulting virtual network rule on the firewall pane.

    ![See the new rule, on the firewall pane.][image-portal-firewall-vnet-result-rule-30-png]


> [!NOTE]
> The following statuses or states apply to the rules:
> - **Ready:** Indicates that the operation that you initiated has Succeeded.
> - **Failed:** Indicates that the operation that you initiated has Failed.
> - **Deleted:** Only applies to the Delete operation, and indicates that the rule has been deleted and no longer applies.
> - **InProgress:** Indicates that the operation is in progress. The old rule applies while the operation is in this state.


<a name="anchor-how-to-links-60h" />

## Related articles

- [Azure virtual network service endpoints][vm-virtual-network-service-endpoints-overview-649d]
- [Azure SQL Database server-level and database-level firewall rules][sql-db-firewall-rules-config-715d]

The virtual network rule feature for Azure SQL Database became available in late September 2017.

## Next steps

- [Use PowerShell to create a virtual network service endpoint, and then a virtual network rule for Azure SQL Database.][sql-db-vnet-service-endpoint-rule-powershell-md-52d]
- [Virtual Network Rules: Operations][rest-api-virtual-network-rules-operations-862r] with REST APIs



<!-- Link references, to images. -->

[image-portal-firewall-vnet-add-existing-10-png]: media/sql-database-vnet-service-endpoint-rule-overview/portal-firewall-vnet-add-existing-10.png

[image-portal-firewall-create-update-vnet-rule-20-png]: media/sql-database-vnet-service-endpoint-rule-overview/portal-firewall-create-update-vnet-rule-20.png

[image-portal-firewall-vnet-result-rule-30-png]: media/sql-database-vnet-service-endpoint-rule-overview/portal-firewall-vnet-result-rule-30.png



<!-- Link references, to text, Within this same Github repo. -->

[arm-deployment-model-568f]: ../azure-resource-manager/resource-manager-deployment-model.md

[expressroute-indexmd-744v]: ../expressroute/index.yml

[rbac-what-is-813s]:../role-based-access-control/overview.md

[sql-db-firewall-rules-config-715d]: sql-database-firewall-configure.md

[sql-database-develop-error-messages-419g]: sql-database-develop-error-messages.md

[sql-db-vnet-service-endpoint-rule-powershell-md-52d]: sql-database-vnet-service-endpoint-rule-powershell.md

[sql-db-vnet-service-endpoint-rule-powershell-md-a-verify-subnet-is-endpoint-ps-100]: sql-database-vnet-service-endpoint-rule-powershell.md#a-verify-subnet-is-endpoint-ps-100

[vm-configure-private-ip-addresses-for-a-virtual-machine-using-the-azure-portal-321w]: ../virtual-network/virtual-networks-static-private-ip-arm-pportal.md

[vm-virtual-network-service-endpoints-overview-649d]: https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoints-overview

[vpn-gateway-indexmd-608y]: ../vpn-gateway/index.yml



<!-- Link references, to text, Outside this Github repo (HTTP). -->

[http-azure-portal-link-ref-477t]: https://portal.azure.com/

[rest-api-virtual-network-rules-operations-862r]: https://docs.microsoft.com/rest/api/sql/virtualnetworkrules



<!-- ??2
#### Syntax related articles
- REST API Reference, including JSON

- Azure CLI

- ARM templates
-->
