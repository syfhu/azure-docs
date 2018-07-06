---
title: 'Step 1: Create a Machine Learning workspace | Microsoft Docs'
description: 'Step 1 of the Develop a predictive solution walkthrough: Learn how to set up a new Azure Machine Learning Studio workspace.'
services: machine-learning
documentationcenter: ''
author: heatherbshapiro
ms.author: hshapiro
manager: hjerez
editor: cgronlun

ms.assetid: b3c97e3d-16ba-4e42-9657-2562854a1e04
ms.service: machine-learning
ms.component: studio
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/23/2017

---
# Walkthrough Step 1: Create a Machine Learning workspace
This is the first step of the walkthrough, [Develop a predictive analytics solution in Azure Machine Learning](walkthrough-develop-predictive-solution.md).

1. **Create a Machine Learning workspace**
2. [Upload existing data](walkthrough-2-upload-data.md)
3. [Create a new experiment](walkthrough-3-create-new-experiment.md)
4. [Train and evaluate the models](walkthrough-4-train-and-evaluate-models.md)
5. [Deploy the Web service](walkthrough-5-publish-web-service.md)
6. [Access the Web service](walkthrough-6-access-web-service.md)

- - -
<!-- This needs to be updated to refer to the new way of creating workspaces in the Ibiza portal -->

To use Machine Learning Studio, you need to have a Microsoft Azure Machine Learning workspace. This workspace contains the tools you need to create, manage, and publish experiments.  

The administrator for your Azure subscription needs to create the workspace and then add you as an owner or contributor. For details, see [Create and share an Azure Machine Learning workspace](create-workspace.md).

After your workspace is created, open Machine Learning Studio ([https://studio.azureml.net/Home](https://studio.azureml.net/Home)). If you have more than one workspace, you can select the workspace in the toolbar in the upper-right corner of the window.

![Select workspace in Studio][2]

> [!TIP]
> If you were made an owner of the workspace, you can share the experiments you're working on by inviting others to the workspace. You can do this in Machine Learning Studio on the **SETTINGS** page. You just need the Microsoft account or organizational account for each user.
> 
> On the **SETTINGS** page, click **USERS**, then click **INVITE MORE USERS** at the bottom of the window.
> 
> 

- - -
**Next: [Upload existing data](walkthrough-2-upload-data.md)**

[1]: ./media/walkthrough-1-create-ml-workspace/create1.png
[2]: ./media/walkthrough-1-create-ml-workspace/open-workspace.png
