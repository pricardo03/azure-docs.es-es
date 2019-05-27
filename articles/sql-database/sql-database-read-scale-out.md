---
title: 'Azure SQL Database: lectura de consultas en réplicas| Microsoft Docs'
description: La base de datos de SQL Azure proporciona la capacidad de equilibrar la carga de cargas de trabajo de solo lectura mediante la capacidad de réplicas de solo lectura - denominadas escalado horizontal de lectura.
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
ms.date: 04/19/2019
ms.openlocfilehash: cbcdcfd151951334246a4e85d9f521a15bb6269d
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "66146108"
---
# <a name="use-read-only-replicas-to-load-balance-read-only-query-workloads"></a>Usar réplicas de solo lectura para las cargas de trabajo de consulta de solo lectura de equilibrio de carga

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]
> [!IMPORTANT]
> El módulo de PowerShell de Azure Resource Manager es compatible aún con Azure SQL Database, pero todo el desarrollo futuro es para el módulo Az.Sql. Para estos cmdlets, consulte [AzureRM.Sql](https://docs.microsoft.com/powershell/module/AzureRM.Sql/). Los argumentos para los comandos en el módulo de Az y en los módulos AzureRm son esencialmente idénticos.

Como parte de la [arquitectura de alta disponibilidad](./sql-database-high-availability.md#premium-and-business-critical-service-tier-availability), cada base de datos en el nivel de servicio Premium, crítico para la empresa o a gran escala se aprovisiona automáticamente con una réplica principal y varias réplicas secundarias. Las réplicas secundarias se aprovisionan con el mismo tamaño de proceso que la réplica principal. El **escalado horizontal de lectura** característica le permite equilibrar la carga base de datos SQL de solo lectura las cargas de trabajo mediante la capacidad de una de las réplicas de solo lectura en lugar de compartir la réplica de lectura y escritura. De este modo, la carga de trabajo de solo lectura se aísla de la carga de trabajo principal de lectura y escritura y no afecta a su rendimiento. La característica está pensada para las aplicaciones que tienen cargas de trabajo separadas lógicamente de solo lectura, como el análisis. Pueden obtener ventajas de rendimiento con esta capacidad adicional sin ningún costo adicional.

El siguiente diagrama ilustra mediante una base de datos críticos para la empresa.

![Réplicas de solo lectura](media/sql-database-read-scale-out/business-critical-service-tier-read-scale-out.png)

La característica de escalado horizontal de lectura está habilitada de forma predeterminada en la nueva Premium, crítico para la empresa y las bases de datos a gran escala. Si la cadena de conexión de SQL está configurada con `ApplicationIntent=ReadOnly`, la aplicación se redirigirá a la puerta de enlace a una réplica de solo lectura de esa base de datos. Para obtener información sobre cómo usar el `ApplicationIntent` propiedad, vea [especificar Application Intent](https://docs.microsoft.com/sql/relational-databases/native-client/features/sql-server-native-client-support-for-high-availability-disaster-recovery#specifying-application-intent).

Si desea asegurarse de que la aplicación se conecta a la réplica principal sin tener en cuenta el `ApplicationIntent` establecer en la cadena de conexión de SQL, debe deshabilitar explícitamente la escala horizontal de lectura al crear la base de datos o al modificar su configuración. Por ejemplo, si actualiza la base de datos de nivel estándar o de uso General al nivel Premium, crítico para la empresa o a gran escala y desea asegurarse de que todas las conexiones siguen yendo a la réplica principal, deshabilite el escalado horizontal de lectura. Para obtener más información sobre cómo deshabilitarla, consulte [habilitar y deshabilitar el escalado horizontal de lectura](#enable-and-disable-read-scale-out).

> [!NOTE]
> Consulta de datos Store, Extended Events, SQL Profiler y las características de auditoría no se admiten en las réplicas de solo lectura. 

## <a name="data-consistency"></a>Coherencia de los datos

Una de las ventajas de las réplicas es que siempre se encuentran en estado de coherencia transaccional, pero es posible que en diferentes momentos pueda haber una pequeña latencia entre las distintas réplicas. El escalado horizontal de lectura admite la coherencia en el nivel de sesión. Significa que, si la sesión de solo lectura se vuelve a conectar después de un error de conexión causado por falta de disponibilidad de réplica, se puede redirigir a una réplica que no esté actualizada con la réplica de lectura y escritura al 100%. Del mismo modo, si una aplicación escribe datos mediante una sesión de lectura y escritura y los lee inmediatamente mediante una sesión de solo lectura, es posible que las actualizaciones más recientes no son inmediatamente visibles en la réplica. La latencia se produce por una operación de rehacer de registro de transacción asincrónica.

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

Escalado horizontal de lectura está habilitado de forma predeterminada en los niveles de servicio Premium, crítico para la empresa y a gran escala. No se puede habilitar el escalado horizontal de lectura en los niveles de servicio básico, estándar o de uso General. Escalado horizontal de lectura se deshabilita automáticamente en las bases de datos a gran escala, configurados con 0 réplicas. 

Puede deshabilitar y volver a habilitar el escalado horizontal de lectura en bases de datos únicas y grupo elástico en el nivel de servicio Premium o crítico para la empresa mediante los métodos siguientes.

> [!NOTE]
> La capacidad de deshabilitar el escalado horizontal de lectura se proporciona por compatibilidad con versiones anteriores.

### <a name="azure-portal"></a>Azure Portal

Puede administrar la configuración de Sacle horizontal de lectura en el **configurar** hoja base de datos. 

### <a name="powershell"></a>PowerShell

Para administrar el escalado horizontal de lectura en Azure PowerShell se requiere la versión de Azure PowerShell de diciembre de 2016 u otra posterior. Para saber dónde encontrar la versión más reciente de PowerShell, consulte [Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-az-ps).

Puede deshabilitar o volver a habilitar el escalado horizontal de lectura en Azure PowerShell invocando el [conjunto AzSqlDatabase](/powershell/module/az.sql/set-azsqldatabase) cmdlet y pasando el valor deseado, `Enabled` o `Disabled` --para la `-ReadScale` parámetro. 

Para deshabilitar la lectura de escalabilidad horizontal en una base de datos existente (los elementos de los corchetes angulares con los valores correctos para su entorno y se quitan los corchetes angulares):

```powershell
Set-AzSqlDatabase -ResourceGroupName <myresourcegroup> -ServerName <myserver> -DatabaseName <mydatabase> -ReadScale Disabled
```
Para deshabilitar la lectura de escalabilidad horizontal en una nueva base de datos (los elementos de los corchetes angulares con los valores correctos para su entorno y se quitan los corchetes angulares):

```powershell
New-AzSqlDatabase -ResourceGroupName <myresourcegroup> -ServerName <myserver> -DatabaseName <mydatabase> -ReadScale Disabled -Edition Premium
```

Para volver a habilitar la lectura de escalabilidad horizontal en una base de datos existente (los elementos de los corchetes angulares con los valores correctos para su entorno y se quitan los corchetes angulares):

```powershell
Set-AzSqlDatabase -ResourceGroupName <myresourcegroup> -ServerName <myserver> -DatabaseName <mydatabase> -ReadScale Enabled
```

### <a name="rest-api"></a>API DE REST

Para crear una base de datos con escalado horizontal de lectura deshabilitado, o para cambiar la configuración para una base de datos existente, utilice el método siguiente con el `readScale` propiedad establecida en `Enabled` o `Disabled` como en la siguiente solicitud de ejemplo.

```rest
Method: PUT
URL: https://management.azure.com/subscriptions/{SubscriptionId}/resourceGroups/{GroupName}/providers/Microsoft.Sql/servers/{ServerName}/databases/{DatabaseName}?api-version= 2014-04-01-preview
Body:
{
   "properties":
   {
      "readScale":"Disabled"
   }
}
```

Para más información, consulte [Databases - Create or Update](https://docs.microsoft.com/rest/api/sql/databases/createorupdate).

## <a name="using-tempdb-on-read-only-replica"></a>Uso de TempDB en la réplica de solo lectura

La base de datos TempDB no se replica en las réplicas de solo lectura. Cada réplica tiene su propia versión de la base de datos TempDB que se crea cuando se crea la réplica. Se asegura de que TempDB es actualizable y puede modificarse durante la ejecución de la consulta. Si depende de la carga de trabajo de solo lectura sobre el uso de objetos de TempDB, debe crear estos objetos como parte de la secuencia de comandos de consulta. 

## <a name="using-read-scale-out-with-geo-replicated-databases"></a>Uso del escalado horizontal de lectura con las bases de datos con replicación geográfica

Si usas el escalado horizontal de lectura para cargas de trabajo de equilibrio de carga de solo lectura en una base de datos con replicación geográfica (por ejemplo, como un miembro de un grupo de conmutación por error), asegúrese de que esa escala horizontal de lectura está habilitado en el servidor principal y las bases de datos secundaria con replicación geográfica. Esta configuración garantiza que la misma experiencia de equilibrio de carga continúa cuando la aplicación se conecta a la nueva réplica principal después de la conmutación por error. Si se conecta a la base de datos secundaria con replicación geográfica con el escalado de lectura habilitado, las sesiones con `ApplicationIntent=ReadOnly` se enrutarán a una de las réplicas del mismo modo que se enrutan las conexiones en la base de datos principal.  Las sesiones sin `ApplicationIntent=ReadOnly` se enrutarán a la réplica principal de la secundaria con replicación geográfica, que también es de solo lectura. Porque la base de datos secundaria con replicación geográfica tiene un punto de conexión diferente que la base de datos principal, históricamente para tener acceso a la base de datos secundaria no era necesario para establecer `ApplicationIntent=ReadOnly`. Para garantizar la compatibilidad con versiones anteriores, la DMV de `sys.geo_replication_links` muestra `secondary_allow_connections=2` (se permite cualquier conexión de cliente).

> [!NOTE]
> Round robin o cualquier otro con equilibrio de carga de enrutamiento entre las réplicas de base de datos secundaria locales no se admite.

## <a name="next-steps"></a>Pasos siguientes

- Para obtener información acerca de la oferta de base de datos de SQL a gran escala, consulte [nivel de servicio de hiperescala](./sql-database-service-tier-hyperscale.md).
