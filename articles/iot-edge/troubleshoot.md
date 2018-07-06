---
title: Troubleshoot Azure IoT Edge | Microsoft Docs 
description: Resolve common issues and learn troubleshooting skills for Azure IoT Edge 
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 06/26/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
---

# Common issues and resolutions for Azure IoT Edge

If you experience issues running Azure IoT Edge in your environment, use this article as a guide for troubleshooting and resolution. 

## Standard diagnostic steps 

When you encounter an issue, learn more about the state of your IoT Edge device by reviewing the container logs and messages that pass to and from the device. Use the commands and tools in this section to gather information. 

### Check the status of the IoT Edge Security Manager and its logs:

On Linux:
- To view the status of the IoT Edge Security Manager:

   ```bash
   sudo systemctl status iotedge
   ```

- To view the logs of the IoT Edge Security Manager:

    ```bash
    sudo journalctl -u iotedge -f
    ```

- To view more detailed logs of the IoT Edge Security Manager:

   - Edit the iotedge daemon settings:

      ```bash
      sudo systemctl edit iotedge.service
      ```
   
   - Update the following lines:
    
      ```
      [Service]
      Environment=IOTEDGE_LOG=edgelet=debug
      ```
    
   - Restart the IoT Edge Security Daemon:
    
      ```bash
      sudo systemctl cat iotedge.service
      sudo systemctl daemon-reload
      sudo systemctl restart iotedge
      ```

On Windows:
- To view the status of the IoT Edge Security Manager:

   ```powershell
   Get-Service iotedge
   ```

