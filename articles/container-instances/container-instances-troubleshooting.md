---
title: Troubleshooting Azure Container Instances
description: Learn how to troubleshoot issues with Azure Container Instances
services: container-instances
author: seanmck
manager: jeconnoc

ms.service: container-instances
ms.topic: article
ms.date: 03/14/2018
ms.author: seanmck
ms.custom: mvc
---

# Troubleshoot common issues in Azure Container Instances

This article shows how to troubleshoot common issues for managing or deploying containers to Azure Container Instances.

## Naming conventions

When defining your container specification, certain parameters require adherence to naming restrictions. Below is a table with specific requirements for container group properties.
For more information on Azure naming conventions, see [Naming conventions](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions#naming-rules-and-restrictions) in the Azure Architecture Center.

| Scope | Length | Casing | Valid characters | Suggested pattern | Example |
| --- | --- | --- | --- | --- | --- | --- |
| Container Group name | 1-64 |Case insensitive |Alphanumeric and hyphen anywhere except the first or last character |`<name>-<role>-CG<number>` |`web-batch-CG1` |
| Container name | 1-64 |Case insensitive |Alphanumeric and hyphen anywhere except the first or last character |`<name>-<role>-CG<number>` |`web-batch-CG1` |
| Container ports | Between 1 and 65535 |Integer |Integer between 1 and 65535 |`<port-number>` |`443` |
| DNS name label | 5-63 |Case insensitive |Alphanumeric and hyphen anywhere except the first or last character |`<name>` |`frontend-site1` |
| Environment variable | 1-63 |Case insensitive |Alphanumeric and the '_' chracter anywhere except the first or last character |`<name>` |`MY_VARIABLE` |
| Volume name | 5-63 |Case insensitive |Lowercase letters, numbers, and hyphens anywhere except the first or last character. Cannot contain two consecutive hyphens. |`<name>` |`batch-output-volume` |

## Image version not supported

If you specify an image that Azure Container Instances cannot support, an `ImageVersionNotSupported` error is returned. The value of the error is `The version of image '{0}' is not supported.`, and currently applies to Windows 1709 images. To mitigate this issue, use an LTS Windows image. Support for Windows 1709 images is underway.

## Unable to pull image

If Azure Container Instances is unable to pull your image initially, it retries for some period before eventually failing. If the image cannot be pulled, events like the following are shown in the output of [az container show][az-container-show]:

```bash
"events": [
  {
    "count": 3,
    "firstTimestamp": "2017-12-21T22:56:19+00:00",
    "lastTimestamp": "2017-12-21T22:57:00+00:00",
    "message": "pulling image \"microsoft/aci-hellowrld\"",
    "name": "Pulling",
    "type": "Normal"
  },
  {
    "count": 3,
    "firstTimestamp": "2017-12-21T22:56:19+00:00",
    "lastTimestamp": "2017-12-21T22:57:00+00:00",
    "message": "Failed to pull image \"microsoft/aci-hellowrld\": rpc error: code 2 desc Error: image t/aci-hellowrld:latest not found",
    "name": "Failed",
    "type": "Warning"
  },
  {
    "count": 3,
    "firstTimestamp": "2017-12-21T22:56:20+00:00",
    "lastTimestamp": "2017-12-21T22:57:16+00:00",
    "message": "Back-off pulling image \"microsoft/aci-hellowrld\"",
    "name": "BackOff",
    "type": "Normal"
  }
],
```

To resolve, delete the container and retry your deployment, paying close attention that you have typed the image name correctly.

## Container continually exits and restarts

If your container runs to completion and automatically restarts, you might need to set a [restart policy](container-instances-restart-policy.md) of **OnFailure** or **Never**. If you specify **OnFailure** and still see continual restarts, there might be an issue with the application or script executed in your container.

The Container Instances API includes a `restartCount` property. To check the number of restarts for a container, you can use the [az container show][az-container-show] command in the Azure CLI. In following example output (which has been truncated for brevity), you can see the `restartCount` property at the end of the output.

```json
...
 "events": [
   {
     "count": 1,
     "firstTimestamp": "2017-11-13T21:20:06+00:00",
     "lastTimestamp": "2017-11-13T21:20:06+00:00",
     "message": "Pulling: pulling image \"myregistry.azurecr.io/aci-tutorial-app:v1\"",
     "type": "Normal"
   },
   {
     "count": 1,
     "firstTimestamp": "2017-11-13T21:20:14+00:00",
     "lastTimestamp": "2017-11-13T21:20:14+00:00",
     "message": "Pulled: Successfully pulled image \"myregistry.azurecr.io/aci-tutorial-app:v1\"",
     "type": "Normal"
   },
   {
     "count": 1,
     "firstTimestamp": "2017-11-13T21:20:14+00:00",
     "lastTimestamp": "2017-11-13T21:20:14+00:00",
     "message": "Created: Created container with id bf25a6ac73a925687cafcec792c9e3723b0776f683d8d1402b20cc9fb5f66a10",
     "type": "Normal"
   },
   {
     "count": 1,
     "firstTimestamp": "2017-11-13T21:20:14+00:00",
     "lastTimestamp": "2017-11-13T21:20:14+00:00",
     "message": "Started: Started container with id bf25a6ac73a925687cafcec792c9e3723b0776f683d8d1402b20cc9fb5f66a10",
     "type": "Normal"
   }
 ],
 "previousState": null,
 "restartCount": 0
...
}
```

> [!NOTE]
> Most container images for Linux distributions set a shell, such as bash, as the default command. Since a shell on its own is not a long-running service, these containers immediately exit and fall into a restart loop when configured with the default **Always** restart policy.

## Container takes a long time to start

The two primary factors that contribute to container startup time in Azure Container Instances are:

* [Image size](#image-size)
* [Image location](#image-location)

Windows images have [additional considerations](#cached-windows-images).

### Image size

If your container takes a long time to start, but eventually succeeds, start by looking at the size of your container image. Because Azure Container Instances pulls your container image on demand, the startup time you experience is directly related to its size.

You can view the size of your container image by using the `docker images` command in the Docker CLI:

```console
$ docker images
REPOSITORY                  TAG       IMAGE ID        CREATED        SIZE
microsoft/aci-helloworld    latest    7f78509b568e    13 days ago    68.1MB
```

The key to keeping image sizes small is ensuring that your final image does not contain anything that is not required at runtime. One way to do this is with [multi-stage builds][docker-multi-stage-builds]. Multi-stage builds make it easy to ensure that the final image contains only the artifacts you need for your application, and not any of the extra content that was required at build time.

### Image location

Another way to reduce the impact of the image pull on your container's startup time is to host the container image in [Azure Container Registry](/azure/container-registry/) in the same region where you intend to deploy container instances. This shortens the network path that the container image needs to travel, significantly shortening the download time.

### Cached Windows images

Azure Container Instances uses a caching mechanism to help speed container startup time for images based on certain Windows images.

To ensure the fastest Windows container startup time, use one of the **three most recent** versions of the following **two images** as the base image:

* [Windows Server 2016][docker-hub-windows-core] (LTS only)
* [Windows Server 2016 Nano Server][docker-hub-windows-nano]

### Windows containers slow network readiness

Windows containers may incur no inbound or outbound connectivity for up to 5 seconds on initial creation. After initial setup container networking should resume appropriately.

## Resource not available error

Due to varying regional resource load in Azure, you might receive the following error when attempting to deploy a container instance:

`The requested resource with 'x' CPU and 'y.z' GB memory is not available in the location 'example region' at this moment. Please retry with a different resource request or in another location.`

This error indicates that due to heavy load in the region in which you are attempting to deploy, the resources specified for your container can't be allocated at that time. Use one or more of the following mitigation steps to help resolve your issue.

* Verify your container deployment settings fall within the parameters defined in [Quotas and region availability for Azure Container Instances](container-instances-quotas.md#region-availability)
* Specify lower CPU and memory settings for the container
* Deploy to a different Azure region
* Deploy at a later time

## Cannot connect to underlying Docker API or run privileged containers

Azure Container Instances does not expose direct access to the underlying infrastructure which hosts container groups. This includes access to the Docker API running on the container's host and running privileged containers. If you require Docker interaction, check our [REST reference documentation](https://aka.ms/aci/rest) to see what the ACI API supports. If there is something missing, submit a request on the [ACI feedback forums](https://aka.ms/aci/feedback).

## Next steps
Learn how to [retrieve container logs & events](container-instances-get-logs.md) to help debug your containers.

<!-- LINKS - External -->
[docker-multi-stage-builds]: https://docs.docker.com/engine/userguide/eng-image/multistage-build/
[docker-hub-windows-core]: https://hub.docker.com/r/microsoft/windowsservercore/
[docker-hub-windows-nano]: https://hub.docker.com/r/microsoft/nanoserver/

<!-- LINKS - Internal -->
[az-container-show]: /cli/azure/container#az_container_show
