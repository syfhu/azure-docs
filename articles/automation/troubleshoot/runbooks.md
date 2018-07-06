---
title: Troubleshoot errors with Azure Automation Runbooks
description: Learn how to troubleshoot issues with Azure Automation runbooks
services: automation
author: georgewallace
ms.author: gwallace
ms.date: 06/19/2018
ms.topic: conceptual
ms.service: automation
manager: carmonm
---
# Troubleshoot errors with runbooks

## Authentication errors when working with Azure Automation runbooks

### <a name="sign-in-failed"></a>Scenario: Sign in to Azure Account failed

#### Issue

You receive the following error when working with the `Add-AzureAccount` or `Connect-AzureRmAccount` cmdlets.
:

```
Unknown_user_type: Unknown User Type
```

#### Cause

This error occurs if the credential asset name is not valid or if the username and password that you used to set up the Automation credential asset are not valid.

#### Resolution

In order to determine what's wrong, take the following steps:  

1. Make sure that you don’t have any special characters, including the **@** character in the Automation credential asset name that you are using to connect to Azure.  
2. Check that you can use the username and password that are stored in the Azure Automation credential in your local PowerShell ISE editor. You can do this by running the following cmdlets in the PowerShell ISE:  

   ```powershell
   $Cred = Get-Credential  
   #Using Azure Service Management   
   Add-AzureAccount –Credential $Cred  
   #Using Azure Resource Manager  
   Connect-AzureRmAccount –Credential $Cred
   ```

