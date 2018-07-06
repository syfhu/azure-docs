---
title: 'Tutorial: Azure Active Directory integration with BlueJeans | Microsoft Docs'
description: Learn how to configure single sign-on between Azure Active Directory and BlueJeans.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman

ms.assetid: dfc634fd-1b55-4ba8-94a8-b8288429b6a9
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/15/2018
ms.author: jeedes

---
# Tutorial: Azure Active Directory integration with BlueJeans

In this tutorial, you learn how to integrate BlueJeans with Azure Active Directory (Azure AD).

Integrating BlueJeans with Azure AD provides you with the following benefits:

- You can control in Azure AD who has access to BlueJeans
- You can enable your users to automatically get signed-on to BlueJeans (Single Sign-On) with their Azure AD accounts
- You can manage your accounts in one central location - the Azure portal

If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## Prerequisites

To configure Azure AD integration with BlueJeans, you need the following items:

- An Azure AD subscription
- A BlueJeans single-sign on enabled subscription

> [!NOTE]
> To test the steps in this tutorial, we do not recommend using a production environment.

To test the steps in this tutorial, you should follow these recommendations:

- Do not use your production environment, unless it is necessary.
- If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).

## Scenario description
In this tutorial, you test Azure AD single sign-on in a test environment. 
The scenario outlined in this tutorial consists of two main building blocks:

1. Adding BlueJeans from the gallery
2. Configuring and testing Azure AD single sign-on

## Adding BlueJeans from the gallery
To configure the integration of BlueJeans into Azure AD, you need to add BlueJeans from the gallery to your list of managed SaaS apps.

**To add BlueJeans from the gallery, perform the following steps:**

1. In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.

	![Active Directory][1]

2. Navigate to **Enterprise applications**. Then go to **All applications**.

	![Applications][2]

3. To add new application, click **New application** button on the top of dialog.

	![Applications][3]

4. In the search box, type **BlueJeans**.

	![Creating an Azure AD test user](./media/bluejeans-tutorial/tutorial_bluejeans_search.png)

5. In the results panel, select **BlueJeans**, and then click **Add** button to add the application.

	![Creating an Azure AD test user](./media/bluejeans-tutorial/tutorial_bluejeans_addfromgallery.png)

##  Configuring and testing Azure AD single sign-on
In this section, you configure and test Azure AD single sign-on with BlueJeans based on a test user called "Britta Simon."

For single sign-on to work, Azure AD needs to know what the counterpart user in BlueJeans is to a user in Azure AD. In other words, a link relationship between an Azure AD user and the related user in BlueJeans needs to be established.

In BlueJeans, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.

