---
title: Supported resources for newer Azure Monitor metric alerts
description: Reference on support metrics and logs for newer Azure near real-time metric alerts.
author: snehithm
services: monitoring
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 04/27/2018
ms.author: snmuvva
ms.component: alerts
---

# Supported metrics and creation methods for new metric alerts
Azure Monitor now supports a [new metric alert type](monitoring-overview-unified-alerts.md) which has significant benefits over the older [classic metric alerts](insights-alerts-portal.md). The older alerts support a [large list of metrics](monitoring-supported-metrics.md). The newer alerts support a (growing) subset of that larger list. This article lists that subset. 

## Portal, PowerShell, CLI, REST support
Currently, you can create newer metric alerts only in the Azure portal, [REST API](https://docs.microsoft.com/en-us/rest/api/monitor/metricalerts/createorupdate) or [Resource Manager Templates](monitoring-create-metric-alerts-with-templates.md). Support for configuring newer alerts  using PowerShell and the Azure command-line interface (Azure CLI 2.0) is coming soon.

## Metrics and Dimensions Supported
Newer metric alerts support alerting for metrics that use dimensions. You can use dimensions to filter your metric to the right level. All supported metrics along with applicable dimensions can be explored and visualized from [Azure Monitor - Metrics Explorer (Preview)](monitoring-metric-charts.md).

Here's the full list of Azure monitor metric sources supported by the newer alerts:

|Resource type  |Dimensions Supported  | Metrics Available|
|---------|---------|----------------|
|Microsoft.ApiManagement/service     | Yes        | [API Management](monitoring-supported-metrics.md#microsoftapimanagementservice)|
|Microsoft.Automation/automationAccounts     |     Yes   | [Automation Accounts](monitoring-supported-metrics.md#microsoftautomationautomationaccounts)|
|Microsoft.Batch/batchAccounts | N/A| [Batch Accounts](monitoring-supported-metrics.md#microsoftbatchbatchaccounts)|
|Microsoft.Cache/Redis     |    N/A     |[Redis Cache](monitoring-supported-metrics.md#microsoftcacheredis)|
|Microsoft.Compute/virtualMachines     |    N/A     | [Virtual Machines](monitoring-supported-metrics.md#microsoftcomputevirtualmachines)|
|Microsoft.Compute/virtualMachineScaleSets     |   N/A      |[Virtual Machine scale sets](monitoring-supported-metrics.md#microsoftcomputevirtualmachinescalesets)|
|Microsoft.ContainerInstance/containerGroups | Yes| [Container groups](monitoring-supported-metrics.md#microsoftcontainerinstancecontainergroups)|
|Microsoft.DataFactory/datafactories| Yes| [Data Factories V1](monitoring-supported-metrics.md#microsoftdatafactorydatafactories)|
|Microsoft.DataFactory/factories     |   Yes     |[Data Factories V2](monitoring-supported-metrics.md#microsoftdatafactoryfactories)|
|Microsoft.DBforMySQL/servers     |   N/A      |[DB for MySQL](monitoring-supported-metrics.md#microsoftdbformysqlservers)|
|Microsoft.DBforPostgreSQL/servers     |    N/A     | [DB for PostgreSQL](monitoring-supported-metrics.md#microsoftdbforpostgresqlservers)|
|Microsoft.EventHub/namespaces     |  Yes      |[Event Hubs](monitoring-supported-metrics.md#microsofteventhubnamespaces)|
|Microsoft.KeyVault/vaults| No | [Vaults](monitoring-supported-metrics.md#microsoftkeyvaultvaults)|
|Microsoft.Logic/workflows     |     N/A    |[Logic Apps](monitoring-supported-metrics.md#microsoftlogicworkflows) |
|Microsoft.Network/applicationGateways     |    N/A     | [Application Gateways](monitoring-supported-metrics.md#microsoftnetworkapplicationgateways) |
|Microsoft.Network/dnsZones | N/A| [DNS Zones](monitoring-supported-metrics.md#microsoftnetworkdnszones) |
|Microsoft.Network/loadBalancers (only for Standard SKUs)| Yes| [Load Balancers](monitoring-supported-metrics.md#microsoftnetworkloadbalancers) |
|Microsoft.Network/publicipaddresses     |  N/A       |[Public IP Addreses](monitoring-supported-metrics.md#microsoftnetworkpublicipaddresses)|
|Microsoft.PowerBIDedicated/capacities | N/A | [Capacities](monitoring-supported-metrics.md#microsoftpowerbidedicatedcapacities)|
|Microsoft.Search/searchServices     |   N/A      |[Search services](monitoring-supported-metrics.md#microsoftsearchsearchservices)|
|Microsoft.ServiceBus/namespaces     |  Yes       |[Service Bus](monitoring-supported-metrics.md#microsoftservicebusnamespaces)|
|Microsoft.Storage/storageAccounts     |    Yes     | [Storage Accounts](monitoring-supported-metrics.md#microsoftstoragestorageaccounts)|
|Microsoft.Storage/storageAccounts/services     |     Yes    | [Blob Services](monitoring-supported-metrics.md#microsoftstoragestorageaccountsblobservices), [File Services](monitoring-supported-metrics.md#microsoftstoragestorageaccountsfileservices), [Queue Services](monitoring-supported-metrics.md#microsoftstoragestorageaccountsqueueservices) and [Table Services](monitoring-supported-metrics.md#microsoftstoragestorageaccountstableservices)|
|Microsoft.StreamAnalytics/streamingjobs     |  N/A       | [Stream Analytics](monitoring-supported-metrics.md#microsoftstreamanalyticsstreamingjobs)|
|Microsoft.CognitiveServices/accounts     |    N/A     | [Cognitive Services](monitoring-supported-metrics.md#microsoftcognitiveservicesaccounts)|
|Microsoft.OperationalInsights/workspaces (Preview) | Yes|[Log Analytics workspaces](#log-analytics-logs-as-metrics-for-alerting)|


## Log Analytics logs as metrics for alerting 

You can also use newer metric alerts on popular Log Analytics logs extracted as metrics as part of Metrics from Logs Preview.  
- [Performance counters](../log-analytics/log-analytics-data-sources-performance-counters.md) for Windows & Linux machines
- [Heartbeat records for Agent Health](../operations-management-suite/oms-solution-agenthealth.md)
- [Update management](../operations-management-suite/oms-solution-update-management.md) records
 
> [!NOTE]
> Specific metric and/or dimension will only be shown if data for it exists in chosen period. These metrics are available for customers with workspaces in East US, West Central US and West Europe who have opted in to the preview. If you would like to be a part of this preview, sign up using [the survey](https://aka.ms/MetricLogPreview).

The following list of Log Analytics log-based metric sources is supported:

Metric Name/Details  |Dimensions Supported  | Type of Log  |
|---------|---------|---------|
|Average_Avg. Disk sec/Read     |     Yes - Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Windows Performance Counter      |
| Average_Avg. Disk sec/Write     |     Yes - Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Windows Performance Counter      |
| Average_Current Disk Queue Length   |     Yes - Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Windows Performance Counter      |
| Average_Disk Reads/sec    |     Yes - Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Windows Performance Counter      |
| Average_Disk Transfers/sec    |     Yes - Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Windows Performance Counter      |
|   Average_% Free Space    |     Yes - Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Windows Performance Counter      |
| Average_Available MBytes     |     Yes - Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Windows Performance Counter      |
| Average_% Committed Bytes In Use    |     Yes - Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Windows Performance Counter      |
| Average_Bytes Received/sec    |     Yes - Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Windows Performance Counter      |
|  Average_Bytes Sent/sec    |     Yes - Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Windows Performance Counter      |
|  Average_Bytes Total/sec    |     Yes - Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Windows Performance Counter      |
|  Average_% Processor Time    |     Yes - Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Windows Performance Counter      |
|   Average_Processor Queue Length    |     Yes - Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Windows Performance Counter      |
|	Average_% Free Inodes	|     Yes - Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Linux Performance Counter      |
|	 Average_% Free Space	|     Yes - Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Linux Performance Counter      |
|	 Average_% Used Inodes	|     Yes - Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Linux Performance Counter      |
|	 Average_% Used Space	|     Yes - Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Linux Performance Counter      |
|	 Average_Disk Read Bytes/sec	|     Yes - Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Linux Performance Counter      |
|	 Average_Disk Reads/sec	|     Yes - Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Linux Performance Counter      |
|	 Average_Disk Transfers/sec	|     Yes - Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Linux Performance Counter      |
|	 Average_Disk Write Bytes/sec	|     Yes - Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Linux Performance Counter      |
|	 Average_Disk Writes/sec	|     Yes - Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Linux Performance Counter      |
|	 Average_Free Megabytes	|     Yes - Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Linux Performance Counter      |
|	 Average_Logical Disk Bytes/sec	|     Yes - Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Linux Performance Counter      |
|	 Average_% Available Memory	|     Yes - Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Linux Performance Counter      |
|	 Average_% Available Swap Space	|     Yes - Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Linux Performance Counter      |
|	 Average_% Used Memory	|     Yes - Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Linux Performance Counter      |
|	 Average_% Used Swap Space	|     Yes - Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Linux Performance Counter      |
|	 Average_Available MBytes Memory	|     Yes - Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Linux Performance Counter      |
|	 Average_Available MBytes Swap	|     Yes - Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Linux Performance Counter      |
|	 Average_Page Reads/sec	|     Yes - Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Linux Performance Counter      |
|	 Average_Page Writes/sec	|     Yes - Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Linux Performance Counter      |
|	 Average_Pages/sec	|     Yes - Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Linux Performance Counter      |
|	 Average_Used MBytes Swap Space	|     Yes - Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Linux Performance Counter      |
|	 Average_Used Memory MBytes	|     Yes - Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Linux Performance Counter      |
|	 Average_Total Bytes Transmitted	|     Yes - Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Linux Performance Counter      |
|	 Average_Total Bytes Received	|     Yes - Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Linux Performance Counter      |
|	 Average_Total Bytes	|     Yes - Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Linux Performance Counter      |
|	 Average_Total Packets Transmitted	|     Yes - Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Linux Performance Counter      |
|	 Average_Total Packets Received	|     Yes - Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Linux Performance Counter      |
|	 Average_Total Rx Errors	|     Yes - Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Linux Performance Counter      |
|	 Average_Total Tx Errors	|     Yes - Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Linux Performance Counter      |
|	 Average_Total Collisions	|     Yes - Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Linux Performance Counter      |
|	 Average_Avg. Disk sec/Read	|     Yes - Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Linux Performance Counter      |
|	 Average_Avg. Disk sec/Transfer	|     Yes - Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Linux Performance Counter      |
|	 Average_Avg. Disk sec/Write	|     Yes - Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Linux Performance Counter      |
|	 Average_Physical Disk Bytes/sec	|     Yes - Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Linux Performance Counter      |
|	 Average_Pct Privileged Time	|     Yes - Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Linux Performance Counter      |
|	 Average_Pct User Time	|     Yes - Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Linux Performance Counter      |
|	 Average_Used Memory kBytes	|     Yes - Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Linux Performance Counter      |
|	 Average_Virtual Shared Memory	|     Yes - Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Linux Performance Counter      |
|	 Average_% DPC Time	|     Yes - Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Linux Performance Counter      |
|	 Average_% Idle Time	|     Yes - Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Linux Performance Counter      |
|	 Average_% Interrupt Time	|     Yes - Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Linux Performance Counter      |
|	 Average_% IO Wait Time	|     Yes - Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Linux Performance Counter      |
|	 Average_% Nice Time	|     Yes - Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Linux Performance Counter      |
|	 Average_% Privileged Time	|     Yes - Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Linux Performance Counter      |
|	 Average_% Processor Time	|     Yes - Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Linux Performance Counter      |
|	 Average_% User Time	|     Yes - Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Linux Performance Counter      |
|	 Average_Free Physical Memory	|     Yes - Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Linux Performance Counter      |
|	 Average_Free Space in Paging Files	|     Yes - Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Linux Performance Counter      |
|	 Average_Free Virtual Memory	|     Yes - Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Linux Performance Counter      |
|	 Average_Processes	|     Yes - Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Linux Performance Counter      |
|	 Average_Size Stored In Paging Files	|     Yes - Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Linux Performance Counter      |
|	 Average_Uptime	|     Yes - Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Linux Performance Counter      |
|	 Average_Users	|     Yes - Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Linux Performance Counter      |
|	 Heartbeat	|     Yes - Computer, OSType, Version & SourceComputerId    |   Heartbeat Records |
|	 Update	|     Yes - Computer, Product, Classification, UpdateState, Optional & Approved    |   Update Management |



## Payload schema

The POST operation contains the following JSON payload and schema for all near newer metric alerts when an appropriately configured [action group](monitoring-action-groups.md) is used:

```json
{"schemaId":"AzureMonitorMetricAlert","data":
    {
    "version": "2.0",
    "status": "Activated",
    "context": {
    "timestamp": "2018-02-28T10:44:10.1714014Z",
    "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/Contoso/providers/microsoft.insights/metricAlerts/StorageCheck",
    "name": "StorageCheck",
    "description": "",
    "conditionType": "SingleResourceMultipleMetricCriteria",
    "condition": {
      "windowSize": "PT5M",
      "allOf": [
        {
          "metricName": "Transactions",
          "dimensions": [
            {
              "name": "AccountResourceId",
              "value": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/Contoso/providers/Microsoft.Storage/storageAccounts/diag500"
            },
            {
              "name": "GeoType",
              "value": "Primary"
            }
          ],
          "operator": "GreaterThan",
          "threshold": "0",
          "timeAggregation": "PT5M",
          "metricValue": 1.0
        },
      ]
    },
    "subscriptionId": "00000000-0000-0000-0000-000000000000",
    "resourceGroupName": "Contoso",
    "resourceName": "diag500",
    "resourceType": "Microsoft.Storage/storageAccounts",
    "resourceId": "/subscriptions/1e3ff1c0-771a-4119-a03b-be82a51e232d/resourceGroups/Contoso/providers/Microsoft.Storage/storageAccounts/diag500",
    "portalLink": "https://portal.azure.com/#resource//subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/Contoso/providers/Microsoft.Storage/storageAccounts/diag500"
  },
        "properties": {
                "key1": "value1",
                "key2": "value2"
        }
    }
}
```

## Next steps

* Learn more about the new [Alerts experience](monitoring-overview-unified-alerts.md).
* Learn about [log alerts in Azure](monitor-alerts-unified-log.md).
* Learn about [alerts in Azure](monitoring-overview-alerts.md).
