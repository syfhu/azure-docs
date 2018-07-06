---
title: Customize the sign-in page for your Azure AD tenant | Microsoft Docs
description: Learn how to add a company branding to the Azure sign-in page
services: active-directory
documentationcenter: ''
author: eross-msft
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.component: fundamentals
ms.topic: quickstart
ms.date: 05/23/2018
ms.author: lizross
ms.reviewer: kexia
custom: it-pro
---

# Quickstart: Add company branding to your sign-in page in Azure AD
To avoid confusion, many companies want to apply a consistent look and feel across all the websites and services they manage. Azure Active Directory (Azure AD) provides this capability by allowing you to customize the appearance of the sign-in page with your company logo and custom color schemes. The sign-in page appears when you sign in to web-based applications such as Office 365 that use Azure AD as your identity provider. You interact with this page to enter your credentials.

> [!NOTE]
> * Company branding is available only if you purchased the Premium or Basic license for Azure AD, or have an Office 365 license. To learn if a feature is supported by your license type, check the [Azure Active Directory pricing information page](https://azure.microsoft.com/pricing/details/active-directory/).
> 
> * Azure AD Premium and Basic editions are available for customers in China using the worldwide instance of Azure Active Directory. Azure AD Premium and Basic editions are not currently supported in the Azure service operated by 21Vianet in China. For more information, talk to us at the [Azure Active Directory Forum](https://feedback.azure.com/forums/169401-azure-active-directory/).

## Customizing the sign-in page

<!--You can customize the following elements on the sign-in page: <attach image>-->

Company branding customizations appear on the Azure AD sign-in page when users access a tenant-specific URL such as
[*https://outlook.com/contoso.com*](https://outlook.com/contoso.com).

For example, when users visit www.office.com, the sign-in page doesn't show any company branding customizations because the user has not yet entered credentials. After a user enters their user ID or selects a user tile, company branding displays.

> [!NOTE]
> * Your domain name must appear as “Active" in the **Domains** portion of the Azure portal in which you have configured branding. For more information, see [Add a custom domain name](add-custom-domain.md).
> * Sign-in page branding doesn’t carry over to the sign-in page for personal Microsoft accounts. If your employees or business guests sign in with a personal Microsoft account, their sign-in page does not reflect the branding of your organization.


### Banner logo	

Description | Constraints | Recommendations
------- | ------- | ----------
The banner logo is displayed on the sign-in and the Access panel pages.<br>On the sign-in page, the logo shows after the username is entered. | Transparent JPG or PNG<br>Max height: 36 px<br>Max width: 245 px | Use your organization’s logo here.<br>Use a transparent image. Don’t assume that the background will be white.<br>Do not add padding around your logo in the image or your logo will look disproportionately small.

### Username hint	
Description | Constraints | Recommendations
------- | ------- | ----------
This option customizes the hint text in the username field. | Unicode text up to 64 characters<br>Plain text only | If you expect guest users outside your organization to sign in to your app, we recommend that you do not set up this option.
		 	
### Sign-in page text	
Description | Constraints | Recommendations
------- | ------- | ----------
This option appears at the bottom of the sign-in form and can be used to communicate additional information such as the phone number to your help desk, or a legal statement. | Unicode text up to 256 characters<br>Plain text only (no links or HTML tags)	

### Sign-in page image	
Description | Constraints | Recommendations
------- | ------- | ----------
This option appears in the background of the sign-in page, is anchored to the center of the viewable space, and scales and crops to fill the browser window.	<br>On narrow screens such as mobile phones, this image is not shown.<br>A black mask with 0.55 opacity is applied over this image when the page is loaded. | JPG or PNG<br>Image dimensions: 1920x1080 px<br>File size: &lt; 300 KB | <br>Use images where there isn't a strong subject focus. The opaque sign-in form appears over the center of this image and can cover any part of the image, depending on the size of the browser window.<br>Keep the file size small to ensure quick load times. 

### Sign-in page background color
Description | Constraints | Recommendations
------- | ------- | ----------
This color is used in place of the background image on low-bandwidth connections. |	RGB color in hexadecimal (example: #FFFFFF | We suggest using the primary color of the banner logo or your organization color.

### Square logo image
Description | Constraints | Recommendations
------- | ------- | ----------
This image appears during setup for new Enterprise Windows 10 PCs. It provides context to employees when they set up their new work PC. The image is displayed for tenants that use [Windows AutoPilot](https://blogs.windows.com/business/2017/06/29/delivering-modern-promise-windows-10/?utm_source=dlvr.it&utm_medium=twitter#gDTp1u6q35bvDWIS.97) to deploy their work devices, and on password entry pages in other Windows 10 experiences. | Transparent PNG (preferred) or JPG<br>Image dimensions: 240x240 px<br>File size: &lt; 10 KB | Use your organization’s logo here.<br> Use a transparent image.<br>Don’t assume that the background will be white.<br>Don't add padding to your logo in the image or your logo will look disproportionately small.

### Show option to remain signed in
Description | Constraints | Recommendations
------- | ------- | ----------
Azure AD sign-in gives the user the option to remain signed in when they close and reopen their browser. This setting hides that option.<br>Set to **No** to hide this option from your users. | &nbsp; | Hiding the option does not affect session lifetime.<br>Some features of SharePoint Online and Office 2010 depend on users being able to choose to remain signed in. If you set this option to **No**, your users may see additional and unexpected prompts to sign-in.

> [!NOTE]
> All elements are optional. For example, if you specify a banner logo with no background image, the sign-in page will show your logo and the background image for the destination site (for example, Office 365).

## Add company branding to your directory

1. Sign in to [the Azure AD admin center](https://aad.portal.azure.com) with an account that's a global admin for the tenant.
2. Select **Azure Active Directory** > **Company branding** > **Edit**.
  
  ![Opening custom branding](./media/customize-branding/navigation-to-branding.png)
3. Modify the elements you want to customize. All elements are optional.
  
  ![Edit custom branding](./media/customize-branding/edit-branding.png)
4. When you're done, select **Save**.

It can take up to an hour for any changes you made to the sign-in page branding to appear.

## Add language-specific company branding to your directory

1. Sign in to the [Azure AD admin center](https://aad.portal.azure.com) with an account that's a global admin for the directory.
2. Select **Azure Active Directory** > **Company branding** > **New language**.
  
  ![Add language-specific branding elements](./media/customize-branding/add-language.png)
3. Modify the elements you want to customize. All elements are optional.
4. When you're done, select **Save**.

It can take up to an hour for any changes you made to the sign-in page branding to appear.

## Next steps
In this quickstart, you’ve learned how to add company branding to your Azure AD directory. 

You can use the following link to configure your company branding in Azure AD from the Azure portal.

> [!div class="nextstepaction"]
> [Configure company branding](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/LoginTenantBrandingBlade) 