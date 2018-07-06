---
title: 'Tutorial: Azure Active Directory integration with Pantheon | Microsoft Docs'
description: Learn how to configure single sign-on between Azure Active Directory and Pantheon.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman

ms.assetid: d2c965d1-666f-44c2-b08f-b73163096374
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2017
ms.author: jeedes

---
# Tutorial: Azure Active Directory integration with Pantheon

In this tutorial, you learn how to integrate Pantheon with Azure Active Directory (Azure AD).

Integrating Pantheon with Azure AD provides you with the following benefits:

- You can control in Azure AD who has access to Pantheon
- You can enable your users to automatically get signed-on to Pantheon (Single Sign-On) with their Azure AD accounts
- You can manage your accounts in one central location - the Azure portal

If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## Prerequisites

To configure Azure AD integration with Pantheon, you need the following items:

- An Azure AD subscription
- A Pantheon single sign-on enabled subscription

> [!NOTE]
> To test the steps in this tutorial, we do not recommend using a production environment.

To test the steps in this tutorial, you should follow these recommendations:

- Do not use your production environment, unless it is necessary.
- If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).

## Scenario description
In this tutorial, you test Azure AD single sign-on in a test environment. 
The scenario outlined in this tutorial consists of two main building blocks:

1. Adding Pantheon from the gallery
2. Configuring and testing Azure AD single sign-on

## Adding Pantheon from the gallery
To configure the integration of Pantheon into Azure AD, you need to add Pantheon from the gallery to your list of managed SaaS apps.

**To add Pantheon from the gallery, perform the following steps:**

1. In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon. 

	![Active Directory][1]

2. Navigate to **Enterprise applications**. Then go to **All applications**.

	![Applications][2]
	
3. To add new application, click **New application** button on the top of dialog.

	![Applications][3]

4. In the search box, type **Pantheon**.

	![Creating an Azure AD test user](./media/pantheon-tutorial/tutorial_pantheon_search.png)

5. In the results panel, select **Pantheon**, and then click **Add** button to add the application.

	![Creating an Azure AD test user](./media/pantheon-tutorial/tutorial_pantheon_addfromgallery.png)

##  Configuring and testing Azure AD single sign-on
In this section, you configure and test Azure AD single sign-on with Pantheon based on a test user called "Britta Simon".

For single sign-on to work, Azure AD needs to know what the counterpart user in Pantheon is to a user in Azure AD. In other words, a link relationship between an Azure AD user and the related user in Pantheon needs to be established.

In Pantheon, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.

