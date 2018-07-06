---
title: How to add or remove a user role | Microsoft Docs
description: Learn how to add roles to privileged identities with the Azure Active Directory Privileged Identity Management application.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.component: protection
ms.date: 01/03/2018
ms.author: rolyon

---
# Azure AD Privileged Identity Management: How to add or remove a user role
With Azure Active Directory (AD), a global administrator (or company administrator) can update which users are **permanently** assigned to roles in Azure AD. This is done with PowerShell cmdlets like `Add-MsolRoleMember` and `Remove-MsolRoleMember`. Or they can use the Azure portal as described in [assigning administrator roles in Azure Active Directory](active-directory-assign-admin-roles.md).

The Azure AD Privileged Identity Management application allows privileged role administrators to make permanent role assignments, as well. Additionally, privileged role administrators can make users **eligible** for admin roles. An eligible admin can activate the role when they need it, and then their permissions expire once they're done.

## Manage roles with PIM in the Azure portal
In your organization, you can assign users to different administrative roles in Azure AD, Office 365, and other Microsoft services and applications.  More details on the available roles can be found at [Roles in Azure AD PIM](active-directory-privileged-identity-management-roles.md).

To add or remove a user in a role using Privileged Identity Management, bring up the PIM dashboard. Then either click the **Users in Admin Roles** button, or select a specific role (such as Global Administrator) from the roles table.

> [!NOTE]
> If you haven't enabled PIM in the Azure portal yet, go to [Get started with Azure AD PIM](active-directory-privileged-identity-management-getting-started.md) for details.

If you want to give another user access to PIM itself, the roles which PIM requires the user to have are described further in [how to give access to PIM](active-directory-privileged-identity-management-how-to-give-access-to-pim.md).

## Add a user to a role
1. In the [Azure portal](https://portal.azure.com/), select the **Azure AD Privileged Identity Management** tile on the dashboard.
2. Select **Manage privileged roles**.
3. In the **Role summary** table, select the role you want to manage.
4. In the role blade, select **Add**.
5. Click **Select users** and search for the user on the **Select users** blade.  
6. Select the user from the search results list, and click **Done**.
7. Click **OK** to save your selection. The user you have selected will appear in the list as eligible for the role.

> [!NOTE]
> New users in a role are only eligible for the role by default. If you want to make the role permanent, click the user in the list. The user's information will appear in a new blade. Select **Make perm** in the user information menu.  
> If a user cannot register for Azure Multi-Factor Authentication (MFA), or is using a Microsoft account (usually @outlook.com), you need to make them permanent in all their roles. Eligible admins are asked to register for MFA during activation.

Now that the user is eligible for a role, let them know that they can activate it according to the instructions in [How to activate or deactivate a role](active-directory-privileged-identity-management-how-to-activate-role.md).

## Remove a user from a role
You can remove users from eligible role assignments, but make sure there is always at least one user who is a permanent global administrator.

Follow these steps to remove a specific user from a role:

1. Navigate to the role in the role list either by selecting a role in the Azure AD PIM dashboard or by clicking on the **Users in Admin Roles** button.
2. Click on the user in the user list.
3. Click **Remove**. A message will ask you to confirm.
4. Click **Yes** to remove the role from the user.

If you're not sure which users still need their role assignments, then you can [start an access review for the role](active-directory-privileged-identity-management-how-to-start-security-review.md).

## Next steps
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

