---
 title: include file
 description: include file
 services: billing
 author: rothja
 ms.service: billing
 ms.topic: include
 ms.date: 05/18/2018
 ms.author: jroth
 ms.custom: include file
---

| Resource | Default Limit | Maximum Limit |
| --- | --- | --- |
| vCPUs per [subscription](../articles/billing-buy-sign-up-azure-subscription.md) <sup>1</sup> |20 |10,000 |
| [Co-administrators](../articles/billing-add-change-azure-subscription-administrator.md) per subscription |200 |200 |
| [Storage accounts](../articles/storage/common/storage-create-storage-account.md) per region per subscription<sup>2</sup> |200 |250 |
| [Cloud services](../articles/cloud-services/cloud-services-choose-me.md) per subscription |20 |200 |
| [Local networks](http://msdn.microsoft.com/library/jj157100.aspx) per subscription |10 |500 |
| SQL Database servers per subscription |6 |200 |
| DNS servers per subscription |9 |100 |
| Reserved IPs per subscription |20 |100 |
| Hosted service certificates per subscription |199 |199 |
| [Affinity groups](../articles/virtual-network/virtual-networks-migrate-to-regional-vnet.md) per subscription |256 |256 |


<sup>1</sup>Extra Small instances count as one vCPU towards the vCPU limit despite using a partial CPU core.

<sup>2</sup>The storage account limit includes both Standard and Premium storage accounts. If you require more than 200 storage accounts in a given region, make a request through [Azure Support](https://azure.microsoft.com/support/faq/). The Azure Storage team will review your business case and may approve up to 250 storage accounts for a given region. 

