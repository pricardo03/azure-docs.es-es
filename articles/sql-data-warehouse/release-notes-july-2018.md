---
title: Notas de la versión de Azure SQL Data Warehouse de julio de 2018 | Microsoft Docs
description: Notas de la versión de Azure SQL Data Warehouse.
services: sql-data-warehouse
author: twounder
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: manage
ms.date: 08/06/2018
ms.author: twounder
ms.reviewer: twounder
ms.openlocfilehash: 123198b21122a23d81794db0a5ca2051b15ee2e7
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/18/2019
ms.locfileid: "57834208"
---
# <a name="whats-new-in-azure-sql-data-warehouse-july-2018"></a>¿Cuáles son las novedades de Azure SQL Data Warehouse? Julio de 2018
Continuamente se aplican mejoras a Azure SQL Data Warehouse. En este artículo se describen las nuevas características y los cambios que se han incluido en julio de 2018.

## <a name="lightning-fast-query-performance"></a>Rendimiento increíble de consultas
[Azure SQL Data Warehouse](https://aka.ms/sqldw) establece nuevas marcas en el rendimiento con la introducción de acceso instantáneo a los datos que mejora las operaciones de orden aleatorio. El acceso instantáneo a los datos reduce la sobrecarga de las operaciones de movimiento de datos al usar operaciones de datos nativas directas de SQL Server a SQL Server. La integración con el motor de SQL Server directamente para el movimiento de datos significa que SQL Data Warehouse es ahora un **67 % más rápido que Amazon Redshift** cuando se usa una carga de trabajo obtenida del reconocido estándar del sector [TPC Benchmark™ H (TPC-H)](http://www.tpc.org/tpch/).

![Azure SQL Data Warehouse es más rápido y más barato que Amazon Redshift](https://azurecomcdn.azureedge.net/mediahandler/acomblog/media/Default/blog/eb3b908a-464d-4847-b384-9f296083a737.png)
<sub>Fuente: [Informe del analista de investigación Gigaom: Data Warehouse in the Cloud Benchmark](https://gigaom.com/report/data-warehouse-in-the-cloud-benchmark/)</sub>

Aparte del rendimiento en tiempo de ejecución, el informe de [Gigaom Research](https://gigaom.com/report/data-warehouse-in-the-cloud-benchmark/) también mide la relación precio-rendimiento para cuantificar el costo en dólares USA de cargas de trabajo específicas. SQL Data Warehouse era **al menos un 23 por ciento menos caro** que Redshift con cargas de trabajo der 30 TB. Gracias a la posibilidad que tiene SQL Data Warehouse de escalar el proceso de manera elástica, así como de pausar y reanudar las cargas de trabajo, los clientes solo pagan cuando usan el servicio, lo que reduce aún más los costos.
![Azure SQL Data Warehouse es más rápido y más barato que Amazon Redshift](https://azurecomcdn.azureedge.net/mediahandler/acomblog/media/Default/blog/cb76447e-621e-414b-861e-732ffee5345a.png)
<sub>Fuente: [Informe del analista de investigación Gigaom: Data Warehouse in the Cloud Benchmark](https://gigaom.com/report/data-warehouse-in-the-cloud-benchmark/)</sub>

### <a name="query-concurrency"></a>Simultaneidad de consultas
SQL Data Warehouse también garantiza que los datos son accesibles entre las organizaciones. Microsoft ha mejorado el servicio para admitir 128 consultas simultáneas, así que más usuarios pueden consultar la misma base de datos sin que otras solicitudes los bloqueen. En comparación, Amazon Redshift restringe el número máximo de consultas simultáneas a 50, lo que limita el acceso a los datos dentro de la organización.

SQL Data Warehouse ofrece estas mejoras de simultaneidad y rendimiento de consultas sin ningún aumento de precio y basa su arquitectura única en almacenamiento y proceso desacoplados.

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
## <a name="sprefreshsqlmodule"></a>SP_REFRESHSQLMODULE
El procedimiento almacenado [sp_refreshsqlmodule](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-refreshsqlmodule-transact-sql) actualiza los metadatos de un objeto de base de datos si los metadatos subyacentes se han desactualizado debido a cambios en los objetos subyacentes. Este caso puede darse si se alteran las tablas base de una vista y la vista no se ha vuelto a crear. Este procedimiento almacenado le ahorra el paso de descartar y volver a crear objetos dependientes.

En el ejemplo siguiente se muestra una vista que se convierte en obsoleta debido al cambio de la tabla subyacente. Observará que los datos son correctos para el primer cambio de columna (1 por Mollie), pero el nombre de columna no es válido y no existe la segunda columna. 
```sql
CREATE TABLE base_table (Id INT);
GO

INSERT INTO base_table (Id) VALUES (1);
GO

CREATE VIEW base_view AS SELECT * FROM base_table;
GO

SELECT * FROM base_view;
GO

-- Id
-- ----
-- 1

DROP TABLE base_table;
GO

CREATE TABLE base_table (fname VARCHAR(10), lname VARCHAR(10));
GO

INSERT INTO base_table (fname, lname) VALUES ('Mollie', 'Gallegos');
GO

SELECT * FROM base_view;
GO

-- Id
-- ----------
-- Mollie

EXEC sp_refreshsqlmodule @Name = 'base_view';
GO

SELECT * FROM base_view;
GO

-- fname     | lname
-- ---------- ----------
-- Mollie    | Gallegos
```

## <a name="next-steps"></a>Pasos siguientes
Ahora que ya conoce un poco SQL Data Warehouse, aprenda a [crear una instancia de SQL Data Warehouse][create a SQL Data Warehouse] rápidamente. Si no está familiarizado con Azure, el [Glosario de Azure][Azure glossary] le puede resultar útil para consultar la nueva terminología que se encuentre. O bien, examine algunos de estos otros recursos de SQL Data Warehouse.  

* [Casos de éxito de clientes]
* [Blogs]
* [Solicitud de función]
* [Vídeos]
* [Blogs de Customer Advisory Team]
* [Foro Stack Overflow]
* [Twitter]


[Blogs]: https://azure.microsoft.com/blog/tag/azure-sql-data-warehouse/
[Blogs de Customer Advisory Team]: https://blogs.msdn.microsoft.com/sqlcat/tag/sql-dw/
[Casos de éxito de clientes]: https://azure.microsoft.com/case-studies/?service=sql-data-warehouse
[Solicitud de función]: https://feedback.azure.com/forums/307516-sql-data-warehouse
[Foro Stack Overflow]: https://stackoverflow.com/questions/tagged/azure-sqldw
[Twitter]: https://twitter.com/hashtag/SQLDW
[Vídeos]: https://azure.microsoft.com/documentation/videos/index/?services=sql-data-warehouse
[create a SQL Data Warehouse]: ./create-data-warehouse-portal.md
[Azure glossary]: ../azure-glossary-cloud-terminology.md
