---
title: Understand role definitions in Azure RBAC | Microsoft Docs
description: Learn about role definitions in role-based access control (RBAC) for fine-grained access management of resources in Azure.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman

ms.assetid: 
ms.service: role-based-access-control
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/18/2018
ms.author: rolyon
ms.reviewer: bagovind
ms.custom: 
---
# Understand role definitions

If you are trying to understand how a role works or if you are creating your own [custom role](custom-roles.md), it's helpful to understand how roles are defined. This article describes the details of role definitions and provides some examples.

## Role definition structure

A *role definition* is a collection of permissions. It's sometimes just called a *role*. A role definition lists the operations that can be performed, such as read, write, and delete. It can also list the operations that can't be performed or operations related to underlying data. A role definition has the following structure:

```
assignableScopes []
description
id
name
permissions []
  actions []
  dataActions []
  notActions []
  notDataActions []
roleName
roleType
type
```

Operations are specified with strings that have the following format:

- `{Company}.{ProviderName}/{resourceType}/{action}`

The `{action}` portion of an operation string specifies the type of operations you can perform on a resource type. For example, you will see the following substrings in `{action}`:

| Action substring    | Description         |
| ------------------- | ------------------- |
| `*` | The wildcard character grants access to all operations that match the string. |
| `read` | Enables read operations (GET). |
| `write` | Enables write operations (PUT, POST, and PATCH). |
| `delete` | Enables delete operations (DELETE). |

Here's the [Contributor](built-in-roles.md#contributor) role definition in JSON format. The wildcard (`*`) operation under `actions` indicates that the principal assigned to this role can perform all actions, or in other words, it can manage everything. This includes actions defined in the future, as Azure adds new resource types. The operations under `notActions` are subtracted from `actions`. In the case of the [Contributor](built-in-roles.md#contributor) role, `notActions` removes this role's ability to manage access to resources and also assign access to resources.

```json
[
  {
    "additionalProperties": {},
    "assignableScopes": [
      "/"
    ],
    "description": "Lets you manage everything except access to resources.",
    "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/b24988ac-6180-42a0-ab88-20f7382dd24c",
    "name": "b24988ac-6180-42a0-ab88-20f7382dd24c",
    "permissions": [
      {
        "actions": [
          "*"
        ],
        "additionalProperties": {},
        "dataActions": [],
        "notActions": [
          "Microsoft.Authorization/*/Delete",
          "Microsoft.Authorization/*/Write",
          "Microsoft.Authorization/elevateAccess/Action"
        ],
        "notDataActions": []
      }
    ],
    "roleName": "Contributor",
    "roleType": "BuiltInRole",
    "type": "Microsoft.Authorization/roleDefinitions"
  }
]
```

## Management and data operations (Preview)

Role-based access control for management operations is specified in the `actions` and `notActions` properties of a role definition. Here are some examples of management operations in Azure:

- Manage access to a storage account
- Create, update, or delete a blob container
- Delete a resource group and all of its resources

