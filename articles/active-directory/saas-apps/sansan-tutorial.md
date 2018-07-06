---
title: 'Tutorial: Azure Active Directory integration with Sansan | Microsoft Docs'
description: Learn how to configure single sign-on between Azure Active Directory and Sansan.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman

ms.assetid: f653a0f2-c44a-4670-b936-68c136b578ea
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2018
ms.author: jeedes

---
# Tutorial: Azure Active Directory integration with Sansan

In this tutorial, you learn how to integrate Sansan with Azure Active Directory (Azure AD).

Integrating Sansan with Azure AD provides you with the following benefits:

- You can control in Azure AD who has access to Sansan
- You can enable your users to automatically get signed-on to Sansan (Single Sign-On) with their Azure AD accounts
- You can manage your accounts in one central location - the Azure portal

If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## Prerequisites

To configure Azure AD integration with Sansan, you need the following items:

- An Azure AD subscription
- A Sansan single sign-on enabled subscription

> [!NOTE]
> To test the steps in this tutorial, we do not recommend using a production environment.

To test the steps in this tutorial, you should follow these recommendations:

- Do not use your production environment, unless it is necessary.
- If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).

## Scenario description
In this tutorial, you test Azure AD single sign-on in a test environment. 
The scenario outlined in this tutorial consists of two main building blocks:

1. Adding Sansan from the gallery
2. Configuring and testing Azure AD single sign-on

## Adding Sansan from the gallery
To configure the integration of Sansan into Azure AD, you need to add Sansan from the gallery to your list of managed SaaS apps.

**To add Sansan from the gallery, perform the following steps:**

1. In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon. 

	![Active Directory][1]

2. Navigate to **Enterprise applications**. Then go to **All applications**.

	![Applications][2]
	
3. To add new application, click **New application** button on the top of dialog.

	![Applications][3]

4. In the search box, type **Sansan**.

	![Creating an Azure AD test user](./media/sansan-tutorial/tutorial_sansan_search.png)

5. In the results panel, select **Sansan**, and then click **Add** button to add the application.

	![Creating an Azure AD test user](./media/sansan-tutorial/tutorial_sansan_addfromgallery.png)

##  Configuring and testing Azure AD single sign-on
In this section, you configure and test Azure AD single sign-on with Sansan based on a test user called "Britta Simon".

For single sign-on to work, Azure AD needs to know what the counterpart user in Sansan is to a user in Azure AD. In other words, a link relationship between an Azure AD user and the related user in Sansan needs to be established.

In Sansan, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.

