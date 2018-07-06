---
title: Discover and assess on-premises VMware VMs for migration to Azure with Azure Migrate | Microsoft Docs
description: Describes how to discover and assess on-premises VMware VMs for migration to Azure, using the Azure Migrate service.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: tutorial
ms.date: 07/03/2018
ms.author: raynew
ms.custom: mvc
---

# Discover and assess on-premises VMware VMs for migration to Azure

The [Azure Migrate](migrate-overview.md) services assesses on-premises workloads for migration to Azure.

In this tutorial, you learn how to:

> [!div class="checklist"]
> * Create an account that Azure Migrate uses to discover on-premises VMs
> * Create an Azure Migrate project.
> * Set up an on-premises collector virtual machine (VM), to discover on-premises VMware VMs for assessment.
> * Group VMs and create an assessment.


If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/pricing/free-trial/) before you begin.


## Prerequisites

- **VMware**: The VMs that you plan to migrate must be managed by vCenter Server running version 5.5, 6.0, or 6.5. Additionally, you need one ESXi host running version 5.0 or higher to deploy the collector VM.
- **vCenter Server account**: You need a read-only account to access the vCenter Server. Azure Migrate uses this account to discover the on-premises VMs.
- **Permissions**: On the vCenter Server, you need permissions to create a VM by importing a file in .OVA format.
- **Statistics settings**: The statistics settings for the vCenter Server should be set to level 3 before you start deployment. If lower than level 3, assessment will work, but performance data for storage and network isn't collected. The size recommendations in this case will be done based on performance data for CPU and memory and configuration data for disk and network adapters.

## Create an account for VM discovery

Azure Migrate needs access to VMware servers to automatically discover VMs for assessment. Create a VMware account with the following properties. You specify this account during Azure Migrate setup.

- User type: At least a read-only user
- Permissions: Data Center object –> Propagate to Child Object, role=Read-only
- Details: User assigned at datacenter level, and has access to all the objects in the datacenter.
- To restrict access, assign the No access role with the Propagate to child object, to the child objects (vSphere hosts, datastores, VMs and networks).


## Log in to the Azure portal

