---
title: Límites de recursos basados en núcleos virtuales de Azure SQL Database para grupos elásticos | Microsoft Docs
description: En esta página se describen algunos límites de recursos basados en núcleos virtuales comunes para grupos elásticos en Azure SQL Database.
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: DBs & servers
ms.topic: conceptual
ms.date: 08/01/2018
ms.author: carlrab
ms.openlocfilehash: 5a7e00e84e5165296bcad83c515fc2af315954be
ms.sourcegitcommit: 96f498de91984321614f09d796ca88887c4bd2fb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/02/2018
ms.locfileid: "39414449"
---
# <a name="azure-sql-database-vcore-based-purchasing-model-limits-for-elastic-pools"></a>Límites del modelo de compra basado en núcleos virtuales de Azure SQL Database para grupos elásticos

En este artículo se proporcionan los límites de recursos detallados para grupos elásticos y bases de datos agrupadas de Azure SQL Database que utilizan el modelo de compra basado en núcleos virtuales.

Para información sobre los límites del modelo de compra basado en DTU, consulte [SQL Database DTU-based resource limits - elastic pools](sql-database-dtu-resource-limits-elastic-pools.md) (Límites de recursos basados en DTU de SQL Database para grupos elásticos).

> [!IMPORTANT]
> En algunas circunstancias, puede que deba reducir una base de datos para reclamar el espacio no utilizado. Para más información, consulte [Administración del espacio de archivo en Azure SQL Database](sql-database-file-space-management.md).

## <a name="elastic-pool-storage-sizes-and-performance-levels"></a>Grupo de bases de datos elásticas: tamaños de almacenamiento y niveles de rendimiento

