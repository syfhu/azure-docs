---
title: Azure Diagnostic Logs supported services and schemas
description: Understand the supported services and event schema for Azure Diagnostic Logs.
author: johnkemnetz
services: azure-monitor
ms.service: azure-monitor
ms.topic: reference
ms.date: 6/08/2018
ms.author: johnkem
ms.component: logs
---
# Supported services, schemas, and categories for Azure Diagnostic Logs

[Azure resource diagnostic logs](monitoring-overview-of-diagnostic-logs.md) are logs emitted by your Azure resources that describe the operation of that resource. These logs are resource-type specific. In this article, we outline the set of supported services and event schema for events emitted by each service. This article also includes a full list of available log categories per resource type.

## Supported services and schemas for resource diagnostic logs
The schema for resource diagnostic logs varies depending on the resource and log category.   

| Service | Schema & Docs |
| --- | --- |
| Analysis Services | https://azure.microsoft.com/blog/azure-analysis-services-integration-with-azure-diagnostic-logs/ |
| API Management | [API Management Diagnostic Logs](../api-management/api-management-howto-use-azure-monitor.md#diagnostic-logs) |
| Application Gateways |[Diagnostics Logging for Application Gateway](../application-gateway/application-gateway-diagnostics.md) |
| Azure Automation |[Log analytics for Azure Automation](../automation/automation-manage-send-joblogs-log-analytics.md) |
| Azure Batch |[Azure Batch diagnostic logging](../batch/batch-diagnostics.md) |
| Content Delivery Network | [Azure Diagnostic Logs for CDN](../cdn/cdn-azure-diagnostic-logs.md) |
| CosmosDB | [Azure Cosmos DB Logging](../cosmos-db/logging.md) |
| Data Factory | [Monitor Data Factories using Azure Monitor](../data-factory/monitor-using-azure-monitor.md) |
| Data Lake Analytics |[Accessing diagnostic logs for Azure Data Lake Analytics](../data-lake-analytics/data-lake-analytics-diagnostic-logs.md) |
| Data Lake Store |[Accessing diagnostic logs for Azure Data Lake Store](../data-lake-store/data-lake-store-diagnostic-logs.md) |
| DB for PostgreSQL |  Schema not available. |
| Event Hubs |[Azure Event Hubs diagnostic logs](../event-hubs/event-hubs-diagnostic-logs.md) |
| Express Route | Schema not available. |
| IoT Hub | [IoT Hub Operations](../iot-hub/iot-hub-monitor-resource-health.md#use-azure-monitor) |
| Key Vault |[Azure Key Vault Logging](../key-vault/key-vault-logging.md) |
| Load Balancer |[Log analytics for Azure Load Balancer](../load-balancer/load-balancer-monitor-log.md) |
| Logic Apps |[Logic Apps B2B custom tracking schema](../logic-apps/logic-apps-track-integration-account-custom-tracking-schema.md) |
| Network Security Groups |[Log analytics for network security groups (NSGs)](../virtual-network/virtual-network-nsg-manage-log.md) |
| DDOS Protection | [Manage Azure DDoS Protection Standard](../virtual-network/manage-ddos-protection.md) |
| PowerBI Dedicated | Schema not available. |
| Recovery Services | [Data Model for Azure Backup](../backup/backup-azure-reports-data-model.md)|
| Search |[Enabling and using Search Traffic Analytics](../search/search-traffic-analytics.md) |
| Service Bus |[Azure Service Bus diagnostic logs](../service-bus-messaging/service-bus-diagnostic-logs.md) |
| SQL Database | [Azure SQL Database diagnostic logging](../sql-database/sql-database-metrics-diag-logging.md) |
| Stream Analytics |[Job diagnostic logs](../stream-analytics/stream-analytics-job-diagnostic-logs.md) |
| Traffic Manager | Schema not available. |
| Virtual Networks | Schema not available. |
| Virtual Network Gateways | Schema not available. |

## Supported log categories per resource type
|Resource Type|Category|Category Display Name|
|---|---|---|
|Microsoft.AnalysisServices/servers|Engine|Engine|
|Microsoft.AnalysisServices/servers|Service|Service|
|Microsoft.ApiManagement/service|GatewayLogs|Logs related to ApiManagement Gateway|
|Microsoft.Automation/automationAccounts|JobLogs|Job Logs|
|Microsoft.Automation/automationAccounts|JobStreams|Job Streams|
|Microsoft.Automation/automationAccounts|DscNodeStatus|Dsc Node Status|
|Microsoft.Batch/batchAccounts|ServiceLog|Service Logs|
|Microsoft.Cdn/profiles/endpoints|CoreAnalytics|Gets the metrics of the endpoint, e.g., bandwidth, egress, etc.|
|Microsoft.CustomerInsights/hubs|AuditEvents|AuditEvents|
|Microsoft.DataFactory/factories|ActivityRuns|Pipeline activity runs log|
|Microsoft.DataFactory/factories|PipelineRuns|Pipeline runs log|
|Microsoft.DataFactory/factories|TriggerRuns|Trigger runs log|
|Microsoft.DataLakeAnalytics/accounts|Audit|Audit Logs|
|Microsoft.DataLakeAnalytics/accounts|Requests|Request Logs|
|Microsoft.DataLakeStore/accounts|Audit|Audit Logs|
|Microsoft.DataLakeStore/accounts|Requests|Request Logs|
|Microsoft.DBforPostgreSQL/servers|PostgreSQLLogs|PostgreSQL Server Logs|
|Microsoft.DBforPostgreSQL/servers|PostgreSQLBackupEvents|PostgreSQL Backup Events|
|Microsoft.Devices/IotHubs|Connections|Connections|
|Microsoft.Devices/IotHubs|DeviceTelemetry|Device Telemetry|
|Microsoft.Devices/IotHubs|C2DCommands|C2D Commands|
|Microsoft.Devices/IotHubs|DeviceIdentityOperations|Device Identity Operations|
|Microsoft.Devices/IotHubs|FileUploadOperations|File Upload Operations|
|Microsoft.Devices/IotHubs|Routes|Routes|
|Microsoft.Devices/IotHubs|D2CTwinOperations|D2CTwinOperations|
|Microsoft.Devices/IotHubs|C2DTwinOperations|C2D Twin Operations|
|Microsoft.Devices/IotHubs|TwinQueries|Twin Queries|
|Microsoft.Devices/IotHubs|JobsOperations|Jobs Operations|
|Microsoft.Devices/IotHubs|DirectMethods|Direct Methods|
|Microsoft.Devices/IotHubs|E2EDiagnostics|E2E Diagnostics (Preview)|
|Microsoft.Devices/provisioningServices|DeviceOperations|Device Operations|
|Microsoft.Devices/provisioningServices|ServiceOperations|Service Operations|
|Microsoft.DocumentDB/databaseAccounts|DataPlaneRequests|DataPlaneRequests|
|Microsoft.DocumentDB/databaseAccounts|MongoRequests|MongoRequests|
|Microsoft.DocumentDB/databaseAccounts|QueryRuntimeStatistics|QueryRuntimeStatistics|
|Microsoft.EventHub/namespaces|ArchiveLogs|Archive Logs|
|Microsoft.EventHub/namespaces|OperationalLogs|Operational Logs|
|Microsoft.EventHub/namespaces|AutoScaleLogs|Auto Scale Logs|
|Microsoft.KeyVault/vaults|AuditEvent|Audit Logs|
|Microsoft.Logic/workflows|WorkflowRuntime|Workflow runtime diagnostic events|
|Microsoft.Logic/integrationAccounts|IntegrationAccountTrackingEvents|Integration Account track events|
|Microsoft.Network/networksecuritygroups|NetworkSecurityGroupEvent|Network Security Group Event|
|Microsoft.Network/networksecuritygroups|NetworkSecurityGroupRuleCounter|Network Security Group Rule Counter|
|Microsoft.Network/loadBalancers|LoadBalancerAlertEvent|Load Balancer Alert Events|
|Microsoft.Network/loadBalancers|LoadBalancerProbeHealthStatus|Load Balancer Probe Health Status|
|Microsoft.Network/publicIPAddresses|DDoSProtectionNotifications|DDoS protection notifications|
|Microsoft.Network/virtualNetworks|VMProtectionAlerts|VM protection alerts|
|Microsoft.Network/applicationGateways|ApplicationGatewayAccessLog|Application Gateway Access Log|
|Microsoft.Network/applicationGateways|ApplicationGatewayPerformanceLog|Application Gateway Performance Log|
|Microsoft.Network/applicationGateways|ApplicationGatewayFirewallLog|Application Gateway Firewall Log|
|Microsoft.Network/virtualNetworkGateways|GatewayDiagnosticLog|Gateway Diagnostic Logs|
|Microsoft.Network/virtualNetworkGateways|TunnelDiagnosticLog|Tunnel Diagnostic Logs|
|Microsoft.Network/virtualNetworkGateways|RouteDiagnosticLog|Route Diagnostic Logs|
|Microsoft.Network/virtualNetworkGateways|IKEDiagnosticLog|IKE Diagnostic Logs|
|Microsoft.Network/virtualNetworkGateways|P2SDiagnosticLog|P2S Diagnostic Logs|
|Microsoft.Network/trafficManagerProfiles|ProbeHealthStatusEvents|Traffic Manager Probe Health Results Event|
|Microsoft.Network/expressRouteCircuits|GWMCountersTable|Table of GWM counters|
|Microsoft.PowerBIDedicated/capacities|Engine|Engine|
|Microsoft.RecoveryServices/Vaults|AzureBackupReport|Azure Backup Reporting Data|
|Microsoft.RecoveryServices/Vaults|AzureSiteRecoveryJobs|Azure Site Recovery Jobs|
|Microsoft.RecoveryServices/Vaults|AzureSiteRecoveryEvents|Azure Site Recovery Events|
|Microsoft.RecoveryServices/Vaults|AzureSiteRecoveryReplicatedItems|Azure Site Recovery Replicated Items|
|Microsoft.RecoveryServices/Vaults|AzureSiteRecoveryReplicationStats|Azure Site Recovery Replication Stats|
|Microsoft.RecoveryServices/Vaults|AzureSiteRecoveryRecoveryPoints|Azure Site Recovery Recovery Points|
|Microsoft.RecoveryServices/Vaults|AzureSiteRecoveryReplicationDataUploadRate|Azure Site Recovery Replication Data Upload Rate|
|Microsoft.RecoveryServices/Vaults|AzureSiteRecoveryProtectedDiskDataChurn|Azure Site Recovery Protected Disk Data Churn|
|Microsoft.Search/searchServices|OperationLogs|Operation Logs|
|Microsoft.ServiceBus/namespaces|OperationalLogs|Operational Logs|
|Microsoft.Sql/servers/databases|QueryStoreRuntimeStatistics|Query Store Runtime Statistics|
|Microsoft.Sql/servers/databases|QueryStoreWaitStatistics|Query Store Wait Statistics|
|Microsoft.Sql/servers/databases|Errors|Errors|
|Microsoft.Sql/servers/databases|DatabaseWaitStatistics|Database Wait Statistics|
|Microsoft.Sql/servers/databases|Timeouts|Timeouts|
|Microsoft.Sql/servers/databases|Blocks|Blocks|
|Microsoft.Sql/servers/databases|SQLInsights|SQL Insights|
|Microsoft.Sql/servers/databases|Audit|Audit Logs|
|Microsoft.Sql/servers/databases|SQLSecurityAuditEvents|SQL Security Audit Event|
|Microsoft.StreamAnalytics/streamingjobs|Execution|Execution|
|Microsoft.StreamAnalytics/streamingjobs|Authoring|Authoring|

## Next Steps

* [Learn more about diagnostic logs](monitoring-overview-of-diagnostic-logs.md)
* [Stream resource diagnostic logs to **Event Hubs**](monitoring-stream-diagnostic-logs-to-event-hubs.md)
* [Change resource diagnostic settings using the Azure Monitor REST API](https://msdn.microsoft.com/library/azure/dn931931.aspx)
* [Analyze logs from Azure storage with Log Analytics](../log-analytics/log-analytics-azure-storage.md)
