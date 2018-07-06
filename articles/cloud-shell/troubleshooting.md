---
title: Azure Cloud Shell troubleshooting | Microsoft Docs
description: Troubleshooting Azure Cloud Shell
services: azure
documentationcenter: ''
author: maertendMSFT
manager: angelc
tags: azure-resource-manager
 
ms.assetid: 
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 07/03/2018
ms.author: damaerte
---

# Troubleshooting & Limitations of Azure Cloud Shell

Known resolutions for troubleshooting issues in Azure Cloud Shell include:

## General troubleshooting

### Early timeouts in FireFox
- **Details**: Cloud Shell utilizes an open websocket to pass input/output to your browser. FireFox has preset policies that can close the websocket prematurely causing early timeouts in Cloud Shell.
- **Resolution**: Open FireFox and navigate to "about:config" in the URL box. Search for "network.websocket.timeout.ping.request" and change the value from 0 to 10.

### Storage Dialog - Error: 403 RequestDisallowedByPolicy
- **Details**: When creating a storage account through Cloud Shell, it is unsuccessful due to an Azure policy placed by your admin. Error message will include: `The resource action 'Microsoft.Storage/storageAccounts/write' is disallowed by one or more policies.`
- **Resolution**: Contact your Azure administrator to remove or update the Azure policy denying storage creation.

### Storage Dialog - Error: 400 DisallowedOperation
 - **Details**: When using an Azure Active Directory subscription, you cannot create storage.
 - **Resolution**: Use an Azure subscription capable of creating storage resources. Azure AD subscriptions are not able to create Azure resources.

### Terminal output - Error: Failed to connect terminal: websocket cannot be established. Press `Enter` to reconnect.
 - **Details**: Cloud Shell requires the ability to establish a websocket connection to Cloud Shell infrastructure.
 - **Resolution**: Check you have configured your network settings to enable sending https requests and websocket requests to domains at *.console.azure.com.

## Bash troubleshooting

### Cannot run the docker daemon

