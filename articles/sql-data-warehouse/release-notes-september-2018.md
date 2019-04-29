---
title: Notas de la versión de Azure SQL Data Warehouse de septiembre de 2018 | Microsoft Docs
description: Notas de la versión de Azure SQL Data Warehouse.
services: sql-data-warehouse
author: twounder
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: manage
ms.date: 10/08/2018
ms.author: mausher
ms.reviewer: twounder
ms.openlocfilehash: bc559a1224aace2ee599c24c8dce07a6d55173fd
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "61474985"
---
# <a name="whats-new-in-azure-sql-data-warehouse-september-2018"></a>¿Cuáles son las novedades de Azure SQL Data Warehouse? Septiembre de 2018
Continuamente se aplican mejoras a Azure SQL Data Warehouse. En este artículo se describen las nuevas características y los cambios que se han incluido en septiembre de 2018.

## <a name="new-lower-entry-point-for-sql-data-warehouse-gen2"></a>Nuevo punto de entrada inferior para SQL Data Warehouse Gen2
En abril de 2018, [Microsoft anunció](https://azure.microsoft.com/blog/turbocharge-cloud-analytics-with-azure-sql-data-warehouse/) Azure SQL Data Warehouse Gen2, que ofrece el mismo rendimiento x5, la escalabilidad de proceso x5, la simultaneidad x4 y almacenamiento ilimitado. Como se indicó en el artículo [Data Warehouse in the cloud Benchmark](https://gigaom.com/report/data-warehouse-in-the-cloud-benchmark/) (Prueba comparativa de Data Warehouse en la nube) de Gigaom, SQL Data Warehouse Gen2 **supera a Amazon Redshift en un 42 %**.

Gen2 ya está disponible con carácter general en un punto de entrada inferior de DWU500c, por lo que puede ejecutar en entornos de desarrollo y pruebas o de almacenamiento de datos de menor tamaño con las mejoras más recientes del servicio. El nuevo punto de entrada conserva todas las características de Gen2, incluido el [almacenamiento en caché adaptativo](https://azure.microsoft.com/blog/adaptive-caching-powers-azure-sql-data-warehouse-performance-gains/), el [aligeramiento de la reestructuración rápida de datos](https://azure.microsoft.com/blog/lightning-fast-query-performance-with-azure-sql-data-warehouse/) y compatibilidad para el [almacenamiento de datos en tiempo real](https://azure.microsoft.com/blog/enabling-real-time-data-warehousing-with-azure-sql-data-warehouse/).

## <a name="sql-vulnerability-assessment"></a>Evaluación de vulnerabilidades de SQL
[SQL Vulnerability Assessment (VA)](https://blogs.msdn.microsoft.com/sqlsecurity/2018/09/25/sql-vulnerability-assessment-now-supports-azure-sql-data-warehouse-and-azure-sql-database-managed-instance/) [Evaluación de vulnerabilidades de SQL (VA)] es un servicio sencillo de usar que supervisa continuamente el almacenamiento de datos. Le ayuda a garantizar un alto nivel de seguridad en todo momento y que se cumplen las directivas de la organización. Proporciona un informe de seguridad completa junto con los pasos de corrección aplicables para cada problema detectado. Este informe facilita la administración proactiva del estado de seguridad de las bases de datos además de centrar la atención hacia las acciones de mayor repercusión, incluso si no es experto en seguridad. En los entornos dinámicos donde los cambios son frecuentes y difíciles de seguir, VA tiene un valor incalculable en la detección de la configuración que puede ocasionar que el almacenamiento de datos sea vulnerable a ataques.

## <a name="improved-availability-with-query-restartability"></a>Disponibilidad mejorada con la capacidad de reinicio de las consultas
Durante la ejecución de las consultas pueden producirse problemas que provoquen errores en ellas. Un error de red o de hardware, u otros tipos de desconexión pueden provocar interrupciones. SQL Data Warehouse ahora admite la capacidad de reinicio para las consultas SELECT a nivel de instrucción o de pasos. 

Con la capacidad de reinicio de las consultas, las consultas en el aire interrumpidas por un error se reiniciarán automáticamente. Dependiendo del número de pasos, la forma de la consulta, o de dónde se haya detenido esta durante la ejecución, el motor de SQL Data Warehouse reiniciará la consulta completa o se reanudará desde el último paso de la consulta completado. Desde el punto de vista del usuario final, la consulta simplemente se completa. 

## <a name="maintenance-scheduling-preview"></a>Programación del mantenimiento (versión preliminar)
Programación de mantenimiento de Azure SQL Data Warehouse está ahora en versión preliminar. Esta nueva característica integra perfectamente las notificaciones de mantenimiento planeado de Service Health, el monitor de comprobación de Resource Health y el servicio de programación de mantenimiento de Azure SQL Data Warehouse. Programación de mantenimiento le permite programar un período de tiempo cuando sea conveniente recibir las nuevas características, actualizaciones y revisiones.

La programación del mantenimiento aprovecha Azure Monitor y permite a los clientes determinar cómo desean recibir las notificaciones de eventos de mantenimiento inminentes y qué flujos automatizados deben activarse para administrar el tiempo de inactividad y que afecte menos a sus operaciones. Las notificaciones pueden incluir texto o correo electrónico. 

## <a name="wide-row-support-in-polybase"></a>Compatibilidad con las filas amplias en PolyBase
Al cargar datos en SQL Data Warehouse, la regla general es utilizar [PolyBase](https://docs.microsoft.com/azure/sql-data-warehouse/design-elt-data-loading#options-for-loading-with-polybase) con su compatibilidad para cargar datos en paralelo. Esta versión habilita la compatibilidad con columnas más amplias, de 32 KB a 1 MB, lo que permite cargar tablas con filas amplias. La compatibilidad con las filas amplias simplifica el proceso de carga de datos en tablas de filas amplias.

> [!Note]
> El tamaño de fila total no puede superar 1 MB.

## <a name="additional-language-support"></a>Compatibilidad para lenguajes adicionales
Esta versión presenta compatibilidad con los elementos del lenguaje siguientes T-SQL:

### <a name="stringsplit"></a>STRING_SPLIT
La función [STRING_SPLIT](https://docs.microsoft.com/sql/t-sql/functions/string-split-transact-sql) divide una cadena de caracteres con el separador especificado. STRING_SPLIT es útil en escenarios de carga de datos donde una columna puede tener varios valores para analizar e insertarlos en otra tabla.

#### <a name="example"></a>Ejemplo
```sql
DECLARE @tags NVARCHAR(400) = 'clothing,road,,touring,bike';

SELECT
    value
FROM
    STRING_SPLIT(@tags, ',')
WHERE
    RTRIM(value) <> '';
```

### <a name="compressdecompress-functions"></a>Funciones COMPRESS/DECOMPRESS
Las funciones [COMPRESS](https://docs.microsoft.com/sql/t-sql/functions/compress-transact-sql) / [DECOMPRESS](https://docs.microsoft.com/sql/t-sql/functions/decompress-transact-sql) permiten comprimir o descomprimir una cadena de entrada con el algoritmo GZIP.

#### <a name="example"></a>Ejemplo

```sql
SELECT
    name [name_original]
    , COMPRESS(name) [name_compressed]
    , CAST(DECOMPRESS(COMPRESS(name)) AS NVARCHAR(MAX)) [name_decompressed]
FROM
    sys.objects;
```

### <a name="if-exists-clause-for-dropping-views"></a>Cláusula IF EXISTS para quitar vistas
La incorporación de la cláusula IF EXISTS en la instrucción [DROP VIEW](https://docs.microsoft.com/sql/t-sql/statements/drop-view-transact-sql) simplifica el código de T-SQL necesario para quitar una vista del almacenamiento de datos. Al aplicar la sintaxis IF EXISTS a una instrucción DROP VIEW se quitará la vista si existe o se pasará por alto la instrucción, en caso contrario.

#### <a name="example"></a>Ejemplo
```sql
DROP VIEW IF EXISTS dbo.TestView;
```
```
Message
--------------------------------------------------
Commands completed successfully.

```

## <a name="improved-compilation-time-for-singleton-inserts-and-ddl-statements"></a>Tiempo de compilación mejorado para las inserciones simples y las instrucciones DDL 
Seguir un modelo de extracción, transformación y carga (ETL) tradicional para la inserción de datos a menudo conduce a la ejecución de una inserción simple ([INSERTAR-VALORES](https://docs.microsoft.com/sql/t-sql/statements/insert-transact-sql)) en una tabla de la base de datos. Esta versión mejora el rendimiento de las operaciones de inserción simple al reducir el tiempo de compilación necesario para ejecutar este tipo de instrucción. En algunos casos, la mejora en la compilación consiste en una rapidez 3 veces mayor. Esta mejora reducirá el tiempo necesario para ejecutar una instrucción de inserción simple. 

La mejora también beneficia al almacenamiento de datos con gran cantidad de objetos al reducir de forma similar el tiempo de compilación de consultas para las instrucciones de lenguaje de definición de datos (DDL) como las operaciones CREATE, ALTER y DELETE. 

Por último, la mejora reduce la ejecución general de las instrucciones que se ejecutan en las tablas anchas (tablas que tienen un gran número de columnas). La mejora es una reducción del tiempo del paso de compilación de la consulta que reduce el tiempo de ejecución total de las consultas.

## <a name="bug-fixes"></a>Corrección de errores

| Título | DESCRIPCIÓN |
|:---|:---|
| **Corrección al crear las estadísticas en las distribuciones para las restricciones únicas** | Esta revisión soluciona un error que los usuarios experimentan al ejecutar UPDATE STATISTICS al especificar solo la tabla, cuando esta tiene una restricción única definida. |
| **Corrección al compilar las consultas en tablas externas** | Esta revisión corrige un defecto que afecta el tiempo de compilación de consultas que implican tablas externas.|
| **Corrección al ejecutar una instrucción con tipos de gran tamaño** | Soluciona un defecto en la compilación de la instrucción preparada con parámetros declarados como uno de los tipos *grandes* [nvarchar(max), varchar(max) y varbinary(max)]. |
| **Corrección cuando se produce un error en las consultas profundamente anidadas** | Proporciona un mensaje de error claro cuando una consulta profundamente anidada supera los límites del sistema.|
| **Corrección de errores en tiempo de compilación cuando una instrucción contiene una subconsulta correlacionada y una constante de tiempo de ejecución** |Resuelve el error en tiempo de compilación para las consultas con una combinación determinada de subconsultas correlacionadas y constantes en tiempo de ejecución [por ejemplo, GETDATE()].|
| **Corrección del tiempo de expiración para adquirir los bloqueos de objeto PDW y el espacio de simultaneidad para las estadísticas automáticas** |Revisión que agrega tiempo de expiración de bloqueo para evitar que las solicitudes de estadísticas automáticas bloqueen las solicitudes originales durante mucho tiempo.|

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
