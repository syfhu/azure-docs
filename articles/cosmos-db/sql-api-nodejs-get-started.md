---
title: Node.js tutorial for the SQL API for Azure Cosmos DB | Microsoft Docs
description: A Node.js tutorial that creates a Cosmos DB with the SQL API. 
keywords: node.js tutorial, node database
services: cosmos-db
author: SnehaGunda
manager: kfile
editor: monicar

ms.service: cosmos-db
ms.component: cosmosdb-sql
ms.devlang: nodejs
ms.topic: tutorial
ms.date: 08/14/2017
ms.author: sngun

---
# Node.js tutorial: Use the SQL API in Azure Cosmos DB to create a Node.js console application

> [!div class="op_single_selector"]
> * [.NET](sql-api-get-started.md)
> * [.NET Core](sql-api-dotnetcore-get-started.md)
> * [Node.js for MongoDB](mongodb-samples.md)
> * [Node.js](sql-api-nodejs-get-started.md)
> * [Java](sql-api-java-get-started.md)


Welcome to the Node.js tutorial for the Azure Cosmos DB Node.js SDK! After following this tutorial, you'll have a console application that creates and queries Azure Cosmos DB resources.

We'll cover:

* Creating and connecting to an Azure Cosmos DB account
* Setting up your application
* Creating a node database
* Creating a collection
* Creating JSON documents
* Querying the collection
* Replacing a document
* Deleting a document
* Deleting the node database

