---
title: Servicios y esquemas admitidos de los registros de Azure Diagnostics
description: Conozca el esquema de los eventos y servicios admitidos para los registros de diagnóstico de Azure.
author: johnkemnetz
services: azure-monitor
ms.service: azure-monitor
ms.topic: reference
ms.date: 7/18/2018
ms.author: johnkem
ms.component: logs
ms.openlocfilehash: c1189e1b120f0bd1b3169618bebdb929d1cee18e
ms.sourcegitcommit: 156364c3363f651509a17d1d61cf8480aaf72d1a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/25/2018
ms.locfileid: "39248798"
---
# <a name="supported-services-schemas-and-categories-for-azure-diagnostic-logs"></a>Servicios, esquemas y categorías admitidos en los registros de diagnóstico de Azure

Los [registros de diagnóstico de Azure Monitor](monitoring-overview-of-diagnostic-logs.md) son los registros emitidos por los servicios de Azure que describen el funcionamiento de dichos servicios o recursos. Todos los registros de diagnóstico disponibles a través de Azure Monitor comparten un esquema común de nivel superior, con flexibilidad para que cada servicio emita propiedades únicas para sus propios eventos.

Una combinación del tipo de recurso (disponible en la propiedad `resourceId`) y la `category` identifica un esquema de forma única. En este artículo se describe el esquema de nivel superior para los registros de diagnóstico y los vínculos a los esquemas para cada servicio.

## <a name="top-level-diagnostic-logs-schema"></a>Esquema de registros de diagnósticos de nivel superior

