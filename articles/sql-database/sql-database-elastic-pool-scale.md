---
title: 'Escalar recursos de grupos elásticos: Azure SQL Database | Microsoft Docs'
description: En esta página se describe el proceso de escalado de recursos de grupos elásticos en Azure SQL Database.
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
ms.date: 3/14/2019
ms.openlocfilehash: d8aaf51c836a8e88c4e9b92798067167cd044e72
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/18/2019
ms.locfileid: "58015373"
---
# <a name="scale-elastic-pool-resources-in-azure-sql-database"></a>Escalar recursos de grupos elásticos en Azure SQL Database

En este artículo se describe cómo escalar los recursos de proceso y almacenamiento disponibles para los grupos elásticos y las bases de datos agrupadas en Azure SQL Database.

## <a name="change-compute-resources-vcores-or-dtus"></a>Cambiar los recursos de proceso (núcleos virtuales o Dtu)

Después de elegir inicialmente el número de núcleos virtuales o Edtu, puede escalar un grupo elástico o reducir verticalmente dinámicamente en función de la experiencia real mediante la [portal Azure](sql-database-elastic-pool-manage.md#azure-portal-manage-elastic-pools-and-pooled-databases), [PowerShell](/powershell/module/az.sql/Get-AzSqlElasticPool), el [CLI de Azure ](/cli/azure/sql/elastic-pool#az-sql-elastic-pool-update), o el [API de REST](https://docs.microsoft.com/rest/api/sql/elasticpools/update).

### <a name="impact-of-changing-service-tier-or-rescaling-compute-size"></a>Impacto de cambiar el tamaño de proceso de nivel o cambiar la escala de servicio

Cambiar el servicio de nivel o calcular el tamaño de un grupo elástico sigue un patrón similar en cuanto a bases de datos únicas e implica principalmente el servicio siguiendo estos pasos:

1. Crear nueva instancia de proceso para el grupo elástico  

    Se crea una nueva instancia de proceso para el grupo elástico con el tamaño de proceso y el nivel de servicio solicitado. Para algunas combinaciones de nivel de servicio y los cambios de tamaño de proceso, una réplica de cada base de datos debe crearse en la nueva instancia de proceso que implica copiar los datos y puede influir notoriamente en la latencia total. En cualquier caso, las bases de datos permanezcan en línea durante este paso y continuarán de conexiones para que se dirijan a las bases de datos en la instancia de proceso original.

2. Cambiar el enrutamiento de conexiones a la nueva instancia de proceso

    Se quitan las conexiones existentes con las bases de datos en la instancia de proceso original. Las nuevas conexiones se establecen en las bases de datos en la nueva instancia de proceso. Para algunas combinaciones de nivel de servicio y los cambios de tamaño de proceso, los archivos de base de datos se separa y volver a adjuntar durante la conmutación.  No obstante, el conmutador puede provocar una breve interrupción del servicio cuando las bases de datos no están disponibles con carácter general para menos de 30 segundos y a menudo solo unos segundos. Si no hay de larga ejecución transacciones que se ejecutan cuando se quitan conexiones, la duración de este paso puede tardar más tiempo con el fin de recuperar las transacciones anuladas. [Acelerado de recuperación de base de datos](sql-database-accelerated-database-recovery.md) puede reducir el impacto de anulación de transacciones de larga ejecución.

> [!IMPORTANT]
> No se pierden datos durante los pasos del flujo de trabajo.

### <a name="latency-of-changing-service-tier-or-rescaling-compute-size"></a>Latencia del cambio de tamaño de proceso de nivel o cambiar la escala de servicio

La latencia para cambiar el nivel de servicio o cambiar el tamaño de proceso de una sola base de datos o el grupo elástico se parametriza como sigue:

|Nivel de servicio|Básica única base de datos</br>Estándar (S0-S1)|Grupo elástico básico,</br>Estándar (S2-S12) </br>A gran escala, </br>Finalidad única base de datos general o del grupo elástico|Base de datos única Premium o crítico para la empresa o el grupo elástico|
|:---|:---|:---|:---|
|**Base de datos único básica,</br> estándar (S0-S1)**|&bull; &nbsp;Latencia de tiempo constante independientemente del espacio utilizado</br>&bull; &nbsp;Normalmente, menos de 5 minutos|&bull; &nbsp;Latencia proporcional al espacio de base de datos puede utilizar debido a copiar los datos</br>&bull; &nbsp;Normalmente, menos de 1 minuto por GB de espacio usado|&bull; &nbsp;Latencia proporcional al espacio de base de datos puede utilizar debido a copiar los datos</br>&bull; &nbsp;Normalmente, menos de 1 minuto por GB de espacio usado|
|**Grupo elástico básico, </br>estándar (S2-S12) </br>a gran escala, </br>única base de datos de uso General o el grupo elástico**|&bull; &nbsp;Latencia proporcional al espacio de base de datos puede utilizar debido a copiar los datos</br>&bull; &nbsp;Normalmente, menos de 1 minuto por GB de espacio usado|&bull; &nbsp;Latencia de tiempo constante independientemente del espacio utilizado</br>&bull; &nbsp;Normalmente, menos de 5 minutos|&bull; &nbsp;Latencia proporcional al espacio de base de datos puede utilizar debido a copiar los datos</br>&bull; &nbsp;Normalmente, menos de 1 minuto por GB de espacio usado|
|**Base de datos única Premium o crítico para la empresa o el grupo elástico**|&bull; &nbsp;Latencia proporcional al espacio de base de datos puede utilizar debido a copiar los datos</br>&bull; &nbsp;Normalmente, menos de 1 minuto por GB de espacio usado|&bull; &nbsp;Latencia proporcional al espacio de base de datos puede utilizar debido a copiar los datos</br>&bull; &nbsp;Normalmente, menos de 1 minuto por GB de espacio usado|&bull; &nbsp;Latencia proporcional al espacio de base de datos puede utilizar debido a copiar los datos</br>&bull; &nbsp;Normalmente, menos de 1 minuto por GB de espacio usado|

> [!NOTE]
>
> - En el caso de cambiar el nivel de servicio o cambiar la escala de proceso para un grupo elástico, la suma del espacio utilizado en todas las bases de datos en el grupo debe usarse para calcular la estimación.
> - En el caso de mover una base de datos de un grupo elástico, solo el espacio usado por la base de datos afecta a la latencia, no el espacio utilizado por el grupo elástico.
>
> [!TIP]
> Para supervisar las operaciones en curso, consulte: [Administración de operaciones mediante la API REST de SQL](https://docs.microsoft.com/rest/api/sql/operations/list), [Administración de operaciones mediante la CLI](/cli/azure/sql/db/op), [Supervisión de operaciones mediante T-SQL](/sql/relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database) y estos dos comandos de PowerShell: [Get-AzSqlDatabaseActivity](/powershell/module/az.sql/get-azsqldatabaseactivity) y [Stop AzSqlDatabaseActivity](/powershell/module/az.sql/stop-azsqldatabaseactivity).

### <a name="additional-considerations-when-changing-service-tier-or-rescaling-compute-size"></a>Consideraciones adicionales cuando se cambia de servicio de nivel o cambiar la escala de tamaño de proceso

- Al reducir el tamaño de Edtu para un grupo elástico o núcleos virtuales, el espacio de grupo usado debe ser menor que el tamaño máximo permitido de las Edtu de grupo y del servicio de destino.
- Al cambiar la escala de núcleos virtuales o Edtu para un grupo elástico, se aplica un costo de almacenamiento adicional si (1) el tamaño máximo de almacenamiento del grupo es compatible con el grupo de destino, y (2) el tamaño máximo de almacenamiento supera la cantidad de almacenamiento incluido del grupo de destino. Por ejemplo, si un grupo Estándar de 100 eDTU con un tamaño máximo de 100 GB se reduce a un grupo Estándar de 50 eDTU, se aplica un costo de almacenamiento adicional porque el grupo de destino admite un tamaño máximo de 100 GB y su cantidad de almacenamiento incluido es solo de 50 GB. Por lo tanto, la cantidad de almacenamiento adicional es 100 GB – 50 GB = 50 GB. Para conocer el precio del almacenamiento adicional, consulte los [precios de SQL Database](https://azure.microsoft.com/pricing/details/sql-database/). Si la cantidad de espacio real utilizada es menor que la cantidad de almacenamiento incluido, este costo adicional puede evitarse si se reduce el tamaño máximo de la base de datos a la cantidad incluida.

### <a name="billing-during-rescaling"></a>Facturación durante el cambio de escala

Se le cobrará por cada hora que una base de datos exista con el mayor nivel de servicio + tamaño de proceso aplicable durante esa hora, independientemente del uso o de si la base de datos estuvo activa durante menos tiempo. Por ejemplo, si crea una base de datos única y la elimina a los cinco minutos, se le efectuará un cargo de una hora por usar la base de datos.

## <a name="change-elastic-pool-storage-size"></a>cambiar el tamaño de almacenamiento de grupos elásticos

> [!IMPORTANT]
> En algunas circunstancias, puede que deba reducir una base de datos para reclamar el espacio no utilizado. Para más información, consulte [Administración del espacio de archivo en Azure SQL Database](sql-database-file-space-management.md).

### <a name="vcore-based-purchasing-model"></a>Modelo de compra basado en núcleos virtuales

- Se puede aprovisionar el almacenamiento hasta el límite de tamaño máximo:

  - En el caso del almacenamiento en niveles de servicio estándar o de uso general, aumente o disminuya el tamaño en incrementos de 10 GB.
  - Para el almacenamiento en niveles de servicio premium o críticos para la empresa, aumente o disminuya el tamaño en incrementos de 250 GB.
- El almacenamiento de un grupo elástico se puede aprovisionar aumentando o reduciendo su tamaño máximo.
- El precio del almacenamiento para un grupo elástico es la cantidad de almacenamiento multiplicada por el precio de la unidad de almacenamiento del nivel de servicio. Para más información sobre el precio del almacenamiento adicional, consulte los [precios de SQL Database](https://azure.microsoft.com/pricing/details/sql-database/).

> [!IMPORTANT]
> En algunas circunstancias, puede que deba reducir una base de datos para reclamar el espacio no utilizado. Para más información, consulte [Administración del espacio de archivo en Azure SQL Database](sql-database-file-space-management.md).

### <a name="dtu-based-purchasing-model"></a>Modelo de compra basado en DTU

- El precio de la eDTU de un grupo elástico incluye una cierta cantidad de almacenamiento sin ningún costo adicional. El almacenamiento adicional que supere la cantidad incluida se puede aprovisionar por un costo extra hasta el límite de tamaño máximo en incrementos de 250 GB hasta 1 TB, y luego en incrementos de 256 GB superando 1 TB. Para los límites de tamaño máximo y las cantidades de almacenamiento incluidas, consulte [Grupos elásticos: tamaños de almacenamiento y de proceso](sql-database-dtu-resource-limits-elastic-pools.md#elastic-pool-storage-sizes-and-compute-sizes).
- Se puede aprovisionar el almacenamiento adicional para un grupo elástico si se aumenta su tamaño máximo mediante [Azure Portal](sql-database-elastic-pool-manage.md#azure-portal-manage-elastic-pools-and-pooled-databases), [PowerShell](/powershell/module/az.sql/Get-AzSqlElasticPool), la [CLI de Azure](/cli/azure/sql/elastic-pool#az-sql-elastic-pool-update) o la [API de REST](https://docs.microsoft.com/rest/api/sql/elasticpools/update).
- El precio del almacenamiento adicional para un grupo de bases de datos elásticas es la cantidad de almacenamiento adicional multiplicada por el precio de la unidad de almacenamiento adicional del nivel de servicio. Para más información sobre el precio del almacenamiento adicional, consulte los [precios de SQL Database](https://azure.microsoft.com/pricing/details/sql-database/).

> [!IMPORTANT]
> En algunas circunstancias, puede que deba reducir una base de datos para reclamar el espacio no utilizado. Para más información, consulte [Administración del espacio de archivo en Azure SQL Database](sql-database-file-space-management.md).

## <a name="next-steps"></a>Pasos siguientes

Para conocer los límites de recursos globales, consulte [Límites de recursos basados en núcleos virtuales de SQL Database para grupos elásticos](sql-database-vcore-resource-limits-elastic-pools.md) y [SQL Database DTU-based resource limits - elastic pools](sql-database-dtu-resource-limits-elastic-pools.md) (Límites de recursos basados en DTU de SQL Database para grupos elásticos).
