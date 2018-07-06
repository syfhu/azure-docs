---
title: 'Tutorial: Azure Active Directory integration with Mindflash | Microsoft Docs'
description: Learn how to configure single sign-on between Azure Active Directory and Mindflash.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman

ms.assetid: bdf91993-aaaa-4598-89b7-77ef8ca065d5
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2017
ms.author: jeedes

---
# Tutorial: Azure Active Directory integration with Mindflash

In this tutorial, you learn how to integrate Mindflash with Azure Active Directory (Azure AD).

Integrating Mindflash with Azure AD provides you with the following benefits:

- You can control in Azure AD who has access to Mindflash
- You can enable your users to automatically get signed-on to Mindflash (Single Sign-On) with their Azure AD accounts
- You can manage your accounts in one central location - the Azure portal

If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## Prerequisites

To configure Azure AD integration with Mindflash, you need the following items:

- An Azure AD subscription
- A Mindflash single sign-on enabled subscription

> [!NOTE]
> To test the steps in this tutorial, we do not recommend using a production environment.

To test the steps in this tutorial, you should follow these recommendations:

- Do not use your production environment, unless it is necessary.
- If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).

## Scenario description
In this tutorial, you test Azure AD single sign-on in a test environment. 
The scenario outlined in this tutorial consists of two main building blocks:

1. Adding Mindflash from the gallery
2. Configuring and testing Azure AD single sign-on

## Adding Mindflash from the gallery
To configure the integration of Mindflash into Azure AD, you need to add Mindflash from the gallery to your list of managed SaaS apps.

**To add Mindflash from the gallery, perform the following steps:**

1. In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon. 

	![Active Directory][1]

2. Navigate to **Enterprise applications**. Then go to **All applications**.

	![Applications][2]
	
3. To add new application, click **New application** button on the top of dialog.

	![Applications][3]

4. In the search box, type **Mindflash**.

	![Creating an Azure AD test user](./media/mindflash-tutorial/tutorial_mindflash_search.png)

5. In the results panel, select **Mindflash**, and then click **Add** button to add the application.

	![Creating an Azure AD test user](./media/mindflash-tutorial/tutorial_mindflash_addfromgallery.png)

##  Configuring and testing Azure AD single sign-on
In this section, you configure and test Azure AD single sign-on with Mindflash based on a test user called "Britta Simon".

For single sign-on to work, Azure AD needs to know what the counterpart user in Mindflash is to a user in Azure AD. In other words, a link relationship between an Azure AD user and the related user in Mindflash needs to be established.

In Mindflash, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.

