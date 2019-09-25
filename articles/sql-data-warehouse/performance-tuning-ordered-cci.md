---
title: Optimización del rendimiento con el índice de almacén de columnas agrupado ordenado de Azure SQL Data Warehouse| Microsoft Docs
description: Recomendaciones y consideraciones que debe conocer al usar el índice de almacén de columnas agrupado ordenado para mejorar el rendimiento de las consultas.
services: sql-data-warehouse
author: XiaoyuMSFT
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: development
ms.date: 09/05/2019
ms.author: xiaoyul
ms.reviewer: nibruno; jrasnick
ms.openlocfilehash: 41fbebcf4b85f6e48babba30c2d05fedb3e7a5c7
ms.sourcegitcommit: 909ca340773b7b6db87d3fb60d1978136d2a96b0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2019
ms.locfileid: "70985271"
---
# <a name="performance-tuning-with-ordered-clustered-columnstore-index"></a>Optimización del rendimiento con el índice de almacén de columnas agrupado ordenado  

Cuando los usuarios consultan una tabla del almacén de columnas de Azure SQL Data Warehouse, el optimizador comprueba los valores mínimo y máximo almacenados en cada segmento.  Los segmentos que están fuera de los límites del predicado de la consulta no se leen del disco a la memoria.  Una consulta puede tener un rendimiento más rápido si el número de segmentos que se van a leer y su tamaño total son pequeños.   

## <a name="ordered-vs-non-ordered-clustered-columnstore-index"></a>Índice de almacén de columnas agrupado ordenado frente al no ordenado 
De forma predeterminada, para cada tabla de Azure Data Warehouse que se crea sin una opción de índice, un componente interno (generador de índices) crea un índice de almacén de columnas agrupado (CCI) no ordenado.  Los datos de cada columna se comprimen en un segmento de grupo de filas de CCI independiente.  Hay metadatos en el intervalo de valores de cada segmento, por lo que los segmentos que están fuera de los límites del predicado de la consulta no se leen desde el disco durante la ejecución de la consulta.  CCI ofrece el máximo nivel de compresión de datos y reduce el tamaño de los segmentos que se van a leer para que las consultas se ejecuten más rápido. Sin embargo, dado que el generador de índices no ordena los datos antes de comprimirlos en segmentos, pueden darse segmentos con intervalos de valores superpuestos, lo que hace que las consultas lean más segmentos del disco y tarden más en finalizar.  

Al crear un CCI ordenado, el motor de Azure SQL Data Warehouse ordena los datos en memoria por las claves de orden antes de que el generador de índices los comprima en segmentos de índice.  Con los datos ordenados, se reduce la superposición de segmentos, lo que permite que las consultas tengan una eliminación de segmentos más eficaz y, por tanto, un rendimiento más rápido, ya que el número de segmentos que se leerán desde el disco es menor.  Si todos los datos se pueden ordenar en memoria de una vez, se puede evitar la superposición de segmentos.  Dado el gran tamaño de los datos de las tablas del almacenamiento de datos, este escenario no se produce con frecuencia.  

Para comprobar los intervalos de segmentos de una columna, ejecute este comando con el nombre de la tabla y el nombre de la columna:

```sql
SELECT o.name, pnp.index_id, pnp.rows, pnp.data_compression_desc, pnp.pdw_node_id, 
pnp.distribution_id, cls.segment_id, cls.column_id, cls.min_data_id, cls.max_data_id, cls.max_data_id-cls.min_data_id as difference
FROM sys.pdw_nodes_partitions AS pnp
   JOIN sys.pdw_nodes_tables AS Ntables ON pnp.object_id = NTables.object_id AND pnp.pdw_node_id = NTables.pdw_node_id
   JOIN sys.pdw_table_mappings AS Tmap  ON NTables.name = TMap.physical_name AND substring(TMap.physical_name,40, 10) = pnp.distribution_id
   JOIN sys.objects AS o ON TMap.object_id = o.object_id
   JOIN sys.pdw_nodes_column_store_segments AS cls ON pnp.partition_id = cls.partition_id AND pnp.distribution_id  = cls.distribution_id
   JOIN sys.columns as cols ON o.object_id = cols.object_id AND cls.column_id = cols.column_id
WHERE o.name = '<table_name>' and c.name = '<column_name>'
ORDER BY o.name, pnp.distribution_id, cls.min_data_id
```

