---
title: Límites de recursos basados en núcleos virtuales de Azure SQL Database para bases de datos únicas | Microsoft Docs
description: En esta página se describen algunos límites de recursos basados en núcleos virtuales comunes para una base de datos única en Azure SQL Database.
services: sql-database
ms.service: sql-database
ms.subservice: single-database
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: CarlRabeler
ms.author: carlrab
ms.reviewer: ''
manager: craigg
ms.date: 09/14/2018
ms.openlocfilehash: 5f0e5de7503d06d1aff319434d763d3b034053b3
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/25/2018
ms.locfileid: "47166385"
---
# <a name="azure-sql-database-vcore-based-purchasing-model-limits-for-a-single-database"></a>Límites del modelo de compra basado en núcleos virtuales de Azure SQL Database para una base de datos única

En este artículo se proporcionan los límites de recursos detallados para bases de datos únicas de Azure SQL Database que utilizan el modelo de compra basado en núcleos virtuales.

Para información sobre los límites del modelo de compra basado en DTU, consulte [SQL Database DTU-based resource limits](sql-database-dtu-resource-limits.md) (Límites de recursos basados en DTU de SQL Database).

> [!IMPORTANT]
> En algunas circunstancias, puede que deba reducir una base de datos para reclamar el espacio no utilizado. Para más información, consulte [Administración del espacio de archivo en Azure SQL Database](sql-database-file-space-management.md).


## <a name="single-database-storage-sizes-and-compute-sizes"></a>Base de datos única: tamaños de almacenamiento y tamaños de proceso

