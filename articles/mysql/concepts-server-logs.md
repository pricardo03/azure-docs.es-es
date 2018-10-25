---
title: Registros de servidor en Azure Database for MySQL
description: Describe los registros disponibles en Azure Database for MySQL y los parámetros disponibles para habilitar diferentes niveles de registro.
services: mysql
author: rachel-msft
ms.author: raagyema
manager: kfile
editor: jasonwhowell
ms.service: mysql
ms.topic: article
ms.date: 10/03/2018
ms.openlocfilehash: 73be0e4ecff4bc0d9b69249430bba69a93cc54ae
ms.sourcegitcommit: 4047b262cf2a1441a7ae82f8ac7a80ec148c40c4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/11/2018
ms.locfileid: "49093789"
---
# <a name="server-logs-in-azure-database-for-mysql"></a>Registros de servidor en Azure Database for MySQL
En Azure Database for MySQL, el registro de consultas lentas está disponible para los usuarios. No se admite el acceso al registro de transacciones. El registro de consultas lentas puede utilizarse para identificar cuellos de botella que afectan al rendimiento a fin de solucionar el problema. 

Para obtener más información sobre el registro de consultas lentas de MySQL, vea la [sección de registro de consultas lentas](https://dev.mysql.com/doc/refman/5.7/en/slow-query-log.html) del manual de referencia.

## <a name="access-server-logs"></a>Acceso a registros de servidor
Puede enumerar y descargar los registros de servidor de Azure Database for MySQL mediante Azure Portal y la CLI de Azure.

En Azure Portal, seleccione el servidor de Azure Database for MySQL. En el encabezado **Supervisión**, seleccione la página **Registros de servidor**.

Para obtener más información sobre la CLI de Azure, consulte [Configuración y acceso a los registros del servidor con la CLI de Azure](howto-configure-server-logs-in-cli.md).

## <a name="log-retention"></a>Retención de registros
Los registros están disponibles hasta siete días después de su creación. Si el tamaño total de los registros disponibles supera los 7 GB, se eliminan los archivos más antiguos hasta que haya espacio disponible. 

Los registros se rotan cada 24 horas o 7 GB, lo que ocurra primero.


## <a name="configure-logging"></a>registro 
De forma predeterminada, el registro de consultas lentas está deshabilitado. Para habilitarlo, establezca low_query_log en ON.

Otros parámetros que se pueden ajustar son los siguientes:

- **long_query_time**: si una consulta tarda más que long_query_time (en segundos), esa consulta se registra. El valor predeterminado es 10 segundos.
- **log_slow_admin_statements**: si ON incluye instrucciones administrativas como ALTER_TABLE y ANALYZE_TABLE en las instrucciones escritas en slow_query_log.
- **log_queries_not_using_indexes**: determina si las consultas que no utilizan índices se registran en slow_query_log.
- **log_throttle_queries_not_using_indexes**: este parámetro limita el número de consultas no de índice que se pueden escribir en el registro de consultas lentas. Este parámetro surte efecto cuando log_queries_not_using_indexes está configurado en ON.

Consulte la [documentación rel registro de consultas lentas](https://dev.mysql.com/doc/refman/5.7/en/slow-query-log.html) de MySQL para obtener una descripción completa de los parámetros de registro de consultas lentas.

## <a name="diagnostic-logs"></a>Registros de diagnóstico
Azure Database for MySQL se integra con los registros de diagnóstico de Azure Monitor. Después de habilitar los registros de consultas lentas en el servidor MySQL, puede optar por hacer que se emitan a Log Analytics, Event Hubs o Azure Storage. Para más información sobre cómo habilitar los registros de diagnóstico, consulte la sección de la [documentación de registros de diagnóstico](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md).

En la tabla siguiente se describe lo que contiene cada registro. En función del método de salida que elija, pueden variar los campos incluidos y el orden en el que aparecen.

| **Propiedad** | **Descripción** |
|---|---|---|
| TenantId | Id. del inquilino |
| SourceSystem | `Azure` |
| TimeGenerated [UTC] | Marca de tiempo de cuando se grabó el registro en UTC |
| Escriba | Tipo del registro. Siempre `AzureDiagnostics` |
| SubscriptionId | GUID de la suscripción a la que pertenece el servidor |
| ResourceGroup | Nombre del grupo de recursos al que pertenece el servidor |
| ResourceProvider | Nombre del proveedor de recursos Siempre `MICROSOFT.DBFORMYSQL` |
| ResourceType | `Servers` |
| ResourceId | URI de recurso |
| Recurso | Nombre del servidor |
| Categoría | `MySqlSlowLogs` |
| nombreOperación | `LogEvent` |
| Logical_server_name_s | Nombre del servidor |
| start_time_t [UTC] | Hora de inicio de la consulta |
| query_time_s | Tiempo total que tardó en ejecutarse la consulta |
| lock_time_s | Tiempo total durante el que se bloqueó la consulta |
| user_host_s | Nombre de usuario |
| rows_sent_s | Número de filas enviadas |
| rows_examined_s | Número de filas examinadas |
| last_insert_id_s | [last_insert_id](https://dev.mysql.com/doc/refman/8.0/en/information-functions.html#function_last-insert-id) |
| insert_id_s | Id. de inserción |
| sql_text_s | Consulta completa |
| server_id_s | Id. del servidor |
| thread_id_s | Id. del subproceso |
| \_ResourceId | URI de recurso |

## <a name="next-steps"></a>Pasos siguientes
- [How to configure and access server logs from the Azure CLI](howto-configure-server-logs-in-cli.md) (Configuración y acceso de registros de servidor desde la CLI de Azure).
