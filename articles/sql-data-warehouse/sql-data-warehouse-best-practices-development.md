---
title: Procedimientos recomendados de desarrollo para Azure SQL Data Warehouse | Microsoft Docs
description: Recomendaciones y procedimientos recomendados que debe saber para desarrollar soluciones de Azure SQL Data Warehouse.
services: sql-data-warehouse
author: ronortloff
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: implement
ms.date: 09/04/2018
ms.author: rortloff
ms.reviewer: igorstan
ms.openlocfilehash: 1682a26ba713db564484e8984010e9c12ce9d79e
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2019
ms.locfileid: "57838517"
---
# <a name="development-best-practices-for-azure-sql-data-warehouse"></a>Procedimientos recomendados de desarrollo para Azure SQL Data Warehouse
En este artículo se describen la guía y los procedimientos recomendados a medida que desarrolla una solución de almacenamiento de datos. 

## <a name="reduce-cost-with-pause-and-scale"></a>Menos costos gracias a las características de pausa y escalado
Para más información acerca de cómo reducir los costos mediante la pausa y el escalado, consulte [Administración de procesos](sql-data-warehouse-manage-compute-overview.md). 


## <a name="maintain-statistics"></a>Mantenimiento de estadísticas
Asegúrese de actualizar las estadísticas diariamente o después de cada carga.  Existen inconvenientes entre el rendimiento y el costo de crear y actualizar las estadísticas. Si cree que tarda demasiado en realizar el mantenimiento de todas las estadísticas, puede intentar ser más selectivo acerca de las columnas con estadísticas o las que necesitan actualizarse con frecuencia.  Por ejemplo, puede actualizar las columnas de fecha, donde se añadan valores todos los días. **Sacará el máximo provecho con las estadísticas en columnas relacionadas con combinaciones, columnas que se usan en la cláusula WHERE y columnas de GROUP BY.**

Consulte también [Administración de estadísticas en tablas en SQL Data Warehouse][Manage table statistics], [CREATE STATISTICS][CREATE STATISTICS] y [UPDATE STATISTICS][UPDATE STATISTICS]

## <a name="hash-distribute-large-tables"></a>Distribución Hash para tablas grandes
De forma predeterminada, las tablas se distribuyen según el patrón Round Robin.  Esto facilita a los usuarios empezar a crear tablas sin tener que decidir sobre la distribución.  Las tablas round robin pueden ser suficientes para algunas cargas de trabajo, pero en la mayoría de los casos, la selección de una columna de distribución funcionará mucho opción.  El ejemplo más común de tabla distribuida por una columna que supera con creces a una Round Robin es al combinarse dos tablas grandes de hechos.  Por ejemplo, si tiene una tabla de pedidos, que se distribuye por order_id, y una tabla de transacciones, que también se distribuye por order_id, al unir la tabla de pedidos a la de transacciones en order_id, esta consulta se convierte en una consulta de paso a través, lo que significa que se eliminan las operaciones de movimiento de datos.  Menos pasos suponen consultas más rápidas.  Menos movimiento de datos también se traduce en consultas más rápidas.  Esta explicación es muy general. Al cargar una tabla con distribución, asegúrese de que no se ordenan los datos entrantes en la clave de distribución, ya que esto ralentizará la carga.  En los siguientes vínculos se muestra con mucho más detalle cómo seleccionar una columna de distribución mejora el rendimiento y cómo definir una tabla distribuida en la cláusula WITH de la instrucción CREATE TABLES.

Consulte también [Información general de tablas en SQL Data Warehouse][Table overview], [Distribución de tablas en SQL Data Warehouse][Table distribution], [Selección de la distribución de tablas][Selecting table distribution], [CREATE TABLE][CREATE TABLE], [CREATE TABLE AS SELECT][CREATE TABLE AS SELECT]

