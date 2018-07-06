---
title: 'Tutorial: Azure Active Directory integration with IdeaScale | Microsoft Docs'
description: Learn how to configure single sign-on between Azure Active Directory and IdeaScale.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman

ms.assetid: e16dda6b-fdf9-43cc-9bbb-a523f085a8af
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes

---
# Tutorial: Azure Active Directory integration with IdeaScale

In this tutorial, you learn how to integrate IdeaScale with Azure Active Directory (Azure AD).

Integrating IdeaScale with Azure AD provides you with the following benefits:

- You can control in Azure AD who has access to IdeaScale
- You can enable your users to automatically get signed-on to IdeaScale (Single Sign-On) with their Azure AD accounts
- You can manage your accounts in one central location - the Azure portal

If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## Prerequisites

To configure Azure AD integration with IdeaScale, you need the following items:

- An Azure AD subscription
- An IdeaScale single-sign on enabled subscription

> [!NOTE]
> To test the steps in this tutorial, we do not recommend using a production environment.

To test the steps in this tutorial, you should follow these recommendations:

- Do not use your production environment, unless it is necessary.
- If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).

## Scenario description
In this tutorial, you test Azure AD single sign-on in a test environment. 
The scenario outlined in this tutorial consists of two main building blocks:

1. Adding IdeaScale from the gallery
2. Configuring and testing Azure AD single sign-on

## Adding IdeaScale from the gallery
To configure the integration of IdeaScale into Azure AD, you need to add IdeaScale from the gallery to your list of managed SaaS apps.

**To add IdeaScale from the gallery, perform the following steps:**

1. In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon. 

	![Active Directory][1]

2. Navigate to **Enterprise applications**. Then go to **All applications**.

	![Applications][2]
	
3. To add new application, click **New application** button on the top of dialog.

	![Applications][3]

4. In the search box, type **IdeaScale**.

	![Creating an Azure AD test user](./media/ideascale-tutorial/tutorial_ideascale_search.png)

5. In the results panel, select **IdeaScale**, and then click **Add** button to add the application.

	![Creating an Azure AD test user](./media/ideascale-tutorial/tutorial_ideascale_addfromgallery.png)

##  Configuring and testing Azure AD single sign-on
In this section, you configure and test Azure AD single sign-on with IdeaScale based on a test user called "Britta Simon."

For single sign-on to work, Azure AD needs to know what the counterpart user in IdeaScale is to a user in Azure AD. In other words, a link relationship between an Azure AD user and the related user in IdeaScale needs to be established.

In IdeaScale, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.