Para las bases de datos únicas, las siguientes tablas muestran los recursos disponibles para una base de datos única en cada nivel de servicio y tamaño de proceso. Puede establecer el nivel de servicio, el tamaño de proceso y la cantidad de almacenamiento para una base de datos única mediante [Azure Portal](sql-database-single-databases-manage.md#azure-portal-manage-logical-servers-and-databases), [Transact-SQL](sql-database-single-databases-manage.md#transact-sql-manage-logical-servers-and-databases), [PowerShell](sql-database-single-databases-manage.md#powershell-manage-logical-servers-and-databases), la [CLI de Azure](sql-database-single-databases-manage.md#azure-cli-manage-logical-servers-and-databases) o la [API REST](sql-database-single-databases-manage.md#rest-api-manage-logical-servers-and-databases).

### <a name="general-purpose-service-tier"></a>Nivel de servicio de uso general

#### <a name="generation-4-compute-platform"></a>Plataforma de procesos de generación 4
|Tamaño de proceso|GP_Gen4_1|GP_Gen4_2|GP_Gen4_4|GP_Gen4_8|GP_Gen4_16|GP_Gen4_24
|:--- | --: |--: |--: |--: |--: |--: |
|Generación de H/W|4|4|4|4|4|4|
|Núcleos virtuales|1|2|4|8|16|24|
|Memoria (GB)|7|14|28|56|112|168|
|Compatible con almacén de columnas|SÍ|Sí|Sí|Sí|Sí|SÍ|
|Almacenamiento OLTP en memoria (GB)|N/D|N/D|N/D|N/D|N/D|N/D|
|Tipo de almacenamiento|Premium Storage (remoto)|Premium Storage (remoto)|Premium Storage (remoto)|Premium Storage (remoto)|Premium Storage (remoto)|Premium Storage (remoto)|
|Latencia de E/S (aproximada)|5-7 ms (escritura)<br>5-10 ms (lectura)|5-7 ms (escritura)<br>5-10 ms (lectura)|5-7 ms (escritura)<br>5-10 ms (lectura)|5-7 ms (escritura)<br>5-10 ms (lectura)|5-7 ms (escritura)<br>5-10 ms (lectura)|5-7 ms (escritura)<br>5-10 ms (lectura)|
|Tamaño máximo de datos (GB)|1024|1024|1536|3072|4096|4096|
|Tamaño máximo de registro (GB)|307|307|461|922|1229|1229|
|Tamaño de TempDB (GB)|32|64|128|256|384|384|
|IOPS de destino (64 KB)|500|1000|2000|4000|7000|7000|
|Cantidad máxima de trabajos (solicitudes) simultáneos|200|400|800|1600|3200|4800|
|Sesiones máximas permitidas|30000|30000|30000|30000|30000|30000|
|Número de réplicas|1|1|1|1|1|1|
|AZ múltiple|N/D|N/D|N/D|N/D|N/D|N/D|000
|Escalado horizontal de lectura|N/D|N/D|N/D|N/D|N/D|N/D|
|Almacenamiento de copia de seguridad incluido|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|
|||

#### <a name="generation-5-compute-platform"></a>Plataforma de procesos de generación 5
|Tamaño de proceso|GP_Gen5_2|GP_Gen5_4|GP_Gen5_8|GP_Gen5_16|GP_Gen5_24|GP_Gen5_32|GP_Gen5_40| GP_Gen5_80|
|:--- | --: |--: |--: |--: |---: | --: |--: |--: |--: |
|Generación de H/W|5|5|5|5|5|5|5|
|Núcleos virtuales|2|4|8|16|24|32|40|80|
|Memoria (GB)|11|22|44|88|132|176|220|440|
|Compatible con almacén de columnas|SÍ|Sí|Sí|Sí|Sí|Sí|Sí|SÍ|
|Almacenamiento OLTP en memoria (GB)|N/D|N/D|N/D|N/D|N/D|N/D|N/D|N/D|
|Tipo de almacenamiento|Premium Storage (remoto)|Premium Storage (remoto)|Premium Storage (remoto)|Premium Storage (remoto)|Premium Storage (remoto)|Premium Storage (remoto)|Premium Storage (remoto)|Premium Storage (remoto)|
|Latencia de E/S (aproximada)|5-7 ms (escritura)<br>5-10 ms (lectura)|5-7 ms (escritura)<br>5-10 ms (lectura)|5-7 ms (escritura)<br>5-10 ms (lectura)|5-7 ms (escritura)<br>5-10 ms (lectura)|5-7 ms (escritura)<br>5-10 ms (lectura)|5-7 ms (escritura)<br>5-10 ms (lectura)|5-7 ms (escritura)<br>5-10 ms (lectura)|5-7 ms (escritura)<br>5-10 ms (lectura)|
|Tamaño máximo de datos (GB)|1024|1024|1536|3072|4096|4096|4096|4096|
|Tamaño máximo de registro (GB)|307|307|461|614|1229|1229|1229|1229|
|Tamaño de TempDB (GB)|64|128|256|384|384|384|384|384|
|IOPS de destino (64 KB)|500|1000|2000|4000|6000|7000|7000|7000|
|Cantidad máxima de trabajos (solicitudes) simultáneos|200|400|800|1600|2400|3200|4000|8000|
|Sesiones máximas permitidas|30000|30000|30000|30000|30000|30000|30000|30000|
|Número de réplicas|1|1|1|1|1|1|1|1|
|AZ múltiple|N/D|N/D|N/D|N/D|N/D|N/D|N/D|N/D|
|Escalado horizontal de lectura|N/D|N/D|N/D|N/D|N/D|N/D|N/D|N/D|
|Almacenamiento de copia de seguridad incluido|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|
|||

### <a name="business-critical-service-tier"></a>Nivel de servicio Crítico para la empresa

#### <a name="generation-4-compute-platform"></a>Plataforma de procesos de generación 4
|Tamaño de proceso|BC_Gen4_1|BC_Gen4_2|BC_Gen4_4|BC_Gen4_8|BC_Gen4_16|BC_Gen4_24|
|:--- | --: |--: |--: |--: |--: |--: |
|Generación de H/W|4|4|4|4|4|4|
|Núcleos virtuales|1|2|4|8|16|24|
|Memoria (GB)|7|14|28|56|112|168|
|Compatible con almacén de columnas|N/D|N/D|N/D|N/D|N/D|N/D|
|Almacenamiento OLTP en memoria (GB)|1|2|4|8|20|36|
|Tipo de almacenamiento|SSD local|SSD local|SSD local|SSD local|SSD local|SSD local|
|Tamaño máximo de datos (GB)|1024|1024|1024|1024|1024|1024|
|Tamaño máximo de registro (GB)|307|307|307|307|307|307|
|Tamaño de TempDB (GB)|32|64|128|256|384|384|
|IOPS de destino (64 KB)|5000|10000|20000|40000|80000|120000|
|Latencia de E/S (aproximada)|1-2 ms (escritura)<br>1-2 ms (lectura)|1-2 ms (escritura)<br>1-2 ms (lectura)|1-2 ms (escritura)<br>1-2 ms (lectura)|1-2 ms (escritura)<br>1-2 ms (lectura)|1-2 ms (escritura)<br>1-2 ms (lectura)|1-2 ms (escritura)<br>1-2 ms (lectura)|
|Cantidad máxima de trabajos (solicitudes) simultáneos|200|400|800|1600|3200|4800|
|Sesiones máximas permitidas|30000|30000|30000|30000|30000|30000|
|Número de réplicas|3|3|3|3|3|3|
|AZ múltiple|N/D|N/D|N/D|N/D|N/D|N/D|
|Escalado horizontal de lectura|SÍ|Sí|Sí|Sí|Sí|SÍ|
|Almacenamiento de copia de seguridad incluido|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|
|||

#### <a name="generation-5-compute-platform"></a>Plataforma de procesos de generación 5
|Tamaño de proceso|BC_Gen5_2|BC_Gen5_4|BC_Gen5_8|BC_Gen5_16|BC_Gen5_24|BC_Gen5_32|BC_Gen5_40|BC_Gen5_80|
|:--- | --: |--: |--: |--: |---: | --: |--: |--: |--: |--: |--: |--: |--: |
|Generación de H/W|5|5|5|5|5|5|5|5|
|Núcleos virtuales|2|4|8|16|24|32|40|80|
|Memoria (GB)|11|22|44|88|132|176|220|440|
|Compatible con almacén de columnas|SÍ|Sí|Sí|Sí|Sí|Sí|Sí|SÍ|
|Almacenamiento OLTP en memoria (GB)|1571|3142|6284|15 768|25 252|37 936|52.22|131,64|
|Tipo de almacenamiento|SSD local|SSD local|SSD local|SSD local|SSD local|SSD local|SSD local|SSD local|
|Latencia de E/S (aproximada)|1-2 ms (escritura)<br>1-2 ms (lectura)|1-2 ms (escritura)<br>1-2 ms (lectura)|1-2 ms (escritura)<br>1-2 ms (lectura)|1-2 ms (escritura)<br>1-2 ms (lectura)|1-2 ms (escritura)<br>1-2 ms (lectura)|1-2 ms (escritura)<br>1-2 ms (lectura)|1-2 ms (escritura)<br>1-2 ms (lectura)|1-2 ms (escritura)<br>1-2 ms (lectura)|
|Tamaño máximo de datos (GB)|1024|1024|1024|1024|2048|4096|4096|4096|
|Tamaño máximo de registro (GB)|307|307|307|307|614|1229|1229|1229|
|Tamaño de TempDB (GB)|64|128|256|384|384|384|384|384|
|IOPS de destino (64 KB)|5000|10000|20000|40000|60000|80000|100000|200000
|Cantidad máxima de trabajos (solicitudes) simultáneos|200|400|800|1600|2400|3200|4000|8000|
|Sesiones máximas permitidas|30000|30000|30000|30000|30000|30000|30000|30000|
|Número de réplicas|3|3|3|3|3|3|3|3|
|AZ múltiple|N/D|N/D|N/D|N/D|N/D|N/D|N/D|N/D|
|Escalado horizontal de lectura|N/D|N/D|N/D|N/D|N/D|N/D|N/D|N/D|
|Almacenamiento de copia de seguridad incluido|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|
|||

### <a name="hyperscale-service-tier-preview"></a>Nivel de servicio de hiperescala (versión preliminar)

#### <a name="generation-4-compute-platform"></a>Plataforma de procesos de generación 4
|Nivel de rendimiento|HS_Gen4_1|HS_Gen4_2|HS_Gen4_4|HS_Gen4_8|HS_Gen4_16|HS_Gen4_24|
|:--- | --: |--: |--: |--: |--: |--: |--: |
|Generación de H/W|4|4|4|4|4|4|
|Núcleos virtuales|1|2|4|8|16|24|
|Memoria (GB)|7|14|28|56|112|168|
|Compatible con almacén de columnas|SÍ|Sí|Sí|Sí|Sí|SÍ|
|Almacenamiento OLTP en memoria (GB)|N/D|N/D|N/D|N/D|N/D|N/D|
|Tipo de almacenamiento|SSD local|SSD local|SSD local|SSD local|SSD local|SSD local|
|Tamaño máximo de datos (TB)|100 |100 |100 |100 |100 |100 |
|Tamaño máximo de registro (TB)|1 |1 |1 |1 |1 |1 |
|Tamaño de TempDB (GB)|32|64|128|256|384|384|
|IOPS de destino (64 KB)|Por determinar|Por determinar|Por determinar|Por determinar|Por determinar|Por determinar|
|Latencia de E/S (aproximada)|Por determinar|Por determinar|Por determinar|Por determinar|Por determinar|Por determinar|
|Cantidad máxima de trabajos (solicitudes) simultáneos|200|400|800|1600|3200|4800|
|Sesiones máximas permitidas|30000|30000|30000|30000|30000|30000|
|Número de réplicas|2|2|2|2|2|2|
|AZ múltiple|N/D|N/D|N/D|N/D|N/D|N/D|
|Escalado horizontal de lectura|SÍ|Sí|Sí|Sí|Sí|SÍ|
|Almacenamiento de copia de seguridad incluido|7|7|7|7|7|7|
|||
### <a name="generation-5-compute-platform"></a>Plataforma de procesos de generación 5
|Nivel de rendimiento|HS_Gen5_2|HS_Gen5_4|HS_Gen5_8|HS_Gen5_16|HS_Gen5_24|HS_Gen5_32|HS_Gen5_40|HS_Gen5_80|
|:--- | --: |--: |--: |--: |---: | --: |--: |--: |--: |--: |--: |--: |--: |
|Generación de H/W|5|5|5|5|5|5|5|5|
|Núcleos virtuales|2|4|8|16|24|32|40|80|
|Memoria (GB)|11|22|44|88|132|176|220|440|
|Compatible con almacén de columnas|SÍ|Sí|Sí|Sí|Sí|Sí|Sí|SÍ|
|Almacenamiento OLTP en memoria (GB)|N/D|N/D|N/D|N/D|N/D|N/D|N/D|N/D|
|Tipo de almacenamiento|SSD local|SSD local|SSD local|SSD local|SSD local|SSD local|SSD local|SSD local|
|Tamaño máximo de datos (TB)|100 |100 |100 |100 |100 |100 |100 |100 |
|Tamaño máximo de registro (TB)|1 |1 |1 |1 |1 |1 |1 |1 |
|Tamaño de TempDB (GB)|64|128|256|384|384|384|384|384|
|IOPS de destino (64 KB)|Por determinar|Por determinar|Por determinar|Por determinar|Por determinar|Por determinar|Por determinar|Por determinar|
|Latencia de E/S (aproximada)|Por determinar|Por determinar|Por determinar|Por determinar|Por determinar|Por determinar|Por determinar|Por determinar|
|Cantidad máxima de trabajos (solicitudes) simultáneos|200|400|800|1600|2400|3200|4000|8000|
|Sesiones máximas permitidas|30000|30000|30000|30000|30000|30000|30000|30000|
|Número de réplicas|2|2|2|2|2|2|2|2|
|AZ múltiple|N/D|N/D|N/D|N/D|N/D|N/D|N/D|N/D|
|Escalado horizontal de lectura|SÍ|Sí|Sí|Sí|Sí|Sí|Sí|SÍ|
|Almacenamiento de copia de seguridad incluido (límite de la versión preliminar)|7|7|7|7|7|7|7|7|
|||

## <a name="next-steps"></a>Pasos siguientes

- Consulte [Preguntas más frecuentes sobre SQL Database](sql-database-faq.md) para conocer las respuestas a las preguntas más frecuentes.
- Para más información sobre los límites generales de Azure, consulte [Límites, cuotas y restricciones de suscripción y servicios de Microsoft Azure](../azure-subscription-service-limits.md).
