---
title: Formato de registro de auditoría de base de datos SQL | Microsoft Docs
description: Comprender cómo se estructuran los registros de auditoría de base de datos SQL.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: vainolo
ms.author: arib
ms.reviewer: vanto
manager: craigg
ms.date: 01/03/2019
ms.openlocfilehash: 0fefe01e413e30e4aa3c1fa90de77cbdece39c38
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/18/2019
ms.locfileid: "58001694"
---
# <a name="sql-database-audit-log-format"></a>Formato de registro de auditoría de base de datos SQL

[La auditoría de base de datos de SQL Azure](sql-database-auditing.md) realiza el seguimiento de eventos de base de datos y escribe en una auditoría de registro en la cuenta de almacenamiento de Azure o envía al centro de eventos o Log Analytics para análisis y procesamiento de bajada.

## <a name="naming-conventions"></a>Convenciones de nomenclatura

### <a name="blob-audit"></a>Auditoría de blobs

Los registros de auditoría almacenados en Blob storage se almacenan en un contenedor denominado `sqldbauditlogs` en la cuenta de almacenamiento de Azure. La jerarquía de directorios dentro del contenedor es el formato `<ServerName>/<DatabaseName>/<AuditName>/<Date>/`. El formato de nombre de archivo de Blob es `<CreationTime>_<FileNumberInSession>.xel`, donde `CreationTime` está en formato UTC `hh_mm_ss_ms` formato, y `FileNumberInSession` es un ejecución de índice en caso de intervalos de registros de sesión a través de varios archivos de Blob.

Por ejemplo, para la base de datos `Database1` en `Server1` lo siguiente es una ruta válida posibles:

    Server1/Database1/SqlDbAuditing_ServerAudit_NoRetention/2019-02-03/12_23_30_794_0.xel

### <a name="event-hub"></a>Centro de eventos