- **Details**: Cloud Shell utilizes a container to host your shell environment, as a result running the daemon is disallowed.
- **Resolution**: Utilize [docker-machine](https://docs.docker.com/machine/overview/), which is installed by default, to manage docker containers from a remote Docker host.

## PowerShell troubleshooting

### GUI applications are not supported

- **Details**: If a user launches a GUI app, the prompt does not return. For example, when a user clones a private GitHub repo that is two factor authentication enabled, a dialog box is displayed for completing the two factor authentication.  
- **Resolution**: Close and reopen the shell.

### Get-Help -online does not open the help page

- **Details**: If a user types `Get-Help Find-Module -online`, one sees an error message such as:
 `Starting a browser to display online Help failed. No program or browser is associated to open the URI http://go.microsoft.com/fwlink/?LinkID=398574.`
- **Resolution**: Copy the url and open it manually on your browser.

### Troubleshooting remote management of Azure VMs

- **Details**: Due to the default Windows Firewall settings for WinRM the user may see the following error:
 `Ensure the WinRM service is running. Remote Desktop into the VM for the first time and ensure it can be discovered.`
- **Resolution**:  Run `Enable-AzureRmVMPSRemoting` to enable all aspects of PowerShell remoting on the target machine.
 

### `dir` caches the result in Azure drive

- **Details**: The result of `dir` is cached in Azure drive.
- **Resolution**: After you create or remove a resource in the Azure drive view, run `dir -force` to update.

## General limitations
Azure Cloud Shell has the following known limitations:

### System state and persistence

The machine that provides your Cloud Shell session is temporary, and it is recycled after your session is inactive for 20 minutes. Cloud Shell requires an Azure file share to be mounted. As a result, your subscription must be able to set up storage resources to access Cloud Shell. Other considerations include:

* With mounted storage, only modifications within the `clouddrive` directory are persisted. In Bash, your `$Home` directory is also persisted.
* Azure file shares can be mounted only from within your [assigned region](persisting-shell-storage.md#mount-a-new-clouddrive).
  * In Bash, run `env` to find your region set as `ACC_LOCATION`.
* Azure Files supports only locally redundant storage and geo-redundant storage accounts.

### Browser support

Cloud Shell supports the latest versions of Microsoft Edge, Microsoft Internet Explorer, Google Chrome, Mozilla Firefox, and Apple Safari. Safari in private mode is not supported.

### Copy and paste

[!include [copy-paste](../../includes/cloud-shell-copy-paste.md)]

### For a given user, only one shell can be active

Users can only launch one type of shell at a time, either **Bash** or **PowerShell**. However, you may have multiple instances of Bash or PowerShell running at one time. Swapping between Bash or PowerShell causes Cloud Shell to restart, which terminates existing sessions.

### Usage limits

Cloud Shell is intended for interactive use cases. As a result, any long-running non-interactive sessions are ended without warning.

## Bash limitations

### User permissions

Permissions are set as regular users without sudo access. Any installation outside your `$Home` directory is not persisted.

### Editing .bashrc

Take caution when editing .bashrc, doing so can cause unexpected errors in Cloud Shell.

## PowerShell limitations

### `AzureAD` module name

The `AzureAD` module name is currently `AzureAD.Standard.Preview`, the module provides the same functionality.

### `SqlServer` module functionality

The `SqlServer` module included in Cloud Shell has only prerelease support for PowerShell Core. In particular, `Invoke-SqlCmd` is not available yet.

### Default file location when created from Azure drive:

Using PowerShell cmdlets, users can not create files under the Azure drive. When users create new files using other tools, such as vim or nano, the files are saved to the `$HOME` by default. 

### GUI applications are not supported

If the user runs a command that would create a Windows dialog box, such as `Connect-AzureAD` or `Connect-AzureRmAccount`, one sees an error message such as: `Unable to load DLL 'IEFRAME.dll': The specified module could not be found. (Exception from HRESULT: 0x8007007E)`.

### Tab completion crashes PSReadline

If the user's EditMode in PSReadline is set to Emacs, the user tries to display all possibilities via tab completion, and the window size is too small to display all the possibilities, PSReadline will crash.

### Large Gap after displaying progress bar

If the user performs an action that displays a progress bar, such a tab completing while in the `Azure:` drive, then it is possible that the cursor is not set properly and a gap appears where the progress bar was previously.

### Random characters appear inline

The cursor position sequence codes, for example `5;13R`, can appear in the user input.  The characters can be manually removed.

## Personal data in Cloud Shell

Azure Cloud Shell takes your personal data seriously, the data captured and stored by the Azure Cloud Shell service are used to provide defaults for your experience such as your most recently used shell, preferred font size, preferred font type, and file share details that back cloud drive. Should you wish to export or delete this data, we have included the following instructions.

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-intro-sentence.md)]

### Export
In order to **export** the user settings Cloud Shell saves for you such as preferred shell, font size, and font type run the following commands.

1. Launch Bash in Cloud Shell
2. Run the following commands:
```
user@Azure:~$ token="Bearer $(curl http://localhost:50342/oauth2/token --data "resource=https://management.azure.com/" -H Metadata:true -s | jq -r ".access_token")"
user@Azure:~$ curl https://management.azure.com/providers/Microsoft.Portal/usersettings/cloudconsole?api-version=2017-12-01-preview -H Authorization:"$token" -s | jq
```

### Delete
In order to **delete** your user settings Cloud Shell saves for you such as preferred shell, font size, and font type run the following commands. The next time you start Cloud Shell you will be asked to onboard a file share again. 

The actual Azure Files share will not be deleted if you delete your user settings, go to Azure Files to complete that action.

1. Launch Bash in Cloud Shell
2. Run the following commands:
```
user@Azure:~$ token="Bearer $(curl http://localhost:50342/oauth2/token --data "resource=https://management.azure.com/" -H Metadata:true -s | jq -r ".access_token")"
user@Azure:~$ curl -X DELETE https://management.azure.com/providers/Microsoft.Portal/usersettings/cloudconsole?api-version=2017-12-01-preview -H Authorization:"$token"
```
