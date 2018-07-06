---
title: 'Tutorial: Azure Active Directory integration with TeamSeer | Microsoft Docs'
description: Learn how to configure single sign-on between Azure Active Directory and TeamSeer.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman

ms.assetid: 6ec4806f-fe0f-4ed7-8cfa-32d1c840433f
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: jeedes

---
# Tutorial: Azure Active Directory integration with TeamSeer

In this tutorial, you learn how to integrate TeamSeer with Azure Active Directory (Azure AD).

Integrating TeamSeer with Azure AD provides you with the following benefits:

- You can control in Azure AD who has access to TeamSeer
- You can enable your users to automatically get signed-on to TeamSeer (Single Sign-On) with their Azure AD accounts
- You can manage your accounts in one central location - the Azure portal

If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## Prerequisites

To configure Azure AD integration with TeamSeer, you need the following items:

- An Azure AD subscription
- A TeamSeer single-sign on enabled subscription

> [!NOTE]
> To test the steps in this tutorial, we do not recommend using a production environment.

To test the steps in this tutorial, you should follow these recommendations:

- Do not use your production environment, unless it is necessary.
- If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).

## Scenario description
In this tutorial, you test Azure AD single sign-on in a test environment. 
The scenario outlined in this tutorial consists of two main building blocks:

1. Adding TeamSeer from the gallery
2. Configuring and testing Azure AD single sign-on

## Adding TeamSeer from the gallery
To configure the integration of TeamSeer in to Azure AD, you need to add TeamSeer from the gallery to your list of managed SaaS apps.

**To add TeamSeer from the gallery, perform the following steps:**

1. In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon. 

	![Active Directory][1]

2. Navigate to **Enterprise applications**. Then go to **All applications**.

	![Applications][2]
	
3. To add new application, click **New application** button on the top of dialog.

	![Applications][3]

4. In the search box, type **TeamSeer**.

	![Creating an Azure AD test user](./media/teamseer-tutorial/tutorial_teamseer_search.png)

5. In the results panel, select **TeamSeer**, and then click **Add** button to add the application.

	![Creating an Azure AD test user](./media/teamseer-tutorial/tutorial_teamseer_addfromgallery.png)

##  Configuring and testing Azure AD single sign-on
In this section, you configure and test Azure AD single sign-on with TeamSeer based on a test user called "Britta Simon."

For single sign-on to work, Azure AD needs to know what the counterpart user in TeamSeer is to a user in Azure AD. In other words, a link relationship between an Azure AD user and the related user in TeamSeer needs to be established.

In TeamSeer, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.

