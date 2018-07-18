---
title: 'Niveles de servicio de Azure SQL Database: DTU | Microsoft Docs'
description: Obtenga información acerca de los niveles de servicio para las bases de datos de grupo y únicas a fin de proporcionar niveles de rendimiento y tamaños de almacenamiento.
services: sql-database
author: sachinpMSFT
ms.service: sql-database
ms.custom: DBs & servers
ms.topic: conceptual
ms.date: 06/28/2018
manager: craigg
ms.author: carlrab
ms.openlocfilehash: d6dc641123e2bf840940f6246245a89fdd792db5
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/29/2018
ms.locfileid: "37131845"
---
# <a name="choosing-a-dtu-based-service-tier-performance-level-and-storage-resources"></a>Elección de un nivel de servicio basado en DTU, nivel de rendimiento y recursos de almacenamiento 

Los niveles de servicio se diferencian por una variedad de niveles de rendimiento con una cantidad fija de almacenamiento incluido, un período de retención fijo para copias de seguridad y un precio fijo. Todos los niveles de servicio proporcionan la flexibilidad de cambiar los niveles de rendimiento sin tiempo de inactividad. Las bases de datos únicas y los grupos elásticos se facturan por horas en función del nivel de servicio y el nivel de rendimiento.

> [!IMPORTANT]
> Instancia administrada de Azure SQL Database, actualmente en versión preliminar pública, no admite un modelo de compra basado en DTU. Para más información, consulte [Instancia administrada de Azure SQL Database](sql-database-managed-instance.md). 

## <a name="choosing-a-dtu-based-service-tier"></a>Elección de un nivel de servicio basado en DTU

La selección de un nivel de servicio depende sobre todo de los requisitos de continuidad del negocio, de almacenamiento y de rendimiento.
||Básica|Estándar|Premium|
| :-- | --: |--:| --:| --:| 
|Carga de trabajo de destino|Desarrollo y producción|Desarrollo y producción|Desarrollo y producción||
|Acuerdo de Nivel de Servicio de tiempo de actividad|99,99%|99,99%|99,99%|N/D en versión preliminar|
|Retención de copias de seguridad|7 días|35 días|35 días|
|CPU|Bajo|Bajo, medio, alto|Medio, alto|
|Rendimiento de E/S (aproximado) |2,5 IOPS por DTU| 2,5 IOPS por DTU | 48 IOPS por DTU|
|Latencia de E/S (aproximada)|5 ms (lectura), 10 ms (escritura)|5 ms (lectura), 10 ms (escritura)|2 ms (lectura/escritura)|
|Índice de almacén de columnas |N/D|S3 y versiones posteriores|Compatible|
|OLTP en memoria (optimización en memoria|N/D|N/D|Compatible|
|||||

## <a name="single-database-dtu-and-storage-limits"></a>Límites de DTU de una sola base de datos y almacenamiento

Los niveles de rendimiento se expresan como unidades de transmisión de datos (DTU) para las bases de datos únicas y como unidades de transmisión de datos elásticas (eDTU) para los grupos de bases de datos elásticas. Para más información sobre las DTU y las eDTU, consulte [¿Qué son las DTU y las eDTU?](sql-database-service-tiers.md#what-are-database-transaction-units-dtus)

||Básica|Estándar|Premium|
| :-- | --: | --: | --: | --: |
| Tamaño máximo de almacenamiento | 2 GB | 1 TB | 4 TB  | 
| Cantidad máxima de DTU | 5 | 3000 | 4000 | |
||||||

## <a name="elastic-pool-edtu-storage-and-pooled-database-limits"></a>Límites de eDTU de grupo elástico, almacenamiento y base de datos agrupada

| | **Básico** | **Estándar** | **Premium** | 
| :-- | --: | --: | --: | --: |
| Tamaño máximo de almacenamiento por base de datos  | 2 GB | 1 TB | 1 TB | 
| Tamaño máximo de almacenamiento por grupo | 156 GB | 4 TB | 4 TB | 
| Cantidad máxima de eDTU por base de datos | 5 | 3000 | 4000 | 
| Cantidad máxima de eDTU por grupo | 1600 | 3000 | 4000 | 
| Cantidad máxima de bases de datos por grupo | 500  | 500 | 100 | 
||||||

> [!IMPORTANT]
> Hay más de 1 TB de almacenamiento en el nivel Premium disponible actualmente en todas las regiones, excepto en las siguientes: Centro-oeste de EE. UU., Este de China, USDoD (centro), Centro de Alemania, USDoD (este), US Gov (suroeste), USGov Iowa, Nordeste de Alemania, Norte de China. En otras regiones, el almacenamiento máximo del nivel Premium está limitado a 1 TB. Consulte [Limitaciones actuales P11-P15](sql-database-dtu-resource-limits-single-databases.md#single-database-limitations-of-p11-and-p15-when-the-maximum-size-greater-than-1-tb).  

## <a name="next-steps"></a>Pasos siguientes

- Para más información sobre niveles de rendimiento y opciones de tamaño de almacenamiento específicos disponibles para bases de datos únicas, consulte [SQL Database DTU-based resource limits for single databases](sql-database-dtu-resource-limits-single-databases.md#single-database-storage-sizes-and-performance-levels) (Límites de recursos basados en DTU de SQL Database para bases de datos únicas).
- Para más información sobre niveles de rendimiento y opciones de tamaño de almacenamiento específicos disponibles para grupos elásticos, consulte [SQL Database DTU-based resource limits](sql-database-dtu-resource-limits-elastic-pools.md#elastic-pool-storage-sizes-and-performance-levels) (Límites de recursos basados en DTU de SQL Database).
