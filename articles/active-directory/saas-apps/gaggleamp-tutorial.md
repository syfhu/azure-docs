---
title: 'Tutorial: Azure Active Directory integration with GaggleAMP | Microsoft Docs'
description: Learn how to configure single sign-on between Azure Active Directory and GaggleAMP.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman

ms.assetid: 9cc1a4b7-964b-406b-9e0c-05cb1a7c9856
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/06/2018
ms.author: jeedes

---
# Tutorial: Azure Active Directory integration with GaggleAMP

In this tutorial, you learn how to integrate GaggleAMP with Azure Active Directory (Azure AD).

Integrating GaggleAMP with Azure AD provides you with the following benefits:

- You can control in Azure AD who has access to GaggleAMP
- You can enable your users to automatically get signed-on to GaggleAMP (Single Sign-On) with their Azure AD accounts
- You can manage your accounts in one central location - the Azure portal

If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## Prerequisites

To configure Azure AD integration with GaggleAMP, you need the following items:

- An Azure AD subscription
- A GaggleAMP single sign-on enabled subscription

> [!NOTE]
> To test the steps in this tutorial, we do not recommend using a production environment.

To test the steps in this tutorial, you should follow these recommendations:

- Do not use your production environment, unless it is necessary.
- If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).

## Scenario description
In this tutorial, you test Azure AD single sign-on in a test environment. 
The scenario outlined in this tutorial consists of two main building blocks:

1. Adding GaggleAMP from the gallery
2. Configuring and testing Azure AD single sign-on

## Adding GaggleAMP from the gallery
To configure the integration of GaggleAMP into Azure AD, you need to add GaggleAMP from the gallery to your list of managed SaaS apps.

**To add GaggleAMP from the gallery, perform the following steps:**

1. In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon. 

	![Active Directory][1]

2. Navigate to **Enterprise applications**. Then go to **All applications**.

	![Applications][2]
	
3. To add new application, click **New application** button on the top of dialog.

	![Applications][3]

4. In the search box, type **GaggleAMP**.

	![Creating an Azure AD test user](./media/gaggleamp-tutorial/tutorial_gaggleamp_search.png)

5. In the results panel, select **GaggleAMP**, and then click **Add** button to add the application.

	![Creating an Azure AD test user](./media/gaggleamp-tutorial/tutorial_gaggleamp_addfromgallery.png)

##  Configuring and testing Azure AD single sign-on
In this section, you configure and test Azure AD single sign-on with GaggleAMP based on a test user called "Britta Simon".

For single sign-on to work, Azure AD needs to know what the counterpart user in GaggleAMP is to a user in Azure AD. In other words, a link relationship between an Azure AD user and the related user in GaggleAMP needs to be established.

In GaggleAMP, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.

