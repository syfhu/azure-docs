---
title: 'Tutorial: Azure Active Directory integration with ClickTime | Microsoft Docs'
description: Learn how to configure single sign-on between Azure Active Directory and ClickTime.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore

ms.assetid: d437b5ab-4d71-4c13-96d0-79018cebbbd4
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/10/2017
ms.author: jeedes

---
# Tutorial: Azure Active Directory integration with ClickTime

In this tutorial, you learn how to integrate ClickTime with Azure Active Directory (Azure AD).

Integrating ClickTime with Azure AD provides you with the following benefits:

- You can control in Azure AD who has access to ClickTime
- You can enable your users to automatically get signed-on to ClickTime (Single Sign-On) with their Azure AD accounts
- You can manage your accounts in one central location - the Azure portal

If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## Prerequisites

To configure Azure AD integration with ClickTime, you need the following items:

- An Azure AD subscription
- A ClickTime single sign-on enabled subscription

> [!NOTE]
> To test the steps in this tutorial, we do not recommend using a production environment.

To test the steps in this tutorial, you should follow these recommendations:

- Do not use your production environment, unless it is necessary.
- If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).

## Scenario description
In this tutorial, you test Azure AD single sign-on in a test environment. 
The scenario outlined in this tutorial consists of two main building blocks:

1. Adding ClickTime from the gallery
2. Configuring and testing Azure AD single sign-on

## Adding ClickTime from the gallery
To configure the integration of ClickTime into Azure AD, you need to add ClickTime from the gallery to your list of managed SaaS apps.

**To add ClickTime from the gallery, perform the following steps:**

1. In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon. 

	![The Azure Active Directory button][1]

2. Navigate to **Enterprise applications**. Then go to **All applications**.

	![The Enterprise applications blade][2]
	
3. To add new application, click **New application** button on the top of dialog.

	![The New application button][3]

4. In the search box, type **ClickTime**, select **ClickTime** from result panel then click **Add** button to add the application.

	![ClickTime in the results list](./media/clicktime-tutorial/tutorial_clicktime_addfromgallery.png)

## Configure and test Azure AD single sign-on

In this section, you configure and test Azure AD single sign-on with ClickTime based on a test user called "Britta Simon".

For single sign-on to work, Azure AD needs to know what the counterpart user in ClickTime is to a user in Azure AD. In other words, a link relationship between an Azure AD user and the related user in ClickTime needs to be established.

In ClickTime, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.

To configure and test Azure AD single sign-on with ClickTime, you need to complete the following building blocks:

