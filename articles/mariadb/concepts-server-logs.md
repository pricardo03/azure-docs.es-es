---
title: Registros de servidor en Azure Database for MariaDB
description: Se describen los registros disponibles en Azure Database for MariaDB y los parámetros disponibles para habilitar diferentes niveles de registro.
author: rachel-msft
ms.author: raagyema
ms.service: mariadb
ms.topic: conceptual
ms.date: 06/12/2019
ms.openlocfilehash: 10dbd4d7fa838ee7f8a3f70b3caadb570877d685
ms.sourcegitcommit: 55f7fc8fe5f6d874d5e886cb014e2070f49f3b94
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/25/2019
ms.locfileid: "71259964"
---
# <a name="slow-query-logs-in-azure-database-for-mariadb"></a>Registros de consultas lentas en Azure Database for MariaDB
En Azure Database for MariaDB, el registro de consultas lentas está disponible para los usuarios. No se admite el acceso al registro de transacciones. El registro de consultas lentas puede utilizarse para identificar cuellos de botella que afectan al rendimiento a fin de solucionar el problema.

Para más información acerca del registro de consultas lentas, consulte la [documentación de MariaDB](https://mariadb.com/kb/en/library/slow-query-log-overview/).

## <a name="access-slow-query-logs"></a>Acceso a registros de consultas lentas
Puede enumerar y descargar los registros de consultas lentas de Azure Database for MariaDB mediante Azure Portal y la CLI de Azure.

En Azure Portal, seleccione el servidor de Azure Database for MariaDB. En el encabezado **Supervisión**, seleccione la página **Registros de servidor**.

Para obtener más información sobre la CLI de Azure, consulte [Configuración y acceso a los registros del servidor con la CLI de Azure](howto-configure-server-logs-cli.md).

## <a name="log-retention"></a>Retención de registros
Los registros están disponibles hasta siete días después de su creación. Si el tamaño total de los registros disponibles supera los 7 GB, se eliminan los archivos más antiguos hasta que haya espacio disponible.

Los registros se rotan cada 24 horas o 7 GB, lo que ocurra primero.

## <a name="configure-slow-query-logging"></a>Configuración del registro de consultas lentas
De forma predeterminada, el registro de consultas lentas está deshabilitado. Para habilitarlo, establezca low_query_log en ON.

Otros parámetros que se pueden ajustar son los siguientes:

- **long_query_time**: si una consulta tarda más que long_query_time (en segundos), esa consulta se registra. El valor predeterminado es 10 segundos.
- **log_slow_admin_statements**: si ON incluye instrucciones administrativas como ALTER_TABLE y ANALYZE_TABLE en las instrucciones escritas en slow_query_log.
- **log_queries_not_using_indexes**: determina si las consultas que no utilizan índices se registran en slow_query_log.
- **log_throttle_queries_not_using_indexes**: este parámetro limita el número de consultas que no son de índice que se pueden escribir en el registro de consultas lentas. Este parámetro surte efecto cuando log_queries_not_using_indexes está configurado en ON.

Consulte la [documentación del registro de consultas lentas](https://mariadb.com/kb/en/library/slow-query-log-overview/) de MariaDB para obtener una descripción completa de los parámetros de registro de consultas lentas.

## <a name="diagnostic-logs"></a>Registros de diagnóstico
Azure Database for MariaDB se integra con los registros de diagnóstico de Azure Monitor. Después de habilitar los registros de consultas lentas en el servidor MariaDB, puede optar por hacer que se emitan a los registros de Azure Monitor, Event Hubs o Azure Storage. Para más información sobre cómo habilitar los registros de diagnóstico, consulte la sección de la [documentación de registros de diagnóstico](../azure-monitor/platform/resource-logs-overview.md).

> [!IMPORTANT]
> Esta característica de diagnóstico para registros de servidor solo está disponible en los [planes de tarifa](concepts-pricing-tiers.md) de uso general y optimizados para memoria.

En la tabla siguiente se describe lo que contiene cada registro. En función del método de salida que elija, pueden variar los campos incluidos y el orden en el que aparecen.

| **Propiedad** | **Descripción** |
|---|---|
| `TenantId` | El identificador de inquilino |
| `SourceSystem` | `Azure` |
| `TimeGenerated` [UTC] | Marca de tiempo de cuando se grabó el registro en UTC |
| `Type` | Tipo del registro. Siempre `AzureDiagnostics` |
| `SubscriptionId` | GUID de la suscripción a la que pertenece el servidor |
| `ResourceGroup` | Nombre del grupo de recursos al que pertenece el servidor |
| `ResourceProvider` | Nombre del proveedor de recursos Siempre `MICROSOFT.DBFORMARIADB` |
| `ResourceType` | `Servers` |
| `ResourceId` | URI de recurso |
| `Resource` | Nombre del servidor |
| `Category` | `MySqlSlowLogs` |
| `OperationName` | `LogEvent` |
| `Logical_server_name_s` | Nombre del servidor |
| `start_time_t` [UTC] | Hora de inicio de la consulta |
| `query_time_s` | Tiempo total que tardó en ejecutarse la consulta |
| `lock_time_s` | Tiempo total durante el que se bloqueó la consulta |
| `user_host_s` | Nombre de usuario |
| `rows_sent_s` | Número de filas enviadas |
| `rows_examined_s` | Número de filas examinadas |
| `last_insert_id_s` | [last_insert_id](https://mariadb.com/kb/en/library/last_insert_id/) |
| `insert_id_s` | Id. de inserción |
| `sql_text_s` | Consulta completa |
| `server_id_s` | Id. de servidor |
| `thread_id_s` | Id. de subproceso |
| `\_ResourceId` | URI de recurso |

## <a name="next-steps"></a>Pasos siguientes
- [Configuración y acceso a los registros de servidor en Azure Portal](howto-configure-server-logs-portal.md).
