---
title: 'Tutorial: Azure Active Directory integration with Humanity | Microsoft Docs'
description: Learn how to configure single sign-on between Azure Active Directory and Humanity.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman

ms.assetid: 6aa771e9-31c6-48d1-8dde-024bebc06943
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/10/2017
ms.author: jeedes

---
# Tutorial: Azure Active Directory integration with Humanity

In this tutorial, you learn how to integrate Humanity with Azure Active Directory (Azure AD).

Integrating Humanity with Azure AD provides you with the following benefits:

- You can control in Azure AD who has access to Humanity
- You can enable your users to automatically get signed-on to Humanity (Single Sign-On) with their Azure AD accounts
- You can manage your accounts in one central location - the Azure portal

If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## Prerequisites

To configure Azure AD integration with Humanity, you need the following items:

- An Azure AD subscription
- A Humanity single-sign on enabled subscription

> [!NOTE]
> To test the steps in this tutorial, we do not recommend using a production environment.

To test the steps in this tutorial, you should follow these recommendations:

- Do not use your production environment, unless it is necessary.
- If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).

## Scenario description
In this tutorial, you test Azure AD single sign-on in a test environment. 
The scenario outlined in this tutorial consists of two main building blocks:

1. Adding Humanity from the gallery
2. Configuring and testing Azure AD single sign-on

## Adding Humanity from the gallery
To configure the integration of Humanity into Azure AD, you need to add Humanity from the gallery to your list of managed SaaS apps.

**To add Humanity from the gallery, perform the following steps:**

1. In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon. 

	![Active Directory][1]

2. Navigate to **Enterprise applications**. Then go to **All applications**.

	![Applications][2]
	
3. To add new application, click **New application** button on the top of dialog.

	![Applications][3]

4. In the search box, type **Humanity**.

	![Creating an Azure AD test user](./media/shiftplanning-tutorial/tutorial_humanity_search.png)

5. In the results panel, select **Humanity**, and then click **Add** button to add the application.

	![Creating an Azure AD test user](./media/shiftplanning-tutorial/tutorial_humanity_addfromgallery.png)

##  Configuring and testing Azure AD single sign-on
In this section, you configure and test Azure AD single sign-on with Humanity based on a test user called "Britta Simon."

For single sign-on to work, Azure AD needs to know what the counterpart user in Humanity is to a user in Azure AD. In other words, a link relationship between an Azure AD user and the related user in Humanity needs to be established.

In Humanity, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.

