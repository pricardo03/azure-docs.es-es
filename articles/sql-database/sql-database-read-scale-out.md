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
ms.reviewer: carlrab
manager: craigg
ms.date: 12/05/2018
ms.openlocfilehash: 16737ed525147968c97ca20a9f4e674a0dee34fc
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/06/2018
ms.locfileid: "52955061"
---
# <a name="use-read-only-replicas-to-load-balance-read-only-query-workloads-preview"></a>Uso de réplicas de solo lectura para equilibrar la carga de las cargas de trabajo de consultas de solo lectura (versión preliminar)

El **escalado horizontal de lectura** le permite equilibrar la carga de las cargas de trabajo de solo lectura de Azure SQL Database mediante la capacidad de una réplica de solo lectura.

Cada base de datos del nivel Premium ([modelo de compra basado en la unidad de transmisión de datos (DTU)](sql-database-service-tiers-dtu.md)) o del nivel Crítico para la empresa ([modelo de compra basado en núcleos virtuales](sql-database-service-tiers-vcore.md)) se aprovisiona automáticamente con varias réplicas AlwaysOn para permitir el Acuerdo de Nivel de Servicio de disponibilidad.

![Réplicas de solo lectura](media/sql-database-managed-instance/business-critical-service-tier.png)

Estas réplicas se aprovisionan con el mismo tamaño de proceso que la réplica de lectura-escritura usada en las conexiones normales de base de datos. La característica de **escalado horizontal de lectura** le permite equilibrar la carga de las cargas de trabajo de solo lectura de SQL Database gracias al uso de la capacidad de una de las réplicas de solo lectura en lugar de compartir réplicas de solo escritura. De este modo, la carga de trabajo de solo lectura se aísla de la carga de trabajo principal de lectura y escritura y no afecta a su rendimiento. La característica está destinada a las aplicaciones que incluyen cargas de trabajo de solo lectura separadas lógicamente, como los casos de análisis, y, por tanto, esta capacidad adicional podría suponer ventajas para el rendimiento sin costo adicional.

