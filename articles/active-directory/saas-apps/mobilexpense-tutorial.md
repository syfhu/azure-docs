---
title: 'Tutorial: Azure Active Directory integration with Mobile Xpense | Microsoft Docs'
description: Learn how to configure single sign-on between Azure Active Directory and Mobile Xpense.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore

ms.assetid: e649fc4e-3e15-4948-b977-00bfe9f7db13
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/05/2018
ms.author: jeedes

---
# Tutorial: Azure Active Directory integration with Mobile Xpense

In this tutorial, you learn how to integrate Mobile Xpense with Azure Active Directory (Azure AD).

Integrating Mobile Xpense with Azure AD provides you with the following benefits:

- You can control in Azure AD who has access to Mobile Xpense.
- You can enable your users to automatically get signed-on to Mobile Xpense (Single Sign-On) with their Azure AD accounts.
- You can manage your accounts in one central location - the Azure portal.

If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## Prerequisites

To configure Azure AD integration with Mobile Xpense, you need the following items:

- An Azure AD subscription
- A Mobile Xpense single-sign on enabled subscription

> [!NOTE]
> To test the steps in this tutorial, we do not recommend using a production environment.

To test the steps in this tutorial, you should follow these recommendations:

- Do not use your production environment, unless it is necessary.
- If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).

## Scenario description
In this tutorial, you test Azure AD single sign-on in a test environment. 
The scenario outlined in this tutorial consists of two main building blocks:

1. Adding Mobile Xpense from the gallery
2. Configuring and testing Azure AD single sign-on

## Adding Mobile Xpense from the gallery
To configure the integration of Mobile Xpense into Azure AD, you need to add Mobile Xpense from the gallery to your list of managed SaaS apps.

**To add Mobile Xpense from the gallery, perform the following steps:**

1. In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon. 

	![The Azure Active Directory button][1]

2. Navigate to **Enterprise applications**. Then go to **All applications**.

	![The Enterprise applications blade][2]
	
3. To add new application, click **New application** button on the top of dialog.

	![The New application button][3]

4. In the search box, type **Mobile Xpense**, select **Mobile Xpense** from result panel then click **Add** button to add the application.

	![Mobile Xpense in the results list](./media/mobilexpense-tutorial/tutorial_mobilexpense_addfromgallery.png)

## Configure and test Azure AD single sign-on

In this section, you configure and test Azure AD single sign-on with Mobile Xpense based on a test user called "Britta Simon".

For single sign-on to work, Azure AD needs to know what the counterpart user in Mobile Xpense is to a user in Azure AD. In other words, a link relationship between an Azure AD user and the related user in Mobile Xpense needs to be established.

In Mobile Xpense, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.

To configure and test Azure AD single sign-on with Mobile Xpense, you need to complete the following building blocks:

