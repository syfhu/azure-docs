---
title: Quickstart Azure IoT Edge + Linux | Microsoft Docs 
description: In this quickstart, learn how to deploy prebuilt code remotely to an IoT Edge device.
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 06/27/2018
ms.topic: quickstart
ms.service: iot-edge
services: iot-edge
ms.custom: mvc

experimental: true
experiment_id: 21cb7321-bcff-4b
---

# Quickstart: Deploy your first IoT Edge module to a Linux x64 device

Azure IoT Edge moves the power of the cloud to your Internet of Things devices. In this quickstart, learn how to use the cloud interface to deploy prebuilt code remotely to an IoT Edge device.

In this quickstart you learn how to:

1. Create an IoT Hub.
2. Register an IoT Edge device to your IoT hub.
3. Install and start the IoT Edge runtime on your device.
4. Remotely deploy a module to an IoT Edge device.

![Quickstart architecture][2]

This quickstart turns your Linux computer or virtual machine into an IoT Edge device. Then you can deploy a module from the Azure portal to your device. The module that you deploy in this quickstart is a simulated sensor that generates temperature, humidity, and pressure data. The other Azure IoT Edge tutorials build upon the work you do here by deploying modules that analyze the simulated data for business insights. 

If you don't have an active Azure subscription, create a [free account][lnk-account] before you begin.


