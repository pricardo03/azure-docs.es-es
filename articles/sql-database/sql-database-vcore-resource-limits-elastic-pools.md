---
title: Límites de recursos basados en núcleos virtuales de Azure SQL Database para grupos elásticos | Microsoft Docs
description: En esta página se describen algunos límites de recursos basados en núcleos virtuales comunes para grupos elásticos en Azure SQL Database.
services: sql-database
ms.service: sql-database
ms.subservice: elastic-pools
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: oslake
ms.author: moslake
ms.reviewer: carlrab
manager: craigg
ms.date: 01/30/2019
ms.openlocfilehash: 48cdd064155bfbdfa2fef2ce5a8410e4c95e4c3b
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/31/2019
ms.locfileid: "55461073"
---
# <a name="azure-sql-database-vcore-based-purchasing-model-limits-for-elastic-pools"></a>Límites del modelo de compra basado en núcleos virtuales de Azure SQL Database para grupos elásticos

En este artículo se proporcionan los límites de recursos detallados para grupos elásticos y bases de datos agrupadas de Azure SQL Database que utilizan el modelo de compra basado en núcleos virtuales.

Para información sobre los límites del modelo de compra basado en DTU, consulte [SQL Database DTU-based resource limits - elastic pools](sql-database-dtu-resource-limits-elastic-pools.md) (Límites de recursos basados en DTU de SQL Database para grupos elásticos).

> [!IMPORTANT]
> En algunas circunstancias, puede que deba reducir una base de datos para reclamar el espacio no utilizado. Para más información, consulte [Administración del espacio de archivo en Azure SQL Database](sql-database-file-space-management.md).