Don't have time? Don't worry! The complete solution is available on [GitHub](https://github.com/Azure-Samples/documentdb-node-getting-started). See [Get the complete solution](#GetSolution) for quick instructions.

After you've completed the Node.js tutorial, please use the voting buttons at the top and bottom of this page to give us feedback. If you'd like us to contact you directly, feel free to include your email address in your comments.

Now let's get started!

## Prerequisites for the Node.js tutorial

Please make sure you have the following:

* An active Azure account. If you don't have one, you can sign up for a [Free Azure Trial](https://azure.microsoft.com/pricing/free-trial/). 

  [!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

* [Node.js](https://nodejs.org/) version v0.10.29 or higher.

## Step 1: Create an Azure Cosmos DB account

Let's create an Azure Cosmos DB account. If you already have an account you want to use, you can skip ahead to [Set up your Node.js application](#SetupNode). If you are using the Azure Cosmos DB Emulator, please follow the steps at [Azure Cosmos DB Emulator](local-emulator.md) to setup the emulator and skip ahead to [Set up your Node.js application](#SetupNode).

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a id="SetupNode"></a>Step 2: Set up your Node.js application

1. Open your favorite terminal.
2. Locate the folder or directory where you'd like to save your Node.js application.
3. Create two empty JavaScript files with the following commands:
   * Windows:
     * ```fsutil file createnew app.js 0```
     * ```fsutil file createnew config.js 0```
   * Linux/OS X:
     * ```touch app.js```
     * ```touch config.js```
4. Install the documentdb module via npm. Use the following command:
   * ```npm install documentdb --save```

Great! Now that you've finished setting up, let's start writing some code.

## <a id="Config"></a>Step 3: Set your app's configurations

Open ```config.js``` in your favorite text editor.

Then, copy and paste the code snippet below and set properties ```config.endpoint``` and ```config.primaryKey``` to your Azure Cosmos DB endpoint uri and primary key. Both these configurations can be found in the [Azure portal](https://portal.azure.com).

![Node.js tutorial - Screen shot of the Azure portal, showing an Azure Cosmos DB account, with the ACTIVE hub highlighted, the KEYS button highlighted on the Azure Cosmos DB account blade, and the URI, PRIMARY KEY and SECONDARY KEY values highlighted on the Keys blade - Node database][keys]

    // ADD THIS PART TO YOUR CODE
    var config = {}

    config.endpoint = "~your Azure Cosmos DB endpoint uri here~";
    config.primaryKey = "~your primary key here~";

Copy and paste the ```database id```, ```collection id```, and ```JSON documents``` to your ```config``` object below where you set your ```config.endpoint``` and ```config.primaryKey``` properties. If you already have data you'd like to store in your database, you can use Azure Cosmos DB's [Data Migration tool](import-data.md) rather than adding the document definitions.

    config.endpoint = "~your Azure Cosmos DB endpoint uri here~";
    config.primaryKey = "~your primary key here~";

    // ADD THIS PART TO YOUR CODE
    config.database = {
        "id": "FamilyDB"
    };

    config.collection = {
        "id": "FamilyColl"
    };

    config.documents = {
        "Andersen": {
            "id": "Anderson.1",
            "lastName": "Andersen",
            "parents": [{
                "firstName": "Thomas"
            }, {
                    "firstName": "Mary Kay"
                }],
            "children": [{
                "firstName": "Henriette Thaulow",
                "gender": "female",
                "grade": 5,
                "pets": [{
                    "givenName": "Fluffy"
                }]
            }],
            "address": {
                "state": "WA",
                "county": "King",
                "city": "Seattle"
            }
        },
        "Wakefield": {
            "id": "Wakefield.7",
            "parents": [{
                "familyName": "Wakefield",
                "firstName": "Robin"
            }, {
                    "familyName": "Miller",
                    "firstName": "Ben"
                }],
            "children": [{
                "familyName": "Merriam",
                "firstName": "Jesse",
                "gender": "female",
                "grade": 8,
                "pets": [{
                    "givenName": "Goofy"
                }, {
                        "givenName": "Shadow"
                    }]
            }, {
                    "familyName": "Miller",
                    "firstName": "Lisa",
                    "gender": "female",
                    "grade": 1
                }],
            "address": {
                "state": "NY",
                "county": "Manhattan",
                "city": "NY"
            },
            "isRegistered": false
        }
    };

The database, collection, and document definitions will act as your Azure Cosmos DB ```database id```, ```collection id```, and documents' data.

Finally, export your ```config``` object, so that you can reference it within the ```app.js``` file.

            },
            "isRegistered": false
        }
    };

    // ADD THIS PART TO YOUR CODE
    module.exports = config;

## <a id="Connect"></a> Step 4: Connect to an Azure Cosmos DB account

Open your empty ```app.js``` file in the text editor. Copy and paste the code below to import the ```documentdb``` module and your newly created ```config``` module.

    // ADD THIS PART TO YOUR CODE
    "use strict";

    var documentClient = require("documentdb").DocumentClient;
    const uriFactory = require('documentdb').UriFactory;
    var config = require("./config");

Copy and paste the code to use the previously saved ```config.endpoint``` and ```config.primaryKey``` to create a new DocumentClient.

    var config = require("./config");

    // ADD THIS PART TO YOUR CODE
    var client = new documentClient(config.endpoint, { "masterKey": config.primaryKey });

Now that you have the code to initialize the Azure Cosmos DB client, let's take a look at working with Azure Cosmos DB resources.

## Step 5: Create a Node database

Copy and paste the code below to set the HTTP status for Not Found, the database id, and the collection id. These ids are how the Azure Cosmos DB client will find the right database and collection.

    var client = new documentClient(config.endpoint, { "masterKey": config.primaryKey });

    // ADD THIS PART TO YOUR CODE
    var HttpStatusCodes = { NOTFOUND: 404 };
    var databaseId = config.database.id;
    var collectionId = config.collection.id;

A [database](sql-api-resources.md#databases) can be created by using the [createDatabase](/javascript/api/documentdb/documentclient) function of the **DocumentClient** class. A database is the logical container of document storage partitioned across collections.

Copy and paste the **getDatabase** function for creating your new database in the app.js file with the ```databaseId``` specified from the ```config``` object. The function will check if the database with the same ```FamilyRegistry``` id does not already exist. If it does exist, we'll return that database instead of creating a new one.

    // ADD THIS PART TO YOUR CODE
    function getDatabase() {
        console.log(`Getting database:\n${databaseId}\n`);
        let databaseUrl = uriFactory.createDatabaseUri(databaseId);
        return new Promise((resolve, reject) => {
            client.readDatabase(databaseUrl, (err, result) => {
                if (err) {
                    if (err.code == HttpStatusCodes.NOTFOUND) {
                        client.createDatabase({ id: databaseId }, (err, created) => {
                            if (err) reject(err)
                            else resolve(created);
                        });
                    } else {
                        reject(err);
                    }
                } else {
                    resolve(result);
                }
            });
        });
    };

Copy and paste the code below where you set the **getDatabase** function to add the helper function **exit** that will print the exit message and the call to **getDatabase** function.

                } else {
                    resolve(result);
                }
            });
        });
    }

    // ADD THIS PART TO YOUR CODE
    function exit(message) {
        console.log(message);
        console.log('Press any key to exit');
        process.stdin.setRawMode(true);
        process.stdin.resume();
        process.stdin.on('data', process.exit.bind(process, 0));
    };

    getDatabase()
    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