## <a name="do-not-over-partition"></a>Sin particiones excesivas
Crear particiones de datos puede ser muy eficaz para el mantenimiento de los datos mediante la modificación de particiones o exámenes de optimización, pero el exceso de particiones puede ralentizar las consultas.  A menudo una estrategia de división con granularidad alta que puede funcionar bien en SQL Server no funciona correctamente en SQL Data Warehouse.  El exceso de particiones también puede reducir la eficacia de los índices de almacén de columnas agrupadas si cada partición tiene menos de 1 millón de filas.  Tenga en cuenta que, en segundo plano, SQL Data Warehouse divide los datos automáticamente en 60 bases de datos, por lo que si crea una tabla con 100 particiones, se generan realmente 6000 particiones.  Cada carga de trabajo es diferente, por lo mejor es probar con las particiones para ver qué funciona mejor para la suya.  Considere la posibilidad de reducir la granularidad respecto a lo que le funcionaba en SQL Server.  Por ejemplo, puede usar particiones semanales o mensuales, en lugar de diarias.

Consulte también cómo [Creación de particiones de tablas en SQL Data Warehouse][Table partitioning]

## <a name="minimize-transaction-sizes"></a>Reducción del tamaño de las transacciones
Las instrucciones INSERT, UPDATE Y DELETE se ejecutan en las transacciones y, cuando fallan, deben deshacerse.  Para que no se tarde tanto en deshacer, reduzca el tamaño de las transacciones siempre que pueda.  Puede hacerlo si divide las instrucciones INSERT, UPDATE y DELETE en partes.  Por ejemplo, si tiene una instrucción INSERT que se suele tardar de 1 hora, si puede, divídala en 4 partes de 15 minutos cada una.  Aproveche los casos de registro mínimo, como CTAS, TRUNCATE, DROP TABLE o INSERT para vaciar las tablas y así reducir el riesgo de reversión.  Otra manera de eliminar reversiones es usar funciones de solo metadatos, como la modificación de particiones para la administración de datos.  Por ejemplo, en lugar de ejecutar una instrucción DELETE para eliminar todas las filas de una tabla cuyo order_date fuera octubre de 2001, podría dividir los datos mensualmente y desactivar la división con los datos de una partición vacía de otra tabla (consulte los ejemplos de ALTER TABLE).  Para tablas sin divisiones, puede usar CTAS en lugar de DELETE para escribir los datos que quiera mantener en una tabla.  Si CTAS tarda lo mismo, es una operación mucho más segura, ya que su registro de transacciones es mínimo y se puede cancelar rápidamente si es necesario.

Consulte también [Transacciones en SQL Data Warehouse][Understanding transactions], [Optimización de transacciones para SQL Data Warehouse][Optimizing transactions], [Creación de particiones de tablas en SQL Data Warehouse][Table partitioning], [TRUNCATE TABLE][TRUNCATE TABLE], [ALTER TABLE][ALTER TABLE], [Create table as select (CTAS) en SQL Data Warehouse][Create table as select (CTAS)]

## <a name="use-the-smallest-possible-column-size"></a>Uso del tamaño de columna mínimo
Al definir el DDL, usar el tipo de datos mínimo compatible con los datos mejorará el rendimiento de la consulta.  Esto tiene especial importancia para las columnas CHAR y VARCHAR.  Si el valor mayor máximo de una columna es 25 caracteres, defina la columna como VARCHAR(25).  Evite definir todas las columnas de caracteres con una longitud predeterminada de gran tamaño.  Defina las columnas como VARCHAR en lugar de NVARCHAR cuando no se necesite nada más.

Consulte también [Información general de tablas en SQL Data Warehouse][Table overview], [Tipos de datos de las tablas en SQL Data Warehouse][Table data types], [CREATE TABLE][CREATE TABLE]