- To view the logs of the IoT Edge Security Manager:

   ```powershell
   # Displays logs from today, newest at the bottom.
 
   Get-WinEvent -ea SilentlyContinue `
   -FilterHashtable @{ProviderName= "iotedged";
     LogName = "application"; StartTime = [datetime]::Today} |
   select TimeCreated, Message |
   sort-object @{Expression="TimeCreated";Descending=$false}
   ```

### If the IoT Edge Security Manager is not running, verify your yaml configuration file

> [!WARNING]
> YAML files cannot contain tabs as identation. Use 2 spaces instead.

On Linux:

   ```bash
   sudo nano /etc/iotedge/config.yaml
   ```

On Windows:

   ```cmd
   notepad C:\ProgramData\iotedge\config.yaml
   ```

### Check container logs for issues

Once the IoT Edge Security Daemon is running, look at the logs of the containers to detect issues. Start with your deployed containers, then look at the containers that make up the IoT Edge runtime: Edge Agent and Edge Hub. The Edge Agent logs typically provide info on the lifecycle of each container. The Edge Hub logs provide info on messaging and routing. 

   ```cmd
   iotedge logs <container name>
   ```

### View the messages going through the Edge hub

View the messages going through the Edge hub, and gather insights on device properties updates with verbose logs from the edgeAgent and edgeHub runtime containers. To turn on verbose logs on these containers, set the `RuntimeLogLevel` environment variable: 

On Linux:
    
   ```cmd
   export RuntimeLogLevel="debug"
   ```
    
On Windows:
    
   ```powershell
   [Environment]::SetEnvironmentVariable("RuntimeLogLevel", "debug")
   ```

You can also check the messages being sent between IoT Hub and the IoT Edge devices. View these messages by using the [Azure IoT Toolkit](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit) extension for Visual Studio Code. For more guidance, see [Handy tool when you develop with Azure IoT](https://blogs.msdn.microsoft.com/iotdev/2017/09/01/handy-tool-when-you-develop-with-azure-iot/).

### Restart containers
After investigating the logs and messages for information, you can try restarting containers:

```
iotedge restart <container name>
```

Restart the IoT Edge runtime containers:

```
iotedge restart edgeAgent && iotedge restart edgeHub
```

### Restart the IoT Edge security manager

If issue is still persisting, you can try restarting the IoT Edge security manager.

On Linux:

   ```cmd
   sudo systemctl restart iotedge
   ```

On Windows:

   ```powershell
   Stop-Service iotedge -NoWait
   sleep 5
   Start-Service iotedge
   ```

## Edge Agent stops after about a minute

The Edge Agent starts and runs successfully for about a minute, then stops. The logs indicate that the Edge Agent is attempting to connect to IoT Hub over AMQP, and then approximately 30 seconds later attempt to connect using AMQP over websocket. When that fails, the Edge Agent exits. 

Example Edge Agent logs:

```output
2017-11-28 18:46:19 [INF] - Starting module management agent. 
2017-11-28 18:46:19 [INF] - Version - 1.0.7516610 (03c94f85d0833a861a43c669842f0817924911d5) 
2017-11-28 18:46:19 [INF] - Edge agent attempting to connect to IoT Hub via AMQP... 
2017-11-28 18:46:49 [INF] - Edge agent attempting to connect to IoT Hub via AMQP over WebSocket... 
```

### Root cause
A networking configuration on the host network is preventing the Edge Agent from reaching the network. The agent attempts to connect over AMQP (port 5671) first. If this fails, it tries websockets (port 443).

The IoT Edge runtime sets up a network for each of the modules to communicate on. On Linux, this network is a bridge network. On Windows, it uses NAT. This issue is more common on Windows devices using Windows containers that use the NAT network. 

### Resolution
Ensure that there is a route to the internet for the IP addresses assigned to this bridge/NAT network. Sometimes a VPN configuration on the host overrides the IoT Edge network. 

## Edge Hub fails to start

The Edge Hub fails to start, and prints the following message to the logs: 

```output
One or more errors occurred. 
(Docker API responded with status code=InternalServerError, response=
{\"message\":\"driver failed programming external connectivity on endpoint edgeHub (6a82e5e994bab5187939049684fb64efe07606d2bb8a4cc5655b2a9bad5f8c80): 
Error starting userland proxy: Bind for 0.0.0.0:443 failed: port is already allocated\"}\n) 
```

### Root cause
Some other process on the host machine has bound port 443. The Edge Hub maps ports 5671 and 443 for use in gateway scenarios. This port mapping fails if another process has already bound this port. 

### Resolution
Find and stop the process that is using port 443. This process is usually a web server.

## Edge Agent can't access a module's image (403)
A container fails to run, and the Edge Agent logs show a 403 error. 

### Root cause
The Edge Agent doesn't have permissions to access a module's image. 

### Resolution
Make sure that your registry credentials are correctly specified in your deployment manifest

## IoT Edge security daemon fails with an invalid hostname

The command `sudo journalctl -u iotedge` fails and prints the following message: 

```output
Error parsing user input data: invalid hostname. Hostname cannot be empty or greater than 64 characters
```

### Root cause
The IoT Edge runtime can only support hostnames that are shorter than 64 characters. This usually isn't an issue for physical machines, but can occur when you set up the runtime on a virtual machine. The automatically generated hostnames for Windows virtual machines hosted in Azure, in particular, tend to be long. 

### Resolution
When you see this error, you can resolve it by configuring the DNS name of your virtual machine, and then setting the DNS name as the hostname in the setup command.

1. In the Azure portal, navigate to the overview page of your virtual machine. 
2. Select **configure** under DNS name. If your virtual machine already has a DNS name configured, you don't need to configure a new one. 

   ![Configure DNS name](./media/troubleshoot/configure-dns.png)

3. Provide a value for **DNS name label** and select **Save**.
4. Copy the new DNS name, which should be in the format **\<DNSnamelabel\>.\<vmlocation\>.cloudapp.azure.com**.
5. Inside the virtual machine, use the following command to set up the IoT Edge runtime with your DNS name:

   - On Linux:

      ```bash
      sudo nano /etc/iotedge/config.yaml
      ```

   - On Windows:

      ```cmd
      notepad C:\ProgramData\iotedge\config.yaml
      ```

## Stability issues on resource constrained devices 
You may encounter stability problems on constrained devices like the Raspberry Pi, especially when used as a gateway. Symptoms include out of memory exceptions in the edge hub module, downstream devices cannot connect or the device stops sending telemetry messages after a few hours.

### Root cause
The edge hub, which is part of the edge runtime, is optimized for performance by default and attempts to allocate large chunks of memory. This is not ideal for constrained edge devices and can cause stability problems.

### Resolution
For the edge hub set an environment variable **OptimizeForPerformance** to **false**. There are two ways to do this:

In the UI: 
In the portal from *Device Details*->*Set Modules*->*Configure advanced Edge Runtime settings*, create an environment variable called *OptimizeForPerformance* that is set to *false* for the *Edge Hub*.

![optimizeforperformance][img-optimize-for-perf]

In the deployment manifest:

```json
  "edgeHub": {
    "type": "docker",
    "settings": {
      "image": "mcr.microsoft.com/azureiotedge-hub:1.0",
      "createOptions": <snipped>
    },
    "env": {
      "OptimizeForPerformance": {
          "value": "false"
      }
    },
```

## Next steps
Do you think that you found a bug in the IoT Edge platform? Please, [submit an issue](https://github.com/Azure/iotedge/issues) so that we can continue to improve. 

<!-- Images -->
[img-optimize-for-perf]: ./media/troubleshoot/OptimizeForPerformanceFalse.png