---
title: 'Escalar recursos de grupos elásticos: Azure SQL Database | Microsoft Docs'
description: En esta página se describe el proceso de escalado de recursos de grupos elásticos en Azure SQL Database.
services: sql-database
ms.service: sql-database
ms.subservice: elastic-pool
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: oslake
ms.author: moslake
ms.reviewer: carlrab
manager: craigg
ms.date: 10/15/2018
ms.openlocfilehash: cd9886f11685397cbfb82f88bb0b37c8ccc41b67
ms.sourcegitcommit: dbfd977100b22699823ad8bf03e0b75e9796615f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2018
ms.locfileid: "50240178"
---
# <a name="scale-elastic-pool-resources-in-azure-sql-database"></a>Escalar recursos de grupos elásticos en Azure SQL Database

En este artículo se describe cómo escalar los recursos de proceso y almacenamiento disponibles para los grupos elásticos y las bases de datos agrupadas en Azure SQL Database.

## <a name="vcore-based-purchasing-model-change-elastic-pool-storage-size"></a>Modelo de compra basado en núcleos virtuales: cambiar el tamaño del almacenamiento de los grupos elásticos

- Se puede aprovisionar el almacenamiento hasta el límite de tamaño máximo:

  - Para Standard Storage, aumente o disminuya el tamaño en incrementos de 10 GB.
  - Para Premium Storage, aumente o disminuya el tamaño en incrementos de 250 GB.