| NOMBRE | Obligatorio/opcional | DESCRIPCIÓN |
|---|---|---|
| Twitter en tiempo | Obligatorio | Marca de tiempo (UTC) del evento. |
| ResourceId | Obligatorio | Identificador del recurso que ha emitido el evento. Para los servicios de inquilino, presenta la forma /tenants/tenant-id/providers/provider-name. |
| tenantId | Necesario para los registros de inquilinos | El identificador del inquilino de Active Directory al que está asociado este evento. Esta propiedad solo se usa para los registros de nivel de inquilino; no aparece en los registros de nivel de recurso. |
| operationName | Obligatorio | Nombre de la operación representada por este evento. Si el evento representa una operación de RBAC, este es el nombre de la operación de RBAC (p. ej. Microsoft.Storage/storageAccounts/blobServices/blobs/Read). Se suele modelar con la forma de una operación de Resource Manager, incluso si no son operaciones de Resource Manager documentadas reales (`Microsoft.<providerName>/<resourceType>/<subtype>/<Write/Read/Delete/Action>`) |
| operationVersion | Opcional | La versión de API asociada con la operación, si operationName se ha realizado mediante una API (p. ej. http://myservice.windowsazure.net/object?api-version=2016-06-01). Si no hay ninguna API que se corresponde con esta operación, la versión representa la versión de esa operación en caso de que las propiedades asociadas con la operación cambien en el futuro. |
| categoría | Obligatorio | Categoría de registro del evento. La categoría es la granularidad con la que se pueden habilitar o deshabilitar los registros en un recurso determinado. Las propiedades que aparecen en el blob de propiedades de un evento son las mismas dentro de una categoría de registro y un tipo de recurso concretos. Las categorías de registro típicas son "Audit", "Operational" "Execution" y "Request". |
| resultType | Opcional | Estado del evento. Entre los valores habituales, se incluyen Started, In Progress, Succeeded, Failed, Active y Resolved. |
| resultSignature | Opcional | Subestado del evento. Si esta operación se corresponde con una llamada API REST, este es el código de estado HTTP de la llamada REST correspondiente. |
| resultDescription | Opcional | Descripción de texto estático de esta operación, por ejemplo. "Obtener el archivo de almacenamiento". |
| durationMs | Opcional | Duración de la operación en milisegundos. |
| callerIpAddress | Opcional | Dirección IP del autor de la llamada, si la operación se corresponde con una llamada API que podría proceder de una entidad con una dirección IP disponible públicamente. |
| correlationId | Opcional | GUID que se usa para agrupar un conjunto de eventos relacionados. Normalmente, si dos eventos tienen el mismo valor operationName pero dos estados diferentes (p. ej. "Started" y "Succeeded"), comparten el mismo identificador de correlación. Esto también puede representar otras relaciones entre los eventos. |
| identidad | Opcional | Blob JSON que describe la identidad del usuario o la aplicación que realizó la operación. Normalmente esto incluirá la autorización y las notificaciones o el token JWT de Active Directory. |
| Nivel | Opcional | Nivel de gravedad del evento. Debe ser uno de entre Informativo, Advertencia, Error o Crítico. |
| location | Opcional | Región del recurso que emite el evento, por ejemplo, "Este de EE. UU." o "Sur de Francia". |
| propiedades | Opcional | Todas las propiedades extendidas relacionadas con esta categoría de eventos determinada. Todas las propiedades personalizadas o únicas se deben colocar dentro de esta "Parte B" del esquema. |

## <a name="service-specific-schemas-for-resource-diagnostic-logs"></a>Esquemas específicos del servicio para los registros de diagnóstico de recursos
El esquema para los registros de diagnóstico de recursos varía según la categoría de registro y el recurso. En esta lista se muestran todos los servicios que hacen que los registros de diagnóstico y los vínculos estén disponibles para el servicio y el esquema específico de la categoría cuando sea posible.

| Servicio | Esquema y documentos |
| --- | --- |
| Azure Active Directory | [Información general](../active-directory/reporting-azure-monitor-diagnostics-overview.md), [Esquema de registros de auditoría](../active-directory/reporting-azure-monitor-diagnostics-audit-log-schema.md) y [Esquema de inicios de sesión](../active-directory/reporting-azure-monitor-diagnostics-sign-in-log-schema.md) |
| Analysis Services | https://azure.microsoft.com/blog/azure-analysis-services-integration-with-azure-diagnostic-logs/ |
| API Management | [Registros de diagnóstico de API Management](../api-management/api-management-howto-use-azure-monitor.md#diagnostic-logs) |
| Puertas de enlace de aplicaciones |[Registro de diagnóstico para Application Gateway](../application-gateway/application-gateway-diagnostics.md) |
| Azure Automation |[Log Analytics para Azure Automation](../automation/automation-manage-send-joblogs-log-analytics.md) |
| Azure Batch |[Registros de diagnósticos de Azure Batch](../batch/batch-diagnostics.md) |
| Content Delivery Network | [Registro de Azure Diagnostics para CDN](../cdn/cdn-azure-diagnostic-logs.md) |
| CosmosDB | [Registro de Azure Cosmos DB](../cosmos-db/logging.md) |
| Data Factory | [Supervisión de factorías de datos mediante Azure Monitor](../data-factory/monitor-using-azure-monitor.md) |
| Data Lake Analytics |[Acceso a los registros de diagnóstico de Azure Data Lake Analytics](../data-lake-analytics/data-lake-analytics-diagnostic-logs.md) |
| Data Lake Store |[Acceso a los registros de diagnóstico de Azure Data Lake Store](../data-lake-store/data-lake-store-diagnostic-logs.md) |
| DB for PostgreSQL |  Esquema no disponible. |
| Event Hubs |[Registros de diagnóstico de Azure Event Hubs](../event-hubs/event-hubs-diagnostic-logs.md) |
| ExpressRoute | Esquema no disponible. |
| IoT Hub | [Operaciones de IoT Hub](../iot-hub/iot-hub-monitor-resource-health.md#use-azure-monitor) |
| Key Vault |[Registro de Azure Key Vault](../key-vault/key-vault-logging.md) |
| Load Balancer |[Log Analytics para Azure Load Balancer](../load-balancer/load-balancer-monitor-log.md) |
| Logic Apps |[Esquema de seguimiento personalizado de Logic Apps B2B](../logic-apps/logic-apps-track-integration-account-custom-tracking-schema.md) |
| Grupos de seguridad de red |[Análisis del registro para grupos de seguridad de red (NSG)](../virtual-network/virtual-network-nsg-manage-log.md) |
| DDOS Protection | [Administración de Azure DDoS Protection estándar](../virtual-network/manage-ddos-protection.md) |
| Dedicado a PowerBI | Esquema no disponible. |
| Recovery Services | [Modelo de datos para Azure Backup](../backup/backup-azure-reports-data-model.md)|
| Search |[Habilitación y uso de Análisis de tráfico de búsqueda](../search/search-traffic-analytics.md) |
| Azure Service Bus |[Registros de diagnóstico de Azure Service Bus](../service-bus-messaging/service-bus-diagnostic-logs.md) |
| SQL Database | [Registro de diagnóstico de Azure SQL Database](../sql-database/sql-database-metrics-diag-logging.md) |
| Stream Analytics |[Registros de diagnósticos de trabajos](../stream-analytics/stream-analytics-job-diagnostic-logs.md) |
| Traffic Manager | Esquema no disponible. |
| Virtual Networks | Esquema no disponible. |
| Puertas de enlace de red virtual | Esquema no disponible. |

## <a name="supported-log-categories-per-resource-type"></a>Categorías de registro admitidas por tipo de recurso
|Tipo de recurso|Categoría|Nombre para mostrar de categoría|
|---|---|---|
|Microsoft.AnalysisServices/servers|Motor|Motor|
|Microsoft.AnalysisServices/servers|Servicio|Servicio|
|Microsoft.ApiManagement/service|GatewayLogs|Registros relacionados con la puerta de enlace de ApiManagement|
|Microsoft.Automation/automationAccounts|JobLogs|Registros de trabajo|
|Microsoft.Automation/automationAccounts|JobStreams|Flujos de trabajo|
|Microsoft.Automation/automationAccounts|DscNodeStatus|Estado del nodo de DSC|
|Microsoft.Batch/batchAccounts|ServiceLog|Registros de servicios|
|Microsoft.Cdn/profiles/endpoints|CoreAnalytics|Obtiene las métricas del punto de conexión; por ejemplo, ancho de banda, salida, etc.|
|Microsoft.CustomerInsights/hubs|AuditEvents|AuditEvents|
|Microsoft.DataFactory/factories|ActivityRuns|Registro de ejecuciones de actividad de canalización|
|Microsoft.DataFactory/factories|PipelineRuns|Registro de ejecuciones de canalización|
|Microsoft.DataFactory/factories|TriggerRuns|Registro de ejecuciones de desencadenador|
|Microsoft.DataLakeAnalytics/accounts|Auditoría|Registros de auditoría|
|Microsoft.DataLakeAnalytics/accounts|Solicitudes|Registros de solicitud|
|Microsoft.DataLakeStore/accounts|Auditoría|Registros de auditoría|
|Microsoft.DataLakeStore/accounts|Requests|Registros de solicitud|
|Microsoft.DBforPostgreSQL/servers|PostgreSQLLogs|Registros del servidor PostgreSQL|
|Microsoft.DBforPostgreSQL/servers|PostgreSQLBackupEvents|Eventos de copia de seguridad de PostgreSQL|
|Microsoft.Devices/IotHubs|Conexiones|Conexiones|
|Microsoft.Devices/IotHubs|DeviceTelemetry|Telemetría de dispositivo|
|Microsoft.Devices/IotHubs|C2DCommands|Comandos C2D|
|Microsoft.Devices/IotHubs|DeviceIdentityOperations|Operaciones de identidad de dispositivos|
|Microsoft.Devices/IotHubs|FileUploadOperations|Operaciones de carga de archivos|
|Microsoft.Devices/IotHubs|Rutas|Rutas|
|Microsoft.Devices/IotHubs|D2CTwinOperations|D2CTwinOperations|
|Microsoft.Devices/IotHubs|C2DTwinOperations|Operaciones gemelas C2D|
|Microsoft.Devices/IotHubs|TwinQueries|Consultas gemelas|
|Microsoft.Devices/IotHubs|JobsOperations|Operaciones de trabajos|
|Microsoft.Devices/IotHubs|DirectMethods|Métodos directos|
|Microsoft.Devices/IotHubs|E2EDiagnostics|Diagnóstico de E2E (versión preliminar)|
|Microsoft.Devices/provisioningServices|DeviceOperations|Operaciones de dispositivo|
|Microsoft.Devices/provisioningServices|ServiceOperations|Operaciones del servicio|
|Microsoft.DocumentDB/databaseAccounts|DataPlaneRequests|DataPlaneRequests|
|Microsoft.DocumentDB/databaseAccounts|MongoRequests|MongoRequests|
|Microsoft.DocumentDB/databaseAccounts|QueryRuntimeStatistics|QueryRuntimeStatistics|
|Microsoft.EventHub/namespaces|ArchiveLogs|Registros de archivo|
|Microsoft.EventHub/namespaces|OperationalLogs|Registros operativos|
|Microsoft.EventHub/namespaces|AutoScaleLogs|Registros de escalado automático|
|Microsoft.KeyVault/vaults|AuditEvent|Registros de auditoría|
|Microsoft.Logic/workflows|WorkflowRuntime|Eventos de diagnóstico en tiempo de ejecución de flujo de trabajo|
|Microsoft.Logic/integrationAccounts|IntegrationAccountTrackingEvents|Eventos de seguimiento de la cuenta de integración|
|Microsoft.Network/networksecuritygroups|NetworkSecurityGroupEvent|Evento de grupo de seguridad de red|
|Microsoft.Network/networksecuritygroups|NetworkSecurityGroupRuleCounter|Contador de reglas de grupo de seguridad de red|
|Microsoft.Network/loadBalancers|LoadBalancerAlertEvent|Eventos de alerta de equilibrador de carga|
|Microsoft.Network/loadBalancers|LoadBalancerProbeHealthStatus|Estado de mantenimiento de sondeo de equilibrador de carga|
|Microsoft.Network/publicIPAddresses|DDoSProtectionNotifications|Notificaciones de protección contra DDoS|
|Microsoft.Network/virtualNetworks|VMProtectionAlerts|Alertas de protección de máquina virtual|
|Microsoft.Network/applicationGateways|ApplicationGatewayAccessLog|Registro de acceso de Application Gateway|
|Microsoft.Network/applicationGateways|ApplicationGatewayPerformanceLog|Registro de rendimiento de Application Gateway|
|Microsoft.Network/applicationGateways|ApplicationGatewayFirewallLog|Registro de Firewall de Application Gateway|
|Microsoft.Network/virtualNetworkGateways|GatewayDiagnosticLog|Registros de diagnóstico de puerta de enlace|
|Microsoft.Network/virtualNetworkGateways|TunnelDiagnosticLog|Registros de diagnóstico de túnel|
|Microsoft.Network/virtualNetworkGateways|RouteDiagnosticLog|Registros de diagnóstico de ruta|
|Microsoft.Network/virtualNetworkGateways|IKEDiagnosticLog|Registros de IKE Diagnostic|
|Microsoft.Network/virtualNetworkGateways|P2SDiagnosticLog|Registros de P2S Diagnostic|
|Microsoft.Network/trafficManagerProfiles|ProbeHealthStatusEvents|Evento de resultados de estado de sondeo de Traffic Manager|
|Microsoft.Network/expressRouteCircuits|GWMCountersTable|Tabla de contadores GWM|
|Microsoft.PowerBIDedicated/capacities|Motor|Motor|
|Microsoft.RecoveryServices/Vaults|AzureBackupReport|Datos de informes de Azure Backup|
|Microsoft.RecoveryServices/Vaults|AzureSiteRecoveryJobs|Trabajos de Azure Site Recovery|
|Microsoft.RecoveryServices/Vaults|AzureSiteRecoveryEvents|Eventos de Azure Site Recovery|
|Microsoft.RecoveryServices/Vaults|AzureSiteRecoveryReplicatedItems|Elementos replicados de Azure Site Recovery|
|Microsoft.RecoveryServices/Vaults|AzureSiteRecoveryReplicationStats|Estadísticas de replicación de Azure Site Recovery|
|Microsoft.RecoveryServices/Vaults|AzureSiteRecoveryRecoveryPoints|Puntos de recuperación de Azure Site Recovery|
|Microsoft.RecoveryServices/Vaults|AzureSiteRecoveryReplicationDataUploadRate|Velocidad de carga de datos de replicación de Azure Site Recovery|
|Microsoft.RecoveryServices/Vaults|AzureSiteRecoveryProtectedDiskDataChurn|Actividad de los datos del disco protegido de Azure Site Recovery|
|Microsoft.Search/searchServices|OperationLogs|Registros de operaciones|
|Microsoft.ServiceBus/namespaces|OperationalLogs|Registros operativos|
|Microsoft.Sql/servers/databases|QueryStoreRuntimeStatistics|Estadísticas de tiempo de ejecución del Almacén de consultas|
|Microsoft.Sql/servers/databases|QueryStoreWaitStatistics|Estadísticas de espera del Almacén de consultas|
|Microsoft.Sql/servers/databases|Errors|Errors|
|Microsoft.Sql/servers/databases|DatabaseWaitStatistics|Estadísticas de espera de base de datos|
|Microsoft.Sql/servers/databases|Tiempos de expiración|Tiempos de expiración|
|Microsoft.Sql/servers/databases|Blocks|Blocks|
|Microsoft.Sql/servers/databases|SQLInsights|SQL Insights|
|Microsoft.Sql/servers/databases|Auditoría|Registros de auditoría|
|Microsoft.Sql/servers/databases|SQLSecurityAuditEvents|Evento de auditoría de seguridad de SQL|
|Microsoft.StreamAnalytics/streamingjobs|Ejecución|Ejecución|
|Microsoft.StreamAnalytics/streamingjobs|Creación|Creación|

## <a name="next-steps"></a>Pasos siguientes

* [Más información sobre los registros de diagnóstico](monitoring-overview-of-diagnostic-logs.md)
* [Transmisión de registros de diagnóstico a **Event Hubs**](monitoring-stream-diagnostic-logs-to-event-hubs.md)
* [Cambio de la configuración de diagnóstico de recursos con la API de REST de Azure Monitor](https://msdn.microsoft.com/library/azure/dn931931.aspx)
* [Análisis de registros desde Azure Storage con Log Analytics](../log-analytics/log-analytics-azure-storage.md)