Para los grupos elásticos de SQL Database, las siguientes tablas muestran los recursos disponibles en cada nivel de servicio y nivel de rendimiento. Puede establecer el nivel de servicio, el nivel de rendimiento y la cantidad de almacenamiento mediante [Azure Portal](sql-database-elastic-pool-manage.md#azure-portal-manage-elastic-pools-and-pooled-databases), [PowerShell](sql-database-elastic-pool-manage.md#powershell-manage-elastic-pools-and-pooled-databases), la [CLI de Azure](sql-database-elastic-pool-manage.md#azure-cli-manage-elastic-pools-and-pooled-databases) o la [API de REST](sql-database-elastic-pool-manage.md#rest-api-manage-elastic-pools-and-pooled-databases).

> [!NOTE]
> Los límites de recursos de las bases de datos individuales de los grupos elásticos suelen ser los mismos que los de las bases de datos únicas fuera de los grupos que tienen el mismo nivel de rendimiento. Por ejemplo, el número máximo de trabajos simultáneos en una base de datos GP_Gen4_1 es 200 trabajos. Por lo tanto, el número máximo de trabajos simultáneos en una base de datos de un grupo GP_Gen4_1 también es 200 trabajos. Tenga en cuenta que el número total de trabajos simultáneos en el grupo GP_Gen4_1 es 210.

### <a name="general-purpose-service-tier"></a>Nivel de servicio de uso general

#### <a name="generation-4-compute-platform"></a>Plataforma de procesos de generación 4
|Nivel de rendimiento|GP_Gen4_1|GP_Gen4_2|GP_Gen4_4|GP_Gen4_8|GP_Gen4_16|GP_Gen4_24|
|:--- | --: |--: |--: |--: |--: |--: |
|Generación de H/W|4|4|4|4|4|4|
|Núcleos virtuales|1|2|4|8|16|24|
|Memoria (GB)|7|14|28|56|112|168|
|Compatible con almacén de columnas|SÍ|Sí|Sí|Sí|Sí|SÍ|
|Almacenamiento OLTP en memoria (GB)|N/D|N/D|N/D|N/D|N/D|N/D|
|Tipo de almacenamiento|Premium Storage (remoto)|Premium Storage (remoto)|Premium Storage (remoto)|Premium Storage (remoto)|Premium Storage (remoto)|Premium Storage (remoto)|
|Tamaño máximo de datos (GB)|512|756|1536|2048|3584|4096|
|Tamaño máximo de registro|154|227|461|614|1075|1229|
|Tamaño de TempDB (DB)|32|64|128|256|384|384|
|IOPS de destino (64 KB)|500|1000|2000|4000|7000|7000|
|Latencia de E/S (aproximada)|5-7 ms (escritura)<br>5-10 ms (lectura)|5-7 ms (escritura)<br>5-10 ms (lectura)|5-7 ms (escritura)<br>5-10 ms (lectura)|5-7 ms (escritura)<br>5-10 ms (lectura)|5-7 ms (escritura)<br>5-10 ms (lectura)|5-7 ms (escritura)<br>5-10 ms (lectura)|5-7 ms (escritura)<br>5-10 ms (lectura)|
|Cantidad máxima de trabajos (solicitudes) simultáneos|210|420|840|1680|3360|5040|
|Sesiones máximas permitidas|30000|30000|30000|30000|30000|30000|
|Densidad máxima del grupo|100|200|500|500|500|500|
|Número mín./máx. de clics para detener del grupo elástico|0, 0.25, 0.5, 1|0, 0.25, 0.5, 1, 2|0, 0.25, 0.5, 1, 2, 4|0, 0.25, 0.5, 1, 2, 4, 8|0, 0.25, 0.5, 1, 2, 4, 8, 16|0, 0.25, 0.5, 1, 2, 4, 8, 16, 24|
|Número de réplicas|1|1|1|1|1|1|
|AZ múltiple|N/D|N/D|N/D|N/D|N/D|N/D|
|Escalado horizontal de lectura|N/D|N/D|N/D|N/D|N/D|N/D|
|Almacenamiento de copia de seguridad incluido|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|
|||

#### <a name="generation-5-compute-platform"></a>Plataforma de procesos de generación 5
|Nivel de rendimiento|GP_Gen5_2|GP_Gen5_4|GP_Gen5_8|GP_Gen5_16|GP_Gen5_24|GP_Gen5_32|GP_Gen5_40|GP_Gen5_80|
|:--- | --: |--: |--: |--: |--: |--: |--: |--: |
|Generación de H/W|5|5|5|5|5|5|5|5|
|Núcleos virtuales|2|4|8|16|24|32|40|80|
|Memoria (GB)|11|22|44|88|132|176|220|440|
|Compatible con almacén de columnas|SÍ|Sí|Sí|Sí|Sí|Sí|Sí|SÍ|
|Almacenamiento OLTP en memoria (GB)|N/D|N/D|N/D|N/D|N/D|N/D|N/D|N/D|
|Tipo de almacenamiento|Premium Storage (remoto)|Premium Storage (remoto)|Premium Storage (remoto)|Premium Storage (remoto)|Premium Storage (remoto)|Premium Storage (remoto)|Premium Storage (remoto)|Premium Storage (remoto)|
|Tamaño máximo de datos (GB)|512|756|1536|2048|3072|4096|4096|4096|
|Tamaño máximo de registro|154|227|461|614|922|1229|1229|1229|
|Tamaño de TempDB (DB)|64|128|256|384|384|384|384|384|
|IOPS de destino (64 KB)|500|1000|2000|4000|6000|7000|7000|7000|
|Latencia de E/S (aproximada)|5-7 ms (escritura)<br>5-10 ms (lectura)|5-7 ms (escritura)<br>5-10 ms (lectura)|5-7 ms (escritura)<br>5-10 ms (lectura)|5-7 ms (escritura)<br>5-10 ms (lectura)|5-7 ms (escritura)<br>5-10 ms (lectura)|5-7 ms (escritura)<br>5-10 ms (lectura)|5-7 ms (escritura)<br>5-10 ms (lectura)|5-7 ms (escritura)<br>5-10 ms (lectura)|5-7 ms (escritura)<br>5-10 ms (lectura)|
|Cantidad máxima de trabajos (solicitudes) simultáneos|210|420|840|1680|2520|3360|4200|8400
|Sesiones máximas permitidas|30000|30000|30000|30000|30000|30000|30000|30000|
|Densidad máxima del grupo|100|200|500|500|500|500|500|500|
|Número mín./máx. de clics para detener del grupo elástico|0, 0.25, 0.5, 1, 2|0, 0.25, 0.5, 1, 2, 4|0, 0.25, 0.5, 1, 2, 4, 8|0, 0.25, 0.5, 1, 2, 4, 8, 16|0, 0.25, 0.5, 1, 2, 4, 8, 16, 24|0, 0.5, 1, 2, 4, 8, 16, 24, 32|0, 0.5, 1, 2, 4, 8, 16, 24, 32, 40|0, 0.5, 1, 2, 4, 8, 16, 24, 32, 40, 80|
|Número de réplicas|1|1|1|1|1|1|1|1|
|AZ múltiple|N/D|N/D|N/D|N/D|N/D|N/D|N/D|N/D|
|Escalado horizontal de lectura|N/D|N/D|N/D|N/D|N/D|N/D|N/D|N/D|
|Almacenamiento de copia de seguridad incluido|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|
|||

### <a name="business-critical-service-tier"></a>Nivel de servicio Crítico para la empresa

#### <a name="generation-4-compute-platform"></a>Plataforma de procesos de generación 4
|Nivel de rendimiento|BC_Gen4_1|BC_Gen4_2|BC_Gen4_4|BC_Gen4_8|BC_Gen4_16|BC_Gen4_24|
|:--- | --: |--: |--: |--: |--: |--: |
|Generación de H/W|4|4|4|4|4|4|
|Núcleos virtuales|1|2|4|8|16|24|
|Memoria (GB)|7|14|28|56|112|168|
|Compatible con almacén de columnas|SÍ|Sí|Sí|Sí|Sí|SÍ|
|Almacenamiento OLTP en memoria (GB)|1|2|4|8|20|36|
|Tipo de almacenamiento|SSD local|SSD local|SSD local|SSD local|SSD local|SSD local|
|Tamaño máximo de datos (GB)|1024|1024|1024|1024|1024|1024|
|Tamaño máximo de registro|307|307|307|307|307|307|
|Tamaño de TempDB (DB)|32|64|128|256|384|384|
|IOPS de destino (64 KB)|5000|10000|20000|40000|80000|120000|
|Latencia de E/S (aproximada)|1-2 ms (escritura)<br>1-2 ms (lectura)|1-2 ms (escritura)<br>1-2 ms (lectura)|1-2 ms (escritura)<br>1-2 ms (lectura)|1-2 ms (escritura)<br>1-2 ms (lectura)|1-2 ms (escritura)<br>1-2 ms (lectura)|1-2 ms (escritura)<br>1-2 ms (lectura)|
|Cantidad máxima de trabajos (solicitudes) simultáneos|210|420|840|1680|3360|5040|
|Sesiones máximas permitidas|30000|30000|30000|30000|30000|30000|
|Densidad máxima del grupo|N/D|50|100|100|100|100|
|Número mín./máx. de clics para detener del grupo elástico|N/D|0, 0.25, 0.5, 1, 2|0, 0.25, 0.5, 1, 2, 4|0, 0.25, 0.5, 1, 2, 4, 8|0, 0.25, 0.5, 1, 2, 4, 8, 16|0, 0.25, 0.5, 1, 2, 4, 8, 16, 24|
|Número de réplicas|3|3|3|3|3|3|
|AZ múltiple|N/D|N/D|N/D|N/D|N/D|N/D|
|Escalado horizontal de lectura|SÍ|Sí|Sí|Sí|Sí|SÍ|
|Almacenamiento de copia de seguridad incluido|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|
|||

#### <a name="generation-5-compute-platform"></a>Plataforma de procesos de generación 5
|Nivel de rendimiento|BC_Gen5_2|BC_Gen5_4|BC_Gen5_8|BC_Gen5_16|BC_Gen5_24|BC_Gen5_32|BC_Gen5_40|BC_Gen5_80|
|:--- | --: |--: |--: |--: |--: |--: |--: |--: |
|Generación de H/W|5|5|5|5|5|5|5|5|
|Núcleos virtuales|2|4|8|16|24|32|40|80|
|Memoria (GB)|11|22|44|88|132|176|220|440|
|Compatible con almacén de columnas|SÍ|Sí|Sí|Sí|Sí|Sí|Sí|SÍ|
|Almacenamiento OLTP en memoria (GB)|1571|3142|6284|15 768|25 252|37 936|52.22|131,64|
|Tipo de almacenamiento|SSD local|SSD local|SSD local|SSD local|SSD local|SSD local|SSD local|SSD local|
|Latencia de E/S (aproximada)|1-2 ms (escritura)<br>1-2 ms (lectura)|1-2 ms (escritura)<br>1-2 ms (lectura)|1-2 ms (escritura)<br>1-2 ms (lectura)|1-2 ms (escritura)<br>1-2 ms (lectura)|1-2 ms (escritura)<br>1-2 ms (lectura)|1-2 ms (escritura)<br>1-2 ms (lectura)|1-2 ms (escritura)<br>1-2 ms (lectura)|1-2 ms (escritura)<br>1-2 ms (lectura)|
|Tamaño máximo de datos (GB)|1024|1024|1024|1024|2048|4096|4096|4096|
|Tamaño máximo de registro|307|307|307|307|614|1229|1229|1229|
|Tamaño de TempDB (DB)|64|128|256|384|384|384|384|384|
|IOPS de destino (64 KB)|5000|10000|20000|40000|60000|80000|100000|200000
|Cantidad máxima de trabajos (solicitudes) simultáneos|210|420|840|1680|2520|3360|5040|8400|
|Sesiones máximas permitidas|30000|30000|30000|30000|30000|30000|30000|30000|
|Densidad máxima del grupo|N/D|50|100|100|100|100|100|100|
|Número mín./máx. de clics para detener del grupo elástico|N/D|0, 0.25, 0.5, 1, 2, 4|0, 0.25, 0.5, 1, 2, 4, 8|0, 0.25, 0.5, 1, 2, 4, 8, 16|0, 0.25, 0.5, 1, 2, 4, 8, 16, 24|0, 0.5, 1, 2, 4, 8, 16, 24, 32|0, 0.5, 1, 2, 4, 8, 16, 24, 32, 40|0, 0.5, 1, 2, 4, 8, 16, 24, 32, 40, 80|
|Número de réplicas|3|3|3|3|3|3|3|3|
|AZ múltiple|N/D|N/D|N/D|N/D|N/D|N/D|N/D|N/D|
|Escalado horizontal de lectura|SÍ|Sí|Sí|Sí|Sí|Sí|Sí|SÍ|
|Almacenamiento de copia de seguridad incluido|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|
|||

Si todos los núcleos virtuales de un grupo elástico están ocupados, cada una de las bases de datos del grupo recibe la misma cantidad de recursos de proceso para procesar las consultas. El servicio SQL Database proporciona ecuanimidad de uso compartido de recursos entre bases de datos garantizando los mismos segmentos de tiempo de proceso. La ecuanimidad de uso compartido de recursos del grupo elástico es adicional a cualquier cantidad de recursos garantizados de otro modo a cada base de datos cuando el número mínimo de núcleos virtuales por base de datos se establece en un valor distinto de cero.

### <a name="database-properties-for-pooled-databases"></a>Propiedades de base de datos para bases de datos agrupadas

En la tabla siguiente se describen las propiedades de las bases de datos agrupadas.

| Propiedad | DESCRIPCIÓN |
|:--- |:--- |
| Núcleos virtuales máximos por base de datos |El número máximo de núcleos virtuales que puede usar cualquier base de datos del grupo, si está disponible según el uso que hacen otras bases de datos del grupo. El número de núcleos virtuales por base de datos no garantiza la disponibilidad de recursos para una base de datos. Se trata de una configuración global que se aplica a todas las bases de datos del grupo. Establezca un número de núcleos virtuales por base de datos lo suficientemente alto como para gestionar los picos de uso de la base de datos. Se admite cierto grado de exceso de asignación de recursos, ya que el grupo suele basarse en patrones de uso en frío y caliente de las bases de datos, cuando en realidad los picos de uso no tienen lugar en todas las bases de datos a la vez.|
| Número mínimo de núcleos virtuales por base de datos |El número mínimo de núcleos virtuales que se garantiza en cualquier base de datos del grupo. Se trata de una configuración global que se aplica a todas las bases de datos del grupo. El número mínimo de núcleos virtuales por base de datos se puede establecer en 0, y también se trata del valor predeterminado. Esta propiedad se establece en cualquier valor entre 0 y el uso medio de núcleos virtuales por base de datos. El resultado de multiplicar el número de bases de datos del grupo y el número mínimo de núcleos virtuales por base de datos no puede superar el número de núcleos virtuales por grupo.|
| Almacenamiento máximo por base de datos |El tamaño máximo de base de datos establecido por el usuario para una base de datos de un grupo. Las bases de datos agrupadas comparten el almacenamiento del grupo asignado, de modo que el tamaño que puede alcanzar una base de datos se limita a la menor cantidad de almacenamiento restante del grupo y el tamaño de la base de datos. El tamaño máximo de la base de datos hace referencia al tamaño máximo de los archivos de datos, y no incluye el espacio utilizado por los archivos de registro. |
|||
 
## <a name="next-steps"></a>Pasos siguientes

- Consulte [Preguntas más frecuentes sobre SQL Database](sql-database-faq.md) para conocer las respuestas a las preguntas más frecuentes.
- Para más información sobre los límites generales de Azure, consulte [Límites, cuotas y restricciones de suscripción y servicios de Microsoft Azure](../azure-subscription-service-limits.md).