To configure and test Azure AD single sign-on with Pantheon, you need to complete the following building blocks:

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.
2. **[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.
3. **[Creating a Pantheon test user](#creating-a-pantheon-test-user)** - to have a counterpart of Britta Simon in Pantheon that is linked to the Azure AD representation of user.
4. **[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.
5. **[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.

### Configuring Azure AD single sign-on

In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Pantheon application.

**To configure Azure AD single sign-on with Pantheon, perform the following steps:**

1. In the Azure portal, on the **Pantheon** application integration page, click **Single sign-on**.

	![Configure Single Sign-On][4]

2. On the **Single sign-on** dialog, select **Mode** as	**SAML-based Sign-on** to enable single sign-on.
 
	![Configure Single Sign-On](./media/pantheon-tutorial/tutorial_pantheon_samlbase.png)

3. On the **Pantheon Domain and URLs** section, perform the following steps:

	![Configure Single Sign-On](./media/pantheon-tutorial/tutorial_pantheon_url.png)

    a. In the **Identifier** textbox, type a URL using the following pattern: `urn:auth0:pantheon:<orgname>-SSO`

	b. In the **Reply URL** textbox, type a URL using the following pattern: `https://pantheon.auth0.com/login/callback?connection=<orgname>-SSO`

	> [!NOTE] 
	> These values are not real. Update these values with the actual Identifier and Reply URL. Contact [Pantheon support team](https://pantheon.io/docs/getting-support/) to get these values.

4. Pantheon application expects the SAML assertion in specific format, which requires you to set the UserIdentifier attribute value with the user’s email address. By default Azure AD uses the UserPrincipalName for UserIdentifier attribute. But for successful integration you need to adjust this value to match with user’s email address. The integration will only work after doing the correct mapping.

	![Configure Single Sign-On](./media/pantheon-tutorial/tutorial_attribute.png)	 


5. On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.

	![Configure Single Sign-On](./media/pantheon-tutorial/tutorial_pantheon_certificate.png)

6. Click **Save** button.

	![Configure Single Sign-On](./media/pantheon-tutorial/tutorial_general_400.png)

7. On the **Pantheon Configuration** section, click **Configure Pantheon** to open **Configure sign-on** window. Copy the **SAML Single Sign-On Service URL** from the **Quick Reference section.**

	![Configure Single Sign-On](./media/pantheon-tutorial/tutorial_pantheon_configure.png) 

8. To configure single sign-on on **Pantheon** side, you need to send the downloaded **Certificate** and **SAML Single Sign-On Service URL** to [Pantheon support team](https://pantheon.io/docs/getting-support/).

     > [!Note]
     > You also need to provide the Email Domain(s) information and Date Time when you want to enable this connection. You can find more details about it from [here](https://pantheon.io/docs/sso-organizations/)

> [!TIP]
> You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!  After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom. You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)

### Creating an Azure AD test user
The objective of this section is to create a test user in the Azure portal called Britta Simon.

![Create Azure AD User][100]

**To create a test user in Azure AD, perform the following steps:**

1. In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.

	![Creating an Azure AD test user](./media/pantheon-tutorial/create_aaduser_01.png) 

2. To display the list of users, go to **Users and groups** and click **All users**.
	
	![Creating an Azure AD test user](./media/pantheon-tutorial/create_aaduser_02.png) 

3. To open the **User** dialog, click **Add** on the top of the dialog.
 
	![Creating an Azure AD test user](./media/pantheon-tutorial/create_aaduser_03.png) 

4. On the **User** dialog page, perform the following steps:
 
	![Creating an Azure AD test user](./media/pantheon-tutorial/create_aaduser_04.png) 

    a. In the **Name** textbox, type **BrittaSimon**.

    b. In the **User name** textbox, type the **email address** of BrittaSimon.

	c. Select **Show Password** and write down the value of the **Password**.

    d. Click **Create**.
 
### Creating a Pantheon test user

In this section, you create a user called Britta Simon in Pantheon. Please follow the below steps to add the user in Pantheon. 

>[!NOTE] 
>For SSO to work user needs to be created first in Pantheon.

1. Login to Pantheon with admin credentials.

2. Navigate to **Organization** dashboard page.
 
3. Click **People**.

4. Click **Add user**.

5. Enter the user's email address.

6. Choose the user's role.

7. Click **Add user**.

### Assigning the Azure AD test user

In this section, you enable Britta Simon to use Azure single sign-on by granting access to Pantheon.

![Assign User][200] 

**To assign Britta Simon to Pantheon, perform the following steps:**

1. In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.

	![Assign User][201] 

2. In the applications list, select **Pantheon**.

	![Configure Single Sign-On](./media/pantheon-tutorial/tutorial_pantheon_app.png) 

3. In the menu on the left, click **Users and groups**.

	![Assign User][202] 

4. Click **Add** button. Then select **Users and groups** on **Add Assignment** dialog.

	![Assign User][203]

5. On **Users and groups** dialog, select **Britta Simon** in the Users list.

6. Click **Select** button on **Users and groups** dialog.

7. Click **Assign** button on **Add Assignment** dialog.
	
### Testing single sign-on

In this section, you test your Azure AD single sign-on configuration using the Access Panel.

When you click the Pantheon tile in the Access Panel, you should get automatically signed-on to your Pantheon application.
For more information about the Access Panel, see [Introduction to the Access Panel](../active-directory-saas-access-panel-introduction.md). 

## Additional resources

* [List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/pantheon-tutorial/tutorial_general_01.png
[2]: ./media/pantheon-tutorial/tutorial_general_02.png
[3]: ./media/pantheon-tutorial/tutorial_general_03.png
[4]: ./media/pantheon-tutorial/tutorial_general_04.png

[100]: ./media/pantheon-tutorial/tutorial_general_100.png

[200]: ./media/pantheon-tutorial/tutorial_general_200.png
[201]: ./media/pantheon-tutorial/tutorial_general_201.png
[202]: ./media/pantheon-tutorial/tutorial_general_202.png
[203]: ./media/pantheon-tutorial/tutorial_general_203.png

