---
title: What is role-based access control (RBAC) in Azure? | Microsoft Docs
description: Get an overview of role-based access control (RBAC) in Azure. Use role assignments to control access to resources in Azure.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman

ms.assetid: 8f8aadeb-45c9-4d0e-af87-f1f79373e039
ms.service: role-based-access-control
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/02/2018
ms.author: rolyon
ms.reviewer: bagovind

#Customer intent: As a dev, devops, or it admin, I want to learn how permissions and roles work in Azure, so that I can better understand how to grant access to resources.
---

# What is role-based access control (RBAC)?

Access management for cloud resources is a critical function for any organization that is using the cloud. Role-based access control (RBAC) helps you manage who has access to Azure resources, what they can do with those resources, and what areas they have access to.

RBAC is an authorization system built on [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) that provides fine-grained access management of resources in Azure. Using RBAC, you can segregate duties within your team and grant only the amount of access to users that they need to perform their jobs. Instead of giving everybody unrestricted permissions in your Azure subscription or resources, you can allow only certain actions at a particular scope.

## What can I do with RBAC?

Here are some examples of what you can do with RBAC:

- Allow one user to manage virtual machines in a subscription and another user to manage virtual networks
- Allow a DBA group to manage SQL databases in a subscription
- Allow a user to manage all resources in a resource group, such as virtual machines, websites, and subnets
- Allow an application to access all resources in a resource group

## How RBAC works

The way you control access to resources using RBAC is to create role assignments. This is a key concept to understand – it’s how permissions are enforced. A role assignment consists of three elements: security principal, role definition, and scope.

### Security principal

A *security principal* is an object that represents a user, group, or service principal that is requesting access to Azure resources.

![Security principal for a role assignment](./media/overview/rbac-security-principal.png)

- User - An individual who has a profile in Azure Active Directory. You can also assign roles to users in other tenants. For information about users in other organizations, see [Azure Active Directory B2B](/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b).
- Group - A set of users created in Azure Active Directory. When you assign a role to a group, all users within that group have that role. 
- Service principal - A security identity used by applications or services to access specific Azure resources. You can think of it as a *user identity* (username and password or certificate) for an application.

### Role definition

A *role definition* is a collection of permissions. It's sometimes just called a *role*. A role definition lists the operations that can be performed, such as read, write, and delete. Roles can be high-level, like owner, or specific, like virtual machine reader.

![Role definition for a role assignment](./media/overview/rbac-role-definition.png)

Azure includes several [built-in roles](built-in-roles.md) that you can use. The following lists four fundamental built-in roles. The first three apply to all resource types.

- [Owner](built-in-roles.md#owner) - Has full access to all resources including the right to delegate access to others.
- [Contributor](built-in-roles.md#contributor) - Can create and manage all types of Azure resources but can’t grant access to others.
- [Reader](built-in-roles.md#reader) - Can view existing Azure resources.
- [User Access Administrator](built-in-roles.md#user-access-administrator) - Lets you manage user access to Azure resources.

The rest of the built-in roles allow management of specific Azure resources. For example, the [Virtual Machine Contributor](built-in-roles.md#virtual-machine-contributor) role allows a user to create and manage virtual machines. If the built-in roles don't meet the specific needs of your organization, you can create your own [custom roles](custom-roles.md).

Azure has introduced data operations (currently in preview) that enable you to grant access to data within an object. For example, if a user has read data access to a storage account, then they can read the blobs or messages within that storage account. For more information, see [Understand role definitions](role-definitions.md).

### Scope

*Scope* is the boundary that the access applies to. When you assign a role, you can further limit the actions allowed by defining a scope. This is helpful if you want to make someone a [Website Contributor](built-in-roles.md#website-contributor), but only for one resource group.

In Azure, you can specify a scope at multiple levels: subscription, resource group, or resource. Scopes are structured in a parent-child relationship where every child will have only one parent.

![Scope for a role assignment](./media/overview/rbac-scope.png)

Access that you assign at a parent scope is inherited at the child scope. For example:

- If you assign the [Reader](built-in-roles.md#reader) role to a group at the subscription scope, the members of that group can view every resource group and resource in the subscription.
- If you assign the [Contributor](built-in-roles.md#contributor) role to an application at the resource group scope, it can manage resources of all types in that resource group, but not other resource groups in the subscription.

Azure also includes a scope above subscriptions called [management groups](../azure-resource-manager/management-groups-overview.md), which is in preview. Management groups are a way to manage multiple subscriptions. When you specify scope for RBAC, you can either specify a management group or specify a subscription, resource group, or resource hierarchy.

### Role assignment

A *role assignment* is the process of binding a role definition to a user, group, or service principal at a particular scope for the purpose of granting access. Access is granted by creating a role assignment, and access is revoked by removing a role assignment.

The following diagram shows an example of a role assignment. In this example, the Marketing group has been assigned the [Contributor](built-in-roles.md#contributor) role for the pharma-sales resource group. This means that users in the Marketing group can create or manage any Azure resource in the pharma-sales resource group. Marketing users do not have access to resources outside the pharma-sales resource group, unless they are part of another role assignment.

![Role assignment to control access](./media/overview/rbac-overview.png)

You can create role assignments using the Azure portal, Azure CLI, Azure PowerShell, Azure SDKs, or REST APIs. You can have up to 2000 role assignments in each subscription. To create and remove role assignments, you must have `Microsoft.Authorization/roleAssignments/*` permission. This permission is granted through the [Owner](built-in-roles.md#owner) or [User Access Administrator](built-in-roles.md#user-access-administrator) roles.

## Next steps

- [Quickstart: Grant access for a user using RBAC and the Azure portal](quickstart-assign-role-user-portal.md)
- [Manage access using RBAC and the Azure portal](role-assignments-portal.md)
- [Understand the different roles in Azure](rbac-and-directory-admin-roles.md)
