---
title: Introducción a los registros de recurso de Azure | Microsoft Docs
description: Conozca el esquema de los eventos y servicios admitidos para los registros de recurso de Azure.
author: rboucher
services: azure-monitor
ms.service: azure-monitor
ms.topic: reference
ms.date: 09/20/2019
ms.author: robb
ms.subservice: logs
ms.openlocfilehash: bfcd2ded96c2679ba9177a760a8b11dc7d2c9a77
ms.sourcegitcommit: 55f7fc8fe5f6d874d5e886cb014e2070f49f3b94
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/25/2019
ms.locfileid: "71263035"
---
# <a name="azure-resource-logs-overview"></a>Introducción a los registros de recurso de Azure
Los registros de recurso de Azure son [registros de plataforma](platform-logs-overview.md) emitidos por los recursos de Azure que describen su funcionamiento interno. Todos los registros de recurso comparten un esquema general común, con flexibilidad para que cada servicio emita propiedades únicas para sus propios eventos.

> [!NOTE]
> Los registros de recurso se conocían anteriormente como registros de diagnóstico.

## <a name="collecting-resource-logs"></a>Recopilación de registros de recurso
Los registros de recurso los generan los recursos de Azure compatibles automáticamente, pero no se recopilan a menos que establezca una [configuración de diagnóstico](diagnostic-settings.md). Cree una configuración de diagnóstico para cada recurso de Azure para reenviar los registros a los destinos siguientes:

| Destino | Escenario |
|:---|:---|:---|
| [Área de trabajo de Log Analytics](resource-logs-collect-storage.md) | Analice los registros con otros datos de supervisión y aproveche las características de Azure Monitor, como las consultas de registro y las alertas de registro. |
| [Almacenamiento de Azure](archive-diagnostic-logs.md) | Archive los registros para la auditoría o la copia de seguridad. |
| [Centro de eventos](resource-logs-stream-event-hubs.md) | Transmita los registros a sistemas de registro y telemetría de terceros.  |