Para usar la característica de escalado horizontal de lectura con una base de datos determinada, debe habilitarla explícitamente al crear la base de datos. También puede habilitarla más adelante modificando la configuración con los cmdlets [Set-AzureRmSqlDatabase](/powershell/module/azurerm.sql/set-azurermsqldatabase) o [New-AzureRmSqlDatabase](/powershell/module/azurerm.sql/new-azurermsqldatabase) de PowerShell o con el método [Databases - Create or Update](https://docs.microsoft.com/rest/api/sql/databases/createorupdate) de la API REST de Azure Resource Manager.

Después de habilitar el escalado horizontal de lectura en una base de datos, las aplicaciones que se conecten a ella se dirigirán a la réplica de lectura-escritura o a la réplica de solo lectura de esa base de datos, en función de la propiedad `ApplicationIntent` configurada en la cadena de conexión de la aplicación. Para más información sobre la propiedad `ApplicationIntent`, consulte [Specifying Application Intent](https://docs.microsoft.com/sql/relational-databases/native-client/features/sql-server-native-client-support-for-high-availability-disaster-recovery#specifying-application-intent) (Especificación de la intención de la aplicación).

Si está deshabilitado el escalado de lectura o establece la propiedad ReadScale en un nivel de servicio no admitido, todas las conexiones se dirigen a la réplica de lectura y escritura, con independencia de la propiedad `ApplicationIntent`.

> [!NOTE]
> Durante la versión preliminar, el Almacén de datos de consultas y los Eventos extendidos no se admiten en las réplicas de solo lectura.

## <a name="data-consistency"></a>Coherencia de datos

Una de las ventajas de las réplicas es que siempre se encuentran en estado de coherencia transaccional, pero es posible que en diferentes momentos pueda haber una pequeña latencia entre las distintas réplicas. El escalado horizontal de lectura admite la coherencia en el nivel de sesión. Esto significa que, si la sesión de solo lectura se vuelve a conectar después de un error de conexión debido a la falta de disponibilidad de las réplicas, se puede redirigir a una réplica que no esté actualizada al 100 % con la réplica de lectura-escritura. Del mismo modo, si una aplicación escribe datos mediante una sesión de lectura-escritura y los lee inmediatamente mediante una sesión de solo lectura, es posible que las actualizaciones más recientes no sean visibles inmediatamente. El motivo es que la fase de puesta al día del registro de transacciones con las réplicas es asincrónico.

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

## <a name="enable-and-disable-read-scale-out"></a>Habilitar y deshabilitar el escalado horizontal de lectura

El escalado horizontal de lectura está habilitado de forma predeterminada en el nivel Crítico para la empresa de [Instancia administrada](sql-database-managed-instance.md). Debe habilitarse explícitamente en los niveles Premium y Crítico para la empresa de la [base de datos situada en el servidor lógico](sql-database-logical-servers.md). Aquí se describen los métodos para habilitar y deshabilitar el escalado horizontal de lectura.

### <a name="powershell-enable-and-disable-read-scale-out"></a>PowerShell: Habilitar y deshabilitar el escalado horizontal de lectura

Para administrar el escalado horizontal de lectura en Azure PowerShell se requiere la versión de Azure PowerShell de diciembre de 2016 u otra posterior. Para saber dónde encontrar la versión más reciente de PowerShell, consulte [Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps).

Para habilitar o deshabilitar el escalado horizontal de lectura en Azure PowerShell, invoque el cmdlet [Set-AzureRmSqlDatabase](/powershell/module/azurerm.sql/set-azurermsqldatabase) y pase el valor deseado, `Enabled` o `Disabled`, al parámetro `-ReadScale`. Como alternativa, puede usar el cmdlet [New-AzureRmSqlDatabase](/powershell/module/azurerm.sql/new-azurermsqldatabase) para crear una nueva base de datos que tenga habilitado el escalado horizontal de lectura.

Por ejemplo, para habilitar el escalado horizontal de lectura en una base de datos existente (los elementos entre corchetes angulares se sustituyen por los valores correctos para su entorno y se quitan dichos corchetes):

```powershell
Set-AzureRmSqlDatabase -ResourceGroupName <myresourcegroup> -ServerName <myserver> -DatabaseName <mydatabase> -ReadScale Enabled
```

Para deshabilitar el escalado horizontal de lectura en una base de datos existente (los elementos entre corchetes angulares se sustituyen por los valores correctos para su entorno y se quitan dichos corchetes):

```powershell
Set-AzureRmSqlDatabase -ResourceGroupName <myresourcegroup> -ServerName <myserver> -DatabaseName <mydatabase> -ReadScale Disabled
```

Para crear una nueva base de datos con el escalado horizontal de lectura habilitado (los elementos entre corchetes angulares se sustituyen por los valores correctos para su entorno y se quitan los corchetes angulares):

```powershell
New-AzureRmSqlDatabase -ResourceGroupName <myresourcegroup> -ServerName <myserver> -DatabaseName <mydatabase> -ReadScale Enabled -Edition Premium
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

## <a name="using-read-scale-out-with-geo-replicated-databases"></a>Uso del escalado horizontal de lectura con las bases de datos con replicación geográfica

Si usa el escalado horizontal de lectura para equilibrar la carga de cargas de trabajo de solo lectura en una base de datos con replicación geográfica (por ejemplo, como miembro de un grupo de conmutación por error), asegúrese de que el escalado horizontal de lectura esté habilitado en las bases de datos principal y secundaria con replicación geográfica. Esto garantizará el mismo efecto de equilibrio de carga cuando la aplicación se conecte a la nueva base de datos principal después de la conmutación por error. Si se conecta a la base de datos secundaria con replicación geográfica con el escalado de lectura habilitado, las sesiones con `ApplicationIntent=ReadOnly` se enrutarán a una de las réplicas del mismo modo que se enrutan las conexiones en la base de datos principal.  Las sesiones sin `ApplicationIntent=ReadOnly` se enrutarán a la réplica principal de la secundaria con replicación geográfica, que también es de solo lectura. Dado que una base de datos secundaria con replicación geográfica tiene un punto de conexión diferente que la base de datos principal, históricamente para tener acceso a la base de datos secundaria no era necesario establecer `ApplicationIntent=ReadOnly`. Para garantizar la compatibilidad con versiones anteriores, la DMV de `sys.geo_replication_links` muestra `secondary_allow_connections=2` (se permite cualquier conexión de cliente).

> [!NOTE]
> Durante la versión preliminar, no se admite round-robin ni ningún otro enrutamiento de carga equilibrada entre las réplicas locales de la base de datos secundaria.

## <a name="next-steps"></a>Pasos siguientes

- Para información sobre el uso de PowerShell para establecer el escalado horizontal de lectura, consulte los cmdlets [Set-AzureRmSqlDatabase](/powershell/module/azurerm.sql/set-azurermsqldatabase) o [New-AzureRmSqlDatabase](/powershell/module/azurerm.sql/new-azurermsqldatabase).
- Para información sobre el uso de la API REST para establecer el escalado horizontal de lectura, consulte [Databases - Create or Update](https://docs.microsoft.com/rest/api/sql/databases/createorupdate) (Bases de datos: creación o actualización).
