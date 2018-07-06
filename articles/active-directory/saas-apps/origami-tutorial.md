---
title: 'Tutorial: Azure Active Directory integration with Origami | Microsoft Docs'
description: Learn how to configure single sign-on between Azure Active Directory and Origami.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman

ms.assetid: a28bb0ba-b564-46ba-accc-e587699295d4
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2017
ms.author: jeedes

---
# Tutorial: Azure Active Directory integration with Origami

In this tutorial, you learn how to integrate Origami with Azure Active Directory (Azure AD).

Integrating Origami with Azure AD provides you with the following benefits:

- You can control in Azure AD who has access to Origami
- You can enable your users to automatically get signed-on to Origami (Single Sign-On) with their Azure AD accounts
- You can manage your accounts in one central location - the Azure portal

If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## Prerequisites

To configure Azure AD integration with Origami, you need the following items:

- An Azure AD subscription
- An Origami single sign-on enabled subscription

> [!NOTE]
> To test the steps in this tutorial, we do not recommend using a production environment.

To test the steps in this tutorial, you should follow these recommendations:

- Do not use your production environment, unless it is necessary.
- If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).

## Scenario description
In this tutorial, you test Azure AD single sign-on in a test environment. 
The scenario outlined in this tutorial consists of two main building blocks:

1. Adding Origami from the gallery
2. Configuring and testing Azure AD single sign-on

## Adding Origami from the gallery
To configure the integration of Origami into Azure AD, you need to add Origami from the gallery to your list of managed SaaS apps.

**To add Origami from the gallery, perform the following steps:**

1. In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon. 

	![Active Directory][1]

2. Navigate to **Enterprise applications**. Then go to **All applications**.

	![Applications][2]
	
3. To add new application, click **New application** button on the top of dialog.

	![Applications][3]

4. In the search box, type **Origami**.

	![Creating an Azure AD test user](./media/origami-tutorial/tutorial_origami_search.png)

5. In the results panel, select **Origami**, and then click **Add** button to add the application.

	![Creating an Azure AD test user](./media/origami-tutorial/tutorial_origami_addfromgallery.png)

##  Configuring and testing Azure AD single sign-on
In this section, you configure and test Azure AD single sign-on with Origami based on a test user called "Britta Simon".

For single sign-on to work, Azure AD needs to know what the counterpart user in Origami is to a user in Azure AD. In other words, a link relationship between an Azure AD user and the related user in Origami needs to be established.

In Origami, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.

