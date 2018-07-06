---
title: Migrate Azure VMs to Managed Disks | Microsoft Docs
description: Migrate Azure virtual machines created using unmanaged disks in storage accounts to use Managed Disks.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager

ms.assetid:
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 01/03/2018
ms.author: cynthn

---

# Migrate Azure VMs to Managed Disks in Azure

Azure Managed Disks simplifies your storage management by removing the need to separately manage storage accounts.  You can also migrate your existing Azure VMs to Managed Disks to benefit from better reliability of VMs in an Availability Set. It ensures that the disks of different VMs in an Availability Set is sufficiently isolated from each other to avoid single point of failures. It automatically places disks of different VMs in an Availability Set in different Storage scale units (stamps) which limits the impact of single Storage scale unit failures caused due to hardware and software failures.
Based on your needs, you can choose from two types of storage options:

- [Premium Managed Disks](premium-storage.md) are Solid State Drive (SSD) based storage media which delivers high performance, low-latency disk support for virtual machines running I/O-intensive workloads. You can take advantage of the speed and performance of these disks by migrating to Premium Managed Disks.

- [Standard Managed Disks](standard-storage.md) use Hard Disk Drive (HDD) based storage media and are best suited for Dev/Test and other infrequent access workloads that are less sensitive to performance variability.

You can migrate to Managed Disks in following scenarios:

| Migrate...                                            | Documentation link                                                                                                                                                                                                                                                                  |
|----------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Convert stand alone VMs and VMs in an availability set to managed disks   | [Convert VMs to use managed disks](convert-unmanaged-to-managed-disks.md) |
| A single VM from classic to Resource Manager on managed disks     | [Migrate a single VM](migrate-single-classic-to-resource-manager.md)  | 
| All the VMs in a vNet from classic to Resource Manager on managed disks     | [Migrate IaaS resources from classic to Resource Manager](migration-classic-resource-manager-ps.md) and then [Convert a VM from unmanaged disks to managed disks](convert-unmanaged-to-managed-disks.md) | 






## Plan for the conversion to Managed Disks

This section helps you to make the best decision on VM and disk types.


## Location

Pick a location where Azure Managed Disks are available. If you are moving to Premium Managed Disks, also ensure that Premium storage is available in the region where you are planning to move to. See [Azure Services by Region](https://azure.microsoft.com/regions/#services) for up-to-date information on available locations.

## VM sizes

If you are migrating to Premium Managed Disks, you have to update the size of the VM to Premium Storage capable size available in the region where VM is located. Review the VM sizes that are Premium Storage capable. The Azure VM size specifications are listed in [Sizes for virtual machines](sizes.md).
Review the performance characteristics of virtual machines that work with Premium Storage and choose the most appropriate VM size that best suits your workload. Make sure that there is sufficient bandwidth available on your VM to drive the disk traffic.

## Disk sizes

**Premium Managed Disks**

There are seven types of premium managed disks that can be used with your VM and each has specific IOPs and throughput limits. Take into consideration these limits when choosing the Premium disk type for your VM based on the needs of your application in terms of capacity, performance, scalability, and peak loads.

| Premium Disks Type  | P4    | P6    | P10   | P15   | P20   | P30   | P40   | P50   | 
|---------------------|-------|-------|-------|-------|-------|-------|-------|-------|
| Disk size           | 32 GB| 64 GB| 128 GB| 256 GB|512 GB | 1024 GB (1 TB)    | 2048 GB (2 TB)    | 4095 GB (4 TB)    | 
| IOPS per disk       | 120   | 240   | 500   | 1100  |2300              | 5000              | 7500              | 7500              | 
| Throughput per disk | 25 MB per second  | 50 MB per second  | 100 MB per second | 125 MB per second |150 MB per second | 200 MB per second | 250 MB per second | 250 MB per second |

**Standard Managed Disks**

There are seven types of standard managed disks that can be used with your VM. Each of them have different capacity but have same IOPS and throughput limits. Choose the type of Standard Managed disks based on the capacity needs of your application.

| Standard Disk Type  | S4               | S6               | S10              | S15              | S20              | S30              | S40              | S50              | 
|---------------------|------------------|------------------|------------------|------------------|------------------|------------------|------------------|------------------| 
| Disk size           | 30 GB            | 64 GB            | 128 GB           | 256 GB           |512 GB           | 1024 GB (1 TB)   | 2048 GB (2TB)    | 4095 GB (4 TB)   | 
| IOPS per disk       | 500              | 500              | 500              | 500              |500              | 500              | 500             | 500              | 
| Throughput per disk | 60 MB per second | 60 MB per second | 60 MB per second | 60 MB per second |60 MB per second | 60 MB per second | 60 MB per second | 60 MB per second | 

## Disk caching policy

**Premium Managed Disks**

By default, disk caching policy is *Read-Only* for all the Premium data disks, and *Read-Write* for the Premium operating system disk attached to the VM. This configuration setting is recommended to achieve the optimal performance for your application’s IOs. For write-heavy or write-only data disks (such as SQL Server log files), disable disk caching so that you can achieve better application performance.

## Pricing

Review the [pricing for Managed Disks](https://azure.microsoft.com/pricing/details/managed-disks/). Pricing of Premium Managed Disks is same as the Premium Unmanaged Disks. But pricing for Standard Managed Disks is different than Standard Unmanaged Disks.



## Next steps

- Learn more about [Managed Disks](managed-disks-overview.md)
