---
title: Registros de auditoría en Azure Database for MariaDB
description: Se describen los registros de auditoría disponibles en Azure Database for MariaDB y los parámetros disponibles para habilitar los niveles de registro.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 06/26/2019
ms.openlocfilehash: 13ea60c62283db35ce4bf9fde6c3b36ba7f88013
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/28/2019
ms.locfileid: "67439221"
---
# <a name="audit-logs-in-azure-database-for-mariadb"></a>Registros de auditoría en Azure Database for MariaDB

Los usuarios tienen disponible el registro de auditoría en Azure Database for MariaDB. El registro de auditoría se puede usar para realizar un seguimiento de la actividad a nivel de la base de datos y se usa normalmente para cumplimiento.

> [!IMPORTANT]
> La funcionalidad del registro de auditoría se encuentra actualmente en versión preliminar.

## <a name="configure-audit-logging"></a>Configurar el registro de auditoría

De forma predeterminada, el registro de auditoría está deshabilitado. Para habilitarlo, cambie `audit_log_enabled` a Activado.

Otros parámetros que se pueden ajustar son los siguientes:

- `audit_log_events`: controla los eventos que se registrarán. Consulte la tabla a continuación para los eventos de auditoría específicos.
- `audit_log_exclude_users`: Los usuarios de MariaDB que se excluirán del registro. Permite a lo sumo cuatro usuarios. La longitud máxima del parámetro es de 256 caracteres.

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

## <a name="access-audit-logs"></a>Acceso a registros de auditoría

Los registros de auditoría están integrados en los registros de diagnóstico de Azure Monitor. Una vez que haya habilitado los registros de auditoría en el servidor de MariaDB, puede emitirlos a los registros de Azure Monitor, Event Hubs o Azure Storage. Para más información sobre cómo habilitar los registros de diagnóstico en Azure Portal, consulte el [artículo sobre registros de auditoría en el portal](howto-configure-audit-logs-portal.md#set-up-diagnostic-logs).

## <a name="diagnostic-logs-schemas"></a>Esquemas de registros de diagnóstico

En las secciones siguientes se describe lo que generan los registros de auditoría de MariaDB en función del tipo de evento. En función del método de salida que elija, pueden variar los campos incluidos y el orden en el que aparecen.

### <a name="connection"></a>Conexión

| **Propiedad** | **Descripción** |
|---|---|
| `TenantId` | El identificador de inquilino |
| `SourceSystem` | `Azure` |
| `TimeGenerated [UTC]` | Marca de tiempo de cuando se grabó el registro en UTC |
| `Type` | Tipo del registro. Siempre `AzureDiagnostics` |
| `SubscriptionId` | GUID de la suscripción a la que pertenece el servidor |
| `ResourceGroup` | Nombre del grupo de recursos al que pertenece el servidor |
| `ResourceProvider` | Nombre del proveedor de recursos Siempre `MICROSOFT.DBFORMARIADB` |
| `ResourceType` | `Servers` |
| `ResourceId` | URI de recurso |
| `Resource` | Nombre del servidor |
| `Category` | `MySqlAuditLogs` |
| `OperationName` | `LogEvent` |
| `event_class_s` | `connection_log` |
| `event_subclass_s` | `CONNECT`, `DISCONNECT` |
| `connection_id_d` | Identificador único de conexión generado por MariaDB |
| `host_s` | En blanco |
| `ip_s` | Dirección IP del cliente que se conecta a MariaDB |
| `user_s` | Nombre de usuario que ejecuta la consulta |
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
| `ResourceProvider` | Nombre del proveedor de recursos Siempre `MICROSOFT.DBFORMARIADB` |
| `ResourceType` | `Servers` |
| `ResourceId` | URI de recurso |
| `Resource` | Nombre del servidor |
| `Category` | `MySqlAuditLogs` |
| `OperationName` | `LogEvent` |
| `LogicalServerName_s` | Nombre del servidor |
| `event_class_s` | `general_log` |
| `event_subclass_s` | `LOG`, `ERROR`, `RESULT` |
| `event_time` | Segundos del inicio de la consulta en la marca de tiempo de UNIX |
| `error_code_d` | Código de error si la consulta no ha sido correcta. `0` significa que no hay error |
| `thread_id_d` | Id. del subproceso que ejecutó la consulta |
| `host_s` | En blanco |
| `ip_s` | Dirección IP del cliente que se conecta a MariaDB |
| `user_s` | Nombre de usuario que ejecuta la consulta |
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
| `ResourceProvider` | Nombre del proveedor de recursos Siempre `MICROSOFT.DBFORMARIADB` |
| `ResourceType` | `Servers` |
| `ResourceId` | URI de recurso |
| `Resource` | Nombre del servidor |
| `Category` | `MySqlAuditLogs` |
| `OperationName` | `LogEvent` |
| `LogicalServerName_s` | Nombre del servidor |
| `event_class_s` | `table_access_log` |
| `event_subclass_s` | `READ`, `INSERT`, `UPDATE` o `DELETE` |
| `connection_id_d` | Identificador único de conexión generado por MariaDB |
| `db_s` | Nombre de la base de datos a la que se accede |
| `table_s` | Nombre de la tabla a la que se accede |
| `sql_text_s` | Texto de la consulta completa |
| `\_ResourceId` | URI de recurso |

## <a name="next-steps"></a>Pasos siguientes

- [Procedimientos para configurar los registros de auditoría en Azure Portal](howto-configure-audit-logs-portal.md)