---
title: 'Tutorial: Azure Active Directory integration with Egnyte | Microsoft Docs'
description: Learn how to configure single sign-on between Azure Active Directory and Egnyte.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman

ms.assetid: 8c2101d4-1779-4b36-8464-5c1ff780da18
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/18/2017
ms.author: jeedes

---
# Tutorial: Azure Active Directory integration with Egnyte

In this tutorial, you learn how to integrate Egnyte with Azure Active Directory (Azure AD).

Integrating Egnyte with Azure AD provides you with the following benefits:

- You can control in Azure AD who has access to Egnyte
- You can enable your users to automatically get signed-on to Egnyte (Single Sign-On) with their Azure AD accounts
- You can manage your accounts in one central location - the Azure portal

If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## Prerequisites

To configure Azure AD integration with Egnyte, you need the following items:

- An Azure AD subscription
- An Egnyte single sign-on enabled subscription

> [!NOTE]
> To test the steps in this tutorial, we do not recommend using a production environment.

To test the steps in this tutorial, you should follow these recommendations:

- Do not use your production environment, unless it is necessary.
- If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).

## Scenario description
In this tutorial, you test Azure AD single sign-on in a test environment. 
The scenario outlined in this tutorial consists of two main building blocks:

1. Adding Egnyte from the gallery
2. Configuring and testing Azure AD single sign-on

## Adding Egnyte from the gallery
To configure the integration of Egnyte into Azure AD, you need to add Egnyte from the gallery to your list of managed SaaS apps.

**To add Egnyte from the gallery, perform the following steps:**

1. In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon. 

	![Active Directory][1]

2. Navigate to **Enterprise applications**. Then go to **All applications**.

	![Applications][2]
	
3. To add new application, click **New application** button on the top of dialog.

	![Applications][3]

4. In the search box, type **Egnyte**.

	![Creating an Azure AD test user](./media/egnyte-tutorial/tutorial_egnyte_search.png)

5. In the results panel, select **Egnyte**, and then click **Add** button to add the application.

	![Creating an Azure AD test user](./media/egnyte-tutorial/tutorial_egnyte_addfromgallery.png)

##  Configuring and testing Azure AD single sign-on
In this section, you configure and test Azure AD single sign-on with Egnyte based on a test user called "Britta Simon."

For single sign-on to work, Azure AD needs to know what the counterpart user in Egnyte is to a user in Azure AD. In other words, a link relationship between an Azure AD user and the related user in Egnyte needs to be established.

In Egnyte, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.