To configure and test Azure AD single sign-on with Origami, you need to complete the following building blocks:

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.
2. **[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.
3. **[Creating an Origami test user](#creating-an-origami-test-user)** - to have a counterpart of Britta Simon in Origami that is linked to the Azure AD representation of user.
4. **[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.
5. **[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.

### Configuring Azure AD single sign-on

In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Origami application.

**To configure Azure AD single sign-on with Origami, perform the following steps:**

1. In the Azure portal, on the **Origami** application integration page, click **Single sign-on**.

	![Configure Single Sign-On][4]

2. On the **Single sign-on** dialog, select **Mode** as	**SAML-based Sign-on** to enable single sign-on.
 
	![Configure Single Sign-On](./media/origami-tutorial/tutorial_origami_samlbase.png)

3. On the **Origami Domain and URLs** section, perform the following steps:

	![Configure Single Sign-On](./media/origami-tutorial/tutorial_origami_url.png)

    In the **Sign-on URL** textbox, type a URL using the following pattern: `https://live.origamirisk.com/origami/account/login?account=<companyname>`

	> [!NOTE] 
	> The value is not real. Update the value with the actual Sign-On URL. Contact [Origami Client support team](https://wordpress.org/support/theme/origami) to get the value. 
 
4. On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.

	![Configure Single Sign-On](./media/origami-tutorial/tutorial_origami_certificate.png) 

5. Click **Save** button.

	![Configure Single Sign-On](./media/origami-tutorial/tutorial_general_400.png)

6. On the **Origami Configuration** section, click **Configure Origami** to open **Configure sign-on** window. Copy the **Sign-Out URL, and SAML Single Sign-On Service URL** from the **Quick Reference section.**

	![Configure Single Sign-On](./media/origami-tutorial/tutorial_origami_configure.png) 

7. Log in to the Origami account with Admin rights.

8. In the menu on the top, click **Admin**.
   
    ![Configure Single Sign-On](./media/origami-tutorial/tutorial_origami_51.png)

9. On the Single Sign On Setup dialog page, perform the following steps:
   
    ![Configure Single Sign-On](./media/origami-tutorial/tutorial_origami_531.png)

    a. Select **Enable Single Sign On**.

    b. In the **Identity Provider's Sign-in Page URL** textbox, paste the value of **SAML Single Sign-On Service URL**, which you have copied from Azure portal.

    c. In the **Identity Provider's Sign-out Page URL** textbox, paste the value of **Sign-Out URL**, which you have copied from Azure portal.

    d. Click **Browse** to upload the certificate you have downloaded from the Azure portal.

    e. Click **Save Changes**.

> [!TIP]
> You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!  After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom. You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### Creating an Azure AD test user
The objective of this section is to create a test user in the Azure portal called Britta Simon.

![Create Azure AD User][100]

**To create a test user in Azure AD, perform the following steps:**

1. In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.

	![Creating an Azure AD test user](./media/origami-tutorial/create_aaduser_01.png) 

2. To display the list of users, go to **Users and groups** and click **All users**.
	
	![Creating an Azure AD test user](./media/origami-tutorial/create_aaduser_02.png) 

3. To open the **User** dialog, click **Add** on the top of the dialog.
 
	![Creating an Azure AD test user](./media/origami-tutorial/create_aaduser_03.png) 

4. On the **User** dialog page, perform the following steps:
 
	![Creating an Azure AD test user](./media/origami-tutorial/create_aaduser_04.png) 

    a. In the **Name** textbox, type **BrittaSimon**.

    b. In the **User name** textbox, type the **email address** of BrittaSimon.

	c. Select **Show Password** and write down the value of the **Password**.

    d. Click **Create**.
 
### Creating an Origami test user

In this section, you create a user called Britta Simon in Origami. 

1. Log in to the Origami account with Admin rights.

2. In the menu on the top, click **Admin**.
   
    ![Configure Single Sign-On](./media/origami-tutorial/tutorial_origami_51.png)

3. On the **Users and Security** dialog, click **Users**.
   
    ![Configure Single Sign-On](./media/origami-tutorial/tutorial_origami_54.png)

4. Click **Add New User**.
   
    ![Configure Single Sign-On](./media/origami-tutorial/tutorial_origami_55.png)

5. On the Add New User dialog, perform the following steps:
   
    ![Configure Single Sign-On](./media/origami-tutorial/tutorial_origami_56.png)

    a. In the **User Name** textbox, enter the email of user like **brittasimon@contoso.com**.

    b. In the **Password** textbox, type a password.

    c. In the **Confirm Password** textbox, type the password again.

    d. In the **First Name** textbox, enter the first name of user like **Britta**.

    e. In the **Last Name** textbox, enter the last name of user like **Simon**.

    f. Click **Save**.
   
    ![Configure Single Sign-On](./media/origami-tutorial/tutorial_origami_57.png)

6. Assign **User Roles** and **Client Access** to the user. 
   
    ![Configure Single Sign-On](./media/origami-tutorial/tutorial_origami_58.png)

### Assigning the Azure AD test user

In this section, you enable Britta Simon to use Azure single sign-on by granting access to Origami.

![Assign User][200] 

**To assign Britta Simon to Origami, perform the following steps:**

1. In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.

	![Assign User][201] 

2. In the applications list, select **Origami**.

	![Configure Single Sign-On](./media/origami-tutorial/tutorial_origami_app.png) 

3. In the menu on the left, click **Users and groups**.

	![Assign User][202] 

4. Click **Add** button. Then select **Users and groups** on **Add Assignment** dialog.

	![Assign User][203]

5. On **Users and groups** dialog, select **Britta Simon** in the Users list.

6. Click **Select** button on **Users and groups** dialog.

7. Click **Assign** button on **Add Assignment** dialog.
	
### Testing single sign-on

In this section, you test your Azure AD single sign-on configuration using the Access Panel.

When you click the Origami tile in the Access Panel, you should get automatically signed-on to your Origami application.

## Additional resources

* [List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/origami-tutorial/tutorial_general_01.png
[2]: ./media/origami-tutorial/tutorial_general_02.png
[3]: ./media/origami-tutorial/tutorial_general_03.png
[4]: ./media/origami-tutorial/tutorial_general_04.png

[100]: ./media/origami-tutorial/tutorial_general_100.png

[200]: ./media/origami-tutorial/tutorial_general_200.png
[201]: ./media/origami-tutorial/tutorial_general_201.png
[202]: ./media/origami-tutorial/tutorial_general_202.png
[203]: ./media/origami-tutorial/tutorial_general_203.png