- El almacenamiento de un grupo elástico se puede aprovisionar aumentando o reduciendo su tamaño máximo.
- El precio del almacenamiento para un grupo elástico es la cantidad de almacenamiento multiplicada por el precio de la unidad de almacenamiento del nivel de servicio. Para más información sobre el precio del almacenamiento adicional, consulte los [precios de SQL Database](https://azure.microsoft.com/pricing/details/sql-database/).

> [!IMPORTANT]
> En algunas circunstancias, puede que deba reducir una base de datos para reclamar el espacio no utilizado. Para más información, consulte [Administración del espacio de archivo en Azure SQL Database](sql-database-file-space-management.md).

## <a name="vcore-based-purchasing-model-change-elastic-pool-compute-resources-vcores"></a>Modelo de compra basado en núcleos virtuales: cambiar los recursos de proceso de los grupos elásticos (núcleos virtuales)

Puede aumentar o disminuir el tamaño de proceso para un grupo elástico en función de los recursos que necesita mediante [Azure Portal](sql-database-elastic-pool-manage.md#azure-portal-manage-elastic-pools-and-pooled-databases), [PowerShell](/powershell/module/azurerm.sql/set-azurermsqlelasticpool), la [CLI de Azure](/cli/azure/sql/elastic-pool#az-sql-elastic-pool-update) o la [ API REST](https://docs.microsoft.com/rest/api/sql/elasticpools/update).

- Al cambiar la escala de los núcleos virtuales de un grupo elástico, se interrumpen brevemente las conexiones de base de datos. Este comportamiento es el mismo que se produce cuando se cambia la escala de las DTU para una base de datos única. Para más información sobre la duración y el impacto de las conexiones interrumpidas para una base de datos durante las operaciones de cambio de escala, vea [Cambiar los recursos de proceso (DTU)](sql-database-single-database-scale.md#dtu-based-purchasing-model-change-compute-resources-dtus).
- La duración para cambiar la escala de los núcleos virtuales del grupo puede depender de la cantidad total de espacio de almacenamiento que usan todas las bases de datos del grupo. En general, la latencia del cambio de escalado calcula el promedio de 90 minutos o menos por cada 100 GB. Por ejemplo, si el espacio total que usan todas las bases de datos del grupo es de 200 GB, la latencia esperada para cambiar la escala del grupo es de 3 horas o menos. En algunos casos en el nivel Estándar o Básico, la latencia del cambio de escala puede ser menos de cinco minutos sin tener en cuenta la cantidad de espacio utilizado.
- En general, lo que se tarda en cambiar los núcleos virtuales mínimos por base de datos o los núcleos virtuales máximos por base de datos suele ser cinco minutos o menos.
- Cuando se reduce el tamaño de los núcleos virtuales del grupo, el espacio usado por el grupo debe ser menor que el tamaño máximo permitido del nivel de servicio de destino y de los núcleos virtuales del grupo.

## <a name="dtu-based-purchasing-model-change-elastic-pool-storage-size"></a>Modelo de compra basado en DTU: cambiar el tamaño del almacenamiento de los grupos elásticos

- El precio de la eDTU de un grupo elástico incluye una cierta cantidad de almacenamiento sin ningún costo adicional. El almacenamiento adicional que supere la cantidad incluida se puede aprovisionar por un costo extra hasta el límite de tamaño máximo en incrementos de 250 GB hasta 1 TB, y luego en incrementos de 256 GB superando 1 TB. Para los límites de tamaño máximo y las cantidades de almacenamiento incluidas, consulte [Grupos elásticos: tamaños de almacenamiento y de proceso](sql-database-dtu-resource-limits-elastic-pools.md#elastic-pool-storage-sizes-and-compute-sizes).
- Se puede aprovisionar el almacenamiento adicional para un grupo elástico si se aumenta su tamaño máximo mediante [Azure Portal](sql-database-elastic-pool-manage.md#azure-portal-manage-elastic-pools-and-pooled-databases), [PowerShell](/powershell/module/azurerm.sql/set-azurermsqlelasticpool), la [CLI de Azure](/cli/azure/sql/elastic-pool#az-sql-elastic-pool-update) o la [API de REST](https://docs.microsoft.com/rest/api/sql/elasticpools/update).
- El precio del almacenamiento adicional para un grupo de bases de datos elásticas es la cantidad de almacenamiento adicional multiplicada por el precio de la unidad de almacenamiento adicional del nivel de servicio. Para más información sobre el precio del almacenamiento adicional, consulte los [precios de SQL Database](https://azure.microsoft.com/pricing/details/sql-database/).

> [!IMPORTANT]
> En algunas circunstancias, puede que deba reducir una base de datos para reclamar el espacio no utilizado. Para más información, consulte [Administración del espacio de archivo en Azure SQL Database](sql-database-file-space-management.md).

## <a name="dtu-based-purchasing-model-change-elastic-pool-compute-resources-edtus"></a>Modelo de compra basado en DTU: cambiar los recursos de proceso de los grupos elásticos (eDTU)

Puede aumentar o disminuir los recursos disponibles para un grupo elástico en función de los recursos que necesita mediante [Azure Portal](sql-database-elastic-pool-manage.md#azure-portal-manage-elastic-pools-and-pooled-databases), [PowerShell](/powershell/module/azurerm.sql/set-azurermsqlelasticpool), la [CLI de Azure](/cli/azure/sql/elastic-pool#az-sql-elastic-pool-update) o la [ API de REST](https://docs.microsoft.com/rest/api/sql/elasticpools/update).

- Al cambiar la escala de las eDTU de grupo, se interrumpe brevemente las conexiones de base de datos. Este comportamiento es el mismo que se produce cuando se cambia la escala de las DTU para una base de datos única. Para más información sobre la duración y el impacto de las conexiones interrumpidas para una base de datos durante las operaciones de cambio de escala, vea [Cambiar los recursos de proceso (DTU)](sql-database-single-database-scale.md#dtu-based-purchasing-model-change-compute-resources-dtus).
- La duración para cambiar la escala de las eDTU de grupo puede depender de la cantidad total de espacio de almacenamiento utilizado por todas las bases de datos del grupo. En general, la latencia del cambio de escalado calcula el promedio de 90 minutos o menos por cada 100 GB. Por ejemplo, si el espacio total que usan todas las bases de datos del grupo es de 200 GB, la latencia esperada para cambiar la escala del grupo es de 3 horas o menos. En algunos casos en el nivel Estándar o Básico, la latencia del cambio de escala puede ser menos de cinco minutos sin tener en cuenta la cantidad de espacio utilizado.
- En general, la duración del cambio de las eDTU mínimas por base de datos o de las eDTU máximas por base de datos suele ser de cinco minutos o menos.
- Cuando se reducen las eDTU del grupo elástico, el grupo que se utilice debe ser menor que el tamaño máximo permitido del nivel de servicio de destino y de las eDTU de grupo.
- Cuando se cambia la escala de las eDTU de un grupo elástico, se aplica un costo de almacenamiento adicional si (1) el tamaño máximo de almacenamiento del grupo de es compatible con el grupo de destino y (2) el tamaño máximo de almacenamiento supera la cantidad de almacenamiento incluido del grupo de destino. Por ejemplo, si un grupo Estándar de 100 eDTU con un tamaño máximo de 100 GB se reduce a un grupo Estándar de 50 eDTU, se aplica un costo de almacenamiento adicional porque el grupo de destino admite un tamaño máximo de 100 GB y su cantidad de almacenamiento incluido es solo de 50 GB. Por lo tanto, la cantidad de almacenamiento adicional es 100 GB – 50 GB = 50 GB. Para conocer el precio del almacenamiento adicional, consulte los [precios de SQL Database](https://azure.microsoft.com/pricing/details/sql-database/). Si la cantidad de espacio real utilizada es menor que la cantidad de almacenamiento incluido, este costo adicional puede evitarse si se reduce el tamaño máximo de la base de datos a la cantidad incluida.

## <a name="next-steps"></a>Pasos siguientes

Para conocer los límites de recursos globales, consulte [Límites de recursos basados en núcleos virtuales de SQL Database para grupos elásticos](sql-database-vcore-resource-limits-elastic-pools.md) y [SQL Database DTU-based resource limits - elastic pools](sql-database-dtu-resource-limits-elastic-pools.md) (Límites de recursos basados en DTU de SQL Database para grupos elásticos).
