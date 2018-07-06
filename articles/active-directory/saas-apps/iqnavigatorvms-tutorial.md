---
title: 'Tutorial: Azure Active Directory integration with IQNavigator VMS | Microsoft Docs'
description: Learn how to configure single sign-on between Azure Active Directory and IQNavigator VMS.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila

ms.assetid: a8a09b25-dfa5-4c31-aea2-53bf1853b365
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/17/2018
ms.author: jeedes

---
# Tutorial: Azure Active Directory integration with IQNavigator VMS

In this tutorial, you learn how to integrate IQNavigator VMS with Azure Active Directory (Azure AD).

Integrating IQNavigator VMS with Azure AD provides you with the following benefits:

- You can control in Azure AD who has access to IQNavigator VMS
- You can enable your users to automatically get signed-on to IQNavigator VMS (Single Sign-On) with their Azure AD accounts
- You can manage your accounts in one central location - the Azure portal

If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## Prerequisites

To configure Azure AD integration with IQNavigator VMS, you need the following items:

- An Azure AD subscription
- A IQNavigator VMS single sign-on enabled subscription

> [!NOTE]
> To test the steps in this tutorial, we do not recommend using a production environment.

To test the steps in this tutorial, you should follow these recommendations:

- Do not use your production environment, unless it is necessary.
- If you don't have an Azure AD trial environment, you can get a one-month trial here [Trial offer](https://azure.microsoft.com/pricing/free-trial/).

## Scenario description
In this tutorial, you test Azure AD single sign-on in a test environment. 
The scenario outlined in this tutorial consists of two main building blocks:

1. Adding IQNavigator VMS from the gallery
2. Configuring and testing Azure AD single sign-on

## Adding IQNavigator VMS from the gallery
To configure the integration of IQNavigator VMS into Azure AD, you need to add IQNavigator VMS from the gallery to your list of managed SaaS apps.

**To add IQNavigator VMS from the gallery, perform the following steps:**

1. In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon. 

	![Active Directory][1]

2. Navigate to **Enterprise applications**. Then go to **All applications**.

	![Applications][2]
	
3. To add new application, click **New application** button on the top of dialog.

	![Applications][3]

4. In the search box, type **IQNavigator VMS**.

	![Creating an Azure AD test user](./media/iqnavigatorvms-tutorial/tutorial_iqnavigatorvms_search.png)

5. In the results panel, select **IQNavigator VMS**, and then click **Add** button to add the application.

	![Creating an Azure AD test user](./media/iqnavigatorvms-tutorial/tutorial_iqnavigatorvms_addfromgallery.png)

##  Configuring and testing Azure AD single sign-on
In this section, you configure and test Azure AD single sign-on with IQNavigator VMS based on a test user called "Britta Simon".

For single sign-on to work, Azure AD needs to know what the counterpart user in IQNavigator VMS is to a user in Azure AD. In other words, a link relationship between an Azure AD user and the related user in IQNavigator VMS needs to be established.

In IQNavigator VMS, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.

