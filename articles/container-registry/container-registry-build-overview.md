---
title: Automate OS and framework patching with Azure Container Registry Build (ACR Build)
description: An introduction to ACR Build, a suite of features in Azure Container Registry that provides secure, automated container image build and patching in the cloud.
services: container-registry
author: mmacy
manager: jeconnoc

ms.service: container-registry
ms.topic: article
ms.date: 05/01/2018
ms.author: marsma
---

# Automate OS and framework patching with ACR Build

Containers provide new levels of virtualization, isolating application and developer dependencies from infrastructure and operational requirements. What remains, however, is the need to address how this application virtualization is patched.

**ACR Build**, a suite of features within Azure Container Registry, provides not only native container image build capability, but also automates [OS and framework patching](#automate-os-and-framework-patching) for your Docker containers.

[!INCLUDE [container-registry-build-preview-note](../../includes/container-registry-build-preview-note.md)]

## What is ACR Build?

Azure Container Registry Build is an Azure-native container image build service. ACR Build enables inner-loop development in the cloud with on-demand container image builds, and automated builds on source code commit and base image update.

Trigger container image builds automatically when code is committed to a Git repository, or when a container's base image is updated. With base image update triggers, you can automate your OS and application framework patching workflow, maintaining secure environments while adhering to the principals of immutable containers.

## Quick Build: inner-loop extended to the cloud

The beginning of lifecycle management starts before developers commit their first lines of code. ACR Build's [Quick Build](container-registry-tutorial-quick-build.md) feature enables an integrated local inner-loop development experience, offloading builds to Azure. With Quick Builds, you can verify your automated build definitions prior to committing your code.

Using the familiar `docker build` format, the [az acr build][az-acr-build] command in the Azure CLI takes a local context, sends it to the ACR Build service and, by default, pushes the built image to its registry upon completion. ACR Build follows your geo-replicated registries, enabling dispersed development teams to leverage the closest replicated registry. During preview, ACR build is available in the East US and West Europe regions.

ACR Build is designed as a container lifecycle primitive. For example, integrate ACR Build into your CI/CD solution. By executing [az login][az-login] with a [service principal][az-login-service-principal], your CI/CD solution could then issue [az acr build][az-acr-build] commands to kick off image builds.

Learn how to use Quick Builds in the first ACR Build tutorial, [Build container images in the cloud with Azure Container Registry Build](container-registry-tutorial-quick-build.md).

## Automatic build on source code commit

Use ACR Build to automatically trigger a container image build when code is committed to a Git repository. Build tasks, configurable with the Azure CLI command [az acr build-task][az-acr-build-task], allow you to specify a Git repository and optionally a branch and Dockerfile. When your team commits code to the repository, an ACR Build-created webhook triggers a build of the container image defined in the repo.

Learn how to trigger builds on source code commit in the second ACR Build tutorial, [Automate container image builds with Azure Container Registry Build](container-registry-tutorial-build-task.md).

## Automate OS and framework patching

The power of ACR Build to truly enhance your container build pipeline comes from its ability to detect an update to a base image. When the updated base image is pushed to your registry, ACR Build can automatically build any application images based on it.

Container images can be broadly categorized into *base* images and *application* images. Your base images typically include the operating system and application frameworks upon which your application is built, along with other customizations. These base images are themselves typically based on public upstream images, for example [Alpine Linux][base-alpine] or [Node.js][base-node]. Several of your application images might share a common base image.

When an OS or app framework image is updated by the upstream maintainer, for example with a critical OS security patch, you must also update your base images to include the critical fix. Each application image must then also be rebuilt to include these upstream fixes now included in your base image.

Because ACR Build dynamically discovers base image dependencies when it builds a container image, it can detect when an application image's base image is updated. With one preconfigured [build task](container-registry-tutorial-base-image-update.md#create-build-task), ACR Build then **automatically rebuilds every application image** for you. With this automatic detection and rebuilding, ACR Build saves you the time and effort normally required to manually track and update each and every application image referencing your updated base image.

Learn about OS and framework patching in the third ACR Build tutorial, [Automate image builds on base image update with Azure Container Registry Build](container-registry-tutorial-base-image-update.md).

> [!NOTE]
> For the initial preview, base image updates trigger builds only when both the base and application images reside in the same Azure container registry.

## Next steps

When you're ready to automate OS and framework patching by building your container images in the cloud, check out the three-part ACR Build tutorial series.

> [!div class="nextstepaction"]
> [Build container images in the cloud with Azure Container Registry Build](container-registry-tutorial-quick-build.md)

<!-- LINKS - External -->
[base-alpine]: https://hub.docker.com/_/alpine/
[base-dotnet]: https://hub.docker.com/r/microsoft/dotnet/
[base-node]: https://hub.docker.com/_/node/
[base-windows]: https://hub.docker.com/r/microsoft/nanoserver/
[sample-archive]: https://github.com/Azure-Samples/acr-build-helloworld-node/archive/master.zip

<!-- LINKS - Internal -->
[azure-cli]: /cli/azure/install-azure-cli
[az-acr-build]: /cli/azure/acr#az-acr-build
[az-acr-build-task]: /cli/azure/acr#az-acr-build-task
[az-login]: /cli/azure/reference-index#az-login
[az-login-service-principal]: /cli/azure/authenticate-azure-cli#log-in-with-a-service-principal

<!-- IMAGES -->
[quick-build-01-fork]: ./media/container-registry-tutorial-quick-build/quick-build-01-fork.png
[quick-build-02-browser]: ./media/container-registry-tutorial-quick-build/quick-build-02-browser.png
