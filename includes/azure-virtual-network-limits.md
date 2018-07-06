---
 title: include file
 description: include file
 services: networking
 author: jimdial
 ms.service: networking
 ms.topic: include
 ms.date: 06/20/2018
 ms.author: jdial
 ms.custom: include file

---

<a name="virtual-networking-limits-classic"></a>The following limits apply only for networking resources managed through the classic deployment model per subscription. Learn how to [view your current resource usage against your subscription limits](../articles/networking/check-usage-against-limits.md).

| Resource | Default limit | Maximum limit |
| --- | --- | --- |
| Virtual networks |50 |100 |
| Local network sites |20 |contact support |
| DNS Servers per virtual network |20 |100 |
| Private IP Addresses per virtual network |4096 |4096 |
| Concurrent TCP or UDP flows per NIC of a virtual machine or role instance |500K |500K |
| Network Security Groups (NSG) |100 |200 |
| NSG rules per NSG |200 |1000 |
| User defined route tables |100 |200 |
| User defined routes per route table |100 |400 |
| Public IP addresses (dynamic) |5 |contact support |
| Reserved public IP addresses |20 |contact support |
| Public VIP per deployment |5 |contact support |
| Private VIP (ILB) per deployment |1 |1 |
| Endpoint Access Control Lists (ACLs) |50 |50 |

#### <a name="azure-resource-manager-virtual-networking-limits"></a>Networking Limits - Azure Resource Manager
The following limits apply only for networking resources managed through Azure Resource Manager per region per subscription. Learn how to [view your current resource usage against your subscription limits](../articles/networking/check-usage-against-limits.md).

| Resource | Default limit | Maximum Limit |
| --- | --- | --- |
| Virtual networks |50 |1000 |
| Subnets per virtual network |1000 |10000 |
| Virtual network peerings per Virtual Network |10 |50 |
| DNS Servers per virtual network |9 |25 |
| Private IP Addresses per virtual network |16384** |16384 |
| Private IP Addresses per network interface |256 |256 |
| Concurrent TCP or UDP flows per NIC of a virtual machine or role instance |500K |500K |
| Network Interfaces (NIC) |24000** |24000 |
| Network Security Groups (NSG) |100 |5000 |
| NSG rules per NSG |1000** |1000 |
| IP addresses and ranges specified for source or destination in a security group |2000 |4000 |
| Application security groups |500 |3000 |
| Application security groups per IP configuration, per NIC |10 |20 |
| IP configurations per application security group |1000 |4000 |
| Application security groups that can be specified within all security rules of a network security group |50 |100 |
| User defined route tables |100 |200 |
| User defined routes per route table |100 |400 |
| Public IP addresses - dynamic |(Basic) 60 |contact support |
| Public IP addresses - static |(Basic) 20 |contact support |
| Public IP addresses - static |(Standard) 20 |contact support |
| Point-to-Site Root Certificates per VPN Gateway |20 |20 |

**These default limits apply to subscriptions that have not previously had these limits increased through support

#### <a name="load-balancer"></a>Load Balancer limits
The following limits apply only for networking resources managed through Azure Resource Manager per region per subscription. Learn how to [view your current resource usage against your subscription limits](../articles/networking/check-usage-against-limits.md)

| Resource | Default limit | Maximum Limit |
| --- | --- | --- |
| Load Balancers | 100 | 1000 |
| Rules per resource, Basic | 150 | 250 |
| Rules per resource, Standard | 1250 | 1500 |
| Rules per IP configuration | 299 |299 |
| Frontend IP configurations, Basic | 10 | 200 |
| Frontend IP configurations, Standard | 10 | 600 |
| Backend pool, Basic | 100, single Availability Set | 100, single Availability Set |
| Backend pool, Standard | 1000, single VNet | 1000, single VNet |
| Backend resources per Load Balancer, Standard &ast; | 50 | 150 |
| HA Ports, Standard | 1 per internal frontend | 1 per internal frontend |

&ast; Up to 150 resources, any combination of standalone virtual machines, availability sets, and virtual machine scale sets.

[Contact support](../articles/azure-supportability/resource-manager-core-quotas-request.md ) in case you need to increase limits from default.