In your terminal, locate your ```app.js``` file and run the command: ```node app.js```

Congratulations! You have successfully created an Azure Cosmos DB database.

## <a id="CreateColl"></a>Step 6: Create a collection

> [!WARNING]
> **createCollection** will create a new collection, which has pricing implications. For more details, please visit our [pricing page](https://azure.microsoft.com/pricing/details/cosmos-db/).

A [collection](sql-api-resources.md#collections) can be created by using the [createCollection](/javascript/api/documentdb/documentclient) function of the **DocumentClient** class. A collection is a container of JSON documents and associated JavaScript application logic.

Copy and paste the **getCollection** function underneath the **getDatabase** function in the app.js file to create your new collection with the ```collectionId``` specified from the ```config``` object. Again, we'll check to make sure a collection with the same ```FamilyCollection``` id does not already exist. If it does exist, we'll return that collection instead of creating a new one.

                } else {
                    resolve(result);
                }
            });
        });
    };

    // ADD THIS PART TO YOUR CODE
    function getCollection() {
        console.log(`Getting collection:\n${collectionId}\n`);
        let collectionUrl = uriFactory.createDocumentCollectionUri(databaseId, collectionId);
        return new Promise((resolve, reject) => {
            client.readCollection(collectionUrl, (err, result) => {
                if (err) {
                    if (err.code == HttpStatusCodes.NOTFOUND) {
                        let databaseUrl = uriFactory.createDatabaseUri(databaseId);
                        client.createCollection(databaseUrl, { id: collectionId }, { offerThroughput: 400 }, (err, created) => {
                            if (err) reject(err)
                            else resolve(created);
                        });
                    } else {
                        reject(err);
                    }
                } else {
                    resolve(result);
                }
            });
        });
    };

Copy and paste the code below the call to **getDatabase** to execute the **getCollection** function.

    getDatabase()

    // ADD THIS PART TO YOUR CODE
    .then(() => getCollection())
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

In your terminal, locate your ```app.js``` file and run the command: ```node app.js```

Congratulations! You have successfully created an Azure Cosmos DB collection.

## <a id="CreateDoc"></a>Step 7: Create a document

A [document](sql-api-resources.md#documents) can be created by using the [createDocument](/javascript/api/documentdb/documentclient) function of the **DocumentClient** class. Documents are user defined (arbitrary) JSON content. You can now insert a document into Azure Cosmos DB.

Copy and paste the **getFamilyDocument** function underneath the **getCollection** function for creating the documents containing the JSON data saved in the ```config``` object. Again, we'll check to make sure a document with the same id does not already exist.

                } else {
                    resolve(result);
                }
            });
        });
    };

    // ADD THIS PART TO YOUR CODE
    function getFamilyDocument(document) {
        console.log(`Getting document:\n${document.id}\n`);
        let documentUrl = uriFactory.createDocumentUri(databaseId, collectionId, document.id);
        return new Promise((resolve, reject) => {
            client.readDocument(documentUrl, (err, result) => {
                if (err) {
                    if (err.code == HttpStatusCodes.NOTFOUND) {
                        let collectionUrl = uriFactory.createDocumentCollectionUri(databaseId, collectionId);
                        client.createDocument(collectionUrl, document, (err, created) => {
                            if (err) reject(err)
                            else resolve(created);
                        });
                    } else {
                        reject(err);
                    }
                } else {
                    resolve(result);
                }
            });
        });
    };

Copy and paste the code below the call to **getCollection** to execute the **getFamilyDocument** function.

    getDatabase()
    .then(() => getCollection())

    // ADD THIS PART TO YOUR CODE
    .then(() => getFamilyDocument(config.documents.Andersen))
    .then(() => getFamilyDocument(config.documents.Wakefield))
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

In your terminal, locate your ```app.js``` file and run the command: ```node app.js```

Congratulations! You have successfully created an Azure Cosmos DB document.

![Node.js tutorial - Diagram illustrating the hierarchical relationship between the account, the database, the collection, and the documents - Node database](./media/sql-api-nodejs-get-started/node-js-tutorial-cosmos-db-account.png)

