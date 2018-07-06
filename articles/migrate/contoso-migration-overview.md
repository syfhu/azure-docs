---
title: Overview of Contoso migration to Azure | Microsoft Docs
description: Provides an overview of the migration strategy and scenarios used by Contoso to migrate their on-premises datacenter to Azure.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 06/19/2018
ms.author: raynew

---
# Contoso migration: Overview


This is the first article in a series that demonstrates how the fictitious organization Contoso are migrating their on-premises infrastructure to the [Microsoft Azure](https://azure.microsoft.com/overview/what-is-azure/) cloud. 

The article series provides example deployments that help you to understand and try out different options for migration to Azure, and for working in a hybrid environment. Articles show how the Contoso company completes its migration mission, but pointers for general reading and specific instructions are provided throughout.

## Introduction

Azure provides access to a comprehensive set of cloud services. As developers and IT professionals, you can use these services to build, deploy, and manage applications on a range of tools and frameworks, through a global network of datacenters. As your business faces challenges associated with the digital shift, the Azure cloud helps you to figure out how to optimize resources and operations, engage with your customers and employees, and transform your products.

However, Azure recognizes that even with all the advantages that the cloud provides in terms of speed and flexibility, minimized costs, performance, and reliability, many organizations are going to need to run on-premises datacenters for some time to come. In response to cloud adoption barriers, Azure provides a hybrid cloud strategy that builds bridges between your on-premises datacenters, and the Azure public cloud. For example, using Azure cloud resources like backup to protect on-premises resources, or using Azure analytics to gain insights into on-premises workloads. 

As part of the hybrid cloud strategy, Azure provides growing solutions for migrating on-premises apps and workloads to the cloud. With simple steps, you can comprehensively assess your on-premises resources to figure out how they'll run in the Azure cloud. Then, with a deep assessment in hand, you can confidently migrate resources to Azure. When resources are up and running in Azure, you can optimize them to retain and improve access, flexibility, security, and reliability.

## Migration strategies

Strategies for migration to the cloud fall into four broad categories: rehost, refactor, rearchitect, or rebuild. The strategy you adopt depends upon your business drivers, and migration goals. You might adopt multiple strategies. For example, you could choose to rehost (lift-and-shift) simple apps, or apps that aren't critical to your business, but rearchitect those that are more complex and business-critical. Let's look at the strategies.


**Strategy** | **Definition** | **When to use** 
--- | --- | --- 
**Rehost** | Often referred to as a "lift-and-shift" migration. This option doesn't require code changes, and let's you migrate your existing apps to Azure quickly. Each app is migrated as is, to reap the benefits of the cloud, without the risk and cost associated with code changes. | When you need to move apps quickly to the cloud.<br/><br/> When you want to move an app without modifying it.<br/><br/> When your apps are architected so that they can leverage [Azure IaaS](https://azure.microsoft.com/overview/what-is-iaas/) scalability after migration.<br/><br/> When apps are important to your business, but you don't need immediate changes to app capabilities.
**Refactor** | Often referred to as “repackaging,” refactoring requires minimal changes to apps, so that they can connect to [Azure PaaS](https://azure.microsoft.com/overview/what-is-paas/), and use cloud offerings.<br/><br/> For example, you could migrate existing apps to Azure App Service or Azure Kubernetes Service (AKS).<br/><br/> Or, you could refactor relational and non-relational databases into options such as Azure SQL Database Managed Instance, Azure Database for MySQL, Azure Database for PostgreSQL, and Azure Cosmos DB. | If your app can easily be repackaged to work in Azure.<br/><br/> If you want to apply innovative DevOps practices provided by Azure, or you're thinking about DevOps using a container strategy for workloads.<br/><br/> For refactoring, you need to think about the portability of your existing code base, and available development skills.
**Rearchitect** | Rearchitecting for migration focuses on modifying and extending app functionality and code base to optimize the app architecture for cloud scalability.<br/><br/> For example, you could break down a monolithic application into a group of microservices that work together and scale easily.<br/><br/> Or, you could rearchitect relational and non-relational databases to fully managed DBaaS solutions, such as Azure SQL Database Managed Instance, Azure Database for MySQL, Azure Database for PostgreSQL, and Azure Cosmos DB. | When your apps need major revisions to incorporate new capabilities, or to work effectively on a cloud platform.<br/><br/> When you want to use existing application investments, meet scalability requirements, apply innovative Azure DevOps practices, and minimize use of virtual machines.
**Rebuild** | Rebuild takes things a step further by rebuilding an app from scratch using Azure cloud technologies.<br/><br/> For example, you could build green field apps with cloud-native technologies like serverless, Azure AI, Azure SQL Database Managed Instance, and Azure Cosmos DB. | When you want rapid development, and existing apps have limited functionality and lifespan.<br/><br/> When you're ready to expedite business innovation (including DevOps practices provided by Azure), build new applications using cloud-native technologies, and take advantage of advancements in AI, blockchain, and IoT.

## Migration articles

The articles that make up the Contoso migration series are summarized in the table below.  

- The scenarios are of increasing complexity, and we're adding to them over time.
- Each migration scenario is driven by slightly different business goals that determine the migration strategy.
- For each deployment scenario, we provide information about business drivers and goals, a proposed architecture, steps to perform the migration, and recommendation for cleanup and next steps after migration is complete.

**Article** | **Details** | **Status**
--- | --- | ---
Article 1: Overview (this article) | Provides an overview of Contoso's migration strategy, the article series, and the sample apps used. | Available
[Article 2: Deploy an Azure infrastructure](contoso-migration-overview.md) | Describes how Contoso prepares its on-premises and Azure infrastructure for migration. The same infrastructure is used for all Contoso migration scenarios. | Available
[Article 3: Assess on-premises resources](contoso-migration-assessment.md) | Shows how Contoso runs an assessment of their on-premises two-tier SmartHotel app running on VMware. They assess app VMs with the [Azure Migrate](migrate-overview.md) service, and the app SQL Server database with the [Azure Database Migration Assistant](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017). | Available
[Article 4: Rehost to Azure VMs and a SQL Managed Instance](contoso-migration-rehost-vm-sql-managed-instance.md) | Demonstrates how Contoso migrates the SmartHotel app to Azure. They migrate the app web VM using [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview), and the app database to a SQL Managed Instance using the [Azure Database Migration](https://docs.microsoft.com/azure/dms/dms-overview) service. | Available
[Article 5: Rehost to Azure VMs](contoso-migration-rehost-vm.md) | Shows how Contoso migrate their SmartHotel app VMs using the Site Recovery service.
[Article 6: Rehost to Azure VMs and SQL Server Availability Groups](contoso-migration-rehost-vm-sql-ag.md) | Shows how Contoso migrates the SmartHotel app. They use Site Recovery to migrate the app VM, and the Database Migration service to migrate the app database to a SQL Server AlwaysOn Availability Group. | Available
[Article 7: Rehost a Linux app to Azure VMs](contoso-migration-rehost-linux-vm.md) | Demonstrates how Contoso migrates the app VMs to Azure using Site Recovery. | Planned
[Article 8: Rehost a Linux app to Azure VMs and Azure MySQL Server](contoso-migration-rehost-linux-vm-mysql.md) | Demonstrates how Contoso migrates the app VM using Site Recovery, and uses MySQL Workbench to migrate to an Azure MySQL Server instance. | Planned



### Demo apps

The articles use two demo apps - SmartHotel, and osTicket.

- **SmartHotel360**: This app was developed by Microsoft as a test app that you can use when working with Azure. It's provided as open source and you can download it from [GitHub](https://github.com/Microsoft/SmartHotel360). It's an ASP.NET app connected to a SQL Server database. Currently the app is on two VMware VMs running Windows Server 2008 R2, and SQL Server 2008 R2. The app VMs are hosted on-premises and managed by vCenter Server.
- **osTicket**: An open-source service desk ticketing app that runs on Linux. You can download it from [GitHub](https://github.com/osTicket/osTicket). Current the app is on two VMware VMs running Ubuntu 16.04LTS, using Apache 2, PHP 7.0, and MySQL 5.7
    

## Next steps

[Learn how](contoso-migration-infrastructure.md) Contoso sets up their on-premises and Azure infrastructure as they prepare for migration.