Puede establecer el nivel de servicio, el tamaño de proceso y la cantidad de almacenamiento mediante [Azure Portal](sql-database-elastic-pool-manage.md#azure-portal-manage-elastic-pools-and-pooled-databases), [PowerShell](sql-database-elastic-pool-manage.md#powershell-manage-elastic-pools-and-pooled-databases), la [CLI de Azure](sql-database-elastic-pool-manage.md#azure-cli-manage-elastic-pools-and-pooled-databases) o la [API REST](sql-database-elastic-pool-manage.md#rest-api-manage-elastic-pools-and-pooled-databases).

> [!NOTE]
> Los límites de recursos de las bases de datos individuales de los grupos elásticos suelen ser los mismos que los de las bases de datos únicas fuera de los grupos que tienen el mismo tamaño de proceso. Por ejemplo, el número máximo de trabajos simultáneos en una base de datos GP_Gen4_1 es 200 trabajos. Por lo tanto, el número máximo de trabajos simultáneos en una base de datos de un grupo GP_Gen4_1 también es 200 trabajos. Tenga en cuenta que el número total de trabajos simultáneos en el grupo GP_Gen4_1 es 210.

## <a name="general-purpose-service-tier-storage-sizes-and-compute-sizes"></a>Nivel de servicio de uso general: tamaños de almacenamiento y de proceso

### <a name="general-purpose-service-tier-generation-4-compute-platform-part-1"></a>Nivel de servicio de uso general: Plataforma de procesos de generación 4 (parte 1)

|Tamaño de proceso|GP_Gen4_1|GP_Gen4_2|GP_Gen4_3|GP_Gen4_4|GP_Gen4_5|GP_Gen4_6
|:--- | --: |--: |--: |--: |--: |--: |
|Generación de H/W|4|4|4|4|4|4|
|Núcleos virtuales|1|2|3|4|5|6|
|Memoria (GB)|7|14|21|28|35|42|
|Compatible con almacén de columnas|SÍ|Sí|Sí|Sí|Sí|SÍ|
|Almacenamiento OLTP en memoria (GB)|N/D|N/D|N/D|N/D|N/D|N/D|
|Tamaño máximo de datos (GB)|512|756|756|1536|1536|1536|
|Tamaño máximo de registro|154|227|227|461|461|461|
|Tamaño de TempDB (GB)|32|64|96|128|160|192|
|Tipo de almacenamiento|Premium Storage (remoto)|Premium Storage (remoto)|Premium Storage (remoto)|Premium Storage (remoto)|Premium Storage (remoto)|Premium Storage (remoto)|
|Latencia de E/S (aproximada)|5-7 ms (escritura)<br>5-10 ms (lectura)|5-7 ms (escritura)<br>5-10 ms (lectura)|5-7 ms (escritura)<br>5-10 ms (lectura)|5-7 ms (escritura)<br>5-10 ms (lectura)|5-7 ms (escritura)<br>5-10 ms (lectura)|5-7 ms (escritura)<br>5-10 ms (lectura)|
|IOPS de destino (64 KB)|500|1000|1.500|2000|2.500|3000|
|Cantidad máxima de trabajos simultáneos por grupo (solicitudes)* |210|420|630|840|1050|1260|
|Sesiones máximas permitidas|30000|30000|30000|30000|30000|30000|
|Máximo número de bases de datos por grupo|100|200|300|500|500|500|
|Opciones de núcleo virtual mín./máx. de grupos elásticos por base de datos|0, 0.25, 0.5, 1|0, 0.25, 0.5, 1, 2|0, 0.25, 0.5, 1...3|0, 0.25, 0.5, 1...4|0, 0.25, 0.5, 1...5|0, 0.25, 0.5, 1...6|
|Número de réplicas|1|1|1|1|1|1|
|AZ múltiple|N/D|N/D|N/D|N/D|N/D|N/D|
|Escalado horizontal de lectura|N/D|N/D|N/D|N/D|N/D|N/D|
|Almacenamiento de copia de seguridad incluido|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|

\* Para conocer el número máximo actual de trabajos (solicitudes) para cualquier base de datos individual, consulte [Single database resource limits](sql-database-vcore-resource-limits-single-databases.md) (Límites de recursos de bases de datos únicas).

### <a name="general-purpose-service-tier-generation-4-compute-platform-part-2"></a>Nivel de servicio de uso general: Plataforma de procesos de generación 4 (parte 2)

|Tamaño de proceso|BC_Gen4_7|BC_Gen4_8|BC_Gen4_9|BC_Gen4_10|BC_Gen4_16|BC_Gen4_24|
|:--- | --: |--: |--: |--: |--: |--: |
|Generación de H/W|4|4|4|4|4|4|
|Núcleos virtuales|7|8|9|10|16|24|
|Memoria (GB)|49|56|63|70|112|168|
|Compatible con almacén de columnas|SÍ|Sí|Sí|Sí|Sí|SÍ|
|Almacenamiento OLTP en memoria (GB)|N/D|N/D|N/D|N/D|N/D|N/D|
|Tamaño máximo de datos (GB)|1536|2048|2048|2048|3584|4096|
|Tamaño máximo de registro (GB)|461|614|614|614|1075|1229|
|Tamaño de TempDB (GB)|224|256|288|320|384|384|
|Tipo de almacenamiento|Premium Storage (remoto)|Premium Storage (remoto)|Premium Storage (remoto)|Premium Storage (remoto)|Premium Storage (remoto)|Premium Storage (remoto)|
|Latencia de E/S (aproximada)|5-7 ms (escritura)<br>5-10 ms (lectura)|5-7 ms (escritura)<br>5-10 ms (lectura)|5-7 ms (escritura)<br>5-10 ms (lectura)|5-7 ms (escritura)<br>5-10 ms (lectura)|5-7 ms (escritura)<br>5-10 ms (lectura)|5-7 ms (escritura)<br>5-10 ms (lectura)|
|IOPS de destino (64 KB)|3500|4000|4500|5000|7000|7000|
|Cantidad máxima de trabajos simultáneos por grupo (solicitudes)*|1470|1680|1890|2100|3360|5040|
|Sesiones máximas permitidas|30000|30000|30000|30000|30000|30000|
|Máximo número de bases de datos por grupo|200|500|500|500|500|500|
|Opciones de núcleo virtual mín./máx. de grupos elásticos por base de datos|0, 0.25, 0.5, 1...7|0, 0.25, 0.5, 1...8|0, 0.25, 0.5, 1...9|0, 0.25, 0.5, 1...10|0, 0.25, 0.5, 1...10, 16|0, 0.25, 0.5, 1...10, 16, 24|
|Número de réplicas|1|1|1|1|1|1|
|AZ múltiple|N/D|N/D|N/D|N/D|N/D|N/D|
|Escalado horizontal de lectura|N/D|N/D|N/D|N/D|N/D|N/D|
|Almacenamiento de copia de seguridad incluido|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|

\* Para conocer el número máximo actual de trabajos (solicitudes) para cualquier base de datos individual, consulte [Single database resource limits](sql-database-vcore-resource-limits-single-databases.md) (Límites de recursos de bases de datos únicas).

### <a name="general-purpose-service-tier-generation-5-compute-platform-part-1"></a>Nivel de servicio de uso general: Plataforma de procesos de generación 5 (parte 1)

|Tamaño de proceso|GP_Gen5_2|GP_Gen5_4|GP_Gen5_6|GP_Gen5_8|GP_Gen5_10|GP_Gen5_12|GP_Gen5_14|
|:--- | --: |--: |--: |--: |---: | --: |--: |--: |
|Generación de H/W|5|5|5|5|5|5|5|
|Núcleos virtuales|2|4|6|8|10|12|14|
|Memoria (GB)|10.2|20.4|30.6|40.8|51|61.2|71.4|
|Compatible con almacén de columnas|SÍ|Sí|Sí|Sí|Sí|Sí|SÍ|
|Almacenamiento OLTP en memoria (GB)|N/D|N/D|N/D|N/D|N/D|N/D|N/D|
|Tamaño máximo de datos (GB)|512|756|756|1536|1536|1536|
|Tamaño máximo de registro (GB)|154|227|227|461|461|461|461|
|Tamaño de TempDB (GB)|64|128|192|256|320|384|384|
|Tipo de almacenamiento|Premium Storage (remoto)|Premium Storage (remoto)|Premium Storage (remoto)|Premium Storage (remoto)|Premium Storage (remoto)|Premium Storage (remoto)|Premium Storage (remoto)|
|Latencia de E/S (aproximada)|5-7 ms (escritura)<br>5-10 ms (lectura)|5-7 ms (escritura)<br>5-10 ms (lectura)|5-7 ms (escritura)<br>5-10 ms (lectura)|5-7 ms (escritura)<br>5-10 ms (lectura)|5-7 ms (escritura)<br>5-10 ms (lectura)|5-7 ms (escritura)<br>5-10 ms (lectura)|5-7 ms (escritura)<br>5-10 ms (lectura)|
|IOPS de destino (64 KB)|500|1000|1.500|2000|2.500|3000|3500|
|Cantidad máxima de trabajos simultáneos por grupo (solicitudes)*|210|420|630|840|1050|1260|1470|
|Sesiones máximas permitidas|30000|30000|30000|30000|30000|30000|30000|
|Máximo número de bases de datos por grupo|200|500|500|500|500|500|500|
|Opciones de núcleo virtual mín./máx. de grupos elásticos por base de datos|0, 0.25, 0.5, 1, 2|0, 0.25, 0.5, 1...4|0, 0.25, 0.5, 1...6|0, 0.25, 0.5, 1...8|0, 0.25, 0.5, 1...10|0, 0.25, 0.5, 1...12|0, 0.25, 0.5, 1...14|
|Número de réplicas|1|1|1|1|1|1|1|
|AZ múltiple|N/D|N/D|N/D|N/D|N/D|N/D|N/D|
|Escalado horizontal de lectura|N/D|N/D|N/D|N/D|N/D|N/D|N/D|
|Almacenamiento de copia de seguridad incluido|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|

\* Para conocer el número máximo actual de trabajos (solicitudes) para cualquier base de datos individual, consulte [Single database resource limits](sql-database-vcore-resource-limits-single-databases.md) (Límites de recursos de bases de datos únicas).

### <a name="general-purpose-service-tier-generation-5-compute-platform-part-2"></a>Nivel de servicio de uso general: Plataforma de procesos de generación 5 (parte 2)

|Tamaño de proceso|GP_Gen5_16|GP_Gen5_18|GP_Gen5_20|GP_Gen5_24|GP_Gen5_32|GP_Gen5_40|GP_Gen5_80|
|:--- | --: |--: |--: |--: |---: | --: |--: |--: |
|Generación de H/W|5|5|5|5|5|5|5|
|Núcleos virtuales|16|18|20|24|32|40|80|
|Memoria (GB)|81.6|91.8|102|122.4|163.2|204|408|
|Compatible con almacén de columnas|SÍ|Sí|Sí|Sí|Sí|Sí|SÍ|
|Almacenamiento OLTP en memoria (GB)|N/D|N/D|N/D|N/D|N/D|N/D|N/D|
|Tamaño máximo de datos (GB)|2048|2048|3072|3072|4096|4096|4096|
|Tamaño máximo de registro (GB)|614|614|922|922|1229|1229|1229|
|Tamaño de TempDB (GB)|384|384|384|384|384|384|384|
|Tipo de almacenamiento|Premium Storage (remoto)|Premium Storage (remoto)|Premium Storage (remoto)|Premium Storage (remoto)|Premium Storage (remoto)|Premium Storage (remoto)|Premium Storage (remoto)|
|Latencia de E/S (aproximada)|5-7 ms (escritura)<br>5-10 ms (lectura)|5-7 ms (escritura)<br>5-10 ms (lectura)|5-7 ms (escritura)<br>5-10 ms (lectura)|5-7 ms (escritura)<br>5-10 ms (lectura)|5-7 ms (escritura)<br>5-10 ms (lectura)|5-7 ms (escritura)<br>5-10 ms (lectura)|5-7 ms (escritura)<br>5-10 ms (lectura)|
|IOPS de destino (64 KB)|4000|4500|5000|6000|7000|7000|7000|
|Cantidad máxima de trabajos simultáneos por grupo (solicitudes)*|1680|1890|2100|2520|33600|4200|8400|
|Máximo número de bases de datos por grupo|500|500|500|500|500|500|500|
|Opciones de núcleo virtual mín./máx. de grupos elásticos por base de datos|0, 0.25, 0.5, 1...16|0, 0.25, 0.5, 1...18|0, 0.25, 0.5, 1...20|0, 0.25, 0.5, 1...20, 24|0, 0.25, 0.5, 1...20, 24, 32|0, 0.25, 0.5, 1...16, 24, 32, 40|0, 0.25, 0.5, 1...16, 24, 32, 40, 80|
|Número de réplicas|1|1|1|1|1|1|1|
|AZ múltiple|N/D|N/D|N/D|N/D|N/D|N/D|N/D|
|Escalado horizontal de lectura|N/D|N/D|N/D|N/D|N/D|N/D|N/D|
|Almacenamiento de copia de seguridad incluido|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|

\* Para conocer el número máximo actual de trabajos (solicitudes) para cualquier base de datos individual, consulte [Single database resource limits](sql-database-vcore-resource-limits-single-databases.md) (Límites de recursos de bases de datos únicas).

## <a name="business-critical-service-tier-storage-sizes-and-compute-sizes"></a>Nivel de servicio Crítico para la empresa: tamaños de almacenamiento y de proceso

### <a name="business-critical-service-tier-generation-4-compute-platform-part-1"></a>Nivel de servicio Crítico para la empresa: Plataforma de procesos de generación 4 (parte 1)

|Tamaño de proceso|BC_Gen4_1|BC_Gen4_2|BC_Gen4_3|BC_Gen4_4|BC_Gen4_5|BC_Gen4_6|
|:--- | --: |--: |--: |--: |--: |--: |
|Generación de H/W|4|4|4|4|4|4|
|Núcleos virtuales|1|2|3|4|5|6|
|Memoria (GB)|10.2|20.4|30.6|40.8|51|61.2|71.4|
|Compatible con almacén de columnas|N/D|N/D|N/D|N/D|N/D|N/D|
|Almacenamiento OLTP en memoria (GB)|1|2|3|4|5|6|
|Tipo de almacenamiento|SSD local|SSD local|SSD local|SSD local|SSD local|SSD local|
|Tamaño máximo de datos (GB)|1024|1024|1024|1024|1024|1024|
|Tamaño máximo de registro (GB)|307|307|307|307|307|307|
|Tamaño de TempDB (GB)|32|64|96|128|160|192|
|Latencia de E/S (aproximada)|1-2 ms (escritura)<br>1-2 ms (lectura)|1-2 ms (escritura)<br>1-2 ms (lectura)|1-2 ms (escritura)<br>1-2 ms (lectura)|1-2 ms (escritura)<br>1-2 ms (lectura)|1-2 ms (escritura)<br>1-2 ms (lectura)|1-2 ms (escritura)<br>1-2 ms (lectura)|
|IOPS de destino (64 KB)|5000|10000|15000|20000|25000|30000|
|Cantidad máxima de trabajos simultáneos por grupo (solicitudes)*|210|420|630|840|1050|1260|
|Sesiones máximas permitidas|30000|30000|30000|30000|30000|30000|
|Máximo número de bases de datos por grupo|Solo se admiten las bases de datos únicas con este tamaño de proceso|50|100|100|100|100|
|Opciones de núcleo virtual mín./máx. de grupos elásticos por base de datos|N/D|0, 0.25, 0.5, 1, 2|0, 0.25, 0.5, 1...3|0, 0.25, 0.5, 1...4|0, 0.25, 0.5, 1...5|0, 0.25, 0.5, 1...6|
|Número de réplicas|4|4|4|4|4|4|
|AZ múltiple|SÍ|Sí|Sí|Sí|Sí|SÍ|
|Escalado horizontal de lectura|SÍ|Sí|Sí|Sí|Sí|SÍ|
|Almacenamiento de copia de seguridad incluido|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|

\* Para conocer el número máximo actual de trabajos (solicitudes) para cualquier base de datos individual, consulte [Single database resource limits](sql-database-vcore-resource-limits-single-databases.md) (Límites de recursos de bases de datos únicas).

### <a name="business-critical-service-tier-generation-4-compute-platform-part-2"></a>Nivel de servicio Crítico para la empresa: Plataforma de procesos de generación 4 (parte 2)

|Tamaño de proceso|BC_Gen4_7|BC_Gen4_8|BC_Gen4_9|BC_Gen4_10|BC_Gen4_16|BC_Gen4_24|
|:--- | --: |--: |--: |--: |--: |--: |
|Generación de H/W|4|4|4|4|4|4|
|Núcleos virtuales|7|8|9|10|16|24|
|Memoria (GB)|81.6|91.8|102|122.4|163.2|204|408|
|Compatible con almacén de columnas|N/D|N/D|N/D|N/D|N/D|N/D|
|Almacenamiento OLTP en memoria (GB)|7|8|9.5|11|20|36|
|Tipo de almacenamiento|SSD local|SSD local|SSD local|SSD local|SSD local|SSD local|
|Tamaño máximo de datos (GB)|1024|1024|1024|1024|1024|1024|
|Tamaño máximo de registro (GB)|307|307|307|307|307|307|
|Tamaño de TempDB (GB)|224|256|288|320|384|384|
|Latencia de E/S (aproximada)|1-2 ms (escritura)<br>1-2 ms (lectura)|1-2 ms (escritura)<br>1-2 ms (lectura)|1-2 ms (escritura)<br>1-2 ms (lectura)|1-2 ms (escritura)<br>1-2 ms (lectura)|1-2 ms (escritura)<br>1-2 ms (lectura)|1-2 ms (escritura)<br>1-2 ms (lectura)|
|IOPS de destino (64 KB)|35000|40000|45000|50000|80000|120000|
|Cantidad máxima de trabajos simultáneos por grupo (solicitudes)*|1470|1680|1890|2100|3360|5040|
|Sesiones máximas permitidas|30000|30000|30000|30000|30000|30000|
|Máximo número de bases de datos por grupo|100|100|100|100|100|100|
|Opciones de núcleo virtual mín./máx. de grupos elásticos por base de datos|0, 0.25, 0.5, 1...7|0, 0.25, 0.5, 1...8|0, 0.25, 0.5, 1...9|0, 0.25, 0.5, 1...10|0, 0.25, 0.5, 1...10, 16|0, 0.25, 0.5, 1...10, 16, 24|
|Número de réplicas|4|4|4|4|4|4|
|AZ múltiple|SÍ|Sí|Sí|Sí|Sí|SÍ|
|Escalado horizontal de lectura|SÍ|Sí|Sí|Sí|Sí|SÍ|
|Almacenamiento de copia de seguridad incluido|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|

\* Para conocer el número máximo actual de trabajos (solicitudes) para cualquier base de datos individual, consulte [Single database resource limits](sql-database-vcore-resource-limits-single-databases.md) (Límites de recursos de bases de datos únicas).

#### <a name="business-critical-service-tier-generation-5-compute-platform-part-1"></a>Nivel de servicio Crítico para la empresa: Plataforma de procesos de generación 5 (parte 1)

|Tamaño de proceso|BC_Gen5_2|BC_Gen5_4|BC_Gen5_6|BC_Gen5_8|BC_Gen5_10|BC_Gen5_12|BC_Gen5_14|
|:--- | --: |--: |--: |--: |---: | --: |--: |--: |--: |--: |--: |--: |
|Generación de H/W|5|5|5|5|5|5|5|
|Núcleos virtuales|2|4|6|8|10|12|14|
|Memoria (GB)|11|22|33|44|55|66|77|
|Compatible con almacén de columnas|SÍ|Sí|Sí|Sí|Sí|Sí|SÍ|
|Almacenamiento OLTP en memoria (GB)|1571|3142|4.713|6284|8.655|11.026|13.397|
|Tamaño máximo de datos (GB)|1024|1024|1024|1536|1536|1536|1536|
|Tamaño máximo de registro (GB)|307|307|307|461|461|461|461|
|Tamaño de TempDB (GB)|64|128|192|256|320|384|384|
|Tipo de almacenamiento|SSD local|SSD local|SSD local|SSD local|SSD local|SSD local|SSD local|
|Latencia de E/S (aproximada)|1-2 ms (escritura)<br>1-2 ms (lectura)|1-2 ms (escritura)<br>1-2 ms (lectura)|1-2 ms (escritura)<br>1-2 ms (lectura)|1-2 ms (escritura)<br>1-2 ms (lectura)|1-2 ms (escritura)<br>1-2 ms (lectura)|1-2 ms (escritura)<br>1-2 ms (lectura)|1-2 ms (escritura)<br>1-2 ms (lectura)|
|IOPS de destino (64 KB)|5000|10000|15000|20000|25000|30000|35000|
|Cantidad máxima de trabajos simultáneos por grupo (solicitudes)*|210|420|630|840|1050|1260|1470|
|Sesiones máximas permitidas|30000|30000|30000|30000|30000|30000|30000|
|Máximo número de bases de datos por grupo|Solo se admiten las bases de datos únicas con este tamaño de proceso|50|100|100|100|100|100|
|Opciones de núcleo virtual mín./máx. de grupos elásticos por base de datos|N/D|0, 0.25, 0.5, 1...4|0, 0.25, 0.5, 1...6|0, 0.25, 0.5, 1...8|0, 0.25, 0.5, 1...10|0, 0.25, 0.5, 1...12|0, 0.25, 0.5, 1...14|
|Número de réplicas|4|4|4|4|4|4|4|
|AZ múltiple|SÍ|Sí|Sí|Sí|Sí|SÍ|
|Escalado horizontal de lectura|SÍ|Sí|Sí|Sí|Sí|Sí|SÍ|
|Almacenamiento de copia de seguridad incluido|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|

\* Para conocer el número máximo actual de trabajos (solicitudes) para cualquier base de datos individual, consulte [Single database resource limits](sql-database-vcore-resource-limits-single-databases.md) (Límites de recursos de bases de datos únicas).

#### <a name="business-critical-service-tier-generation-5-compute-platform-part-2"></a>Nivel de servicio Crítico para la empresa: Plataforma de procesos de generación 5 (parte 2)

|Tamaño de proceso|BC_Gen5_16|BC_Gen5_18|BC_Gen5_20|BC_Gen5_24|BC_Gen5_32|BC_Gen5_40|BC_Gen5_80|
|:--- | --: |--: |--: |--: |---: | --: |--: |--: |--: |--: |--: |--: |
|Generación de H/W|5|5|5|5|5|5|5|
|Núcleos virtuales|16|18|20|24|32|40|80|
|Memoria (GB)|81.6|91.8|102|122.4|163.2|204|408|
|Compatible con almacén de columnas|SÍ|Sí|Sí|Sí|Sí|Sí|SÍ|
|Almacenamiento OLTP en memoria (GB)|15 768|18.139|20.51|25 252|37 936|52.22|131,64|
|Tamaño máximo de datos (GB)|3072|3072|3072|4096|4096|4096|4096|
|Tamaño máximo de registro (GB)|922|922|922|1229|1229|1229|1229|
|Tamaño de TempDB (GB)|384|384|384|384|384|384|384|
|Tipo de almacenamiento|SSD local|SSD local|SSD local|SSD local|SSD local|SSD local|SSD local|
|Latencia de E/S (aproximada)|1-2 ms (escritura)<br>1-2 ms (lectura)|1-2 ms (escritura)<br>1-2 ms (lectura)|1-2 ms (escritura)<br>1-2 ms (lectura)|1-2 ms (escritura)<br>1-2 ms (lectura)|1-2 ms (escritura)<br>1-2 ms (lectura)|1-2 ms (escritura)<br>1-2 ms (lectura)|1-2 ms (escritura)<br>1-2 ms (lectura)|
|IOPS de destino (64 KB)|40000|45000|50000|60000|80000|100000|200000|
|Cantidad máxima de trabajos simultáneos por grupo (solicitudes)*|1680|1890|2100|2520|3360|4200|8400|
|Sesiones máximas permitidas|30000|30000|30000|30000|30000|30000|30000|
|Máximo número de bases de datos por grupo|100|100|100|100|100|100|100|
|Opciones de núcleo virtual mín./máx. de grupos elásticos por base de datos|0, 0.25, 0.5, 1...16|0, 0.25, 0.5, 1...18|0, 0.25, 0.5, 1...20|0, 0.25, 0.5, 1...20, 24|0, 0.25, 0.5, 1...20, 24, 32|0, 0.25, 0.5, 1...20, 24, 32, 40|0, 0.25, 0.5, 1...20, 24, 32, 40, 80|
|Número de réplicas|4|4|4|4|4|4|4|
|AZ múltiple|SÍ|Sí|Sí|Sí|Sí|SÍ|
|Escalado horizontal de lectura|SÍ|Sí|Sí|Sí|Sí|Sí|SÍ|
|Almacenamiento de copia de seguridad incluido|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|

\* Para conocer el número máximo actual de trabajos (solicitudes) para cualquier base de datos individual, consulte [Single database resource limits](sql-database-vcore-resource-limits-single-databases.md) (Límites de recursos de bases de datos únicas).

Si todos los núcleos virtuales de un grupo elástico están ocupados, cada una de las bases de datos del grupo recibe la misma cantidad de recursos de proceso para procesar las consultas. El servicio SQL Database proporciona ecuanimidad de uso compartido de recursos entre bases de datos garantizando los mismos segmentos de tiempo de proceso. La ecuanimidad de uso compartido de recursos del grupo elástico es adicional a cualquier cantidad de recursos garantizados de otro modo a cada base de datos cuando el número mínimo de núcleos virtuales por base de datos se establece en un valor distinto de cero.

## <a name="database-properties-for-pooled-databases"></a>Propiedades de base de datos para bases de datos agrupadas

En la tabla siguiente se describen las propiedades de las bases de datos agrupadas.

| Propiedad | DESCRIPCIÓN |
|:--- |:--- |
| Núcleos virtuales máximos por base de datos |El número máximo de núcleos virtuales que puede usar cualquier base de datos del grupo, si está disponible según el uso que hacen otras bases de datos del grupo. El número de núcleos virtuales por base de datos no garantiza la disponibilidad de recursos para una base de datos. Se trata de una configuración global que se aplica a todas las bases de datos del grupo. Establezca un número de núcleos virtuales por base de datos lo suficientemente alto como para gestionar los picos de uso de la base de datos. Se admite cierto grado de exceso de asignación de recursos, ya que el grupo suele basarse en patrones de uso en frío y caliente de las bases de datos, cuando en realidad los picos de uso no tienen lugar en todas las bases de datos a la vez.|
| Número mínimo de núcleos virtuales por base de datos |El número mínimo de núcleos virtuales que se garantiza en cualquier base de datos del grupo. Se trata de una configuración global que se aplica a todas las bases de datos del grupo. El número mínimo de núcleos virtuales por base de datos se puede establecer en 0, y también se trata del valor predeterminado. Esta propiedad se establece en cualquier valor entre 0 y el uso medio de núcleos virtuales por base de datos. El resultado de multiplicar el número de bases de datos del grupo y el número mínimo de núcleos virtuales por base de datos no puede superar el número de núcleos virtuales por grupo.|
| Almacenamiento máximo por base de datos |El tamaño máximo de base de datos establecido por el usuario para una base de datos de un grupo. Las bases de datos agrupadas comparten el almacenamiento del grupo asignado, de modo que el tamaño que puede alcanzar una base de datos se limita a la menor cantidad de almacenamiento restante del grupo y el tamaño de la base de datos. El tamaño máximo de la base de datos hace referencia al tamaño máximo de los archivos de datos, y no incluye el espacio utilizado por los archivos de registro. |
|||

## <a name="next-steps"></a>Pasos siguientes

- Consulte [Preguntas más frecuentes sobre SQL Database](sql-database-faq.md) para conocer las respuestas a las preguntas más frecuentes.
- Consulte [Overview of resource limits on a SQL Database server](sql-database-resource-limits-database-server.md) (Introducción a los límites de recursos de un servidor de SQL Database) para obtener información acerca de los límites en los niveles de servidor y suscripción.
- Para más información sobre los límites generales de Azure, consulte [Límites, cuotas y restricciones de suscripción y servicios de Microsoft Azure](../azure-subscription-service-limits.md).
