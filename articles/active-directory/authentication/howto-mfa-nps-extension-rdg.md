---
title: Remote Desktop Gateway integration with Azure MFA NPS extension | Microsoft Docs
description: Integrate your Remote Desktop Gateway infrastructure with Azure MFA using the Network Policy Server extension for Microsoft Azure

services: multi-factor-authentication
ms.service: active-directory
ms.component: authentication
ms.topic: article
ms.date: 08/15/2017

ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: richagi

---
# Integrate your Remote Desktop Gateway infrastructure using the Network Policy Server (NPS) extension and Azure AD

This article provides details for integrating your Remote Desktop Gateway infrastructure with Azure Multi-Factor Authentication (MFA) using the Network Policy Server (NPS) extension for Microsoft Azure. 

The Network Policy Server (NPS) extension for Azure allows customers to safeguard Remote Authentication Dial-In User Service (RADIUS) client authentication using Azure’s cloud-based [Multi-Factor Authentication (MFA)](multi-factor-authentication.md). This solution provides two-step verification for adding a second layer of security to user sign-ins and transactions.

This article provides step-by-step instructions for integrating the NPS infrastructure with Azure MFA using the NPS extension for Azure. This enables secure verification for users attempting to log on to a Remote Desktop Gateway. 

The Network Policy and Access Services (NPS) gives organizations the ability to do the following:
* Define central locations for the management and control of network requests by specifying who can connect, what times of day connections are allowed, the duration of connections, and the level of security that clients must use to connect, and so on. Rather than specifying these policies on each VPN or Remote Desktop (RD) Gateway server, these policies can be specified once in a central location. The RADIUS protocol provides the centralized Authentication, Authorization, and Accounting (AAA). 
* Establish and enforce Network Access Protection (NAP) client health policies that determine whether devices are granted unrestricted or restricted access to network resources.
* Provide a means to enforce authentication and authorization for access to 802.1x-capable wireless access points and Ethernet switches.    

Typically, organizations use NPS (RADIUS) to simplify and centralize the management of VPN polices. However, many organizations also use NPS to simplify and centralize the management of RD Desktop Connection Authorization Policies (RD CAPs). 

Organizations can also integrate NPS with Azure MFA to enhance security and provide a high level of compliance. This helps ensure that users establish two-step verification to log on to the Remote Desktop Gateway. For users to be granted access, they must provide their username/password combination along with information that the user has in their control. This information must be trusted and not easily duplicated, such as a cell phone number, landline number, application on a mobile device, and so on.

Prior to the availability of the NPS extension for Azure, customers who wished to implement two-step verification for integrated NPS and Azure MFA environments had to configure and maintain a separate MFA Server in the on-premises environment as documented in [Remote Desktop Gateway and Azure Multi-Factor Authentication Server using RADIUS](howto-mfaserver-nps-rdg.md).

The availability of the NPS extension for Azure now gives organizations the choice to deploy either an on-premises based MFA solution or a cloud-based MFA solution to secure RADIUS client authentication.

## Authentication Flow

For users to be granted access to network resources through a Remote Desktop Gateway, they must meet the conditions specified in one RD Connection Authorization Policy (RD CAP) and one RD Resource Authorization Policy (RD RAP). RD CAPs specify who is authorized to connect to RD Gateways. RD RAPs specify the network resources, such as remote desktops or remote apps, that the user is allowed to connect to through the RD Gateway. 

An RD Gateway can be configured to use a central policy store for RD CAPs. RD RAPs cannot use a central policy, as they are processed on the RD Gateway. An example of an RD Gateway configured to use a central policy store for RD CAPs is a RADIUS client to another NPS server that serves as the central policy store.

When the NPS extension for Azure is integrated with the NPS and Remote Desktop Gateway, the successful authentication flow is as follows:

