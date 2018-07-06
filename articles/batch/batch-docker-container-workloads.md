---
title: Container workloads on Azure Batch | Microsoft Docs
description: Learn how to run applications from container images on Azure Batch.
services: batch
author: dlepow
manager: jeconnoc

ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.workload: na
ms.date: 06/04/2018
ms.author: danlep

---

# Run container applications on Azure Batch

Azure Batch lets you run and scale large numbers of batch computing jobs on Azure. Batch tasks can run directly on virtual machines (nodes) in a Batch pool, but you can also set up a Batch pool to run tasks in Docker-compatible containers on the nodes. This article shows you how to create a pool of compute nodes that support running container tasks, and then run container tasks on the pool. 

You should be familiar with container concepts and how to create a Batch pool and job. The code examples use the Batch .NET and Python SDKs. You can also use other Batch SDKs and tools, including the Azure portal, to create container-enabled Batch pools and to run container tasks.

## Why use containers?

Using containers provides an easy way to run Batch tasks without having to manage an environment and dependencies to run applications. Containers deploy applications as lightweight, portable, self-sufficient units that can run in several different environments. For example, you can build and test a container locally, then upload the container image to a registry in Azure or elsewhere. The container deployment model ensures that the runtime environment of your application is always correctly installed and configured wherever you host the application. Container-based tasks in Batch can also take advantage of features of non-container tasks, including application packages and management of resource files and output files. 

## Prerequisites

* **SDK versions**: The Batch SDKs support container images as of the following versions:
    * Batch REST API version 2017-09-01.6.0
    * Batch .NET SDK version 8.0.0
    * Batch Python SDK version 4.0
    * Batch Java SDK version 3.0
    * Batch Node.js SDK version 3.0

* **Accounts**: In your Azure subscription, you need to create a Batch account and optionally an Azure Storage account.

* **A supported VM image**: Containers are only supported in pools created with the Virtual Machine Configuration, from images detailed in the following section, "Supported virtual machine images." If you provide a custom image, see the considerations in the following section and the requirements in [Use a managed custom image to create a pool of virtual machines](batch-custom-images.md). 

### Limitations

* Batch provides RDMA support only for containers running on Linux pools.

## Supported virtual machine images

