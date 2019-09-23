---
title: Registros de servidor en Azure Database for PostgreSQL con un único servidor
description: En este artículo se describe cómo Azure Database for PostgreSQL con un único servidor genera registros de consultas y de errores, y cómo se configura la retención de registros.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 5/6/2019
ms.openlocfilehash: 4d1cf2c59e324cedd9b747b1ac65d6edcb9deb45
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "65067381"
---
# <a name="server-logs-in-azure-database-for-postgresql---single-server"></a>Registros de servidor en Azure Database for PostgreSQL con un único servidor
Azure Database for PostgreSQL genera registros de errores y consultas. Los registros de consulta y errores se pueden usar para identificar, solucionar y reparar errores de configuración y casos de rendimiento no óptimo. (No se incluye acceso a los registros de transacciones). 

## <a name="configure-logging"></a>registro 
Puede configurar el registro en el servidor mediante los parámetros del servidor de registro. En cada nuevo servidor, **log_checkpoints** y **log_connections** están activados de forma predeterminada. Existen parámetros adicionales que puede ajustar según sus necesidades de registro: 

![Azure Database for PostgreSQL: parámetros de registro](./media/concepts-server-logs/log-parameters.png)

Para más información sobre estos parámetros, consulte la documentación de [informes y registro de errores](https://www.postgresql.org/docs/current/static/runtime-config-logging.html) de PostgreSQL. Para aprender a configurar los parámetros de Azure Database for PostgreSQL, consulte la [documentación del portal](howto-configure-server-parameters-using-portal.md) o la [documentación de la CLI](howto-configure-server-parameters-using-cli.md).

## <a name="access-server-logs-through-portal-or-cli"></a>Acceso a los registros del servidor mediante el portal o la CLI
Si ha habilitado los registros, puede acceder a ellos desde el almacenamiento de registros de Azure Database for PostgreSQL mediante [Azure Portal](howto-configure-server-logs-in-portal.md), la [CLI de Azure](howto-configure-server-logs-using-cli.md) y las API REST de Azure. Los archivos de registro rotan cada hora o cada 100 MB de tamaño, lo que ocurra primero. Puede establecer el período de retención de este almacenamiento de registro mediante el parámetro  **log\_retention\_period**  asociado al servidor. El valor predeterminado es 3 días y el valor máximo 7 días. El servidor debe tener suficiente almacenamiento asignado para albergar los archivos de registro. (Este parámetro de retención no controla los registros de diagnóstico de Azure).


## <a name="diagnostic-logs"></a>Registros de diagnóstico
Azure Database for PostgreSQL se integra con los registros de diagnóstico de Azure Monitor. Después de habilitar los registros en el servidor PostgreSQL, puede optar por hacer que se emitan a los [registros de Azure Monitor](../azure-monitor/log-query/log-query-overview.md), Event Hubs o Azure Storage. Para más información sobre cómo habilitar los registros de diagnóstico, consulte la sección de procedimientos de la [documentación de registros de diagnóstico](../azure-monitor/platform/diagnostic-logs-overview.md). 

> [!IMPORTANT]
> Esta función de diagnóstico para registros de servidor solo está disponibles en los [planes de tarifa](concepts-pricing-tiers.md) de uso general y optimizados para memoria.

En la tabla siguiente se describe lo que contiene cada registro. En función del punto de conexión de salida que elija, pueden variar los campos incluidos y el orden en el que aparecen. 

|**Campo** | **Descripción** |
|---|---|
| TenantId | El identificador de inquilino |
| SourceSystem | `Azure` |
| TimeGenerated [UTC] | Marca de tiempo de cuando se grabó el registro en UTC |
| Type | Tipo del registro. Siempre `AzureDiagnostics` |
| SubscriptionId | GUID de la suscripción a la que pertenece el servidor |
| ResourceGroup | Nombre del grupo de recursos al que pertenece el servidor |
| ResourceProvider | Nombre del proveedor de recursos Siempre `MICROSOFT.DBFORPOSTGRESQL` |
| ResourceType | `Servers` |
| ResourceId | URI de recurso |
| Resource | Nombre del servidor |
| Category | `PostgreSQLLogs` |
| OperationName | `LogEvent` |
| errorLevel | Ejemplo de nivel de registro: LOG, ERROR, NOTICE |
| Message | Mensaje de registro principal | 
| Domain | Versión del servidor, ejemplo: postgres-10 |
| Detail | Mensaje de registro secundario (si procede) |
| ColumnName | Nombre de la columna (si procede) |
| SchemaName | Nombre del esquema (si procede) |
| DatatypeName | Nombre del tipo de datos (si procede) |
| LogicalServerName | Nombre del servidor | 
| _ResourceId | URI de recurso |

## <a name="next-steps"></a>Pasos siguientes
- Más información sobre el acceso a los registros desde [Azure Portal](howto-configure-server-logs-in-portal.md) o la [CLI de Azure](howto-configure-server-logs-using-cli.md).
- Más información sobre los [precios de Azure Monitor](https://azure.microsoft.com/pricing/details/monitor/).