## <a name="data-loading-performance"></a>Rendimiento de la carga de datos

El rendimiento de la carga de datos en una tabla de CCI ordenada es similar a la carga de datos en una tabla con particiones.  
La carga de datos en una tabla de CCI ordenada puede tardar más tiempo que la carga de datos en una tabla de CCI no ordenada debido a la ordenación de los datos.  

A continuación se muestra un ejemplo de comparación de rendimiento de la carga de datos en tablas con esquemas diferentes.
![Comparativa de rendimiento de la carga de datos](media/performance-tuning-ordered-cci/cci-data-loading-performance.png)
 
## <a name="reduce-segment-overlapping"></a>Reducción de la superposición de segmentos
A continuación se muestran opciones para reducir aún más la superposición de segmentos al crear un CCI ordenado en una nueva tabla mediante CTAS o en una tabla existente con datos:

- Use una clase de recurso mayor para permitir que se ordenen más datos a la vez en memoria antes de que el generador de índices los comprima en segmentos.  Una vez que está en un segmento de índice, no se puede cambiar la ubicación física de los datos.  No hay ninguna ordenación de datos dentro de un segmento o entre segmentos.  

- Use un grado de paralelismo más bajo (DOP = 1, por ejemplo).  Cada subproceso que se usa para la creación del CCI ordenado trabaja en un subconjunto de datos y lo ordena localmente.  No hay ninguna ordenación global entre los datos ordenados por subprocesos diferentes.  El uso de subprocesos paralelos puede reducir el tiempo de creación de un CCI ordenado, pero se generarán más segmentos superpuestos que con el uso de un único subproceso. 
- Ordene previamente los datos por las claves de ordenación antes de cargarlos en tablas de Azure SQL Data Warehouse.

## <a name="create-ordered-cci-on-large-tables"></a>Creación de un CCI ordenado en tablas grandes
La creación de un CCI ordenado es una operación sin conexión.  En el caso de tablas sin particiones, los datos no serán accesibles para los usuarios hasta que se complete el proceso de creación del CCI ordenado.   En el caso de tablas con particiones, como el motor crea el CCI ordenado partición a partición, los usuarios todavía pueden acceder a los datos de las particiones en las que la creación del CCI ordenado no está en curso.   Puede usar esta opción para minimizar el tiempo de inactividad durante la creación del CCI ordenado en tablas grandes: 

1.  Cree particiones en la tabla grande de destino (llamada tabla A).
2.  Cree una tabla de CCI ordenado vacía (llamada tabla B) con la misma tabla y esquema de particiones que la tabla A.
3.  Cambie una partición de la tabla A a la tabla B.
4.  Ejecute ALTER INDEX <Índice_CCI_ordenado> REBUILD en la tabla B para volver a generar la partición cambiada.  
5.  Repita los pasos 3 y 4 para cada partición de la tabla A.
6.  Una vez que todas las particiones se han cambiado de la tabla A a la tabla B y se han vuelto a generar, elimine la tabla A y cambie el nombre de la tabla B a tabla A. 

## <a name="examples"></a>Ejemplos

**A. Para comprobar las columnas ordenadas y el ordinal del orden:**
```sql
SELECT object_name(c.object_id) table_name, c.name column_name, i.column_store_order_ordinal 
FROM sys.index_columns i 
JOIN sys.columns c ON i.object_id = c.object_id AND c.column_id = i.column_id
WHERE column_store_order_ordinal <>0
```

**B. Para cambiar el ordinal de la columna, agregar o eliminar columnas de la lista de ordenación o cambiar de CCI a CCI ordenado:**
```sql
CREATE CLUSTERED COLUMNSTORE INDEX InternetSales ON  InternetSales
ORDER (ProductKey, SalesAmount)
WITH (DROP_EXISTING = ON)
```

## <a name="next-steps"></a>Pasos siguientes
Para obtener más sugerencias sobre desarrollo, consulte la [información general sobre desarrollo de SQL Data Warehouse](sql-data-warehouse-overview-develop.md).
