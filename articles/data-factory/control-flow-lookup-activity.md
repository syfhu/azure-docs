---
title: Lookup activity in Azure Data Factory | Microsoft Docs
description: Learn how to use lookup activity to look up a value from an external source. This output can further be referenced by succeeding activities. 
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
editor: 

ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/15/2018
ms.author: shlo

---
# Lookup activity in Azure Data Factory

Lookup activity can be used to retrieve a dataset from any of the ADF-supported data source.  It can be used in the following scenario:
- Dynamically determine which objects (files, tables, etc) to operate on in a subsequent activity, instead of hard-coding the object name

Lookup activity can read and return the content of a configuration file, a configuration table, or the result of executing a query or stored procedure.  The output from Lookup activity can be used in a subsequent copy or transformation activity if it is a singleton value, or used in a ForEach activity if it is an array of attributes.

## Supported capabilities

The following data sources are supported for lookup. The maximum number of rows can be returned by Lookup activity is **5000**, and up to **2MB** in size. And currently the max duration for Lookup activity before timeout is one hour.

[!INCLUDE [data-factory-v2-supported-data-stores](../../includes/data-factory-v2-supported-data-stores-for-lookup-activity.md)]

## Syntax

```json
{
    "name": "LookupActivity",
    "type": "Lookup",
    "typeProperties": {
        "source": {
            "type": "<source type>"
            <additional source specific properties (optional)>
        },
        "dataset": { 
            "referenceName": "<source dataset name>",
            "type": "DatasetReference"
        },
        "firstRowOnly": false
    }
}
```

## Type properties
Name | Description | Type | Required?
---- | ----------- | ---- | --------
dataset | Provides the dataset reference for the lookup. Get details from the "Dataset properties" section in each corresponding connector article. | Key/value pair | Yes
source | Contains dataset-specific source properties, the same as the copy activity source. Get details from the "Copy activity properties" section in each corresponding connector article. | Key/value pair | Yes
firstRowOnly | Indicates whether to return only the first row or all rows. | Boolean | No. Default is `true`.

**Note the following points:**

1. Source column with ByteArray type is not supported.
2. Structure is not supported in dataset definition. For text format files specifically, you can use the header row to provide the column name.
3. If your lookup source is a JSON file(s), the `jsonPathDefinition` setting for re-shaping the JSON object is not supported, the entire objects will be retrieved.

## Use the lookup activity result in a subsequent activity

The lookup result is returned in the `output` section of the activity run result.

* **When `firstRowOnly` is set to `true` (default)**, the output format is as shown in the following code. The lookup result is under a fixed `firstRow` key. To use the result in subsequent activity, use the pattern of `@{activity('MyLookupActivity').output.firstRow.TableName}`.

    ```json
    {
        "firstRow":
        {
            "Id": "1",
            "TableName" : "Table1"
        }
    }
    ```

* **When `firstRowOnly` is set to `false`**, the output format is as shown in the following code. A `count` field indicates how many records are returned, and detailed values are displayed under a fixed `value` array. In such a case, the lookup activity is usually followed by a [Foreach activity](control-flow-for-each-activity.md). You can pass the `value` array to the ForEach activity `items` field by using the pattern of `@activity('MyLookupActivity').output.value`. To access elements in the `value` array, use the following syntax: `@{activity('lookupActivity').output.value[zero based index].propertyname}`. Here is an example: `@{activity('lookupActivity').output.value[0].tablename}`.

    ```json
    {
        "count": "2",
        "value": [
            {
                "Id": "1",
                "TableName" : "Table1"
            },
            {
                "Id": "2",
                "TableName" : "Table2"
            }
        ]
    } 
    ```

## Example
In this example, the copy activity copies data from a SQL table in your Azure SQL Database instance to Azure Blob storage. The name of the SQL table is stored in a JSON file in Blob storage. The lookup activity looks up the table name at runtime. This approach allows JSON to be modified dynamically without your having to redeploy pipelines or datasets. 

This example demonstrates lookup for the first row only. For lookup for all rows and to chain the results with ForEach activity, see the samples in [Copy multiple tables in bulk by using Azure Data Factory](tutorial-bulk-copy.md).

### Pipeline
This pipeline contains two activities: *lookup* and *copy*. 

- The lookup activity is configured to use LookupDataset, which refers to a location in Azure Blob storage. The lookup activity reads the name of the SQL table from a JSON file in this location. 
- The copy activity uses the output of the lookup activity (name of the SQL table). The tableName property in the source dataset (SourceDataset) is configured to use the output from the lookup activity. The copy activity copies data from the SQL table to a location in Azure Blob storage that is specified by the SinkDataset property. 


