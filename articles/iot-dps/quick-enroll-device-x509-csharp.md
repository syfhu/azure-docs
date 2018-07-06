---
title: Enroll X.509 device to Azure Device Provisioning Service using C# | Microsoft Docs
description: Azure Quickstart - Enroll X.509 device to Azure IoT Hub Device Provisioning Service using C# service SDK
author: bryanla
ms.author: bryanla
ms.date: 01/21/2018
ms.topic: quickstart
ms.service: iot-dps
services: iot-dps 
manager: timlt
ms.devlang: csharp
ms.custom: mvc
---
 
# Enroll X.509 devices to IoT Hub Device Provisioning Service using C# service SDK

[!INCLUDE [iot-dps-selector-quick-enroll-device-x509](../../includes/iot-dps-selector-quick-enroll-device-x509.md)]


These steps show how to programmatically create an enrollment group for an intermediate or root CA X.509 certificate using the [C# Service SDK](https://github.com/Azure/azure-iot-sdk-csharp) and a sample C# .NET Core application. An enrollment group controls access to the provisioning service for devices that share a common signing certificate in their certificate chain. To learn more, see [Controlling device access to the provisioning service with X.509 certificates](./concepts-security.md#controlling-device-access-to-the-provisioning-service-with-x509-certificates). For more information about using X.509 certificate-based Public Key Infrastructure (PKI) with Azure IoT Hub and Device Provisioning Service, see [X.509 CA certificate security overview](https://docs.microsoft.com/azure/iot-hub/iot-hub-x509ca-overview). Although the steps in this article work on both Windows and Linux machines, this article uses a Windows development machine.

## Prepare the development environment

1. Make sure you have [Visual Studio 2017](https://www.visualstudio.com/vs/) installed on your machine. 
2. Make sure you have the [.Net Core SDK](https://www.microsoft.com/net/download/windows) installed on your machine. 
3. Make sure to complete the steps in [Set up the IoT Hub Device Provisioning Service with the Azure portal](./quick-setup-auto-provision.md) before you proceed.
4. You need a .pem or a .cer file that contains the public portion of an intermediate or root CA X.509 certificate that has been uploaded to and verified with your provisioning service. The [Azure IoT C SDK](https://github.com/Azure/azure-iot-sdk-c) contains tooling that can help you create an X.509 certificate chain, upload a root or intermediate certificate from that chain, and perform proof-of-possession with the service to verify the certificate. To use this tooling, download the contents of the [azure-iot-sdk-c/tools/CACertificates](https://github.com/Azure/azure-iot-sdk-c/tree/master/tools/CACertificates) folder to a working folder on your machine and follow the steps in [azure-iot-sdk-c\tools\CACertificates\CACertificateOverview.md](https://github.com/Azure/azure-iot-sdk-c/blob/master/tools/CACertificates/CACertificateOverview.md). In addition to the tooling in the C SDK, the [Group certificate verification sample](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/provisioning/service/samples/GroupCertificateVerificationSample) in the **C# Service SDK** shows how to perform proof-of-possession with an existing X.509 intermediate or root CA certificate. 

  > [!IMPORTANT]
  > The certificates created with the SDK tooling are designed to be used for development only. To learn about obtaining obtaining certificates suitable for production code, see [How to get an X.509 CA certificate](https://docs.microsoft.com/azure/iot-hub/iot-hub-x509ca-overview#how-to-get-an-x509-ca-certificate) in the Azure IoT Hub documentation.

## Get the connection string for your provisioning service

For the sample in this Quickstart, you need the connection string for your provisioning service.
1. Log in to the Azure portal, click on the **All resources** button on the left-hand menu and open your Device Provisioning Service. 
2. Click **Shared access policies**, then click the access policy you want to use to open its properties. In the **Access Policy** window, copy and note down the primary key connection string. 

    ![Get provisioning service connection string from the portal](media/quick-enroll-device-x509-csharp/get-service-connection-string.png)

## Create the enrollment group sample 

The steps in this section show how to create a .NET Core console app that adds an enrollment group to your provisioning service. With some modification, you can also follow these steps to create a [Windows IoT Core](https://developer.microsoft.com/en-us/windows/iot) console app to add the enrollment group. To learn more about developing with IoT Core, see the [Windows IoT Core developer documentation](https://docs.microsoft.com/windows/iot-core/).
1. In Visual Studio, add a Visual C# .NET Core Console App project to a new solution by using the **Console App (.NET Core)** project template. Make sure the .NET Framework version is 4.5.1 or later. Name the project **CreateEnrollmentGroup**.

    ![New Visual C# Windows Classic Desktop project](media//quick-enroll-device-x509-csharp/create-app.png)

2. In Solution Explorer, right-click the **CreateEnrollmentGroup** project, and then click **Manage NuGet Packages**.
3. In the **NuGet Package Manager** window, select **Browse**, search for **Microsoft.Azure.Devices.Provisioning.Service**, select **Install** to install the **Microsoft.Azure.Devices.Provisioning.Service** package, and accept the terms of use. This procedure downloads, installs, and adds a reference to the [Azure IoT Provisioning Service Client SDK](https://www.nuget.org/packages/Microsoft.Azure.Devices.Provisioning.Service/) NuGet package and its dependencies.

    ![NuGet Package Manager window](media//quick-enroll-device-x509-csharp/add-nuget.png)

4. Add the following `using` statements after the other `using` statements at the top of the **Program.cs** file:
   
   ```csharp
   using System.Security.Cryptography.X509Certificates;
   using System.Threading.Tasks;
   using Microsoft.Azure.Devices.Provisioning.Service;
   ```
    
5. Add the following fields to the **Program** class.  
   - Replace the **ProvisioningConnectionString** placeholder value with the connection string of the provisioning service that you want to create the enrollment for.
   - Replace the **X509RootCertPath** placeholder value with the path to a .pem or .cer file that represents the public part of an intermediate or root CA X.509 certificate that has been previously uploaded and verified with your provisioning service.
   - You may optionally change the **EnrollmentGroupId** value. The string can contain only lower case characters and hyphens. 

   > [!IMPORTANT]
   > In production code, be aware of the following security considerations:
   >
   > - Hard-coding the connection string for the provisioning service administrator is against security best practices. Instead, the connection string should be held in a secure manner, such as in a secure configuration file or in the registry.
   > - Be sure to upload only the public part of the signing certificate. Never upload .pfx (PKCS12) or .pem files containing private keys to the provisioning service.
        
   ```csharp
   private static string ProvisioningConnectionString = "{Your provisioning service connection string}";
   private static string EnrollmentGroupId = "enrollmentgrouptest";
   private static string X509RootCertPath = @"{Path to a .cer or .pem file for a verified root CA or intermediate CA X.509 certificate}";
   ```
    
6. Add the following method to the **Program** class. This code creates an enrollment group entry and then calls the **CreateOrUpdateEnrollmentGroupAsync** method on the **ProvisioningServiceClient** to add the enrollment group to the provisioning service.
   
   ```csharp
   public static async Task RunSample()
   {
       Console.WriteLine("Starting sample...");
 
       using (ProvisioningServiceClient provisioningServiceClient =
               ProvisioningServiceClient.CreateFromConnectionString(ProvisioningConnectionString))
       {
           #region Create a new enrollmentGroup config
           Console.WriteLine("\nCreating a new enrollmentGroup...");
           var certificate = new X509Certificate2(X509RootCertPath);
           Attestation attestation = X509Attestation.CreateFromRootCertificates(certificate);
           EnrollmentGroup enrollmentGroup =
                   new EnrollmentGroup(
                           EnrollmentGroupId,
                           attestation)
                   {
                       ProvisioningStatus = ProvisioningStatus.Enabled
                   };
           Console.WriteLine(enrollmentGroup);
           #endregion
 
           #region Create the enrollmentGroup
           Console.WriteLine("\nAdding new enrollmentGroup...");
           EnrollmentGroup enrollmentGroupResult =
               await provisioningServiceClient.CreateOrUpdateEnrollmentGroupAsync(enrollmentGroup).ConfigureAwait(false);
           Console.WriteLine("\nEnrollmentGroup created with success.");
           Console.WriteLine(enrollmentGroupResult);
           #endregion
 
       }
   }
   ```

7. Finally, replace the body of the **Main** method with the following lines:
   
   ```csharp
   RunSample().GetAwaiter().GetResult();
   Console.WriteLine("\nHit <Enter> to exit ...");
   Console.ReadLine();
   ```
        
8. Build the solution.

## Run the enrollment group sample
  
1. Run the sample in Visual Studio to create the enrollment group.
 
2. On successful creation, the command window displays the properties of the new enrollment group.

    ![Enrollment properties in the command output](media/quick-enroll-device-x509-csharp/output.png)

3. To verify that the enrollment group has been created, on the Device Provisioning Service summary blade in the Azure portal, select **Manage enrollments**, then select the **Enrollment Groups** tab. You should see a new enrollment entry that corresponds to the registration ID you used in the sample. Click the entry to verify the certificate thumbprint and other properties for the entry.

    ![Enrollment properties in the portal](media/quick-enroll-device-x509-csharp/verify-enrollment-portal.png)
 
## Clean up resources
If you plan to explore the C# service sample, do not clean up the resources created in this Quickstart. If you do not plan to continue, use the following steps to delete all resources created by this Quickstart.

1. Close the C# sample output window on your machine.
2. Navigate to your Device Provisioning service in the Azure portal, click **Manage enrollments**, and then select the **Enrollment Groups** tab. Select the *Registration ID* for the enrollment entry you created using this Quickstart and click the **Delete** button at the top of the blade.  
3. From your Device Provisioning service in the Azure portal, click **Certificates**, click the certificate you uploaded for this Quickstart, and click the **Delete** button at the top of the **Certificate Details** window.  
 
## Next steps
In this Quickstart, you created an enrollment group for an X.509 intermediate or root CA certificate using the Azure IoT Hub Device Provisioning Service. To learn about device provisioning in depth, continue to the tutorial for the Device Provisioning Service setup in the Azure portal. 
 
> [!div class="nextstepaction"]
> [Azure IoT Hub Device Provisioning Service tutorials](./tutorial-set-up-cloud.md)
