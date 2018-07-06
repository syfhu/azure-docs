---
title: Compiling configurations in Azure Automation DSC
description: This article describes how to compile Desired State Configuration (DSC) configurations for Azure Automation.
services: automation
ms.service: automation
ms.component: dsc
author: georgewallace
ms.author: gwallace
ms.date: 03/16/2018
ms.topic: conceptual
manager: carmonm
---

# Compiling configurations in Azure Automation DSC

You can compile Desired State Configuration (DSC) configurations in two ways with Azure Automation: in the Azure portal, and with Windows PowerShell. The following table helps you determine when to use which method based on the characteristics of each:

### Azure portal

* Simplest method with interactive user interface
* Form to provide simple parameter values
* Easily track job state
* Access authenticated with Azure logon

### Windows PowerShell

* Call from command line with Windows PowerShell cmdlets
* Can be included in automated solution with multiple steps
* Provide simple and complex parameter values
* Track job state
* Client required to support PowerShell cmdlets
* Pass ConfigurationData
* Compile configurations that use credentials

Once you have decided on a compilation method, use the following procedures to start compiling.

## Compiling a DSC Configuration with the Azure portal

1. From your Automation account, click **DSC Configurations**.
2. Click a configuration to open its blade.
3. Click **Compile**.
4. If the configuration has no parameters, you are prompted to confirm whether you want to compile it. If the configuration has parameters, the **Compile Configuration** blade opens so you can provide parameter values. See the following [**Basic Parameters**](#basic-parameters) section for further details on parameters.
5. The **Compilation Job** blade is opened so that you can track the compilation job's status, and the node configurations (MOF configuration documents) it caused to be placed on the Azure Automation DSC Pull Server.

## Compiling a DSC Configuration with Windows PowerShell

You can use [`Start-AzureRmAutomationDscCompilationJob`](/powershell/module/azurerm.automation/start-azurermautomationdsccompilationjob) to start compiling with Windows PowerShell. The following sample code starts compilation of a DSC configuration called **SampleConfig**.

```powershell
Start-AzureRmAutomationDscCompilationJob -ResourceGroupName "MyResourceGroup" -AutomationAccountName "MyAutomationAccount" -ConfigurationName "SampleConfig"
```

`Start-AzureRmAutomationDscCompilationJob` returns a compilation job object that you can use to track its status. You can then use this compilation job object with [`Get-AzureRmAutomationDscCompilationJob`](/powershell/module/azurerm.automation/get-azurermautomationdsccompilationjob) to determine the status of the compilation job, and [`Get-AzureRmAutomationDscCompilationJobOutput`](/powershell/module/azurerm.automation/get-azurermautomationdsccompilationjoboutput) to view its streams (output). The following sample code starts compilation of the **SampleConfig** configuration, waits until it has completed, and then displays its streams.

```powershell
$CompilationJob = Start-AzureRmAutomationDscCompilationJob -ResourceGroupName "MyResourceGroup" -AutomationAccountName "MyAutomationAccount" -ConfigurationName "SampleConfig"

while($CompilationJob.EndTime –eq $null -and $CompilationJob.Exception –eq $null)
{
    $CompilationJob = $CompilationJob | Get-AzureRmAutomationDscCompilationJob
    Start-Sleep -Seconds 3
}

$CompilationJob | Get-AzureRmAutomationDscCompilationJobOutput –Stream Any
```

## Basic Parameters
Parameter declaration in DSC configurations, including parameter types and properties, works the same as in Azure Automation runbooks. See [Starting a runbook in Azure Automation](automation-starting-a-runbook.md) to learn more about runbook parameters.

The following example uses two parameters called **FeatureName** and **IsPresent**, to determine the values of properties in the **ParametersExample.sample** node configuration, generated during compilation.

```powershell
Configuration ParametersExample
{
    param(
        [Parameter(Mandatory=$true)]

        [string] $FeatureName,

        [Parameter(Mandatory=$true)]
        [boolean] $IsPresent
    )

    $EnsureString = "Present"
    if($IsPresent -eq $false)
    {
        $EnsureString = "Absent"
    }

    Node "sample"
    {
        WindowsFeature ($FeatureName + "Feature")
        {
            Ensure = $EnsureString
            Name = $FeatureName
        }
    }
}
```

You can compile DSC Configurations that use basic parameters in the Azure Automation DSC portal, or with Azure PowerShell:

### Portal

In the portal, you can enter parameter values after clicking **Compile**.

![alt text](./media/automation-dsc-compile/DSC_compiling_1.png)

### PowerShell

PowerShell requires parameters in a [hashtable](http://technet.microsoft.com/library/hh847780.aspx) where the key matches the parameter name, and the value equals the parameter value.

```powershell
$Parameters = @{
    "FeatureName" = "Web-Server"
    "IsPresent" = $False
}

Start-AzureRmAutomationDscCompilationJob -ResourceGroupName "MyResourceGroup" -AutomationAccountName "MyAutomationAccount" -ConfigurationName "ParametersExample" -Parameters $Parameters
```

For information about passing PSCredentials as parameters, see <a href="#credential-assets">**Credential Assets**</a> below.

## Composite Resources

**Composite Resources** allow you to use DSC configurations as nested resources inside of a configuration. This enables you to apply multiple configurations to a single resource. See [Composite resources: Using a DSC configuration as a resource](https://docs.microsoft.com/powershell/dsc/authoringresourcecomposite) to learn more about **Composite Resources**

> [!NOTE]
> In order for **Composite Resources** to compile correctly, you must first ensure that any DSC Resources that the composite relies on are first installed in the Azure Automation Account Modules repository or it doesn't import properly.

To add a DSC **Composite Resource**, you must add the resource module to an archive (*.zip). Go to the Modules repository on your Azure Automation Account. Then click on the 'Add a Module' button.

![Add Module](./media/automation-dsc-compile/add_module.png)

Navigate to the directory where your archive is located. Select the archive file, and click OK.

![Select Module](./media/automation-dsc-compile/select_dscresource.png)

You are taken back to the modules directory, where you can monitor the status of your **Composite Resource** while it unpacks and registers with Azure Automation.

![Import Composite Resource](./media/automation-dsc-compile/register_composite_resource.png)

Once the module is registered, you can then click on it to validate that the **Composite Resources** are now available to be used in a configuration.

![Validate Composite Resource](./media/automation-dsc-compile/validate_composite_resource.png)

Then you can call the **Composite Resource** into your configuration like so:

```powershell

    Node ($AllNodes.Where{$_.Role -eq "WebServer"}).NodeName
    {
            
            JoinDomain DomainJoin
            {
                DomainName = $DomainName
                Admincreds = $Admincreds
            }

            PSWAWebServer InstallPSWAWebServer
            {
                DependsOn = "[JoinDomain]DomainJoin"
            }        
    }

```

## ConfigurationData
**ConfigurationData** allows you to separate structural configuration from any environment-specific configuration while using PowerShell DSC. See [Separating "What" from "Where" in PowerShell DSC](http://blogs.msdn.com/b/powershell/archive/2014/01/09/continuous-deployment-using-dsc-with-minimal-change.aspx) to learn more about **ConfigurationData**.

> [!NOTE]
> You can use **ConfigurationData** when compiling in Azure Automation DSC using Azure PowerShell, but not in the Azure portal.

The following example DSC configuration uses **ConfigurationData** via the **$ConfigurationData** and **$AllNodes** keywords. You also need the [**xWebAdministration** module](https://www.powershellgallery.com/packages/xWebAdministration/) for this example:

```powershell
Configuration ConfigurationDataSample
{
    Import-DscResource -ModuleName xWebAdministration -Name MSFT_xWebsite

    Write-Verbose $ConfigurationData.NonNodeData.SomeMessage

    Node $AllNodes.Where{$_.Role -eq "WebServer"}.NodeName
    {
        xWebsite Site
        {
            Name = $Node.SiteName
            PhysicalPath = $Node.SiteContents
            Ensure   = "Present"
        }
    }
}
```

You can compile the preceding DSC configuration with PowerShell. The following PowerShell adds two node configurations to the Azure Automation DSC Pull Server: **ConfigurationDataSample.MyVM1** and **ConfigurationDataSample.MyVM3**:

```powershell
$ConfigData = @{
    AllNodes = @(
        @{
            NodeName = "MyVM1"
            Role = "WebServer"
        },
        @{
            NodeName = "MyVM2"
            Role = "SQLServer"
        },
        @{
            NodeName = "MyVM3"
            Role = "WebServer"
        }
    )

    NonNodeData = @{
        SomeMessage = "I love Azure Automation DSC!"
    }
}

Start-AzureRmAutomationDscCompilationJob -ResourceGroupName "MyResourceGroup" -AutomationAccountName "MyAutomationAccount" -ConfigurationName "ConfigurationDataSample" -ConfigurationData $ConfigData
```

## Assets

Asset references are the same in Azure Automation DSC configurations and runbooks. See the following for more information:

* [Certificates](automation-certificates.md)
* [Connections](automation-connections.md)
* [Credentials](automation-credentials.md)
* [Variables](automation-variables.md)

### Credential Assets

While DSC configurations in Azure Automation can reference credential assets using **Get-AutomationPSCredential**, credential assets can also be passed in via parameters, if desired. If a configuration takes a parameter of **PSCredential** type, then you need to pass the string name of an Azure Automation credential asset as that parameter’s value, rather than a PSCredential object. Behind the scenes, the Azure Automation credential asset with that name is retrieved and passed to the configuration.

Keeping credentials secure in node configurations (MOF configuration documents) requires encrypting the credentials in the node configuration MOF file. Azure Automation takes this one step further and encrypts the entire MOF file. However, currently you must tell PowerShell DSC it is okay for credentials to be outputted in plain text during node configuration MOF generation, because PowerShell DSC doesn’t know that Azure Automation will be encrypting the entire MOF file after its generation via a compilation job.

You can tell PowerShell DSC that it is okay for credentials to be outputted in plain text in the generated node configuration MOFs using [**ConfigurationData**](#configurationdata). You should pass `PSDscAllowPlainTextPassword = $true` via **ConfigurationData** for each node block’s name that appears in the DSC configuration and uses credentials.

The following example shows a DSC configuration that uses an Automation credential asset.

```powershell
Configuration CredentialSample
{
    Import-DscResource -ModuleName PSDesiredStateConfiguration
    $Cred = Get-AutomationPSCredential "SomeCredentialAsset"

    Node $AllNodes.NodeName
    {
        File ExampleFile
        {
            SourcePath = "\\Server\share\path\file.ext"
            DestinationPath = "C:\destinationPath"
            Credential = $Cred
        }
    }
}
```

You can compile the preceding DSC configuration with PowerShell. The following PowerShell adds two node configurations to the Azure Automation DSC Pull Server: **CredentialSample.MyVM1** and **CredentialSample.MyVM2**.

```powershell
$ConfigData = @{
    AllNodes = @(
        @{
            NodeName = "*"
            PSDscAllowPlainTextPassword = $True
        },
        @{
            NodeName = "MyVM1"
        },
        @{
            NodeName = "MyVM2"
        }
    )
}

Start-AzureRmAutomationDscCompilationJob -ResourceGroupName "MyResourceGroup" -AutomationAccountName "MyAutomationAccount" -ConfigurationName "CredentialSample" -ConfigurationData $ConfigData
```

> [!NOTE]
> When compilation is complete you may receive an error stating: **The 'Microsoft.PowerShell.Management' module was not imported because the 'Microsoft.PowerShell.Management' snap-in was already imported.** This warning can safely be ignored.

## Importing node configurations

You can also import node configurations (MOFs) that you have compiled outside of Azure. One advantage of this is that node configurations can be signed.
A signed node configuration is verified locally on a managed node by the DSC agent, ensuring that the configuration being applied to the node comes from an
authorized source.

> [!NOTE]
> You can use import signed configurations into your Azure Automation account, but Azure Automation does not currently support compiling signed configurations.

> [!NOTE]
> A node configuration file must be no larger than 1 MB to allow it to be imported into Azure Automation.

You can learn how to sign node configurations at https://msdn.microsoft.com/powershell/wmf/5.1/dsc-improvements#how-to-sign-configuration-and-module.

### Importing a node configuration in the Azure portal

1. From your Automation account, click **DSC node configurations** under **Configuration Management**.

    ![DSC node configurations](./media/automation-dsc-compile/node-config.png)
2. In the **DSC node configurations** blade, click **Add a NodeConfiguration**.
3. In the **Import** blade, click the folder icon next to the **Node Configuration File** textbox to browse for a node configuration file (MOF) on your local computer.

    ![Browse for local file](./media/automation-dsc-compile/import-browse.png)
4. Enter a name in the **Configuration Name** textbox. This name must match the name of the configuration from which the node configuration was compiled.
5. Click **OK**.

### Importing a node configuration with PowerShell

You can use the [Import-AzureRmAutomationDscNodeConfiguration](/powershell/module/azurerm.automation/import-azurermautomationdscnodeconfiguration)
cmdlet to import a node configuration into your automation account.

```powershell
Import-AzureRmAutomationDscNodeConfiguration -AutomationAccountName "MyAutomationAccount" -ResourceGroupName "MyResourceGroup" -ConfigurationName "MyNodeConfiguration" -Path "C:\MyConfigurations\TestVM1.mof"
```