```json
{
    "name": "LookupPipelineDemo",
    "properties": {
        "activities": [
            {
                "name": "LookupActivity",
                "type": "Lookup",
                "typeProperties": {
                    "source": {
                        "type": "BlobSource"
                    },
                    "dataset": { 
                        "referenceName": "LookupDataset", 
                        "type": "DatasetReference" 
                    }
                }
            },
            {
                "name": "CopyActivity",
                "type": "Copy",
                "typeProperties": {
                    "source": { 
                        "type": "SqlSource", 
                        "sqlReaderQuery": "select * from @{activity('LookupActivity').output.firstRow.tableName}" 
                    },
                    "sink": { 
                        "type": "BlobSink" 
                    }
                },                
                "dependsOn": [ 
                    { 
                        "activity": "LookupActivity", 
                        "dependencyConditions": [ "Succeeded" ] 
                    }
                 ],
                "inputs": [ 
                    { 
                        "referenceName": "SourceDataset", 
                        "type": "DatasetReference" 
                    } 
                ],
                "outputs": [ 
                    { 
                        "referenceName": "SinkDataset", 
                        "type": "DatasetReference" 
                    } 
                ]
            }
        ]
	}
}
```

### Lookup dataset
The lookup dataset refers to the *sourcetable.json* file in the Azure Storage lookup folder that's specified by the AzureStorageLinkedService type. 

```json
{
	"name": "LookupDataset",
	"properties": {
		"type": "AzureBlob",
		"typeProperties": {
			"folderPath": "lookup",
			"fileName": "sourcetable.json",
			"format": {
				"type": "JsonFormat",
				"filePattern": "SetOfObjects"
			}
		},
		"linkedServiceName": {
			"referenceName": "AzureStorageLinkedService",
			"type": "LinkedServiceReference"
		}
	}
}
```

### Source dataset for the copy activity
The source dataset uses the output of the lookup activity, which is the name of the SQL table. The copy activity copies data from this SQL table to a location in Azure Blob storage that's specified by the sink dataset. 

```json
{
    "name": "SourceDataset",
	"properties": {
		"type": "AzureSqlTable",
		"typeProperties":{
			"tableName": "@{activity('LookupActivity').output.firstRow.tableName}"
		},
		"linkedServiceName": {
			"referenceName": "AzureSqlLinkedService",
			"type": "LinkedServiceReference"
		}
	}
}
```

### Sink dataset for the copy activity
The copy activity copies data from the SQL table to the *filebylookup.csv* file in the *csv* folder in the Azure storage that's specified by the AzureStorageLinkedService property. 

```json
{
	"name": "SinkDataset",
	"properties": {
		"type": "AzureBlob",
		"typeProperties": {
			"folderPath": "csv",
			"fileName": "filebylookup.csv",
			"format": {
				"type": "TextFormat"                                                                    
			}
		},
		"linkedServiceName": {
			"referenceName": "AzureStorageLinkedService",
			"type": "LinkedServiceReference"
		}
	}
}
```

### Azure Storage linked service
This storage account contains the JSON file with the names of the SQL tables. 

```json
{
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": {
                "value": "DefaultEndpointsProtocol=https;AccountName=<StorageAccountName>;AccountKey=<StorageAccountKey>",
                "type": "SecureString"
            }
        }
    },
        "name": "AzureStorageLinkedService"
}
```

### Azure SQL Database linked service
This Azure SQL Database instance contains the data to be copied to Blob storage. 

```json
{
    "name": "AzureSqlLinkedService",
    "properties": {
        "type": "AzureSqlDatabase",
        "description": "",
		"typeProperties": {
			"connectionString": {
				"value": "Server=<server>;Initial Catalog=<database>;User ID=<user>;Password=<password>;",
				"type": "SecureString"
			}
		}
	}
}
```

### sourcetable.json

#### Set of objects

```json
{
  "Id": "1",
  "tableName": "Table1"
}
{
   "Id": "2",
  "tableName": "Table2"
}
```

#### Array of objects

```json
[ 
    {
        "Id": "1",
        "tableName": "Table1"
    },
    {
        "Id": "2",
        "tableName": "Table2"
    }
]
```

## Next steps
See other control flow activities that are supported by Data Factory: 

- [Execute Pipeline activity](control-flow-execute-pipeline-activity.md)
- [For Each activity](control-flow-for-each-activity.md)
- [Get Metadata activity](control-flow-get-metadata-activity.md)
- [Web activity](control-flow-web-activity.md)
