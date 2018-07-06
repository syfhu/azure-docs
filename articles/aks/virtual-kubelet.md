---
title: Run virtual kubelet in an Azure Kubernetes Service (AKS) cluster
description: Use virtual kubelet to run Kubernetes containers on Azure Container Instances.
services: container-service
author: iainfoulds
manager: jeconnoc

ms.service: container-service
ms.topic: article
ms.date: 06/12/2018
ms.author: iainfou
---

# Virtual Kubelet with AKS

Azure Container Instances (ACI) provide a hosted environment for running containers in Azure. When using ACI, there is no need to manage the underlying compute infrastructure, Azure handles this management for you. When running containers in ACI, you are charged by the second for each running container.

When using the Virtual Kubelet provider for Azure Container Instances, both Linux and Windows containers can be scheduled on a container instance as if it is a standard Kubernetes node. This configuration allows you to take advantage of both the capabilities of Kubernetes and the management value and cost benefit of container instances.

> [!NOTE]
> Virtual Kubelet is an experimental open source project and should be used as such. To contribute, file issues, and read more about virtual kubelet, see the [Virtual Kubelet GitHub project][vk-github].

This document details configuring the Virtual Kubelet for container instances on an AKS.

## Prerequisite

This document assumes that you have an AKS cluster. If you need an AKS cluster, see the [Azure Kubernetes Service (AKS) quickstart][aks-quick-start].

You also need the Azure CLI version **2.0.33** or later. Run `az --version` to find the version. If you need to install or upgrade, see [Install Azure CLI](/cli/azure/install-azure-cli).

[Helm](https://docs.helm.sh/using_helm/#installing-helm) is also required in order to install the Virtual Kubelet.

## Installation

Use the [az aks install-connector][aks-install-connector] command to install Virtual Kubelet. The following example deploys both the Linux and Windows connector.

```azurecli-interactive
az aks install-connector --resource-group myAKSCluster --name myAKSCluster --connector-name virtual-kubelet --os-type Both
```

These arguments are available for the `aks install-connector` command.

| Argument: | Description | Required |
|---|---|:---:|
| `--connector-name` | Name of the ACI Connector.| Yes |
| `--name` `-n` | Name of the managed cluster. | Yes |
| `--resource-group` `-g` | Name of resource group. | Yes |
| `--os-type` | Container instances operating system type. Allowed values: Both, Linux, Windows. Default: Linux. | No |
| `--aci-resource-group` | The resource group in which to create the ACI container groups. | No |
| `--location` `-l` | The location to create the ACI container groups. | No |
| `--service-principal` | Service principal used for authentication to Azure APIs. | No |
| `--client-secret` | Secret associated with the service principal. | No |
| `--chart-url` | URL of a Helm chart that installs ACI Connector. | No |
| `--image-tag` | The image tag of the virtual kubelet container image. | No |

## Validate Virtual Kubelet

To validate that Virtual Kubelet has been installed, return a list of Kubernetes nodes using the [kubectl get nodes][kubectl-get] command.

```console
$ kubectl get nodes

NAME                                    STATUS    ROLES     AGE       VERSION
aks-nodepool1-23443254-0                Ready     agent     16d       v1.9.6
aks-nodepool1-23443254-1                Ready     agent     16d       v1.9.6
aks-nodepool1-23443254-2                Ready     agent     16d       v1.9.6
virtual-kubelet-virtual-kubelet-linux   Ready     agent     4m        v1.8.3
virtual-kubelet-virtual-kubelet-win     Ready     agent     4m        v1.8.3
```

## Run Linux container

Create a file named `virtual-kubelet-linux.yaml` and copy in the following YAML. Replace the `kubernetes.io/hostname` value with the name of the Linux Virtual Kubelet node. Take note that a [nodeSelector][node-selector] and [toleration][toleration] are being used to schedule the container on the node.

```yaml
apiVersion: apps/v1beta1
kind: Deployment
metadata:
  name: aci-helloworld
spec:
  replicas: 1
  template:
    metadata:
      labels:
        app: aci-helloworld
    spec:
      containers:
      - name: aci-helloworld
        image: microsoft/aci-helloworld
        ports:
        - containerPort: 80
      nodeSelector:
        kubernetes.io/hostname: virtual-kubelet-virtual-kubelet-linux
      tolerations:
      - key: azure.com/aci
        effect: NoSchedule
```

Run the application with the [kubectl create][kubectl-create] command.

```azurecli-interactive
kubectl create -f virtual-kubelet-linux.yaml
```

Use the [kubectl get pods][kubectl-get] command with the `-o wide` argument to output a list of pods with the scheduled node. Notice that the `aci-helloworld` pod has been scheduled on the `virtual-kubelet-virtual-kubelet-linux` node.

```console
$ kubectl get pods -o wide

NAME                                READY     STATUS    RESTARTS   AGE       IP             NODE
aci-helloworld-2559879000-8vmjw     1/1       Running   0          39s       52.179.3.180   virtual-kubelet-virtual-kubelet-linux
```

## Run Windows container

Create a file named `virtual-kubelet-windows.yaml` and copy in the following YAML. Replace the `kubernetes.io/hostname` value with the name of the Windows Virtual Kubelet node. Take note that a [nodeSelector][node-selector] and [toleration][toleration] are being used to schedule the container on the node.

```yaml
apiVersion: apps/v1beta1
kind: Deployment
metadata:
  name: nanoserver-iis
spec:
  replicas: 1
  template:
    metadata:
      labels:
        app: nanoserver-iis
    spec:
      containers:
      - name: nanoserver-iis
        image: nanoserver/iis
        ports:
        - containerPort: 80
      nodeSelector:
        kubernetes.io/hostname: virtual-kubelet-virtual-kubelet-win
      tolerations:
      - key: azure.com/aci
        effect: NoSchedule
```

Run the application with the [kubectl create][kubectl-create] command.

```azurecli-interactive
kubectl create -f virtual-kubelet-windows.yaml
```

Use the [kubectl get pods][kubectl-get] command with the `-o wide` argument to output a list of pods with the scheduled node. Notice that the `nanoserver-iis` pod has been scheduled on the `virtual-kubelet-virtual-kubelet-win` node.

```console
$ kubectl get pods -o wide

NAME                                READY     STATUS    RESTARTS   AGE       IP             NODE
nanoserver-iis-868bc8d489-tq4st     1/1       Running   8         21m       138.91.121.91   virtual-kubelet-virtual-kubelet-win
```

## Remove Virtual Kubelet

Use the [az aks remove-connector][aks-remove-connector] command to remove Virtual Kubelet. Replace the argument values with the name of the connector, AKS cluster, and the AKS cluster resource group.

```azurecli-interactive
az aks remove-connector --resource-group myAKSCluster --name myAKSCluster --connector-name virtual-kubelet
```

## Next steps

Read more about Virtual Kubelet at the [Virtual Kubelet Github projet][vk-github].

<!-- LINKS - internal -->
[aks-quick-start]: ./kubernetes-walkthrough.md
[aks-remove-connector]: /cli/azure/aks#az-aks-remove-connector
[az-container-list]: /cli/azure/aks#az_aks_list
[aks-install-connector]: /cli/azure/aks#az-aks-install-connector

<!-- LINKS - external -->
[kubectl-create]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#create
[kubectl-get]: https://kubernetes.io/docs/user-guide/kubectl/v1.8/#get
[node-selector]:https://kubernetes.io/docs/concepts/configuration/assign-pod-node/
[toleration]: https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/
[vk-github]: https://github.com/virtual-kubelet/virtual-kubelet