1. **[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.
2. **[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.
3. **[Create a Mobile Xpense test user](#create-a-mobile-xpense-test-user)** - to have a counterpart of Britta Simon in Mobile Xpense that is linked to the Azure AD representation of user.
4. **[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.
5. **[Test single sign-on](#test-single-sign-on)** - to verify whether the configuration works.

### Configure Azure AD single sign-on

In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Mobile Xpense application.

**To configure Azure AD single sign-on with Mobile Xpense, perform the following steps:**

1. In the Azure portal, on the **Mobile Xpense** application integration page, click **Single sign-on**.

	![Configure single sign-on link][4]

2. On the **Single sign-on** dialog, select **Mode** as	**SAML-based Sign-on** to enable single sign-on.
 
	![Single sign-on dialog box](./media/mobilexpense-tutorial/tutorial_mobilexpense_samlbase.png)

3. On the **Mobile Xpense Domain and URLs** section, perform the following steps if you wish to configure the application in IDP initiated mode:

	![Mobile Xpense Domain and URLs single sign-on information](./media/mobilexpense-tutorial/tutorial_mobilexpense_url11.png)

    a. In the **Identifier** textbox, type a URL: `https://mobilexpense.com/ServiceProvider`

	b. In the **Reply URL** textbox, type a URL using the following pattern: `https://<sub-domain>.mobilexpense.com/NET/SSO/SAML20/SAML/AssertionConsumerService.aspx`

4. Check **Show advanced URL settings** and perform the following step if you wish to configure the application in **SP** initiated mode:

	![Mobile Xpense Domain and URLs single sign-on information](./media/mobilexpense-tutorial/tutorial_mobilexpense_url22.png)

    In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<sub-domain>.mobilexpense.com/<customername>`
	 
	> [!NOTE] 
	> These values are not real. Update these values with the actual Reply URL, and Sign-On URL. Contact [Mobile Xpense Client support team](http://www.mobilexpense.net/contact) to get these values. 

5. On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.

	![The Certificate download link](./media/mobilexpense-tutorial/tutorial_mobilexpense_certificate.png) 

6. Click **Save** button.

	![Configure Single Sign-On Save button](./media/mobilexpense-tutorial/tutorial_general_400.png)

7. To configure single sign-on on **Mobile Xpense** side, you need to send the downloaded **Metadata XML** to [Mobile Xpense support team](http://www.mobilexpense.net/contact). They set this setting to have the SAML SSO connection set properly on both sides.

> [!TIP]
> You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!  After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom. You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)

### Create an Azure AD test user

The objective of this section is to create a test user in the Azure portal called Britta Simon.

   ![Create an Azure AD test user][100]

**To create a test user in Azure AD, perform the following steps:**

1. In the Azure portal, in the left pane, click the **Azure Active Directory** button.

    ![The Azure Active Directory button](./media/mobilexpense-tutorial/create_aaduser_01.png)

2. To display the list of users, go to **Users and groups**, and then click **All users**.

    ![The "Users and groups" and "All users" links](./media/mobilexpense-tutorial/create_aaduser_02.png)

3. To open the **User** dialog box, click **Add** at the top of the **All Users** dialog box.

    ![The Add button](./media/mobilexpense-tutorial/create_aaduser_03.png)

4. In the **User** dialog box, perform the following steps:

    ![The User dialog box](./media/mobilexpense-tutorial/create_aaduser_04.png)

    a. In the **Name** box, type **BrittaSimon**.

    b. In the **User name** box, type the email address of user Britta Simon.

    c. Select the **Show Password** check box, and then write down the value that's displayed in the **Password** box.

    d. Click **Create**.
 
### Create a Mobile Xpense test user

In this section, you create a user called Britta Simon in MobileXpense. work with [MobileXpense support team](http://www.mobilexpense.net/contact) to add the users in the MobileXpense platform. Users must be created and activated before you use single sign-on. 

### Assign the Azure AD test user

In this section, you enable Britta Simon to use Azure single sign-on by granting access to Mobile Xpense.

![Assign the user role][200] 

**To assign Britta Simon to Mobile Xpense, perform the following steps:**

1. In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.

	![Assign User][201] 

2. In the applications list, select **Mobile Xpense**.

	![The Mobile Xpense link in the Applications list](./media/mobilexpense-tutorial/tutorial_mobilexpense_app.png)  

3. In the menu on the left, click **Users and groups**.

	![The "Users and groups" link][202]

4. Click **Add** button. Then select **Users and groups** on **Add Assignment** dialog.

	![The Add Assignment pane][203]

5. On **Users and groups** dialog, select **Britta Simon** in the Users list.

6. Click **Select** button on **Users and groups** dialog.

7. Click **Assign** button on **Add Assignment** dialog.
	
### Test single sign-on

In this section, you test your Azure AD single sign-on configuration using the Access Panel.

When you click the Mobile Xpense tile in the Access Panel, you should get automatically signed-on to your Mobile Xpense application.
For more information about the Access Panel, see [Introduction to the Access Panel](../active-directory-saas-access-panel-introduction.md). 

## Additional resources

* [List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/mobilexpense-tutorial/tutorial_general_01.png
[2]: ./media/mobilexpense-tutorial/tutorial_general_02.png
[3]: ./media/mobilexpense-tutorial/tutorial_general_03.png
[4]: ./media/mobilexpense-tutorial/tutorial_general_04.png

[100]: ./media/mobilexpense-tutorial/tutorial_general_100.png

[200]: ./media/mobilexpense-tutorial/tutorial_general_200.png
[201]: ./media/mobilexpense-tutorial/tutorial_general_201.png
[202]: ./media/mobilexpense-tutorial/tutorial_general_202.png
[203]: ./media/mobilexpense-tutorial/tutorial_general_203.png