## <a id="Query"></a>Step 8: Query Azure Cosmos DB resources
Azure Cosmos DB supports [rich queries](sql-api-sql-query.md) against JSON documents stored in each collection. The following sample code shows a query that you can run against the documents in your collection.

Copy and paste the **queryCollection** function underneath the **getFamilyDocument** function in the app.js file. Azure Cosmos DB supports SQL-like queries as shown below. For more information on building complex queries, check out the [Query Playground](https://www.documentdb.com/sql/demo) and the [query documentation](sql-api-sql-query.md).

                } else {
                    resolve(result);
                }
            });
        });
    };

    // ADD THIS PART TO YOUR CODE
    function queryCollection() {
        console.log(`Querying collection through index:\n${collectionId}`);
        let collectionUrl = uriFactory.createDocumentCollectionUri(databaseId, collectionId);
        return new Promise((resolve, reject) => {
            client.queryDocuments(
                collectionUrl,
                'SELECT VALUE r.children FROM root r WHERE r.lastName = "Andersen"'
            ).toArray((err, results) => {
                if (err) reject(err)
                else {
                    for (var queryResult of results) {
                        let resultString = JSON.stringify(queryResult);
                        console.log(`\tQuery returned ${resultString}`);
                    }
                    console.log();
                    resolve(results);
                }
            });
        });
    };

The following diagram illustrates how the Azure Cosmos DB SQL query syntax is called against the collection you created.

![Node.js tutorial - Diagram illustrating the scope and meaning of the query - Node database](./media/sql-api-nodejs-get-started/node-js-tutorial-collection-documents.png)

The [FROM](sql-api-sql-query.md#FromClause) keyword is optional in the query because Azure Cosmos DB queries are already scoped to a single collection. Therefore, "FROM Families f" can be swapped with "FROM root r", or any other variable name you choose. Azure Cosmos DB will infer that Families, root, or the variable name you chose, reference the current collection by default.

Copy and paste the code below the call to **getFamilyDocument** to execute the **queryCollection** function.

    .then(() => getFamilyDocument(config.documents.Andersen))
    .then(() => getFamilyDocument(config.documents.Wakefield))

    // ADD THIS PART TO YOUR CODE
    .then(() => queryCollection())
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

In your terminal, locate your ```app.js``` file and run the command: ```node app.js```

Congratulations! You have successfully queried Azure Cosmos DB documents.

## <a id="ReplaceDocument"></a>Step 9: Replace a document
Azure Cosmos DB supports replacing JSON documents.

Copy and paste the **replaceFamilyDocument** function underneath the **queryCollection** function in the app.js file.

                    }
                    console.log();
                    resolve(result);
                }
            });
        });
    };

    // ADD THIS PART TO YOUR CODE
    function replaceFamilyDocument(document) {
        console.log(`Replacing document:\n${document.id}\n`);
        let documentUrl = uriFactory.createDocumentUri(databaseId, collectionId, document.id);
        document.children[0].grade = 6;
        return new Promise((resolve, reject) => {
            client.replaceDocument(documentUrl, document, (err, result) => {
                if (err) reject(err);
                else {
                    resolve(result);
                }
            });
        });
    };

Copy and paste the code below the call to **queryCollection** to execute the **replaceDocument** function. Also, add the code to call **queryCollection** again to verify that the document had successfully changed.

    .then(() => getFamilyDocument(config.documents.Andersen))
    .then(() => getFamilyDocument(config.documents.Wakefield))
    .then(() => queryCollection())

    // ADD THIS PART TO YOUR CODE
    .then(() => replaceFamilyDocument(config.documents.Andersen))
    .then(() => queryCollection())
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

In your terminal, locate your ```app.js``` file and run the command: ```node app.js```

Congratulations! You have successfully replaced an Azure Cosmos DB document.

## <a id="DeleteDocument"></a>Step 10: Delete a document

Azure Cosmos DB supports deleting JSON documents.

Copy and paste the **deleteFamilyDocument** function underneath the **replaceFamilyDocument** function.

                else {
                    resolve(result);
                }
            });
        });
    };

    // ADD THIS PART TO YOUR CODE
    function deleteFamilyDocument(document) {
        console.log(`Deleting document:\n${document.id}\n`);
        let documentUrl = uriFactory.createDocumentUri(databaseId, collectionId, document.id);
        return new Promise((resolve, reject) => {
            client.deleteDocument(documentUrl, (err, result) => {
                if (err) reject(err);
                else {
                    resolve(result);
                }
            });
        });
    };