[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

You use the Azure CLI to complete many of the steps in this quickstart, and Azure IoT has an extension to enable additional functionality. 

Add the Azure IoT extension to the cloud shell instance.

   ```azurecli-interactive
   az extension add --name azure-cli-iot-ext
   ```

## Prerequisites

This quickstart uses a Linux machine as an IoT Edge device. If you don't have one available for testing, you can create one using the Azure CLI. 

Create a new resource group. You can use this resource group for the other Azure resources that you create in this quickstart, for easy management.  

   ```azurecli-interactive
   az group create --name IoTEdgeResources --location westus
   ```

Create the virtual machine. You don't need a very large virtual machine to test IoT Edge. A size like **B1ms** is sufficient.

   ```azurecli-interactive
   az vm create --resource-group IoTEdgeResources --name EdgeVM --image Canonical:UbuntuServer:16.04-LTS:latest --admin-username azureuser --generate-ssh-keys --size Standard_B1ms
   ```

## Create an IoT hub

Start the quickstart by creating your IoT Hub in the Azure portal.
![Create IoT Hub][3]

The free level of IoT Hub works for this quickstart. If you've used IoT Hub in the past and already have a free hub created, you can use that IoT hub. Each subscription can only have one free IoT hub. 

1. In the Azure cloud shell, create a resource group if you didn't as part of the prerequisites. By putting all the resources for the quickstarts and tutorials in a group, you can manage them together. 

   ```azurecli-interactive
   az group create --name IoTEdgeResources --location westus
   ```

1. Create an IoT hub in your new resource group. The following code creates a free **F1** hub in the resource group **IoTEdgeResources**. Replace *{hub_name}* with a unique name for your IoT hub.

   ```azurecli-interactive
   az iot hub create --resource-group TestResources --name {hub_name} --sku F1 
   ```

   If you get an error because there's already one free hub in your subscription, change the SKU to **S1**. 

## Register an IoT Edge device

Register an IoT Edge device with your newly created IoT Hub.
![Register a device][4]

Create a device identity for your simulated device so that it can communicate with your IoT hub. Since IoT Edge devices behave and can be managed differently than typical IoT devices, you declare this to be an IoT Edge device from the beginning. 

1. In the Azure cloud shell, enter the following command to create a device named **myEdgeDevice** in your hub.

   ```azurecli-interactive
   az iot hub device-identity create --hub-name {hub_name} --device-id myEdgeDevice --edge-enabled
   ```

1. Retrieve the connection string for your device, which links your physical device with its identity in IoT Hub. 

   ```azurecli-interactive
   az iot hub device-identity show-connection-string --device-id myEdgeDevice --hub-name {hub_name}
   ```

1. Copy the connection string and save it. You'll use this value to configure the IoT Edge runtime in the next section. 


## Install and start the IoT Edge runtime

Install and start the Azure IoT Edge runtime on your device. 
![Register a device][5]

The IoT Edge runtime is deployed on all IoT Edge devices. It has three components. The **IoT Edge security daemon** starts each time an Edge device boots and bootstraps the device by starting the IoT Edge agent. The **IoT Edge agent** facilitates deployment and monitoring of modules on the IoT Edge device, including the IoT Edge hub. The **IoT Edge hub** manages communications between modules on the IoT Edge device, and between the device and IoT Hub. 

Complete the following steps in the Linux machine or VM that you prepared for this quickstart. 

### Register your device to use the software repository

The packages that you need to run the IoT Edge runtime are managed in a software repository. Configure your IoT Edge device to access this repository. 

The steps in this section are for devices running **Ubuntu 16.04**. To access the software repository on other versions of Linux, see [Install the Azure IoT Edge runtime on Linux (x64)](how-to-install-iot-edge-linux.md) or [Install Azure IoT Edge runtime on Linux (ARM32v7/armhf)](how-to-install-iot-edge-linux-arm.md).

1. On the machine that you're using as an IoT Edge device, install the repository configuration.

   ```bash
   curl https://packages.microsoft.com/config/ubuntu/16.04/prod.list > ./microsoft-prod.list
   sudo cp ./microsoft-prod.list /etc/apt/sources.list.d/
   ```

2. Install a public key to access the repository.

   ```bash
   curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
   sudo cp ./microsoft.gpg /etc/apt/trusted.gpg.d/
   ```

### Install a container runtime

The IoT Edge runtime is a set of containers, and the logic that you deploy to your IoT Edge device is packaged as containers. Prepare your device for these components by installing a container runtime.

Update **apt-get**.

   ```bash
   sudo apt-get update
   ```

Install **Moby**, a container runtime.

   ```bash
   sudo apt-get install moby-engine
   ```

Install the CLI commands for Moby. 

   ```bash
   sudo apt-get install moby-cli
   ```

### Install and configure the IoT Edge security daemon

The security daemon installs as a system service so that the IoT Edge runtime starts every time your device boots. The installation also includes a version of **hsmlib** that allows the security daemon to interact with the device's hardware security. 

1. Download and install the IoT Edge Security Daemon. 

   ```bash
   sudo apt-get update
   sudo apt-get install iotedge
   ```

2. Open the IoT Edge configuration file. It is a protected file so you may have to use elevated privileges to access it.
   
   ```bash
   sudo nano /etc/iotedge/config.yaml
   ```

3. Add the IoT Edge device connection string. Find the variable **device_connection_string** and update its value with the string that you copied after registering your device.

4. Save and close the file. 

   `CTRL + X`, `Y`, `Enter`

4. Restart the IoT Edge security daemon.

   ```bash
   sudo systemctl restart iotedge
   ```

5. Check to see that the Edge Security Daemon is running as a system service.

   ```bash
   sudo systemctl status iotedge
   ```

   ![See the Edge Daemon running as a system service](./media/quickstart-linux/iotedged-running.png)

   You can also see logs from the Edge Security Daemon by running the following command:

   ```bash
   journalctl -u iotedge
   ```

6. View the modules running on your device. 

   >[!TIP]
   >You need to use *sudo* to run `iotedge` commands at first. Sign out of your machine and sign back in to update permissions, then you can run `iotedge` commands without elevated privileges. 

   ```bash
   sudo iotedge list
   ```

   ![View one module on your device](./media/quickstart-linux/iotedge-list-1.png)

## Deploy a module

Manage your Azure IoT Edge device from the cloud to deploy a module that will send telemetry data to IoT Hub.
![Register a device][6]

[!INCLUDE [iot-edge-deploy-module](../../includes/iot-edge-deploy-module.md)]

## View generated data

In this quickstart, you created a new IoT Edge device and installed the IoT Edge runtime on it. Then, you used the Azure portal to push an IoT Edge module to run on the device without having to make changes to the device itself. In this case, the module that you pushed creates environmental data that you can use for the tutorials. 

Open the command prompt on the computer running your simulated device again. Confirm that the module deployed from the cloud is running on your IoT Edge device:

   ```bash
   sudo iotedge list
   ```
   After a logoff and login, *sudo* is not required for the above command.

   ![View three modules on your device](./media/quickstart-linux/iotedge-list-2.png)

View the messages being sent from the tempSensor module:

   ```bash
   sudo iotedge logs tempSensor -f 
   ```
After a logoff and login, *sudo* is not required for the above command.

![View the data from your module](./media/quickstart-linux/iotedge-logs.png)

The temperature sensor module may be waiting to connect to Edge Hub if the last line you see in the log is `Using transport Mqtt_Tcp_Only`. Try killing the module and letting the Edge Agent restart it. You can kill it with the command `sudo docker stop tempSensor`.

You can also view the telemetry the device is sending by using the [IoT Hub explorer tool][lnk-iothub-explorer] or the [Azure IoT Toolkit extension for Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit). 


## Clean up resources

If you want to continue on to the IoT Edge tutorials, you can use the device that you registered and set up in this quickstart. Otherwise, you can delete the Azure resources that you created and remove the IoT Edge runtime from your device. 

### Delete Azure resources

If you created your virtual machine and IoT hub in a new resource group, you can delete that group and all the associated resources. If there's anything in that resource group that you want to keep, then just delete the individual resources that you want to clean up. 

To remove a resource group, follow these steps: 

1. Sign in to the [Azure portal](https://portal.azure.com) and click **Resource groups**.
2. In the **Filter by name...** textbox, type the name of the resource group containing your IoT Hub. 
3. To the right of your resource group in the result list, click **...** then **Delete resource group**.
4. You will be asked to confirm the deletion of the resource group. Type the name of your resource group again to confirm, and then click **Delete**. After a few moments, the resource group and all of its contained resources are deleted.

### Remove the IoT Edge runtime

If you want to remove the installations from your device, use the following commands.  

Remove the IoT Edge runtime.

   ```bash
   sudo apt-get remove --purge iotedge
   ```

When the IoT Edge runtime is removed, the containers that it created are stopped, but still exist on your device. View all containers.

   ```bash
   sudo docker ps -a
   ```

Delete the containers that were created on your device by the IoT Edge runtime. Change the name of the tempSensor container if you called it something different. 

   ```bash
   sudo docker rm -f tempSensor
   sudo docker rm -f edgeHub
   sudo docker rm -f edgeAgent
   ```

Remove the container runtime.

   ```bash
   sudo apt-get remove --purge moby
   ```

## Next steps

This quickstart is the prerequisite for all of the IoT Edge tutorials. You can continue on to any of the other tutorials to learn how Azure IoT Edge can help you turn this data into business insights at the edge.

> [!div class="nextstepaction"]
> [Filter sensor data using an Azure Function](tutorial-deploy-function.md)



<!-- Images -->
[0]: ./media/quickstart-linux/cloud-shell.png
[1]: ./media/quickstart-linux/view-module.png
[2]: ./media/quickstart-linux/install-edge-full.png
[3]: ./media/quickstart-linux/create-iot-hub.png
[4]: ./media/quickstart-linux/register-device.png
[5]: ./media/quickstart-linux/start-runtime.png
[6]: ./media/quickstart-linux/deploy-module.png
[7]: ./media/quickstart-linux/iotedged-running.png
[8]: ./media/tutorial-simulate-device-linux/running-modules.png
[9]: ./media/tutorial-simulate-device-linux/sensor-data.png


<!-- Links -->
[lnk-docker-ubuntu]: https://docs.docker.com/engine/installation/linux/docker-ce/ubuntu/ 
[lnk-iothub-explorer]: https://github.com/azure/iothub-explorer
[lnk-account]: https://azure.microsoft.com/free
[lnk-portal]: https://portal.azure.com
[lnk-delete]: https://docs.microsoft.com/cli/azure/iot/hub?view=azure-cli-latest#az_iot_hub_delete

<!-- Anchor links -->
[anchor-register]: #register-an-iot-edge-device
