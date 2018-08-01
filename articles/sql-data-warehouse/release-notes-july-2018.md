---
title: Notas de la versión de Azure SQL Data Warehouse de junio de 2018 | Microsoft Docs
description: Notas de la versión de Azure SQL Data Warehouse.
services: sql-data-warehouse
author: twounder
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: manage
ms.date: 07/23/2018
ms.author: twounder
ms.reviewer: twounder
ms.openlocfilehash: 5f4d39c6aa1a5c2c30e84fbf26535fe3ee7799a6
ms.sourcegitcommit: 248c2a76b0ab8c3b883326422e33c61bd2735c6c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2018
ms.locfileid: "39216741"
---
# <a name="whats-new-in-azure-sql-data-warehouse-july-2018"></a>¿Cuáles son las novedades de Azure SQL Data Warehouse? Julio de 2018
Continuamente se aplican mejoras a Azure SQL Data Warehouse. En este artículo se describen las nuevas características y los cambios que se han incluido en julio de 2018.


## <a name="finer-granularity-for-cross-region-and-server-restores"></a>Granularidad más fina para restauraciones entre regiones y servidores
Ahora puede restaurar en regiones y servidores mediante cualquier punto de restauración en lugar de seleccionar copias de seguridad con redundancia geográfica que se realizan cada 24 horas. La restauración entre regiones y servidores es compatibles con puntos de restauración definidos por el usuario o automáticos, lo que permite una granularidad más fina para una protección de datos adicional. Con más puntos de restauración disponibles, puede estar seguro de que el almacenamiento de datos será lógicamente coherente cuando la restauración se realice entre regiones.

![Comando de restauración: Azure SQL Data Warehouse](https://azurecomcdn.azureedge.net/mediahandler/acomblog/media/Default/blog/6ac23972-9ec0-4502-ab10-7b6bc1a3d947.png)
![Opciones de restauración: Azure SQL Data Warehouse](https://azurecomcdn.azureedge.net/mediahandler/acomblog/media/Default/blog/6c63bd0e-9c52-414d-b4be-d3bd3774ee08.png)

Para más información, consulte la entrada de blog [Accelerated and Flexible Restore Points](https://azure.microsoft.com/blog/accelerated-and-flexible-restore-points-with-sql-data-warehouse/) (Puntos de restauración acelerados y flexibles).

## <a name="20-minute-restorations"></a>Restauraciones de 20 minutos
SQL Data Warehouse ahora ofrece la restauración de cualquier almacenamiento de datos **en menos de 20 minutos** dentro de la misma región, independientemente del tamaño de la base de datos. El tiempo de restauración se aplica si la restauración es en el mismo servidor lógico o uno diferente dentro de la misma región. Además, se ha mejorado el proceso de instantáneas para reducir la cantidad de tiempo que tarda en crear un punto de restauración. En los niveles de rendimiento inferiores (configuración de DWU inferior), la mejora es del *doble de reducción* del tiempo para la creación de instantáneas.

Para más información, consulte la entrada de blog [Accelerated and Flexible Restore Points](https://azure.microsoft.com/blog/accelerated-and-flexible-restore-points-with-sql-data-warehouse/) (Puntos de restauración acelerados y flexibles).

## <a name="custom-restoration-configurations"></a>Configuraciones de restauración personalizadas
Ahora puede cambiar el nivel de rendimiento (DWU) cuando se restaura en Azure Portal. También puede restaurar a un almacenamiento de datos actualizado de Gen2. Mediante la restauración a una instancia de Gen2, ahora puede evaluar el impacto de Gen2 antes de [actualizar el almacenamiento de datos de Gen1](https://docs.microsoft.com/azure/sql-data-warehouse/upgrade-to-latest-generation).

![Configuración de restauración personalizada: Azure SQL Data Warehouse](https://azurecomcdn.azureedge.net/mediahandler/acomblog/media/Default/blog/f4c410c7-8515-409c-a983-0976792b8628.png)

## <a name="spdescribeundeclaredparameters"></a>SP_DESCRIBE_UNDECLARED_PARAMETERS
El procedimiento almacenado [sp_describe_undeclared_parameters](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-describe-undeclared-parameters-transact-sql) lo suelen utilizar las herramientas para obtener los metadatos sobre los parámetros en el lote de Transact-SQL. El procedimiento devuelve una fila para cada parámetro del lote con la información de tipo deducida para dicho parámetro. 

```sql
EXEC sp_describe_undeclared_parameters
    @tsql = 
    N'SELECT
        object_id, name, type_desc
      FROM
        sys.indexes
      WHERE
        object_id = @id;'
```

El conjunto de resultados incluye los metadatos sobre el parámetro `@id` (se muestran resultados parciales)
```
parameter_ordinal | name | suggested_system_type_id | suggested_system_type_name
--------------------------------------------------------------------------------
1                 | @id  | 56                       | int
```