Management access is not inherited to your data. This separation prevents roles with wildcards (`*`) from having unrestricted access to your data. For example, if a user has a [Reader](built-in-roles.md#reader) role on a subscription, then they can view the storage account, but by default they can't view the underlying data.

Previously, role-based access control was not used for data operations. Authorization for data operations varied across resource providers. The same role-based access control authorization model used for management operations has been extended to data operations (currently in preview).

To support data operations, new data properties have been added to the role definition structure. Data operations are specified in the `dataActions` and `notDataActions` properties. By adding these data properties, the separation between management and data is maintained. This prevents current role assignments with wildcards (`*`) from suddenly having accessing to data. Here are some data operations that can be specified in `dataActions` and `notDataActions`:

- Read a list of blobs in a container
- Write a storage blob in a container
- Delete a message in a queue

Here's the [Storage Blob Data Reader (Preview)](built-in-roles.md#storage-blob-data-reader-preview) role definition, which includes operations in both the `actions` and `dataActions` properties. This role allows you to read the blob container and also the underlying blob data.

```json
[
  {
    "additionalProperties": {},
    "assignableScopes": [
      "/"
    ],
    "description": "Allows for read access to Azure Storage blob containers and data.",
    "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/2a2b9908-6ea1-4ae2-8e65-a410df84e7d1",
    "name": "2a2b9908-6ea1-4ae2-8e65-a410df84e7d1",
    "permissions": [
      {
        "actions": [
          "Microsoft.Storage/storageAccounts/blobServices/containers/read"
        ],
        "additionalProperties": {},
        "dataActions": [
          "Microsoft.Storage/storageAccounts/blobServices/containers/blobs/read"
        ],
        "notActions": [],
        "notDataActions": []
      }
    ],
    "roleName": "Storage Blob Data Reader (Preview)",
    "roleType": "BuiltInRole",
    "type": "Microsoft.Authorization/roleDefinitions"
  }
]
```

Only data operations can be added to the `dataActions` and `notDataActions` properties. Resource providers identify which operations are data operations, by setting the `isDataAction` property to `true`. To see a list of the operations where `isDataAction` is `true`, see [Resource provider operations](resource-provider-operations.md). Roles that do not have data operations are not required to have `dataActions` and `notDataActions` properties within the role definition.

Authorization for all management operation API calls is handled by Azure Resource Manager. Authorization for data operation API calls is handled by either a resource provider or Azure Resource Manager.

### Data operations example

To better understand how management and data operations work, let's consider a specific example. Alice has been assigned the [Owner](built-in-roles.md#owner) role at the subscription scope. Bob has been assigned the [Storage Blob Data Contributor (Preview)](built-in-roles.md#storage-blob-data-contributor-preview) role at a storage account scope. The following diagram shows this example.

![Role-based access control has been extended to support both management and data operations](./media/role-definitions/rbac-management-data.png)

The [Owner](built-in-roles.md#owner) role for Alice and the [Storage Blob Data Contributor (Preview)](built-in-roles.md#storage-blob-data-contributor-preview) role for  Bob have the following actions:

Owner

&nbsp;&nbsp;&nbsp;&nbsp;Actions<br>
&nbsp;&nbsp;&nbsp;&nbsp;`*`

Storage Blob Data Contributor (Preview)

&nbsp;&nbsp;&nbsp;&nbsp;Actions<br>
&nbsp;&nbsp;&nbsp;&nbsp;`Microsoft.Storage/storageAccounts/blobServices/containers/delete`<br>
&nbsp;&nbsp;&nbsp;&nbsp;`Microsoft.Storage/storageAccounts/blobServices/containers/read`<br>
&nbsp;&nbsp;&nbsp;&nbsp;`Microsoft.Storage/storageAccounts/blobServices/containers/write`<br>
&nbsp;&nbsp;&nbsp;&nbsp;DataActions<br>
&nbsp;&nbsp;&nbsp;&nbsp;`Microsoft.Storage/storageAccounts/blobServices/containers/blobs/delete`<br>
&nbsp;&nbsp;&nbsp;&nbsp;`Microsoft.Storage/storageAccounts/blobServices/containers/blobs/read`<br>
&nbsp;&nbsp;&nbsp;&nbsp;`Microsoft.Storage/storageAccounts/blobServices/containers/blobs/write`

Since Alice has a wildcard (`*`) action at a subscription scope, her permissions inherit down to enable her to perform all management actions. However, Alice cannot perform data operations. For example, by default, Alice cannot read the blobs inside a container, but she can read, write, and delete containers.

Bob's permissions are restricted to just the `actions` and `dataActions` specified in the [Storage Blob Data Contributor (Preview)](built-in-roles.md#storage-blob-data-contributor-preview) role. Based on the role, Bob can perform both management and data operations. For example, Bob can read, write, and delete containers in the specified storage account and he can also read, write, and delete the blobs.

### What tools support using RBAC for data operations?

To view and work with data operations, you must have the correct versions of the tools or SDKs:

| Tool  | Version  |
|---------|---------|
| [Azure PowerShell](/powershell/azure/install-azurerm-ps) | 5.6.0 or later |
| [Azure CLI](/cli/azure/install-azure-cli) | 2.0.30 or later |
| [Azure for .NET](/dotnet/azure/) | 2.8.0-preview or later |
| [Azure SDK for Go](/go/azure/azure-sdk-go-install) | 15.0.0 or later |
| [Azure for Java](/java/azure/) | 1.9.0 or later |
| [Azure for Python](/python/azure) | 0.40.0 or later |
| [Azure SDK for Ruby](https://rubygems.org/gems/azure_sdk) | 0.17.1 or later |

## actions

The `actions` permission specifies the management operations that the role allows to be performed. It is a collection of operation strings that identify securable operations of Azure resource providers. Here are some examples of management  operations that can be used in `actions`.

| Operation string    | Description         |
| ------------------- | ------------------- |
| `*/read` | Grants access to read operations for all resource types of all Azure resource providers.|
| `Microsoft.Compute/*` | Grants access to all operations for all resource types in the Microsoft.Compute resource provider.|
| `Microsoft.Network/*/read` | Grants access to read operations for all resource types in the Microsoft.Network resource provider.|
| `Microsoft.Compute/virtualMachines/*` | Grants access to all operations of virtual machines and its child resource types.|
| `microsoft.web/sites/restart/Action` | Grants access to restart a web app.|

## notActions

The `notActions` permission specifies the management operations that are excluded from the allowed `actions`. Use the `notActions` permission if the set of operations that you want to allow is more easily defined by excluding restricted operations. The access granted by a role (effective permissions) is computed by subtracting the `notActions` operations from the `actions` operations.

> [!NOTE]
> If a user is assigned a role that excludes an operation in `notActions`, and is assigned a second role that grants access to the same operation, the user is allowed to perform that operation. `notActions` is not a deny rule – it is simply a convenient way to create a set of allowed operations when specific operations need to be excluded.
>

## dataActions (Preview)

The `dataActions` permission specifies the data operations that the role allows to be performed to your data within that object. For example, if a user has read blob data access to a storage account, then they can read the blobs within that storage account. Here are some examples of data operations that can be used in `dataActions`.

| Operation string    | Description         |
| ------------------- | ------------------- |
| `Microsoft.Storage/storageAccounts/ blobServices/containers/blobs/read` | Returns a blob or a list of blobs. |
| `Microsoft.Storage/storageAccounts/ blobServices/containers/blobs/write` | Returns the result of writing a blob. |
| `Microsoft.Storage/storageAccounts/ queueServices/queues/messages/read` | Returns a message. |
| `Microsoft.Storage/storageAccounts/ queueServices/queues/messages/*` | Returns a message or the result of writing or deleting a message. |

## notDataActions (Preview)

The `notDataActions` permission specifies the data operations that are excluded from the allowed `dataActions`. The access granted by a role (effective permissions) is computed by subtracting the `notDataActions` operations from the `dataActions` operations. Each resource provider provides its respective set of APIs to fulfill data operations.

> [!NOTE]
> If a user is assigned a role that excludes a data operation in `notDataActions`, and is assigned a second role that grants access to the same data operation, the user is allowed to perform that data operation. `notDataActions` is not a deny rule – it is simply a convenient way to create a set of allowed data operations when specific data operations need to be excluded.
>

## assignableScopes

The `assignableScopes` property specifies the scopes (management groups (currently in preview), subscriptions, resource groups, or resources) that the role is available for assignment. You can make the role available for assignment in only the subscriptions or resource groups that require it, and not the clutter user experience for the rest of the subscriptions or resource groups. You must use at least one management group, subscription, resource group, or resource ID.

Built-in roles have `assignableScopes` set to the root scope (`"/"`). The root scope indicates that the role is available for assignment in all scopes. Examples of valid assignable scopes include:

| Scenario | Example |
|----------|---------|
| Role is available for assignment in a single subscription | `"/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e"` |
| Role is available for assignment in two subscriptions | `"/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e", "/subscriptions/e91d47c4-76f3-4271-a796-21b4ecfe3624"` |
| Role is available for assignment only in the Network resource group | `"/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/Network"` |
| Role is available for assignment in all scopes | `"/"` |

For information about `assignableScopes` for custom roles, see [Custom roles](custom-roles.md).

## Next steps

* [Built-in roles](built-in-roles.md)
* [Custom roles](custom-roles.md)
* [Azure Resource Manager resource provider operations](resource-provider-operations.md)
