---
title: Cortana Intelligence solution evaluation tool | Microsoft Docs
description: As a Microsoft Partner, here are all the steps you need to follow to publish your Cortana Intelligence solution to AppSource.
services: machine-learning
documentationcenter: ''
author: AnupamMicrosoft
manager: jhubbard
editor: cgronlun

ms.service: machine-learning
ms.component: team-data-science-process
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/07/2017
ms.author: anupams
--- 
# Cortana Intelligence solution evaluation tool
## Overview
You can use the Cortana Intelligence solution evaluation tool to assess your advanced analytics solutions for compliance with Microsoft-recommended best practices. Microsoft is excited to work with our partners (ISVs / SIs) to provide high-quality solutions for customers, resellers and implementation. This guide will walk through the process of using the Solution evaluation tool with your solution and describe the specific best practices in checks for.

## Getting started
Please [download](https://aka.ms/aa-evaluation-tool-download) and install the Cortana Intelligence solution evaluation tool.

Prerequisites:
- Windows 10: [Official site for Windows 10](https://www.microsoft.com/en-us/windows)
- Azure Powershell:
[Install and configure Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-4.0.0).

## Identifying your app
After installation completes, open the tool and begin your first evaluation.

![Open evaluation tool](./media/cortana-intelligence-appsource-evaluation-tool/1-open-evaluation-tool.png)

Provide identifying information about your solution.

![Connect Azure subscription](./media/cortana-intelligence-appsource-evaluation-tool/2-connect-azure-subscription.png)

Connect to your Azure subscription and provide the Resource Group
containing your app.

![Select resources](./media/cortana-intelligence-appsource-evaluation-tool/3-select-resources.png)

Once the resource group has been loaded, please select the resources that are included in your solution and identify the accessibility of any data resources as either:
- Ingestion
- Consumption
- Internal

We use this information to better understand how your solution is utilizing various components and to ensure user-facing components are consistent with best practices.

### Ingestion
Ingestion in this case means any data sources that are used to pull in data from outside the solution or that any services outside the solution use to push data into it.

### Consumption
Consumption in this case means any datasets that are used to push data to end users, either directly or indirectly. For example:
- Datasets used in direct query from PowerBI.
- Datasets queried in a WebApp.

>[!NOTE]
If a specific resource is used for both ingestion and consumption, please choose **Consumption**.

### Internal
Use internal for any data resources that are used only in internal application processing.

Next, you will be prompted to provide valid credentials for any databases specified in the prior step:

![Set test prerequisites](./media/cortana-intelligence-appsource-evaluation-tool/4-set-test-prerequisites.png)

## Solution test cases
The solution tool will perform a collection of automated tests on your solution.

![Set test execution](./media/cortana-intelligence-appsource-evaluation-tool/5-set-test-execution.png)

After the tests complete, you will be asked to provide an explanation or justification for why your solution does not comply with the requirement.

![Provide business justification](./media/cortana-intelligence-appsource-evaluation-tool/6-provide-business-justification.png)

For example, if your solution publishes to Azure SQL DW, the evaluation tests require you to also publish to Azure Analysis Services. 

Your solution may use IaaS virtual machines running Sql Server Analysis Services instead of Azure Analysis Services. This would be an acceptable reason for failure of the test.
## Packaging your evaluation results
After completing the test cases, your evaluation package will be exported to a zip file and you will be asked to provide feedback on the evaluation tool. 

You need to share this test results zip file with Microsoft for your solution to be evaluated before getting approval to be added to AppSource

![Grade evaluation tool](./media/cortana-intelligence-appsource-evaluation-tool/7-grade-evaluation-tool.png)

Above section of this article covers various features of the tool, now let us review types of best practices that this tool evaluates.

## Security evaluation considerations
### Databases should use Azure Active Directory authentication
Any Azure SQL or Azure SQL DW resources in the sloution should be enabled with Azure Active Directory (AAD) authentication. AAD provides a single place to manage all of your identities and roles.

| For more information about | See this article |
| --- | --- |
| AAD with SQL Database and SQL Data Warehouse | [Use Azure Active Directory Authentication for authentication with SQL Database or SQL Data Warehouse](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication) |
| Configure and manage AAD | [Configure and manage Azure Active Directory authentication with SQL Database or SQL Data Warehouse](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication-configure) |
| Azure WebApps authentication | [Authentication and authorization in Azure App Service](https://docs.microsoft.com/azure/app-service/app-service-authentication-overview) |
| Configure WebApps with AAD | [How to configure your App Service application to use Azure Active Directory login](https://docs.microsoft.com/azure/app-service/app-service-mobile-how-to-configure-active-directory-authentication)|

### Datasets accessible to end-users should support role-based access control
While executing the evaluation tool, you will be asked to specify any reporting or publishing resources. It is assumed that these resources are intended for access by end-users, not developers. These resources should be provide role-based access control (RBAC) in order to ensure that end users are only able to access authorized data.

Specifically, any of the following Azure resources can be configured with RBAC and are considered acceptable:
- Secure HDInsight, see [An introduction to Hadoop security with domain-joined HDInsight clusters](https://docs.microsoft.com/azure/hdinsight/hdinsight-domain-joined-introduction)
- Azure SQL, see [AAD authentication with Azure SQL]( https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication)
- Azure Analysis Services, see [Manage database roles and users for Azure Analysis Services](https://docs.microsoft.com/azure/analysis-services/analysis-services-database-users)
- Azure SQL Data Warehouse (Be aware that because SQL DW does support RBAC, it is not recommended for direct end-user access.)

If you are using a different resource type that supports RBAC, please
specify that in the test case justification.

### Azure Data Lake Store should use at-rest encryption
Azure Data Lake Store (ADLS) supports at-rest encryption by default using ADLS-managed encryption keys. You may also configure encryption using Azure Key Vault.

For information about specifying ADLS encryption settings, [Create an Azure Data Lake Store account](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-get-started-portal#create-an-azure-data-lake-store-account).

### Azure SQL and Azure SQL Data Warehouse should use encryption
Azure SQL and Azure SQL DW both support Transparent Data Encryption (TDE), which provides real-time encryption and decryption of both data and log files.

| For more information about | See this article |
| --- | --- |
| Transparent Data Encryption (TDE) | [Transparent Data Encryption](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-tde) |
| Azure SQL Data Warehouse with TDE | [SQL Data Warehouse Encrption TDE TSQL](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-encryption-tde-tsql) |
| Configure Azure SQL with TDE | [Transparent Data Encryption with Azure SQL Database](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-with-azure-sql-database) |
| Configure Azure SQL with Always Encrypted | [SQL Database Always Encrypted Azure Key Vault](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault)|

In addition to TDE, Azure SQL also supports Always Encrypted, a new data encryption technology that ensures data is encrypted not only at-rest and during movement between client and server, but also while data is in use while executing commands on the server.

### Any Virtual Machines must be deployed from the Azure Marketplace
In order to provide a consistent level of security across AppSource, we require that any virtual machines deployed as part of a Cortana Intelligence solution be certified and published on the Azure Marketplace.

To search the current list of Azure Marketplace images, see [Microsoft Azure Marketplace](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/category/compute).

For information on how to publish a virtual machine image for Azure Marketplace, see [Guide to create a virtual machine image for the Azure Marketplace](https://docs.microsoft.com/azure/marketplace-publishing/marketplace-publishing-vm-image-creation).

## Scalability evaluation considerations
### Cortana Intelligence solutions should include a scalable big data platform
Cortana Intelligence solutions should scale to very large data sizes. In Azure, this means they should include one of the two Petabyte-scale data platforms:
- Azure Data Lake Store
- Azure SQL Data Warehouse

If your solution does not require support for these data sizes or if you are using an alternative data platform, please explain this in the test case justification.
### Cortana Intelligence solutions should include dedicated ingestion data environments
Cortana Intelligence solutions should generally avoid directly inserting data into relational data sources. Instead, raw data should be stored in an unstructured environment, with idempotent inserts/updates into any relational stores using Azure Data Factory.

For more information on copying data with Azure Data Factory, [Tutorial: Create a pipeline with Copy Activity using Visual Studio](https://docs.microsoft.com/azure/data-factory/quickstart-create-data-factory-dot-net).

### Azure SQL Data Warehouse should use PolyBase for data ingestion
Azure SQL DW supports PolyBase for highly scalable, parallel data ingestion. PolyBase allows you to use Azure SQL DW to issue queries against external datasets stored in either Azure Blob Storage or Azure Data Lake Store. This provides superior performance to alternative methods of bulk updates.

For instructions on getting started with PolyBase and Azure SQL DW, see [Load data with PolyBase in SQL Data Warehouse](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-get-started-load-with-polybase).

For more information about best practices with PolyBase and Azure SQL DW, see [Guide for using PolyBase in SQL Data Warehouse](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-load-polybase-guide).

## Availability evaluation considerations

### Datasets accessible to end users should support a large volume of concurrent users
While executing the evaluation tool, you will be asked to specify any reporting or publishing resources. It is assumed that these resources are intended for access by end-users, not developers. These resources should support medium-large numbers of concurrent users.

Specifically, Azure SQL Data Warehouse should NOT be the sole data source available to end users. If Azure SQL DW is provided as a resource for Power Users, Azure Analysis Services should be made available to typical users.

For more information about Azure SQL DW concurrency limits, see [Concurrency and workload management in SQL Data Warehouse](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-develop-concurrency).

For more information about Azure Analysis Services, see [Analysis Services Overview](https://docs.microsoft.com/azure/analysis-services/analysis-services-overview).

### Azure SQL resources should have a read-only replica for failover
Azure SQL databases support geo-replication to a secondary instance. This instance can then be used as a failover instance to provide high availability applications.

For more information about geo-replication for Azure SQL databases, see [SQL Database GEO Replication overview](https://docs.microsoft.com/azure/sql-database/sql-database-geo-replication-overview).

For instructions on how to configure geo-replication for Azure SQL, see [Configure active geo-replication for Azure SQL Database with Transact-SQL](https://docs.microsoft.com/azure/sql-database/sql-database-geo-replication-transact-sql).

### Azure SQL Data Warehouse should have geo-redundant backups enabled
Azure SQL DW supports daily backups to geo-redundant storage. This geo-replication ensures you can restore the data warehouse even in situations where you cannot access snapshots stored in your primary region. This feature is on by default and should not be disable for Cortana Intelligence solutions.

For more information about Azure SQL DW backups and restoration, see here [SQL Data Warehouse Backups](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-backups).

### Virtual machines should be configured with availability sets
Azure virtual machines should be configured in availability sets in order to minimize the impact of planned and unplanned maintenance events.

For more information about Azure virtual machine availability, see [Manage the availability of Windows virtual machines in Azure](https://docs.microsoft.com/azure/virtual-machines/windows/manage-availability).

## Other evaluation considerations
### Cortana Intelligence apps should use a centralized tool for data orchestration
Using a single tool for managing and scheduling data movement and transformation ensures consistency around mission-critical data. It provides a clear logic around retry logic, dependency management, alert/logging, etc. We recommend the use of [Azure Data Factory](https://docs.microsoft.com/azure/data-factory/introduction) for data orchestration in Azure.

If you are using a tool other than Azure Data Factory for data orchestration, please describe which tool or tools you are using.
### Azure Machine Learning models should be retrained using Azure Data Factory
Azure Machine Learning (AzureML) provides easy to use tools for the creation and deployment of predictive modeling and machine learning pipelines. However, it is important that production deployments of these AzureML models is not based on a single fixed dataset, but instead adapts to the shifting dynamics of real-world phenomena.

For more information on creating retraining web services in AzureML, see [Retrain Machine Learning models programmatically](https://docs.microsoft.com/azure/machine-learning/machine-learning-retrain-models-programmatically).

For more information about automating the model training process using Azure Data Factory, see [Updating Azure Machine Learning models using Update Resource Activity](https://docs.microsoft.com/azure//data-factory/update-machine-learning-models).

## Existing documentation
[Microsoft Azure Certified to grow your cloud business](https://azure.microsoft.com/marketplace/programs/certified/)

[Microsoft Azure Certified for Cortana Intellignece](https://azure.microsoft.com/marketplace/programs/certified/cortana/)