To configure and test Azure AD single sign-on with BlueJeans, you need to complete the following building blocks:

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.
2. **[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.
3. **[Creating a BlueJeans test user](#creating-a-bluejeans-test-user)** - to have a counterpart of Britta Simon in BlueJeans that is linked to the Azure AD representation of user.
4. **[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.
5. **[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.

### Configuring Azure AD single sign-on

In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your BlueJeans application.

**To configure Azure AD single sign-on with BlueJeans, perform the following steps:**

1. In the Azure portal, on the **BlueJeans** application integration page, click **Single sign-on**.

	![Configure Single Sign-On][4]

2. On the **Single sign-on** dialog, select **Mode** as	**SAML-based Sign-on** to enable single sign-on.

	![Configure Single Sign-On](./media/bluejeans-tutorial/tutorial_bluejeans_samlbase.png)

3. On the **BlueJeans Domain and URLs** section, perform the following steps:

	![Configure Single Sign-On](./media/bluejeans-tutorial/tutorial_bluejeans_url.png)

    a. In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<companyname>.BlueJeans.com`

	b. In the **Identifier** textbox, type a URL using the following pattern: `https://<companyname>.BlueJeans.com`

	> [!NOTE]
	> These values are not real. Update these values with the actual Sign-On URL and Identifier. Contact [BlueJeans Client support team](https://support.bluejeans.com/contact) to get these values.

4. On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.

	![Configure Single Sign-On](./media/bluejeans-tutorial/tutorial_bluejeans_certificate.png) 

5. Click **Save** button.

	![Configure Single Sign-On](./media/bluejeans-tutorial/tutorial_general_400.png)

6. On the **BlueJeans Configuration** section, click **Configure BlueJeans** to open **Configure sign-on** window. Copy the **Sign-Out URL, Change Password URL and SAML Single Sign-On Service URL** from the **Quick Reference section.**

	![Configure Single Sign-On](./media/bluejeans-tutorial/tutorial_bluejeans_configure.png) 

7. In a different web browser window, log in to your **BlueJeans** company site as an administrator.

8. Go to **ADMIN \> Group Settings \> Security**.

   ![Admin](./media/bluejeans-tutorial/IC785868.png "Admin")

9. In the **Security** section, perform the following steps:

   ![SAML Single Sign On](./media/bluejeans-tutorial/IC785869.png "SAML Single Sign On")

   a. Select **SAML Single Sign On**.

   b. Select **Enable automatic provisioning**.

10. Move on with the following steps:

	![Certificate Path](./media/bluejeans-tutorial/IC785870.png "Certificate Path")

	a. Click **Choose File**, and then upload the downloaded certificate.

    b. Paste **SAML Single Sign-On Service URL** into the **Login URL** textbox.

    c. Paste **Change Password URL** into the **Password Change URL** textbox.

    d. Paste **Sign-Out URL** into the **Logout URL** textbox.

11. Move on with the following steps:

	![Save Changes](./media/bluejeans-tutorial/IC785874.png "Save Changes")

	a. In the **User id** textbox, type `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.

    b. In the **Email** textbox, type `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.

    c. Click **Save Changes**.

### Creating an Azure AD test user
The objective of this section is to create a test user in the Azure portal called Britta Simon.

![Create Azure AD User][100]

**To create a test user in Azure AD, perform the following steps:**

1. In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.

	![Creating an Azure AD test user](./media/bluejeans-tutorial/create_aaduser_01.png)

2. To display the list of users, go to **Users and groups** and click **All users**.

	![Creating an Azure AD test user](./media/bluejeans-tutorial/create_aaduser_02.png)

3. To open the **User** dialog, click **Add** on the top of the dialog.

	![Creating an Azure AD test user](./media/bluejeans-tutorial/create_aaduser_03.png)

4. On the **User** dialog page, perform the following steps:

	![Creating an Azure AD test user](./media/bluejeans-tutorial/create_aaduser_04.png) 

    a. In the **Name** textbox, type **BrittaSimon**.

    b. In the **User name** textbox, type the **email address** of BrittaSimon.

	c. Select **Show Password** and write down the value of the **Password**.

    d. Click **Create**.

### Creating a BlueJeans test user

The objective of this section is to create a user called Britta Simon in BlueJeans. BlueJeans supports automatic user provisioning, which is by default enabled. You can find more details [here](bluejeans-provisioning-tutorial.md) on how to configure automatic user provisioning.

**If you need to create user manually, perform following steps:**

1. Log in to your **BlueJeans** company site as an administrator.

2. Go to **ADMIN \> Manage Users \> Add User**.

   ![Admin](./media/bluejeans-tutorial/IC785877.png "Admin")

   >[!IMPORTANT]
   >The **Add User** tab is only available if, in the **Security tab**, **Enable automatic provisioning** is unchecked. 

3. In the **Add User** section, perform the following steps:

	![Add User](./media/bluejeans-tutorial/IC785886.png "Add User")

	a. Type a **BlueJeans Username**, an **Email address**, a **BlueJeans Meeting ID**, a **Moderator Passcode**, a **Full Name**, the **Company** of a valid AAD account you want to provision into the related textboxes.

	b. Click **Add User**.

>[!NOTE]
>You can use any other BlueJeans user account creation tools or APIs provided by BlueJeans to provision AAD user accounts.

### Assigning the Azure AD test user

In this section, you enable Britta Simon to use Azure single sign-on by granting access to BlueJeans.

![Assign User][200]

**To assign Britta Simon to BlueJeans, perform the following steps:**

1. In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.

	![Assign User][201]

2. In the applications list, select **BlueJeans**.

	![Configure Single Sign-On](./media/bluejeans-tutorial/tutorial_bluejeans_app.png)

3. In the menu on the left, click **Users and groups**.

	![Assign User][202]

4. Click **Add** button. Then select **Users and groups** on **Add Assignment** dialog.

	![Assign User][203]

5. On **Users and groups** dialog, select **Britta Simon** in the Users list.

6. Click **Select** button on **Users and groups** dialog.

7. Click **Assign** button on **Add Assignment** dialog.

### Testing single sign-on

In this section, you test your Azure AD single sign-on configuration using the Access Panel.

When you click the BlueJeans tile in the Access Panel, you should get login page of BlueJeans application.
For more information about the Access Panel, see [Introduction to the Access Panel](../active-directory-saas-access-panel-introduction.md). 

## Additional resources

* [List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)
* [Configure User Provisioning](bluejeans-provisioning-tutorial.md)

<!--Image references-->

[1]: ./media/bluejeans-tutorial/tutorial_general_01.png
[2]: ./media/bluejeans-tutorial/tutorial_general_02.png
[3]: ./media/bluejeans-tutorial/tutorial_general_03.png
[4]: ./media/bluejeans-tutorial/tutorial_general_04.png

[100]: ./media/bluejeans-tutorial/tutorial_general_100.png

[200]: ./media/bluejeans-tutorial/tutorial_general_200.png
[201]: ./media/bluejeans-tutorial/tutorial_general_201.png
[202]: ./media/bluejeans-tutorial/tutorial_general_202.png
[203]: ./media/bluejeans-tutorial/tutorial_general_203.png