To configure and test Azure AD single sign-on with Egnyte, you need to complete the following building blocks:

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.
2. **[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.
3. **[Creating an Egnyte test user](#creating-an-egnyte-test-user)** - to have a counterpart of Britta Simon in Egnyte that is linked to the Azure AD representation of user.
4. **[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.
5. **[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.

### Configuring Azure AD single sign-on

In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Egnyte application.

**To configure Azure AD single sign-on with Egnyte, perform the following steps:**

1. In the Azure portal, on the **Egnyte** application integration page, click **Single sign-on**.

	![Configure Single Sign-On][4]

2. On the **Single sign-on** dialog, select **Mode** as	**SAML-based Sign-on** to enable single sign-on.
 
	![Configure Single Sign-On](./media/egnyte-tutorial/tutorial_egnyte_samlbase.png)

3. On the **Egnyte Domain and URLs** section, perform the following steps:

	![Configure Single Sign-On](./media/egnyte-tutorial/tutorial_egnyte_url.png)

    In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<companyname>.egnyte.com`

	> [!NOTE] 
	> This value is not real. Update this value with the actual Sign-On URL. Contact [Egnyte Client support team](https://www.egnyte.com/corp/contact_egnyte.html) to get this value. 
 
4. On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.

	![Configure Single Sign-On](./media/egnyte-tutorial/tutorial_egnyte_certificate.png) 

5. Click **Save** button.

	![Configure Single Sign-On](./media/egnyte-tutorial/tutorial_general_400.png)

6. On the **Egnyte Configuration** section, click **Configure Egnyte** to open **Configure sign-on** window. Copy the **SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**

	![Configure Single Sign-On](./media/egnyte-tutorial/tutorial_egnyte_configure.png) 

7. In a different web browser window, log in to your Egnyte company site as an administrator.

8. Click **Settings**.
   
   ![Settings](./media/egnyte-tutorial/ic787819.png "Settings")

9. In the menu, click **Settings**.

   ![Settings](./media/egnyte-tutorial/ic787820.png "Settings")

10. Click the **Configuration** tab, and then click **Security**.

	![Security](./media/egnyte-tutorial/ic787821.png "Security")

11. In the **Single Sign-On Authentication** section, perform the following steps:

	![Single Sign On Authentication](./media/egnyte-tutorial/ic787822.png "Single Sign On Authentication")   
	
	a. As **Single sign-on authentication**, select **SAML 2.0**.
   
    b. As **Identity provider**, select **AzureAD**.
   
    c. Paste **SAML Single Sign-On Service URL** copied from Azure portal into the **Identity provider login URL** textbox.
   
    d. Paste **SAML Entity ID** which you have copied from Azure portal into the **Identity provider entity ID** textbox.
      
	e. Open your base-64 encoded certificate in notepad downloaded from Azure portal, copy the content of it into your clipboard, and then paste it to the **Identity provider certificate** textbox.
   
    f. As **Default user mapping**, select **Email address**.
   
    g. As **Use domain-specific issuer value**, select **disabled**.
   
    h. Click **Save**.

> [!TIP]
> You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!  After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom. You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### Creating an Azure AD test user
The objective of this section is to create a test user in the Azure portal called Britta Simon.

![Create Azure AD User][100]

**To create a test user in Azure AD, perform the following steps:**

1. In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.

	![Creating an Azure AD test user](./media/egnyte-tutorial/create_aaduser_01.png) 

2. To display the list of users, go to **Users and groups** and click **All users**.
	
	![Creating an Azure AD test user](./media/egnyte-tutorial/create_aaduser_02.png) 

3. To open the **User** dialog, click **Add** on the top of the dialog.
 
	![Creating an Azure AD test user](./media/egnyte-tutorial/create_aaduser_03.png) 

4. On the **User** dialog page, perform the following steps:
 
	![Creating an Azure AD test user](./media/egnyte-tutorial/create_aaduser_04.png) 

    a. In the **Name** textbox, type **BrittaSimon**.

    b. In the **User name** textbox, type the **email address** of BrittaSimon.

	c. Select **Show Password** and write down the value of the **Password**.

    d. Click **Create**.
 
### Creating an Egnyte test user

To enable Azure AD users to log in to Egnyte, they must be provisioned into Egnyte. In the case of Egnyte, provisioning is a manual task.

**To provision a user accounts, perform the following steps:**

1. Log in to your **Egnyte** company site as administrator.

2. Go to **Settings \> Users & Groups**.

3. Click **Add New User**, and then select the type of user you want to add.
   
   ![Users](./media/egnyte-tutorial/ic787824.png "Users")

4. In the **New Standard User** section, perform the following steps:
   
   ![New Standard User](./media/egnyte-tutorial/ic787825.png "New Standard User")   

   a. Type the **Email**, **Username**, and other details of a valid Azure Active Directory account you want to provision.
   
   b. Click **Save**.
    
	>[!NOTE]
    >The Azure Active Directory account holder will receive a notification email.
    >

>[!NOTE]
>You can use any other Egnyte user account creation tools or APIs provided by Egnyte to provision AAD user accounts.
> 

### Assigning the Azure AD test user

In this section, you enable Britta Simon to use Azure single sign-on by granting access to Egnyte.

![Assign User][200] 

**To assign Britta Simon to Egnyte, perform the following steps:**

1. In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.

	![Assign User][201] 

2. In the applications list, select **Egnyte**.

	![Configure Single Sign-On](./media/egnyte-tutorial/tutorial_egnyte_app.png) 

3. In the menu on the left, click **Users and groups**.

	![Assign User][202] 

4. Click **Add** button. Then select **Users and groups** on **Add Assignment** dialog.

	![Assign User][203]

5. On **Users and groups** dialog, select **Britta Simon** in the Users list.

6. Click **Select** button on **Users and groups** dialog.

7. Click **Assign** button on **Add Assignment** dialog.
	
### Testing single sign-on

In this section, you test your Azure AD single sign-on configuration using the Access Panel.

When you click the Egnyte tile in the Access Panel, you should get automatically signed-on to your Egnyte application.
For more information about the Access Panel, see [Introduction to the Access Panel](../active-directory-saas-access-panel-introduction.md). 

## Additional resources

* [List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/egnyte-tutorial/tutorial_general_01.png
[2]: ./media/egnyte-tutorial/tutorial_general_02.png
[3]: ./media/egnyte-tutorial/tutorial_general_03.png
[4]: ./media/egnyte-tutorial/tutorial_general_04.png

[100]: ./media/egnyte-tutorial/tutorial_general_100.png

[200]: ./media/egnyte-tutorial/tutorial_general_200.png
[201]: ./media/egnyte-tutorial/tutorial_general_201.png
[202]: ./media/egnyte-tutorial/tutorial_general_202.png
[203]: ./media/egnyte-tutorial/tutorial_general_203.png