Copy and paste the code below the call to the second **queryCollection** to execute the **deleteDocument** function.

    .then(() => queryCollection())
    .then(() => replaceFamilyDocument(config.documents.Andersen))
    .then(() => queryCollection())

    // ADD THIS PART TO YOUR CODE
    .then(() => deleteFamilyDocument(config.documents.Andersen))
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

In your terminal, locate your ```app.js``` file and run the command: ```node app.js```

Congratulations! You have successfully deleted an Azure Cosmos DB document.

## <a id="DeleteDatabase"></a>Step 11: Delete the Node database

Deleting the created database will remove the database and all children resources (collections, documents, etc.).

Copy and paste the **cleanup** function underneath the **deleteFamilyDocument** function to remove the database and all the children resources.

                else {
                    resolve(result);
                }
            });
        });
    };

    // ADD THIS PART TO YOUR CODE
    function cleanup() {
        console.log(`Cleaning up by deleting database ${databaseId}`);
        let databaseUrl = uriFactory.createDatabaseUri(databaseId);
        return new Promise((resolve, reject) => {
            client.deleteDatabase(databaseUrl, (err) => {
                if (err) reject(err)
                else resolve(null);
            });
        });
    };

Copy and paste the code below the call to **deleteFamilyDocument** to execute the **cleanup** function.

    .then(() => deleteFamilyDocument(config.documents.Andersen))

    // ADD THIS PART TO YOUR CODE
    .then(() => cleanup())
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

## <a id="Run"></a>Step 12: Run your Node.js application all together!

Altogether, the sequence for calling your functions should look like this:

    getDatabase()
    .then(() => getCollection())
    .then(() => getFamilyDocument(config.documents.Andersen))
    .then(() => getFamilyDocument(config.documents.Wakefield))
    .then(() => queryCollection())
    .then(() => replaceFamilyDocument(config.documents.Andersen))
    .then(() => queryCollection())
    .then(() => deleteFamilyDocument(config.documents.Andersen))
    .then(() => cleanup())
    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

In your terminal, locate your ```app.js``` file and run the command: ```node app.js```

You should see the output of your get started app. The output should match the example text below.

    Getting database:
    FamilyDB

    Getting collection:
    FamilyColl

    Getting document:
    Anderson.1

    Getting document:
    Wakefield.7

    Querying collection through index:
    FamilyColl
        Query returned [{"firstName":"Henriette Thaulow","gender":"female","grade":5,"pets":[{"givenName":"Fluffy"}]}]

    Replacing document:
    Anderson.1

    Querying collection through index:
    FamilyColl
        Query returned [{"firstName":"Henriette Thaulow","gender":"female","grade":6,"pets":[{"givenName":"Fluffy"}]}]

    Deleting document:
    Anderson.1

    Cleaning up by deleting database FamilyDB
    Completed successfully
    Press any key to exit

Congratulations! You've created you've completed the Node.js tutorial and have your first Azure Cosmos DB console application!

## <a id="GetSolution"></a>Get the complete Node.js tutorial solution

If you didn't have time to complete the steps in this tutorial, or just want to download the code, you can get it from [GitHub](https://github.com/Azure-Samples/documentdb-node-getting-started).

To run the GetStarted solution that contains all the samples in this article, you will need the following:

* [Azure Cosmos DB account][create-account].
* The [GetStarted](https://github.com/Azure-Samples/documentdb-node-getting-started) solution available on GitHub.

Install the **documentdb** module via npm. Use the following command:

* ```npm install documentdb --save```

Next, in the ```config.js``` file, update the config.endpoint and config.primaryKey values as described in [Step 3: Set your app's configurations](#Config). 

Then in your terminal, locate your ```app.js``` file and run the command: ```node app.js```.

That's it, build it and you're on your way! 

## Next steps
* Want a more complex Node.js sample? See [Build a Node.js web application using Azure Cosmos DB](sql-api-nodejs-application.md).
* Learn how to [monitor an Azure Cosmos DB account](monitor-accounts.md).
* Run queries against our sample dataset in the [Query Playground](https://www.documentdb.com/sql/demo).

[create-account]: create-sql-api-dotnet.md#create-account
[keys]: media/sql-api-nodejs-get-started/node-js-tutorial-keys.png
