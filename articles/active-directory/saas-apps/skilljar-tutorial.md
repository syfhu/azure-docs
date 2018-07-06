---
title: 'Tutorial: Azure Active Directory integration with Skilljar | Microsoft Docs'
description: Learn how to configure single sign-on between Azure Active Directory and Skilljar.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman

ms.assetid: c572f556-98a3-48e6-8e4c-e634b7a2ba70
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/10/2017
ms.author: jeedes

---
# Tutorial: Azure Active Directory integration with Skilljar

In this tutorial, you learn how to integrate Skilljar with Azure Active Directory (Azure AD).

Integrating Skilljar with Azure AD provides you with the following benefits:

- You can control in Azure AD who has access to Skilljar
- You can enable your users to automatically get signed-on to Skilljar (Single Sign-On) with their Azure AD accounts
- You can manage your accounts in one central location - the Azure portal

If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## Prerequisites

To configure Azure AD integration with Skilljar, you need the following items:

- An Azure AD subscription
- A Skilljar single sign-on enabled subscription

> [!NOTE]
> To test the steps in this tutorial, we do not recommend using a production environment.

To test the steps in this tutorial, you should follow these recommendations:

- Do not use your production environment, unless it is necessary.
- If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).

## Scenario description
In this tutorial, you test Azure AD single sign-on in a test environment. 
The scenario outlined in this tutorial consists of two main building blocks:

1. Adding Skilljar from the gallery
2. Configuring and testing Azure AD single sign-on

## Adding Skilljar from the gallery
To configure the integration of Skilljar into Azure AD, you need to add Skilljar from the gallery to your list of managed SaaS apps.

**To add Skilljar from the gallery, perform the following steps:**

1. In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon. 

	![Active Directory][1]

2. Navigate to **Enterprise applications**. Then go to **All applications**.

	![Applications][2]
	
3. To add new application, click **New application** button on the top of dialog.

	![Applications][3]

4. In the search box, type **Skilljar**.

	![Creating an Azure AD test user](./media/skilljar-tutorial/tutorial_skilljar_search.png)

5. In the results panel, select **Skilljar**, and then click **Add** button to add the application.

	![Creating an Azure AD test user](./media/skilljar-tutorial/tutorial_skilljar_addfromgallery.png)

##  Configuring and testing Azure AD single sign-on
In this section, you configure and test Azure AD single sign-on with Skilljar based on a test user called "Britta Simon".

For single sign-on to work, Azure AD needs to know what the counterpart user in Skilljar is to a user in Azure AD. In other words, a link relationship between an Azure AD user and the related user in Skilljar needs to be established.

In Skilljar, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.

To configure and test Azure AD single sign-on with Skilljar, you need to complete the following building blocks:

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.
2. **[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.
3. **[Creating a Skilljar test user](#creating-a-skilljar-test-user)** - to have a counterpart of Britta Simon in Skilljar that is linked to the Azure AD representation of user.
4. **[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.
5. **[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.

### Configuring Azure AD single sign-on

In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Skilljar application.

**To configure Azure AD single sign-on with Skilljar, perform the following steps:**

1. In the Azure portal, on the **Skilljar** application integration page, click **Single sign-on**.

	![Configure Single Sign-On][4]

2. On the **Single sign-on** dialog, select **Mode** as	**SAML-based Sign-on** to enable single sign-on.
 
	![Configure Single Sign-On](./media/skilljar-tutorial/tutorial_skilljar_samlbase.png)

3. On the **Skilljar Domain and URLs** section, perform the following steps:

	![Configure Single Sign-On](./media/skilljar-tutorial/tutorial_skilljar_url.png)

    a. In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<companyname>.skilljar.com/`

	b. In the **Identifier** textbox, type a URL using the following pattern: `https://<companyname>.skilljar.com/`

	> [!NOTE] 
	> These values are not real. Update these values with the actual Sign-On URL and Identifier. Contact [Skilljar Client support team](http://support.skilljar.com/hc/) to get these values. 
 
4. On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.

	![Configure Single Sign-On](./media/skilljar-tutorial/tutorial_skilljar_certificate.png) 

5. Click **Save** button.

	![Configure Single Sign-On](./media/skilljar-tutorial/tutorial_general_400.png)

6. To configure single sign-on on **Skilljar** side, you need to send the downloaded **Metadata XML**, and **Name Identifier Format Value - urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress** to [Skilljar support team](http://support.skilljar.com/hc/). They set this setting to have the SAML SSO connection set properly on both sides.

> [!TIP]
> You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!  After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom. You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### Creating an Azure AD test user
The objective of this section is to create a test user in the Azure portal called Britta Simon.

![Create Azure AD User][100]

**To create a test user in Azure AD, perform the following steps:**

1. In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.

	![Creating an Azure AD test user](./media/skilljar-tutorial/create_aaduser_01.png) 

2. To display the list of users, go to **Users and groups** and click **All users**.
	
	![Creating an Azure AD test user](./media/skilljar-tutorial/create_aaduser_02.png) 

3. To open the **User** dialog, click **Add** on the top of the dialog.
 
	![Creating an Azure AD test user](./media/skilljar-tutorial/create_aaduser_03.png) 

4. On the **User** dialog page, perform the following steps:
 
	![Creating an Azure AD test user](./media/skilljar-tutorial/create_aaduser_04.png) 

    a. In the **Name** textbox, type **BrittaSimon**.

    b. In the **User name** textbox, type the **email address** of BrittaSimon.

	c. Select **Show Password** and write down the value of the **Password**.

    d. Click **Create**.
 
### Creating a Skilljar test user

The objective of this section is to create a user called Britta Simon in Skilljar. Skilljar supports just-in-time provisioning, which is by default enabled.

There is no action item for you in this section. A new user is created during an attempt to access Skilljar if it doesn't exist yet. 

>[!NOTE]
>If you need to create a user manually, you need to contact the [Skilljar support team](http://support.skilljar.com/hc/). 
> 

### Assigning the Azure AD test user

In this section, you enable Britta Simon to use Azure single sign-on by granting access to Skilljar.

![Assign User][200] 

**To assign Britta Simon to Skilljar, perform the following steps:**

1. In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.

	![Assign User][201] 

2. In the applications list, select **Skilljar**.

	![Configure Single Sign-On](./media/skilljar-tutorial/tutorial_skilljar_app.png) 

3. In the menu on the left, click **Users and groups**.

	![Assign User][202] 

4. Click **Add** button. Then select **Users and groups** on **Add Assignment** dialog.

	![Assign User][203]

5. On **Users and groups** dialog, select **Britta Simon** in the Users list.

6. Click **Select** button on **Users and groups** dialog.

7. Click **Assign** button on **Add Assignment** dialog.
	
### Testing single sign-on

The objective of this section is to test your Azure AD SSO configuration using the Access Panel.  

When you click the Skilljar tile in the Access Panel, you should get automatically signed-on to your Skilljar application.

## Additional resources

* [List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/skilljar-tutorial/tutorial_general_01.png
[2]: ./media/skilljar-tutorial/tutorial_general_02.png
[3]: ./media/skilljar-tutorial/tutorial_general_03.png
[4]: ./media/skilljar-tutorial/tutorial_general_04.png

[100]: ./media/skilljar-tutorial/tutorial_general_100.png

[200]: ./media/skilljar-tutorial/tutorial_general_200.png
[201]: ./media/skilljar-tutorial/tutorial_general_201.png
[202]: ./media/skilljar-tutorial/tutorial_general_202.png
[203]: ./media/skilljar-tutorial/tutorial_general_203.png

