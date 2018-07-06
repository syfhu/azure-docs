---
title: 'Tutorial: Azure Active Directory integration with Trakstar | Microsoft Docs'
description: Learn how to configure single sign-on between Azure Active Directory and Trakstar.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman

ms.assetid: 411cb8c3-95c6-4138-acf2-ffc7f663e89a
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: jeedes

---
# Tutorial: Azure Active Directory integration with Trakstar

In this tutorial, you learn how to integrate Trakstar with Azure Active Directory (Azure AD).

Integrating Trakstar with Azure AD provides you with the following benefits:

- You can control in Azure AD who has access to Trakstar
- You can enable your users to automatically get signed-on to Trakstar (Single Sign-On) with their Azure AD accounts
- You can manage your accounts in one central location - the Azure portal

If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## Prerequisites

To configure Azure AD integration with Trakstar, you need the following items:

- An Azure AD subscription
- A Trakstar single-sign on enabled subscription
    - SSO is a paid feature in Trakstar. To enable it for your organization, reach out to [Trakstar Client support team](mailto:support@trakstar.com).

> [!NOTE]
> To test the steps in this tutorial, we do not recommend using a production environment.

To test the steps in this tutorial, you should follow these recommendations:

- Do not use your production environment, unless it is necessary.
- If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).

## Scenario description
In this tutorial, you test Azure AD single sign-on in a test environment. 
The scenario outlined in this tutorial consists of two main building blocks:

1. Adding Trakstar from the gallery
2. Configuring and testing Azure AD single sign-on

## Adding Trakstar from the gallery
To configure the integration of Trakstar into Azure AD, you need to add Trakstar from the gallery to your list of managed SaaS apps.

**To add Trakstar from the gallery, perform the following steps:**

1. In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon. 

	![Active Directory][1]

2. Navigate to **Enterprise applications**. Then go to **All applications**.

	![Applications][2]
	
3. To add new application, click **New application** button on the top of dialog.

	![Applications][3]

4. In the search box, type **Trakstar**.

	![Creating an Azure AD test user](./media/trakstar-tutorial/tutorial_trakstar_search.png)

5. In the results panel, select **Trakstar**, and then click **Add** button to add the application.

	![Creating an Azure AD test user](./media/trakstar-tutorial/tutorial_trakstar_addfromgallery.png)

##  Configuring and testing Azure AD single sign-on
In this section, you configure and test Azure AD single sign-on with Trakstar based on a test user called "Britta Simon."

For single sign-on to work, Azure AD needs to know what the counterpart user in Trakstar is to a user in Azure AD. In other words, a link relationship between an Azure AD user and the related user in Trakstar needs to be established.

In Trakstar, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.

To configure and test Azure AD single sign-on with Trakstar, you need to complete the following building blocks:

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.
2. **[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.
3. **[Creating a Trakstar test user](#creating-a-trakstar-test-user)** - to have a counterpart of Britta Simon in Trakstar that is linked to the Azure AD representation of user.
4. **[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.
5. **[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.

### Configuring Azure AD single sign-on

In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Trakstar application.

**To configure Azure AD single sign-on with Trakstar, perform the following steps:**

1. In the Azure portal, on the **Trakstar** application integration page, click **Single sign-on**.

	![Configure Single Sign-On][4]

2. On the **Single sign-on** dialog, select **Mode** as	**SAML-based Sign-on** to enable single sign-on.
 
	![Configure Single Sign-On](./media/trakstar-tutorial/tutorial_trakstar_samlbase.png)

3. On the **Trakstar Domain and URLs** section, perform the following steps:

	![Configure Single Sign-On](./media/trakstar-tutorial/tutorial_trakstar_url.png)

    a. In the **Sign-on URL** textbox, copy the value found in the **ACS (Consumer) URL** within Trakstar (Settings > Authentication & SSO) in the format: `https://app.trakstar.com/auth/saml/callback?namespace=<YOUR_NAMESPACE>`

	b. In the **Identifier** textbox, leave the default: `https://app.trakstar.com`

	> [!NOTE] 
	> These values are not real. Update these values with the actual Sign-On URL and Identifier. Log into Trakstar as an Administrator to get these values.
	> If you don't see the "Authentication & SSO" tab within Settings, you might not have the feature
 
4. On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.

	![Configure Single Sign-On](./media/trakstar-tutorial/tutorial_trakstar_certificate.png) 

5. Click **Save** button.

	![Configure Single Sign-On](./media/trakstar-tutorial/tutorial_general_400.png)

6. On the **Trakstar Configuration** section, click **Configure Trakstar** to open **Configure sign-on** window. Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**

	![Configure Single Sign-On](./media/trakstar-tutorial/tutorial_trakstar_configure.png) 

7. To configure single sign-on on **Trakstar** side, you need to log in as an Administrator and enter the **Certificate (Base64)**, **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL**. 

> [!TIP]
> You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!  After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom. You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### Creating an Azure AD test user
The objective of this section is to create a test user in the Azure portal called Britta Simon.

![Create Azure AD User][100]

**To create a test user in Azure AD, perform the following steps:**

1. In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.

	![Creating an Azure AD test user](./media/trakstar-tutorial/create_aaduser_01.png) 

2. To display the list of users, go to **Users and groups** and click **All users**.
	
	![Creating an Azure AD test user](./media/trakstar-tutorial/create_aaduser_02.png) 

3. To open the **User** dialog, click **Add** on the top of the dialog.
 
	![Creating an Azure AD test user](./media/trakstar-tutorial/create_aaduser_03.png) 

4. On the **User** dialog page, perform the following steps:
 
	![Creating an Azure AD test user](./media/trakstar-tutorial/create_aaduser_04.png) 

    a. In the **Name** textbox, type **BrittaSimon**.

    b. In the **User name** textbox, type the **email address** of BrittaSimon.

	c. Select **Show Password** and write down the value of the **Password**.

    d. Click **Create**.
 
### Creating a Trakstar test user

The objective of this section is to create a user called Britta Simon in Trakstar.


### Assigning the Azure AD test user

In this section, you enable Britta Simon to use Azure single sign-on by granting access to Trakstar.

![Assign User][200] 

**To assign Britta Simon to Trakstar, perform the following steps:**

1. In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.

	![Assign User][201] 

2. In the applications list, select **Trakstar**.

	![Configure Single Sign-On](./media/trakstar-tutorial/tutorial_trakstar_app.png) 

3. In the menu on the left, click **Users and groups**.

	![Assign User][202] 

4. Click **Add** button. Then select **Users and groups** on **Add Assignment** dialog.

	![Assign User][203]

5. On **Users and groups** dialog, select **Britta Simon** in the Users list.

6. Click **Select** button on **Users and groups** dialog.

7. Click **Assign** button on **Add Assignment** dialog.
	
### Testing single sign-on

The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.  
When you click the Trakstar tile in the Access Panel, you should get automatically signed-on to your Trakstar application. 

## Additional resources

* [List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/trakstar-tutorial/tutorial_general_01.png
[2]: ./media/trakstar-tutorial/tutorial_general_02.png
[3]: ./media/trakstar-tutorial/tutorial_general_03.png
[4]: ./media/trakstar-tutorial/tutorial_general_04.png

[100]: ./media/trakstar-tutorial/tutorial_general_100.png

[200]: ./media/trakstar-tutorial/tutorial_general_200.png
[201]: ./media/trakstar-tutorial/tutorial_general_201.png
[202]: ./media/trakstar-tutorial/tutorial_general_202.png
[203]: ./media/trakstar-tutorial/tutorial_general_203.png

