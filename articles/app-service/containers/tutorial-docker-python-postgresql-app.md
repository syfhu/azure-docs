---
title: Build a Docker Python and PostgreSQL web app in Azure | Microsoft Docs 
description: Learn how to get a Docker Python app working in Azure, with connection to a PostgreSQL database.
services: app-service\web
documentationcenter: python
author: berndverst
manager: cfowler
ms.service: app-service-web
ms.workload: web
ms.devlang: python
ms.topic: tutorial
ms.date: 01/28/2018
ms.author: beverst;cephalin
ms.custom: mvc
---
# Build a Docker Python and PostgreSQL web app in Azure

Web App for Containers provides a highly scalable, self-patching web hosting service. This tutorial shows how to create a basic Docker Python web app in Azure. You connect this app to a PostgreSQL database. When you are done, you have a Python Flask application running within a Docker container on [App Service on Linux](app-service-linux-intro.md).

![Docker Python Flask app in App Service on Linux](./media/tutorial-docker-python-postgresql-app/docker-flask-in-azure.png)

In this tutorial, you learn how to:

> [!div class="checklist"]
> * Create a PostgreSQL database in Azure
> * Connect a Python app to PostgreSQL
> * Deploy the app to Azure
> * Update the data model and redeploy the app
> * Manage the app in the Azure portal

You can follow the steps in this article on macOS. Linux and Windows instructions are the same in most cases, but the differences are not detailed in this tutorial.
 
