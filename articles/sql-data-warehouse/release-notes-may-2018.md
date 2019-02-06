---
title: Notas de la versión de Azure SQL Data Warehouse de mayo de 2018 | Microsoft Docs
description: Notas de la versión de Azure SQL Data Warehouse.
services: sql-data-warehouse
author: twounder
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: manage
ms.date: 07/23/2018
ms.author: twounder
ms.reviewer: twounder
ms.openlocfilehash: 0ffb9b9105ee455d35d080bb45063c158cadce1e
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/31/2019
ms.locfileid: "55463402"
---
# <a name="whats-new-in-azure-sql-data-warehouse-may-2018"></a>¿Cuáles son las novedades de Azure SQL Data Warehouse? Mayo de 2018 
Continuamente se aplican mejoras a Azure SQL Data Warehouse. En este artículo se describen las nuevas características y los cambios que se han incluido en mayo de 2018. 

## <a name="gen-2-instances"></a>Instancias de segunda generación
![Texto alternativo](https://azurecomcdn.azureedge.net/mediahandler/acomblog/media/Default/blog/2528b41b-f09f-45b1-aa65-fc60d562d3bd.png) El nivel de segunda generación optimizado para proceso de Azure SQL Data Warehouse establece nuevos estándares de rendimiento para el almacenamiento de datos en la nube. Ahora los clientes obtienen un rendimiento de consultas hasta cinco veces mejor, una simultaneidad cuatro veces mayor y una potencia de procesamiento cinco veces mayor en comparación con la generación actual. Actualmente se pueden ofrecer 128 consultas simultáneas desde un único clúster, la cifra más elevada de todos los servicios de almacenamiento de datos en la nube.

Consulte el anuncio de blog [Turbocharge cloud analytics with Azure SQL Data Warehouse](https://azure.microsoft.com/blog/turbocharge-cloud-analytics-with-azure-sql-data-warehouse/) (Optimización del análisis de datos en la nube con Azure SQL Data Warehouse) de Rohan Kumar, vicepresidente ejecutivo de Azure Data.

## <a name="auto-statistics"></a>Estadísticas automáticas
Las estadísticas son fundamentales para optimizar la generación de planes de consulta en optimizadores modernos basados en costos, como el motor de SQL Data Warehouse. Cuando se conocen todas las consultas de antemano, determinar qué objetos de estadísticas se deben crear es una tarea factible. Sin embargo, cuando el sistema se tiene que enfrentar a consultas ad-hoc y aleatorias, algo típico de las cargas de trabajo de almacenamiento de datos, es posible que los administradores del sistema se vean en un compromiso para predecir qué estadísticas deben crearse, lo que puede ocasionar planes de ejecución de consultas subóptimos y mayores tiempos de respuesta de consultas. Un método para reducir este problema es crear objetos de estadísticas en todas las columnas de tablas de antemano. Sin embargo, ese proceso tiene sus inconvenientes, ya que los objetos de estadísticas necesitan mantenerse durante el proceso de carga de la tabla, lo que conlleva tiempos de carga mayores.

SQL Data Warehouse ya admite la creación automática de objetos de estadísticas, lo que proporciona una mayor flexibilidad, productividad y facilidad de uso para administradores de sistema y desarrolladores. Además, garantiza que el sistema ofrece planes de ejecución de calidad y los mejores tiempos de respuesta.

Para habilitar o deshabilitar la creación automática de estadísticas en SQL Data Warehouse, ejecute esta instrucción:
```sql
ALTER DATABASE { database_name } SET { AUTO_CREATE_STATISTICS { OFF | ON } } [;]
```

Como práctica recomendada, lo mejor es establecer la opción `AUTO_CREATE_STATISTICS` en `ON`.

> [!NOTE]
> La creación automática de estadísticas está *habilitada de manera predeterminada* para todos los almacenes de datos nuevos.
>  

Consulte el artículo [Opciones de ALTER DATABASE SET](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-set-options) para ver información adicional.

## <a name="rejected-row-support"></a>Compatibilidad con filas rechazadas
A menudo, los clientes usan [PolyBase (tablas externas) para cargar datos](design-elt-data-loading.md) en SQL Data Warehouse debido a la naturaleza paralela y al alto rendimiento de la carga de datos. PolyBase también es el modelo de carga predeterminado para cargar datos mediante [Azure Data Factory](http://azure.com/adf). 

SQL Data Warehouse agrega la posibilidad de definir una ubicación de fila rechazada mediante el parámetro `REJECTED_ROW_LOCATION` con la instrucción [CREATE EXTERNAL TABLE](https://docs.microsoft.com/sql/t-sql/statements/create-external-table-transact-sql). Después de ejecutar una instrucción [CREATE TABLE AS SELECT (CTAS)](https://docs.microsoft.com/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse) desde la tabla externa, las filas que no se hayan podido cargar se almacenarán en un archivo próximo al origen para poder investigarlo con más detenimiento. 

Consulte el blog [Load confidently with SQL Data Warehouse PolyBase Rejected Row Location](https://azure.microsoft.com/blog/load-confidently-with-sql-data-warehouse-polybase-rejected-row-location/) (Cargar sin problemas con la ubicación de filas rechazadas de PolyBase en SQL Data Warehouse) para obtener más información sobre el comportamiento de las filas rechazadas.

En el ejemplo siguiente se muestra la nueva sintaxis para especificar filas rechazadas.

```sql
CREATE EXTERNAL TABLE [dbo].[Reject_Example]
(
    ...
)
WITH
(
    ...
    ,REJECTED_ROW_LOCATION=‘/Reject_Directory'
)
```

## <a name="alter-view"></a>ALTER VIEW
[ALTER VIEW](https://docs.microsoft.com/sql/t-sql/statements/alter-view-transact-sql) permite que un usuario modifique una vista creada previamente sin tener que eliminar y volver a crear la vista y aplicar los permisos de nuevo. 

En el ejemplo siguiente se modifica una vista creada previamente.
```sql
ALTER VIEW test_view AS SELECT 1 [data];
```

## <a name="concatws"></a>CONCAT_WS
La función [CONCAT_WS](https://docs.microsoft.com/sql/t-sql/functions/concat-ws-transact-sql) devuelve una cadena que es el resultado de la concatenación de dos o más valores siguiendo un método de un extremo a otro. Separa los valores concatenados con el delimitador especificado en el primer argumento. La función `CONCAT_WS` es útil para generar salidas de valores separados por comas (CSV).

En el ejemplo siguiente se muestra la concatenación de un conjunto de valores int con una coma.
```sql
SELECT CONCAT_WS(',', 1, 2, 3) [result];
```
La instrucción devuelve este resultado:
```
result
---------
1,2,3
```
En el ejemplo siguiente se muestra la concatenación de valores de tipos de datos mezclados con una coma.
```sql
SELECT CONCAT_WS(',', 1, 2, 'String', NEWID()) [result]
```
La instrucción devuelve este resultado:
```
result
---------
1,2,String,26E1F74D-5746-44DC-B47F-2FC1DA1B6E49
```

## <a name="spdatatypeinfo"></a>SP_DATATYPE_INFO
El procedimiento almacenado del sistema [sp_datatype_info](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-datatype-info-transact-sql) devuelve información sobre los tipos de datos compatibles con el entorno actual. Suele usarse con herramientas que se conectan mediante conexiones ODBC para la investigación de tipos de datos.

En el ejemplo siguiente se recuperan detalles para todos los tipos de datos compatibles con SQL Data Warehouse.

```sql
EXEC sp_datatype_info
```

## <a name="select-into-with-order-by-behavior-change"></a>SELECT INTO con ORDER BY (cambio de comportamiento)
Ahora SQL Data Warehouse bloqueará las consultas de `SELECT INTO` que contengan una cláusula `ORDER BY`. Anteriormente, esta operación se realizaría correctamente si se ordenasen primero los datos en la memoria y se insertasen posteriormente en la tabla de destino, para así reordenar los datos para que se ajustasen a la forma de la tabla.

### <a name="previous-behavior"></a>Comportamiento anterior
La siguiente instrucción se ejecutaría correctamente con una sobrecarga de procesamiento adicional.
```sql
SELECT * INTO table2 FROM table1 ORDER BY 1;
```

### <a name="current-behavior"></a>Comportamiento actual
La siguiente instrucción producirá un error en el que se indica que la cláusula `ORDER BY` no es compatible con una instrucción `SELECT INTO`.
```sql
SELECT * INTO table2 FROM table1 ORDER BY 1;
```
Instrucción de error devuelta:
```
Msg 104381, Level 16, State 1, Line 1
The ORDER BY clause is invalid in views, CREATE TABLE AS SELECT, INSERT SELECT, SELECT INTO, inline functions, derived tables, subqueries, and common table expressions, unless TOP or FOR XML is also specified.
```

## <a name="set-parseonly-on-query-status-behavior-change"></a>Estado de la consulta SET PARSEONLY ON (cambio de comportamiento)
Mediante el uso de la sintaxis `SET PARSEONLY ON`, un usuario puede hacer que el motor de SQL Data Warehouse examine la sintaxis de todas las instrucciones T-SQL y que devuelva los mensajes de error sin tener que compilar ni ejecutar la instrucción. Anteriormente, en la vista de sistema [sys.dm_pdw_exec_requests](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-requests-transact-sql), el estado de estas instrucciones se mantenía en estado `Running`. Ahora, en la vista `sys.dm_pdw_exec_requests`, se devolverá el estado `Complete`.

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
[Foro Stack Overflow]: http://stackoverflow.com/questions/tagged/azure-sqldw
[Twitter]: https://twitter.com/hashtag/SQLDW
[Vídeos]: https://azure.microsoft.com/documentation/videos/index/?services=sql-data-warehouse
[create a SQL Data Warehouse]: ./create-data-warehouse-portal.md
[Azure glossary]: ../azure-glossary-cloud-terminology.md