1. The Remote Desktop Gateway server receives an authentication request from a remote desktop user to connect to a resource, such as a Remote Desktop session. Acting as a RADIUS client, the Remote Desktop Gateway server converts the request to a RADIUS Access-Request message and sends the message to the RADIUS (NPS) server where the NPS extension is installed. 
2. The username and password combination is verified in Active Directory and the user is authenticated.
3. If all the conditions as specified in the NPS Connection Request and the Network Policies are met (for example, time of day or group membership restrictions), the NPS extension triggers a request for secondary authentication with Azure MFA. 
4. Azure MFA communicates with Azure AD, retrieves the user’s details, and performs the secondary authentication using the method configured by the user (text message, mobile app, and so on). 
5. Upon success of the MFA challenge, Azure MFA communicates the result to the NPS extension.
6. The NPS server, where the extension is installed, sends a RADIUS Access-Accept message for the RD CAP policy to the Remote Desktop Gateway server.
7. The user is granted access to the requested network resource through the RD Gateway.

## Prerequisites
This section details the prerequisites necessary before integrating Azure MFA with the Remote Desktop Gateway. Before you begin, you must have the following prerequisites in place.  

* Remote Desktop Services (RDS) infrastructure
* Azure MFA License
* Windows Server software
* Network Policy and Access Services (NPS) role
* Azure Active Directory synched with on-premises Active Directory
* Azure Active Directory GUID ID