## <a name="optimize-clustered-columnstore-tables"></a>Optimización de tablas de almacén de columnas agrupadas
Los índices de almacén de columnas agrupadas son una de las maneras más eficaces para almacenar datos en SQL Data Warehouse.  De forma predeterminada, las tablas de SQL Data Warehouse se crean como almacén de columnas agrupadas.  Para conseguir el máximo rendimiento de las consultas en las tablas de almacén de columnas, es importante la calidad de los segmentos.  Escriben filas en las tablas de almacén de columnas bajo presión de memoria afecta a la calidad de segmento.  La calidad de segmento se puede medir por el número de filas de un grupo de filas comprimido.  Para obtener instrucciones detalladas acerca de la detección y mejora de la calidad de los segmentos en las tablas de almacén de columnas agrupadas, consulte la sección [Causas de una calidad deficiente del índice de almacén de columnas][Causes of poor columnstore index quality] del artículo [Indexación de tablas en SQL Data Warehouse][Table indexes].  Como es importante que los segmentos de almacén de columnas sean de una buena calidad, es conveniente usar identificadores de usuario que se encuentren en la clase de recursos grande o mediana para cargar los datos. El uso de [unidades de almacenamiento de datos](what-is-a-data-warehouse-unit-dwu-cdwu.md) inferiores significa que desea asignar una clase de recurso mayor para el usuario que realiza la carga.

Dado que las tablas de almacén de columnas generalmente no insertan datos en un segmento del almacén de columnas comprimido hasta que hay más de 1 millón de filas por tabla y cada tabla de SQL Data Warehouse se divide en 60 partes, como norma general, las tablas de almacén de columnas no serán útiles para las consultas a menos que la tabla tenga más de 60 millones de filas.  Para las tablas con menos de 60 millones de filas, podría no tener sentido el índice de almacén de columnas.  Pero tampoco molesta.  Además, si divide los datos, recuerde que cada parte deberá tener 1 millón de filas para beneficiarse de un índice de almacén de columnas agrupadas.  Si una tabla tiene 100 particiones, deberá tener al menos 6 mil millones de filas para beneficiarse del almacén de columnas agrupadas (60 distribuciones * 100 particiones * 1 millón de filas).  Si la tabla no tiene 6 mil millones de filas en este ejemplo, reduzca el número de particiones o considere la posibilidad de usar una tabla de apilamiento en su lugar.  También puede experimentar para ver si consigue un mejor rendimiento con una tabla de apilamiento con índices secundarios, en lugar de con una tabla de almacén de columnas.

Al consultar una tabla de almacén de columnas, las consultas se ejecutarán más rápido si selecciona solo las que necesita.  

Consulte también [Indexación de tablas en SQL Data Warehouse][Table indexes], [Guía de índices de almacén de columnas][Columnstore indexes guide] y [Regeneración de índices de almacén de columnas][Rebuilding columnstore indexes]

## <a name="next-steps"></a>Pasos siguientes
Si no encuentra lo que buscaba en este artículo, pruebe a usar la "búsqueda de documentos" en la parte izquierda de esta página para buscar todos los documentos de Azure SQL Data Warehouse.  El [foro de Azure SQL Data Warehouse][Azure SQL Data Warehouse MSDN Forum] es un lugar para formular preguntas a otros usuarios y al grupo de productos de SQL Data Warehouse.  Supervisamos continuamente este foro para garantizar que sus preguntas las responde otro usuario o alguno de nosotros.  Si prefiere formular sus preguntas en Stack Overflow, también tenemos un [foro de Stack Overflow acerca de Azure SQL Data Warehouse][Azure SQL Data Warehouse Stack Overflow Forum].

Por último, use la página [de comentarios de Azure SQL Data Warehouse][Azure SQL Data Warehouse Feedback] para realizar solicitudes de características.  Añadir sus solicitudes o valoraciones positivas sobre otras solicitudes realmente nos ayuda a priorizar las características.

<!--Image references-->

