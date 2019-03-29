---
title: 'Azure SQL Database: lectura de consultas en réplicas| Microsoft Docs'
description: Azure SQL Database ofrece la posibilidad de equilibrar la carga de las cargas de trabajo de solo lectura mediante la capacidad de las réplicas de solo lectura, lo que se conoce como escalado horizontal de lectura.
services: sql-database
ms.service: sql-database
ms.subservice: scale-out
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: anosov1960
ms.author: sashan
ms.reviewer: sstein, carlrab
manager: craigg
ms.date: 03/28/2019
ms.openlocfilehash: d9ad859ef24b51dc337dc23281d2fe4e1eada1e6
ms.sourcegitcommit: f8c592ebaad4a5fc45710dadc0e5c4480d122d6f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2019
ms.locfileid: "58619898"
---
# <a name="use-read-only-replicas-to-load-balance-read-only-query-workloads"></a>Usar réplicas de solo lectura para las cargas de trabajo de consulta de solo lectura de equilibrio de carga

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]
> [!IMPORTANT]
> El módulo de PowerShell de Azure Resource Manager es compatible aún con Azure SQL Database, pero todo el desarrollo futuro es para el módulo Az.Sql. Para estos cmdlets, consulte [AzureRM.Sql](https://docs.microsoft.com/powershell/module/AzureRM.Sql/). Los argumentos para los comandos en el módulo de Az y en los módulos AzureRm son esencialmente idénticos.

El **escalado horizontal de lectura** le permite equilibrar la carga de las cargas de trabajo de solo lectura de Azure SQL Database mediante la capacidad de una réplica de solo lectura.

Cada base de datos del nivel Premium ([modelo de compra basado en la unidad de transmisión de datos (DTU)](sql-database-service-tiers-dtu.md)) o del nivel Crítico para la empresa ([modelo de compra basado en núcleos virtuales](sql-database-service-tiers-vcore.md)) se aprovisiona automáticamente con varias réplicas AlwaysOn para permitir el Acuerdo de Nivel de Servicio de disponibilidad. Esto se ilustra en el diagrama siguiente

![Réplicas de solo lectura](media/sql-database-read-scale-out/business-critical-service-tier-read-scale-out.png)

Las réplicas secundarias se aprovisionan con el mismo tamaño de proceso que la réplica principal. La característica de **escalado horizontal de lectura** le permite equilibrar la carga de las cargas de trabajo de solo lectura de SQL Database gracias al uso de la capacidad de una de las réplicas de solo lectura en lugar de compartir réplicas de solo escritura. De este modo, la carga de trabajo de solo lectura se aísla de la carga de trabajo principal de lectura y escritura y no afecta a su rendimiento. La característica está destinada a las aplicaciones que incluyen cargas de trabajo de solo lectura separadas lógicamente, como los casos de análisis, y, por tanto, esta capacidad adicional podría suponer ventajas para el rendimiento sin costo adicional.

Para usar la característica de escalado horizontal de lectura con una base de datos determinada, debe habilitarla explícitamente al crear la base de datos o posteriormente modificando la configuración con PowerShell mediante la invocación del [conjunto AzSqlDatabase](/powershell/module/az.sql/set-azsqldatabase) o el [New AzSqlDatabase](/powershell/module/az.sql/new-azsqldatabase) cmdlets o a través de la REST API de Azure Resource Manager mediante la [bases de datos - crear o actualizar](https://docs.microsoft.com/rest/api/sql/databases/createorupdate) método.

Después de habilita el escalado horizontal de lectura para una base de datos, las aplicaciones se conectan a esa base de datos se dirigirá la puerta de enlace a la réplica de lectura y escritura o a una réplica de solo lectura de esa base de datos según la `ApplicationIntent` propiedad configurada en el cadena de conexión de la aplicación. Para más información sobre la propiedad `ApplicationIntent`, consulte [Specifying Application Intent](https://docs.microsoft.com/sql/relational-databases/native-client/features/sql-server-native-client-support-for-high-availability-disaster-recovery#specifying-application-intent) (Especificación de la intención de la aplicación).

Si está deshabilitado el escalado de lectura o establece la propiedad ReadScale en un nivel de servicio no admitido, todas las conexiones se dirigen a la réplica de lectura y escritura, con independencia de la propiedad `ApplicationIntent`.

> [!NOTE]
> Consulta de datos Store, Extended Events, SQL Profiler y las características de auditoría no se admiten en las réplicas de solo lectura. 
## <a name="data-consistency"></a>Coherencia de datos

Una de las ventajas de las réplicas es que siempre se encuentran en estado de coherencia transaccional, pero es posible que en diferentes momentos pueda haber una pequeña latencia entre las distintas réplicas. El escalado horizontal de lectura admite la coherencia en el nivel de sesión. Esto significa que, si la sesión de solo lectura se vuelve a conectar después de un error de conexión debido a la falta de disponibilidad de las réplicas, se puede redirigir a una réplica que no esté actualizada al 100 % con la réplica de lectura-escritura. Del mismo modo, si una aplicación escribe datos mediante una sesión de lectura y escritura y los lee inmediatamente mediante una sesión de solo lectura, es posible que las actualizaciones más recientes no son inmediatamente visibles en la réplica. La latencia se produce por una operación de rehacer de registro de transacción asincrónica.

> [!NOTE]
> Las latencias de replicación en la región son bajas y esta situación es poco frecuente.

## <a name="connect-to-a-read-only-replica"></a>Conexión a una réplica de solo lectura

Cuando se habilita el escalado horizontal de lectura en una base de datos, la opción `ApplicationIntent` de la cadena de conexión proporcionada por el cliente dictamina si la conexión se enruta a la réplica de lectura o a una réplica de solo lectura. En concreto, si el valor de `ApplicationIntent` es `ReadWrite` (el valor predeterminado), la conexión se dirigirá a la réplica de lectura-escritura de la base de datos. Este comportamiento es idéntico al existente. Si el valor de `ApplicationIntent` es `ReadOnly`, la conexión se enruta a una réplica de solo lectura.

Por ejemplo, la siguiente cadena de conexión conecta el cliente a una réplica de solo lectura (los elementos entre corchetes angulares se sustituyen por los valores correctos para su entorno y se quitan dichos corchetes):

```SQL
Server=tcp:<server>.database.windows.net;Database=<mydatabase>;ApplicationIntent=ReadOnly;User ID=<myLogin>;Password=<myPassword>;Trusted_Connection=False; Encrypt=True;
```

Cualquiera de las siguientes cadenas de conexión conecta el cliente a una réplica de lectura-escritura (los elementos entre corchetes angulares se sustituyen por los valores correctos para su entorno y se quitan dichos corchetes):

```SQL
Server=tcp:<server>.database.windows.net;Database=<mydatabase>;ApplicationIntent=ReadWrite;User ID=<myLogin>;Password=<myPassword>;Trusted_Connection=False; Encrypt=True;

Server=tcp:<server>.database.windows.net;Database=<mydatabase>;User ID=<myLogin>;Password=<myPassword>;Trusted_Connection=False; Encrypt=True;
```

## <a name="verify-that-a-connection-is-to-a-read-only-replica"></a>Verificación de que se trata de una conexión a una réplica de solo lectura

Puede comprobar si está conectado a una réplica de solo lectura mediante la ejecución de la consulta siguiente. Si está conectado a una réplica de solo lectura, se devolverá READ_ONLY.

```SQL
SELECT DATABASEPROPERTYEX(DB_NAME(), 'Updateability')
```

> [!NOTE]
> En un momento dado, las sesiones de solo lectura solo pueden acceder a una de las réplicas de Always On.

## <a name="monitoring-and-troubleshooting-read-only-replica"></a>Supervisión y solución de problemas de réplica de solo lectura

Cuando se conecta a una réplica de solo lectura, puede tener acceso a las métricas de rendimiento mediante el `sys.dm_db_resource_stats` DMV. Para acceder a las estadísticas del plan de consulta, use la `sys.dm_exec_query_stats`, `sys.dm_exec_query_plan` y `sys.dm_exec_sql_text` DMV.

> [!NOTE]
> La DMV `sys.resource_stats` en la base de datos maestra lógica devuelve datos de almacenamiento y uso de CPU de la réplica principal.


## <a name="enable-and-disable-read-scale-out"></a>Habilitar y deshabilitar el escalado horizontal de lectura

El escalado horizontal de lectura está habilitado de forma predeterminada en el nivel Crítico para la empresa de [Instancia administrada](sql-database-managed-instance.md). Debe habilitarse explícitamente en los niveles Premium y Crítico para la empresa de la [base de datos situada en el servidor de SQL Database](sql-database-servers.md). Aquí se describen los métodos para habilitar y deshabilitar el escalado horizontal de lectura.

### <a name="powershell-enable-and-disable-read-scale-out"></a>PowerShell: Habilitar y deshabilitar el escalado horizontal de lectura

Para administrar el escalado horizontal de lectura en Azure PowerShell se requiere la versión de Azure PowerShell de diciembre de 2016 u otra posterior. Para saber dónde encontrar la versión más reciente de PowerShell, consulte [Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-az-ps).

Habilitar o deshabilitar la lectura de escalabilidad horizontal en Azure PowerShell invocando el [conjunto AzSqlDatabase](/powershell/module/az.sql/set-azsqldatabase) cmdlet y pasando el valor deseado, `Enabled` o `Disabled` --para la `-ReadScale` parámetro. Como alternativa, puede usar el [New AzSqlDatabase](/powershell/module/az.sql/new-azsqldatabase) para crear una nueva base de datos con habilitada de escala horizontal de lectura.

Por ejemplo, para habilitar el escalado horizontal de lectura en una base de datos existente (los elementos entre corchetes angulares se sustituyen por los valores correctos para su entorno y se quitan dichos corchetes):

```powershell
Set-AzSqlDatabase -ResourceGroupName <myresourcegroup> -ServerName <myserver> -DatabaseName <mydatabase> -ReadScale Enabled
```

Para deshabilitar el escalado horizontal de lectura en una base de datos existente (los elementos entre corchetes angulares se sustituyen por los valores correctos para su entorno y se quitan dichos corchetes):

```powershell
Set-AzSqlDatabase -ResourceGroupName <myresourcegroup> -ServerName <myserver> -DatabaseName <mydatabase> -ReadScale Disabled
```

Para crear una nueva base de datos con el escalado horizontal de lectura habilitado (los elementos entre corchetes angulares se sustituyen por los valores correctos para su entorno y se quitan los corchetes angulares):

```powershell
New-AzSqlDatabase -ResourceGroupName <myresourcegroup> -ServerName <myserver> -DatabaseName <mydatabase> -ReadScale Enabled -Edition Premium
```

### <a name="rest-api-enable-and-disable-read-scale-out"></a>API REST: Habilitar y deshabilitar el escalado horizontal de lectura

Para crear una base de datos que tenga habilitado el escalado de lectura, o para habilitar o deshabilitar esta característica en una base de datos existente, cree o actualice la entidad correspondiente de la base de datos con la propiedad `readScale` establecida en `Enabled` o `Disabled`, como en la siguiente solicitud de ejemplo.

```rest
Method: PUT
URL: https://management.azure.com/subscriptions/{SubscriptionId}/resourceGroups/{GroupName}/providers/Microsoft.Sql/servers/{ServerName}/databases/{DatabaseName}?api-version= 2014-04-01-preview
Body:
{
   "properties":
   {
      "readScale":"Enabled"
   }
}
```

Para más información, consulte [Databases - Create or Update](https://docs.microsoft.com/rest/api/sql/databases/createorupdate).

## <a name="using-tempdb-on-read-only-replica"></a>Uso de TempDB en la réplica de solo lectura

La base de datos TempDB no se replica en las réplicas de solo lectura. Cada réplica tiene su propia versión de la base de datos TempDB que se crea cuando se crea la réplica. Se asegura de que TempDB es actualizable y puede modificarse durante la ejecución de la consulta. Si depende de la carga de trabajo de solo lectura sobre el uso de objetos de TempDB, debe crear estos objetos como parte de la secuencia de comandos de consulta. 

## <a name="using-read-scale-out-with-geo-replicated-databases"></a>Uso del escalado horizontal de lectura con las bases de datos con replicación geográfica

Si usas el escalado horizontal de lectura para cargar las cargas de trabajo de saldo de solo lectura en una base de datos con replicación geográfica (por ejemplo, como un miembro de un grupo de conmutación por error), asegúrese de que esa escala horizontal de lectura está habilitado en el servidor principal y las bases de datos secundaria con replicación geográfica. Esta configuración garantiza que la misma experiencia de equilibrio de carga continúa cuando la aplicación se conecta a la nueva réplica principal después de la conmutación por error. Si se conecta a la base de datos secundaria con replicación geográfica con el escalado de lectura habilitado, las sesiones con `ApplicationIntent=ReadOnly` se enrutarán a una de las réplicas del mismo modo que se enrutan las conexiones en la base de datos principal.  Las sesiones sin `ApplicationIntent=ReadOnly` se enrutarán a la réplica principal de la secundaria con replicación geográfica, que también es de solo lectura. Dado que una base de datos secundaria con replicación geográfica tiene un punto de conexión diferente que la base de datos principal, históricamente para tener acceso a la base de datos secundaria no era necesario establecer `ApplicationIntent=ReadOnly`. Para garantizar la compatibilidad con versiones anteriores, la DMV de `sys.geo_replication_links` muestra `secondary_allow_connections=2` (se permite cualquier conexión de cliente).

> [!NOTE]
> Round robin o cualquier otra carga equilibrada enrutamiento entre las réplicas de base de datos secundaria locales no se admite.

## <a name="next-steps"></a>Pasos siguientes

- Para obtener información sobre cómo usar PowerShell para establecer el escalado horizontal de lectura, vea el [conjunto AzSqlDatabase](/powershell/module/az.sql/set-azsqldatabase) o [New AzSqlDatabase](/powershell/module/az.sql/new-azsqldatabase) cmdlets.
- Para información sobre el uso de la API REST para establecer el escalado horizontal de lectura, consulte [Databases - Create or Update](https://docs.microsoft.com/rest/api/sql/databases/createorupdate) (Bases de datos: creación o actualización).