To configure and test Azure AD single sign-on with GaggleAMP, you need to complete the following building blocks:

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.
2. **[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.
3. **[Creating a GaggleAMP test user](#creating-a-gaggleamp-test-user)** - to have a counterpart of Britta Simon in GaggleAMP that is linked to the Azure AD representation of user.
4. **[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.
5. **[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.

### Configuring Azure AD single sign-on

In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your GaggleAMP application.

**To configure Azure AD single sign-on with GaggleAMP, perform the following steps:**

1. In the Azure portal, on the **GaggleAMP** application integration page, click **Single sign-on**.

	![Configure Single Sign-On][4]

2. On the **Single sign-on** dialog, select **Mode** as	**SAML-based Sign-on** to enable single sign-on.
 
	![Configure Single Sign-On](./media/gaggleamp-tutorial/tutorial_gaggleamp_samlbase.png)

3. On the **GaggleAMP Domain and URLs** section, perform the following steps if you wish to configure the application in **IDP** initiated mode:

	![Configure Single Sign-On](./media/gaggleamp-tutorial/tutorial_gaggleamp_url.png)

     In the **Identifier** textbox, type the URL: `https://accounts.gaggleamp.com/auth/saml/callback`

4. Check **Show advanced URL settings** and perform the following step if you wish to configure the application in **SP** initiated mode:

    ![Configure Single Sign-On](./media/gaggleamp-tutorial/tutorial_gaggleamp_url1.png)

     In the **Sign-on URL** textbox, type a URL using the following pattern: `https://gaggleamp.com/i/<customerid>`

    > [!NOTE]
    > The Sign-on URL value is not real. Update this value with the actual Sign-on URL. Contact [GaggleAMP Client support team](mailto:sales@gaggleamp.com) to get this value.
 
5. On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.

	![Configure Single Sign-On](./media/gaggleamp-tutorial/tutorial_gaggleamp_certificate.png) 

6. Click **Save** button.

	![Configure Single Sign-On](./media/gaggleamp-tutorial/tutorial_general_400.png)

7. On the **GaggleAMP Configuration** section, click **Configure GaggleAMP** to open **Configure sign-on** window. Copy the **SAML Entity ID and SAML Single Sign-On Service URL** from the **Quick Reference section.**

	![Configure Single Sign-On](./media/gaggleamp-tutorial/tutorial_gaggleamp_configure.png) 

8. In another browser instance, navigate to the SAML SSO page created for you by the Gaggle support team (for example: *https://accounts.gaggleamp.com/saml_configurations/oXH8sQcP79dOzgFPqrMTyw/edit*).

9. On your **SAML SSO** page, perform the following steps:  
   
    ![GaggleAMP Single Sign-On](./media/gaggleamp-tutorial/tutorial_gaggleamp_06.png)

	a. Select **Other** form the **Identity provider** dropdown menu.
	
	b. In the **Identity Provider Issuer** textbox, paste the value of **Issuer URL** which you have copied from Azure portal.
	
	c. In the **Identity Provider Single Sign-On URL** textbox, paste the  value of **Single Sign-On Service URL** which you have copied from Azure portal.
	
	d. Open your downloaded **Certificate(Base64)** file in notepad, copy the content of it into your clipboard, and then paste it to the **X.509 Certificate** textbox.
	
	e. Click **Save**.

### Creating an Azure AD test user
The objective of this section is to create a test user in the Azure portal called Britta Simon.

![Create Azure AD User][100]

**To create a test user in Azure AD, perform the following steps:**

1. In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.

	![Creating an Azure AD test user](./media/gaggleamp-tutorial/create_aaduser_01.png) 

2. To display the list of users, go to **Users and groups** and click **All users**.
	
	![Creating an Azure AD test user](./media/gaggleamp-tutorial/create_aaduser_02.png) 

3. To open the **User** dialog, click **Add** on the top of the dialog.
 
	![Creating an Azure AD test user](./media/gaggleamp-tutorial/create_aaduser_03.png) 

4. On the **User** dialog page, perform the following steps:
 
	![Creating an Azure AD test user](./media/gaggleamp-tutorial/create_aaduser_04.png) 

    a. In the **Name** textbox, type **BrittaSimon**.

    b. In the **User name** textbox, type the **email address** of BrittaSimon.

	c. Select **Show Password** and write down the value of the **Password**.

    d. Click **Create**.
 
### Creating a GaggleAMP test user

The objective of this section is to create a user called Britta Simon in GaggleAMP. GaggleAMP supports just-in-time provisioning, which is by default enabled.

There is no action item for you in this section. A new user is created during an attempt to access GaggleAMP if it doesn't exist yet. 

### Assigning the Azure AD test user

In this section, you enable Britta Simon to use Azure single sign-on by granting access to GaggleAMP.

![Assign User][200] 

**To assign Britta Simon to GaggleAMP, perform the following steps:**

1. In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.

	![Assign User][201] 

2. In the applications list, select **GaggleAMP**.

	![Configure Single Sign-On](./media/gaggleamp-tutorial/tutorial_gaggleamp_app.png) 

3. In the menu on the left, click **Users and groups**.

	![Assign User][202] 

4. Click **Add** button. Then select **Users and groups** on **Add Assignment** dialog.

	![Assign User][203]

5. On **Users and groups** dialog, select **Britta Simon** in the Users list.

6. Click **Select** button on **Users and groups** dialog.

7. Click **Assign** button on **Add Assignment** dialog.
	
### Testing single sign-on

The objective of this section is to test your Azure AD SSO configuration using the Access Panel.

When you click the GaggleAMP tile in the Access Panel, you should get automatically signed-on to your GaggleAMP application.

## Additional resources

* [List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/gaggleamp-tutorial/tutorial_general_01.png
[2]: ./media/gaggleamp-tutorial/tutorial_general_02.png
[3]: ./media/gaggleamp-tutorial/tutorial_general_03.png
[4]: ./media/gaggleamp-tutorial/tutorial_general_04.png

[100]: ./media/gaggleamp-tutorial/tutorial_general_100.png

[200]: ./media/gaggleamp-tutorial/tutorial_general_200.png
[201]: ./media/gaggleamp-tutorial/tutorial_general_201.png
[202]: ./media/gaggleamp-tutorial/tutorial_general_202.png
[203]: ./media/gaggleamp-tutorial/tutorial_general_203.png
