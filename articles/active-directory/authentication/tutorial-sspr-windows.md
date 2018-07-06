---
title: 'Azure AD SSPR from the Windows 10 login screen | Microsoft Docs'
description: Configure Windows 10 login screen Azure AD password reset and I forgot my PIN

services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: get-started-article
ms.date: 04/27/2018

ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: sahenry

---
# Azure AD password reset from the login screen

You have already deployed Azure AD self-service password reset (SSPR) but your users still call the helpdesk when they forget their passwords. They call the helpdesk because they can't get to a web browser to access SSPR.

With the new Windows 10 April 2018 Update, users with **Azure AD joined** or **hybrid Azure AD joined** devices can see and use a “Reset password” link on their login screen. When they click this link, they are brought to the same self-service password reset (SSPR) experience they are familiar with.

To enable users to reset their Azure AD password from the Windows 10 login screen, the following requirements need to be met:

* Windows 10 April 2018 Update, or newer client that is [Azure AD joined](../device-management-azure-portal.md) or [hybrid Azure AD joined](../device-management-hybrid-azuread-joined-devices-setup.md).
* Azure AD self-service password reset must be enabled.
* Configure and deploy the setting to enable the Reset password link via one of the following methods:
   * [Intune device configuration profile](tutorial-sspr-windows.md#configure-reset-password-link-using-intune)
   * [Registry key](tutorial-sspr-windows.md#configure-reset-password-link-using-the-registry)

## Configure Reset password link using Intune

### Create a device configuration policy in Intune

1. Log in to the [Azure portal](https://portal.azure.com) and click on **Intune**.
2. Create a new device configuration profile by going to **Device configuration** > **Profiles** > **Create Profile**
   * Provide a meaningful name for the profile
   * Optionally provide a meaningful description of the profile
   * Platform **Windows 10 and later**
   * Profile type **Custom**

   ![CreateProfile][CreateProfile]

3. Configure **Settings**
   * **Add** the following OMA-URI Setting to enable the Reset password link
      * Provide a meaningful name to explain what the setting is doing
      * Optionally provide a meaningful description of the setting
      * **OMA-URI** set to `./Vendor/MSFT/Policy/Config/Authentication/AllowAadPasswordReset`
      * **Data type** set to **Integer**
      * **Value** set to **1**
      * Click **OK**
   * Click **OK**
4. Click **Create**

### Assign a device configuration policy in Intune

#### Create a group to apply device configuration policy to

1. Log in to the [Azure portal](https://portal.azure.com) and click on **Azure Active Directory**.
2. Browse to **Users and groups** > **All groups** > **New group**
3. Provide a name for the group and under **Membership type** choose **Assigned**
   * Under **Members**, choose the Azure AD joined Windows 10 devices that you want to apply the policy to.
   * Click **Select**
4. Click **Create**

More information on creating groups can be found in the article [Manage access to resources with Azure Active Directory groups](../fundamentals/active-directory-manage-groups.md).

#### Assign device configuration policy to device group

1. Log in to the [Azure portal](https://portal.azure.com) and click on **Intune**.
2. Find the device configuration profile created previously by going to **Device configuration** > **Profiles** > Click on the profile created earlier
3. Assign the profile to a group of devices 
   * Click on **Assignments** > under **Include** > **Select groups to include**
   * Select the group created previously and click **Select**
   * Click on **Save**

   ![Assignment][Assignment]

You have now created and assigned a device configuration policy to enable the Reset password link on the logon screen using Intune.

## Configure Reset password link using the registry

We recommend using this method only to test the setting change.

1. Log in to the Azure AD joined device using administrative credentials
2. Run **regedit** as an administrator
3. Set the following registry key
   * `HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\AzureADAccount`
      * `"AllowPasswordReset"=dword:00000001`

## What do users see

Now that the policy is configured and assigned, what changes for the user? How do they know that they can reset their password at the logon screen?

![LoginScreen][LoginScreen]

When users attempt to log in, they now see a Reset password link that opens the self-service password reset experience at the logon screen. This functionality allows users to reset their password without having to use another device to access a web browser.

Your users will find guidance for using this feature in [Reset your work or school password](../active-directory-passwords-update-your-own-password.md#reset-password-at-sign-in)

## Common issues

When testing this functionality using Hyper-V, the "Reset password" link does not appear.

* Go to the VM you are using to test click on **View** and then uncheck **Enhanced session**.

When testing this functionality using Remote Desktop, the "Reset password" link does not appear

* Password reset is not currently supported from a Remote Desktop.

## Next steps

The following links provide additional information regarding password reset using Azure AD

* [How do I deploy SSPR?](howto-sspr-deployment.md)
* [How do I enable PIN reset from the login screen?](https://docs.microsoft.com/intune/device-windows-pin-reset)
* [More information about MDM authentication policies](https://docs.microsoft.com/windows/client-management/mdm/policy-csp-authentication)

[CreateProfile]: ./media/tutorial-sspr-windows/create-profile.png "Create Intune device configuration profile to enable Reset password link on the Windows 10 logon screen"
[Assignment]: ./media/tutorial-sspr-windows/profile-assignment.png "Assign Intune device configuration policy to a group of Windows 10 devices"
[LoginScreen]: ./media/tutorial-sspr-windows/logon-reset-password.png "Reset password link at the Windows 10 logon screen"