1. **[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.
2. **[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.
3. **[Create a ClickTime test user](#create-a-clicktime-test-user)** - to have a counterpart of Britta Simon in ClickTime that is linked to the Azure AD representation of user.
4. **[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.
5. **[Test single sign-on](#test-single-sign-on)** - to verify whether the configuration works.

### Configure Azure AD single sign-on

In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your ClickTime application.

**To configure Azure AD single sign-on with ClickTime, perform the following steps:**

1. In the Azure portal, on the **ClickTime** application integration page, click **Single sign-on**.

	![Configure single sign-on link][4]

2. On the **Single sign-on** dialog, select **Mode** as	**SAML-based Sign-on** to enable single sign-on.
 
	![Single sign-on dialog box](./media/clicktime-tutorial/tutorial_clicktime_samlbase.png)

3. On the **ClickTime Domain and URLs** section, perform the following steps:

	![ClickTime Domain and URLs single sign-on information](./media/clicktime-tutorial/tutorial_clicktime_url.png)

    a. In the **Identifier** textbox, type a URL as: `https://app.clicktime.com/sp/`
	
	b. In the **Reply URL** textbox, type a URL using the following patterns: 

	| |
	|--|
	| `https://app.clicktime.com/Login/` |
	| `https://app.clicktime.com/App/Login/Consume.aspx` |

4. On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.

	![The Certificate download link](./media/clicktime-tutorial/tutorial_clicktime_certificate.png) 

5. Click **Save** button.

	![Configure Single Sign-On Save button](./media/clicktime-tutorial/tutorial_general_400.png)

6. On the **ClickTime Configuration** section, click **Configure ClickTime** to open **Configure sign-on** window. Copy the **SAML Single Sign-On Service URL** from the **Quick Reference section.**

	![ClickTime configuration](./media/clicktime-tutorial/tutorial_clicktime_configure.png) 

7. In a different web browser window, log into your ClickTime company site as an administrator.

8. In the toolbar on the top, click **Preferences**, and then click **Security Settings**.

9. In the **Single Sign-On Preferences** configuration section, perform the following steps:
   
    ![Security Settings](./media/clicktime-tutorial/tic777280.png "Security Settings")
   
    a.  Select **Allow** sign-in using Single Sign-On (SSO) with **Azure AD**.
   
    b. In the **Identity Provider Endpoint** textbox, paste **SAML Single Sign-On Service URL** which you have copied from Azure portal.
   
    c.  Open the **base-64 encoded certificate** downloaded from Azure portal in **Notepad**, copy the content, and then paste it into the **X.509 Certificate** textbox.
   
    d.  Click **Save**.

> [!TIP]
> You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!  After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom. You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)

### Create an Azure AD test user
The objective of this section is to create a test user in the Azure portal called Britta Simon.

![Create an Azure AD test user][100]

**To create a test user in Azure AD, perform the following steps:**

1. In the Azure portal, in the left pane, click the **Azure Active Directory** button.

	![The Azure Active Directory button](./media/clicktime-tutorial/create_aaduser_01.png) 

2. To display the list of users, go to **Users and groups**, and then click **All users**.
	
	![The "Users and groups" and "All users" links](./media/clicktime-tutorial/create_aaduser_02.png) 

3. To open the **User** dialog box, click **Add** at the top of the **All Users** dialog box.
 
	![The Add button](./media/clicktime-tutorial/create_aaduser_03.png) 

4. In the **User** dialog box, perform the following steps:
 
	![The User dialog box](./media/clicktime-tutorial/create_aaduser_04.png) 

    a. In the **Name** textbox, type **BrittaSimon**.

    b. In the **User name** textbox, type the **email address** of BrittaSimon.

	c. Select **Show Password** and write down the value of the **Password**.

    d. Click **Create**.
 
### Create a ClickTime test user

In order to enable Azure AD users to log into ClickTime, they must be provisioned into ClickTime.  
In the case of ClickTime, provisioning is a manual task.

> [!NOTE]
> You can use any other ClickTime user account creation tools or APIs provided by ClickTime to provision Azure AD user accounts.

**To provision a user account, perform the following steps:**
1. Log in to your **ClickTime** tenant.
2. In the toolbar on the top, click **Company**, and then click **People**.
   
    ![People](./media/clicktime-tutorial/tic777282.png "People")
3. Click **Add Person**.
   
    ![Add Person](./media/clicktime-tutorial/tic777283.png "Add Person")
4. In the New Person section, perform the following steps:
   
    ![People](./media/clicktime-tutorial/tic777284.png "People")
   
    a.  In the **full name** textbox, type full name of user like **Britta Simon**. 
  
    b.  In the **email address** textbox, type the email of user like **brittasimon@contoso.com**.
       
    > [!NOTE]
    > If you want to, you can set additional properties of the new person object.
   
    c.  Click **Save**.

### Assign the Azure AD test user

In this section, you enable Britta Simon to use Azure single sign-on by granting access to ClickTime.

![Assign the user role][200] 

**To assign Britta Simon to ClickTime, perform the following steps:**

1. In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.

	![Assign User][201] 

2. In the applications list, select **ClickTime**.

	![ClickTimne link in the Applications list](./media/clicktime-tutorial/tutorial_clicktime_app.png) 

3. In the menu on the left, click **Users and groups**.

	![The "Users and groups" link][202] 

4. Click **Add** button. Then select **Users and groups** on **Add Assignment** dialog.

	![The Add Assignment pane][203]

5. On **Users and groups** dialog, select **Britta Simon** in the Users list.

6. Click **Select** button on **Users and groups** dialog.

7. Click **Assign** button on **Add Assignment** dialog.
	
### Test single sign-on

In this section, you test your Azure AD single sign-on configuration using the Access Panel.

When you click the ClickTime tile in the Access Panel, you should get automatically signed-on to your ClickTime application.
For more information about the Access Panel, see [Introduction to the Access Panel](../active-directory-saas-access-panel-introduction.md).

## Additional resources

* [List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/clicktime-tutorial/tutorial_general_01.png
[2]: ./media/clicktime-tutorial/tutorial_general_02.png
[3]: ./media/clicktime-tutorial/tutorial_general_03.png
[4]: ./media/clicktime-tutorial/tutorial_general_04.png

[100]: ./media/clicktime-tutorial/tutorial_general_100.png

[200]: ./media/clicktime-tutorial/tutorial_general_200.png
[201]: ./media/clicktime-tutorial/tutorial_general_201.png
[202]: ./media/clicktime-tutorial/tutorial_general_202.png
[203]: ./media/clicktime-tutorial/tutorial_general_203.png