### Remote Desktop Services (RDS) infrastructure
You must have a working Remote Desktop Services (RDS) infrastructure in place. If you do not, then you can quickly create this infrastructure in Azure using the following quick start template: [Create Remote Desktop Session Collection deployment](https://github.com/Azure/azure-quickstart-templates/tree/ad20c78b36d8e1246f96bb0e7a8741db481f957f/rds-deployment). 

If you wish to manually create an on-premises RDS infrastructure quickly for testing purposes, follow the steps to deploy one. 
**Learn more**: [Deploy RDS with Azure quick start](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/rds-in-azure) and [Basic RDS infrastructure deployment](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/rds-deploy-infrastructure). 

### Azure MFA License
Required is a license for Azure MFA, which is available through an Azure AD Premium, Enterprise Mobility plus Security (EMS), or an MFA subscription. Consumption-based licenses for Azure MFA, such as per user or per authentication licenses, are not compatible with the NPS extension. For more information, see [How to get Azure Multi-Factor Authentication](concept-mfa-licensing.md). For testing purposes, you can use a trial subscription. 

### Windows Server software
The NPS extension requires Windows Server 2008 R2 SP1 or above with the NPS role service installed. All the steps in this section were performed using Windows Server 2016.

### Network Policy and Access Services (NPS) role
The NPS role service provides the RADIUS server and client functionality as well as Network Access Policy health service. This role must be installed on at least two computers in your infrastructure: The Remote Desktop Gateway and another member server or domain controller. By default, the role is already present on the computer configured as the Remote Desktop Gateway.  You must also install the NPS role on at least on another computer, such as a domain controller or member server.

For information on installing the NPS role service Windows Server 2012 or older, see [Install a NAP Health Policy Server](https://technet.microsoft.com/library/dd296890.aspx). For a description of best practices for NPS, including the recommendation to install NPS on a domain controller, see [Best Practices for NPS](https://technet.microsoft.com/library/cc771746).

### Azure Active Directory synched with on-premises Active Directory
To use the NPS extension, on-premises users must be synced with Azure AD and enabled for MFA. This section assumes that on-premises users are synched with Azure AD using AD Connect. For information on Azure AD connect, see [Integrate your on-premises directories with Azure Active Directory](../connect/active-directory-aadconnect.md). 

### Azure Active Directory GUID ID
To install NPS extension, you need to know the GUID of the Azure AD. Instructions for finding the GUID of the Azure AD are provided below.

## Configure Multi-Factor Authentication 
This section provides instructions for integrating Azure MFA with the Remote Desktop Gateway. As an administrator, you must configure the Azure MFA service before users can self-register their multi-factor devices or applications.

Follow the steps in [Getting started with Azure Multi-Factor Authentication in the cloud](howto-mfa-getstarted.md) to enable MFA for your Azure AD users. 

### Configure accounts for two-step verification
Once an account has been enabled for MFA, you cannot sign in to resources governed by the MFA policy until you have successfully configured a trusted device to use for the second authentication factor and have authenticated using two-step verification.

Follow the steps in [What does Azure Multi-Factor Authentication mean for me?](end-user/current/multi-factor-authentication-end-user.md) to understand and properly configure your devices for MFA with your user account.

## Install and configure NPS extension
This section provides instructions for configuring RDS infrastructure to use Azure MFA for client authentication with the Remote Desktop Gateway.

### Acquire Azure Active Directory GUID ID

As part of the configuration of the NPS extension, you need to supply admin credentials and the Azure AD ID for your Azure AD tenant. The following steps show you how to get the tenant ID.

1. Sign in to the [Azure portal](https://portal.azure.com) as the global administrator of the Azure tenant.
2. In the left navigation, select the **Azure Active Directory** icon.
3. Select **Properties**.
4. In the Properties blade, beside the Directory ID, click the **Copy** icon, as shown below, to copy the ID to clipboard.

 ![Properties](./media/howto-mfa-nps-extension-rdg/image1.png)

### Install the NPS extension
Install the NPS extension on a server that has the Network Policy and Access Services (NPS) role installed. This functions as the RADIUS server for your design. 

>[!Important]
> Be sure you do not install the NPS extension on your Remote Desktop Gateway server.
> 

1. Download the [NPS extension](https://aka.ms/npsmfa). 
2. Copy the setup executable file (NpsExtnForAzureMfaInstaller.exe) to the NPS server.
3. On the NPS server, double-click **NpsExtnForAzureMfaInstaller.exe**. If prompted, click **Run**.
4. In the NPS Extension For Azure MFA Setup dialog box, review the software license terms, check **I agree to the license terms and conditions**, and click **Install**.
 
  ![Azure MFA Setup](./media/howto-mfa-nps-extension-rdg/image2.png)

5. In the NPS Extension For Azure MFA Setup dialog box, click **Close**. 

  ![NPS Extension for Azure MFA](./media/howto-mfa-nps-extension-rdg/image3.png)

### Configure certificates for use with the NPS extension using a PowerShell script
Next, you need to configure certificates for use by the NPS extension to ensure secure communications and assurance. The NPS components include a Windows PowerShell script that configures a self-signed certificate for use with NPS. 

The script performs the following actions:

* Creates a self-signed certificate
* Associates public key of certificate to service principal on Azure AD
* Stores the cert in the local machine store
* Grants access to the certificate’s private key to the network user
* Restarts Network Policy Server service

If you want to use your own certificates, you need to associate the public key of your certificate to the service principal on Azure AD, and so on.

To use the script, provide the extension with your Azure AD Admin credentials and the Azure AD tenant ID that you copied earlier. Run the script on each NPS server where you installed the NPS extension. Then do the following:

1. Open an administrative Windows PowerShell prompt.
2. At the PowerShell prompt, type `cd ‘c:\Program Files\Microsoft\AzureMfa\Config’`, and press **ENTER**.
3. Type `.\AzureMfsNpsExtnConfigSetup.ps1`, and press **ENTER**. The script checks to see if the Azure Active Directory PowerShell module is installed. If not installed, the script installs the module for you.

  ![Azure AD PowerShell](./media/howto-mfa-nps-extension-rdg/image4.png)
  
4. After the script verifies the installation of the PowerShell module, it displays the Azure Active Directory PowerShell module dialog box. In the dialog box, enter your Azure AD admin credentials and password, and click **Sign In**.

  ![Open Powershell account](./media/howto-mfa-nps-extension-rdg/image5.png)

5. When prompted, paste the tenant ID you copied to the clipboard earlier, and press **ENTER**.

  ![Enter Tenant ID](./media/howto-mfa-nps-extension-rdg/image6.png)

6. The script creates a self-signed certificate and performs other configuration changes. The output should be like the image shown below.

  ![Self signed certificate](./media/howto-mfa-nps-extension-rdg/image7.png)

## Configure NPS components on Remote Desktop Gateway
In this section, you configure the Remote Desktop Gateway connection authorization policies and other RADIUS settings.

The authentication flow requires that RADIUS messages be exchanged between the Remote Desktop Gateway and the NPS server where the NPS Server is installed. This means that you must configure RADIUS client settings on both Remote Desktop Gateway and the NPS server where the NPS extension is installed. 

### Configure Remote Desktop Gateway connection authorization policies to use central store
Remote Desktop connection authorization policies (RD CAPs) specify the requirements for connecting to a Remote Desktop Gateway server. RD CAPs can be stored locally (default) or they can be stored in a central RD CAP store that is running NPS. To configure integration of Azure MFA with RDS, you need to specify the use of a central store.

1. On the RD Gateway server, open **Server Manager**. 
2. On the menu, click **Tools**, point to **Remote Desktop Services**, and then click **Remote Desktop Gateway Manager**.

  ![Remote Desktop Services](./media/howto-mfa-nps-extension-rdg/image8.png)

3. In the RD Gateway Manger, right-click **\[Server Name\] (Local)**, and click **Properties**.

  ![Server Name](./media/howto-mfa-nps-extension-rdg/image9.png)

4. In the Properties dialog box, select the **RD CAP Store** tab.
5. On the RD CAP Store tab, select **Central Server running NPS**. 
6. In the **Enter a name or IP address for the server running NPS** field, type the IP address or server name of the server where you installed the NPS extension.

  ![Enter Name or IP Address](./media/howto-mfa-nps-extension-rdg/image10.png)
  
7. Click **Add**.
8. In the **Shared Secret** dialog box, enter a shared secret, and then click **OK**. Ensure you record this shared secret and store the record securely.

 >[!NOTE]
 >Shared secret is used to establish trust between the RADIUS servers and clients. Create a long and complex secret.
 >

 ![Shared Secret](./media/howto-mfa-nps-extension-rdg/image11.png)

9. Click **OK** to close the dialog box.

### Configure RADIUS timeout value on Remote Desktop Gateway NPS
To ensure there is time to validate users’ credentials, perform two-step verification, receive responses, and respond to RADIUS messages, it is necessary to adjust the RADIUS timeout value.

1. On the RD Gateway server, open Server Manager. On the menu, click **Tools**, and then click **Network Policy Server**. 
2. In the **NPS (Local)** console, expand **RADIUS Clients and Servers**, and select **Remote RADIUS Server**.

 ![Remote RADIUS Server](./media/howto-mfa-nps-extension-rdg/image12.png)

3. In the details pane, double-click **TS GATEWAY SERVER GROUP**.

 >[!NOTE]
 >This RADIUS Server Group was created when you configured the central server for NPS policies. The RD Gateway forwards RADIUS messages to this server or group of servers, if more than one in the group.
 >

4. In the **TS GATEWAY SERVER GROUP Properties** dialog box, select the IP address or name of the NPS server you configured to store RD CAPs, and then click **Edit**. 

 ![TS Gateway Server Group](./media/howto-mfa-nps-extension-rdg/image13.png)

5. In the **Edit RADIUS Server** dialog box, select the **Load Balancing** tab.
6. In the **Load Balancing** tab, in the **Number of seconds without response before request is considered dropped** field, change the default value from 3 to a value between 30 and 60 seconds.
7. In the **Number of seconds between requests when server is identified as unavailable** field, change the default value of 30 seconds to a value that is equal to or greater than the value you specified in the previous step.

 ![Edit Radius Server](./media/howto-mfa-nps-extension-rdg/image14.png)

8.	Click **OK** two times to close the dialog boxes.

### Verify Connection Request Policies 
By default, when you configure the RD Gateway to use a central policy store for connection authorization policies, the RD Gateway is configured to forward CAP requests to the NPS server. The NPS server with the Azure MFA extension installed, processes the RADIUS access request. The following steps show you how to verify the default connection request policy. 

1. On the RD Gateway, in the NPS (Local) console, expand **Policies**, and select **Connection Request Policies**.
2. Right-click **Connect Request Policies**, and double-click **TS GATEWAY AUTHORIZATION POLICY**.
3. In the **TS GATEWAY AUTHORIZATION POLICY properties** dialog box, click the **Settings** tab.
4. On **Settings** tab, under Forwarding Connection Request, click **Authentication**. RADIUS client is configured to forward requests for authentication.

 ![Authentication Settings](./media/howto-mfa-nps-extension-rdg/image15.png)
 
5. Click **Cancel**. 

## Configure NPS on the server where the NPS extension is installed
The NPS server where the NPS extension is installed needs to be able to exchange RADIUS messages with the NPS server on the Remote Desktop Gateway. To enable this message exchange, you need to configure the NPS components on the server where the NPS extension service is installed. 

### Register Server in Active Directory
To function properly in this scenario, the NPS server needs to be registered in Active Directory.

1. On the NPS server, open **Server Manager**.
2. In Server Manager, click **Tools**, and then click **Network Policy Server**. 
3. In the Network Policy Server console, right-click **NPS (Local)**, and then click **Register server in Active Directory**. 
4. Click **OK** two times.

 ![Register server in AD](./media/howto-mfa-nps-extension-rdg/image16.png)

5. Leave the console open for the next procedure.

### Create and configure RADIUS client 
The Remote Desktop Gateway needs to be configured as a RADIUS client to the NPS server. 

1. On the NPS server where the NPS extension is installed, in the **NPS (Local)** console, right-click **RADIUS Clients** and click **New**.

 ![New RADIUS Clients](./media/howto-mfa-nps-extension-rdg/image17.png)

2. In the **New RADIUS Client** dialog box, provide a friendly name, such as _Gateway_, and the IP address or DNS name of the Remote Desktop Gateway server. 
3. In the **Shared secret** and the **Confirm shared secret** fields, enter the same secret that you used before.

 ![Name and Address](./media/howto-mfa-nps-extension-rdg/image18.png)

4. Click **OK** to close the New RADIUS Client dialog box.

### Configure Network Policy
Recall that the NPS server with the Azure MFA extension is the designated central policy store for the Connection Authorization Policy (CAP). Therefore, you need to implement a CAP on the NPS server to authorize valid connections requests.  

1. In the NPS (Local) console, expand **Policies**, and click **Network Policies**.
2. Right-click **Connections to other access servers**, and click **Duplicate policy**. 

 ![Duplicate Policy](./media/howto-mfa-nps-extension-rdg/image19.png)

3. Right-click **Copy of Connections to other access servers**, and click **Properties**.

 ![Network Properties](./media/howto-mfa-nps-extension-rdg/image20.png)

4. In the **Copy of Connections to other access servers** dialog box, in **Policy Name**, enter a suitable name, such as _RDG_CAP_. Check **Policy enabled**, and select **Grant access**. Optionally, in **Type of network access server**, select **Remote Desktop Gateway**, or you can leave it as **Unspecified**.

 ![Copy of Connections](./media/howto-mfa-nps-extension-rdg/image21.png)

5.	Click the **Constraints** tab, and check **Allow clients to connect without negotiating an authentication method**.

 ![Allow clients to connect](./media/howto-mfa-nps-extension-rdg/image22.png)

6. Optionally, click the **Conditions** tab and add conditions that must be met for the connection to be authorized, for example, membership in a specific Windows group.

 ![Conditions](./media/howto-mfa-nps-extension-rdg/image23.png)

7. Click **OK**. When prompted to view the corresponding Help topic, click **No**.
8. Ensure that your new policy is at the top of the list, that the policy is enabled, and that it grants access.

 ![Network Policies](./media/howto-mfa-nps-extension-rdg/image24.png)

## Verify configuration
To verify the configuration, you need to log on to the Remote Desktop Gateway with a suitable RDP client. Be sure to use an account that is allowed by your Connection Authorization Policies and is enabled for Azure MFA. 

As show in the image below, you can use the **Remote Desktop Web Access** page.

![Remote Desktop Web Access](./media/howto-mfa-nps-extension-rdg/image25.png)

Upon successfully entering your credentials for primary authentication, the Remote Desktop Connect dialog box shows a status of Initiating remote connection, as shown below. 

If you successfully authenticate with the secondary authentication method you previously configured in Azure MFA, you are connected to the resource. However, if the secondary authentication is not successful, you are denied access to the resource. 

![Initiate remote connection](./media/howto-mfa-nps-extension-rdg/image26.png)

In the example below, the Authenticator app on a Windows phone is used to provide the secondary authentication.

![Accounts](./media/howto-mfa-nps-extension-rdg/image27.png)

Once you have successfully authenticated using the secondary authentication method, you are logged into the Remote Desktop Gateway as normal. However, because you are required to use a secondary authentication method using a mobile app on a trusted device, the log-in process is more secure than it would be otherwise.

### View Event Viewer logs for successful logon events
To view the successful sign-in events in the Windows Event Viewer logs, you can issue the following Windows PowerShell command to query the Windows Terminal Services and Windows Security logs.

To query successful sign-in events in the Gateway operational logs _(Event Viewer\Applications and Services Logs\Microsoft\Windows\TerminalServices-Gateway\Operational)_, use the following PowerShell commands:

* _Get-WinEvent -Logname Microsoft-Windows-TerminalServices-Gateway/Operational_ | where {$_.ID -eq '300'} | FL 
* This command displays Windows events that show the user met resource authorization policy requirements (RD RAP) and was granted access.

![View Event Viewer](./media/howto-mfa-nps-extension-rdg/image28.png)

* _Get-WinEvent -Logname Microsoft-Windows-TerminalServices-Gateway/Operational_ | where {$_.ID -eq '200'} | FL
* This command displays the events that show when user met connection authorization policy requirements.

![Connection authorization](./media/howto-mfa-nps-extension-rdg/image29.png)

You can also view this log and filter on event IDs, 300 and 200. To query successful logon events in the Security event viewer logs, use the following command:

* _Get-WinEvent -Logname Security_ | where {$_.ID -eq '6272'} | FL 
* This command can be run on either the central NPS or the RD Gateway Server. 

![Successful logon events](./media/howto-mfa-nps-extension-rdg/image30.png)

You can also view the Security log or the Network Policy and Access Services custom view, as shown below:

![Network Policy and Access Services](./media/howto-mfa-nps-extension-rdg/image31.png)

On the server where you installed the NPS extension for Azure MFA, you can find Event Viewer application logs specific to the extension at _Application and Services Logs\Microsoft\AzureMfa_. 

![Event Viewer application logs](./media/howto-mfa-nps-extension-rdg/image32.png)

## Troubleshoot Guide

If the configuration is not working as expected, the first place to start to troubleshoot is to verify that the user is configured to use Azure MFA. Have the user connect to the [Azure portal](https://portal.azure.com). If users are prompted for secondary verification and can successfully authenticate, you can eliminate an incorrect configuration of Azure MFA.

If Azure MFA is working for the user(s), you should review the relevant Event logs. These include the Security Event, Gateway operational, and Azure MFA logs that are discussed in the previous section. 

Below is an example output of Security log showing a failed logon event (Event ID 6273).

![Failed logon event](./media/howto-mfa-nps-extension-rdg/image33.png)

Below is a related event from the AzureMFA logs:

![Azure MFA log](./media/howto-mfa-nps-extension-rdg/image34.png)

To perform advanced troubleshoot options, consult the NPS database format log files where the NPS service is installed. These log files are created in _%SystemRoot%\System32\Logs_ folder as comma-delimited text files. 

For a description of these log files, see [Interpret NPS Database Format Log Files](https://technet.microsoft.com/library/cc771748.aspx). The entries in these log files can be difficult to interpret without importing them into a spreadsheet or a database. You can find several IAS parsers online to assist you in interpreting the log files. 

The image below shows the output of one such downloadable [shareware application](http://www.deepsoftware.com/iasviewer). 

![Shareware app](./media/howto-mfa-nps-extension-rdg/image35.png)

Finally, for additional troubleshoot options, you can use a protocol analyzer, such [Microsoft Message Analyzer](https://technet.microsoft.com/library/jj649776.aspx). 

The image below from Microsoft Message Analyzer shows network traffic filtered on RADIUS protocol that contains the user name **Contoso\AliceC**.

![Microsoft Message Analyzer](./media/howto-mfa-nps-extension-rdg/image36.png)

## Next steps
[How to get Azure Multi-Factor Authentication](concept-mfa-licensing.md)

[Remote Desktop Gateway and Azure Multi-Factor Authentication Server using RADIUS](howto-mfaserver-nps-rdg.md)

[Integrate your on-premises directories with Azure Active Directory](../connect/active-directory-aadconnect.md)
