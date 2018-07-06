---
title: Tutorial - Automate container image builds on base image update with Azure Container Registry Build
description: In this tutorial, you learn how to configure a build task to automatically trigger container image builds in the cloud when a base image is updated.
services: container-registry
author: mmacy
manager: jeconnoc

ms.service: container-registry
ms.topic: tutorial
ms.date: 05/11/2018
ms.author: marsma
ms.custom: mvc
# Customer intent: As a developer or devops engineer, I want container
# images to be built automatically when the base image of a container is
# updated in the registry.
---

# Tutorial: Automate image builds on base image update with Azure Container Registry Build

ACR Build supports automated build execution when a container's base image is updated, such as when you patch the OS or application framework in one of your base images. In this tutorial, you learn how to create a build task in ACR Build that triggers a build in the cloud when a container's base image has been pushed to your registry.

In this tutorial, the last in the series:

> [!div class="checklist"]
> * Build the base image
> * Create an application image build task
> * Update the base image to trigger an application image build
> * Display the triggered build
> * Verify updated application image

[!INCLUDE [container-registry-build-preview-note](../../includes/container-registry-build-preview-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

If you'd like to use the Azure CLI locally, you must have the Azure CLI version **2.0.32** or later installed. Run `az --version` to find the version. If you need to install or upgrade the CLI, see [Install Azure CLI][azure-cli].

## Prerequisites

### Complete previous tutorials

This tutorial assumes you've already completed the steps in the first two tutorials in the series, in which you:

* Create Azure container registry
* Fork sample repository
* Clone sample repository
* Create GitHub personal access token

If you haven't already done so, complete the first two tutorials before proceeding:

[Build container images in the cloud with Azure Container Registry Build](container-registry-tutorial-quick-build.md)

[Automate container image builds with Azure Container Registry Build](container-registry-tutorial-build-task.md)

### Configure environment

Populate these shell environment variables with values appropriate for your environment. This isn't strictly required, but makes executing the multiline Azure CLI commands in this tutorial a bit easier. If you don't populate these, you must manually replace each value wherever they appear in the example commands.

```azurecli-interactive
ACR_NAME=mycontainerregistry # The name of your Azure container registry
GIT_USER=gituser             # Your GitHub user account name
GIT_PAT=personalaccesstoken  # The PAT you generated in the second tutorial
```

## Base images

Dockerfiles defining most container images specify a parent image from which it is based, often referred to as its *base image*. Base images typically contain the operating system, for example [Alpine Linux][base-alpine] or [Windows Nano Server][base-windows], on which the rest of the container's layers are applied. They might also include application frameworks such as [Node.js][base-node] or [.NET Core][base-dotnet].

### Base image updates

A base image is often updated by the image maintainer to include new features or improvements to the OS or framework in the image. Security patches are another common cause for a base image update.

When a base image is updated, you're presented with the need to rebuild any container images in your registry based on it to include the new features and fixes. ACR Build includes the ability to automatically build images for you when a container's base image is updated.

### Base image update scenario

This tutorial walks you through a base image update scenario. The [code sample][code-sample] includes two Dockerfiles: an application image, and an image it specifies as its base. In the following sections, you create an ACR Build task that automatically triggers a build of the application image when a new version of the base image is pushed to your container registry.

[Dockerfile-app][dockerfile-app]: A small Node.js web application that renders a static web page displaying the Node.js version on which it's based. The version string is simulated, in that it displays the contents of an environment variable, `NODE_VERSION`, that's defined in the base image.

[Dockerfile-base][dockerfile-base]: The image that `Dockerfile-app` specifies as its base. It is itself based on a [Node][base-node] image, and includes the `NODE_VERSION` environment variable.

In the following sections, you create a build task, update the `NODE_VERSION` value in the base image Dockerfile, then use ACR Build to build the base image. When ACR Build pushes the new base image to your registry, it automatically triggers a build of the application image. Optionally, you run the application container image locally to see the different version strings in the built images.

## Build base image

Start by building the base image with an ACR Build *Quick Build*. As discussed in the [first tutorial](container-registry-tutorial-quick-build.md) in the series, this not only builds the image, but pushes it to your container registry if the build is successful.

```azurecli-interactive
az acr build --registry $ACR_NAME --image baseimages/node:9-alpine --file Dockerfile-base .
```

## Create build task

Next, create a build task with [az acr build-task create][az-acr-build-task-create]:

```azurecli-interactive
az acr build-task create \
    --registry $ACR_NAME \
    --name buildhelloworld \
    --image helloworld:{{.Build.ID}} \
    --build-arg REGISTRY_NAME=$ACR_NAME.azurecr.io \
    --context https://github.com/$GIT_USER/acr-build-helloworld-node \
    --file Dockerfile-app \
    --branch master \
    --git-access-token $GIT_PAT
```

This build task is similar to the task created in the [previous tutorial](container-registry-tutorial-build-task.md). It instructs ACR Build to trigger an image build when commits are pushed to the repository specified by `--context`.

Where it differs is in its behavior, in that it also triggers a build of the image when its *base image* is updated. The Dockerfile specified by the `--file` argument, [Dockerfile-app][dockerfile-app], supports specifying an image from within the same registry as its base:

```Dockerfile
FROM ${REGISTRY_NAME}/baseimages/node:9-alpine
```

When you run a build task, ACR Build detects an image's dependencies. If the base image specified in the `FROM` statement resides within the same registry, it adds a hook to ensure this image is rebuilt any time its base is updated.

> [!NOTE]
> During preview, ACR Build supports triggering a derived image build only when both the base image and the image referencing it reside in the same Azure container registry.

## Build application container

To enable ACR Build to determine and track a container image's dependencies--which include its base image--you **must** first trigger its build task **at least once**.

Use [az acr build-task run][az-acr-build-task-run] to manually trigger the build task and build the application image:

```azurecli-interactive
az acr build-task run --registry $ACR_NAME --name buildhelloworld
```

Once the build has completed, take note of the **Build ID** (for example, "aa6") if you wish to complete the following optional step.

### Optional: Run application container locally

If you're working locally (not in the Cloud Shell), and you have Docker installed, run the container to see the application rendered in a web browser before you rebuild its base image. If you're using the Cloud Shell, skip this section (Cloud Shell does not support `az acr login` or `docker run`).

First, log in to your container registry with [az acr login][az-acr-login]:

```azurecli
az acr login --name $ACR_NAME
```

Now, run the container locally with `docker run`. Replace **\<build-id\>** with the Build ID found in the output from the previous step (for example, "aa6").

```azurecli
docker run -d -p 8080:80 $ACR_NAME.azurecr.io/helloworld:<build-id>
```

Navigate to http://localhost:8080 in your browser, and you should see the Node.js version number rendered in the web page, similar to the following. In a later step, you bump the version by adding an "a" to the version string.

![Screenshot of sample application rendered in browser][base-update-01]

## List builds

Next, list the builds that ACR Build has completed for your registry:

```azurecli-interactive
az acr build-task list-builds --registry $ACR_NAME --output table
```

If you completed the previous tutorial (and didn't delete the registry), you should see output similar to the following. Take note of the number of builds, and the latest BUILD ID, so you can compare the output after you update the base image in the next section.

```console
$ az acr build-task list-builds --registry $ACR_NAME --output table
BUILD ID    TASK             PLATFORM    STATUS     TRIGGER     STARTED               DURATION
----------  ---------------  ----------  ---------  ----------  --------------------  ----------
aa6         buildhelloworld  Linux       Succeeded  Manual      2018-05-10T20:00:12Z  00:00:50
aa5                          Linux       Succeeded  Manual      2018-05-10T19:57:35Z  00:00:55
aa4         buildhelloworld  Linux       Succeeded  Git Commit  2018-05-10T19:49:40Z  00:00:45
aa3         buildhelloworld  Linux       Succeeded  Manual      2018-05-10T19:41:50Z  00:01:20
aa2         buildhelloworld  Linux       Succeeded  Manual      2018-05-10T19:37:11Z  00:00:50
aa1                          Linux       Succeeded  Manual      2018-05-10T19:10:14Z  00:00:55
```

## Update base image

Here you simulate a framework patch in the base image. Edit **Dockerfile-base**, and add an "a" after the version number defined in `NODE_VERSION`:

```Dockerfile
ENV NODE_VERSION 9.11.1a
```

Run a Quick Build in ACR Build to build the modified base image. Take note of the **Build ID** in the output.

```azurecli-interactive
az acr build --registry $ACR_NAME --image baseimages/node:9-alpine --file Dockerfile-base .
```

Once the build is complete and ACR Build has pushed the new base image to your registry, it triggers a build of the application image. It may take few moments for the ACR Build task you created earlier to trigger the application image build, as it must detect the newly completed and pushed base image.

## List builds

Now that you've updated the base image, list your builds again to compare to the earlier list. If at first the output doesn't differ, periodically run the command to see the new build appear in the list.

```azurecli-interactive
az acr build-task list-builds --registry $ACR_NAME --output table
```

Output is similar to the following. The TRIGGER for the last-executed build should be "Image Update", indicating that the build task was kicked off by your Quick Build of the base image.

```console
$ az acr build-task list-builds --registry $ACR_NAME --output table
BUILD ID    TASK             PLATFORM    STATUS     TRIGGER       STARTED               DURATION
----------  ---------------  ----------  ---------  ------------  --------------------  ----------
aa8         buildhelloworld  Linux       Succeeded  Image Update  2018-05-10T20:09:52Z  00:00:45
aa7                          Linux       Succeeded  Manual        2018-05-10T20:09:17Z  00:00:40
aa6         buildhelloworld  Linux       Succeeded  Manual        2018-05-10T20:00:12Z  00:00:50
aa5                          Linux       Succeeded  Manual        2018-05-10T19:57:35Z  00:00:55
aa4         buildhelloworld  Linux       Succeeded  Git Commit    2018-05-10T19:49:40Z  00:00:45
aa3         buildhelloworld  Linux       Succeeded  Manual        2018-05-10T19:41:50Z  00:01:20
aa2         buildhelloworld  Linux       Succeeded  Manual        2018-05-10T19:37:11Z  00:00:50
aa1                          Linux       Succeeded  Manual        2018-05-10T19:10:14Z  00:00:55
```

If you'd like to perform the following optional step of running the newly built container to see the updated version number, take note of the **BUILD ID** value for the Image Update-triggered build (in the preceding output, it's "aa8").

### Optional: Run newly built image

If you're working locally (not in the Cloud Shell), and you have Docker installed, run the new application image once its build has completed. Replace `<build-id>` with the BUILD ID you obtained in the previous step. If you're using the Cloud Shell, skip this section (Cloud Shell does not support `docker run`).

```bash
docker run -d -p 8081:80 $ACR_NAME.azurecr.io/helloworld:<build-id>
```

Navigate to http://localhost:8081 in your browser, and you should see the updated Node.js version number (with the "a") in the web page:

![Screenshot of sample application rendered in browser][base-update-02]

What's important to note is that you updated your **base** image with a new version number, but the last-built **application** image displays the new version. ACR Build picked up your change to the base image, and rebuilt your application image automatically.

## Clean up resources

To remove all resources you've created in this tutorial series, including the container registry, container instance, key vault, and service principal, issue the following commands:

```azurecli-interactive
az group delete --resource-group $RES_GROUP
az ad sp delete --id http://$ACR_NAME-pull
```

## Next steps

In this tutorial, you learned how to use a build task to automatically trigger container image builds when the image's base image has been updated. Now, move on to learning about authentication for your container registry.

> [!div class="nextstepaction"]
> [Authentication in Azure Container Registry](container-registry-authentication.md)

<!-- LINKS - External -->
[base-alpine]: https://hub.docker.com/_/alpine/
[base-dotnet]: https://hub.docker.com/r/microsoft/dotnet/
[base-node]: https://hub.docker.com/_/node/
[base-windows]: https://hub.docker.com/r/microsoft/nanoserver/
[code-sample]: https://github.com/Azure-Samples/acr-build-helloworld-node
[dockerfile-app]: https://github.com/Azure-Samples/acr-build-helloworld-node/blob/master/Dockerfile-app
[dockerfile-base]: https://github.com/Azure-Samples/acr-build-helloworld-node/blob/master/Dockerfile-base

<!-- LINKS - Internal -->
[azure-cli]: /cli/azure/install-azure-cli
[az-acr-build]: /cli/azure/acr#az-acr-build-run
[az-acr-build-task-create]: /cli/azure/acr#az-acr-build-task-create
[az-acr-build-task-run]: /cli/azure/acr#az-acr-build-task-run
[az-acr-login]: /cli/azure/acr#az-acr-login

<!-- IMAGES -->
[base-update-01]: ./media/container-registry-tutorial-base-image-update/base-update-01.png
[base-update-02]: ./media/container-registry-tutorial-base-image-update/base-update-02.png