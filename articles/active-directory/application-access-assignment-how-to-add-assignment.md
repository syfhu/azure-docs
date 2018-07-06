---
title: How to assign users and groups to an application | Microsoft Docs
description: Assign users to the application to grant access
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman

ms.assetid: 
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: barbkess

---

# How to assign users and groups to an application

Before your users can do any of the following for a specific application, you need to first **assign them to the application** to grant them access:

-   Access an application by **navigating to the application’s URL directly** (also known as SP-initiated sign-on).

-   Access an application by using the **User Access URL** on an application’s **Properties** page (also known as IDP-initiated sign on).

-   See an application appear on their [Application Access Panel](https://myapps.microsoft.com/) or mobile application.

-   See an application appear on their [Office 365 Application Launcher](https://support.office.com/article/Meet-the-Office-365-app-launcher-79f12104-6fed-442f-96a0-eb089a3f476a).

## Methods to assign applications with Azure Active Directory 

There are 3 ways you can assign applications with Azure Active Directory:

-   [Assign a user directly to an application as an administrator](#assign-a-user-directly-as-an-administrator)

-   [Assign a group directly to an application as an administrator](#assign-a-group-directly-to-an-application-as-an-administrator)

-   [Enable self-service application access to allow users to find their own applications](#enable-self-service-application-access-to-allow-users-to-find-their-own-applications)

## Assign a user directly as an administrator

To assign one or more users to an application directly, follow the steps below:

1.  Open the [**Azure portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**

2.  Open the **Azure Active Directory Extension** by clicking **All services** at the top of the main left hand navigation menu.

3.  Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.

4.  click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.

5.  click **All Applications** to view a list of all your applications.

  * If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**

6.  Select the application you want to assign a user to from the list.

7.  Once the application loads, click **Users and Groups** from the application’s left hand navigation menu.

8.  Click the **Add** button on top of the **Users and Groups** list to open the **Add Assignment** pane.

9.  click the **Users and groups** selector from the **Add Assignment** pane.

10. Type in the **full name** or **email address** of the user you are interested in assigning into the **Search by name or email address** search box.

11. Hover over the **user** in the list to reveal a **checkbox**. Click the checkbox next to the user’s profile photo or logo to add your user to the **Selected** list.

12. **Optional:** If you would like to **add more than one user**, type in another **full name** or **email address** into the **Search by name or email address** search box, and click the checkbox to add this user to the **Selected** list.

13. When you are finished selecting users, click the **Select** button to add them to the list of users and groups to be assigned to the application.

14. **Optional:** click the **Select Role** selector in the **Add Assignment** pane to select a role to assign to the users you have selected.

15. Click the **Assign** button to assign the application to the selected users.

After a short period of time, the users you have selected be able to launch these applications using the methods described in the solution description section.

## Assign a group directly to an application as an administrator

To assign one or more groups to an application directly, follow the steps below:

1.  Open the [**Azure portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**

2.  Open the **Azure Active Directory Extension** by clicking **All services** at the top of the main left hand navigation menu.

3.  Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.

4.  click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.

5.  click **All Applications** to view a list of all your applications.

  * If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**

6.  Select the application you want to assign a user to from the list.

7.  Once the application loads, click **Users and Groups** from the application’s left hand navigation menu.

8.  Click the **Add** button on top of the **Users and Groups** list to open the **Add Assignment** pane.

9.  click the **Users and groups** selector from the **Add Assignment** pane.

10. Type in the **full group name** of the group you are interested in assigning into the **Search by name or email address** search box.

11. Hover over the **group** in the list to reveal a **checkbox**. Click the checkbox next to the group’s profile photo or logo to add your user to the **Selected** list.

12. **Optional:** If you would like to **add more than one group**, type in another **full group name** into the **Search by name or email address** search box, and click the checkbox to add this group to the **Selected** list.

13. When you are finished selecting groups, click the **Select** button to add them to the list of users and groups to be assigned to the application.

14. **Optional:** click the **Select Role** selector in the **Add Assignment** pane to select a role to assign to the groups you have selected.

15. Click the **Assign** button to assign the application to the selected groups.

After a short period of time, the users within the groups you have selected be able to launch these applications using the methods described in the solution description section. If these are dynamic groups, there may be some additional processing delay in these assignments appearing for users within these assigned groups.

## Enable self-service application access to allow users to find their own applications

Self-service application access is a great way to allow users to self-discover applications, optionally allow the business group to approve access to those applications. You can allow the business group to manage the credentials assigned to those users for Password Single-Sign On Applications right from their access panels.

To enable self-service application access to an application, follow the steps below:

1.  Open the [**Azure portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**

2.  Open the **Azure Active Directory Extension** by clicking **All services** at the top of the main left hand navigation menu.

3.  Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.

4.  click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.

5.  click **All Applications** to view a list of all your applications.

   * If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**

6.  Select the application you want to enable Self-service access to from the list.

7.  Once the application loads, click **Self-service** from the application’s left hand navigation menu.

8.  To enable Self-service application access for this application, turn the **Allow users to request access to this application?** toggle to **Yes.**

9.  Next, to select the group to which users who request access to this application should be added, click the selector next to the label **To which group should assigned users be added?** and select a group.

10. **Optional:** If you wish to require a business approval before users are allowed access, set the **Require approval before granting access to this application?** toggle to **Yes**.

11. **Optional: For applications using password single-sign on only,** if you wish to allow those business approvers to specify the passwords that are sent to this application for approved users, set the **Allow approvers to set user’s passwords for this application?** toggle to **Yes**.

12. **Optional:** To specify the business approvers who are allowed to approve access to this application, click the selector next to the label **Who is allowed to approve access to this application?** to select up to 10 individual business approvers.

  >[!NOTE]
  >Groups are not supported.
  >
  >

13. **Optional:** **For applications which expose roles**, if you wish to assign self-service approved users to a role, click the selector next to the **To which role should users be assigned in this application?** to select the role to which these users should be assigned.

14. Click the **Save** button at the top of the pane to finish.

Once you complete Self-service application configuration, users can navigate to their [Application Access Panel](https://myapps.microsoft.com/) and click the **+Add** button to find the apps to which you have enabled Self-service access. Business approvers also see a notification in their [Application Access Panel](https://myapps.microsoft.com/). You can enable an email notifying them when a user has requested access to an application that requires their approval. 

These approvals support single approval workflows only, meaning that if you specify multiple approvers, any single approver may approver access to the application.

## Next steps
[Provide single sign-on to your apps with Application Proxy](manage-apps/application-proxy-configure-single-sign-on-with-kcd.md)
