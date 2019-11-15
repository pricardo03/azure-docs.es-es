---
title: 'Límites de recursos de núcleo virtual: grupos elásticos'
description: En esta página se describen algunos límites de recursos de núcleos virtuales comunes para grupos elásticos en Azure SQL Database.
services: sql-database
ms.service: sql-database
ms.subservice: elastic-pools
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: oslake
ms.author: moslake
ms.reviewer: carlrab, sstein
ms.date: 11/04/2019
ms.openlocfilehash: f356b9d248ac9c5f0bcfaaeeb37b43d958eaa528
ms.sourcegitcommit: ac56ef07d86328c40fed5b5792a6a02698926c2d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/08/2019
ms.locfileid: "73822371"
---
# <a name="resource-limits-for-elastic-pools-using-the-vcore-purchasing-model"></a>Límites de recursos para grupos elásticos que usan el modelo de compra de núcleo virtual

En este artículo se proporcionan los límites de recursos detallados para grupos elásticos y bases de datos agrupadas de Azure SQL Database que utilizan el modelo de compra de núcleo virtual.

Para información sobre los límites del modelo de compra de DTU, consulte [Límites de recursos para grupos elásticos que utilizan el modelo de compra de DTU](sql-database-dtu-resource-limits-elastic-pools.md).

> [!IMPORTANT]
> En algunas circunstancias, puede que deba reducir una base de datos para reclamar el espacio no utilizado. Para obtener más información, consulte [Administración del espacio de archivo en Azure SQL Database](sql-database-file-space-management.md).

