---
title: 'Tutorial: Azure Active Directory integration with Infogix Data3Sixty Govern | Microsoft Docs'
description: Learn how to configure single sign-on between Azure Active Directory and Infogix Data3Sixty Govern.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore

ms.assetid: aa3109b8-bdbe-45ae-933a-2eb4dc03855c
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/23/2018
ms.author: jeedes

---
# Tutorial: Azure Active Directory integration with Infogix Data3Sixty Govern

In this tutorial, you learn how to integrate Infogix Data3Sixty Govern with Azure Active Directory (Azure AD).

Integrating Infogix Data3Sixty Govern with Azure AD provides you with the following benefits:

- You can control in Azure AD who has access to Infogix Data3Sixty Govern.
- You can enable your users to automatically get signed-on to Infogix Data3Sixty Govern (Single Sign-On) with their Azure AD accounts.
- You can manage your accounts in one central location - the Azure portal.

If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## Prerequisites

To configure Azure AD integration with Infogix Data3Sixty Govern, you need the following items:

- An Azure AD subscription
- An Infogix Data3Sixty Govern single sign-on enabled subscription

> [!NOTE]
> To test the steps in this tutorial, we do not recommend using a production environment.

To test the steps in this tutorial, you should follow these recommendations:

- Do not use your production environment, unless it is necessary.
- If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).

## Scenario description
In this tutorial, you test Azure AD single sign-on in a test environment. 
The scenario outlined in this tutorial consists of two main building blocks:

1. Adding Infogix Data3Sixty Govern from the gallery
2. Configuring and testing Azure AD single sign-on

## Adding Infogix Data3Sixty Govern from the gallery
To configure the integration of Infogix Data3Sixty Govern into Azure AD, you need to add Infogix Data3Sixty Govern from the gallery to your list of managed SaaS apps.

**To add Infogix Data3Sixty Govern from the gallery, perform the following steps:**

1. In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon. 

	![The Azure Active Directory button][1]

2. Navigate to **Enterprise applications**. Then go to **All applications**.

	![The Enterprise applications blade][2]
	
3. To add new application, click **New application** button on the top of dialog.

	![The New application button][3]

4. In the search box, type **Infogix Data3Sixty Govern**, select **Infogix Data3Sixty Govern** from result panel then click **Add** button to add the application.

	![Infogix Data3Sixty Govern in the results list](./media/infogix-tutorial/tutorial_infogix_addfromgallery.png)

## Configure and test Azure AD single sign-on

In this section, you configure and test Azure AD single sign-on with Infogix Data3Sixty Govern based on a test user called "Britta Simon".

For single sign-on to work, Azure AD needs to know what the counterpart user in Infogix Data3Sixty Govern is to a user in Azure AD. In other words, a link relationship between an Azure AD user and the related user in Infogix Data3Sixty Govern needs to be established.

To configure and test Azure AD single sign-on with Infogix Data3Sixty Govern, you need to complete the following building blocks:

1. **[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.
2. **[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.
3. **[Create an Infogix Data3Sixty Govern test user](#create-an-infogix-data3sixty-govern-test-user)** - to have a counterpart of Britta Simon in Infogix Data3Sixty Govern that is linked to the Azure AD representation of user.
4. **[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.
5. **[Test single sign-on](#test-single-sign-on)** - to verify whether the configuration works.

### Configure Azure AD single sign-on

In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Infogix Data3Sixty Govern application.

**To configure Azure AD single sign-on with Infogix Data3Sixty Govern, perform the following steps:**

1. In the Azure portal, on the **Infogix Data3Sixty Govern** application integration page, click **Single sign-on**.

	![Configure single sign-on link][4]

2. On the **Single sign-on** dialog, select **Mode** as	**SAML-based Sign-on** to enable single sign-on.
 
	![Single sign-on dialog box](./media/infogix-tutorial/tutorial_infogix_samlbase.png)

3. On the **Infogix Data3Sixty Govern Domain and URLs** section, perform the following steps if you wish to configure the application in **IDP** initiated mode:

	![Infogix Data3Sixty Govern Domain and URLs single sign-on information](./media/infogix-tutorial/tutorial_infogix_url.png)

    a. In the **Identifier** textbox, type a URL: `https://data3sixty.com/ui`

	b. In the **Reply URL** textbox, type a URL using the following pattern: `https://<subdomain>.data3sixty.com/sso/acs`

4. Check **Show advanced URL settings** and perform the following step if you wish to configure the application in **SP** initiated mode:

	![Infogix Data3Sixty Govern Domain and URLs single sign-on information](./media/infogix-tutorial/tutorial_infogix_url1.png)

    In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<subdomain>.data3sixty.com`
	 
	> [!NOTE] 
	> These values are not real. Update these values with the actual Reply URL and Sign-On URL. Contact [Infogix Data3Sixty Govern Client support team](mailto:data3sixtysupport@infogix.com) to get these values.

5. Infogix Data3Sixty Govern application expects the SAML assertions in a specific format. Configure the following claims for this application. You can manage the values of these attributes from the **User Attributes** section on application integration page. The following screenshot shows an example for this.
	
	![Configure Single Sign-On attb](./media/infogix-tutorial/tutorial_infogix_attribute.png)
	
6. In the **User Attributes** section on the **Single sign-on** dialog, configure SAML token attribute as shown in the image and perform the following steps:
	
	| Attribute Name | Attribute Value |
	| ------------------- | -------------------- |    
	| firstname 		  | user.givenname |
	| lastname 		  | user.surname |
	| username       | user.mail    |
	
	a. Click **Add attribute** to open the **Add Attribute** dialog.

	![Configure Single Sign-On Add](./media/infogix-tutorial/tutorial_attribute_04.png)

	![Configure Single Sign-On Addattb](./media/infogix-tutorial/tutorial_attribute_05.png)

	b. In the **Name** textbox, type the attribute name shown for that row.

	c. From the **Value** list, type the attribute value shown for that row.

	d. Leave the **Namespace** blank.
	
	e. Click **Ok**.

7. On the **SAML Signing Certificate** section, click **Certificate (Raw)** and then save the certificate file on your computer.

	![The Certificate download link](./media/infogix-tutorial/tutorial_infogix_certificate.png)

8. Click **Save** button.

	![Configure Single Sign-On Save button](./media/infogix-tutorial/tutorial_general_400.png)
	
9. On the **Infogix Data3Sixty Govern Configuration** section, click **Configure Infogix Data3Sixty Govern** to open **Configure sign-on** window. Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**

	![Infogix Data3Sixty Govern Configuration](./media/infogix-tutorial/tutorial_infogix_configure.png) 

10. To configure single sign-on on **Infogix Data3Sixty Govern** side, you need to send the downloaded **Certificate (Raw), Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** to [Infogix Data3Sixty Govern support team](mailto:data3sixtysupport@infogix.com). They set this setting to have the SAML SSO connection set properly on both sides.

### Create an Azure AD test user

The objective of this section is to create a test user in the Azure portal called Britta Simon.

   ![Create an Azure AD test user][100]

**To create a test user in Azure AD, perform the following steps:**

1. In the Azure portal, in the left pane, click the **Azure Active Directory** button.

    ![The Azure Active Directory button](./media/infogix-tutorial/create_aaduser_01.png)

2. To display the list of users, go to **Users and groups**, and then click **All users**.

    ![The "Users and groups" and "All users" links](./media/infogix-tutorial/create_aaduser_02.png)

3. To open the **User** dialog box, click **Add** at the top of the **All Users** dialog box.

    ![The Add button](./media/infogix-tutorial/create_aaduser_03.png)

4. In the **User** dialog box, perform the following steps:

    ![The User dialog box](./media/infogix-tutorial/create_aaduser_04.png)

    a. In the **Name** box, type **BrittaSimon**.

    b. In the **User name** box, type the email address of user Britta Simon.

    c. Select the **Show Password** check box, and then write down the value that's displayed in the **Password** box.

    d. Click **Create**.
 
### Create an Infogix Data3Sixty Govern test user


The objective of this section is to create a user called Britta Simon in Infogix Data3Sixty Govern. Infogix Data3Sixty Govern supports just-in-time provisioning, which is by default enabled. There is no action item for you in this section. A new user is created during an attempt to access Infogix Data3Sixty Govern if it doesn't exist yet.

>[!Note]
>If you need to create a user manually, contact [Infogix Data3Sixty Govern support team](mailto:data3sixtysupport@infogix.com).

### Assign the Azure AD test user

In this section, you enable Britta Simon to use Azure single sign-on by granting access to Infogix Data3Sixty Govern.

![Assign the user role][200] 

**To assign Britta Simon to Infogix Data3Sixty Govern, perform the following steps:**

1. In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.

	![Assign User][201] 

2. In the applications list, select **Infogix Data3Sixty Govern**.

	![The Infogix Data3Sixty Govern link in the Applications list](./media/infogix-tutorial/tutorial_infogix_app.png)  

3. In the menu on the left, click **Users and groups**.

	![The "Users and groups" link][202]

4. Click **Add** button. Then select **Users and groups** on **Add Assignment** dialog.

	![The Add Assignment pane][203]

5. On **Users and groups** dialog, select **Britta Simon** in the Users list.

6. Click **Select** button on **Users and groups** dialog.

7. Click **Assign** button on **Add Assignment** dialog.
	
### Test single sign-on

In this section, you test your Azure AD single sign-on configuration using the Access Panel.

When you click the Infogix Data3Sixty Govern tile in the Access Panel, you should get automatically signed-on to your Infogix Data3Sixty Govern application.
For more information about the Access Panel, see [Introduction to the Access Panel](../active-directory-saas-access-panel-introduction.md). 

## Additional resources

* [List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/infogix-tutorial/tutorial_general_01.png
[2]: ./media/infogix-tutorial/tutorial_general_02.png
[3]: ./media/infogix-tutorial/tutorial_general_03.png
[4]: ./media/infogix-tutorial/tutorial_general_04.png

[100]: ./media/infogix-tutorial/tutorial_general_100.png

[200]: ./media/infogix-tutorial/tutorial_general_200.png
[201]: ./media/infogix-tutorial/tutorial_general_201.png
[202]: ./media/infogix-tutorial/tutorial_general_202.png
[203]: ./media/infogix-tutorial/tutorial_general_203.png

