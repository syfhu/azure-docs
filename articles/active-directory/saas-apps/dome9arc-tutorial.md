---
title: 'Tutorial: Azure Active Directory integration with Dome9 Arc | Microsoft Docs'
description: Learn how to configure single sign-on between Azure Active Directory and Dome9 Arc.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore

ms.assetid: 4c12875f-de71-40cb-b9ac-216a805334e5
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/18/2018
ms.author: jeedes

---
# Tutorial: Azure Active Directory integration with Dome9 Arc

In this tutorial, you learn how to integrate Dome9 Arc with Azure Active Directory (Azure AD).

Integrating Dome9 Arc with Azure AD provides you with the following benefits:

- You can control in Azure AD who has access to Dome9 Arc.
- You can enable your users to automatically get signed-on to Dome9 Arc (Single Sign-On) with their Azure AD accounts.
- You can manage your accounts in one central location - the Azure portal.

If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## Prerequisites

To configure Azure AD integration with Dome9 Arc, you need the following items:

- An Azure AD subscription
- A Dome9 Arc single sign-on enabled subscription

> [!NOTE]
> To test the steps in this tutorial, we do not recommend using a production environment.

To test the steps in this tutorial, you should follow these recommendations:

- Do not use your production environment, unless it is necessary.
- If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).

## Scenario description
In this tutorial, you test Azure AD single sign-on in a test environment. 
The scenario outlined in this tutorial consists of two main building blocks:

1. Adding Dome9 Arc from the gallery
2. Configuring and testing Azure AD single sign-on

## Adding Dome9 Arc from the gallery
To configure the integration of Dome9 Arc into Azure AD, you need to add Dome9 Arc from the gallery to your list of managed SaaS apps.

**To add Dome9 Arc from the gallery, perform the following steps:**

1. In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon. 

	![The Azure Active Directory button][1]

2. Navigate to **Enterprise applications**. Then go to **All applications**.

	![The Enterprise applications blade][2]
	
3. To add new application, click **New application** button on the top of dialog.

	![The New application button][3]

4. In the search box, type **Dome9 Arc**, select **Dome9 Arc** from result panel then click **Add** button to add the application.

	![Dome9 Arc in the results list](./media/dome9arc-tutorial/tutorial_dome9arc_addfromgallery.png)

## Configure and test Azure AD single sign-on

In this section, you configure and test Azure AD single sign-on with Dome9 Arc based on a test user called "Britta Simon".

For single sign-on to work, Azure AD needs to know what the counterpart user in Dome9 Arc is to a user in Azure AD. In other words, a link relationship between an Azure AD user and the related user in Dome9 Arc needs to be established.

In Dome9 Arc, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.

To configure and test Azure AD single sign-on with Dome9 Arc, you need to complete the following building blocks:

1. **[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.
2. **[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.
3. **[Create a Dome9 Arc test user](#create-a-dome9-arc-test-user)** - to have a counterpart of Britta Simon in Dome9 Arc that is linked to the Azure AD representation of user.
4. **[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.
5. **[Test single sign-on](#test-single-sign-on)** - to verify whether the configuration works.

### Configure Azure AD single sign-on

In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Dome9 Arc application.

**To configure Azure AD single sign-on with Dome9 Arc, perform the following steps:**

1. In the Azure portal, on the **Dome9 Arc** application integration page, click **Single sign-on**.

	![Configure single sign-on link][4]

2. On the **Single sign-on** dialog, select **Mode** as	**SAML-based Sign-on** to enable single sign-on.
 
	![Single sign-on dialog box](./media/dome9arc-tutorial/tutorial_dome9arc_samlbase.png)

3. On the **Dome9 Arc Domain and URLs** section, perform the following steps if you wish to configure the application in **IDP** initiated mode:

	![Dome9 Arc Domain and URLs single sign-on information](./media/dome9arc-tutorial/tutorial_dome9arc_url.png)

    a. In the **Identifier** textbox, type the URL: `https://secure.dome9.com/`

	b. In the **Reply URL** textbox, type a URL using the following pattern: `https://secure.dome9.com/sso/saml/yourcompanyname`

	> [!NOTE]
	> You will select your company name value in the dome9 admin portal, which is explained later in the tutorial.

4. Check **Show advanced URL settings** and perform the following step if you wish to configure the application in **SP** initiated mode:

	![Dome9 Arc Domain and URLs single sign-on information](./media/dome9arc-tutorial/tutorial_dome9arc_url1.png)

    In the **Sign-on URL** textbox, type a URL using the following pattern: `https://secure.dome9.com/sso/saml/<yourcompanyname>`
	 
	> [!NOTE] 
	> These values are not real. Update these values with the actual Reply URL and Sign-On URL. Contact [Dome9 Arc Client support team](https://dome9.com/about/contact-us/) to get these values. 

5. The Dome9 Arc Software application expects the SAML assertions in a specific format. Configure the following claims for this application. You can manage the values of these attributes from the "**User Attributes**" section on application integration page. The following screenshot shows an example for this.

	![Configure Single Sign-On attb](./media/dome9arc-tutorial/tutorial_dome9arc_attribute.png)

6. In the **User Attributes** section on the **Single sign-on** dialog, configure SAML token attribute as shown in the image above and perform the following steps:
	
	| Attribute Name  | Attribute Value | 
	| --------------- | --------------- | 
	| memberof | user.assignedroles | 
	
	a. Click **Add attribute** to open the **Add Attribute** dialog.

	![Configure Single Sign-On add attb](./media/dome9arc-tutorial/tutorial_dome9_04.png)

	![Configure Single Sign-On edit attb](./media/dome9arc-tutorial/tutorial_attribute_05.png)

	b. In the **Name** textbox, type the attribute name shown for that row.

	c. From the **Value** list, type the attribute value shown for that row.
	
	d. Click **Ok**.

7. On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.

	![The Certificate download link](./media/dome9arc-tutorial/tutorial_dome9arc_certificate.png) 

8. Click **Save** button.

	![Configure Single Sign-On Save button](./media/dome9arc-tutorial/tutorial_general_400.png)
	
9. On the **Dome9 Arc Configuration** section, click **Configure Dome9 Arc** to open **Configure sign-on** window. Copy the **SAML Entity ID and SAML Single Sign-On Service URL** from the **Quick Reference section.**

	![Dome9 Arc Configuration](./media/dome9arc-tutorial/tutorial_dome9arc_configure.png) 

10. In a different web browser window, log into your Dome9 Arc company site as an administrator.

11. Click on the **Profile Settings** on the right top corner and then click **Account Settings**. 

	![Dome9 Arc Configuration](./media/dome9arc-tutorial/configure1.png)

12. Navigate to **SSO** and then click **ENABLE**.

	![Dome9 Arc Configuration](./media/dome9arc-tutorial/configure2.png)

13. In the SSO Configuration section, perform the following steps:

	![Dome9 Arc Configuration](./media/dome9arc-tutorial/configure3.png)

	a. Enter company name in the **Account ID** textbox. This value is to be used in the reply url mentioned in the Azure portal URL section.

	b. In the **Issuer** textbox, paste the value of **SAML Entity ID**, which you have copied form the Azure portal.

	c. In the **Idp endpoint url** textbox, paste the value of **SAML Single Sign-On Service URL**, which you have copied form the Azure portal.

	d. Open your downloaded Base64 encoded certificate in notepad, copy the content of it into your clipboard, and then paste it to the **X.509 certificate** textbox.

	e. Click **Save**.

> [!TIP]
> You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!  After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom. You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)

### Create an Azure AD test user

The objective of this section is to create a test user in the Azure portal called Britta Simon.

   ![Create an Azure AD test user][100]

**To create a test user in Azure AD, perform the following steps:**

1. In the Azure portal, in the left pane, click the **Azure Active Directory** button.

    ![The Azure Active Directory button](./media/dome9arc-tutorial/create_aaduser_01.png)

2. To display the list of users, go to **Users and groups**, and then click **All users**.

    ![The "Users and groups" and "All users" links](./media/dome9arc-tutorial/create_aaduser_02.png)

3. To open the **User** dialog box, click **Add** at the top of the **All Users** dialog box.

    ![The Add button](./media/dome9arc-tutorial/create_aaduser_03.png)

4. In the **User** dialog box, perform the following steps:

    ![The User dialog box](./media/dome9arc-tutorial/create_aaduser_04.png)

    a. In the **Name** box, type **BrittaSimon**.

    b. In the **User name** box, type the email address of user Britta Simon.

    c. Select the **Show Password** check box, and then write down the value that's displayed in the **Password** box.

    d. Click **Create**.
 
### Create a Dome9 Arc test user

To enable Azure AD users to log in to Dome9 Arc, they must be provisioned into application. Dome9 Arc supports just-in-time provisioning but for that to work properly, user have to select particular **Role** and assign the same to the user.

   >[!Note] 
   >For **Role** creation and other details contact [Dome9 Arc Client support team](https://dome9.com/about/contact-us/).

**To provision a user account manually, perform the following steps:**

1. Log in to your Dome9 Arc company site as an administrator.

2. Click on the **Users & Roles** and then click **Users**.

	![Add Employee](./media/dome9arc-tutorial/user1.png)

3. Click **ADD USER**.

	![Add Employee](./media/dome9arc-tutorial/user2.png)

4. In the **Create User** section, perform the following steps:
	
	![Add Employee](./media/dome9arc-tutorial/user3.png)

	a. In the **Email** textbox, type the email of user like Brittasimon@contoso.com.

	b. In the **First Name** textbox, type first name of the user like Britta.

	c. In the **Last Name** textbox, type last name of the user like Simon.

	d. Make **SSO User** as **On**.

	e. Click **CREATE**.

### Assign the Azure AD test user

In this section, you enable Britta Simon to use Azure single sign-on by granting access to Dome9 Arc.

![Assign the user role][200] 

**To assign Britta Simon to Dome9 Arc, perform the following steps:**

1. In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.

	![Assign User][201] 

2. In the applications list, select **Dome9 Arc**.

	![The Dome9 Arc link in the Applications list](./media/dome9arc-tutorial/tutorial_dome9arc_app.png)  

3. In the menu on the left, click **Users and groups**.

	![The "Users and groups" link][202]

4. Click **Add** button. Then select **Users and groups** on **Add Assignment** dialog.

	![The Add Assignment pane][203]

5. On **Users and groups** dialog, select **Britta Simon** in the Users list.

6. Click **Select** button on **Users and groups** dialog.

7. Click **Assign** button on **Add Assignment** dialog.
	
### Test single sign-on

In this section, you test your Azure AD single sign-on configuration using the Access Panel.

When you click the Dome9 Arc tile in the Access Panel, you should get automatically signed-on to your Dome9 Arc application.
For more information about the Access Panel, see [Introduction to the Access Panel](../active-directory-saas-access-panel-introduction.md). 

## Additional resources

* [List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/dome9arc-tutorial/tutorial_general_01.png
[2]: ./media/dome9arc-tutorial/tutorial_general_02.png
[3]: ./media/dome9arc-tutorial/tutorial_general_03.png
[4]: ./media/dome9arc-tutorial/tutorial_general_04.png

[100]: ./media/dome9arc-tutorial/tutorial_general_100.png

[200]: ./media/dome9arc-tutorial/tutorial_general_200.png
[201]: ./media/dome9arc-tutorial/tutorial_general_201.png
[202]: ./media/dome9arc-tutorial/tutorial_general_202.png
[203]: ./media/dome9arc-tutorial/tutorial_general_203.png

