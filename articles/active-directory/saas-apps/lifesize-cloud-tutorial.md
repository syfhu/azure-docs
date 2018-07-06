---
title: 'Tutorial: Azure Active Directory integration with Lifesize Cloud | Microsoft Docs'
description: Learn how to configure single sign-on between Azure Active Directory and Lifesize Cloud.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman

ms.assetid: 75fab335-fdcd-4066-b42c-cc738fcb6513
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes

---
# Tutorial: Azure Active Directory integration with Lifesize Cloud

In this tutorial, you learn how to integrate Lifesize Cloud with Azure Active Directory (Azure AD).

Integrating Lifesize Cloud with Azure AD provides you with the following benefits:

- You can control in Azure AD who has access to Lifesize Cloud
- You can enable your users to automatically get signed-on to Lifesize Cloud (Single Sign-On) with their Azure AD accounts
- You can manage your accounts in one central location - the Azure portal

If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## Prerequisites

To configure Azure AD integration with Lifesize Cloud, you need the following items:

- An Azure AD subscription
- A Lifesize Cloud single sign-on enabled subscription

> [!NOTE]
> To test the steps in this tutorial, we do not recommend using a production environment.

To test the steps in this tutorial, you should follow these recommendations:

- Do not use your production environment, unless it is necessary.
- If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).

## Scenario description
In this tutorial, you test Azure AD single sign-on in a test environment. 
The scenario outlined in this tutorial consists of two main building blocks:

1. Adding Lifesize Cloud from the gallery
2. Configuring and testing Azure AD single sign-on

## Adding Lifesize Cloud from the gallery
To configure the integration of Lifesize Cloud into Azure AD, you need to add Lifesize Cloud from the gallery to your list of managed SaaS apps.

**To add Lifesize Cloud from the gallery, perform the following steps:**

1. In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon. 

	![Active Directory][1]

2. Navigate to **Enterprise applications**. Then go to **All applications**.

	![Applications][2]
	
3. To add new application, click **New application** button on the top of dialog.

	![Applications][3]

4. In the search box, type **Lifesize Cloud**.

	![Creating an Azure AD test user](./media/lifesize-cloud-tutorial/tutorial_lifesize-cloud_search.png)

5. In the results panel, select **Lifesize Cloud**, and then click **Add** button to add the application.

	![Creating an Azure AD test user](./media/lifesize-cloud-tutorial/tutorial_lifesize-cloud_addfromgallery.png)

##  Configuring and testing Azure AD single sign-on
In this section, you configure and test Azure AD single sign-on with Lifesize Cloud based on a test user called "Britta Simon".

For single sign-on to work, Azure AD needs to know what the counterpart user in Lifesize Cloud is to a user in Azure AD. In other words, a link relationship between an Azure AD user and the related user in Lifesize Cloud needs to be established.

In Lifesize Cloud, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.