To configure and test Azure AD single sign-on with Sansan, you need to complete the following building blocks:

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.
2. **[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.
3. **[Creating a Sansan test user](#creating-a-sansan-test-user)** - to have a counterpart of Britta Simon in Sansan that is linked to the Azure AD representation of user.
4. **[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.
5. **[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.

### Configuring Azure AD single sign-on

In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Sansan application.

**To configure Azure AD single sign-on with Sansan, perform the following steps:**

1. In the Azure portal, on the **Sansan** application integration page, click **Single sign-on**.

	![Configure Single Sign-On][4]

2. On the **Single sign-on** dialog, select **Mode** as	**SAML-based Sign-on** to enable single sign-on.
 
	![Configure Single Sign-On](./media/sansan-tutorial/tutorial_sansan_samlbase.png)

3. On the **Sansan Domain and URLs** section, perform the following steps:

	![Configure Single Sign-On](./media/sansan-tutorial/tutorial_sansan_url.png)

    In the **Sign-on URL** textbox, type a URL using the following patterns: 
	
	| Environment | URL |
    |:--- |:--- |
    | PC web |`https://ap.sansan.com/v/saml2/<company name>/acs` |
    | Native Mobile app |`https://internal.api.sansan.com/saml2/<company name>/acs` |
    | Mobile browser settings |`https://ap.sansan.com/s/saml2/<company name>/acs` |  

	> [!NOTE] 
	> These values are not real. Update these values with the actual Sign-On URL. Contact [Sansan Client support team](https://www.sansan.com/form/contact) to get these values. 
	 
4. On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.

    ![Configure Single Sign-On](./media/sansan-tutorial/tutorial_sansan_certificate.png) 

5. Click **Save** button.

	![Configure Single Sign-On](./media/sansan-tutorial/tutorial_general_400.png)

6. Sansan application expects multiple **Identifiers** and **Reply URLs** to support multiple environments (PC web, Native Mobile app, Mobile browser settings), which can be configured using PowerShell script. The detailed steps are explained below.

7. To configure multiple **Identifiers** and **Reply URLs** for Sansan application using PowerShell script, perform following steps:

	![Configure Single Sign-On obj](./media/sansan-tutorial/tutorial_sansan_objid.png)	

	a. Go to the **Properties** page of **Sansan** application and copy the **Object ID** using **Copy** button and paste it into Notepad.

	b. The **Object ID**, which you have copied from Azure portal will be used as **ServicePrincipalObjectId** in PowerShell script used later in the tutorial. 

	c. Now open an elevated Windows PowerShell command prompt.
	
	>[!NOTE] 
	> You need to install the AzureAD module (use the command `Install-Module -Name AzureAD`). If prompted to install a NuGet module or the new Azure Active Directory V2 PowerShell module, type Y and press ENTER.

	d. Run `Connect-AzureAD` and sign in with a Global Admin user account.

	e. Use the following script to update multiple URLs to an application:

	```poweshell
	 Param(
	[Parameter(Mandatory=$true)][guid]$ServicePrincipalObjectId,
	[Parameter(Mandatory=$false)][string[]]$ReplyUrls,
	[Parameter(Mandatory=$false)][string[]]$IdentifierUrls
	)

	$servicePrincipal = Get-AzureADServicePrincipal -ObjectId $ServicePrincipalObjectId

	if($ReplyUrls.Length)
	{
	echo "Updating Reply urls"
	Set-AzureADServicePrincipal -ObjectId $ServicePrincipalObjectId -ReplyUrls $ReplyUrls
	echo "updated"
	}
	if($IdentifierUrls.Length)
	{
	echo "Updating Identifier urls"
	$applications = Get-AzureADApplication -SearchString $servicePrincipal.AppDisplayName 
	echo "Found Applications =" $applications.Length
	$i = 0;
	do
	{  
	$application = $applications[$i];
	if($application.AppId -eq $servicePrincipal.AppId){
	Set-AzureADApplication -ObjectId $application.ObjectId -IdentifierUris $IdentifierUrls
	$servicePrincipal = Get-AzureADServicePrincipal -ObjectId $ServicePrincipalObjectId
	echo "Updated"
	return;
	}
	$i++;
	}while($i -lt $applications.Length);
	echo "Not able to find the matched application with this service principal"
	}
	```

8. After successfull completion of PowerShell script, the result of the script will be like this as shown below and the URL values get updated but they won't get reflected in Azure portal. 

	![Configure Single Sign-On script](./media/sansan-tutorial/tutorial_sansan_powershell.png)


9. On the **Sansan Configuration** section, click **Configure Sansan** to open **Configure sign-on** window. Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**

	![Configure Single Sign-On](./media/sansan-tutorial/tutorial_sansan_configure.png) 

10. To configure single sign-on on **Sansan** side, you need to send the downloaded **Certificate**, **Sign-Out URL**, **SAML Entity ID**, and **SAML Single Sign-On Service URL** to [Sansan support team](https://www.sansan.com/form/contact). They set this setting to have the SAML SSO connection set properly on both sides.

>[!NOTE]
>PC browser setting also work for Mobile app and Mobile browser along with PC web. 

### Creating an Azure AD test user

The objective of this section is to create a test user in the Azure portal called Britta Simon.

![Create Azure AD User][100]

**To create a test user in Azure AD, perform the following steps:**

1. In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.

	![Creating an Azure AD test user](./media/sansan-tutorial/create_aaduser_01.png) 

2. To display the list of users, go to **Users and groups** and click **All users**.
	
	![Creating an Azure AD test user](./media/sansan-tutorial/create_aaduser_02.png) 

3. To open the **User** dialog, click **Add** on the top of the dialog.
 
	![Creating an Azure AD test user](./media/sansan-tutorial/create_aaduser_03.png) 

4. On the **User** dialog page, perform the following steps:
 
	![Creating an Azure AD test user](./media/sansan-tutorial/create_aaduser_04.png) 

    a. In the **Name** textbox, type **BrittaSimon**.

    b. In the **User name** textbox, type the **email address** of BrittaSimon.

	c. Select **Show Password** and write down the value of the **Password**.

    d. Click **Create**.
 
### Creating a Sansan test user

In this section, you create a user called Britta Simon in Sansan. Sansan application needs the user to be provisioned in the application before doing SSO. 

>[!NOTE]
>If you need to create a user manually or batch of users, you need to contact the [Sansan support team](https://www.sansan.com/form/contact). 

### Assigning the Azure AD test user

In this section, you enable Britta Simon to use Azure single sign-on by granting access to Sansan.

![Assign User][200] 

**To assign Britta Simon to Sansan, perform the following steps:**

1. In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.

	![Assign User][201] 

2. In the applications list, select **Sansan**.

	![Configure Single Sign-On](./media/sansan-tutorial/tutorial_sansan_app.png) 

3. In the menu on the left, click **Users and groups**.

	![Assign User][202] 

4. Click **Add** button. Then select **Users and groups** on **Add Assignment** dialog.

	![Assign User][203]

5. On **Users and groups** dialog, select **Britta Simon** in the Users list.

6. Click **Select** button on **Users and groups** dialog.

7. Click **Assign** button on **Add Assignment** dialog.
	
### Testing single sign-on

In this section, you test your Azure AD single sign-on configuration using the Access Panel.

When you click the Sansan tile in the Access Panel, you should get automatically signed-on to your Sansan application.
For more information about the Access Panel, see [Introduction to the Access Panel](../active-directory-saas-access-panel-introduction.md).

## Additional resources

* [List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/sansan-tutorial/tutorial_general_01.png
[2]: ./media/sansan-tutorial/tutorial_general_02.png
[3]: ./media/sansan-tutorial/tutorial_general_03.png
[4]: ./media/sansan-tutorial/tutorial_general_04.png

[100]: ./media/sansan-tutorial/tutorial_general_100.png

[200]: ./media/sansan-tutorial/tutorial_general_200.png
[201]: ./media/sansan-tutorial/tutorial_general_201.png
[202]: ./media/sansan-tutorial/tutorial_general_202.png
[203]: ./media/sansan-tutorial/tutorial_general_203.png

