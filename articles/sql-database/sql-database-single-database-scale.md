---
title: 'Escalar recursos de base de datos única: Azure SQL Database | Microsoft Docs'
description: En este artículo se describe cómo escalar los recursos de proceso y almacenamiento disponibles para una base de datos única en Azure SQL Database.
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: DBs & servers
ms.topic: conceptual
ms.date: 06/20/2018
ms.author: carlrab
ms.openlocfilehash: 525416506a22f386de574ca02b2e919ac47b8737
ms.sourcegitcommit: 638599eb548e41f341c54e14b29480ab02655db1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/21/2018
ms.locfileid: "36311139"
---
# <a name="scale-single-database-resources-in-azure-sql-database"></a>Escalar recursos de base de datos única en Azure SQL Database

En este artículo se describe cómo escalar los recursos de proceso y almacenamiento disponibles para una base de datos única en Azure SQL Database. 

## <a name="vcore-based-purchasing-model-change-storage-size"></a>Modelo de compra basado en núcleos virtuales: cambiar el tamaño del almacenamiento

- Se puede aprovisionar el almacenamiento hasta el límite máximo de tamaño con incrementos de 1 GB. El almacenamiento de datos mínimo configurable es 5 GB 
- Se puede aprovisionar almacenamiento para una base de datos única mediante el aumento o disminución de su tamaño máximo con [Azure Portal](https://portal.azure.com), [Transact-SQL](/sql/t-sql/statements/alter-database-azure-sql-database#examples), [PowerShell](/powershell/module/azurerm.sql/set-azurermsqldatabase), la [CLI de Azure](/cli/azure/sql/db#az_sql_db_update) o la [API REST](/rest/api/sql/databases/update).
- SQL Database asigna automáticamente 30 % de almacenamiento adicional para los archivos de registro y 32 GB por núcleo virtual para TempDB, sin exceder los 384 GB. TempDB se encuentra en una SSD asociada en todos los niveles de servicio.
- El precio de almacenamiento para una única base de datos es la suma de los importes de almacenamiento de datos y almacenamiento de registros multiplicado por el precio de la unidad de almacenamiento del nivel de servicio. El costo de TempDB se incluye en el precio del núcleo virtual. Para más información sobre el precio del almacenamiento adicional, consulte los [precios de SQL Database](https://azure.microsoft.com/pricing/details/sql-database/).

## <a name="vcore-based-purchasing-model-change-compute-resources"></a>Modelo de compra basado en núcleos virtuales: cambiar los recursos de proceso

Después de elegir inicialmente el número de núcleos virtuales, puede escalar o reducir una base de datos verticalmente de manera dinámica en función de la experiencia real mediante [Azure Portal](sql-database-single-database-scale.md#azure-portal-manage-logical-servers-and-databases), [Transact-SQL](/sql/t-sql/statements/alter-database-azure-sql-database#examples), [PowerShell](/powershell/module/azurerm.sql/set-azurermsqldatabase), la [CLI de Azure](/cli/azure/sql/db#az_sql_db_update) o la [API REST](/rest/api/sql/databases/update). 

Al cambiar el nivel de servicio o de rendimiento de una base de datos, se crea una réplica de la base de datos original en el nuevo nivel de rendimiento y, a continuación, se cambian las conexiones a la réplica. Durante este proceso, no se pierde ningún dato; sin embargo, durante el breve momento en que se cambie a la réplica, las conexiones a la base de datos estarán deshabilitadas, por tanto, es posible que se reviertan algunas transacciones en curso. La longitud de tiempo del cambio varía, pero suele estar por debajo de 4 segundos y es inferior a 30 segundos en el 99 % de las veces. Si el número de transacciones en curso es elevado en el momento en que las conexiones están deshabilitadas, esta longitud de tiempo puede ser mayor. 

La duración de todo el proceso de escalado vertical depende del nivel de servicio y del tamaño de la base de datos antes y después del cambio. Por ejemplo, el cambio de una base de datos de 250 GB dentro de un nivel de servicio De uso general, o bien desde o hacia este, se completará en seis horas. En una base de datos del mismo tamaño que cambie los niveles de rendimiento en el nivel de servicio Crítico para la empresa, el escalado vertical se completará en unas tres horas.

> [!TIP]
> Para supervisar las operaciones en curso, consulte los temas sobre [administración de operaciones mediante la API de REST de SQL](/rest/api/sql/Operations/List), [administración de operaciones mediante la CLI](/cli/azure/sql/db/op) y [supervisión de operaciones mediante T-SQL](/sql/relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database), y los dos siguientes comandos de PowerShell: [Get-AzureRmSqlDatabaseActivity](/powershell/module/azurerm.sql/get-azurermsqldatabaseactivity) y [Stop-AzureRmSqlDatabaseActivity](/powershell/module/azurerm.sql/stop-azurermsqldatabaseactivity).

* Si va a actualizar a un nivel de servicio o rendimiento más elevado, el tamaño máximo de la base de datos no aumenta a no ser que especifique un tamaño mayor (maxsize).
* Para cambiar una base de datos a una versión anterior, su espacio usado no debe alcanzar el tamaño máximo permitido del nivel de servicio de destino y del nivel de rendimiento. 
* Al actualizar una base de datos con la [replicación geográfica](sql-database-geo-replication-portal.md) habilitada, actualice sus bases de datos secundarias al nivel de rendimiento deseado antes de actualizar la base de datos principal (instrucciones generales para mejorar el rendimiento). Al actualizar a una diferente, antes hay que actualizar la base de datos secundaria.
* Al degradar una base de datos con la [replicación geográfica](sql-database-geo-replication-portal.md) habilitada, degrade sus bases de datos principales al nivel de rendimiento deseado antes de degradar la secundaria (regla general para un mejor rendimiento). Al cambiar a una versión anterior a una edición diferente, antes hay que cambiar la principal a una versión anterior.
* Las nuevas propiedades de la base de datos no se aplican hasta que se completan los cambios.

## <a name="dtu-based-purchasing-model-change-storage-size"></a>Modelo de compra basado en DTU: cambiar el tamaño del almacenamiento

- El precio de la DTU para una base de datos única incluye una cierta cantidad de almacenamiento sin costo adicional. El almacenamiento adicional que supere la cantidad incluida se puede aprovisionar por un costo extra hasta el límite de tamaño máximo en incrementos de 250 GB hasta 1 TB, y luego en incrementos de 256 GB superando 1 TB. Para cantidades de almacenamiento incluido y límites de tamaño máximo, consulte [Base de datos única: tamaños de almacenamiento y niveles de rendimiento](#single-database-storage-sizes-and-performance-levels).
- Se puede aprovisionar almacenamiento adicional para una base de datos única mediante el aumento de su tamaño máximo con [Azure Portal](sql-database-servers-databases-manage.md#azure-portal-manage-logical-servers-and-databases), [Transact-SQL](/sql/t-sql/statements/alter-database-azure-sql-database#examples), [PowerShell](/powershell/module/azurerm.sql/set-azurermsqldatabase), la [CLI de Azure](/cli/azure/sql/db#az_sql_db_update) o la [API de REST](/rest/api/sql/databases/update).
- El precio del almacenamiento adicional para una base de datos única es la cantidad de almacenamiento adicional multiplicado por el precio de la unidad de almacenamiento adicional del nivel de servicio. Para más información sobre el precio del almacenamiento adicional, consulte los [precios de SQL Database](https://azure.microsoft.com/pricing/details/sql-database/).

## <a name="dtu-based-purchasing-model-change-compute-resources-dtus"></a>Modelo de compra basado en DTU: cambiar los recursos de proceso (DTU)

Después de elegir inicialmente un nivel de servicio, un nivel de rendimiento y una cantidad de almacenamiento, puede escalar o reducir una base de datos verticalmente de manera dinámica en función de la experiencia real mediante [Azure Portal](sql-database-servers-databases-manage.md#azure-portal-manage-logical-servers-and-databases), [Transact-SQL](/sql/t-sql/statements/alter-database-azure-sql-database#examples), [PowerShell](/powershell/module/azurerm.sql/set-azurermsqldatabase), la [CLI de Azure](/cli/azure/sql/db#az_sql_db_update) o la [API de REST](/rest/api/sql/databases/update). 

El vídeo siguiente muestra cómo cambia de manera dinámica el nivel de rendimiento para aumentar las DTU disponibles para una base de datos única.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-SQL-Database-dynamically-scale-up-or-scale-down/player]
>

Al cambiar el nivel de servicio o de rendimiento de una base de datos, se crea una réplica de la base de datos original en el nuevo nivel de rendimiento y, a continuación, se cambian las conexiones a la réplica. Durante este proceso, no se pierde ningún dato; sin embargo, durante el breve momento en que se cambie a la réplica, las conexiones a la base de datos estarán deshabilitadas, por tanto, es posible que se reviertan algunas transacciones en curso. La duración del cambio varía, pero suele ser menor de 30 segundos el 99 % de las veces. Si el número de transacciones en curso es elevado en el momento en que las conexiones están deshabilitadas, esta longitud de tiempo puede ser mayor. 

La duración de todo el proceso de escalado vertical depende del nivel de servicio y del tamaño de la base de datos antes y después del cambio. Por ejemplo, el cambio de una base de datos de 250 GB dentro de un nivel de servicio Estándar, o bien desde o hacia este, se completará en seis horas. Para una base de datos del mismo tamaño que cambie los niveles de rendimiento del nivel de servicio Premium, el escalado vertical se completará en unas tres horas.

> [!TIP]
> Para supervisar las operaciones en curso, consulte los temas sobre [administración de operaciones mediante la API de REST de SQL](/rest/api/sql/Operations/List), [administración de operaciones mediante la CLI](/cli/azure/sql/db/op) y [supervisión de operaciones mediante T-SQL](/sql/relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database), y los dos siguientes comandos de PowerShell: [Get-AzureRmSqlDatabaseActivity](/powershell/module/azurerm.sql/get-azurermsqldatabaseactivity) y [Stop-AzureRmSqlDatabaseActivity](/powershell/module/azurerm.sql/stop-azurermsqldatabaseactivity).

* Si va a actualizar a un nivel de servicio o rendimiento más elevado, el tamaño máximo de la base de datos no aumenta a no ser que especifique un tamaño mayor (maxsize).
* Para cambiar una base de datos a una versión anterior, su espacio usado no debe alcanzar el tamaño máximo permitido del nivel de servicio de destino y del nivel de rendimiento. 
* Al pasar del nivel **Premium** al nivel **Estándar**, se aplica un costo de almacenamiento adicional si (1) el tamaño máximo de la base de datos es compatible con el nivel de rendimiento de destino y (2) el tamaño máximo supera la cantidad de almacenamiento incluido del nivel de rendimiento de destino. Por ejemplo, si una base de datos P1 con un tamaño máximo de 500 GB se reduce a S3, se aplica un costo de almacenamiento adicional porque S3 admite un tamaño máximo de 500 GB y su cantidad de almacenamiento incluido es solo de 250 GB. Por lo tanto, la cantidad de almacenamiento adicional es 500 GB – 250 GB = 250 GB. Para conocer el precio del almacenamiento adicional, consulte los [precios de SQL Database](https://azure.microsoft.com/pricing/details/sql-database/). Si la cantidad de espacio real utilizada es menor que la cantidad de almacenamiento incluido, este costo adicional puede evitarse si se reduce el tamaño máximo de la base de datos a la cantidad incluida. 
* Al actualizar una base de datos con la [replicación geográfica](sql-database-geo-replication-portal.md) habilitada, actualice sus bases de datos secundarias al nivel de rendimiento deseado antes de actualizar la base de datos principal (instrucciones generales para mejorar el rendimiento). Al actualizar a una diferente, antes hay que actualizar la base de datos secundaria.
* Al degradar una base de datos con la [replicación geográfica](sql-database-geo-replication-portal.md) habilitada, degrade sus bases de datos principales al nivel de rendimiento deseado antes de degradar la secundaria (regla general para un mejor rendimiento). Al cambiar a una versión anterior a una edición diferente, antes hay que cambiar la principal a una versión anterior.
* Las ofertas del servicio de restauración son diferentes para los distintos niveles de servicio. Si va a cambiar al nivel **Básico**, hay un período de retención de copia de seguridad más bajo; consulte [Copias de seguridad de Azure SQL Database](sql-database-automated-backups.md).
* Las nuevas propiedades de la base de datos no se aplican hasta que se completan los cambios.

## <a name="dtu-based-purchasing-model-limitations-of-p11-and-p15-when-the-maximum-size-greater-than-1-tb"></a>Modelo de compra basado en DTU: limitaciones de P11 y P15 cuando el máximo tamaño es superior a 1 TB

Se admite un tamaño máximo superior a 1 TB en bases de datos P11 y P15 en las siguientes regiones: Este de Australia, Sudeste de Australia, Sur de Brasil, Centro de Canadá, Este de Canadá, Centro de EE. UU., Centro de Francia, Centro de Alemania, Este de Japón, Oeste de Japón, Centro de Corea, Centro y Norte de EE. UU., Europa del Norte, Centro y Sur de EE. UU., Sudeste Asiático, Sur de Reino Unido, Oeste de Reino Unido, Este de EE. UU. 2, Oeste de EE. UU., Virginia Gob. EE. UU. y Europa Occidental. Las siguientes consideraciones y limitaciones se aplican a las bases de datos P11 y P15 con un tamaño máximo mayor de 1 TB:

- Si elige un tamaño máximo mayor de 1 TB al crear una base de datos (con un valor de 4 TB o 4096 GB), el comando de creación produce un error si la base de datos se aprovisiona en una región no admitida.
- Para las bases de datos P11 y P15 existentes ubicadas en alguna de las regiones admitidas, puede aumentar el almacenamiento máximo hasta superar 1 TB en incrementos de 256 MB hasta 4 TB. Para ver si se admite un tamaño mayor en su región, utilice la función [DATABASEPROPERTYEX](/sql/t-sql/functions/databasepropertyex-transact-sql) o inspeccione el tamaño de la base de datos en Azure Portal. La actualización de una base de datos P11 o P15 existente solo puede realizarse mediante un inicio de sesión de entidad de seguridad de nivel de servidor o por los miembros del rol de base de datos dbmanager. 
- Si se ha ejecutado una operación de actualización en una región admitida, la configuración se actualiza inmediatamente. La base de datos permanecerá en línea durante el proceso de actualización. Sin embargo, no puede utilizar la cantidad total de almacenamiento a partir de 1 TB de almacenamiento hasta que se hayan actualizado los archivos de base de datos reales con el nuevo tamaño máximo. El período de tiempo necesario depende del tamaño de la base de datos que se va a actualizar. 
- Al crear o actualizar una base de datos P11 o P15, solo puede optar entre un tamaño máximo de 1 TB y 4 TB en incrementos de 256 GB. Cuando crea una base de datos P11 o P15, se selecciona previamente la opción de almacenamiento predeterminada de 1 TB. En el caso de bases de datos ubicadas en alguna de las regiones admitidas, puede aumentar el almacenamiento máximo hasta un máximo de 4 TB en bases de datos únicas nuevas o existentes. Para todas las demás regiones, el tamaño máximo no se puede aumentar por encima de 1 TB. El precio no cambia cuando se selecciona la opción de 4 TB de almacenamiento incluido.
- Si el tamaño máximo de una base de datos se establece en más de 1 TB, no podrá modificarse a 1 TB incluso si el almacenamiento real utilizado es inferior a 1 TB. Por lo tanto, no puede cambiar a una versión anterior una P11 o P15 con un tamaño máximo mayor que 1 TB a un P11 de 1 TB o a un P15 de 1 TB o un nivel de rendimiento menor, por ejemplo, P1-P6). Esta restricción también se aplica a los escenarios de copia y restauración, lo que incluye la restauración a un momento dado, la restauración geográfica, la retención de copias de seguridad a largo plazo y la copia de base de datos. Cuando se configura una base de datos con un tamaño máximo de 1 TB, todas las operaciones de restauración de esta base de datos deben ejecutarse en una P11/P15 con un tamaño máximo mayor de 1 TB.
- En escenarios de replicación geográfica activa:
   - Configuración de una relación de replicación geográfica: si la base de datos principal es P11 o P15, la secundaria (una o varias) también debe P11 o P15; aquellas con un nivel de rendimiento inferior se rechazarán como secundarias, puesto que no tienen capacidad para admitir más de 1 TB.
   - Actualización de la base de datos principal en una relación de replicación geográfica: al cambiar el tamaño máximo a 1 TB en una base de datos principal, se desencadenará el mismo cambio en la base de datos secundaria. Ambas actualizaciones deben realizarse correctamente para que el cambio en la principal surta efecto. Se aplican limitaciones por región para la opción de más de 1 TB. Si la base de datos secundaria está en una región que no admite más de 1 TB, no se actualizará la principal.
- No se admite el uso del servicio Import/Export para cargar bases de datos P11/P15 con más de 1 TB. Use SqlPackage.exe para [importar](sql-database-import.md) y [exportar](sql-database-export.md) datos.


## <a name="next-steps"></a>Pasos siguientes

Para conocer los límites de recursos globales, consulte [Límites de recursos basados en núcleos virtuales de Azure SQL Database para bases de datos únicas](sql-database-vcore-resource-limits-single-databases.md) y [SQL Database DTU-based resource limits - elastic pools](sql-database-dtu-resource-limits-single-databases.md) (Límites de recursos basados en DTU de SQL Database para grupos elásticos).