To configure and test Azure AD single sign-on with IdeaScale, you need to complete the following building blocks:

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.
2. **[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.
3. **[Creating an IdeaScale test user](#creating-an-ideascale-test-user)** - to have a counterpart of Britta Simon in IdeaScale that is linked to the Azure AD representation of user.
4. **[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.
5. **[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.

### Configuring Azure AD single sign-on

In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your IdeaScale application.

**To configure Azure AD single sign-on with IdeaScale, perform the following steps:**

1. In the Azure portal, on the **IdeaScale** application integration page, click **Single sign-on**.

	![Configure Single Sign-On][4]

2. On the **Single sign-on** dialog, select **Mode** as	**SAML-based Sign-on** to enable single sign-on.
 
	![Configure Single Sign-On](./media/ideascale-tutorial/tutorial_ideascale_samlbase.png)

3. On the **IdeaScale Domain and URLs** section, perform the following steps:

	![Configure Single Sign-On](./media/ideascale-tutorial/tutorial_ideascale_url.png)

    a. In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<companyname>.ideascale.com`

	b. In the **Identifier** textbox, type a URL using the following pattern:
	| |
	|--|
	| `http://<companyname>.ideascale.com`  |
	| `https://<companyname>.ideascale.com` |

	> [!NOTE] 
	> These values are not real. Update these values with the actual Sign-On URL and Identifier. Contact [IdeaScale Client support team](http://support.ideascale.com/) to get these values. 
 
4. On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.

	![Configure Single Sign-On](./media/ideascale-tutorial/tutorial_ideascale_certificate.png) 

5. Click **Save** button.

	![Configure Single Sign-On](./media/ideascale-tutorial/tutorial_general_400.png)

6. On the **IdeaScale Configuration** section, click **Configure IdeaScale** to open **Configure sign-on** window. Copy the **Sign-Out URL, and SAML Entity ID** from the **Quick Reference section**.

	![Configure Single Sign-On](./media/ideascale-tutorial/tutorial_ideascale_configure.png) 

7. In a different web browser window, log in to your IdeaScale company site as an administrator.

8. Go to **Community Settings**.
   
    ![Community Settings](./media/ideascale-tutorial/ic790847.png "Community Settings")

9. Go to **Security \> Single Signon Settings**.
   
    ![Single Signon Settings](./media/ideascale-tutorial/ic790848.png "Single Signon Settings")

10. As **Single-Signon Type**, select **SAML 2.0**.
   
    ![Single Signon Type](./media/ideascale-tutorial/ic790849.png "Single Signon Type")

11. On the **Single Signon Settings** dialog, perform the following steps:
   
    ![Single Signon Settings](./media/ideascale-tutorial/ic790850.png "Single Signon Settings")
   
    a. In **SAML IdP Entity ID** textbox, paste the value of **SAML Entity ID** which you have copied from Azure portal.

    b. Copy the content of your downloaded metadata file from Azure portal, and paste it into the **SAML IdP Metadata** textbox.

    c. In **Logout Success URL** textbox, paste the value of **Sign-Out URL** which you have copied from Azure portal.

    d. Click **Save Changes**.

> [!TIP]
> You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!  After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom. You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### Creating an Azure AD test user
The objective of this section is to create a test user in the Azure portal called Britta Simon.

![Create Azure AD User][100]

**To create a test user in Azure AD, perform the following steps:**

1. In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.

	![Creating an Azure AD test user](./media/ideascale-tutorial/create_aaduser_01.png) 

2. To display the list of users, go to **Users and groups** and click **All users**.
	
	![Creating an Azure AD test user](./media/ideascale-tutorial/create_aaduser_02.png) 

3. To open the **User** dialog, click **Add** on the top of the dialog.
 
	![Creating an Azure AD test user](./media/ideascale-tutorial/create_aaduser_03.png) 

4. On the **User** dialog page, perform the following steps:
 
	![Creating an Azure AD test user](./media/ideascale-tutorial/create_aaduser_04.png) 

    a. In the **Name** textbox, type **BrittaSimon**.

    b. In the **User name** textbox, type the **email address** of BrittaSimon.

	c. Select **Show Password** and write down the value of the **Password**.

    d. Click **Create**.
 
### Creating an IdeaScale test user

To enable Azure AD users to log into IdeaScale, they must be provisioned in to IdeaScale. In the case of IdeaScale, provisioning is a manual task.

**To configure user provisioning, perform the following steps:**

1. Log in to your **IdeaScale** company site as administrator.

2. Go to **Community Settings**.
   
    ![Community Settings](./media/ideascale-tutorial/ic790847.png "Community Settings")

3. Go to **Basic Settings \> Member Management**.

4. Click **Add Member**.
   
    ![Member Management](./media/ideascale-tutorial/ic790852.png "Member Management")

5. In the Add New Member section, perform the following steps:
   
    ![Add New Member](./media/ideascale-tutorial/ic790853.png "Add New Member")
   
    a. In the **Email Addresses** textbox, type the email address of a valid AAD account you want to provision.
   
    b. Click **Save Changes**. 
   
    >[!NOTE]
    >The Azure Active Directory account holder gets an email with a link to confirm the account before it becomes active.
      
>[!NOTE]
>You can use any other IdeaScale user account creation tools or APIs provided by IdeaScale to provision AAD user accounts.
 

### Assigning the Azure AD test user

In this section, you enable Britta Simon to use Azure single sign-on by granting access to IdeaScale.

![Assign User][200] 

**To assign Britta Simon to IdeaScale, perform the following steps:**

1. In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.

	![Assign User][201] 

2. In the applications list, select **IdeaScale**.

	![Configure Single Sign-On](./media/ideascale-tutorial/tutorial_ideascale_app.png) 

3. In the menu on the left, click **Users and groups**.

	![Assign User][202] 

4. Click **Add** button. Then select **Users and groups** on **Add Assignment** dialog.

	![Assign User][203]

5. On **Users and groups** dialog, select **Britta Simon** in the Users list.

6. Click **Select** button on **Users and groups** dialog.

7. Click **Assign** button on **Add Assignment** dialog.
	
### Testing single sign-on


The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.

When you click the IdeaScale tile in the Access Panel, you should get automatically signed-on to your IdeaScale application.

## Additional resources

* [List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/ideascale-tutorial/tutorial_general_01.png
[2]: ./media/ideascale-tutorial/tutorial_general_02.png
[3]: ./media/ideascale-tutorial/tutorial_general_03.png
[4]: ./media/ideascale-tutorial/tutorial_general_04.png

[100]: ./media/ideascale-tutorial/tutorial_general_100.png

[200]: ./media/ideascale-tutorial/tutorial_general_200.png
[201]: ./media/ideascale-tutorial/tutorial_general_201.png
[202]: ./media/ideascale-tutorial/tutorial_general_202.png
[203]: ./media/ideascale-tutorial/tutorial_general_203.png