Eventos de auditoría se escriben en el espacio de nombres y centro de eventos que se definió durante la configuración de auditoría y se captura en el cuerpo de [Apache Avro](https://avro.apache.org/) eventos y se almacenan con el formato JSON con codificación UTF-8. Para leer los registros de auditoría, puede usar [Avro Tools](https://docs.microsoft.com/azure/event-hubs/event-hubs-capture-overview#use-avro-tools) o herramientas similares que procesen este formato.

### <a name="log-analytics"></a>Log Analytics

Eventos de auditoría se escriben en el área de trabajo de Log Analytics definida durante la configuración de auditoría, en el `AzureDiagnostics` tabla con la categoría `SQLSecurityAuditEvents`. Para información útil adicional sobre los comandos y el lenguaje de búsqueda de Log Analytics, consulte la [referencia de búsqueda de Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-log-search).

## <a id="subheading-1"></a>Campos de registro de auditoría

| Nombre (Blob) | Nombre (Event Hubs o Log Analytics) | DESCRIPCIÓN | Tipo de blob | Tipo de evento Hubs o Log Analytics |
|-------------|---------------------------------|-------------|-----------|-------------------------------|
| action_id | action_id_s | Identificador de la acción | varchar(4) | string |
| action_name | action_name_s | Nombre de la acción | N/D | string |
| additional_information | additional_information_s | Información adicional sobre el evento, almacenada como XML | nvarchar(4000) | string |
| affected_rows | affected_rows_d | Número de filas afectadas por la consulta | bigint | int |
| application_name | application_name_s| Nombre de aplicación cliente | nvarchar(128) | string |
| audit_schema_version | audit_schema_version_d | Siempre es 1 | int | int |
| class_type | class_type_s | Tipo de entidad auditable en la que se produce la auditoría | varchar(2) | string |
| class_type_desc | class_type_description_s | Descripción de entidad auditable en la que se produce la auditoría | N/D | string |
| client_ip | client_ip_s | IP de la aplicación cliente de origen | nvarchar(128) | string |
| connection_id | N/D | Identificador de la conexión en el servidor | GUID | N/D |
| data_sensitivity_information | data_sensitivity_information_s | Tipos de información y las etiquetas de confidencialidad devueltas por la consulta auditada, basándose en las columnas clasificadas en la base de datos. Obtenga más información sobre [clasificación y detección de datos de Azure SQL Database](sql-database-data-discovery-and-classification.md) | nvarchar(4000) | string |
| database_name | database_name_s | El contexto de base de datos en el que se produjo la acción | sysname | string |
| database_principal_id | database_principal_id_d | Id. del contexto del usuario de base de datos que se realiza la acción | int | int |
| database_principal_name | database_principal_name_s | Nombre del contexto del usuario de base de datos en el que se realiza la acción | sysname | string |
| duration_milliseconds | duration_milliseconds_d | Duración de la ejecución de consulta en milisegundos | bigint | int |
| event_time | event_time_t | Fecha y hora cuando se desencadena la acción auditable | datetime2 | Datetime |
| host_name | N/D | Nombre de host del cliente | string | N/D |
| is_column_permission | is_column_permission_s | Marca que indica si se trata de un permiso de nivel de columna. 1 = true, 0 = false | bit | string |
| N/D | is_server_level_audit_s | Marca que indica si esta auditoría está en el nivel de servidor | N/D | string |
| object_ id | object_id_d | El identificador de la entidad en el que se produjo la auditoría. Esto incluye el: objetos de servidor, las bases de datos, objetos de base de datos y objetos de esquema. 0 si la entidad es el propio servidor o si la auditoría no se realiza en un nivel de objeto | int | int |
| object_name | object_name_s | El nombre de la entidad en el que se produjo la auditoría. Esto incluye el: objetos de servidor, las bases de datos, objetos de base de datos y objetos de esquema. 0 si la entidad es el propio servidor o si la auditoría no se realiza en un nivel de objeto | sysname | string |
| permission_bitmask | permission_bitmask_s | Cuando sea aplicable, muestra los permisos que se han concedido, denegados, o revocado. | varbinary (16) | string |
| response_rows | response_rows_d | Número de filas devueltas en el conjunto de resultados | bigint | int |
| schema_name | schema_name_s | El contexto de esquema en el que se produjo la acción. NULL para las auditorías que se producen fuera de un esquema | sysname | string |
| N/D | securable_class_type_s | Objeto protegible que se asigna al valor class_type que se está auditando | N/D | string |
| sequence_group_id | sequence_group_id_g | Identificador único | varbinary | GUID |
| sequence_number | sequence_number_d | Realiza un seguimiento de la secuencia de registros dentro de un único registro de auditoría que era demasiado grande para caber en el búfer de escritura para las auditorías | int | int |
| server_instance_name | server_instance_name_s | Nombre de la instancia del servidor donde se produjo la auditoría | sysname | string |
| server_principal_id | server_principal_id_d | Identificador del contexto de inicio de sesión en el que se realiza la acción | int | int |
| server_principal_name | server_principal_name_s | Inicio de sesión actual | sysname | string |
| server_principal_sid | server_principal_sid_s | SID del inicio de sesión actual | varbinary | string |
| session_id | session_id_d | Identificador de la sesión en el que se produjo el evento | smallint | int |
| session_server_principal_name | session_server_principal_name_s | Entidad de seguridad de servidor para la sesión | sysname | string |
| instrucción | statement_s | Instrucción de Transact-SQL que se ha ejecutado (si existe) | nvarchar(4000) | string |
| correcto | succeeded_s | Indica si la acción que desencadenó el evento se realizó correctamente. Para los eventos que no sean de inicio de sesión y por lotes, esto solo notifica si la comprobación del permiso se ha realizado correctamente o no, no a la operación. 1 = correcto, 0 = error | bit | string |
| target_database_principal_id | target_database_principal_id_d | La entidad de seguridad de base de datos se realiza la operación GRANT/DENY/REVOKE en. 0 si no es aplicable | int | int |
| target_database_principal_name | target_database_principal_name_s | Usuario de destino de la acción. NULL si no es aplicable | string | string |
| target_server_principal_id | target_server_principal_id_d | Entidad de seguridad de servidor que se realiza la operación GRANT/DENY/REVOKE en. Devuelve 0 si no es aplicable | int | int |
| target_server_principal_name | target_server_principal_name_s | Inicio de sesión de destino de acción. NULL si no es aplicable | sysname | string |
| target_server_principal_sid | target_server_principal_sid_s | SID del inicio de sesión de destino. NULL si no es aplicable | varbinary | string |
| transaction_id | transaction_id_d | Sólo servidor SQL Server (a partir de 2016) - 0 para Azure SQL DB | bigint | int |
| user_defined_event_id | user_defined_event_id_d | Id. de evento que se pasa como argumento a sp_audit_write definido por el usuario. NULL para los eventos del sistema (valor predeterminado) y distinto de cero para el evento definido por el usuario. Para obtener más información, consulte [sp_audit_write (Transact-SQL)](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-audit-write-transact-sql) | smallint | int |
| user_defined_information | user_defined_information_s | Información que se pasa como argumento a sp_audit_write definido por el usuario. NULL para los eventos del sistema (valor predeterminado) y distinto de cero para el evento definido por el usuario. Para obtener más información, consulte [sp_audit_write (Transact-SQL)](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-audit-write-transact-sql) | nvarchar(4000) | string |

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre [auditoría de Azure SQL Database](sql-database-auditing.md).