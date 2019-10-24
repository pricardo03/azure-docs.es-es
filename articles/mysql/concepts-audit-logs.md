---
title: Registros de auditoría para Azure Database for MySQL
description: Se describen los registros de auditoría disponibles en Azure Database for MySQL y los parámetros disponibles para habilitar los niveles de registro.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 06/26/2019
ms.openlocfilehash: 42881fcb12f29ec14bbdc0ec4942b2eef17c7312
ms.sourcegitcommit: 77bfc067c8cdc856f0ee4bfde9f84437c73a6141
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/16/2019
ms.locfileid: "72434404"
---
# <a name="audit-logs-in-azure-database-for-mysql"></a>Registros de auditoría en Azure Database for MySQL

Los usuarios tienen disponible el registro de auditoría en Azure Database for MySQL. El registro de auditoría se puede usar para realizar un seguimiento de la actividad a nivel de la base de datos y se usa normalmente para cumplimiento.

> [!IMPORTANT]
> La funcionalidad del registro de auditoría se encuentra actualmente en versión preliminar.

## <a name="configure-audit-logging"></a>Configuración del registro de auditoría

De forma predeterminada, el registro de auditoría está deshabilitado. Para habilitarlo, cambie `audit_log_enabled` a Activado.

Otros parámetros que se pueden ajustar son los siguientes:

- `audit_log_events`: controla los eventos que se registrarán. Consulte la tabla a continuación para ver los eventos de auditoría específicos.
- `audit_log_include_users`: Usuarios de MySQL que se incluirán para el registro. El valor predeterminado de este parámetro es estar vacío, lo que incluirá todos los usuarios en el registro. Este tiene una mayor prioridad que `audit_log_exclude_users`. La longitud máxima del parámetro es de 512 caracteres.
> [!Note]
> `audit_log_include_users` tiene mayor prioridad que `audit_log_exclude_users`, por ejemplo, si audit_log_include_users = `demouser` y audit_log_exclude_users = `demouser`, auditará los registros porque `audit_log_include_users` tiene mayor prioridad.
- `audit_log_exclude_users`: Los usuarios de MySQL que se excluirán del registro. La longitud máxima del parámetro es de 512 caracteres.

> [!Note]
> Para `sql_text`, el registro se truncará si supera los 2048 caracteres.

| **Evento** | **Descripción** |
|---|---|
| `CONNECTION` | - Iniciación de la conexión (correcta o incorrecta) <br> - Reautenticación de usuario con usuario/contraseña diferente durante la sesión <br> - Terminación de la conexión |
| `DML_SELECT`| Consultas SELECT |
| `DML_NONSELECT` | Consultas INSERT/DELETE/UPDATE |
| `DML` | DML = DML_SELECT + DML_NONSELECT |
| `DDL` | Consultas como "DROP DATABASE" |
| `DCL` | Consultas como "GRANT PERMISSION" |
| `ADMIN` | Consultas como "SHOW STATUS" |
| `GENERAL` | Todos en DML_SELECT, DML_NONSELECT, DML, DDL, DCL y ADMIN |
| `TABLE_ACCESS` | - Solo está disponible para MySQL 5.7 <br> - Instrucciones de lectura de tablas como SELECT o INSERT INTO... SELECT <br> - Instrucciones de eliminación de tablas, como DELETE o TRUNCATE TABLE <br> - Instrucciones de inserción en tablas, como INSERT o REPLACE <br> - Instrucciones de actualización de tablas, como UPDATE |

## <a name="access-audit-logs"></a>Acceso a registros de auditoría