To configure and test Azure AD single sign-on with TeamSeer, you need to complete the following building blocks:

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.
2. **[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.
3. **[Creating a TeamSeer test user](#creating-a-teamseer-test-user)** - to have a counterpart of Britta Simon in TeamSeer that is linked to the Azure AD representation of user.
4. **[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.
5. **[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.

### Configuring Azure AD single sign-on

In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your TeamSeer application.

**To configure Azure AD single sign-on with TeamSeer, perform the following steps:**

1. In the Azure portal, on the **TeamSeer** application integration page, click **Single sign-on**.

	![Configure Single Sign-On][4]

2. On the **Single sign-on** dialog, select **Mode** as	**SAML-based Sign-on** to enable single sign-on.
 
	![Configure Single Sign-On](./media/teamseer-tutorial/tutorial_teamseer_samlbase.png)

3. On the **TeamSeer Domain and URLs** section, perform the following steps:

	![Configure Single Sign-On](./media/teamseer-tutorial/tutorial_teamseer_url.png)

     In the **Sign-on URL** textbox, type a URL using the following pattern: `https://www.teamseer.com/<companyid>`

	> [!NOTE] 
	> The value is not real. Update the value with the actual Sign-On URL. Contact [TeamSeer Client support team](http://pages.theaccessgroup.com/solutions_business-suite_absence-management_contact.html) to get the value. 
 
4. On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.

	![Configure Single Sign-On](./media/teamseer-tutorial/tutorial_teamseer_certificate.png) 

5. Click **Save** button.

	![Configure Single Sign-On](./media/teamseer-tutorial/tutorial_general_400.png)

6. On the **TeamSeer Configuration** section, click **Configure TeamSeer** to open **Configure sign-on** window. Copy the **SAML Single Sign-On Service URL** from the **Quick Reference section.**

	![Configure Single Sign-On](./media/teamseer-tutorial/tutorial_teamseer_configure.png)

7. In a different web browser window, log in to your TeamSeer company site as an administrator.

8. Go to **HR Admin**.
   
    ![HR Admin](./media/teamseer-tutorial/ic789634.png "HR Admin")

9. Click **Setup**.
   
    ![Setup](./media/teamseer-tutorial/ic789635.png "Setup")

10. Click **Set up SAML provider details**.
   
    ![SAML Settings](./media/teamseer-tutorial/ic789636.png "SAML Settings")

11. In the SAML provider details section, perform the following steps:
   
    ![SAML Settings](./media/teamseer-tutorial/ic789637.png "SAML Settings")   

    a. Paste the **Single Sign-On Service URL** value in to the **URL** textbox.
          
    b. Open your base-64 encoded certificate in notepad, copy the content of it in to your clipboard, and then paste it to the **IdP Public Certificate** textbox.

12. To complete the SAML provider configuration, perform the following steps:
    
    ![SAML Settings](./media/teamseer-tutorial/ic789638.png "SAML Settings") 

    a. In the **Test Email Addresses**, type the test user’s email address. 
  
    b. In the **Issuer** textbox, type the Issuer URL of the service provider. 
  
    c. Click **Save**.

> [!TIP]
> You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!  After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom. You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### Creating an Azure AD test user
The objective of this section is to create a test user in the Azure portal called Britta Simon.

![Create Azure AD User][100]

**To create a test user in Azure AD, perform the following steps:**

1. In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.

	![Creating an Azure AD test user](./media/teamseer-tutorial/create_aaduser_01.png) 

2. To display the list of users, go to **Users and groups** and click **All users**.
	
	![Creating an Azure AD test user](./media/teamseer-tutorial/create_aaduser_02.png) 

3. To open the **User** dialog, click **Add** on the top of the dialog.
 
	![Creating an Azure AD test user](./media/teamseer-tutorial/create_aaduser_03.png) 

4. On the **User** dialog page, perform the following steps:
 
	![Creating an Azure AD test user](./media/teamseer-tutorial/create_aaduser_04.png) 

    a. In the **Name** textbox, type **BrittaSimon**.

    b. In the **User name** textbox, type the **email address** of BrittaSimon.

	c. Select **Show Password** and write down the value of the **Password**.

    d. Click **Create**.
 
### Creating a TeamSeer test user

To enable Azure AD users to log in to TeamSeer, they must be provisioned in to ShiftPlanning. In the case of TeamSeer, provisioning is a manual task.

**To provision a user account, perform the following steps:**

1. Log in to your **TeamSeer** company site as an administrator.

2. Perform the following steps:
   
    ![HR Admin](./media/teamseer-tutorial/ic789640.png "HR Admin")  
 
    a. Go to **HR Admin \> Users**.
  
    b. Click **Run the New User wizard**.

3. In the **User Details** section, perform the following steps:
   
    ![User Details](./media/teamseer-tutorial/ic789641.png "User Details")

    a. Type the **First Name**, **Surname**, **User name (Email address)** of a valid AAD account you want to provision in to the related textboxes.
  
    b. Click **Next**.

4. Follow the on-screen instructions for adding a new user, and click **Finish**.

>[!NOTE]
>You can use any other TeamSeer user account creation tools or APIs provided by TeamSeer to provision Azure AD user accounts. 

### Assigning the Azure AD test user

In this section, you enable Britta Simon to use Azure single sign-on by granting access to TeamSeer.

![Assign User][200] 

**To assign Britta Simon to TeamSeer, perform the following steps:**

1. In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.

	![Assign User][201] 

2. In the applications list, select **TeamSeer**.

	![Configure Single Sign-On](./media/teamseer-tutorial/tutorial_teamseer_app.png) 

3. In the menu on the left, click **Users and groups**.

	![Assign User][202] 

4. Click **Add** button. Then select **Users and groups** on **Add Assignment** dialog.

	![Assign User][203]

5. On **Users and groups** dialog, select **Britta Simon** in the Users list.

6. Click **Select** button on **Users and groups** dialog.

7. Click **Assign** button on **Add Assignment** dialog.
	
### Testing single sign-on

If you want to test your single sign-on settings, open the Access Panel. For more details about the Access Panel, see [Introduction to the Access Panel](../active-directory-saas-access-panel-introduction.md).

## Additional resources

* [List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/teamseer-tutorial/tutorial_general_01.png
[2]: ./media/teamseer-tutorial/tutorial_general_02.png
[3]: ./media/teamseer-tutorial/tutorial_general_03.png
[4]: ./media/teamseer-tutorial/tutorial_general_04.png

[100]: ./media/teamseer-tutorial/tutorial_general_100.png

[200]: ./media/teamseer-tutorial/tutorial_general_200.png
[201]: ./media/teamseer-tutorial/tutorial_general_201.png
[202]: ./media/teamseer-tutorial/tutorial_general_202.png
[203]: ./media/teamseer-tutorial/tutorial_general_203.png