To configure and test Azure AD single sign-on with Lifesize Cloud, you need to complete the following building blocks:

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.
2. **[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.
3. **[Creating a Lifesize Cloud test user](#creating-a-lifesize-cloud-test-user)** - to have a counterpart of Britta Simon in Lifesize Cloud that is linked to the Azure AD representation of user.
4. **[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.
5. **[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.

### Configuring Azure AD single sign-on

In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Lifesize Cloud application.

**To configure Azure AD single sign-on with Lifesize Cloud, perform the following steps:**

1. In the Azure portal, on the **Lifesize Cloud** application integration page, click **Single sign-on**.

	![Configure Single Sign-On][4]

2. On the **Single sign-on** dialog, select **Mode** as	**SAML-based Sign-on** to enable single sign-on.
 
	![Configure Single Sign-On](./media/lifesize-cloud-tutorial/tutorial_lifesize-cloud_samlbase.png)

3. On the **Lifesize Cloud Domain and URLs** section, perform the following steps:

	![Configure Single Sign-On](./media/lifesize-cloud-tutorial/tutorial_lifesize-cloud_url.png)

    a. In the **Sign-on URL** textbox, type a URL using the following pattern: `https://login.lifesizecloud.com/ls/?acs`

	b. In the **Identifier** textbox, type a URL using the following pattern: `https://login.lifesizecloud.com/<companyname>`

	 
4. Check **Show advanced URL settings**, perform the following step:	
   
    ![Configure Single Sign-On](./media/lifesize-cloud-tutorial/tutorial_lifesize-cloud_url1.png)

    In the **Relay state** textbox, type a URL using the following pattern:
   `https://webapp.lifesizecloud.com/?ent=<identifier>`
   
   > [!NOTE] 
   >Please note that these are not the real values. you have to update these values with the actual Sign-On URL, Relay State, and Identifier. Contact [Lifesize Cloud Client support team](https://www.lifesize.com/support) to get Sign-On URL, and Identifier values and you can get Relay State  value from SSO Configuration that is explained later in the tutorial.

4. On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.

	![Configure Single Sign-On](./media/lifesize-cloud-tutorial/tutorial_lifesize-cloud_certificate.png) 

5. Click **Save** button.

	![Configure Single Sign-On](./media/lifesize-cloud-tutorial/tutorial_general_400.png)

6. On the **Lifesize Cloud Configuration** section, click **Configure Lifesize Cloud** to open **Configure sign-on** window. Copy the **SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**

	![Configure Single Sign-On](./media/lifesize-cloud-tutorial/tutorial_lifesize-cloud_configure.png) 

7. To get SSO configured for your application, login into the Lifesize Cloud application with Admin privileges.

8. In the top right corner click on your name and then click on the **Advance Settings**.
   
    ![Configure Single Sign-On](./media/lifesize-cloud-tutorial/tutorial_lifesizecloud_06.png)

9. In the Advance Settings now click on the **SSO Configuration** link. It will open the SSO Configuration page for your instance.
   
    ![Configure Single Sign-On](./media/lifesize-cloud-tutorial/tutorial_lifesizecloud_07.png)

10. Now configure the following values in the SSO configuration UI.    
   
    ![Configure Single Sign-On](./media/lifesize-cloud-tutorial/tutorial_lifesizecloud_08.png)
    
	a. In **Identity Provider Issuer** textbox, paste the value of **SAML Entity ID** which you have copied from Azure portal.

    b.  In **Login URL** textbox, paste the value of **SAML Single Sign-On Service URL** which you have copied from Azure portal.

    c. Open your base-64 encoded certificate in notepad downloaded from Azure portal, copy the content of it into your clipboard, and then paste it to the **X.509 Certificate** textbox.
  
    d. In the SAML Attribute mappings for the First Name text box enter the value as **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**
    
	e. In the SAML Attribute mapping for the **Last Name** text box enter the value as **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**
    
	f. In the SAML Attribute mapping for the **Email** text box enter the value as **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**

11. To check the configuration you can click on the **Test** button.
   
    >[!NOTE]
    >For successful testing you need to complete the configuration wizard in Azure AD and also provide access to users or groups who can perform the test.

12. Enable the SSO by checking on the **Enable SSO** button.

13. Now click on the **Update** button so that all the settings are saved. This will generate the RelayState value. Copy the RelayState value, which is generated in the text box, paste it in the **Relay State** textbox under **Lifesize Cloud Domain and URLs** section. 

> [!TIP]
> You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!  After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom. You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### Creating an Azure AD test user

The objective of this section is to create a test user in the Azure portal called Britta Simon.

![Create Azure AD User][100]

**To create a test user in Azure AD, perform the following steps:**

1. In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.

	![Creating an Azure AD test user](./media/lifesize-cloud-tutorial/create_aaduser_01.png) 

2. To display the list of users, go to **Users and groups** and click **All users**.
	
	![Creating an Azure AD test user](./media/lifesize-cloud-tutorial/create_aaduser_02.png) 

3. To open the **User** dialog, click **Add** on the top of the dialog.
 
	![Creating an Azure AD test user](./media/lifesize-cloud-tutorial/create_aaduser_03.png) 

4. On the **User** dialog page, perform the following steps:
 
	![Creating an Azure AD test user](./media/lifesize-cloud-tutorial/create_aaduser_04.png) 

    a. In the **Name** textbox, type **BrittaSimon**.

    b. In the **User name** textbox, type the **email address** of BrittaSimon.

	c. Select **Show Password** and write down the value of the **Password**.

    d. Click **Create**.
 
### Creating a Lifesize Cloud test user

In this section, you create a user called Britta Simon in Lifesize Cloud. Lifesize cloud does support automatic user provisioning. After successful authentication at Azure AD, the user will be automatically provisioned in the application. 

### Assigning the Azure AD test user

In this section, you enable Britta Simon to use Azure single sign-on by granting access to Lifesize Cloud.

![Assign User][200] 

**To assign Britta Simon to Lifesize Cloud, perform the following steps:**

1. In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.

	![Assign User][201] 

2. In the applications list, select **Lifesize Cloud**.

	![Configure Single Sign-On](./media/lifesize-cloud-tutorial/tutorial_lifesize-cloud_app.png) 

3. In the menu on the left, click **Users and groups**.

	![Assign User][202] 

4. Click **Add** button. Then select **Users and groups** on **Add Assignment** dialog.

	![Assign User][203]

5. On **Users and groups** dialog, select **Britta Simon** in the Users list.

6. Click **Select** button on **Users and groups** dialog.

7. Click **Assign** button on **Add Assignment** dialog.
	
### Testing single sign-on

In this section, you test your Azure AD single sign-on configuration using the Access Panel.

When you click the Lifesize Cloud tile in the Access Panel, you should get login page of Lifesize Cloud application.
For more information about the Access Panel, see [Introduction to the Access Panel](../active-directory-saas-access-panel-introduction.md).

## Additional resources

* [List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/lifesize-cloud-tutorial/tutorial_general_01.png
[2]: ./media/lifesize-cloud-tutorial/tutorial_general_02.png
[3]: ./media/lifesize-cloud-tutorial/tutorial_general_03.png
[4]: ./media/lifesize-cloud-tutorial/tutorial_general_04.png

[100]: ./media/lifesize-cloud-tutorial/tutorial_general_100.png

[200]: ./media/lifesize-cloud-tutorial/tutorial_general_200.png
[201]: ./media/lifesize-cloud-tutorial/tutorial_general_201.png
[202]: ./media/lifesize-cloud-tutorial/tutorial_general_202.png
[203]: ./media/lifesize-cloud-tutorial/tutorial_general_203.png

