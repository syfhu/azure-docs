---
title: Azure Service Fabric plug-in for Eclipse | Microsoft Docs
description:  Get started with the Service Fabric plug-in for Eclipse.  
services: service-fabric
documentationcenter: java
author: rapatchi
manager: timlt
editor: ''

ms.assetid: bf84458f-4b87-4de1-9844-19909e368deb
ms.service: service-fabric
ms.devlang: java
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/06/2018
ms.author: rapatchi

---

# Service Fabric plug-in for Eclipse Java application development
Eclipse is one of the most widely used integrated development environments (IDEs) for Java developers. In this article, we describe how to set up your Eclipse development environment to work with Azure Service Fabric. Learn how to install the Service Fabric plug-in, create a Service Fabric application, and deploy your Service Fabric application to a local or remote Service Fabric cluster in Eclipse. 

> [!NOTE]
> The Eclipse plugin is currently not supported on Windows. 

## Install or update the Service Fabric plug-in in Eclipse
You can install a Service Fabric plug-in in Eclipse. The plug-in can help simplify the process of building and deploying Java services.

> [!IMPORTANT]
> The  Service Fabric plug-in requires Eclipse Neon or a later version. See the instructions that follow this note for how to check your version of Eclipse. If you have an earlier version of Eclipse installed, you can download more recent versions from the [Eclipse site](https://www.eclipse.org). It is not recommended that you install on top of (overwrite) an existing installation of Eclipse. You can either remove it before running the installer or install the newer version in a different directory. 
> 
> On Ubuntu, we recommend installing directly from the Eclipse site rather than using a package installer (`apt` or `apt-get`). Doing so ensures that you get the most current version of Eclipse. 

Install Eclipse Neon or later from the [Eclipse site](https://www.eclipse.org).  Also install version 2.2.1 or later of Buildship (the Service Fabric plug-in is not compatible with older versions of Buildship):
-   To check the versions of installed components, in Eclipse, go to **Help** > **About Eclipse** > **Installation Details**.
-   To update Buildship, see [Eclipse Buildship: Eclipse Plug-ins for Gradle][buildship-update].
-   To check for and install updates for Eclipse, go to **Help** > **Check for Updates**.

Install the Service Fabric plug-in, in Eclipse, go to **Help** > **Install New Software**.
1. In the **Work with** box, enter **http://dl.microsoft.com/eclipse**.
2. Click **Add**.
    ![Service Fabric plug-in for Eclipse][sf-eclipse-plugin-install]
3. Select the Service Fabric plug-in, and then click **Next**.
4. Complete the installation steps, and then accept the Microsoft Software License Terms.
  
If you already have the Service Fabric plug-in installed, install the latest version. 
1. To check for available updates, go to **Help** > **About Eclipse** > **Installation Details**. 
2. In the list of installed plug-ins, select Service Fabric, and then click **Update**. Available updates will be installed.
3. Once you update the Service Fabric plug-in, also refresh the Gradle project.  Right click **build.gradle**, then select **Refresh**.

> [!NOTE]
> If installing or updating the Service Fabric plug-in is slow, it might be due to an Eclipse setting. Eclipse collects metadata on all changes to update sites that are registered with your Eclipse instance. To speed up the process of checking for and installing a Service Fabric plug-in update, go to **Available Software Sites**. Clear the check boxes for all sites except for the one that points to the Service Fabric plug-in location (http://dl.microsoft.com/eclipse/azure/servicefabric).

> [!NOTE]
>If Eclipse isn't working as expected on your Mac, or needs you run as super user), go to the **ECLIPSE_INSTALLATION_PATH** folder and navigate to the subfolder **Eclipse.app/Contents/MacOS**. Start Eclipse by running `./eclipse`.


## Create a Service Fabric application in Eclipse

1.  In Eclipse, go to **File** > **New** > **Other**. Select  **Service Fabric Project**, and then click **Next**.

    ![Service Fabric New Project page 1][create-application/p1]

2.  Enter a name for your project, and then click **Next**.

    ![Service Fabric New Project page 2][create-application/p2]

3.  In the list of templates, select **Service Template**. Select your service template type (Actor, Stateless, Container, or Guest Binary), and then click **Next**.

    ![Service Fabric New Project page 3][create-application/p3]

4.  Enter the service name and service details, and then click **Finish**.

    ![Service Fabric New Project page 4][create-application/p4]

5. When you create your first Service Fabric project, in the **Open Associated Perspective** dialog box, click **Yes**.

    ![Service Fabric New Project page 5][create-application/p5]

6.  Your new project looks like this:

    ![Service Fabric New Project page 6][create-application/p6]

## Build and deploy a Service Fabric application in Eclipse

1.  Right-click your new Service Fabric application, and then select **Service Fabric**.

    ![Service Fabric right-click menu][publish/RightClick]

2. In the submenu, select the option you want:
    -   To build the application without cleaning, click **Build Application**.
    -   To do a clean build of the application, click **Rebuild Application**.
    -   To clean the application of built artifacts, click **Clean Application**.

3.  From this menu, you also can deploy, undeploy, and publish your application:
    -   To deploy to your local cluster, click **Deploy Application**.
    -   In the **Publish Application** dialog box, select a publish profile:
        -  **Local.json**
        -  **Cloud.json**

     These JavaScript Object Notation (JSON) files store information (such as connection endpoints and security information) that is required to connect to your local or cloud (Azure) cluster.

  ![Service Fabric Publish menu][publish/Publish]

An alternate way to deploy your Service Fabric application is by using Eclipse run configurations.

  1.    Go to **Run** > **Run Configurations**.
  2.    Under **Gradle Project**, select the **ServiceFabricDeployer** run configuration.
  3.    In the right pane, on the **Arguments** tab, for **publishProfile**, select **local** or **cloud**.  The default is **local**. To deploy to a remote or cloud cluster, select **cloud**.
  4.    To ensure that the proper information is populated in the publish profiles, edit **Local.json** or **Cloud.json** as needed. You can add or update endpoint details and security credentials.
  5.    Ensure that **Working Directory** points to the application you want to deploy. To change the application, click the **Workspace** button, and then select the application you want.
  6.    Click **Apply**, and then click **Run**.

Your application builds and deploys within a few moments. You can monitor the deployment status in Service Fabric Explorer.  

## Add a Service Fabric service to your Service Fabric application

To add a Service Fabric service to an existing Service Fabric application, do the following steps:

1.  Right-click the project you want to add a service to, and then click **Service Fabric**.

    ![Service Fabric Add Service page 1][add-service/p1]

2.  Click **Add Service Fabric Service**, and complete the set of steps to add a service to the project.
3.  Select the service template you want to add to your project, and then click **Next**.

    ![Service Fabric Add Service page 2][add-service/p2]

4.  Enter the service name (and other details, as needed), and then click the **Add Service** button.  

    ![Service Fabric Add Service page 3][add-service/p3]

5.  After the service is added, your overall project structure looks similar to the following project:

    ![Service Fabric Add Service page 4][add-service/p4]

## Edit Manifest versions of your Service Fabric Java application

To edit manifest versions, right click on the project, go to **Service Fabric** and select **Edit Manifest Versions...** from the menu dropdown. In the wizard, you can update the manifest versions for application manifest, service manifest and the versions for **Code**, **Config** and **Data** packages.

If you check the option **Automatically update application and service versions** and then update a version, then the manifest versions will be automatically updated. To give an example, you first select the check-box, then update the version of **Code** version from 0.0.0 to 0.0.1 and click on **Finish**, then service manifest version and application manifest version will be automatically updated to 0.0.1.

## Upgrade your Service Fabric Java application

For an upgrade scenario, say you created the **App1** project by using the Service Fabric plug-in in Eclipse. You deployed it by using the plug-in to create an application named **fabric:/App1Application**. The application type is **App1AppicationType**, and the application version is 1.0. Now, you want to upgrade your application without interrupting availability.

First, make any changes to your application, and then rebuild the modified service. Update the modified service’s manifest file (ServiceManifest.xml) with the updated versions for the service (and Code, Config, or Data, as relevant). Also, modify the application’s manifest (ApplicationManifest.xml) with the updated version number for the application and the modified service.  

To upgrade your application by using Eclipse, you can create a duplicate run configuration profile. Then, use it to upgrade your application as needed.

1.  Go to **Run** > **Run Configurations**. In the left pane, click the small arrow to the left of **Gradle Project**.
2.  Right-click **ServiceFabricDeployer**, and then select **Duplicate**. Enter a new name for this configuration, for example, **ServiceFabricUpgrader**.
3.  In the right panel, on the **Arguments** tab, change **-Pconfig='deploy'** to **-Pconfig='upgrade'**, and then click **Apply**.

This process creates and saves a run configuration profile you can use at any time to upgrade your application. It also gets the latest updated application type version from the application manifest file.

The application upgrade takes a few minutes. You can monitor the application upgrade in Service Fabric Explorer.

## Migrating old Service Fabric Java applications to be used with Maven
We have recently moved Service Fabric Java libraries from Service Fabric Java SDK to Maven repository. While the new applications you generate using Eclipse, will generate latest updated projects (which will be able to work with Maven), you can update your existing Service Fabric stateless or actor Java applications, which were using the Service Fabric Java SDK earlier, to use the Service Fabric Java dependencies from Maven. Please follow the steps mentioned [here](service-fabric-migrate-old-javaapp-to-use-maven.md) to ensure your older application works with Maven.

<!-- Images -->

[sf-eclipse-plugin-install]: ./media/service-fabric-get-started-eclipse/service-fabric-eclipse-plugin.png

[create-application/p1]:./media/service-fabric-get-started-eclipse/create-application/p1.png
[create-application/p2]:./media/service-fabric-get-started-eclipse/create-application/p2.png
[create-application/p3]:./media/service-fabric-get-started-eclipse/create-application/p3.png
[create-application/p4]:./media/service-fabric-get-started-eclipse/create-application/p4.png
[create-application/p5]:./media/service-fabric-get-started-eclipse/create-application/p5.png
[create-application/p6]:./media/service-fabric-get-started-eclipse/create-application/p6.png

[publish/Publish]: ./media/service-fabric-get-started-eclipse/publish/Publish.png
[publish/RightClick]: ./media/service-fabric-get-started-eclipse/publish/RightClick.png

[add-service/p1]: ./media/service-fabric-get-started-eclipse/add-service/p1.png
[add-service/p2]: ./media/service-fabric-get-started-eclipse/add-service/p2.png
[add-service/p3]: ./media/service-fabric-get-started-eclipse/add-service/p3.png
[add-service/p4]: ./media/service-fabric-get-started-eclipse/add-service/p4.png

<!-- Links -->
[buildship-update]: https://projects.eclipse.org/projects/tools.buildship