Log in to the [Azure portal](https://portal.azure.com).

## Create a project

1. In the Azure portal, click **Create a resource**.
2. Search for **Azure Migrate**, and select the service **Azure Migrate** in the search results. Then click **Create**.
3. Specify a project name, and the Azure subscription for the project.
4. Create a new resource group.
5. Specify the location in which to create the project, then click **Create**. You can only create an Azure Migrate project in the West Central US or East US region. However, you can still plan your migration for any target Azure location. The location specified for the project is only used to store the metadata gathered from on-premises VMs.

    ![Azure Migrate](./media/tutorial-assessment-vmware/project-1.png)



## Download the collector appliance

Azure Migrate creates an on-premises VM known as the collector appliance. This VM discovers on-premises VMware VMs, and sends metadata about them to the Azure Migrate service. To set up the collector appliance, you download an .OVA file, and import it to the on-premises vCenter server to create the VM.

1. In the Azure Migrate project, click **Getting Started** > **Discover & Assess** > **Discover Machines**.
2. In **Discover machines**, click **Download**, to download the .OVA file.
3. In **Copy project credentials**, copy the project ID and key. You need these when you configure the collector.

    ![Download .ova file](./media/tutorial-assessment-vmware/download-ova.png)

### Verify the collector appliance

Check that the .OVA file is secure, before you deploy it.

1. On the machine to which you downloaded the file, open an administrator command window.
2. Run the following command to generate the hash for the OVA:
    - ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```
    - Example usage: ```C:\>CertUtil -HashFile C:\AzureMigrate\AzureMigrate.ova SHA256```
3. The generated hash should match these settings.

  For OVA version 1.0.9.8

    **Algorithm** | **Hash value**
    --- | ---
    MD5 | b5d9f0caf15ca357ac0563468c2e6251
    SHA1 | d6179b5bfe84e123fabd37f8a1e4930839eeb0e5
    SHA256 | 09c68b168719cb93bd439ea6a5fe21a3b01beec0e15b84204857061ca5b116ff

	For OVA version 1.0.9.7

    **Algorithm** | **Hash value**
    --- | ---
    MD5 | d5b6a03701203ff556fa78694d6d7c35
    SHA1 | f039feaa10dccd811c3d22d9a59fb83d0b01151e
    SHA256 | e5e997c003e29036f62bf3fdce96acd4a271799211a84b34b35dfd290e9bea9c

	For OVA version 1.0.9.5

    **Algorithm** | **Hash value**
    --- | ---
    MD5 | fb11ca234ed1f779a61fbb8439d82969
    SHA1 | 5bee071a6334b6a46226ec417f0d2c494709a42e
    SHA256 | b92ad637e7f522c1d7385b009e7d20904b7b9c28d6f1592e8a14d88fbdd3241c  

	For OVA version 1.0.9.2

    **Algorithm** | **Hash value**
    --- | ---
    MD5 | 7326020e3b83f225b794920b7cb421fc
    SHA1 | a2d8d496fdca4bd36bfa11ddf460602fa90e30be
    SHA256 | f3d9809dd977c689dda1e482324ecd3da0a6a9a74116c1b22710acc19bea7bb2  

## Create the collector VM

Import the downloaded file to the vCenter Server.

1. In the vSphere Client console, click **File** > **Deploy OVF Template**.

    ![Deploy OVF](./media/tutorial-assessment-vmware/vcenter-wizard.png)

2. In the Deploy OVF Template Wizard > **Source**, specify the location of the .ova file.
3. In **Name** and **Location**, specify a friendly name for the collector VM, and the inventory object in which the VM
will be hosted.
5. In **Host/Cluster**, specify the host or cluster on which the collector VM will run.
7. In storage, specify the storage destination for the collector VM.
8. In **Disk Format**, specify the disk type and size.
9. In **Network Mapping**, specify the network to which the collector VM will connect. The network needs internet connectivity, to send metadata to Azure.
10. Review and confirm the settings, then click **Finish**.

## Run the collector to discover VMs

1. In the vSphere Client console, right-click the VM > **Open Console**.
2. Provide the language, time zone, and password preferences for the appliance.
3. On the desktop, click the **Run collector** shortcut.
4. Click **Check for updates** in the top bar of the collector UI and verify that the collector is running on the latest version. If not, you can choose to download the latest upgrade package from the link and update the collector.
5. In the Azure Migrate Collector, open **Set up prerequisites**.
    - Accept the license terms, and read the third-party information.
    - The collector checks that the VM has internet access.
    - If the VM accesses the internet via a proxy, click **Proxy settings**, and specify the proxy address and listening port. Specify credentials if the proxy needs authentication. [Learn more](https://docs.microsoft.com/en-us/azure/migrate/concepts-collector#internet-connectivity) about the internet connectivity requirements and the list of URLs that the collector accesses.

    > [!NOTE]
    > The proxy address needs to be entered in the form http://ProxyIPAddress or http://ProxyFQDN. Only HTTP proxy is supported.

    - The collector checks that the collector service is running. The service is installed by default on the collector VM.
    - Download and install VMware PowerCLI.

6. In **Specify vCenter Server details**, do the following:
    - Specify the name (FQDN) or IP address of the vCenter server.
    - In **User name** and **Password**, specify the read-only account credentials that the collector will use to discover VMs on the vCenter server.
    - In **Collection scope**, select a scope for VM discovery. The collector can only discover VMs within the specified scope. Scope can be set to a specific folder, datacenter, or cluster. It shouldn't contain more than 1500 VMs. [Learn more](how-to-scale-assessment.md) about how you can discover a larger environment.

7. In **Specify migration project**, specify the Azure Migrate project ID and key that you copied from the portal. If didn't copy them, open the Azure portal from the collector VM. In the project **Overview** page, click **Discover Machines**, and copy the values.  
8. In **View collection progress**, monitor discovery, and check that metadata collected from the VMs is in scope. The collector provides an approximate discovery time. [Learn more](https://docs.microsoft.com/en-us/azure/migrate/concepts-collector#what-data-is-collected) about what data is collected by the Azure Migrate collector.

> [!NOTE]
> The collector only supports "English (United States)" as the operating system language and the collector interface language. 
> If you change the settings on a machine you want to assess, trigger discover again before you run the assessment. In the collector, use the **Start collection again** option to do this. After the collection is done, select the **Recalculate** option for the assessment in the portal, to get updated assessment results.



### Verify VMs in the portal

Discovery time depends on how many VMs you are discovering. Typically, for 100 VMs, after the collector finishes running it takes around an hour for discovery to finish.

1. In the Migration Planner project, click **Manage** > **Machines**.
2. Check that the VMs you want to discover appear in the portal.


## Create and view an assessment

After VMs are discovered, you group them and create an assessment.

1. In the project **Overview** page, click **+Create assessment**.
2. Click **View all** to review the assessment properties.
3. Create the group, and specify a group name.
4. Select the machines that you want to add to the group.
5. Click **Create Assessment**, to create the group and the assessment.
6. After the assessment is created, view it in **Overview** > **Dashboard**.
7. Click **Export assessment**, to download it as an Excel file.

### Assessment details

An assessment includes information about whether the on-premises VMs are compatible for Azure, what would be the right VM size for running the VM in Azure and the estimated monthly Azure costs.

![Assessment report](./media/tutorial-assessment-vmware/assessment-report.png)

#### Azure readiness

The Azure readiness view in the assessment shows the readiness status of each VM. Depending on the properties of the VM, each VM can be marked as:
- Ready for Azure
- Conditionally ready for Azure
- Not ready for Azure
- Readiness unknown

For VMs that are ready, Azure Migrate recommends a VM size in Azure. The size recommendation done by Azure Migrate depends on the sizing criterion specified in the assessment properties. If the sizing criterion is performance-based sizing, the size recommendation is done by considering the performance history of the VMs (CPU and memory) and disks (IOPS and throughput). If the sizing criterion is 'as on-premises', Azure Migrate does not consider the performance data for the VM and disks. The recommendation for the VM size in Azure is done by looking at the size of the VM on-premises and the disk sizing is done based on the Storage type specified in the assessment properties (default is premium disks). [Learn more](concepts-assessment-calculation.md) about how sizing is done in Azure Migrate.

For VMs that aren't ready or conditionally ready for Azure, Azure Migrate explains the readiness issues, and provides remediation steps.

The VMs for which Azure Migrate cannot identify Azure readiness (due to data unavailability) are marked as readiness unknown.

In addition to Azure readiness and sizing, Azure Migrate also suggests tools that you can use for the migrating the VM. This requires a deeper discovery of on the on-premises environment. [Learn more](how-to-get-migration-tool.md) about how you can do a deeper discovery by installing agents on the on-premises machines. If the agents are not installed on the on-premises machines, lift and shift migration is suggested using [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview). If the agents are installed on the on-premises machine, Azure Migrate looks at the processes running inside the machine and identifies whether the machine is a database machine or not. If the machine is a database machine, [Azure Database Migration Service](https://docs.microsoft.com/azure/dms/dms-overview) is suggested, else Azure Site Recovery is suggested as the migration tool.

  ![Assessment readiness](./media/tutorial-assessment-vmware/assessment-suitability.png)  

#### Monthly cost estimate

This view shows the total compute and storage cost of running the VMs in Azure along with the details for each machine. Cost estimates are calculated considering the size recommendations done by Azure Migrate for a machine, its disks, and the assessment properties.

> [!NOTE]
> The cost estimation provided by Azure Migrate is for running the on-premises VMs as Azure Infrastructure as a service (IaaS) VMs. Azure Migrate does not consider any Platform as a service (PaaS) or Software as a service (SaaS) costs.

Estimated monthly costs for compute and storage are aggregated for all VMs in the group.

![Assessment VM cost](./media/tutorial-assessment-vmware/assessment-vm-cost.png)

#### Confidence rating

Each assessment in Azure Migrate is associated with a confidence rating that ranges from 1 star to 5 star (1 star being the lowest and 5 star being the highest). The confidence rating is assigned to an assessment based on the availability of data points needed to compute the assessment. The confidence rating of an assessment helps you estimate the reliability of the size recommendations provided by Azure Migrate.

The confidence-rating of an assessment is more useful for assessments with sizing criterion as 'performance-based sizing. For performance-based sizing, Azure Migrate needs the utilization data for CPU, memory of the VM. Additionally, for every disk attached to the VM, it needs the disk IOPS and throughput data. Similarly for each network adapter attached to a VM, Azure Migrate needs the network in/out to do performance-based sizing. If any of the above utilization numbers are not available in vCenter Server, the size recommendation done by Azure Migrate may not be reliable. Depending on the percentage of data points available, the confidence rating for the assessment is provided as below:

   **Availability of data points** | **Confidence rating**
   --- | ---
   0%-20% | 1 Star
   21%-40% | 2 Star
   41%-60% | 3 Star
   61%-80% | 4 Star
   81%-100% | 5 Star

An assessment may not have all the data points available due to one of the following reasons:
- The statistics setting in vCenter Server is not set to level 3. If the statistics setting in vCenter Server is lower than level 3, performance data for disk and network is not collected from vCenter Server. In this case, the recommendation provided by Azure Migrate for disk and network is not utilization-based. Without considering the IOPS/throughput of the disk, Azure Migrate cannot identify if the disk will need a premium disk in Azure, hence, in this case, Azure Migrate recommends Standard disks for all disks.
- The statistics setting in vCenter Server was set to level 3 for a shorter duration, before kicking off the discovery. For example, let's consider the scenario where you change the statistics setting level to 3 today and kick off the discovery using the collector appliance tomorrow (after 24 hours). If you are creating an assessment for one day, you have all the data points and the confidence rating of the assessment would be 5 star. But if you are changing the performance duration in the assessment properties to one month, the confidence rating goes down as the disk and network performance data for the last one month would not be available. If you would like to consider the performance data of last one month, it is recommended that you keep the vCenter Server statistics setting to level 3 for one month before you kick off the discovery.
- Few VMs were shut down during the period for which the assessment is calculated. If any VMs were powered off for some duration, vCenter Server will not have the performance data for that period.
- Few VMs were created in between the period for which the assessment is calculated. For example, if you are creating an assessment for the performance history of last one month, but few VMs were created in the environment only a week ago. In such cases, the performance history of the new VMs will not be there for the entire duration.

> [!NOTE]
> If the confidence rating of any assessment is below 4 Stars, we recommend you to change the vCenter Server statistics settings level to 3, wait for the duration that you want to consider for assessment (1 day/1 week/1 month) and then do discovery and assessment. If the preceding cannot be done, performance-based sizing may not be reliable and it is recommended to switch to *as on-premises sizing* by changing the assessment properties.

## Next steps

- [Learn](how-to-modify-assessment.md) how to customize an assessment based on your requirements.
- Learn how to create high-confidence assessment groups using [machine dependency mapping](how-to-create-group-machine-dependencies.md)
- [Learn more](concepts-assessment-calculation.md) about how assessments are calculated.
- [Learn](how-to-scale-assessment.md) how to discover and assess a large VMware environment.
- [Learn more](resources-faq.md) about the FAQs on Azure Migrate