## <a name="compute-resources"></a>Recursos de proceso
Los registros de recurso se diferencian de los de nivel de sistema operativo invitado en los recursos de proceso de Azure. Los recursos de proceso requieren un agente para recopilar los registros y las métricas de su sistema operativo invitado, que incluye datos como los registros de eventos, syslog y el contador de rendimiento. Use la [extensión Diagnostics](agents-overview.md#azure-diagnostic-extension) para enrutar los datos de registro de las máquinas virtuales de Azure y el [agente de Log Analytics](agents-overview.md#log-analytics-agent) para recopilar registros y métricas de las máquinas virtuales en Azure, en otras nubes y en entornos locales en un área de trabajo de Log Analytics. Consulte [Orígenes de datos de supervisión para Azure Monitor](data-sources.md) para más detalles.

## <a name="resource-logs-schema"></a>Esquema de registros de recurso
Cada servicio de Azure tiene su propio esquema de escritura de registros de recurso en uno de los destinos, pero todos comparten un esquema general, que se muestra en la tabla siguiente. Consulte a continuación [Esquemas específicos por servicio](#service-specific-schemas) para los vínculos al esquema correspondiente a cada servicio. 

| NOMBRE | Obligatorio/opcional | DESCRIPCIÓN |
|---|---|---|
| time | Obligatorio | Marca de tiempo (UTC) del evento. |
| resourceId | Obligatorio | Identificador del recurso que ha emitido el evento. Para los servicios de inquilino, presenta la forma /tenants/tenant-id/providers/provider-name. |
| tenantId | Necesario para los registros de inquilinos | El identificador del inquilino de Active Directory al que está asociado este evento. Esta propiedad solo se usa para los registros de nivel de inquilino; no aparece en los registros de nivel de recurso. |
| operationName | Obligatorio | Nombre de la operación representada por este evento. Si el evento representa una operación de RBAC, este es el nombre de la operación de RBAC (p. ej. Microsoft.Storage/storageAccounts/blobServices/blobs/Read). Se suele modelar con la forma de una operación de Resource Manager, incluso si no son operaciones de Resource Manager documentadas reales (`Microsoft.<providerName>/<resourceType>/<subtype>/<Write/Read/Delete/Action>`) |
| operationVersion | Opcional | La versión de API asociada con la operación, si operationName se ha realizado mediante una API (p. ej. `http://myservice.windowsazure.net/object?api-version=2016-06-01`). Si no hay ninguna API que se corresponde con esta operación, la versión representa la versión de esa operación en caso de que las propiedades asociadas con la operación cambien en el futuro. |
| category | Obligatorio | Categoría de registro del evento. La categoría es la granularidad con la que se pueden habilitar o deshabilitar los registros en un recurso determinado. Las propiedades que aparecen en el blob de propiedades de un evento son las mismas dentro de una categoría de registro y un tipo de recurso concretos. Las categorías de registro típicas son "Audit", "Operational" "Execution" y "Request". |
| resultType | Opcional | Estado del evento. Entre los valores habituales, se incluyen Started, In Progress, Succeeded, Failed, Active y Resolved. |
| resultSignature | Opcional | Subestado del evento. Si esta operación se corresponde con una llamada API REST, este es el código de estado HTTP de la llamada REST correspondiente. |
| resultDescription | Opcional | Descripción de texto estático de esta operación, por ejemplo. "Obtener el archivo de almacenamiento". |
| durationMs | Opcional | Duración de la operación en milisegundos. |
| callerIpAddress | Opcional | Dirección IP del autor de la llamada, si la operación se corresponde con una llamada API que podría proceder de una entidad con una dirección IP disponible públicamente. |
| correlationId | Opcional | GUID que se usa para agrupar un conjunto de eventos relacionados. Normalmente, si dos eventos tienen el mismo valor operationName pero dos estados diferentes (p. ej. "Started" y "Succeeded"), comparten el mismo identificador de correlación. Esto también puede representar otras relaciones entre los eventos. |
| identity | Opcional | Blob JSON que describe la identidad del usuario o la aplicación que realizó la operación. Normalmente esto incluirá la autorización y las notificaciones o el token JWT de Active Directory. |
| Nivel | Opcional | Nivel de gravedad del evento. Debe ser uno de entre Informativo, Advertencia, Error o Crítico. |
| location | Opcional | Región del recurso que emite el evento, por ejemplo, "Este de EE. UU." o "Sur de Francia". |
| properties | Opcional | Todas las propiedades extendidas relacionadas con esta categoría de eventos determinada. Todas las propiedades personalizadas o únicas se deben colocar dentro de esta "Parte B" del esquema. |

## <a name="service-specific-schemas"></a>Esquemas específicos por servicio
El esquema de los registros de recurso varía en función del tipo de recurso, definido por la propiedad `resourceId`, y las propiedades `category`. En la lista siguiente se muestran todos los servicios de Azure que admiten registros de recurso con vínculos al esquema específico por categoría y servicio, si están disponibles.

| Servicio | Esquema y documentos |
| --- | --- |
| Azure Active Directory | [Información general](../../active-directory/reports-monitoring/concept-activity-logs-azure-monitor.md), [Esquema de registros de auditoría](../../active-directory/reports-monitoring/reference-azure-monitor-audit-log-schema.md) y [Esquema de inicios de sesión](../../active-directory/reports-monitoring/reference-azure-monitor-sign-ins-log-schema.md) |
| Analysis Services | https://azure.microsoft.com/blog/azure-analysis-services-integration-with-azure-diagnostic-logs/ |
| API Management | [Registros de diagnóstico de API Management](../../api-management/api-management-howto-use-azure-monitor.md#diagnostic-logs) |
| Puertas de enlace de aplicaciones |[Registro de diagnóstico para Application Gateway](../../application-gateway/application-gateway-diagnostics.md) |
| Azure Automation |[Log Analytics para Azure Automation](../../automation/automation-manage-send-joblogs-log-analytics.md) |
| Azure Batch |[Registros de diagnósticos de Azure Batch](../../batch/batch-diagnostics.md) |
| Azure Database for MySQL | [Registros de diagnóstico de Azure Database for MySQL](../../mysql/concepts-server-logs.md#diagnostic-logs) |
| Azure Database for PostgreSQL | [Registros de diagnóstico de Azure Database for PostgreSQL](../../postgresql/concepts-server-logs.md#diagnostic-logs) |
| Cognitive Services | [Registro de diagnóstico para Azure Cognitive Services](../../cognitive-services/diagnostic-logging.md) |
| Content Delivery Network | [Registro de Azure Diagnostics para CDN](../../cdn/cdn-azure-diagnostic-logs.md) |
| CosmosDB | [Registro de Azure Cosmos DB](../../cosmos-db/logging.md) |
| Data Factory | [Supervisión de factorías de datos mediante Azure Monitor](../../data-factory/monitor-using-azure-monitor.md) |
| Data Lake Analytics |[Acceso a los registros de diagnóstico de Azure Data Lake Analytics](../../data-lake-analytics/data-lake-analytics-diagnostic-logs.md) |
| Data Lake Store |[Acceso a los registros de diagnóstico de Azure Data Lake Store](../../data-lake-store/data-lake-store-diagnostic-logs.md) |
| Event Hubs |[Registros de diagnóstico de Azure Event Hubs](../../event-hubs/event-hubs-diagnostic-logs.md) |
| ExpressRoute | Esquema no disponible. |
| Azure Firewall | Esquema no disponible. |
| IoT Hub | [Operaciones de IoT Hub](../../iot-hub/iot-hub-monitor-resource-health.md#use-azure-monitor) |
| Key Vault |[Registro de Azure Key Vault](../../key-vault/key-vault-logging.md) |
| Load Balancer |[Log Analytics para Azure Load Balancer](../../load-balancer/load-balancer-monitor-log.md) |
| Logic Apps |[Esquema de seguimiento personalizado de Logic Apps B2B](../../logic-apps/logic-apps-track-integration-account-custom-tracking-schema.md) |
| Grupos de seguridad de red |[Análisis del registro para grupos de seguridad de red (NSG)](../../virtual-network/virtual-network-nsg-manage-log.md) |
| DDOS Protection | [Administración de Azure DDoS Protection estándar](../../virtual-network/manage-ddos-protection.md) |
| Power BI dedicado | [Registro de diagnóstico para Power BI Embedded en Azure](https://docs.microsoft.com/power-bi/developer/azure-pbie-diag-logs) |
| Recovery Services | [Modelo de datos para Azure Backup](../../backup/backup-azure-reports-data-model.md)|
| Search |[Habilitación y uso de Análisis de tráfico de búsqueda](../../search/search-traffic-analytics.md) |
| Azure Service Bus |[Registros de diagnóstico de Azure Service Bus](../../service-bus-messaging/service-bus-diagnostic-logs.md) |
| SQL Database | [Registro de diagnóstico de Azure SQL Database](../../sql-database/sql-database-metrics-diag-logging.md) |
| Stream Analytics |[Registros de diagnósticos de trabajos](../../stream-analytics/stream-analytics-job-diagnostic-logs.md) |
| Traffic Manager | [Esquema de registro de Traffic Manager](../../traffic-manager/traffic-manager-diagnostic-logs.md) |
| Virtual Networks | Esquema no disponible. |
| Puertas de enlace de red virtual | Esquema no disponible. |

## <a name="next-steps"></a>Pasos siguientes

* [Más información sobre otros registros de plataforma de Azure](platform-logs-overview.md) que puede usar para supervisar Azure.
