---
title: Set environment variables in Azure Container Instances
description: Learn how to set environment variables in the containers you run in Azure Container Instances
services: container-instances
author: mmacy
manager: jeconnoc

ms.service: container-instances
ms.topic: article
ms.date: 06/07/2018
ms.author: marsma
---
# Set environment variables

Setting environment variables in your container instances allows you to provide dynamic configuration of the application or script run by the container. To set environment variables in a container, specify them when you create a container instance. You can set environment variables when you start a container with the [Azure CLI](#azure-cli-example), [Azure PowerShell](#azure-powershell-example), and the [Azure portal](#azure-portal-example).

For example, if you run the [microsoft/aci-wordcount][aci-wordcount] container image, you can modify its behavior by specifying the following environment variables:

*NumWords*: The number of words sent to STDOUT.

*MinLength*: The minimum number of characters in a word for it to be counted. A higher number ignores common words like "of" and "the."

If you need to pass secrets as environment variables, Azure Container Instances supports [secure values](#secure-values) secure values for both Windows and Linux containers.

## Azure CLI example

To see the default output of the [microsoft/aci-wordcount][aci-wordcount] container, run it first with this [az container create][az-container-create] command (no environment variables specified):

```azurecli-interactive
az container create \
    --resource-group myResourceGroup \
    --name mycontainer1 \
    --image microsoft/aci-wordcount:latest \
    --restart-policy OnFailure
```

To modify the output, start a second container with the `--environment-variables` argument added, specifying values for the *NumWords* and *MinLength* variables:

```azurecli-interactive
az container create \
    --resource-group myResourceGroup \
    --name mycontainer2 \
    --image microsoft/aci-wordcount:latest \
    --restart-policy OnFailure \
    --environment-variables NumWords=5 MinLength=8
```

Once both containers' state shows as *Terminated* (use [az container show][az-container-show] to check state), display their logs with [az container logs][az-container-logs] to see the output.

```azurecli-interactive
az container logs --resource-group myResourceGroup --name mycontainer1
az container logs --resource-group myResourceGroup --name mycontainer2
```

The output of the containers show how you've modified the second container's script behavior by setting environment variables.

```console
azureuser@Azure:~$ az container logs --resource-group myResourceGroup --name mycontainer1
[('the', 990),
 ('and', 702),
 ('of', 628),
 ('to', 610),
 ('I', 544),
 ('you', 495),
 ('a', 453),
 ('my', 441),
 ('in', 399),
 ('HAMLET', 386)]

azureuser@Azure:~$ az container logs --resource-group myResourceGroup --name mycontainer2
[('CLAUDIUS', 120),
 ('POLONIUS', 113),
 ('GERTRUDE', 82),
 ('ROSENCRANTZ', 69),
 ('GUILDENSTERN', 54)]
```

## Azure PowerShell example

Setting environment variables in PowerShell is similar to the CLI, but uses the `-EnvironmentVariable` command-line argument.

First, launch the [microsoft/aci-wordcount][aci-wordcount] container in its default configuration with this [New-AzureRmContainerGroup][new-azurermcontainergroup] command:

```azurepowershell-interactive
New-AzureRmContainerGroup `
    -ResourceGroupName myResourceGroup `
    -Name mycontainer1 `
    -Image microsoft/aci-wordcount:latest
```

Now run the following [New-AzureRmContainerGroup][new-azurermcontainergroup] command. This one specifies the *NumWords* and *MinLength* environment variables after populating an array variable, `envVars`:

```azurepowershell-interactive
$envVars = @{NumWords=5;MinLength=8}
New-AzureRmContainerGroup `
    -ResourceGroupName myResourceGroup `
    -Name mycontainer2 `
    -Image microsoft/aci-wordcount:latest `
    -RestartPolicy OnFailure `
    -EnvironmentVariable $envVars
```

Once both containers' state is *Terminated* (use [Get-AzureRmContainerInstanceLog][azure-instance-log] to check state), pull their logs with the [Get-AzureRmContainerInstanceLog][azure-instance-log] command.

```azurepowershell-interactive
Get-AzureRmContainerInstanceLog -ResourceGroupName myResourceGroup -ContainerGroupName mycontainer1
Get-AzureRmContainerInstanceLog -ResourceGroupName myResourceGroup -ContainerGroupName mycontainer2
```

The output for each container shows how you've modified the script run by the container by setting environment variables.

```console
PS Azure:\> Get-AzureRmContainerInstanceLog -ResourceGroupName myResourceGroup -ContainerGroupName mycontainer1
[('the', 990),
 ('and', 702),
 ('of', 628),
 ('to', 610),
 ('I', 544),
 ('you', 495),
 ('a', 453),
 ('my', 441),
 ('in', 399),
 ('HAMLET', 386)]

Azure:\
PS Azure:\> Get-AzureRmContainerInstanceLog -ResourceGroupName myResourceGroup -ContainerGroupName mycontainer2
[('CLAUDIUS', 120),
 ('POLONIUS', 113),
 ('GERTRUDE', 82),
 ('ROSENCRANTZ', 69),
 ('GUILDENSTERN', 54)]

Azure:\
```

## Azure portal example

To set environment variables when you start a container in the Azure portal, specify them in the **Configuration** page when you create the container.

When you deploy with the portal, you're currently limited to three variables, and you must enter them in this format: `"variableName":"value"`

To see an example, start the [microsoft/aci-wordcount][aci-wordcount] container with the *NumWords* and *MinLength* variables.

1. In **Configuration**, set the **Restart policy** to *On failure*
2. Enter `"NumWords":"5"` for the first variable, select **Yes** under **Add additional environment variables**, and enter `"MinLength":"8"` for the second variable. Select **OK** to verify and then deploy the container.

![Portal page showing environment variable Enable button and text boxes][portal-env-vars-01]

To view the container's logs, under **SETTINGS** select **Containers**, then **Logs**. Similar to the output shown in the previous CLI and PowerShell sections, you can see how the script's behavior has been modified by the environment variables. Only five words are displayed, each with a minimum length of eight characters.

![Portal showing container log output][portal-env-vars-02]

## Secure values
Objects with secure values are intended to hold sensitive information like passwords or keys for your application. Using secure values for environment variables is both safer and more flexible than including it in your container's image. Another option is to use secret volumes, described in [Mount a secret volume in Azure Container Instances](container-instances-volume-secret.md).

Secure environment variables with secure values won't reveal the secure value in your container's properties so the value can only be accessed from within your container. For example, container properties viewed in the Azure portal or Azure CLI won't display an environment variable with a secure value.

Set a secure environment variable by specifying the `secureValue` property instead of the regular `value` for the variable's type. The two variables defined in the following YAML demonstrate the two variable types.

### YAML deployment

Create a `secure-env.yaml` file with the following snippet.

```yaml
apiVersion: 2018-06-01
location: westus
name: securetest
properties:
  containers:
  - name: mycontainer
    properties:
      environmentVariables:
        - "name": "SECRET"
          "secureValue": "my-secret-value"
        - "name": "NOTSECRET"
          "value": "my-exposed-value"
      image: nginx
      ports: []
      resources:
        requests:
          cpu: 1.0
          memoryInGB: 1.5
  osType: Linux
  restartPolicy: Always
tags: null
type: Microsoft.ContainerInstance/containerGroups
```

Run the following command to deploy the container group with YAML.

```azurecli-interactive
az container create --resource-group myRG --name securetest -f secure-env.yaml
```

### Verify environment variables

Run the following command to query for your container's environment variables.

```azurecli-interactive
az container show --resource-group myRG --name securetest --query 'containers[].environmentVariables`
```

The JSON response with details for this container will show only the non-secure environment variable and secure environment variable's key.

```json
  "environmentVariables": [
    {
      "name": "NOTSECRET",
      "value": "my-exposed-value"
    },
    {
      "name": "SECRET"
    }
```

You can review the secure environment variable is set with the `exec` command which enables executing a command from within a running container. 

Run the following command to start an interactive bash session with the container.
```azurecli-interactive
az container exec --resource-group myRG --name securetest --exec-command "/bin/bash"
```

From within your container, print your environment variable with the following bash command.
```bash
echo $SECRET
```

## Next steps

Task-based scenarios, such as batch processing a large dataset with several containers, can benefit from custom environment variables at runtime. For more information about running task-based containers, see [Run containerized tasks in Azure Container Instances](container-instances-restart-policy.md).

<!-- IMAGES -->
[portal-env-vars-01]: ./media/container-instances-environment-variables/portal-env-vars-01.png
[portal-env-vars-02]: ./media/container-instances-environment-variables/portal-env-vars-02.png

<!-- LINKS - External -->
[aci-wordcount]: https://hub.docker.com/r/microsoft/aci-wordcount/

<!-- LINKS Internal -->
[az-container-create]: /cli/azure/container#az-container-create
[az-container-logs]: /cli/azure/container#az-container-logs
[az-container-show]: /cli/azure/container#az-container-show
[azure-cli-install]: /cli/azure/
[azure-instance-log]: /powershell/module/azurerm.containerinstance/get-azurermcontainerinstancelog
[azure-powershell-install]: /powershell/azure/install-azurerm-ps
[new-azurermcontainergroup]: /powershell/module/azurerm.containerinstance/new-azurermcontainergroup
[portal]: https://portal.azure.com
