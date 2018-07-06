---
title: 'Install Azure AD Connect using SQL delegated administrator permissions | Microsoft Docs'
description: This topic describes an update to Azure AD Connect that allows for installation using an account that only has SQL dbo permissions.
documentationcenter: ''
author: billmath
manager: mtillman
editor: ''
ms.reviewer: jparsons
ms.assetid:
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/07/2018
ms.component: hybrid
ms.author: billmath
---

# Install Azure AD Connect using SQL delegated administrator permissions
Prior to the latest Azure AD Connect build, administrative delegation, when deploying configurations that required SQL, was not supported.  Users who wanted to install Azure AD Connect needed to have server administrator (SA) permissions on the SQL server.

With the latest release of Azure AD Connect, provisioning the database can now be performed out of band by the SQL administrator and then installed by the Azure AD Connect administrator with database owner rights.

## Before you begin
To use this feature, you need to realize that there are several moving parts and each one may involve a different administrator in your organization.  The following table summarizes the individual roles and their respective duties in deploying Azure AD Connect with this feature.

|Role|Description|
|-----|-----|
|Domain or Forest AD administrator|Creates the domain level service account that is used by Azure AD Connect to run the sync service.  For more information on service accounts, see [Accounts and permissions](active-directory-aadconnect-accounts-permissions.md).
|SQL administrator|Creates the ADSync database and grants login + dbo access to the Azure AD Connect administrator and the service account created by the domain/forest admin.|
Azure AD Connect administrator|Installs Azure AD Connect and specifies the service account during custom installation.

## Steps for installing Azure AD Connect using SQL delegated permissions
To provision the database out of band and install Azure AD Connect with database owner permissions, use the following steps.

>[!NOTE]
>Although it is not required, it is **highly recommended** that the Latin1_General_CI_AS collation is selected when creating the database.


1.	Have the SQL Administrator create the ADSync database with a case insensitive collation sequence **(Latin1_General_CI_AS)**.  The database must be named **ADSync**.  The recovery model, compatibility level, and containment type are updated to the correct values when Azure AD Connect is installed.  However the collation sequence must be set correctly by the SQL administrator otherwise Azure AD Connect will block the installation.  To recover the SA must delete and recreate the database.</br>
![Collation](media/active-directory-aadconnect-sql-delegation/sql4.png)
2.	Grant the Azure AD Connect administrator and the domain service account the following permissions:
    - SQL Login 
    - **database owner(dbo)** rights.  </br>
![Permissions](media/active-directory-aadconnect-sql-delegation/sql3a.png)
3.	Send an email to the Azure AD Connect administrator indicating the SQL server and instance name that should be used when installing Azure AD Connect.

## Additional information
Once the database is provisioned, the Azure AD Connect administrator can install and configure on-premises synchronization at their convenience.  

The **/UseExistingDatabase** flag is required when using a pre-created database.  It is not only used in recovery situations.

In addition to supporting new installations of Azure AD Connect, this feature also enables delegation for any scenario related to the **/UseExistingDatabase** flag.  For more information on installing Azure AD Connect with an existing database, see [Install Azure AD Connect using an existing ADSync database](active-directory-aadconnect-existing-database.md)


## Next steps
- [Getting started with Azure AD Connect using express settings](active-directory-aadconnect-get-started-express.md)
- [Custom installation of Azure AD Connect](active-directory-aadconnect-get-started-custom.md)
- [Install Azure AD Connect using an existing ADSync database](active-directory-aadconnect-existing-database.md)  
