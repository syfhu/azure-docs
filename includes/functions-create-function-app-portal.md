1. Select the **New** button found on the upper left-hand corner of the Azure portal, then select **Compute** > **Function App**. 

    ![Create a function app in the Azure portal](./media/functions-create-function-app-portal/function-app-create-flow.png)

2. Use the function app settings as specified in the table below the image.

    ![Define new function app settings](./media/functions-create-function-app-portal/function-app-create-flow2.png)

    | Setting      | Suggested value  | Description                                        |
    | ------------ |  ------- | -------------------------------------------------- |
    | **App name** | Globally unique name | Name that identifies your new function app. Valid characters are `a-z`, `0-9`, and `-`.  | 
    | **Subscription** | Your subscription | The subscription under which this new function app is created. | 
    | **[Resource Group](../articles/azure-resource-manager/resource-group-overview.md)** |  myResourceGroup | Name for the new resource group in which to create your function app. | 
    | **OS** | Windows | Serverless hosting is currently only available when running on Windows. For Linux hosting, see [Create your first function running on Linux using the Azure CLI](../articles/azure-functions/functions-create-first-azure-function-azure-cli-linux.md). |
    | **[Hosting plan](../articles/azure-functions/functions-scale.md)** |   Consumption plan | Hosting plan that defines how resources are allocated to your function app. In the default **Consumption Plan**, resources are added dynamically as required by your functions. In this [serverless](https://azure.microsoft.com/overview/serverless-computing/) hosting, you only pay for the time your functions run.   |
    | **Location** | West Europe | Choose a [region](https://azure.microsoft.com/regions/) near you or near other services your functions access. |
    | **[Storage account](../articles/storage/common/storage-create-storage-account.md#create-a-storage-account)** |  Globally unique name |  Name of the new storage account used by your function app. Storage account names must be between 3 and 24 characters in length and may contain numbers and lowercase letters only. You can also use an existing account. |

3. Select **Create** to provision and deploy the function app. 

4. Select the Notification icon in the upper-right corner of the portal and watch for the **Deployment succeeded** message. 

    ![Define new function app settings](./media/functions-create-function-app-portal/function-app-create-notification.png)

4. Select **Go to resource** to view your new function app.

>[!TIP]
>Having trouble finding your function apps in the portal, try [adding Function Apps to your favorites in the Azure portal](../articles/azure-functions/functions-how-to-use-azure-function-app-settings.md#favorite).   

