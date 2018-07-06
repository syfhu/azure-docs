---
title: Use Azure Files with Linux | Microsoft Docs
description: Learn how to mount an Azure file share over SMB on Linux.
services: storage
documentationcenter: na
author: RenaShahMSFT
manager: aungoo
editor: tamram

ms.assetid: 6edc37ce-698f-4d50-8fc1-591ad456175d
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/29/2018
ms.author: renash
---

# Use Azure Files with Linux
[Azure Files](storage-files-introduction.md) is Microsoft's easy to use cloud file system. Azure file shares can be mounted in Linux distributions using the [SMB kernel client](https://wiki.samba.org/index.php/LinuxCIFS). This article shows two ways to mount an Azure file share: on-demand with the `mount` command and on-boot by creating an entry in `/etc/fstab`.

> [!NOTE]  
> In order to mount an Azure file share outside of the Azure region it is hosted in, such as on-premises or in a different Azure region, the OS must support the encryption functionality of SMB 3.0.

## Prerequisites for mounting an Azure file share with Linux and the cifs-utils package
* **Pick a Linux distribution that can have the cifs-utils package installed.**  
    The following Linux distributions are available for use in the Azure gallery:

    * Ubuntu Server 14.04+
    * RHEL 7+
    * CentOS 7+
    * Debian 8+
    * openSUSE 13.2+
    * SUSE Linux Enterprise Server 12