Los registro de auditoría están integrados en los registros de diagnóstico de Azure Monitor. Una vez que haya habilitado los registros de auditoría en el servidor de MySQL, puede enviarlos a los registros de Azure Monitor, Event Hubs o Azure Storage. Para obtener más información sobre cómo habilitar los registros de diagnóstico en Azure Portal, consulte el [artículo sobre registros de auditoría en el portal](howto-configure-audit-logs-portal.md#set-up-diagnostic-logs).

## <a name="diagnostic-logs-schemas"></a>Esquemas de registros de diagnóstico

En las secciones siguientes se describe lo que generan los registros de auditoría de MySQL en función del tipo de evento. En función del método de salida que elija, pueden variar los campos incluidos y el orden en el que aparecen.

### <a name="connection"></a>Conexión

| **Propiedad** | **Descripción** |
|---|---|
| `TenantId` | El identificador de inquilino |
| `SourceSystem` | `Azure` |
| `TimeGenerated [UTC]` | Marca de tiempo de cuando se grabó el registro en UTC |
| `Type` | Tipo del registro. Siempre `AzureDiagnostics` |
| `SubscriptionId` | GUID de la suscripción a la que pertenece el servidor |
| `ResourceGroup` | Nombre del grupo de recursos al que pertenece el servidor |
| `ResourceProvider` | Nombre del proveedor de recursos Siempre `MICROSOFT.DBFORMYSQL` |
| `ResourceType` | `Servers` |
| `ResourceId` | URI de recurso |
| `Resource` | Nombre del servidor |
| `Category` | `MySqlAuditLogs` |
| `OperationName` | `LogEvent` |
| `LogicalServerName_s` | Nombre del servidor |
| `event_class_s` | `connection_log` |
| `event_subclass_s` | `CONNECT`, `DISCONNECT`, `CHANGE USER` (solo disponible para MySQL 5.7) |
| `connection_id_d` | Identificador único de conexión generado por MySQL |
| `host_s` | En blanco |
| `ip_s` | Dirección IP del cliente que se conecta a MySQL |
| `user_s` | Nombre del usuario que ejecuta la consulta |
| `db_s` | Nombre de la base de datos a la que se conecta |
| `\_ResourceId` | URI de recurso |

### <a name="general"></a>General

El esquema siguiente se aplica a los tipos de evento GENERAL, DML_SELECT, DML_NONSELECT, DML, DDL, DCL y ADMIN.

| **Propiedad** | **Descripción** |
|---|---|
| `TenantId` | El identificador de inquilino |
| `SourceSystem` | `Azure` |
| `TimeGenerated [UTC]` | Marca de tiempo de cuando se grabó el registro en UTC |
| `Type` | Tipo del registro. Siempre `AzureDiagnostics` |
| `SubscriptionId` | GUID de la suscripción a la que pertenece el servidor |
| `ResourceGroup` | Nombre del grupo de recursos al que pertenece el servidor |
| `ResourceProvider` | Nombre del proveedor de recursos Siempre `MICROSOFT.DBFORMYSQL` |
| `ResourceType` | `Servers` |
| `ResourceId` | URI de recurso |
| `Resource` | Nombre del servidor |
| `Category` | `MySqlAuditLogs` |
| `OperationName` | `LogEvent` |
| `LogicalServerName_s` | Nombre del servidor |
| `event_class_s` | `general_log` |
| `event_subclass_s` | `LOG`, `ERROR`, `RESULT` (solo disponible para MySQL 5.6) |
| `event_time` | Hora de inicio de la consulta en marca de tiempo UTC |
| `error_code_d` | Código de error si la consulta no es correcta. `0` significa que no hay error |
| `thread_id_d` | Id. del subproceso que ejecutó la consulta |
| `host_s` | En blanco |
| `ip_s` | Dirección IP del cliente que se conecta a MySQL |
| `user_s` | Nombre del usuario que ejecuta la consulta |
| `sql_text_s` | Texto de la consulta completa |
| `\_ResourceId` | URI de recurso |

### <a name="table-access"></a>Acceso a la tabla

| **Propiedad** | **Descripción** |
|---|---|
| `TenantId` | El identificador de inquilino |
| `SourceSystem` | `Azure` |
| `TimeGenerated [UTC]` | Marca de tiempo de cuando se grabó el registro en UTC |
| `Type` | Tipo del registro. Siempre `AzureDiagnostics` |
| `SubscriptionId` | GUID de la suscripción a la que pertenece el servidor |
| `ResourceGroup` | Nombre del grupo de recursos al que pertenece el servidor |
| `ResourceProvider` | Nombre del proveedor de recursos Siempre `MICROSOFT.DBFORMYSQL` |
| `ResourceType` | `Servers` |
| `ResourceId` | URI de recurso |
| `Resource` | Nombre del servidor |
| `Category` | `MySqlAuditLogs` |
| `OperationName` | `LogEvent` |
| `LogicalServerName_s` | Nombre del servidor |
| `event_class_s` | `table_access_log` |
| `event_subclass_s` | `READ`, `INSERT`, `UPDATE` o `DELETE` |
| `connection_id_d` | Identificador único de conexión generado por MySQL |
| `db_s` | Nombre de la base de datos a la que se accede |
| `table_s` | Nombre de la tabla a la que se accede |
| `sql_text_s` | Texto de la consulta completa |
| `\_ResourceId` | URI de recurso |

## <a name="next-steps"></a>Pasos siguientes

- [Procedimientos para configurar los registros de auditoría en Azure Portal](howto-configure-audit-logs-portal.md)