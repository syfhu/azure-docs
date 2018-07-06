---
title: Frequently asked questions (FAQ) for Azure Files | Microsoft Docs
description: Find answers to frequently asked questions about Azure Files.
services: storage
documentationcenter: ''
author: RenaShahMSFT
manager: aungoo
editor: tamram

ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.date: 05/31/2018
ms.author: renash
---

# Frequently asked questions (FAQ) about Azure Files
[Azure Files](storage-files-introduction.md) offers fully managed file shares in the cloud that are accessible via the industry-standard [Server Message Block (SMB) protocol](https://msdn.microsoft.com/library/windows/desktop/aa365233.aspx). You can mount Azure file shares concurrently on cloud or on-premises deployments of Windows, Linux, and macOS. You also can cache Azure file shares on Windows Server machines by using Azure File Sync (preview) for fast access close to where the data is used.

This article answers common questions about Azure Files features and functionality, including the use of Azure File Sync with Azure Files. If you don't see the answer to your question, you can contact us through the following channels (in escalating order):

1. The comments section of this article.
2. [Azure Storage Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazuredata).
3. [Azure Files UserVoice](https://feedback.azure.com/forums/217298-storage/category/180670-files). 
4. Microsoft Support. To create a new support request, in the Azure portal, on the **Help** tab, select the **Help + support** button, and then select **New support request**.

## General
* <a id="why-files-useful"></a>
**How is Azure Files useful?**  
   You can use Azure Files to create file shares in the cloud, without being responsible for managing the overhead of a physical server, device, or appliance. We do the monotonous work for you, including applying OS updates and replacing bad disks. To learn more about the scenarios that Azure Files can help you with, see [Why Azure Files is useful](storage-files-introduction.md#why-azure-files-is-useful).

* <a id="file-access-options"></a>
**What are different ways to access files in Azure Files?**  
    You can mount the file share on your local machine by using the SMB 3.0 protocol, or you can use tools like [Storage Explorer](http://storageexplorer.com/) to access files in your file share. From your application, you can use storage client libraries, REST APIs, PowerShell, or Azure CLI to access your files in the Azure file share.

* <a id="what-is-afs"></a>
**What is Azure File Sync?**  
    You can use Azure File Sync to centralize your organization's file shares in Azure Files, while keeping the flexibility, performance, and compatibility of an on-premises file server. Azure File Sync transforms your Windows Server machines into a quick cache of your Azure file share. You can use any protocol that's available on Windows Server to access your data locally, including SMB, Network File System (NFS), and File Transfer Protocol Service (FTPS). You can have as many caches as you need across the world.

* <a id="files-versus-blobs"></a>
**Why would I use an Azure file share versus Azure Blob storage for my data?**  
    Azure Files and Azure Blob storage both offer ways to store large amounts of data in the cloud, but they are useful for slightly different purposes. 
    
    Azure Blob storage is useful for massive-scale, cloud-native applications that need to store unstructured data. To maximize performance and scale, Azure Blob storage is a simpler storage abstraction than a true file system. You can access Azure Blob storage only through REST-based client libraries (or directly through the REST-based protocol).

    Azure Files is specifically a file system. Azure Files has all the file abstracts that you know and love from years of working with on-premises operating systems. Like Azure Blob storage, Azure Files offers a REST interface and REST-based client libraries. Unlike Azure Blob storage, Azure Files offers SMB access to Azure file shares. By using SMB, you can mount an Azure file share directly on Windows, Linux, or macOS, either on-premises or in cloud VMs, without writing any code or attaching any special drivers to the file system. You also can cache Azure file shares on on-premises file servers by using Azure File Sync for quick access, close to where the data is used. 
   
    For a more in-depth description on the differences between Azure Files and Azure Blob storage, see [Deciding when to use Azure Blob storage, Azure Files, or Azure Disks](../common/storage-decide-blobs-files-disks.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json). To learn more about Azure Blob storage, see [Introduction to Blob storage](../blobs/storage-blobs-introduction.md).

* <a id="files-versus-disks"></a>**Why would I use an Azure file share instead of Azure Disks?**  
    A disk in Azure Disks is simply a disk. A standalone disk by itself is not too useful. To get value from Azure Disks, you must attach a disk to a virtual machine that's running in Azure. Azure Disks can be used for everything that you would use a disk for on an on-premises server. You can use it as an OS system disk, as swap space for an OS, or as dedicated storage for an application. An interesting use for Azure Disks is to create a file server in the cloud to use in the same places where you might use an Azure file share. Deploying a file server in Azure Virtual Machines is a high-performance way to get file storage in Azure when you require deployment options that currently are not supported by Azure Files (such as NFS protocol support or premium storage). 

    However, running a file server with Azure Disks as back-end storage typically is much more expensive than using an Azure file share, for a few reasons. First, in addition to paying for disk storage, you also must pay for the expense of running one or more Azure VMs. Second, you also must manage the VMs that are used to run the file server. For example, you are responsible for OS upgrades. Finally, if you ultimately require data to be cached on-premises, it's up to you to set up and manage replication technologies, such as Distributed File System Replication (DFSR), to make that happen.

    One approach to getting the best of both Azure Files and a file server that's hosted in Azure Virtual Machines (in addition to using Azure Disks as back-end storage) is to install Azure File Sync on a file server that's hosted on a cloud VM. If the Azure file share is in the same region as your file server, you can enable cloud tiering and set the volume of free space percentage to maximum (99%). This ensures minimal duplication of data. You also can use any applications you want with your file servers, like applications that require NFS protocol support.

    For information about an option for setting up a high-performance and highly available file server in Azure, see [Deploying IaaS VM guest clusters in Microsoft Azure](https://blogs.msdn.microsoft.com/clustering/2017/02/14/deploying-an-iaas-vm-guest-clusters-in-microsoft-azure/). For a more in-depth description of the differences between Azure Files and Azure Disks, see [Deciding when to use Azure Blob storage, Azure Files, or Azure Disks](../common/storage-decide-blobs-files-disks.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json). To learn more about Azure Disks, see [Azure Managed Disks overview](../../virtual-machines/windows/managed-disks-overview.md).

* <a id="get-started"></a>
**How do I get started using Azure Files?**  
   Getting started with Azure Files is easy. First, [create a file share](storage-how-to-create-file-share.md), and then mount it in your preferred operating system: 

    * [Mount in Windows](storage-how-to-use-files-windows.md)
    * [Mount in Linux](storage-how-to-use-files-linux.md)
    * [Mount in macOS](storage-how-to-use-files-mac.md)

   For a more in-depth guide about deploying an Azure file share to replace production file shares in your organization, see [Planning for an Azure Files deployment](storage-files-planning.md).

* <a id="redundancy-options"></a>
**What storage redundancy options are supported by Azure Files?**  
    Currently, Azure Files supports locally redundant storage (LRS), zone redundant storage (ZRS), and geo-redundant storage (GRS). We plan to support read-access geo-redundant (RA-GRS) storage in the future, but we don't have timelines to share at this time.

* <a id="tier-options"></a>
**What storage tiers are supported in Azure Files?**  
    Currently, Azure Files supports only the standard storage tier. We don't have timelines to share for premium storage and cool storage support at this time. 
    
    > [!NOTE]
    > You cannot create Azure file shares from blob-only storage accounts or from premium storage accounts.

* <a id="give-us-feedback"></a>
**I really want to see a specific feature added to Azure Files. Can you add it?**  
    The Azure Files team is interested in hearing any and all feedback you have about our service. Please vote on feature requests at [Azure Files UserVoice](https://feedback.azure.com/forums/217298-storage/category/180670-files)! We're looking forward to delighting you with many new features.

## Azure File Sync

* <a id="afs-region-availability"></a>
**What regions are supported for Azure File Sync (preview)?**  
    Currently, Azure File Sync is available in Australia East, Australia Southeast, Canada Central, Canada East, Central US, East Asia, East US, East US2, North Europe, Southeast Asia, UK South, UK West, West Europe and West US. Support for more regions will be added as we work toward general availability. For more information, see [Region availability](storage-sync-files-planning.md#region-availability).

* <a id="cross-domain-sync"></a>
**Can I have domain-joined and non-domain-joined servers in the same sync group?**  
    Yes. A sync group can contain server endpoints that have different Active Directory memberships, even if they are not domain-joined. Although this configuration technically works, we do not recommend this as a typical configuration because access control lists (ACLs) that are defined for files and folders on one server might not be able to be enforced by other servers in the sync group. For best results, we recommend syncing between servers that are in the same Active Directory forest, between servers that are in different Active Directory forests but which have established trust relationships, or between servers that are not in a domain. We recommend that you avoid using a mix of these configurations.

* <a id="afs-change-detection"></a>
**I created a file directly in my Azure file share by using SMB or in the portal. How long does it take for the file to sync to the servers in the sync group?**  
    [!INCLUDE [storage-sync-files-change-detection](../../../includes/storage-sync-files-change-detection.md)]

* <a id="afs-conflict-resolution"></a>**If the same file is changed on two servers at approximately the same time, what happens?**  
    Azure File Sync uses a simple conflict-resolution strategy: we keep both changes to files that are changed on two servers at the same time. The most recently written change keeps the original file name. The older file has the "source" machine and the conflict number appended to the name. It follows this taxonomy: 
   
    \<FileNameWithoutExtension\>-\<MachineName\>\[-#\].\<ext\>  

    For example, the first conflict of CompanyReport.docx would become CompanyReport-CentralServer.docx if CentralServer is where the older write occurred. The second conflict would be named CompanyReport-CentralServer-1.docx.

* <a id="afs-storage-redundancy"></a>
**Is geo-redundant storage supported for Azure File Sync?**  
    Yes, Azure Files supports both locally redundant storage (LRS) and geo-redundant storage (GRS). If a GRS failover between paired regions occurs, we recommend that you treat the new region as a backup of data only. Azure File Sync does not automatically begin syncing with the new primary region. 

* <a id="sizeondisk-versus-size"></a>
**Why doesn't the *Size on disk* property for a file match the *Size* property after using Azure File Sync?**  
    Windows File Explorer exposes two properties to represent the size of a file: **Size** and **Size on disk**. These properties differ subtly in meaning. **Size** represents the complete size of the file. **Size on disk** represents the size of the file stream that's stored on the disk. The values for these properties can differ for a variety of reasons, such as compression, use of Data Deduplication, or cloud tiering with Azure File Sync. If a file is tiered to an Azure file share, the size on the disk is zero, because the file stream is stored in your Azure file share, and not on the disk. It's also possible for a file to be partially tiered (or partially recalled). In a partially tiered file, part of the file is on disk. This might occur when files are partially read by applications like multimedia players or zip utilities. 

* <a id="is-my-file-tiered"></a>
**How can I tell whether a file has been tiered?**  
    There are several ways to check whether a file has been tiered to your Azure file share:
    
   *  **Check the file attributes on the file.**
     To do this, right-click on a file, go to **Details**, and then scroll down to the **Attributes** property. A tiered file has the following attributes set:     
        
        | Attribute letter | Attribute | Definition |
        |:----------------:|-----------|------------|
        | A | Archive | Indicates that the file should be backed up by backup software. This attribute is always set, regardless of whether the file is tiered or stored fully on disk. |
        | P | Sparse file | Indicates that the file is a sparse file. A sparse file is a specialized type of file that NTFS offers for efficient use when the file on the disk stream is mostly empty. Azure File Sync uses sparse files because a file is either fully tiered or partially recalled. In a fully tiered file, the file stream is stored in the cloud. In a partially recalled file, that part of the file is already on disk. If a file is fully recalled to disk, Azure File Sync converts it from a sparse file to a regular file. |
        | L | Reparse point | Indicates that the file has a reparse point. A reparse point is a special pointer for use by a file system filter. Azure File Sync uses reparse points to define to the Azure File Sync file system filter (StorageSync.sys) the cloud location where the file is stored. This supports seamless access. Users won't need to know that Azure File Sync is being used or how to get access to the file in your Azure file share. When a file is fully recalled, Azure File Sync removes the reparse point from the file. |
        | O | Offline | Indicates that some or all of the file's content is not stored on disk. When a file is fully recalled, Azure File Sync removes this attribute. |

        ![The Properties dialog box for a file, with the Details tab selected](media/storage-files-faq/azure-file-sync-file-attributes.png)
        
        You can see the attributes for all the files in a folder by adding the **Attributes** field to the table display of File Explorer. To do this, right-click on an existing column (for example, **Size**), select **More**, and then select **Attributes** from the drop-down list.
        
   * **Use `fsutil` to check for reparse points on a file.**
       As described in the preceding option, a tiered file always has a reparse point set. A reparse pointer is a special pointer for the Azure File Sync file system filter (StorageSync.sys). To check whether a file has a reparse point, in an elevated Command Prompt or PowerShell window, run the `fsutil` utility:
    
        ```PowerShell
        fsutil reparsepoint query <your-file-name>
        ```

        If the file has a reparse point, you can expect to see **Reparse Tag Value : 0x8000001e**. This hexadecimal value is the reparse point value that is owned by Azure File Sync. The output also contains the reparse data that represents the path to your file on your Azure file share.

        > [!WARNING]  
        > The `fsutil reparsepoint` utility command also has the ability to delete a reparse point. Do not execute this command unless the Azure File Sync engineering team asks you to. Running this command might result in data loss. 

* <a id="afs-recall-file"></a>**A file I want to use has been tiered. How can I recall the file to disk to use it locally?**  
    The easiest way to recall a file to disk is to open the file. The Azure File Sync file system filter (StorageSync.sys) seamlessly downloads the file from your Azure file share without any work on your part. For file types that can be partially read from, such as multimedia or .zip files, opening a file doesn't download the entire file.

    You also can use PowerShell to force a file to be recalled. This option might be useful if you want to recall multiple files at once, such as all the files in a folder. Open a PowerShell session to the server node where Azure File Sync is installed, and then run the following PowerShell commands:
    
    ```PowerShell
    Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.ServerCmdlets.dll"
    Invoke-StorageSyncFileRecall -Path <file-or-directory-to-be-recalled>
    ```

* <a id="afs-force-tiering"></a>
**How do I force a file or directory to be tiered?**  
    When the cloud tiering feature is enabled, cloud tiering automatically tiers files based on last access and modify times to achieve the volume free space percentage specified on the cloud endpoint. Sometimes, though, you might want to manually force a file to tier. This might be useful if you save a large file that you don't intend to use again for a long time, and you want the free space on your volume now to use for other files and folders. You can force tiering by using the following PowerShell commands:

    ```PowerShell
    Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.ServerCmdlets.dll"
    Invoke-StorageSyncCloudTiering -Path <file-or-directory-to-be-tiered>
    ```

* <a id="afs-effective-vfs"></a>
**How is *volume free space* interpreted when I have multiple server endpoints on a volume?**  
    When there is more than one server endpoint on a volume, the effective volume free space threshold is the largest volume free space specified across any server endpoint on that volume. Files will be tiered according to their usage patterns regardless of which server endpoint to which they belong. For example, if you have two server endpoints on a volume, Endpoint1 and Endpoint2, where Endpoint1 has a volume free space threshold of 25% and Endpoint2 has a volume free space threshold of 50%, the volume free space threshold for both server endpoints will be 50%.

* <a id="afs-files-excluded"></a>
**Which files or folders are automatically excluded by Azure File Sync?**  
    By default, Azure File Sync excludes the following files:
    * desktop.ini
    * thumbs.db
    * ehthumbs.db
    * ~$\*.\*
    * \*.laccdb
    * \*.tmp
    * 635D02A9D91C401B97884B82B3BCDAEA.\*

    The following folders are also excluded by default:

    * \System Volume Information
    * \$RECYCLE.BIN
    * \SyncShareState

* <a id="afs-os-support"></a>
**Can I use Azure File Sync with either Windows Server 2008 R2, Linux, or my network-attached storage (NAS) device?**  
    Currently, Azure File Sync supports only Windows Server 2016 and Windows Server 2012 R2. At this time, we don't have any other plans we can share, but we're open to supporting additional platforms based on customer demand. Let us know at [Azure Files UserVoice](https://feedback.azure.com/forums/217298-storage/category/180670-files) what platforms you would like us to support.

* <a id="afs-tiered-files-out-of-endpoint"></a>
**Why do tiered files exist outside of the server endpoint namespace?**  
    Prior to Azure File Sync agent version 3, Azure File Sync blocked the move of tiered files outside the server endpoint but on the same volume as the server endpoint. Copy operations, moves of non-tiered files, and moves of tiered to other volumes were unaffected. The reason for this behavior was the implicit assumption that File Explorer and other Windows APIs have that move operations on the same volume are (nearly) instanenous rename operations. This means moves will make File Explorer or other move methods (such as command line or PowerShell) appear unresponsive while Azure File Sync recalls the data from the cloud. Starting with [Azure File Sync agent version 3.0.12.0](storage-files-release-notes.md#agent-version-30120), Azure File Sync will allow you to move a tiered file outside of the server endpoint. We avoid the negative effects previously mentioned by allowing the tiered file to exist as a tiered file outside of the server endpoint and then recalling the file in the background. This means that moves on the same volume are instaneous, and we do all the work to recall the file to disk after the move has completed. 

* <a id="afs-do-not-delete-server-endpoint"></a>
**I'm having an issue with Azure File Sync on my server (sync, cloud tiering, etc). Should I remove and recreate my server endpoint?**  
    [!INCLUDE [storage-sync-files-remove-server-endpoint](../../../includes/storage-sync-files-remove-server-endpoint.md)]

## Security, authentication, and access control
* <a id="ad-support"></a>
**Is Active Directory-based authentication and access control supported by Azure Files?**  
    Azure Files offers two ways to manage access control:

    - You can use shared access signatures (SAS) to generate tokens that have specific permissions, and which are valid for a specified time interval. For example, you can generate a token with read-only access to a specific file that has a 10-minute expiry. Anyone who possesses the token while the token is valid has read-only access to that file for those 10 minutes. Currently, shared access signature keys are supported only via the REST API or in client libraries. You must mount the Azure file share over SMB by using the storage account keys.

    - Azure File Sync preserves and replicates all discretionary ACLs, or DACLs, (whether Active Directory-based or local) to all server endpoints that it syncs to. Because Windows Server can already authenticate with Active Directory, Azure File Sync is an effective stop-gap option until full support for Active Directory-based authentication and ACL support arrives.

    Currently, Azure Files does not support Active Directory directly.

* <a id="encryption-at-rest"></a>
**How can I ensure that my Azure file share is encrypted at rest?**  
    Azure Storage Service Encryption is in the process of being enabled by default in all regions. For these regions, you don't need to take any actions to enable encryption. For other regions, see [Server-side encryption](../common/storage-service-encryption.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).

* <a id="access-via-browser"></a>
**How can I provide access to a specific file by using a web browser?**  
    You can use shared access signatures to generate tokens that have specific permissions, and which are valid for a specified time interval. For example, you can generate a token that gives read-only access to a specific file, for a set period of time. Anyone who possesses the URL can access the file directly from any web browser while the token is valid. You can easily generate a shared access signature key from a UI like Storage Explorer.

* <a id="file-level-permissions"></a>
**Is it possible to specify read-only or write-only permissions on folders within the share?**  
    If you mount the file share by using SMB, you don't have folder-level control over permissions. However, if you create a shared access signature by using the REST API or client libraries, you can specify read-only or write-only permissions on folders within the share.

* <a id="ip-restrictions"></a>
**Can I implement IP restrictions for an Azure file share?**  
    Yes. Access to your Azure file share can be restricted at the storage account level. For more information, see [Configure Azure Storage Firewalls and Virtual Networks](../common/storage-network-security.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).

* <a id="data-compliance-policies"></a>
**What data compliance policies does Azure Files support?**  
   Azure Files runs on top of the same storage architecture that's used in other storage services in Azure Storage. Azure Files applies the same data compliance policies that are used in other Azure storage services. For more information about Azure Storage data compliance, you can download and refer to the [Microsoft Azure Data Protection document](http://go.microsoft.com/fwlink/?LinkID=398382&clcid=0x409), and go to the [Microsoft Trust Center](https://microsoft.com/en-us/trustcenter/default.aspx).

## On-premises access
* <a id="expressroute-not-required"></a>
**Do I have to use Azure ExpressRoute to connect to Azure Files or to use Azure File Sync on-premises?**  
    No. ExpressRoute is not required to access an Azure file share. If you are mounting an Azure file share directly on-premises, all that's required is to have port 445 (TCP outbound) open for internet access (this is the port that SMB uses to communicate). If you're using Azure File Sync, all that's required is port 443 (TCP outbound) for HTTPS access (no SMB required). However, you *can* use ExpressRoute with either of these access options.

* <a id="mount-locally"></a>
**How can I mount an Azure file share on my local machine?**  
    You can mount the file share by using the SMB protocol if port 445 (TCP outbound) is open and your client supports the SMB 3.0 protocol (for example, if you're using Windows 10 or Windows Server 2016). If port 445 is blocked by your organization's policy or by your ISP, you can use Azure File Sync to access your Azure file share.

## Backup
* <a id="backup-share"></a>
**How do I back up my Azure file share?**  
    You can use periodic [share snapshots](storage-snapshots-files.md) for protection against accidental deletions. You also can use AzCopy, Robocopy, or a third-party backup tool that can back up a mounted file share. Azure Backup offers backup of Azure Files. Learn more about [back up Azure file shares by Azure Backup](https://docs.microsoft.com/en-us/azure/backup/backup-azure-files).

## Share snapshots
### Share snapshots: General
* <a id="what-are-snaphots"></a>
**What are file share snapshots?**  
    You can use Azure file share snapshots to create a read-only version of your file shares. You also can use Azure Files to copy an earlier version of your content back to the same share, to an alternate location in Azure, or on-premises for more modifications. To learn more about share snapshots, see the [Share snapshot overview](storage-snapshots-files.md).

* <a id="where-are-snapshots-stored"></a>
**Where are my share snapshots stored?**  
    Share snapshots are stored in the same storage account as the file share.

* <a id="snapshot-perf-impact"></a>
**Are there any performance implications for using share snapshots?**  
    Share snapshots do not have any performance overhead.

* <a id="snapshot-consistency"></a>
**Are share snapshots application-consistent?**  
    No, share snapshots are not application-consistent. The user must flush the writes from the application to the share before taking the share snapshot.

* <a id="snapshot-limits"></a>
**Are there limits on the number of share snapshots I can use?**  
    Yes. Azure Files can retain a maximum of 200 share snapshots. Share snapshots do not count toward the share quota, so there is no per-share limit on the total space that's used by all the share snapshots. Storage account limits still apply. After 200 share snapshots, you must delete older snapshots to create new share snapshots.
* <a id="snapshot-cost"></a>
**How much does share snapshot cost?**  
    Standard transaction and standard storage cost will apply to snapshot. Snapshots are incremental in nature. The base snapshot is the share itself. All the subsequent snapshots are incremental and will only store the diff from the previous snapshot. This means that the delta changes that will be seen in the bill will be minimal if your workload churn is minimal. See [Pricing page](https://azure.microsoft.com/pricing/details/storage/files/) for Standard Azure Files pricing information. Today the way to look at size consumed by share snapshot is by comparing the billed capacity with used capacity. We are working on tooling to improve the reporting.


### Create share snapshots
* <a id="file-snaphsots"></a>
**Can I create share snapshot of individual files?**  
    Share snapshots are created at the file share level. You can restore individual files from the file share snapshot, but you cannot create file-level share snapshots. However, if you have taken a share-level share snapshot and you want to list share snapshots where a specific file has changed, you can do this under **Previous Versions** on a Windows-mounted share. 
    
    If you need a file snapshot feature, let us know at [Azure Files UserVoice](https://feedback.azure.com/forums/217298-storage/category/180670-files).

* <a id="encypted-snapshots"></a>
**Can I create share snapshots of an encrypted file share?**  
    You can take a share snapshot of Azure file shares that have encryption at rest enabled. You can restore files from a share snapshot to an encrypted file share. If your share is encrypted, your share snapshot also is encrypted.

* <a id="geo-redundant-snaphsots"></a>
**Are my share snapshots geo-redundant?**  
    Share snapshots have the same redundancy as the Azure file share for which they were taken. If you have selected geo-redundant storage for your account, your share snapshot also is stored redundantly in the paired region.

### Manage share snapshots
* <a id="browse-snapshots-linux"></a>
**Can I browse my share snapshots from Linux?**  
    You can use Azure CLI 2.0 to create, list, browse, and restore share snapshots in Linux.

* <a id="copy-snapshots-to-other-storage-account"></a>
**Can I copy the share snapshots to a different storage account?**  
    You can copy files from share snapshots to another location, but you cannot copy the share snapshots themselves.

### Restore data from share snapshots
* <a id="promote-share-snapshot"></a>
**Can I promote a share snapshot to the base share?**  
    You can copy data from a share snapshot to any other destination. You cannot promote a share snapshot to the base share.

* <a id="restore-snapshotted-file-to-other-share"></a>
**Can I restore data from my share snapshot to a different storage account?**  
    Yes. Files from a share snapshot can be copied to the original location or to an alternate location that includes either the same storage account or a different storage account, in either the same region or in different regions. You also can copy files to an on-premises location or to any other cloud.    
  
### Clean up share snapshots
* <a id="delete-share-keep-snapshots"></a>
**Can I delete my share but not delete my share snapshots?**  
    If you have active share snapshots on your share, you cannot delete your share. You can use an API to delete share snapshots, along with the share. You also can delete both the share snapshots and the share in the Azure portal.

* <a id="delete-share-with-snapshots"></a>
**What happens to my share snapshots if I delete my storage account?**  
    If you delete your storage account, the share snapshots also are deleted.

## Billing and pricing
* <a id="vm-file-share-network-traffic"></a>
**Does the network traffic between an Azure VM and an Azure file share count as external bandwidth that is charged to the subscription?**  
    If the file share and VM are in the same Azure region, there is no additional charge for the traffic between the file share and the VM. If the file share and the VM are in different regions, the traffic between them are charged as external bandwidth.

* <a id="share-snapshot-price"></a>
**How much do share snapshots cost?**  
     During preview, there is no charge for share snapshot capacity. Standard storage egress and transaction costs apply. After general availability, subscriptions will be charged for capacity and transactions on share snapshots.
     
     Share snapshots are incremental in nature. The base share snapshot is the share itself. All subsequent share snapshots are incremental and store only the difference from the preceding share snapshot. You are billed only for the changed content. If you have a share with 100 GiB of data but only 5 GiB has changed since your last share snapshot, the share snapshot consumes only 5 additional GiB, and you are billed for 105 GiB. For more information about transaction and standard egress charges, see the [Pricing page](https://azure.microsoft.com/pricing/details/storage/files/).

## Scale and performance
* <a id="files-scale-limits"></a>
**What are the scale limits of Azure Files?**  
    For information about scalability and performance targets for Azure Files, see [Azure Files scalability and performance targets](storage-files-scale-targets.md).

* <a id="need-larger-share"></a>
**I need a larger file share than Azure Files currently offers. Can I increase the size of my Azure file share?**  
    No. The maximum size of an Azure file share is 5 TiB. Currently, this is a hard limit that we cannot adjust. We are working on a solution to increase the share size to 100 TiB, but we don't have timelines to share at this time.

* <a id="open-handles-quota"></a>
**How many clients can access the same file simultaneously?**   
    There is a quota of 2,000 open handles on a single file. When you have 2,000 open handles, an error message is displayed that says the quota is reached.

* <a id="zip-slow-performance"></a>
**My performance is slow when I unzip files in Azure Files. What should I do?**  
    To transfer large numbers of files to Azure Files, we recommend that you use AzCopy (for Windows; in preview for Linux and UNIX) or Azure PowerShell. These tools have been optimized for network transfer.

* <a id="slow-perf-windows-81-2012r2"></a>
**Why is my performance slow after I mount my Azure file share on Windows Server 2012 R2 or Windows 8.1?**  
    There is a known issue when mounting an Azure file share on Windows Server 2012 R2 and Windows 8.1. The issue was patched in the April 2014 cumulative update for Windows 8.1 and Windows Server 2012 R2. For optimum performance, ensure that all instances of Windows Server 2012 R2 and Windows 8.1 have this patch applied. (You should always receive  Windows patches through Windows Update.) For more information, see the associated Microsoft Knowledge Base article [Slow performance when you access Azure Files from Windows 8.1 or Server 2012 R2](https://support.microsoft.com/kb/3114025).

## Features and interoperability with other services
* <a id="cluster-witness"></a>
**Can I use my Azure file share as a *File Share Witness* for my Windows Server Failover Cluster?**  
    Currently, this configuration is not supported for an Azure file share. For more information about how to set this up for Azure Blob storage, see [Deploy a Cloud Witness for a Failover Cluster](https://docs.microsoft.com/windows-server/failover-clustering/deploy-cloud-witness).

* <a id="containers"></a>
**Can I mount an Azure file share on an Azure Container instance?**  
    Yes, Azure file shares are a good option when you want to persist information beyond the lifetime of a container instance. For more information, see [Mount an Azure file share with Azure Container instances](../../container-instances/container-instances-mounting-azure-files-volume.md).

* <a id="rest-rename"></a>
**Is there a rename operation in the REST API?**  
    Not at this time.

* <a id="nested-shares"></a>
**Can I set up nested shares? In other words, a share under a share?**  
    No. The file share *is* the virtual driver that you can mount, so nested shares are not supported.

* <a id="ibm-mq"></a>
**How do I use Azure Files with IBM MQ?**  
    IBM has released a document that helps IBM MQ customers configure Azure Files with the IBM service. For more information, see [How to set up an IBM MQ multi-instance queue manager with Microsoft Azure Files service](https://github.com/ibm-messaging/mq-azure/wiki/How-to-setup-IBM-MQ-Multi-instance-queue-manager-with-Microsoft-Azure-File-Service).

## See also
* [Troubleshoot Azure Files in Windows](storage-troubleshoot-windows-file-connection-problems.md)
* [Troubleshoot Azure Files in Linux](storage-troubleshoot-linux-file-connection-problems.md)
* [Troubleshoot Azure File Sync (preview)](storage-sync-files-troubleshoot.md)