* <a id="install-cifs-utils"></a>**The cifs-utils package is installed.**  
    The cifs-utils package can be installed using the package manager on the Linux distribution of your choice. 

    On **Ubuntu** and **Debian-based** distributions, use the `apt-get` package manager:

    ```bash
    sudo apt-get update
    sudo apt-get install cifs-utils
    ```

    On **RHEL** and **CentOS**, use the `yum` package manager:

    ```bash
    sudo yum install cifs-utils
    ```

    On **openSUSE**, use the `zypper` package manager:

    ```bash
    sudo zypper install cifs-utils
    ```

    On other distributions, use the appropriate package manager or [compile from source](https://wiki.samba.org/index.php/LinuxCIFS_utils#Download).

* <a id="smb-client-reqs"></a>**Understand SMB client requirements.**  
    Azure Files can be mounted either via SMB 2.1 and SMB 3.0. For connections coming from clients on-premises or in other Azure regions, Azure Files will reject SMB 2.1 (or SMB 3.0 without encryption). If *secure transfer required* is enabled for a storage account, Azure Files will only allow connections using SMB 3.0 with encryption.
    
    SMB 3.0 encryption support was introduced in Linux kernel version 4.11 and has been backported to older kernel versions for popular Linux distributions. At the time of this document's publication, the following distributions from the Azure gallery support this feature:

    - Ubuntu Server 16.04+
    - openSUSE 42.3+
    - SUSE Linux Enterprise Server 12 SP3+
    
    If your Linux distribution is not listed here, you can check to see the Linux kernel version with the following command:

    ```bash
    uname -r
    ```

* **Decide on the directory/file permissions of the mounted share**: In the examples below, the permission `0777` is used to give read, write, and execute permissions to all users. You can replace it with other [chmod permissions](https://en.wikipedia.org/wiki/Chmod) as desired. 

* **Storage account name**: To mount an Azure file share, you need the name of the storage account.

* **Storage account key**: To mount an Azure file share, you need the primary (or secondary) storage key. SAS keys are not currently supported for mounting.

* **Ensure port 445 is open**: SMB communicates over TCP port 445 - check to see if your firewall is not blocking TCP ports 445 from client machine.

## Mount the Azure file share on-demand with `mount`
1. **[Install the cifs-utils package for your Linux distribution](#install-cifs-utils)**.

2. **Create a folder for the mount point**: A folder for a mount point can be created anywhere on the file system, but it's common convention to create this under the `/mnt` folder. For example:

    ```bash
    mkdir /mnt/MyAzureFileShare
    ```

3. **Use the mount command to mount the Azure file share**: Remember to replace `<storage-account-name>`, `<share-name>`, `<smb-version>`, `<storage-account-key>`, and `<mount-point>` with the appropriate information for your environment. If your Linux distribution supports SMB 3.0 with encryption (see [Understand SMB client requirements](#smb-client-reqs) for more information), use `3.0` for `<smb-version>`. For Linux distributions that do not support SMB 3.0 with encryption, use `2.1` for `<smb-version>`. Note that an Azure file share can only be mounted outside of an Azure region (including on-premises or in a different Azure region) with SMB 3.0. 

    ```bash
    sudo mount -t cifs //<storage-account-name>.file.core.windows.net/<share-name> <mount-point> -o vers=<smb-version>,username=<storage-account-name>,password=<storage-account-key>,dir_mode=0777,file_mode=0777,serverino
    ```

> [!Note]  
> When you are done using the Azure file share, you may use `sudo umount <mount-point>` to unmount the share.

## Create a persistent mount point for the Azure file share with `/etc/fstab`
1. **[Install the cifs-utils package for your Linux distribution](#install-cifs-utils)**.

2. **Create a folder for the mount point**: A folder for a mount point can be created anywhere on the file system, but it's common convention to create this under the `/mnt` folder. Wherever you create this, note the absolute path of the folder. For example, the following command creates a new folder under `/mnt` (the path is an absolute path).

    ```bash
    sudo mkdir /mnt/MyAzureFileShare
    ```

3. **Create a credential file to store the username (the storage account name) and password (the storage account key) for the file share.** Remember to replace `<storage-account-name>` and `<storage-account-key>` with the appropriate information for your environment. 

    ```bash
    if [ -d "/etc/smbcredentials" ]; then
        sudo mkdir /etc/smbcredentials
    fi

    if [ ! -f "/etc/smbcredentials/<storage-account-name>.cred" ]; then
        sudo bash -c 'echo "username=<storage-account-name>" >> /etc/smbcredentials/<storage-account-name>.cred'
        sudo bash -c 'echo "password=<storage-account-key>" >> /etc/smbcredentials/<storage-account-name>.cred'
    fi
    ```

4. **Change permissions on the credential file so only root can read or modify the password file.** Since the storage account key is essentially a super-administrator password for the storage account, setting the permissions on the file such that only root can access is important so that lower privilege users cannot retrive the storage account key.   

    ```bash
    sudo chmod 600 /etc/smbcredentials/<storage-account-name>.cred
    ```

5. **Use the following command to append the following line to `/etc/fstab`**: Remember to replace `<storage-account-name>`, `<share-name>`, `<smb-version>`, and `<mount-point>` with the appropriate information for your environment. If your Linux distribution supports SMB 3.0 with encryption (see [Understand SMB client requirements](#smb-client-reqs) for more information), use `3.0` for `<smb-version>`. For Linux distributions that do not support SMB 3.0 with encryption, use `2.1` for `<smb-version>`. Note that an Azure file share can only be mounted outside of an Azure region (including on-premises or in a different Azure region) with SMB 3.0. 

    ```bash
    sudo bash -c 'echo "//<storage-account-name>.file.core.windows.net/<share-name> <mount-point> cifs nofail,vers=<smb-version>,credentials=/etc/smbcredentials/<storage-account-name>.cred,dir_mode=0777,file_mode=0777,serverino" >> /etc/fstab'
    ```

> [!Note]  
> You can use `sudo mount -a` to mount the Azure file share after editing `/etc/fstab` instead of rebooting.

## Feedback
Linux users, we want to hear from you!

The Azure Files for Linux users' group provides a forum for you to share feedback as you evaluate and adopt File storage on Linux. Email [Azure Files Linux Users](mailto:azurefileslinuxusers@microsoft.com) to join the users' group.

## Next steps
See these links for more information about Azure Files.
* [Introduction to Azure Files](storage-files-introduction.md)
* [Planning for an Azure Files deployment](storage-files-planning.md)
* [FAQ](../storage-files-faq.md)
* [Troubleshooting](storage-troubleshoot-linux-file-connection-problems.md)