To configure and test Azure AD single sign-on with Humanity, you need to complete the following building blocks:

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.
2. **[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.
3. **[Creating a Humanity test user](#creating-a-humanity-test-user)** - to have a counterpart of Britta Simon in Humanity that is linked to the Azure AD representation of user.
4. **[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.
5. **[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.

### Configuring Azure AD single sign-on

In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Humanity application.

**To configure Azure AD single sign-on with Humanity, perform the following steps:**

1. In the Azure portal, on the **Humanity** application integration page, click **Single sign-on**.

	![Configure Single Sign-On][4]

2. On the **Single sign-on** dialog, select **Mode** as	**SAML-based Sign-on** to enable single sign-on.
 
	![Configure Single Sign-On](./media/shiftplanning-tutorial/tutorial_humanity_samlbase.png)

3. On the **Humanity Domain and URLs** section, perform the following steps:

	![Configure Single Sign-On](./media/shiftplanning-tutorial/tutorial_humanity_url.png)

    a. In the **Sign-on URL** textbox, type a URL using the following pattern: `https://company.humanity.com/includes/saml/`

	b. In the **Identifier** textbox, type a URL using the following pattern: `https://company.humanity.com/app/`

	> [!NOTE] 
	> These values are not real. Update these values with the actual Sign-On URL and Identifier. Contact [Humanity Client support team](https://www.humanity.com/support/) to get these values. 
 
4. On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.

	![Configure Single Sign-On](./media/shiftplanning-tutorial/tutorial_humanity_certificate.png) 

5. Click **Save** button.

	![Configure Single Sign-On](./media/shiftplanning-tutorial/tutorial_general_400.png)

6. On the **Humanity Configuration** section, click **Configure Humanity** to open **Configure sign-on** window. Copy the **SAML Single Sign-On Service URL, and Sign-Out URL** from the **Quick Reference section.**

	![Configure Single Sign-On](./media/shiftplanning-tutorial/tutorial_humanity_configure.png) 

7. In a different web browser window, log in to your **Humanity** company site as an administrator.

8. In the menu on the top, click **Admin**.
   
    ![Admin](./media/shiftplanning-tutorial/iC786619.png "Admin")

9. Under **Integration**, click **Single Sign-On**.
   
    ![Single Sign-On](./media/shiftplanning-tutorial/iC786620.png "Single Sign-On")

10. In the **Single Sign-On** section, perform the following steps:
   
    ![Single Sign-On](./media/shiftplanning-tutorial/iC786905.png "Single Sign-On")
   
    a. Select **SAML Enabled**.

    b. Select **Allow Password Login**.

    c. Paste the **SAML Single Sign-On Service URL** value into the **SAML Issuer URL** textbox.

    d. Paste the **Sign-Out URL** value into the **Remote Logout URL** textbox.
   
    e. Open your base-64 encoded certificate in notepad, copy the content of it into your clipboard, and then paste it to the **X.509 Certificate** textbox.

11. Click **Save Settings**.

> [!TIP]
> You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!  After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom. You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### Creating an Azure AD test user
The objective of this section is to create a test user in the Azure portal called Britta Simon.

![Create Azure AD User][100]

**To create a test user in Azure AD, perform the following steps:**

1. In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.

	![Creating an Azure AD test user](./media/shiftplanning-tutorial/create_aaduser_01.png) 

2. To display the list of users, go to **Users and groups** and click **All users**.
	
	![Creating an Azure AD test user](./media/shiftplanning-tutorial/create_aaduser_02.png) 

3. To open the **User** dialog, click **Add** on the top of the dialog.
 
	![Creating an Azure AD test user](./media/shiftplanning-tutorial/create_aaduser_03.png) 

4. On the **User** dialog page, perform the following steps:
 
	![Creating an Azure AD test user](./media/shiftplanning-tutorial/create_aaduser_04.png) 

    a. In the **Name** textbox, type **BrittaSimon**.

    b. In the **User name** textbox, type the **email address** of BrittaSimon.

	c. Select **Show Password** and write down the value of the **Password**.

    d. Click **Create**.
 
### Creating a Humanity test user

In order to enable Azure AD users to log in to Humanity, they must be provisioned into Humanity. In the case of Humanity, provisioning is a manual task.

**To provision a user account, perform the following steps:**

1. Log in to your **Humanity** company site as an administrator.

2. Click **Admin**.
   
    ![Admin](./media/shiftplanning-tutorial/iC786619.png "Admin")

3. Click **Staff**.
   
    ![Staff](./media/shiftplanning-tutorial/ic786623.png "Staff")

4. Under **Actions**, click **Add Employees**.
   
    ![Add Employees](./media/shiftplanning-tutorial/iC786624.png "Add Employees")

5. In the **Add Employees** section, perform the following steps:
   
    ![Save Employees](./media/shiftplanning-tutorial/iC786625.png "Save Employees")
   
    a. Type the **First Name**, **Last Name**, and **Email** of a valid AAD account you want to provision into the related textboxes.

    b. Click **Save Employees**.

>[!NOTE]
>You can use any other Humanity user account creation tools or APIs provided by Humanity to provision AAD user accounts.

### Assigning the Azure AD test user

In this section, you enable Britta Simon to use Azure single sign-on by granting access to Humanity.

![Assign User][200] 

**To assign Britta Simon to Humanity, perform the following steps:**

1. In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.

	![Assign User][201] 

2. In the applications list, select **Humanity**.

	![Configure Single Sign-On](./media/shiftplanning-tutorial/tutorial_humanity_app.png) 

3. In the menu on the left, click **Users and groups**.

	![Assign User][202] 

4. Click **Add** button. Then select **Users and groups** on **Add Assignment** dialog.

	![Assign User][203]

5. On **Users and groups** dialog, select **Britta Simon** in the Users list.

6. Click **Select** button on **Users and groups** dialog.

7. Click **Assign** button on **Add Assignment** dialog.
	
### Testing single sign-on

In this section, you test your Azure AD single sign-on configuration using the Access Panel.

When you click the Humanity tile in the Access Panel, you should get automatically signed-on to your Humanity application.
For more information about the Access Panel, see [Introduction to the Access Panel](../active-directory-saas-access-panel-introduction.md).

## Additional resources

* [List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/shiftplanning-tutorial/tutorial_general_01.png
[2]: ./media/shiftplanning-tutorial/tutorial_general_02.png
[3]: ./media/shiftplanning-tutorial/tutorial_general_03.png
[4]: ./media/shiftplanning-tutorial/tutorial_general_04.png

[100]: ./media/shiftplanning-tutorial/tutorial_general_100.png

[200]: ./media/shiftplanning-tutorial/tutorial_general_200.png
[201]: ./media/shiftplanning-tutorial/tutorial_general_201.png
[202]: ./media/shiftplanning-tutorial/tutorial_general_202.png
[203]: ./media/shiftplanning-tutorial/tutorial_general_203.png

