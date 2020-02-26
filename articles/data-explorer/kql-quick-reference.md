---
title: Referencia rápida de KQL
description: Una lista de funciones de KQL útiles y sus definiciones con ejemplos de sintaxis.
author: yossi-karp
ms.author: v-yokarp
ms.reviewer: ''
ms.service: data-explorer
ms.topic: conceptual
ms.date: 01/19/2020
ms.openlocfilehash: b53890afd10a3aee131ab3897d01e6b6fdf261a6
ms.sourcegitcommit: b8f2fee3b93436c44f021dff7abe28921da72a6d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/18/2020
ms.locfileid: "77429240"
---
# <a name="kql-quick-reference"></a>Referencia rápida de KQL

En este artículo se muestra una lista de funciones y sus descripciones para ayudarle a comenzar a usar Kusto Query Language.

| Operador o función                               | Descripción                           | Sintaxis                                           |
| :---------------------------------------------- | :------------------------------------ |:-------------------------------------------------|
|**Filtro, búsqueda o condición**                      |**_Búsqueda de datos pertinentes mediante filtrado o búsqueda_** |                      |
| [where](/azure/kusto/query/whereoperator.md)                      | Filtra por un predicado específico.           | `T | where Predicate`                         |
| [where contains/has](/azure/kusto/query/whereoperator.md)        | `Contains`: busca cualquier coincidencia de subcadena. <br> `Has`: busca una palabra específica (mejor rendimiento).  | `T | where col1 contains/has "[search term]"`|
| [search](/azure/kusto/query/searchoperator.md)                    | busca el valor en todas las columnas de la tabla. | `[TabularSource |] search [kind=CaseSensitivity] [in (TableSources)] SearchPredicate` |
| [take](/azure/kusto/query/takeoperator.md)                        | devuelve el número de registros especificado. Úselo para probar una consulta.<br>**_Nota_** : `_take`_ y `_limit`_ son sinónimos. | `T | take NumberOfRows` |
| [case](/azure/kusto/query/casefunction.md)                        | Agrega una instrucción de condición, similar a if/then/elseif en otros sistemas. | `case(predicate_1, then_1, predicate_2, then_2, predicate_3, then_3, else)` |
| [distinct](/azure/kusto/query/distinctoperator.md)                | Genera una tabla con la combinación distinta de las columnas proporcionadas de la tabla de entrada. | `distinct [ColumnName], [ColumnName]` |
| **Date/Time**                                   |**_Operaciones que usan funciones de fecha y hora_**               |                          |
|[ago](/azure/kusto/query/agofunction.md)                           | Devuelve el desfase horario en relación con la hora a la que se ejecuta la consulta. Por ejemplo, `ago(1h)` es una hora antes de la lectura del reloj actual. | `ago(a_timespan)` |
| [format_datetime](/azure/kusto/query/format-datetimefunction.md)  | Devuelve datos en [varios formatos de fecha](/azure/kusto/query/format-datetimefunction.md#supported-formats). | `format_datetime(datetime , format)` |
| [bin](/azure/kusto/query/binfunction.md)                          | Redondea todos los valores de un período de tiempo y los agrupa. | `bin(value,roundTo)` |
| **Creación o eliminación de columnas**                   |**_Adición o eliminación de columnas de una tabla_** |                                                    |
| [print](/azure/kusto/query/printoperator.md)                      | Genera una sola fila con una o más expresiones escalares. | `print [ColumnName =] ScalarExpression [',' ...]` |
| [project](/azure/kusto/query/projectoperator.md)                  | Selecciona las columnas que se van a incluir en el orden especificado. | `T | project ColumnName [= Expression] [, ...]` <br> Or <br> `T | project [ColumnName | (ColumnName[,]) =] Expression [, ...]` |
| [project-away](/azure/kusto/query/projectawayoperator.md)         | Selecciona las columnas que se van a excluir de la salida. | `T | project-away ColumnNameOrPattern [, ...]` |
| [extend](/azure/kusto/query/extendoperator.md)                    | Crea una columna calculada y la agrega al conjunto de resultados. | `T | extend [ColumnName | (ColumnName[, ...]) =] Expression [, ...]` |
| **Ordenación y agregación del conjunto de datos**                 |**_Reestructuración de los datos mediante su ordenación y agrupación de formas significativas_**|                  |
| [sort](/azure/kusto/query/sortoperator.md)                        | Ordena las filas de la tabla de entrada por una o varias columnas en orden ascendente o descendente. | `T | sort by expression1 [asc|desc], expression2 [asc|desc], …` |
| [top](/azure/kusto/query/topoperator.md)                          | Devuelve las n primeras filas del conjunto de datos cuando este se ordena mediante `by` | `T | top numberOfRows by expression [asc|desc] [nulls first|last]` |
| [summarize](/azure/kusto/query/summarizeoperator.md)              | Agrupa las filas según las columnas de grupo `by` y calcula las agregaciones en cada grupo. | `T | summarize [[Column =] Aggregation [, ...]] [by [Column =] GroupExpression [, ...]]` |
| [count](/azure/kusto/query/countoperator.md)                       | Cuenta los registros de la tabla de entrada (por ejemplo, T).<br>Este operador es una abreviatura de `summarize count() `.| `T | count` |
| [join](/azure/kusto/query/joinoperator.md)                        | Combina las filas de dos tablas para formar una nueva tabla haciendo coincidir los valores de las columnas especificadas de cada tabla. Admite una gama completa de tipos de combinación: `flouter`, `inner`, `innerunique`, `leftanti`, `leftantisemi`, `leftouter`, `leftsemi`, `rightanti`, `rightantisemi`, `rightouter`, `rightsemi` | `LeftTable | join [JoinParameters] ( RightTable ) on Attributes` |
| [union](/azure/kusto/query/unionoperator.md)                      | Toma dos o más tablas y devuelve todas sus filas. | `[T1] | union [T2], [T3], …` |
| [range](/azure/kusto/query/rangeoperator.md)                      | Genera una tabla con una serie aritmética de valores. | `range columnName from start to stop step step` |
| **Aplicación de formato a los datos**                                 | **_Reestructuración de los datos para que se generen de forma útil_** | |
| [lookup](/azure/kusto/query/lookupoperator.md)                    | Extiende las columnas de una tabla de hechos con valores buscados en una tabla de dimensiones. | `T1 | lookup [kind = (leftouter|inner)] ( T2 ) on Attributes` |
| [mv-expand](/azure/kusto/query/mvexpandoperator.md)               | Convierte las matrices dinámicas en filas (expansión de varios valores). | `T | mv-expand Column` |
| [parse](/azure/kusto/query/parseoperator.md)                      | evalúa una expresión de cadena y analiza su valor en una o más columnas calculadas. Úselo para estructurar datos no estructurados. | `T | parse [kind=regex  [flags=regex_flags] |simple|relaxed] Expression with * (StringConstant ColumnName [: ColumnType]) *...` |
| [make-series](/azure/kusto/query/make-seriesoperator.md)          | Crea una serie de valores agregados especificados a lo largo de un eje definido. | `T | make-series [MakeSeriesParamters] [Column =] Aggregation [default = DefaultValue] [, ...] on AxisColumn from start to end step step [by [Column =] GroupExpression [, ...]]` |
| [let](/azure/kusto/query/letstatement.md)                         | Enlaza un nombre a expresiones que pueden hacer referencia a su valor enlazado. Los valores pueden ser expresiones lambda para crear funciones ad-hoc como parte de la consulta. Use `let` para crear expresiones sobre tablas cuyos resultados se parezcan a una nueva tabla. | `let Name = ScalarExpression | TabularExpression | FunctionDefinitionExpression` |
| **General**                                     | **_Operaciones y funciones mixtas_** | |
| [invoke](/azure/kusto/query/invokeoperator.md)                    | Ejecuta la función en la tabla que recibe como entrada. | `T | invoke function([param1, param2])` |
| [evaluate pluginName](/azure/kusto/query/evaluateoperator.md)     | Evalúa las extensiones del lenguaje de consulta (complementos). | `[T |] evaluate [ evaluateParameters ] PluginName ( [PluginArg1 [, PluginArg2]... )` |
| **Visualización**                               | **_Operaciones que muestran los datos en formato gráfico_** | |
| [render](/azure/kusto/query/renderoperator.md) | Representa los resultados como una salida gráfica. | `T | render Visualization [with (PropertyName = PropertyValue [, ...] )]` |