[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## Prerequisites

To complete this tutorial:

1. [Install Git](https://git-scm.com/)
1. [Install Python](https://www.python.org/downloads/)
1. [Install and run PostgreSQL](https://www.postgresql.org/download/)
1. [Install Docker Community Edition](https://www.docker.com/community-edition)

## Test local PostgreSQL installation and create a database

Open the terminal window and run `psql` to connect to your local PostgreSQL server.

```bash
sudo -u postgres psql
```

If your connection is successful, your PostgreSQL database is running. If not, make sure that your local PostgresQL database is started by following the steps at [Downloads - PostgreSQL Core Distribution](https://www.postgresql.org/download/).

Create a database called *eventregistration* and set up a separate database user named *manager* with password *supersecretpass*.

```bash
CREATE DATABASE eventregistration;
CREATE USER manager WITH PASSWORD 'supersecretpass';
GRANT ALL PRIVILEGES ON DATABASE eventregistration TO manager;
```
Type `\q` to exit the PostgreSQL client. 

<a name="step2"></a>

## Create local Python Flask application

In this step, you set up the local Python Flask project.

### Clone the sample application

Open the terminal window, and `CD` to a working directory.

Run the following commands to clone the sample repository and go to the *0.1-initialapp* release.

```bash
git clone https://github.com/Azure-Samples/docker-flask-postgres.git
cd docker-flask-postgres
git checkout tags/0.1-initialapp
```

This sample repository contains a [Flask](http://flask.pocoo.org/) application. 

### Run the application

> [!NOTE] 
> In a later step, you simplify this process by building a Docker container to use with the production database.

Install the required packages and start the application.

```bash
pip install virtualenv
virtualenv venv
source venv/bin/activate
pip install -r requirements.txt
cd app
FLASK_APP=app.py DBHOST="localhost" DBUSER="manager" DBNAME="eventregistration" DBPASS="supersecretpass" flask db upgrade
FLASK_APP=app.py DBHOST="localhost" DBUSER="manager" DBNAME="eventregistration" DBPASS="supersecretpass" flask run
```

When the app is fully loaded, you see something similar to the following message:

```bash
INFO  [alembic.runtime.migration] Context impl PostgresqlImpl.
INFO  [alembic.runtime.migration] Will assume transactional DDL.
INFO  [alembic.runtime.migration] Running upgrade  -> 791cd7d80402, empty message
 * Serving Flask app "app"
 * Running on http://127.0.0.1:5000/ (Press CTRL+C to quit)
```

Navigate to `http://localhost:5000` in a browser. Click **Register!** and create a test user.

![Python Flask application running locally](./media/tutorial-docker-python-postgresql-app/local-app.png)

The Flask sample application stores user data in the database. If you are successful at registering a user, your app is writing data to the local PostgreSQL database.

To stop the Flask server at anytime, type Ctrl+C in the terminal. 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## Create a production PostgreSQL database

In this step, you create a PostgreSQL database in Azure. When your app is deployed to Azure, it uses this cloud database.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

### Create a resource group

[!INCLUDE [Create resource group](../../../includes/app-service-web-create-resource-group-linux-no-h.md)] 

### Create an Azure Database for PostgreSQL server

Create a PostgreSQL server with the [`az postgres server create`](/cli/azure/postgres/server?view=azure-cli-latest#az_postgres_server_create) command.

In the following command, substitute a unique server name for the *\<postgresql_name>* placeholder and a user name for the *\<admin_username>* placeholder. The server name is used as part of your PostgreSQL endpoint (`https://<postgresql_name>.postgres.database.azure.com`), so the name needs to be unique across all servers in Azure. The user name is for the initial database admin user account. You are prompted to pick a password for this user.

```azurecli-interactive
az postgres server create --resource-group myResourceGroup --name <postgresql_name> --admin-user <admin_username>  --storage-size 51200
```

When the Azure Database for PostgreSQL server is created, the Azure CLI shows information similar to the following example:

```json
{
  "administratorLogin": "<my_admin_username>",
  "fullyQualifiedDomainName": "<postgresql_name>.postgres.database.azure.com",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.DBforPostgreSQL/servers/<postgresql_name>",
  "location": "westus",
  "name": "<postgresql_name>",
  "resourceGroup": "myResourceGroup",
  "sku": {
    "capacity": 100,
    "family": null,
    "name": "PGSQLS3M100",
    "size": null,
    "tier": "Basic"
  },
  "sslEnforcement": null,
  "storageMb": 2048,
  "tags": null,
  "type": "Microsoft.DBforPostgreSQL/servers",
  "userVisibleState": "Ready",
  "version": null
}
```

### Create a firewall rule for the Azure Database for PostgreSQL server

Run the following Azure CLI command to allow access to the database from all IP addresses. When both starting IP and end IP are set to 0.0.0.0, the firewall is only opened for other Azure resources. 

```azurecli-interactive
az postgres server firewall-rule create --resource-group myResourceGroup --server-name <postgresql_name> --start-ip-address=0.0.0.0 --end-ip-address=0.0.0.0 --name AllowAzureIPs
```

The Azure CLI confirms the firewall rule creation with output similar to the following example:

```json
{
  "endIpAddress": "0.0.0.0",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.DBforPostgreSQL/servers/<postgresql_name>/firewallRules/AllowAzureIPs",
  "name": "AllowAzureIPs",
  "resourceGroup": "myResourceGroup",
  "startIpAddress": "0.0.0.0",
  "type": "Microsoft.DBforPostgreSQL/servers/firewallRules"
}
```

> [!TIP] 
> You can be even more restrictive in your firewall rule by [using only the outbound IP addresses your app uses](../app-service-ip-addresses.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json#find-outbound-ips).
>

## Connect your Python Flask application to the database

In this step, you connect your Python Flask sample application to the Azure Database for PostgreSQL server you created.

### Create an empty database and set up a new database application user

Create a database user with access to a single database only. You use these credentials to avoid giving the application full access to the server.

Connect to the database (you're prompted for your admin password).

```bash
psql -h <postgresql_name>.postgres.database.azure.com -U <my_admin_username>@<postgresql_name> postgres
```

Create the database and user from the PostgreSQL CLI.

```bash
CREATE DATABASE eventregistration;
CREATE USER manager WITH PASSWORD 'supersecretpass';
GRANT ALL PRIVILEGES ON DATABASE eventregistration TO manager;
```

Type `\q` to exit the PostgreSQL client.

### Test the application locally against the Azure PostgreSQL database

Going back now to the *app* folder of the cloned Github repository, you can run the Python Flask application by updating the database environment variables.

```bash
FLASK_APP=app.py DBHOST="<postgresql_name>.postgres.database.azure.com" DBUSER="manager@<postgresql_name>" DBNAME="eventregistration" DBPASS="supersecretpass" flask db upgrade
FLASK_APP=app.py DBHOST="<postgresql_name>.postgres.database.azure.com" DBUSER="manager@<postgresql_name>" DBNAME="eventregistration" DBPASS="supersecretpass" flask run
```

When the app is fully loaded, you see something similar to the following message:

```bash
INFO  [alembic.runtime.migration] Context impl PostgresqlImpl.
INFO  [alembic.runtime.migration] Will assume transactional DDL.
INFO  [alembic.runtime.migration] Running upgrade  -> 791cd7d80402, empty message
 * Serving Flask app "app"
 * Running on http://127.0.0.1:5000/ (Press CTRL+C to quit)
```

Navigate to http://localhost:5000 in a browser. Click **Register!** and create a test registration. You are now writing data to the database in Azure.

![Python Flask application running locally](./media/tutorial-docker-python-postgresql-app/local-app.png)

### Running the application from a Docker Container

Build the Docker container image.

```bash
cd ..
docker build -t flask-postgresql-sample .
```

Docker displays a confirmation that it successfully created the container.

```bash
Successfully built 7548f983a36b
```

In the repository root, add an environment variable file called _db.env_, and then add the following database environment variables to it. The app connects to the Azure Database for PostgreSQL production database.

```text
DBHOST=<postgresql_name>.postgres.database.azure.com
DBUSER=manager@<postgresql_name>
DBNAME=eventregistration
DBPASS=supersecretpass
```

Run the app from within the Docker container. The following command specifies the environment variable file and maps the default Flask port 5000 to local port 5000.

```bash
docker run -it --env-file db.env -p 5000:5000 flask-postgresql-sample
```

The output is similar to what you saw earlier. However, the initial database migration no longer needs to be performed and therefore is skipped.

```bash
INFO  [alembic.runtime.migration] Context impl PostgresqlImpl.
INFO  [alembic.runtime.migration] Will assume transactional DDL.
 * Serving Flask app "app"
 * Running on http://0.0.0.0:5000/ (Press CTRL+C to quit)
```

The database already contains the registration you created previously.

![Docker container-based Python Flask application running locally](./media/tutorial-docker-python-postgresql-app/local-docker.png)

## Upload the Docker container to a container registry

In this step, you upload the Docker container to a container registry. Use Azure Container Registry, but you could also use other popular ones such as Docker Hub.

### Create an Azure Container Registry

In the following command to create a container registry replace *\<registry_name>* with a unique Azure container registry name of your choice.

```azurecli-interactive
az acr create --name <registry_name> --resource-group myResourceGroup --location "West US" --sku Basic
```

Output

```json
{
  "adminUserEnabled": false,
  "creationDate": "2017-05-04T08:50:55.635688+00:00",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.ContainerRegistry/registries/<registry_name>",
  "location": "westus",
  "loginServer": "<registry_name>.azurecr.io",
  "name": "<registry_name>",
  "provisioningState": "Succeeded",
  "sku": {
    "name": "Basic",
    "tier": "Basic"
  },
  "storageAccount": {
    "name": "<registry_name>01234"
  },
  "tags": {},
  "type": "Microsoft.ContainerRegistry/registries"
}
```

### Retrieve the registry credentials for pushing and pulling Docker images

To show registry credentials, enable admin mode first.

```azurecli-interactive
az acr update --name <registry_name> --admin-enabled true
az acr credential show -n <registry_name>
```

You see two passwords. Make note of the user name and the first password.

```json
{
  "passwords": [
    {
      "name": "password",
      "value": "<registry_password>"
    },
    {
      "name": "password2",
      "value": "<registry_password2>"
    }
  ],
  "username": "<registry_name>"
}
```

### Upload your Docker container to Azure Container Registry

Log in to your registry. When prompted, supply the password you retrieved.

```bash
docker login <registry_name>.azurecr.io -u <registry_name>
```

Push your docker image to the registry.

```bash
docker tag flask-postgresql-sample <registry_name>.azurecr.io/flask-postgresql-sample
docker push <registry_name>.azurecr.io/flask-postgresql-sample
```

## Deploy the Docker Python Flask application to Azure

In this step, you deploy your Docker container-based Python Flask application to Azure App Service.

### Create an App Service plan

[!INCLUDE [Create app service plan](../../../includes/app-service-web-create-app-service-plan-linux-no-h.md)]

### Create a web app

Create a web app in the *myAppServicePlan* App Service plan with the [`az webapp create`](/cli/azure/webapp?view=azure-cli-latest#az_webapp_create) command.

The web app gives you a hosting space to deploy your code and provides a URL for you to view the deployed application. Use  to create the web app.

In the following command, replace the *\<app_name>* placeholder with a unique app name. This name is part of the default URL for the web app, so the name needs to be unique across all apps in Azure App Service.

```azurecli
az webapp create --name <app_name> --resource-group myResourceGroup --plan myAppServicePlan --deployment-container-image-name "<registry_name>.azurecr.io/flask-postgresql-sample"
```

When the web app has been created, the Azure CLI shows information similar to the following example:

```json
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "cloningInfo": null,
  "containerSize": 0,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "<app_name>.azurewebsites.net",
  "enabled": true,
  ...
  < Output has been truncated for readability >
}
```

### Configure the database environment variables

Earlier in the tutorial, you defined environment variables to connect to your PostgreSQL database.

In App Service, you set environment variables as _app settings_ by using the [`az webapp config appsettings set`](/cli/azure/webapp/config/appsettings?view=azure-cli-latest#az_webapp_config_appsettings_set) command.

The following example specifies the database connection details as app settings. It also uses the *PORT* variable to map PORT 5000 from your Docker Container to receive HTTP traffic on PORT 80.

```azurecli-interactive
az webapp config appsettings set --name <app_name> --resource-group myResourceGroup --settings DBHOST="<postgresql_name>.postgres.database.azure.com" DBUSER="manager@<postgresql_name>" DBPASS="supersecretpass" DBNAME="eventregistration" PORT=5000
```

### Configure Docker container deployment

AppService can automatically download and run a Docker container.

```azurecli
az webapp config container set --resource-group myResourceGroup --name <app_name> --docker-registry-server-user "<registry_name>" --docker-registry-server-password "<registry_password>" --docker-registry-server-url "https://<registry_name>.azurecr.io"
```

Whenever you update the Docker container or change the settings, restart the app. Restarting ensures that all settings are applied and the latest container is pulled from the registry.

```azurecli-interactive
az webapp restart --resource-group myResourceGroup --name <app_name>
```

### Browse to the Azure web app 

Browse to the deployed web app using your web browser. 

```bash 
http://<app_name>.azurewebsites.net 
```
> [!NOTE]
> The web app takes longer to load because the container has to be downloaded and started after the container configuration is changed.

You see previously registered guests that were saved to the Azure production database in the previous step.

![Docker container-based Python Flask application running locally](./media/tutorial-docker-python-postgresql-app/docker-app-deployed.png)

**Congratulations!** You're running a Docker container-based Python Flask app in Azure App Service.

## Update data model and redeploy

In this step, you add the number of attendees to each event registration by updating the Guest model.

Check out the *0.2-migration* release with the following git command:

```bash
git checkout tags/0.2-migration
```

This release already made the necessary changes to views, controllers, and model. It also includes a database migration generated via *alembic* (`flask db migrate`). You can see all changes made via the following git command:

```bash
git diff 0.1-initialapp 0.2-migration
```

### Test your changes locally

Run the following commands to test your changes locally by running the flask server.

```bash
source venv/bin/activate
cd app
FLASK_APP=app.py DBHOST="localhost" DBUSER="manager" DBNAME="eventregistration" DBPASS="supersecretpass" flask db upgrade
FLASK_APP=app.py DBHOST="localhost" DBUSER="manager" DBNAME="eventregistration" DBPASS="supersecretpass" flask run
```

Navigate to http://localhost:5000 in your browser to view the changes. Create a test registration.

![Docker container-based Python Flask application running locally](./media/tutorial-docker-python-postgresql-app/local-app-v2.png)

### Publish changes to Azure

Build the new docker image, push it to the container registry, and restart the app.

```bash
cd ..
docker build -t flask-postgresql-sample .
docker tag flask-postgresql-sample <registry_name>.azurecr.io/flask-postgresql-sample
docker push <registry_name>.azurecr.io/flask-postgresql-sample
az appservice web restart --resource-group myResourceGroup --name <app_name>
```

Navigate to your Azure web app and try out the new functionality again. Create another event registration.

```bash 
http://<app_name>.azurewebsites.net 
```

![Docker Python Flask app in Azure App Service](./media/tutorial-docker-python-postgresql-app/docker-flask-in-azure.png)

## Manage your Azure web app

Go to the [Azure portal](https://portal.azure.com) to see the web app you created.

From the left menu, click **App Services**, then click the name of your Azure web app.

![Portal navigation to Azure web app](./media/tutorial-docker-python-postgresql-app/app-resource.png)

By default, the portal shows your web app's **Overview** page. This page gives you a view of how your app is doing. Here, you can also perform basic management tasks like browse, stop, start, restart, and delete. The tabs on the left side of the page show the different configuration pages you can open.

![App Service page in Azure portal](./media/tutorial-docker-python-postgresql-app/app-mgmt.png)

## Next steps

Advance to the next tutorial to learn how to map a custom DNS name to your web app.

> [!div class="nextstepaction"]
> [Map an existing custom DNS name to Azure Web Apps](../app-service-web-tutorial-custom-domain.md)
