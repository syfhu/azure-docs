---
title: Private Docker container registries in Azure
description: Introduction to the Azure Container Registry service, providing cloud-based, managed, private Docker registries.
services: container-registry
author: stevelas
manager: jeconnoc

ms.service: container-registry
ms.topic: overview
ms.date: 05/08/2018
ms.author: stevelas
ms.custom: mvc
---
# Introduction to private Docker container registries in Azure

Azure Container Registry is a managed [Docker registry](https://docs.docker.com/registry/) service based on the open-source Docker Registry 2.0. Create and maintain Azure container registries to store and manage your private [Docker container](https://www.docker.com/what-docker) images.

Use container registries in Azure with your existing container development and deployment pipelines. Use Azure Container Registry Build (ACR Build) to build container images in Azure. Build on demand, or fully automate builds with source code commit and base image update build triggers.

For background about Docker and containers, see the [Docker overview](https://docs.docker.com/engine/docker-overview/).

## Use cases

Pull images from an Azure container registry to various deployment targets:

* **Scalable orchestration systems** that manage containerized applications across clusters of hosts, including [DC/OS](https://docs.mesosphere.com/), [Docker Swarm](https://docs.docker.com/swarm/), and [Kubernetes](http://kubernetes.io/docs/).
* **Azure services** that support building and running applications at scale, including [Azure Kubernetes Service (AKS)](../aks/index.yml), [App Service](/app-service/index.md), [Batch](../batch/index.yml), [Service Fabric](/azure/service-fabric/), and others.

Developers can also push to a container registry as part of a container development workflow. For example, target a container registry from a continuous integration and deployment tool such as [Visual Studio Team Services](https://www.visualstudio.com/docs/overview) or [Jenkins](https://jenkins.io/).

Configure [ACR Build](#azure-container-registry-build) build tasks to automatically rebuild application images when their base images are updated. Use ACR Build to automate image builds when your team commits code to a Git repository. *ACR Build is currently in preview.*

## Key concepts

* **Registry** - Create one or more container registries in your Azure subscription. Registries are available in three SKUs: [Basic, Standard, and Premium](container-registry-skus.md), each of which support webhook integration, registry authentication with Azure Active Directory, and delete functionality. Take advantage of local, network-close storage of your container images by creating a registry in the same Azure location as your deployments. Use the [geo-replication](container-registry-geo-replication.md) feature of Premium registries for advanced replication and container image distribution scenarios. A fully qualified registry name has the form `myregistry.azurecr.io`.

  You [control access](container-registry-authentication.md) to a container registry using an Azure Active Directory-backed [service principal](../active-directory/active-directory-application-objects.md) or a provided admin account. Run the standard `docker login` command to authenticate with a registry.

* **Repository** - A registry contains one or more repositories, which are groups of container images. Azure Container Registry supports multilevel repository namespaces. With multilevel namespaces, you can group collections of images related to a specific app, or a collection of apps to specific development or operational teams. For example:

  * `myregistry.azurecr.io/aspnetcore:1.0.1` represents a corporate-wide image
  * `myregistry.azurecr.io/warrantydept/dotnet-build` represents an image used to build .NET apps, shared across the warranty department
  * `myregistry.azurecr.io/warrantydept/customersubmissions/web` represents a web image, grouped in the customer submissions app, owned by the warranty department

* **Image** - Stored in a repository, each image is a read-only snapshot of a Docker container. Azure container registries can include both Windows and Linux images. You control image names for all your container deployments. Use standard [Docker commands](https://docs.docker.com/engine/reference/commandline/) to push images into a repository, or pull an image from a repository.

* **Container** - A container defines a software application and its dependencies wrapped in a complete filesystem including code, runtime, system tools, and libraries. Run Docker containers based on Windows or Linux images that you pull from a container registry. Containers running on a single machine share the operating system kernel. Docker containers are fully portable to all major Linux distros, macOS, and Windows.

## Azure Container Registry Build (Preview)

[Azure Container Registry Build](container-registry-build-overview.md) (ACR Build) is a suite of features within Azure Container Registry that provides streamlined and efficient Docker container image builds in Azure. Use ACR Build to extend your development inner-loop to the cloud by offloading `docker build` operations to Azure. Configure build tasks to automate your container OS and framework patching pipeline, and build images automatically when your team commits code to source control.

[!INCLUDE [container-registry-build-preview-note](../../includes/container-registry-build-preview-note.md)]

## Next steps

* [Create a container registry using the Azure portal](container-registry-get-started-portal.md)
* [Create a container registry using the Azure CLI](container-registry-get-started-azure-cli.md)
* [Automate OS and framework patching with ACR Build](container-registry-build-overview.md) (Preview)