Use one of the following supported Windows or Linux images to create a pool of VM compute nodes for container workloads. For more information about Marketplace images that are compatible with Batch, see [list of virtual machine images](batch-linux-nodes.md#list-of-virtual-machine-images). 

### Windows images

For Windows container workloads, Batch currently supports the **Windows Server 2016 Datacenter with Containers** image in the Azure Marketplace. Only Docker container images are supported on Windows.

You can also create custom images from VMs running Docker on Windows.

### Linux images

For Linux container workloads, Batch currently supports the following Linux images published by Microsoft Azure Batch in the Azure Marketplace:

* **CentOS for Azure Batch container pools**

* **CentOS (with RDMA drivers) for Azure Batch container pools**

* **Ubuntu Server for Azure Batch container pools**

* **Ubuntu Server (with RDMA drivers) for Azure Batch container pools**

These images are only supported for use in Azure Batch pools. They feature:

* A pre-installed [Moby](https://github.com/moby/moby) container runtime 

* Pre-installed NVIDIA GPU drivers, to streamline deployment on Azure N-series VMs

* Images with or without pre-installed RDMA drivers; these drivers allow pool nodes to access the Azure RDMA network when deployed on RDMA-capable VM sizes  

You can also create custom images from VMs running Docker on one of the Linux distributions that is compatible with Batch. If you choose to provide your own custom Linux image, see the instructions in [Use a managed custom image to create a pool of virtual machines](batch-custom-images.md).

For Docker support on a custom image, install [Docker Community Edition (CE)](https://www.docker.com/community-edition) or [Docker Enterprise Edition (EE)](https://www.docker.com/enterprise-edition).

Additional considerations for using a custom Linux image:

* To take advantage of the GPU performance of Azure N-series sizes when using a custom image, pre-install NVIDIA drivers. Also, you need to install the Docker Engine Utility for NVIDIA GPUs, [NVIDIA Docker](https://github.com/NVIDIA/nvidia-docker).

* To access the Azure RDMA network, use an RDMA-capable VM size. Necessary RDMA drivers are installed in the CentOS HPC and Ubuntu images supported by Batch. Additional configuration may be needed to run MPI workloads. See [Use RDMA-capable or GPU-enabled instances in Batch pool](batch-pool-compute-intensive-sizes.md).


## Container configuration for Batch pool

To enable a Batch pool to run container workloads, you must specify [ContainerConfiguration](/dotnet/api/microsoft.azure.batch.containerconfiguration) settings in the pool's [VirtualMachineConfiguration](/dotnet/api/microsoft.azure.batch.virtualmachineconfiguration) object. (This article provides links to the Batch .NET API reference. Corresponding settings are in the [Batch Python](/python/api/azure.batch) API.)

You can create a container-enabled pool with or without prefetched container images, as shown in the following examples. The pull (or prefetch) process lets you pre-load container images from either Docker Hub or another container registry on the Internet. For best performance, use an [Azure container registry](../container-registry/container-registry-intro.md) in the same region as the Batch account.

The advantage of prefetching container images is that when tasks first start running they don't have to wait for the container image to download. The container configuration pulls container images to the VMs when the pool is created. Tasks that run on the pool can then reference the list of container images and container run options.


### Pool without prefetched container images

To configure a container-enabled pool without prefetched container images, define `ContainerConfiguration` and `VirtualMachineConfiguration` objects as shown in the following Python example. This example uses the Ubuntu Server for Azure Batch container pools image from the Marketplace.


```python
image_ref_to_use = batch.models.ImageReference(
        publisher='microsoft-azure-batch',
        offer='ubuntu-server-container',
        sku='16-04-lts',
        version='latest')

"""
Specify container configuration. This is required even though there are no prefetched images.
"""

container_conf = batch.models.ContainerConfiguration()

new_pool = batch.models.PoolAddParameter(
        id=pool_id,
        virtual_machine_configuration=batch.models.VirtualMachineConfiguration(
            image_reference=image_ref_to_use,
            container_configuration=container_conf,
            node_agent_sku_id='batch.node.ubuntu 16.04'),
        vm_size='STANDARD_D1_V2',
        target_dedicated_nodes=1)
...
```


### Prefetch images for container configuration

To prefetch container images on the pool, add the list of container images (`container_image_names`, in Python) to the `ContainerConfiguration`. 

The following basic Python example shows how to prefetch a standard Ubuntu container image from [Docker Hub](https://hub.docker.com).

```python
image_ref_to_use = batch.models.ImageReference(
    publisher='microsoft-azure-batch',
    offer='ubuntu-server-container',
    sku='16-04-lts',
    version='latest')

"""
Specify container configuration, fetching the official Ubuntu container image from Docker Hub. 
"""

container_conf = batch.models.ContainerConfiguration(container_image_names=['ubuntu'])

new_pool = batch.models.PoolAddParameter(
    id=pool_id,
    virtual_machine_configuration=batch.models.VirtualMachineConfiguration(
        image_reference=image_ref_to_use,
        container_configuration=container_conf,
        node_agent_sku_id='batch.node.ubuntu 16.04'),
    vm_size='STANDARD_D1_V2',
    target_dedicated_nodes=1)
...
```


The following example C# example assumes that you want to prefetch a TensorFlow image from [Docker Hub](https://hub.docker.com). This example includes a start task that runs in the VM host on the pool nodes. You might run a start task in the host, for example, to mount a file server that can be accessed from the containers.

```csharp

ImageReference imageReference = new ImageReference(
    publisher: "microsoft-azure-batch",
    offer: "ubuntu-server-container",
    sku: "16-04-lts",
    version: "latest");

// Specify container configuration, prefetching Docker images
ContainerConfiguration containerConfig = new ContainerConfiguration(
    containerImageNames: new List<string> { "tensorflow/tensorflow:latest-gpu" } );

// VM configuration
VirtualMachineConfiguration virtualMachineConfiguration = new VirtualMachineConfiguration(
    imageReference: imageReference,
    containerConfiguration: containerConfig,
    nodeAgentSkuId: "batch.node.ubuntu 16.04");

// Set a native host command line start task
StartTask startTaskNative = new StartTask( CommandLine: "<native-host-command-line>" );

// Create pool
CloudPool pool = batchClient.PoolOperations.CreatePool(
    poolId: poolId,
    targetDedicatedComputeNodes: 4,
    virtualMachineSize: "Standard_NC6",
    virtualMachineConfiguration: virtualMachineConfiguration, startTaskContainer);
...
```


### Prefetch images from a private container registry

You can also prefetch container images by authenticating to a private container registry server. In the following example, the `ContainerConfiguration` and `VirtualMachineConfiguration` objects prefetch a private TensorFlow image from a private Azure container registry. The image reference is the same as in the previous example.

```csharp
// Specify a container registry
ContainerRegistry containerRegistry = new ContainerRegistry (
	registryServer: "myContainerRegistry.azurecr.io",
    username: "myUserName",
    password: "myPassword");

// Create container configuration, prefetching Docker images from the container registry
ContainerConfiguration containerConfig = new ContainerConfiguration(
    containerImageNames: new List<string> {
        "myContainerRegistry.azurecr.io/tensorflow/tensorflow:latest-gpu" },
    containerRegistries: new List<ContainerRegistry> { containerRegistry } );

// VM configuration
VirtualMachineConfiguration virtualMachineConfiguration = new VirtualMachineConfiguration(
    imageReference: imageReference,
    containerConfiguration: containerConfig,
    nodeAgentSkuId: "batch.node.ubuntu 16.04");

// Create pool
CloudPool pool = batchClient.PoolOperations.CreatePool(
    poolId: poolId,
    targetDedicatedComputeNodes: 4,
    virtualMachineSize: "Standard_NC6",
    virtualMachineConfiguration: virtualMachineConfiguration);
...
```


## Container settings for the task

To run container tasks on the compute nodes, you must specify container-specific settings such as task run options, images to use, and registry.

Use the `ContainerSettings` property of the task classes to configure container-specific settings. These settings are defined by the [TaskContainerSettings](/dotnet/api/microsoft.azure.batch.taskcontainersettings) class.

If you run tasks on container images, the [cloud task](/dotnet/api/microsoft.azure.batch.cloudtask) and [job manager task](/dotnet/api/microsoft.azure.batch.cloudjob.jobmanagertask) require container settings. However, the [start task](/dotnet/api/microsoft.azure.batch.starttask), [job preparation task](/dotnet/api/microsoft.azure.batch.cloudjob.jobpreparationtask), and [job release task](/dotnet/api/microsoft.azure.batch.cloudjob.jobreleasetask) do not require container settings (that is, they can run within a container context or directly on the node).

The command line for an Azure Batch container task executes in a working directory in the container that is very similar to the environment Batch sets up for a regular (non-container) task:

* All directories recursively below the `AZ_BATCH_NODE_ROOT_DIR` (the root of Azure Batch directories on the node) are mapped into the container
* All task environment variables are mapped into the container
* The application working directory is set the same as for a regular task, so you can use features such as application packages and resource files

Because Batch changes the default working directory in your container, the task runs in a location different from the typical container entry point (for example, `c:\` by default on a Windows container, or `/` on Linux). Make sure that your task command line or container entry point specifies an absolute path, if it isn't already configured that way.

The following Python snippet shows a basic command line running in an Ubuntu container pulled from Docker Hub. The container run options are additional arguments to the `docker create` command that the task runs. Here, the `--rm` option removes the container after the task finishes.

```python
task_id = 'sampletask'
task_container_settings = batch.models.TaskContainerSettings(
    image_name='ubuntu', 
    container_run_options='--rm')
task = batch.models.TaskAddParameter(
    id=task_id,
    command_line='/bin/echo hello',
    container_settings=task_container_settings
)

```

The following C# example shows basic container settings for a cloud task:

```csharp
// Simple container task command

string cmdLine = "c:\myApp.exe";

TaskContainerSettings cmdContainerSettings = new TaskContainerSettings (
    imageName: "tensorflow/tensorflow:latest-gpu",
    containerRunOptions: "--rm --read-only"
    );

CloudTask containerTask = new CloudTask (
    id: "Task1",
    containerSettings: cmdContainerSettings,
    commandLine: cmdLine); 
```


## Next steps

* Also see the [Batch Shipyard](https://github.com/Azure/batch-shipyard) toolkit for easy deployment of container workloads on Azure Batch through [Shipyard recipes](https://github.com/Azure/batch-shipyard/tree/master/recipes).

* For more information on installing and using Docker CE on Linux, see the [Docker](https://docs.docker.com/engine/installation/) documentation.

* For more information on using custom images, see [Use a managed custom image to create a pool of virtual machines ](batch-custom-images.md).

* Learn more about the [Moby project](https://mobyproject.org/), a framework for creating container-based systems.