Puede establecer el nivel de servicio, el tamaño de proceso y la cantidad de almacenamiento mediante [Azure Portal](sql-database-elastic-pool-manage.md#azure-portal-manage-elastic-pools-and-pooled-databases), [PowerShell](sql-database-elastic-pool-manage.md#powershell-manage-elastic-pools-and-pooled-databases), la [CLI de Azure](sql-database-elastic-pool-manage.md#azure-cli-manage-elastic-pools-and-pooled-databases) o la [API REST](sql-database-elastic-pool-manage.md#rest-api-manage-elastic-pools-and-pooled-databases).

> [!IMPORTANT]
> Para obtener información sobre la guía y otras consideraciones del escalado, consulte [Escalar un grupo elástico](sql-database-elastic-pool-scale.md).


## <a name="general-purpose---provisioned-compute---gen4"></a>Uso general: proceso aprovisionado: Gen4

> [!IMPORTANT]
> Las nuevas bases de datos de Gen4 ya no se admiten en las regiones Este de Australia o Sur de Brasil.

### <a name="general-purpose-service-tier-generation-4-compute-platform-part-1"></a>Nivel de servicio de uso general: Plataforma de procesos de generación 4 (parte 1)

|Tamaño de proceso|GP_Gen4_1|GP_Gen4_2|GP_Gen4_3|GP_Gen4_4|GP_Gen4_5|GP_Gen4_6
|:--- | --: |--: |--: |--: |--: |--: |
|Generación de procesos|Gen4|Gen4|Gen4|Gen4|Gen4|Gen4|
|Núcleos virtuales|1|2|3|4|5|6|
|Memoria (GB)|7|14|21|28|35|42|
|Máximo número de bases de datos por grupo|100|200|500|500|500|500|
|Compatible con almacén de columnas|Sí|Sí|Sí|Sí|Sí|Sí|
|Almacenamiento OLTP en memoria (GB)|N/D|N/D|N/D|N/D|N/D|N/D|
|Tamaño máximo de datos (GB)|512|756|756|1536|1536|1536|
|Tamaño máximo de registro|154|227|227|461|461|461|
|Tamaño de TempDB (GB)|32|64|96|128|160|192|
|Tipo de almacenamiento|Premium Storage (remoto)|Premium Storage (remoto)|Premium Storage (remoto)|Premium Storage (remoto)|Premium Storage (remoto)|Premium Storage (remoto)|
|Latencia de E/S (aproximada)|5-7 ms (escritura)<br>5-10 ms (lectura)|5-7 ms (escritura)<br>5-10 ms (lectura)|5-7 ms (escritura)<br>5-10 ms (lectura)|5-7 ms (escritura)<br>5-10 ms (lectura)|5-7 ms (escritura)<br>5-10 ms (lectura)|5-7 ms (escritura)<br>5-10 ms (lectura)|
|IOPS de destino (64 KB)|500|1000|1\.500|2000|2\.500|3000|
|Límites de velocidad de los registros (MBps)|4,6875|9,375|14,0625|18,75|23,4375|28,125|
|Cantidad máxima de trabajos simultáneos por grupo (solicitudes)* |210|420|630|840|1050|1260|
|Cantidad máxima de inicios de sesión simultáneos por grupo * |210|420|630|840|1050|1260|
|Sesiones máximas permitidas|30000|30000|30000|30000|30000|30000|
|Opciones de núcleo virtual mín./máx. de grupos elásticos por base de datos|0, 0.25, 0.5, 1|0, 0.25, 0.5, 1, 2|0, 0.25, 0.5, 1...3|0, 0.25, 0.5, 1...4|0, 0.25, 0.5, 1...5|0, 0.25, 0.5, 1...6|
|Número de réplicas|1|1|1|1|1|1|
|AZ múltiple|N/D|N/D|N/D|N/D|N/D|N/D|
|Escalado horizontal de lectura|N/D|N/D|N/D|N/D|N/D|N/D|
|Almacenamiento de copia de seguridad incluido|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|

\* Para conocer el número máximo de trabajos (solicitudes) simultáneos para una base de datos individual, consulte [Límites del modelo de compra basado en núcleos virtuales de Azure SQL Database para una base de datos única](sql-database-vcore-resource-limits-single-databases.md). Por ejemplo, si el grupo elástico usa Gen5 y su núcleo virtual máximo por base de datos es 2, el número máximo de trabajos simultáneos es 200.  Si el núcleo virtual máximo por base de datos es 0,5, el número máximo de trabajos simultáneos es 50, puesto que, en Gen5, hay un máximo de 100 trabajos simultáneos por núcleo virtual.  Para otras configuraciones de memoria con núcleo virtual máximo por base de datos que sean un núcleo virtual o menos, la cantidad máxima de trabajos simultáneos se escala de forma similar.


### <a name="general-purpose-service-tier-generation-4-compute-platform-part-2"></a>Nivel de servicio de uso general: Plataforma de procesos de generación 4 (parte 2)

|Tamaño de proceso|GP_Gen4_7|GP_Gen4_8|GP_Gen4_9|GP_Gen4_10|GP_Gen4_16|GP_Gen4_24|
|:--- | --: |--: |--: |--: |--: |--: |
|Generación de procesos|Gen4|Gen4|Gen4|Gen4|Gen4|Gen4|
|Núcleos virtuales|7|8|9|10|16|24|
|Memoria (GB)|49|56|63|70|112|168|
|Máximo número de bases de datos por grupo|500|500|500|500|500|500|
|Compatible con almacén de columnas|Sí|Sí|Sí|Sí|Sí|Sí|
|Almacenamiento OLTP en memoria (GB)|N/D|N/D|N/D|N/D|N/D|N/D|
|Tamaño máximo de datos (GB)|1536|2048|2048|2048|3584|4096|
|Tamaño máximo de registro (GB)|461|614|614|614|1075|1229|
|Tamaño de TempDB (GB)|224|256|288|320|384|384|
|Tipo de almacenamiento|Premium Storage (remoto)|Premium Storage (remoto)|Premium Storage (remoto)|Premium Storage (remoto)|Premium Storage (remoto)|Premium Storage (remoto)|
|Latencia de E/S (aproximada)|5-7 ms (escritura)<br>5-10 ms (lectura)|5-7 ms (escritura)<br>5-10 ms (lectura)|5-7 ms (escritura)<br>5-10 ms (lectura)|5-7 ms (escritura)<br>5-10 ms (lectura)|5-7 ms (escritura)<br>5-10 ms (lectura)|5-7 ms (escritura)<br>5-10 ms (lectura)|
|IOPS de destino (64 KB)|3500|4000|4500|5000|7000|7000|
|Límites de velocidad de los registros (MBps)|32,8125|37.5|37.5|37.5|37.5|37.5|
|Cantidad máxima de trabajos simultáneos por grupo (solicitudes)*|1470|1680|1890|2100|3360|5040|
|Cantidad máxima de inicios de sesión simultáneos por grupo (solicitudes) *|1470|1680|1890|2100|3360|5040|
|Sesiones máximas permitidas|30000|30000|30000|30000|30000|30000|
|Opciones de núcleo virtual mín./máx. de grupos elásticos por base de datos|0, 0.25, 0.5, 1...7|0, 0.25, 0.5, 1...8|0, 0.25, 0.5, 1...9|0, 0.25, 0.5, 1...10|0, 0.25, 0.5, 1...10, 16|0, 0.25, 0.5, 1...10, 16, 24|
|Número de réplicas|1|1|1|1|1|1|
|AZ múltiple|N/D|N/D|N/D|N/D|N/D|N/D|
|Escalado horizontal de lectura|N/D|N/D|N/D|N/D|N/D|N/D|
|Almacenamiento de copia de seguridad incluido|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|

\* Para conocer el número máximo de trabajos (solicitudes) simultáneos para una base de datos individual, consulte [Límites del modelo de compra basado en núcleos virtuales de Azure SQL Database para una base de datos única](sql-database-vcore-resource-limits-single-databases.md). Por ejemplo, si el grupo elástico usa Gen5 y su núcleo virtual máximo por base de datos es 2, el número máximo de trabajos simultáneos es 200.  Si el núcleo virtual máximo por base de datos es 0,5, el número máximo de trabajos simultáneos es 50, puesto que, en Gen5, hay un máximo de 100 trabajos simultáneos por núcleo virtual.  Para otras configuraciones de memoria con núcleo virtual máximo por base de datos que sean un núcleo virtual o menos, la cantidad máxima de trabajos simultáneos se escala de forma similar.


## <a name="general-purpose---provisioned-compute---gen5"></a>Uso general: proceso aprovisionado: Gen5

### <a name="general-purpose-service-tier-generation-5-compute-platform-part-1"></a>Nivel de servicio de uso general: Plataforma de procesos de generación 5 (parte 1)

|Tamaño de proceso|GP_Gen5_2|GP_Gen5_4|GP_Gen5_6|GP_Gen5_8|GP_Gen5_10|GP_Gen5_12|GP_Gen5_14|
|:--- | --: |--: |--: |--: |---: | --: |--: |
|Generación de procesos|Gen5|Gen5|Gen5|Gen5|Gen5|Gen5|Gen5|
|Núcleos virtuales|2|4|6|8|10|12|14|
|Memoria (GB)|10.2|20.4|30.6|40.8|51|61.2|71.4|
|Máximo número de bases de datos por grupo|100|200|500|500|500|500|500|
|Compatible con almacén de columnas|Sí|Sí|Sí|Sí|Sí|Sí|Sí|
|Almacenamiento OLTP en memoria (GB)|N/D|N/D|N/D|N/D|N/D|N/D|N/D|
|Tamaño máximo de datos (GB)|512|756|756|1536|1536|1536|
|Tamaño máximo de registro (GB)|154|227|227|461|461|461|461|
|Tamaño de TempDB (GB)|64|128|192|256|320|384|384|
|Tipo de almacenamiento|Premium Storage (remoto)|Premium Storage (remoto)|Premium Storage (remoto)|Premium Storage (remoto)|Premium Storage (remoto)|Premium Storage (remoto)|Premium Storage (remoto)|
|Latencia de E/S (aproximada)|5-7 ms (escritura)<br>5-10 ms (lectura)|5-7 ms (escritura)<br>5-10 ms (lectura)|5-7 ms (escritura)<br>5-10 ms (lectura)|5-7 ms (escritura)<br>5-10 ms (lectura)|5-7 ms (escritura)<br>5-10 ms (lectura)|5-7 ms (escritura)<br>5-10 ms (lectura)|5-7 ms (escritura)<br>5-10 ms (lectura)|
|IOPS de destino (64 KB)|1000|2000|3000|4000|5000|6000|7000|
|Límites de velocidad de los registros (MBps)|4,6875|9,375|14,0625|18,75|23,4375|28,125|32,8125|
|Cantidad máxima de trabajos simultáneos por grupo (solicitudes)*|210|420|630|840|1050|1260|1470|
|Cantidad máxima de inicios de sesión simultáneos por grupo (solicitudes) *|210|420|630|840|1050|1260|1470|
|Sesiones máximas permitidas|30000|30000|30000|30000|30000|30000|30000|
|Opciones de núcleo virtual mín./máx. de grupos elásticos por base de datos|0, 0.25, 0.5, 1, 2|0, 0.25, 0.5, 1...4|0, 0.25, 0.5, 1...6|0, 0.25, 0.5, 1...8|0, 0.25, 0.5, 1...10|0, 0.25, 0.5, 1...12|0, 0.25, 0.5, 1...14|
|Número de réplicas|1|1|1|1|1|1|1|
|AZ múltiple|N/D|N/D|N/D|N/D|N/D|N/D|N/D|
|Escalado horizontal de lectura|N/D|N/D|N/D|N/D|N/D|N/D|N/D|
|Almacenamiento de copia de seguridad incluido|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|

\* Para conocer el número máximo de trabajos (solicitudes) simultáneos para una base de datos individual, consulte [Límites del modelo de compra basado en núcleos virtuales de Azure SQL Database para una base de datos única](sql-database-vcore-resource-limits-single-databases.md). Por ejemplo, si el grupo elástico usa Gen5 y su núcleo virtual máximo por base de datos es 2, el número máximo de trabajos simultáneos es 200.  Si el núcleo virtual máximo por base de datos es 0,5, el número máximo de trabajos simultáneos es 50, puesto que, en Gen5, hay un máximo de 100 trabajos simultáneos por núcleo virtual.  Para otras configuraciones de memoria con núcleo virtual máximo por base de datos que sean un núcleo virtual o menos, la cantidad máxima de trabajos simultáneos se escala de forma similar.

### <a name="general-purpose-service-tier-generation-5-compute-platform-part-2"></a>Nivel de servicio de uso general: Plataforma de procesos de generación 5 (parte 2)

|Tamaño de proceso|GP_Gen5_16|GP_Gen5_18|GP_Gen5_20|GP_Gen5_24|GP_Gen5_32|GP_Gen5_40|GP_Gen5_80|
|:--- | --: |--: |--: |--: |---: | --: |--: |
|Generación de procesos|Gen5|Gen5|Gen5|Gen5|Gen5|Gen5|Gen5|
|Núcleos virtuales|16|18|20|24|32|40|80|
|Memoria (GB)|81.6|91.8|102|122.4|163.2|204|408|
|Máximo número de bases de datos por grupo|500|500|500|500|500|500|500|
|Compatible con almacén de columnas|Sí|Sí|Sí|Sí|Sí|Sí|Sí|
|Almacenamiento OLTP en memoria (GB)|N/D|N/D|N/D|N/D|N/D|N/D|N/D|
|Tamaño máximo de datos (GB)|2048|2048|3072|3072|4096|4096|4096|
|Tamaño máximo de registro (GB)|614|614|922|922|1229|1229|1229|
|Tamaño de TempDB (GB)|384|384|384|384|384|384|384|
|Tipo de almacenamiento|Premium Storage (remoto)|Premium Storage (remoto)|Premium Storage (remoto)|Premium Storage (remoto)|Premium Storage (remoto)|Premium Storage (remoto)|Premium Storage (remoto)|
|Latencia de E/S (aproximada)|5-7 ms (escritura)<br>5-10 ms (lectura)|5-7 ms (escritura)<br>5-10 ms (lectura)|5-7 ms (escritura)<br>5-10 ms (lectura)|5-7 ms (escritura)<br>5-10 ms (lectura)|5-7 ms (escritura)<br>5-10 ms (lectura)|5-7 ms (escritura)<br>5-10 ms (lectura)|5-7 ms (escritura)<br>5-10 ms (lectura)|
|IOPS de destino (64 KB)|7000|7000|7000|7000|7000|7000|7000|
|Límites de velocidad de los registros (MBps)|37.5|37.5|37.5|37.5|37.5|37.5|37.5|
|Cantidad máxima de trabajos simultáneos por grupo (solicitudes)*|1680|1890|2100|2520|3360|4200|8400|
|Cantidad máxima de inicios de sesión simultáneos por grupo (solicitudes) *|1680|1890|2100|2520|3360|4200|8400|
|Opciones de núcleo virtual mín./máx. de grupos elásticos por base de datos|0, 0.25, 0.5, 1...16|0, 0.25, 0.5, 1...18|0, 0.25, 0.5, 1...20|0, 0.25, 0.5, 1...20, 24|0, 0.25, 0.5, 1...20, 24, 32|0, 0.25, 0.5, 1...16, 24, 32, 40|0, 0.25, 0.5, 1...16, 24, 32, 40, 80|
|Número de réplicas|1|1|1|1|1|1|1|
|AZ múltiple|N/D|N/D|N/D|N/D|N/D|N/D|N/D|
|Escalado horizontal de lectura|N/D|N/D|N/D|N/D|N/D|N/D|N/D|
|Almacenamiento de copia de seguridad incluido|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|

## <a name="general-purpose---provisioned-compute---fsv2-series"></a>Uso general: proceso aprovisionado: serie Fsv2

### <a name="fsv2-series-compute-generation-preview"></a>Generación de proceso de la serie Fsv2 (versión preliminar)

|Tamaño de proceso|GP_Fsv2_72|
|:--- | --: |
|Generación de procesos|Serie Fsv2|
|Núcleos virtuales|72|
|Memoria (GB)|136|
|Máximo número de bases de datos por grupo|500|
|Compatible con almacén de columnas|Sí|
|Almacenamiento OLTP en memoria (GB)|N/D|
|Tamaño máximo de datos (GB)|4096|
|Tamaño máximo de registro (GB)|1024|
|Tamaño máximo de datos de TempDB (GB)|333|
|Tipo de almacenamiento|Premium Storage (remoto)|
|Latencia de E/S (aproximada)|5-7 ms (escritura)<br>5-10 ms (lectura)|
|IOPS de destino (64 KB)|36000|
|Límites de velocidad de los registros (MBps)|37.5|
|Cantidad máxima de trabajos simultáneos por grupo (solicitudes)*|1680|
|Cantidad máxima de inicios de sesión simultáneos por grupo (solicitudes) *|1680|
|Opciones de núcleo virtual mín./máx. de grupos elásticos por base de datos|0-72|
|Número de réplicas|1|
|AZ múltiple|N/D|
|Escalado horizontal de lectura|N/D|
|Almacenamiento de copia de seguridad incluido|1X el tamaño de base de datos|




\* Para conocer el número máximo de trabajos (solicitudes) simultáneos para una base de datos individual, consulte [Límites del modelo de compra basado en núcleos virtuales de Azure SQL Database para una base de datos única](sql-database-vcore-resource-limits-single-databases.md). Por ejemplo, si el grupo elástico usa Gen5 y su núcleo virtual máximo por base de datos es 2, el número máximo de trabajos simultáneos es 200.  Si el núcleo virtual máximo por base de datos es 0,5, el número máximo de trabajos simultáneos es 50, puesto que, en Gen5, hay un máximo de 100 trabajos simultáneos por núcleo virtual.  Para otras configuraciones de memoria con núcleo virtual máximo por base de datos que sean un núcleo virtual o menos, la cantidad máxima de trabajos simultáneos se escala de forma similar.


## <a name="business-critical---provisioned-compute---gen4"></a>Crítico para la empresa: proceso aprovisionado: Gen4

> [!IMPORTANT]
> Las nuevas bases de datos de Gen4 ya no se admiten en las regiones Este de Australia o Sur de Brasil.

### <a name="business-critical-service-tier-generation-4-compute-platform-part-1"></a>Nivel de servicio crítico para la empresa: Plataforma de procesos de generación 4 (parte 1)

|Tamaño de proceso|BC_Gen4_1|BC_Gen4_2|BC_Gen4_3|BC_Gen4_4|BC_Gen4_5|BC_Gen4_6|
|:--- | --: |--: |--: |--: |--: |--: |
|Generación de procesos|Gen4|Gen4|Gen4|Gen4|Gen4|Gen4|
|Núcleos virtuales|1|2|3|4|5|6|
|Memoria (GB)|7|14|21|28|35|42|
|Máximo número de bases de datos por grupo|Solo se admiten las bases de datos únicas con este tamaño de proceso|50|100|100|100|100|
|Compatible con almacén de columnas|Sí|Sí|Sí|Sí|Sí|Sí|
|Almacenamiento OLTP en memoria (GB)|1|2|3|4|5|6|
|Tipo de almacenamiento|SSD local|SSD local|SSD local|SSD local|SSD local|SSD local|
|Tamaño máximo de datos (GB)|650|650|650|650|650|650|
|Tamaño máximo de registro (GB)|195|195|195|195|195|195|
|Tamaño de TempDB (GB)|32|64|96|128|160|192|
|Latencia de E/S (aproximada)|1-2 ms (escritura)<br>1-2 ms (lectura)|1-2 ms (escritura)<br>1-2 ms (lectura)|1-2 ms (escritura)<br>1-2 ms (lectura)|1-2 ms (escritura)<br>1-2 ms (lectura)|1-2 ms (escritura)<br>1-2 ms (lectura)|1-2 ms (escritura)<br>1-2 ms (lectura)|
|IOPS de destino (64 KB)|5000|10000|15000|20000|25000|30000|
|Límites de velocidad de los registros (MBps)|10|20|30|40|50|60|
|Cantidad máxima de trabajos simultáneos por grupo (solicitudes)*|210|420|630|840|1050|1260|
|Cantidad máxima de inicios de sesión simultáneos por grupo (solicitudes) *|210|420|630|840|1050|1260|
|Sesiones máximas permitidas|30000|30000|30000|30000|30000|30000|
|Opciones de núcleo virtual mín./máx. de grupos elásticos por base de datos|N/D|0, 0.25, 0.5, 1, 2|0, 0.25, 0.5, 1...3|0, 0.25, 0.5, 1...4|0, 0.25, 0.5, 1...5|0, 0.25, 0.5, 1...6|
|Número de réplicas|4|4|4|4|4|4|
|AZ múltiple|Sí|Sí|Sí|Sí|Sí|Sí|
|Escalado horizontal de lectura|Sí|Sí|Sí|Sí|Sí|Sí|
|Almacenamiento de copia de seguridad incluido|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|

\* Para conocer el número máximo de trabajos (solicitudes) simultáneos para una base de datos individual, consulte [Límites del modelo de compra basado en núcleos virtuales de Azure SQL Database para una base de datos única](sql-database-vcore-resource-limits-single-databases.md). Por ejemplo, si el grupo elástico usa Gen5 y su núcleo virtual máximo por base de datos es 2, el número máximo de trabajos simultáneos es 200.  Si el núcleo virtual máximo por base de datos es 0,5, el número máximo de trabajos simultáneos es 50, puesto que, en Gen5, hay un máximo de 100 trabajos simultáneos por núcleo virtual.  Para otras configuraciones de memoria con núcleo virtual máximo por base de datos que sean un núcleo virtual o menos, la cantidad máxima de trabajos simultáneos se escala de forma similar.

### <a name="business-critical-service-tier-generation-4-compute-platform-part-2"></a>Nivel de servicio crítico para la empresa: Plataforma de procesos de generación 4 (parte 2)

|Tamaño de proceso|BC_Gen4_7|BC_Gen4_8|BC_Gen4_9|BC_Gen4_10|BC_Gen4_16|BC_Gen4_24|
|:--- | --: |--: |--: |--: |--: |--: |
|Generación de procesos|Gen4|Gen4|Gen4|Gen4|Gen4|Gen4|
|Núcleos virtuales|7|8|9|10|16|24|
|Memoria (GB)|81.6|91.8|102|122.4|163.2|204|
|Máximo número de bases de datos por grupo|100|100|100|100|100|100|
|Compatible con almacén de columnas|N/D|N/D|N/D|N/D|N/D|N/D|
|Almacenamiento OLTP en memoria (GB)|7|8|9.5|11|20|36|
|Tipo de almacenamiento|SSD local|SSD local|SSD local|SSD local|SSD local|SSD local|
|Tamaño máximo de datos (GB)|650|650|650|650|1024|1024|
|Tamaño máximo de registro (GB)|195|195|195|195|307|307|
|Tamaño de TempDB (GB)|224|256|288|320|384|384|
|Latencia de E/S (aproximada)|1-2 ms (escritura)<br>1-2 ms (lectura)|1-2 ms (escritura)<br>1-2 ms (lectura)|1-2 ms (escritura)<br>1-2 ms (lectura)|1-2 ms (escritura)<br>1-2 ms (lectura)|1-2 ms (escritura)<br>1-2 ms (lectura)|1-2 ms (escritura)<br>1-2 ms (lectura)|
|IOPS de destino (64 KB)|35000|40000|45000|50000|80000|120000|
|Límites de velocidad de los registros (MBps)|70|80|80|80|80|80|
|Cantidad máxima de trabajos simultáneos por grupo (solicitudes)*|1470|1680|1890|2100|3360|5040|
|Cantidad máxima de inicios de sesión simultáneos por grupo (solicitudes) *|1470|1680|1890|2100|3360|5040|
|Sesiones máximas permitidas|30000|30000|30000|30000|30000|30000|
|Opciones de núcleo virtual mín./máx. de grupos elásticos por base de datos|0, 0.25, 0.5, 1...7|0, 0.25, 0.5, 1...8|0, 0.25, 0.5, 1...9|0, 0.25, 0.5, 1...10|0, 0.25, 0.5, 1...10, 16|0, 0.25, 0.5, 1...10, 16, 24|
|Número de réplicas|4|4|4|4|4|4|
|AZ múltiple|Sí|Sí|Sí|Sí|Sí|Sí|
|Escalado horizontal de lectura|Sí|Sí|Sí|Sí|Sí|Sí|
|Almacenamiento de copia de seguridad incluido|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|

\* Para conocer el número máximo de trabajos (solicitudes) simultáneos para una base de datos individual, consulte [Límites del modelo de compra basado en núcleos virtuales de Azure SQL Database para una base de datos única](sql-database-vcore-resource-limits-single-databases.md). Por ejemplo, si el grupo elástico usa Gen5 y su núcleo virtual máximo por base de datos es 2, el número máximo de trabajos simultáneos es 200.  Si el núcleo virtual máximo por base de datos es 0,5, el número máximo de trabajos simultáneos es 50, puesto que, en Gen5, hay un máximo de 100 trabajos simultáneos por núcleo virtual.  Para otras configuraciones de memoria con núcleo virtual máximo por base de datos que sean un núcleo virtual o menos, la cantidad máxima de trabajos simultáneos se escala de forma similar.

## <a name="business-critical---provisioned-compute---gen5"></a>Crítico para la empresa: proceso aprovisionado: Gen5

### <a name="business-critical-service-tier-generation-5-compute-platform-part-1"></a>Nivel de servicio crítico para la empresa: Plataforma de procesos de generación 5 (parte 1)

|Tamaño de proceso|BC_Gen5_2|BC_Gen5_4|BC_Gen5_6|BC_Gen5_8|BC_Gen5_10|BC_Gen5_12|BC_Gen5_14|
|:--- | --: |--: |--: |--: |---: | --: |--: |
|Generación de procesos|Gen5|Gen5|Gen5|Gen5|Gen5|Gen5|Gen5|
|Núcleos virtuales|2|4|6|8|10|12|14|
|Memoria (GB)|10.2|20.4|30.6|40.8|51|61.2|71.4|
|Máximo número de bases de datos por grupo|Solo se admiten las bases de datos únicas con este tamaño de proceso|50|100|100|100|100|100|
|Compatible con almacén de columnas|Sí|Sí|Sí|Sí|Sí|Sí|Sí|
|Almacenamiento OLTP en memoria (GB)|1571|3142|4.713|6284|8.655|11.026|13.397|
|Tamaño máximo de datos (GB)|1024|1024|1536|1536|1536|3072|3072|
|Tamaño máximo de registro (GB)|307|307|307|461|461|922|922|
|Tamaño de TempDB (GB)|64|128|192|256|320|384|384|
|Tipo de almacenamiento|SSD local|SSD local|SSD local|SSD local|SSD local|SSD local|SSD local|
|Latencia de E/S (aproximada)|1-2 ms (escritura)<br>1-2 ms (lectura)|1-2 ms (escritura)<br>1-2 ms (lectura)|1-2 ms (escritura)<br>1-2 ms (lectura)|1-2 ms (escritura)<br>1-2 ms (lectura)|1-2 ms (escritura)<br>1-2 ms (lectura)|1-2 ms (escritura)<br>1-2 ms (lectura)|1-2 ms (escritura)<br>1-2 ms (lectura)|
|IOPS de destino (64 KB)|5000|10000|15000|20000|25000|30000|35000|
|Límites de velocidad de los registros (MBps)|15|30|45|60|75|90|105|
|Cantidad máxima de trabajos simultáneos por grupo (solicitudes)*|210|420|630|840|1050|1260|1470|
|Cantidad máxima de inicios de sesión simultáneos por grupo (solicitudes) *|210|420|630|840|1050|1260|1470|
|Sesiones máximas permitidas|30000|30000|30000|30000|30000|30000|30000|
|Opciones de núcleo virtual mín./máx. de grupos elásticos por base de datos|N/D|0, 0.25, 0.5, 1...4|0, 0.25, 0.5, 1...6|0, 0.25, 0.5, 1...8|0, 0.25, 0.5, 1...10|0, 0.25, 0.5, 1...12|0, 0.25, 0.5, 1...14|
|Número de réplicas|4|4|4|4|4|4|4|
|AZ múltiple|Sí|Sí|Sí|Sí|Sí|Sí|
|Escalado horizontal de lectura|Sí|Sí|Sí|Sí|Sí|Sí|Sí|
|Almacenamiento de copia de seguridad incluido|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|

\* Para conocer el número máximo de trabajos (solicitudes) simultáneos para una base de datos individual, consulte [Límites del modelo de compra basado en núcleos virtuales de Azure SQL Database para una base de datos única](sql-database-vcore-resource-limits-single-databases.md). Por ejemplo, si el grupo elástico usa Gen5 y su núcleo virtual máximo por base de datos es 2, el número máximo de trabajos simultáneos es 200.  Si el núcleo virtual máximo por base de datos es 0,5, el número máximo de trabajos simultáneos es 50, puesto que, en Gen5, hay un máximo de 100 trabajos simultáneos por núcleo virtual.  Para otras configuraciones de memoria con núcleo virtual máximo por base de datos que sean un núcleo virtual o menos, la cantidad máxima de trabajos simultáneos se escala de forma similar.

### <a name="business-critical-service-tier-generation-5-compute-platform-part-2"></a>Nivel de servicio crítico para la empresa: Plataforma de procesos de generación 5 (parte 2)

|Tamaño de proceso|BC_Gen5_16|BC_Gen5_18|BC_Gen5_20|BC_Gen5_24|BC_Gen5_32|BC_Gen5_40|BC_Gen5_80|
|:--- | --: |--: |--: |--: |---: | --: |--: |
|Generación de procesos|Gen5|Gen5|Gen5|Gen5|Gen5|Gen5|Gen5|
|Núcleos virtuales|16|18|20|24|32|40|80|
|Memoria (GB)|81.6|91.8|102|122.4|163.2|204|408|
|Máximo número de bases de datos por grupo|100|100|100|100|100|100|100|
|Compatible con almacén de columnas|Sí|Sí|Sí|Sí|Sí|Sí|Sí|
|Almacenamiento OLTP en memoria (GB)|15 768|18.139|20.51|25 252|37 936|52.22|131,64|
|Tamaño máximo de datos (GB)|3072|3072|3072|4096|4096|4096|4096|
|Tamaño máximo de registro (GB)|922|922|922|1229|1229|1229|1229|
|Tamaño de TempDB (GB)|384|384|384|384|384|384|384|
|Tipo de almacenamiento|SSD local|SSD local|SSD local|SSD local|SSD local|SSD local|SSD local|
|Latencia de E/S (aproximada)|1-2 ms (escritura)<br>1-2 ms (lectura)|1-2 ms (escritura)<br>1-2 ms (lectura)|1-2 ms (escritura)<br>1-2 ms (lectura)|1-2 ms (escritura)<br>1-2 ms (lectura)|1-2 ms (escritura)<br>1-2 ms (lectura)|1-2 ms (escritura)<br>1-2 ms (lectura)|1-2 ms (escritura)<br>1-2 ms (lectura)|
|IOPS de destino (64 KB)|40000|45000|50000|60000|80000|100000|200000|
|Límites de velocidad de los registros (MBps)|120|120|120|120|120|120|120|
|Cantidad máxima de trabajos simultáneos por grupo (solicitudes)*|1680|1890|2100|2520|3360|4200|8400|
|Cantidad máxima de inicios de sesión simultáneos por grupo (solicitudes) *|1680|1890|2100|2520|3360|4200|8400|
|Sesiones máximas permitidas|30000|30000|30000|30000|30000|30000|30000|
|Opciones de núcleo virtual mín./máx. de grupos elásticos por base de datos|0, 0.25, 0.5, 1...16|0, 0.25, 0.5, 1...18|0, 0.25, 0.5, 1...20|0, 0.25, 0.5, 1...20, 24|0, 0.25, 0.5, 1...20, 24, 32|0, 0.25, 0.5, 1...20, 24, 32, 40|0, 0.25, 0.5, 1...20, 24, 32, 40, 80|
|Número de réplicas|4|4|4|4|4|4|4|
|AZ múltiple|Sí|Sí|Sí|Sí|Sí|Sí|Sí|
|Escalado horizontal de lectura|Sí|Sí|Sí|Sí|Sí|Sí|Sí|
|Almacenamiento de copia de seguridad incluido|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|

## <a name="business-critical---provisioned-compute---m-series"></a>Crítico para la empresa: proceso aprovisionado: serie M

### <a name="m-series-compute-generation-preview"></a>Generación de proceso de la serie M (versión preliminar)

|Tamaño de proceso|GP_M_128|
|:--- | --: |
|Generación de procesos|Serie M|
|Núcleos virtuales|128|
|Memoria (GB)|3767|
|Máximo número de bases de datos por grupo|100|
|Compatible con almacén de columnas|Sí|
|Almacenamiento OLTP en memoria (GB)|481|
|Tamaño máximo de datos (GB)|4096|
|Tamaño máximo de registro (GB)|2048|
|Tamaño máximo de datos de TempDB (GB)|4096|
|Tipo de almacenamiento|SSD local|
|Latencia de E/S (aproximada)|1-2 ms (escritura)<br>1-2 ms (lectura)|
|IOPS de destino (64 KB)|40000|
|Límites de velocidad de los registros (MBps)|120|
|Cantidad máxima de trabajos simultáneos por grupo (solicitudes)*|1680|
|Cantidad máxima de inicios de sesión simultáneos por grupo (solicitudes) *|1680|
|Sesiones máximas permitidas|30000|
|Opciones de núcleo virtual mín./máx. de grupos elásticos por base de datos|0-128|
|Número de réplicas|4|
|AZ múltiple|Sí|
|Escalado horizontal de lectura|Sí|
|Almacenamiento de copia de seguridad incluido|1X el tamaño de base de datos|



\* Para conocer el número máximo de trabajos (solicitudes) simultáneos para una base de datos individual, consulte [Límites del modelo de compra basado en núcleos virtuales de Azure SQL Database para una base de datos única](sql-database-vcore-resource-limits-single-databases.md). Por ejemplo, si el grupo elástico usa Gen5 y su núcleo virtual máximo por base de datos es 2, el número máximo de trabajos simultáneos es 200.  Si el núcleo virtual máximo por base de datos es 0,5, el número máximo de trabajos simultáneos es 50, puesto que, en Gen5, hay un máximo de 100 trabajos simultáneos por núcleo virtual.  Para otras configuraciones de memoria con núcleo virtual máximo por base de datos que sean un núcleo virtual o menos, la cantidad máxima de trabajos simultáneos se escala de forma similar.

Si todos los núcleos virtuales de un grupo elástico están ocupados, cada una de las bases de datos del grupo recibe la misma cantidad de recursos de proceso para procesar las consultas. El servicio SQL Database proporciona ecuanimidad de uso compartido de recursos entre bases de datos garantizando los mismos segmentos de tiempo de proceso. La ecuanimidad de uso compartido de recursos del grupo elástico es adicional a cualquier cantidad de recursos garantizados de otro modo a cada base de datos cuando el número mínimo de núcleos virtuales por base de datos se establece en un valor distinto de cero.

## <a name="database-properties-for-pooled-databases"></a>Propiedades de base de datos para bases de datos agrupadas

En la tabla siguiente se describen las propiedades de las bases de datos agrupadas.

> [!NOTE]
> Los límites de recursos de las bases de datos individuales de los grupos elásticos suelen ser los mismos que los de las bases de datos únicas fuera de los grupos que tienen el mismo tamaño de proceso. Por ejemplo, el número máximo de trabajos simultáneos en una base de datos GP_Gen4_1 es 200 trabajos. Por lo tanto, el número máximo de trabajos simultáneos en una base de datos de un grupo GP_Gen4_1 también es 200 trabajos. Tenga en cuenta que el número total de trabajos simultáneos en el grupo GP_Gen4_1 es 210.

| Propiedad | DESCRIPCIÓN |
|:--- |:--- |
| Núcleos virtuales máximos por base de datos |El número máximo de núcleos virtuales que puede usar cualquier base de datos del grupo, si está disponible según el uso que hacen otras bases de datos del grupo. El número de núcleos virtuales por base de datos no garantiza la disponibilidad de recursos para una base de datos. Se trata de una configuración global que se aplica a todas las bases de datos del grupo. Establezca un número de núcleos virtuales por base de datos lo suficientemente alto como para gestionar los picos de uso de la base de datos. Se admite cierto grado de exceso de asignación de recursos, ya que el grupo suele basarse en patrones de uso en frío y caliente de las bases de datos, cuando en realidad los picos de uso no tienen lugar en todas las bases de datos a la vez.|
| Número mínimo de núcleos virtuales por base de datos |El número mínimo de núcleos virtuales que se garantiza en cualquier base de datos del grupo. Se trata de una configuración global que se aplica a todas las bases de datos del grupo. El número mínimo de núcleos virtuales por base de datos se puede establecer en 0, y también se trata del valor predeterminado. Esta propiedad se establece en cualquier valor entre 0 y el uso medio de núcleos virtuales por base de datos. El resultado de multiplicar el número de bases de datos del grupo y el número mínimo de núcleos virtuales por base de datos no puede superar el número de núcleos virtuales por grupo.|
| Almacenamiento máximo por base de datos |El tamaño máximo de base de datos establecido por el usuario para una base de datos de un grupo. Las bases de datos agrupadas comparten el almacenamiento del grupo asignado, de modo que el tamaño que puede alcanzar una base de datos se limita a la menor cantidad de almacenamiento restante del grupo y el tamaño de la base de datos. El tamaño máximo de la base de datos hace referencia al tamaño máximo de los archivos de datos, y no incluye el espacio utilizado por los archivos de registro. |
|||

## <a name="next-steps"></a>Pasos siguientes

- Para conocer los límites de recursos de los núcleos virtuales de una base de datos única, consulte los [límites de recursos para bases de datos únicas con el modelo de compra del núcleo virtual](sql-database-vcore-resource-limits-single-databases.md).
- Para conocer los límites de recursos de DTU para una base de datos única, consulte los [límites de recursos para bases de datos únicas que usan el modelo de compra de DTU](sql-database-dtu-resource-limits-single-databases.md).
- Para conocer los límites de recursos de DTU para grupos elásticos, consulte los [límites de recursos para grupos elásticos que usan el modelo de compra de DTU](sql-database-dtu-resource-limits-elastic-pools.md).
- Para conocer los límites de recursos para instancias administradas, consulte los [límites de recursos para instancias administradas](sql-database-managed-instance-resource-limits.md).
- Para más información sobre los límites generales de Azure, consulte [Límites, cuotas y restricciones de suscripción y servicios de Microsoft Azure](../azure-subscription-service-limits.md).
- Para más información sobre los límites de recursos en un servidor de bases de datos, consulte [Límites de recursos en un servidor de SQL Database](sql-database-resource-limits-database-server.md) para obtener información acerca de los límites en los niveles de servidor y suscripción.