To configure and test Azure AD single sign-on with Mindflash, you need to complete the following building blocks:

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.
2. **[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.
3. **[Creating a Mindflash test user](#creating-a-mindflash-test-user)** - to have a counterpart of Britta Simon in Mindflash that is linked to the Azure AD representation of user.
4. **[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.
5. **[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.

### Configuring Azure AD single sign-on

In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Mindflash application.

**To configure Azure AD single sign-on with Mindflash, perform the following steps:**

1. In the Azure portal, on the **Mindflash** application integration page, click **Single sign-on**.

	![Configure Single Sign-On][4]

2. On the **Single sign-on** dialog, select **Mode** as	**SAML-based Sign-on** to enable single sign-on.
 
	![Configure Single Sign-On](./media/mindflash-tutorial/tutorial_mindflash_samlbase.png)

3. On the **Mindflash Domain and URLs** section, perform the following steps:

	![Configure Single Sign-On](./media/mindflash-tutorial/tutorial_mindflash_url.png)

    a. In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<companyname>.mindflash.com`

	b. In the **Identifier** textbox, type a URL using the following pattern: `https://<companyname>.mindflash.com`

	> [!NOTE] 
	> These values are not real. Update these values with the actual Sign-On URL and Identifier. Contact [Mindflash Client support team](https://www.mindflash.com/contact/) to get these values. 
 


4. On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.

	![Configure Single Sign-On](./media/mindflash-tutorial/tutorial_mindflash_certificate.png) 

5. Click **Save** button.

	![Configure Single Sign-On](./media/mindflash-tutorial/tutorial_general_400.png)

6. To configure single sign-on on **Mindflash** side, you need to send the downloaded **Metadata XML** to [Mindflash support team](https://www.mindflash.com/contact/).

> [!TIP]
> You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!  After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom. You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### Creating an Azure AD test user
The objective of this section is to create a test user in the Azure portal called Britta Simon.

![Create Azure AD User][100]

**To create a test user in Azure AD, perform the following steps:**

1. In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.

	![Creating an Azure AD test user](./media/mindflash-tutorial/create_aaduser_01.png) 

2. To display the list of users, go to **Users and groups** and click **All users**.
	
	![Creating an Azure AD test user](./media/mindflash-tutorial/create_aaduser_02.png) 

3. To open the **User** dialog, click **Add** on the top of the dialog.
 
	![Creating an Azure AD test user](./media/mindflash-tutorial/create_aaduser_03.png) 

4. On the **User** dialog page, perform the following steps:
 
	![Creating an Azure AD test user](./media/mindflash-tutorial/create_aaduser_04.png) 

    a. In the **Name** textbox, type **BrittaSimon**.

    b. In the **User name** textbox, type the **email address** of BrittaSimon.

	c. Select **Show Password** and write down the value of the **Password**.

    d. Click **Create**.
 
### Creating a Mindflash test user

In order to enable Azure AD users to log into Mindflash, they must be provisioned into Mindflash. In the case of Mindflash, provisioning is a manual task.

### To provision a user accounts, perform the following steps:

1. Log in to your **Mindflash** company site as an administrator.

2. Go to **Manage Users**.
   
    ![Manage Users](./media/mindflash-tutorial/ic787140.png "Manage Users")

3. Click the **Add Users**, and then click **New**.

4. In the **Add New Users** section, perform the following steps of a valid Azure AD account you want to provision:
   
    ![Add New Users](./media/mindflash-tutorial/ic787141.png "Add New Users")
   
    a. In the **First name** textbox, type **First name** of the user as **Britta**.

	b. In the **Last name** textbox, type **Last name** of the user as **Simon**.
	
	c. In the **Email** textbox, type **Email Address** of the user as **BrittaSimon@contoso.com**.

    b. Click **Add**.

>[!NOTE]
>You can use any other Mindflash user account creation tools or APIs provided by Mindflash to provision AAD user accounts. 
> 

### Assigning the Azure AD test user

In this section, you enable Britta Simon to use Azure single sign-on by granting access to Mindflash.

![Assign User][200] 

**To assign Britta Simon to Mindflash, perform the following steps:**

1. In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.

	![Assign User][201] 

2. In the applications list, select **Mindflash**.

	![Configure Single Sign-On](./media/mindflash-tutorial/tutorial_mindflash_app.png) 

3. In the menu on the left, click **Users and groups**.

	![Assign User][202] 

4. Click **Add** button. Then select **Users and groups** on **Add Assignment** dialog.

	![Assign User][203]

5. On **Users and groups** dialog, select **Britta Simon** in the Users list.

6. Click **Select** button on **Users and groups** dialog.

7. Click **Assign** button on **Add Assignment** dialog.
	
### Testing single sign-on

In this section, you test your Azure AD single sign-on configuration using the Access Panel.

When you click the Mindflash tile in the Access Panel, you should get login page of Mindflash application.
For more information about the Access Panel, see [Introduction to the Access Panel](../active-directory-saas-access-panel-introduction.md).

## Additional resources

* [List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/mindflash-tutorial/tutorial_general_01.png
[2]: ./media/mindflash-tutorial/tutorial_general_02.png
[3]: ./media/mindflash-tutorial/tutorial_general_03.png
[4]: ./media/mindflash-tutorial/tutorial_general_04.png

[100]: ./media/mindflash-tutorial/tutorial_general_100.png

[200]: ./media/mindflash-tutorial/tutorial_general_200.png
[201]: ./media/mindflash-tutorial/tutorial_general_201.png
[202]: ./media/mindflash-tutorial/tutorial_general_202.png
[203]: ./media/mindflash-tutorial/tutorial_general_203.png

