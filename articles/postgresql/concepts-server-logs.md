---
title: Registros de servidor en Azure Database for PostgreSQL con un único servidor
description: En este artículo se describe cómo Azure Database for PostgreSQL con un único servidor genera registros de consultas y de errores, y cómo se configura la retención de registros.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 09/18/2019
ms.openlocfilehash: 17083029f2377037b99abfa3ce8371661eccb957
ms.sourcegitcommit: 11265f4ff9f8e727a0cbf2af20a8057f5923ccda
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/08/2019
ms.locfileid: "72029994"
---
# <a name="server-logs-in-azure-database-for-postgresql---single-server"></a>Registros de servidor en Azure Database for PostgreSQL con un único servidor
Azure Database for PostgreSQL genera registros de errores y consultas. Los registros de consulta y errores se pueden usar para identificar, solucionar y reparar errores de configuración y casos de rendimiento no óptimo. (No se incluye acceso a los registros de transacciones). 

## <a name="configure-logging"></a>registro 
Puede configurar el registro en el servidor mediante los parámetros del servidor de registro. En cada nuevo servidor, **log_checkpoints** y **log_connections** están activados de forma predeterminada. Existen parámetros adicionales que puede ajustar según sus necesidades de registro: 

![Azure Database for PostgreSQL: parámetros de registro](./media/concepts-server-logs/log-parameters.png)

Para más información sobre estos parámetros, consulte la documentación de [informes y registro de errores](https://www.postgresql.org/docs/current/static/runtime-config-logging.html) de PostgreSQL. Para aprender a configurar los parámetros de Azure Database for PostgreSQL, consulte la [documentación del portal](howto-configure-server-parameters-using-portal.md) o la [documentación de la CLI](howto-configure-server-parameters-using-cli.md).

## <a name="access-server-logs-through-portal-or-cli"></a>Acceso a los registros del servidor mediante el portal o la CLI
Si ha habilitado los registros, puede acceder a ellos desde el almacenamiento de registros de Azure Database for PostgreSQL mediante [Azure Portal](howto-configure-server-logs-in-portal.md), la [CLI de Azure](howto-configure-server-logs-using-cli.md) y las API REST de Azure. Los archivos de registro rotan cada hora o cada 100 MB de tamaño, lo que ocurra primero. Puede establecer el período de retención de este almacenamiento de registros mediante el parámetro **log\_retention\_period** asociado al servidor. El valor predeterminado es 3 días y el valor máximo 7 días. El servidor debe tener suficiente almacenamiento asignado para albergar los archivos de registro. (Este parámetro de retención no controla los registros de diagnóstico de Azure).


## <a name="diagnostic-logs"></a>Registros de diagnóstico
Azure Database for PostgreSQL se integra con los registros de diagnóstico de Azure Monitor. Después de habilitar los registros en el servidor PostgreSQL, puede optar por hacer que se emitan a los [registros de Azure Monitor](../azure-monitor/log-query/log-query-overview.md), Event Hubs o Azure Storage. 

> [!IMPORTANT]
> Esta característica de diagnóstico para registros de servidor solo está disponible en los [planes de tarifa](concepts-pricing-tiers.md) de uso general y optimizados para memoria.

Para habilitar los registro de diagnóstico desde Azure Portal:

   1. En el portal, vaya a *Configuración de diagnóstico*  en el menú de navegación del servidor de Postgres.
   2. Seleccione *Agregar configuración de diagnóstico*.
   3. Asigne un nombre a esta configuración. 
   4. Seleccione su ubicación de nivel inferior preferida (cuenta de almacenamiento, centro de eventos o análisis de registros). 
   5. Seleccione los tipos de datos que desee.
   6. Guarde la configuración.

En la tabla siguiente se describe lo que contiene cada registro. En función del punto de conexión de salida que elija, pueden variar los campos incluidos y el orden en el que aparecen. 

|**Campo** | **Descripción** |
|---|---|
| TenantId | El identificador de inquilino |
| SourceSystem | `Azure` |
| TimeGenerated [UTC] | Marca de tiempo de cuando se grabó el registro en UTC |
| type | Tipo del registro. Siempre `AzureDiagnostics` |
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
| Prefijo | Prefijo de la línea de registro |



## <a name="next-steps"></a>Pasos siguientes
- Más información sobre el acceso a los registros desde [Azure Portal](howto-configure-server-logs-in-portal.md) o la [CLI de Azure](howto-configure-server-logs-using-cli.md).
- Más información sobre los [precios de Azure Monitor](https://azure.microsoft.com/pricing/details/monitor/).