To configure and test Azure AD single sign-on with IQNavigator VMS, you need to complete the following building blocks:

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.
2. **[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.
3. **[Creating a IQNavigator VMS test user](#creating-a-iqnavigator-vms-test-user)** - to have a counterpart of Britta Simon in IQNavigator VMS that is linked to the Azure AD representation of user.
4. **[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.
5. **[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.

### Configuring Azure AD single sign-on

In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your IQNavigator VMS application.

**To configure Azure AD single sign-on with IQNavigator VMS, perform the following steps:**

1. In the Azure portal, on the **IQNavigator VMS** application integration page, click **Single sign-on**.

	![Configure Single Sign-On][4]

2. On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.

	![Configure Single Sign-On](./media/iqnavigatorvms-tutorial/tutorial_iqnavigatorvms_samlbase.png)

3. On the **IQNavigator VMS Domain and URLs** section, perform the following steps:

	![Configure Single Sign-On](./media/iqnavigatorvms-tutorial/tutorial_iqnavigatorvms_url.png)

    a. In the **Identifier** textbox, type the URL:`iqn.com`

	b. In the **Reply URL** textbox, type a URL using the following pattern: `https://<subdomain>.iqnavigator.com/security/login?client_name=https://sts.window.net/<instance name>`

4. Check **Show advanced URL settings**, perform the following step:

    ![Configure Single Sign-On](./media/iqnavigatorvms-tutorial/tutorial_iqnavigatorvms_url1.png)

    In the **Relay state** textbox, type a URL using the following pattern:`https://<subdomain>.iqnavigator.com`

	> [!NOTE]
	> These values are not real. Update these values with the actual Reply URL and Relay state. Contact [IQNavigator VMS Client support team](https://www.beeline.com/iqn-product-support/) to get these values.

5. On the **SAML Signing Certificate** section, click the copy button to copy **App Federation Metadata Url** and paste it into notepad.
    
    ![Configure Single Sign-On](./media/iqnavigatorvms-tutorial/tutorial_metadataurl.png)

6. IQNavigator application expect the unique user identifier value in the Name Identifier claim. Customer can map the correct value for the Name Identifier claim. In this case we have mapped the user.UserPrincipalName for the demo purpose. But according to your organization settings you should map the correct value for it.

	![Configure Single Sign-On](./media/iqnavigatorvms-tutorial/tutorial_iqnavigatorvms_attribute.png)

7. Click **Save** button.

	![Configure Single Sign-On](./media/iqnavigatorvms-tutorial/tutorial_general_400.png)

8. On the **IQNavigator VMS Configuration** section, click **Configure IQNavigator VMS** to open **Configure sign-on** window. Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**

	![Configure Single Sign-On](./media/iqnavigatorvms-tutorial/tutorial_iqnavigatorvms_configure.png)

9. To configure single sign-on on **IQNavigator VMS** side, you need to send the **App Federation Metadata Url**, **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** to [IQNavigator VMS support team](https://www.beeline.com/iqn-product-support/). They set this setting to have the SAML SSO connection set properly on both sides.

### Creating an Azure AD test user
The objective of this section is to create a test user in the Azure portal called Britta Simon.

![Create Azure AD User][100]

**To create a test user in Azure AD, perform the following steps:**

1. In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.

	![Creating an Azure AD test user](./media/iqnavigatorvms-tutorial/create_aaduser_01.png) 

2. To display the list of users, go to **Users and groups** and click **All users**.
	
	![Creating an Azure AD test user](./media/iqnavigatorvms-tutorial/create_aaduser_02.png)

3. To open the **User** dialog, click **Add** on the top of the dialog.

	![Creating an Azure AD test user](./media/iqnavigatorvms-tutorial/create_aaduser_03.png)

4. On the **User** dialog page, perform the following steps:
 
	![Creating an Azure AD test user](./media/iqnavigatorvms-tutorial/create_aaduser_04.png) 

    a. In the **Name** textbox, type **BrittaSimon**.

    b. In the **User name** textbox, type the **email address** of BrittaSimon.

	c. Select **Show Password** and write down the value of the **Password**.

    d. Click **Create**.

### Creating a IQNavigator VMS test user

The objective of this section is to create a user called Britta Simon in IQNavigator VMS. Work with  [IQNavigator VMS support team](https://www.beeline.com/iqn-product-support/) to add the users in the IQNavigator VMS account.

### Assigning the Azure AD test user

In this section, you enable Britta Simon to use Azure single sign-on by granting access to IQNavigator VMS.

![Assign User][200]

**To assign Britta Simon to IQNavigator VMS, perform the following steps:**

1. In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.

	![Assign User][201]

2. In the applications list, select **IQNavigator VMS**.

	![Configure Single Sign-On](./media/iqnavigatorvms-tutorial/tutorial_iqnavigatorvms_app.png)

3. In the menu on the left, click **Users and groups**.

	![Assign User][202]

4. Click **Add** button. Then select **Users and groups** on **Add Assignment** dialog.

	![Assign User][203]

5. On **Users and groups** dialog, select **Britta Simon** in the Users list.

6. Click **Select** button on **Users and groups** dialog.

7. Click **Assign** button on **Add Assignment** dialog.
	
### Testing single sign-on

In this section, you test your Azure AD single sign-on configuration using the Access Panel.

When you click the IQNavigator VMS tile in the Access Panel, you should get automatically signed-on to your IQNavigator VMS application.
For more information about the Access Panel, see [Introduction to the Access Panel](../active-directory-saas-access-panel-introduction.md).

## Additional resources

* [List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/iqnavigatorvms-tutorial/tutorial_general_01.png
[2]: ./media/iqnavigatorvms-tutorial/tutorial_general_02.png
[3]: ./media/iqnavigatorvms-tutorial/tutorial_general_03.png
[4]: ./media/iqnavigatorvms-tutorial/tutorial_general_04.png

[100]: ./media/iqnavigatorvms-tutorial/tutorial_general_100.png

[200]: ./media/iqnavigatorvms-tutorial/tutorial_general_200.png
[201]: ./media/iqnavigatorvms-tutorial/tutorial_general_201.png
[202]: ./media/iqnavigatorvms-tutorial/tutorial_general_202.png
[203]: ./media/iqnavigatorvms-tutorial/tutorial_general_203.png