3. If your authentication fails locally, this means that you haven’t set up your Azure Active Directory credentials properly. Refer to [Authenticating to Azure using Azure Active Directory](https://azure.microsoft.com/blog/azure-automation-authenticating-to-azure-using-azure-active-directory/) blog post to get the Azure Active Directory account set up correctly.  

### <a name="unable-to-find-subscription"></a>Scenario: Unable to find the Azure subscription

#### Issue

You receive the following error when working with the `Select-AzureSubscription` or `Select-AzureRmSubscription` cmdlets.:

```
The subscription named <subscription name> cannot be found.
```

#### Error

This error occurs if the subscription name is not valid or if the Azure Active Directory user who is trying to get the subscription details is not configured as an admin of the subscription.

#### Resolution

In order to determine if you have properly authenticated to Azure and have access to the subscription you are trying to select, take the following steps:  

1. Make sure that you run the **Add-AzureAccount** before running the **Select-AzureSubscription** cmdlet.  
2. If you still see this error message, modify your code by adding the **Get-AzureSubscription** cmdlet following the **Add-AzureAccount** cmdlet and then execute the code. Now verify if the output of Get-AzureSubscription contains your subscription details.  

   * If you don't see any subscription details in the output, this means that the subscription isn’t initialized yet.  
   * If you do see the subscription details in the output, confirm that you are using the correct subscription name or ID with the **Select-AzureSubscription** cmdlet.

### <a name="auth-failed-mfa"></a>Scenario: Authentication to Azure failed because multi-factor authentication is enabled

#### Issue

You receive the the following error when authenticating to Azure with your Azure username and password:

```
Add-AzureAccount: AADSTS50079: Strong authentication enrollment (proof-up) is required
```

#### Cause

If you have multi-factor authentication on your Azure account, you can't use an Azure Active Directory user to authenticate to Azure. Instead, you need to use a certificate or a service principal to authenticate to Azure.

#### Resolution

To use a certificate with the Azure classic deployment model cmdlets, refer to [creating and adding a certificate to manage Azure services.](http://blogs.technet.com/b/orchestrator/archive/2014/04/11/managing-azure-services-with-the-microsoft-azure-automation-preview-service.aspx) To use a service principal with Azure Resource Manager cmdlets, refer to [creating service principal using Azure portal](../../azure-resource-manager/resource-group-create-service-principal-portal.md) and [authenticating a service principal with Azure Resource Manager.](../../azure-resource-manager/resource-group-authenticate-service-principal.md)

## Common errors when working with runbooks

### <a name="job-attempted-3-times"></a>Scenario: The runbook job start was attempted three times, but it failed to start each time

#### Issue

Your runbook fails with the error:

```
The job was tried three times but it failed
```

#### Cause

This error can be caused by the following reasons:

1. Memory Limit. There are documented limits on how much memory allocated to a Sandbox  [Automation service limits](../../azure-subscription-service-limits.md#automation-limits) so a job may fail it if it is using more than 400 MB of memory.

2. Module Incompatible. This can occur if module dependencies are not correct and if they are not, your runbook typically returns a "Command not found" or "Cannot bind parameter" message.

#### Resolution

Any of the following solutions fix the problem:

* Suggested methods to work within the memory limit are to split the workload between multiple runbooks, not process as much data in memory, not to write unnecessary output from your runbooks, or consider how many checkpoints you write into your PowerShell workflow runbooks.  

* Update your Azure modules by following the steps [How to update Azure PowerShell modules in Azure Automation](../automation-update-azure-modules.md).  

### <a name="fails-deserialized-object"></a>Scenario: Runbook fails because of deserialized object

#### Issue

Your runbook fails with the error:

```
Cannot bind parameter <ParameterName>.

Cannot convert the <ParameterType> value of type Deserialized <ParameterType> to type <ParameterType>.
```

#### Cause

If your runbook is a PowerShell Workflow, it stores complex objects in a deserialized format in order to persist your runbook state if the workflow is suspended.

#### Resolution

Any of the following three solutions fix this problem:

1. If you are piping complex objects from one cmdlet to another, wrap these cmdlets in an InlineScript.
2. Pass the name or value that you need from the complex object instead of passing the entire object.
3. Use a PowerShell runbook instead of a PowerShell Workflow runbook.

### <a name="quota-exceeded"></a>Scenario: Runbook job failed because the allocated quota exceeded

#### Issue

Your runbook job fails with the error:

```
The quota for the monthly total job run time has been reached for this subscription
```

#### Cause

This error occurs when the job execution exceeds the 500-minute free quota for your account. This quota applies to all types of job execution tasks such as testing a job, starting a job from the portal, executing a job by using webhooks and scheduling a job to execute by using either the Azure portal or in your datacenter. To learn more about pricing for Automation, see [Automation pricing](https://azure.microsoft.com/pricing/details/automation/).

#### Resolution

If you want to use more than 500 minutes of processing per month, you need to change your subscription from the Free tier to the Basic tier. You can upgrade to the Basic tier by taking the following steps:  

1. Sign in to your Azure subscription  
2. Select the Automation account you wish to upgrade  
3. Click on **Settings** > **Pricing**.
4. Click **Enable** on page bottom to upgrade your account to the **Basic** tier.

### <a name="cmdlet-not-recognized"></a>Scenario: Cmdlet not recognized when executing a runbook

#### Issue

Your runbook job fails with the error:

```
<cmdlet name>: The term <cmdlet name> is not recognized as the name of a cmdlet, function, script file, or operable program.
```

#### Cause

This error is caused when the PowerShell engine cannot find the cmdlet you are using in your runbook. This could be because the module containing the cmdlet is missing from the account, there is a name conflict with a runbook name, or the cmdlet also exists in another module and Automation cannot resolve the name.

#### Resolution

Any of the following solutions fix the problem:  

* Check that you have entered the cmdlet name correctly.  
* Make sure the cmdlet exists in your Automation account and that there are no conflicts. To verify if the cmdlet is present, open a runbook in edit mode and search for the cmdlet you want to find in the library or run `Get-Command <CommandName>`. Once you have validated that the cmdlet is available to the account, and that there are no name conflicts with other cmdlets or runbooks, add it to the canvas and ensure that you are using a valid parameter set in your runbook.  
* If you do have a name conflict and the cmdlet is available in two different modules, you can resolve this by using the fully qualified name for the cmdlet. For example, you can use **ModuleName\CmdletName**.  
* If you are executing the runbook on-premises in a hybrid worker group, then make sure that the module/cmdlet is installed on the machine that hosts the hybrid worker.

### <a name="evicted-from-checkpoint"></a>Scenario: A long running runbook consistently fails with the exception: "The job cannot continue running because it was repeatedly evicted from the same checkpoint"

#### Issue

This behavior is by design due to the "Fair Share" monitoring of processes within Azure Automation, which automatically suspends a runbook if it executes longer than three hours. However, the error message returned does not provide "what next" options.

#### Cause

A runbook can be suspended for a number of reasons. Suspends happen mostly due to errors. For example, an uncaught exception in a runbook, a network failure, or a crash on the Runbook Worker running the runbook, all cause the runbook to be suspended and start from its last checkpoint when resumed.

#### Resolution

The documented solution to avoid this issue is to use Checkpoints in a workflow. To learn more, refer to [Learning PowerShell Workflows](../automation-powershell-workflow.md#checkpoints). A more thorough explanation of "Fair Share" and Checkpoint can be found in this blog article [Using Checkpoints in Runbooks](https://azure.microsoft.com/blog/azure-automation-reliable-fault-tolerant-runbook-execution-using-checkpoints/).

## Common errors when importing modules

### <a name="module-fails-to-import"></a>Scenario: Module fails to import or cmdlets can't be executed after importing

#### Issue

A module fails to import or imports successfully, but no cmdlets are extracted.

#### Cause

Some common reasons that a module might not successfully import to Azure Automation are:

* The structure does not match the structure that Automation needs it to be in.
* The module is dependent on another module that has not been deployed to your Automation account.
* The module is missing its dependencies in the folder.
* The `New-AzureRmAutomationModule` cmdlet is being used to upload the module, and you have not given the full storage path or have not loaded the module by using a publicly accessible URL.

#### Resolution

Any of the following solutions fix the problem:

* Make sure that the module follows the following format:
  ModuleName.Zip **->** ModuleName or Version Number **->** (ModuleName.psm1, ModuleName.psd1)
* Open the .psd1 file and see if the module has any dependencies. If it does, upload these modules to the Automation account.
* Make sure that any referenced .dlls are present in the module folder.

## Next steps

If you did not see your problem or are unable to solve your issue, visit one of the following channels for more support:

* Get answers from Azure experts through [Azure Forums](https://azure.microsoft.com/support/forums/)
* Connect with [@AzureSupport](https://twitter.com/azuresupport) – the official Microsoft Azure account for improving customer experience by connecting the Azure community to the right resources: answers, support, and experts.
* If you need more help, you can file an Azure support incident. Go to the [Azure support site](https://azure.microsoft.com/support/options/) and select **Get Support**.