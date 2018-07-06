---
title: Self-service password reset deployment guide - Azure Active Directory
description: Tips for successful rollout of Azure AD self-service password reset

services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: get-started-article
ms.date: 01/11/2018

ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: sahenry

---
# How to successfully roll out self-service password reset

To ensure a smooth rollout of the Azure Active directory (Azure AD) self-service password reset (SSPR) functionality, most customers complete the following steps:

> [!VIDEO https://www.youtube.com/embed/OZn5btP6ZXw]

1. [Enable password reset in your directory](quickstart-sspr.md).
2. [Configure on-premises Active Directory permissions for password writeback](howto-sspr-writeback.md#active-directory-permissions).
3. [Configure password writeback](howto-sspr-writeback.md#configure-password-writeback) to write passwords from Azure AD back to your on-premises directory.
4. [Assign and verify the required licenses](concept-sspr-licensing.md).
5. Determine if you want to do a gradual rollout. If you want to roll out SSPR gradually, you can limit access to a group of users so you can pilot the program with a specific group. To roll out to a specific group, set the **Self Service Password Reset Enabled** switch to **Selected** and select the security group you want to  be able use password reset.  Nesting of security groups is supported here.
6. Populate the [authentication data](howto-sspr-authenticationdata.md) needed for your users to register, such as their office phone, mobile phone, and alternate email address.
7. [Customize the Azure AD sign-in experience to include your company branding](concept-sspr-customization.md).
8. Teach your users how to use SSPR. Send them instructions to show them how to register and how to reset their passwords.
9. Determine when you want to enforce registration. You can choose to enforce registration at any point. You can also require users to reconfirm their authentication information after a certain period of time.
10. Use the reporting capability. Over time, you can review the users registration and usage with the [reporting capability that Azure AD provides](howto-sspr-reporting.md).
11. Enable password reset. When you're ready, enable password reset for all users by setting the **Self Service Password Reset Enabled** switch to **All**. 

   > [!NOTE]
   > Changing this option from a selected group to everyone does not invalidate existing authentication data that a user has registered as part of a test group. Users who are configured and have valid authentication data registered continue to function.

12. [Enable Windows 10 users to reset their password at the login screen](tutorial-sspr-windows.md).

   > [!IMPORTANT]
   > Test SSPR with a user, rather than an administrator, as Microsoft enforces strong authentication requirements for Azure administrator accounts. For more information regarding the administrator password policy, see our [password policy](concept-sspr-policy.md#administrator-password-policy-differences) article.

## Email-based rollout

Many customers find that the easiest way to get users to use SSPR is with an email campaign that includes simple-to-use instructions. [We have created three simple emails that you can use as templates to help in your rollout](https://www.microsoft.com/download/details.aspx?id=56768):

* **Coming soon**: An email template that you use in the weeks or days before the rollout to let users know they need to do something.
* **Available now**: An email template that you use the day of the program launch to drive users to register and confirm their authentication data. If users register now, they have SSPR available when they need it.
* **Sign-up reminder**: An email template for a few days to a few weeks after deployment to remind users to register and confirm their authentication data.

![Email][Email]

## Create your own password portal

Many customers choose to host a webpage and create a root DNS entry, like https://passwords.contoso.com. They populate this page with links to the following information:

* [Azure AD password reset portal - https://aka.ms/sspr](https://aka.ms/sspr)
* [Azure AD password reset registration portal - https://aka.ms/ssprsetup](https://aka.ms/ssprsetup)
* [Azure AD password change portal - https://account.activedirectory.windowsazure.com/ChangePassword.aspx](https://account.activedirectory.windowsazure.com/ChangePassword.aspx)
* Other organization-specific information

In any email communications or fliers you send out you can include a branded, memorable URL that users can go to when they need to use the services. For your benefit, we have created a [sample password reset page](https://github.com/ajamess/password-reset-page) that you can use and customize to your organization’s needs.

## Step-by-step deployment plan

The Azure Active Directory product group has created a [step-by-step deployment plan](https://aka.ms/SSPRDeploymentPlan) that organizations can use in parallel with the documentation found on this site to make a business case and plan for deployment of self-service password reset.

## Use enforced registration

If you want your users to register for password reset, you can require that they register when they sign in through Azure AD. You can enable this option from your directory’s **Password reset** pane by enabling the **Require Users to Register when Signing in** option on the **Registration** tab.

Administrators can require users to re-register after a specific period of time. They can set the **Number of days before users are asked to reconfirm their authentication information** option to 0 to 730 days.

After you enable this option, when users sign in they see a message that says their administrator has required them to verify their authentication information.

## Populate authentication data

You should [populate the authentication data for your users](howto-sspr-authenticationdata.md). That way users don't need to register for password reset before they are able to use SSPR. As long as users have provided the authentication data that meets the password reset policy you have defined, they are able to reset their passwords.

## Disable self-service password reset

It's easy to disable self-service password reset. Open your Azure AD tenant and go to **Password Reset** > **Properties**, and then select **None** under **Self Service Password Reset Enabled**.

## Next steps

* [Reset or change your password](../active-directory-passwords-update-your-own-password.md)
* [Register for self-service password reset](../active-directory-passwords-reset-register.md)
* [Do you have a licensing question?](concept-sspr-licensing.md)
* [What data is used by SSPR and what data should you populate for your users?](howto-sspr-authenticationdata.md)
* [What are the policy options with SSPR?](concept-sspr-policy.md)
* [What is password writeback and why do I care about it?](howto-sspr-writeback.md)
* [How do I report on activity in SSPR?](howto-sspr-reporting.md)
* [What are all of the options in SSPR and what do they mean?](concept-sspr-howitworks.md)
* [I think something is broken. How do I troubleshoot SSPR?](active-directory-passwords-troubleshoot.md)
* [I have a question that was not covered somewhere else](active-directory-passwords-faq.md)

[Email]: ./media/howto-sspr-deployment/sspr-emailtemplates.png "Customize these email templates to fit your organizational requirements"