<!--Article references-->
[Create a support ticket]: ./sql-data-warehouse-get-started-create-support-ticket.md
[Create table as select (CTAS)]: ./sql-data-warehouse-develop-ctas.md
[Table overview]: ./sql-data-warehouse-tables-overview.md
[Table data types]: ./sql-data-warehouse-tables-data-types.md
[Table distribution]: ./sql-data-warehouse-tables-distribute.md
[Table indexes]: ./sql-data-warehouse-tables-index.md
[Causes of poor columnstore index quality]: ./sql-data-warehouse-tables-index.md#causes-of-poor-columnstore-index-quality
[Rebuilding columnstore indexes]: ./sql-data-warehouse-tables-index.md#rebuilding-indexes-to-improve-segment-quality
[Table partitioning]: ./sql-data-warehouse-tables-partition.md
[Manage table statistics]: ./sql-data-warehouse-tables-statistics.md
[Temporary tables]: ./sql-data-warehouse-tables-temporary.md
[Guide for using PolyBase]: ./guidance-for-loading-data.md
[Load data]: ./design-elt-data-loading.md
[Move data with Azure Data Factory]: ../data-factory/transform-data-using-machine-learning.md
[Load data with Azure Data Factory]: ../data-factory/load-azure-sql-data-warehouse.md
[Load data with bcp]: /sql/tools/bcp-utility
[Load data with PolyBase]: ./load-data-wideworldimportersdw.md
[Monitor your workload using DMVs]: ./sql-data-warehouse-manage-monitor.md
[Pause compute resources]: ./sql-data-warehouse-manage-compute-overview.md#pause-compute-bk
[Resume compute resources]: ./sql-data-warehouse-manage-compute-overview.md#resume-compute-bk
[Scale compute resources]: ./sql-data-warehouse-manage-compute-overview.md#scale-compute
[Understanding transactions]: ./sql-data-warehouse-develop-transactions.md
[Optimizing transactions]: ./sql-data-warehouse-develop-best-practices-transactions.md
[Troubleshooting]: ./sql-data-warehouse-troubleshoot.md
[LABEL]: ./sql-data-warehouse-develop-label.md

<!--MSDN references-->
[ALTER TABLE]: https://msdn.microsoft.com/library/ms190273.aspx
[CREATE EXTERNAL FILE FORMAT]: https://msdn.microsoft.com/library/dn935026.aspx
[CREATE STATISTICS]: https://msdn.microsoft.com/library/ms188038.aspx
[CREATE TABLE]: https://msdn.microsoft.com/library/mt203953.aspx
[CREATE TABLE AS SELECT]: https://msdn.microsoft.com/library/mt204041.aspx
[DBCC PDW_SHOWEXECUTIONPLAN]: https://msdn.microsoft.com/library/mt204017.aspx
[INSERT]: https://msdn.microsoft.com/library/ms174335.aspx
[OPTION]: https://msdn.microsoft.com/library/ms190322.aspx
[TRUNCATE TABLE]: https://msdn.microsoft.com/library/ms177570.aspx
[UPDATE STATISTICS]: https://msdn.microsoft.com/library/ms187348.aspx
[sys.dm_exec_sessions]: https://msdn.microsoft.com/library/ms176013.aspx
[sys.dm_pdw_exec_requests]: https://msdn.microsoft.com/library/mt203887.aspx
[sys.dm_pdw_request_steps]: https://msdn.microsoft.com/library/mt203913.aspx
[sys.dm_pdw_sql_requests]: https://msdn.microsoft.com/library/mt203889.aspx
[sys.dm_pdw_dms_workers]: https://msdn.microsoft.com/library/mt203878.aspx
[sys.dm_pdw_waits]: https://msdn.microsoft.com/library/mt203893.aspx
[Columnstore indexes guide]: https://msdn.microsoft.com/library/gg492088.aspx

<!--Other Web references-->
[Selecting table distribution]: https://blogs.msdn.microsoft.com/sqlcat/20../../choosing-hash-distributed-table-vs-round-robin-distributed-table-in-azure-sql-dw-service/
[Azure SQL Data Warehouse Feedback]: https://feedback.azure.com/forums/307516-sql-data-warehouse
[Azure SQL Data Warehouse MSDN Forum]: https://social.msdn.microsoft.com/Forums/sqlserver/home?forum=AzureSQLDataWarehouse
[Azure SQL Data Warehouse Stack Overflow Forum]:  https://stackoverflow.com/questions/tagged/azure-sqldw
[Azure SQL Data Warehouse loading patterns and strategies]: https://blogs.msdn.microsoft.com/sqlcat/20../../azure-sql-data-warehouse-loading-patterns-and-strategies/
