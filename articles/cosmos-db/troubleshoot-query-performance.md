---
title: Solución de problemas de consulta al usar Azure Cosmos DB
description: Aprenda a identificar, diagnosticar y solucionar problemas de consultas SQL de Azure Cosmos DB.
author: timsander1
ms.service: cosmos-db
ms.topic: troubleshooting
ms.date: 02/10/2020
ms.author: tisande
ms.subservice: cosmosdb-sql
ms.reviewer: sngun
ms.openlocfilehash: 0dd3cb12c52e23a0a8acd57bf401ba68acfb9925
ms.sourcegitcommit: 5a71ec1a28da2d6ede03b3128126e0531ce4387d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/26/2020
ms.locfileid: "77623687"
---
# <a name="troubleshoot-query-issues-when-using-azure-cosmos-db"></a>Solución de problemas de consulta al usar Azure Cosmos DB

Este artículo le guía por un enfoque recomendado general para la solución de problemas con las consultas en Azure Cosmos DB. Aunque los pasos descritos en este documento no se deben considerar un recopilatorio de posibles problemas de consulta, hemos incluido las sugerencias de mejora del rendimiento más comunes. Use este documento como punto de partida para la solución de problemas con las consultas lentas o costosas en Core (SQL) API de Azure Cosmos DB. También puede usar los [registros de diagnóstico](cosmosdb-monitor-resource-logs.md) para identificar las consultas lentas o que reducen considerablemente el rendimiento.

En Azure Cosmos DB existen amplias categorías de optimización de consulta: optimizaciones que reducen la carga de unidades de solicitud (RU) de la consulta y otras que solo reducen la latencia. Al reducir la carga de RU de una consulta, seguramente también reduzca la latencia.

En este documento se usan ejemplos que se pueden recrear con el conjunto de datos [nutrition](https://github.com/CosmosDB/labs/blob/master/dotnet/setup/NutritionData.json).

## <a name="important"></a>Importante

- Para obtener el mejor rendimiento, siga las [Sugerencias para mejorar el rendimiento](performance-tips.md).
    > [!NOTE] 
    > El proceso de host de Windows de 64 bits se recomienda para mejorar el rendimiento. El SDK de SQL incluye un ServiceInterop.dll nativo para analizar y optimizar consultas localmente, y solo se admite en la plataforma Windows x64. En el caso de Linux y otras plataformas no compatibles donde el ServiceInterop.dll no está disponible, realizará una llamada de red adicional a la puerta de enlace para obtener la consulta optimizada. 
- La consulta de Cosmos DB no admite un recuento mínimo de elementos.
    - El código debe controlar cualquier tamaño de página entre 0 y el recuento máximo de elementos
    - El número de elementos de una página puede cambiar sin previo aviso, y lo hará.
- Se esperan páginas vacías para las consultas y pueden aparecer en cualquier momento. 
    - El motivo por el que se exponen páginas vacías en los SDK es que permite más oportunidades para cancelar la consulta. También deja claro que el SDK realiza varias llamadas de red.
    - Las páginas vacías pueden mostrarse en cargas de trabajo existentes, ya que una partición física se divide en Cosmos DB. La primera partición tiene 0 resultados actualmente, lo que da lugar a la página vacía.
    - Las páginas vacías se deben al back-end que adelanta la consulta porque esta tarda más que cierto tiempo fijo en el back-end para recuperar los documentos. Si Cosmos DB adelanta una consulta, devolverá un token de continuación que permitirá que continúe la consulta. 
- Asegúrese de purgar la consulta completamente. Revise los ejemplos de SDK y use un bucle WHILE en los `FeedIterator.HasMoreResults` para purgar toda la consulta.

### <a name="obtaining-query-metrics"></a>Obtención de las métricas de consulta:

Al optimizar una consulta en Azure Cosmos DB, el primer paso es siempre [obtener las métricas de la consulta](profile-sql-api-query.md). Estas también están disponibles en Azure Portal, tal y como se muestra a continuación:

[ ![Obtención de las métricas de consulta](./media/troubleshoot-query-performance/obtain-query-metrics.png) ](./media/troubleshoot-query-performance/obtain-query-metrics.png#lightbox)

Después de obtener las métricas de consulta, compare el recuento de documentos recuperados con el de documentos de salida de la consulta. Use esta comparación para identificar las secciones pertinentes a las que se hará referencia a continuación.

El recuento de documentos recuperado es el número de documentos que la consulta necesitaba cargar. El recuento de documentos de salida es el número de documentos necesarios para los resultados de la consulta. Si el recuento de documentos recuperados es significativamente mayor que el de documentos de salida, habrá al menos una parte de la consulta que no haya podido usar el índice y haya tenido que realizar un examen.

Puede consultar la sección siguiente para entender las optimizaciones de consulta pertinentes para su escenario:

### <a name="querys-ru-charge-is-too-high"></a>El cargo por RU de la consulta es demasiado elevado

#### <a name="retrieved-document-count-is-significantly-greater-than-output-document-count"></a>El recuento de documentos recuperados es significativamente mayor que el de documentos de salida

- [Inclusión de las rutas de acceso necesarias en la directiva de indexación](#include-necessary-paths-in-the-indexing-policy)

- [Conocimiento de las funciones del sistema que usan el índice](#understand-which-system-functions-utilize-the-index)

- [Consultas con filtro y una cláusula ORDER BY](#queries-with-both-a-filter-and-an-order-by-clause)

- [Optimización de las expresiones JOIN mediante una subconsulta](#optimize-join-expressions-by-using-a-subquery)

<br>

#### <a name="retrieved-document-count-is-approximately-equal-to-output-document-count"></a>Recuento de documentos recuperados aproximadamente igual al de documentos de salida

- [Cómo evitar las particiones cruzadas](#avoid-cross-partition-queries)

- [Filtros en varias propiedades](#filters-on-multiple-properties)

- [Consultas con filtro y una cláusula ORDER BY](#queries-with-both-a-filter-and-an-order-by-clause)

<br>

### <a name="querys-ru-charge-is-acceptable-but-latency-is-still-too-high"></a>El cargo por RU de la consulta es aceptable, pero la latencia sigue siendo demasiado elevada

- [Mejora de la proximidad](#improve-proximity)

- [Aumento de la capacidad de proceso aprovisionada](#increase-provisioned-throughput)

- [Aumento de MaxConcurrency](#increase-maxconcurrency)

- [Aumento de MaxBufferedItemCount](#increase-maxbuffereditemcount)

## <a name="queries-where-retrieved-document-count-exceeds-output-document-count"></a>Consultas en las que el recuento de documentos recuperados supera el de documentos de salida

 El recuento de documentos recuperado es el número de documentos que la consulta necesitaba cargar. El recuento de documentos de salida es el número de documentos necesarios para los resultados de la consulta. Si el recuento de documentos recuperados es significativamente mayor que el de documentos de salida, habrá al menos una parte de la consulta que no haya podido usar el índice y haya tenido que realizar un examen.

 A continuación se muestra un ejemplo de consulta de examen que el índice no sirvió completamente.

Consulta:

 ```sql
SELECT VALUE c.description
FROM c
WHERE UPPER(c.description) = "BABYFOOD, DESSERT, FRUIT DESSERT, WITHOUT ASCORBIC ACID, JUNIOR"
 ```

Métricas de la consulta:

```
Retrieved Document Count                 :          60,951
Retrieved Document Size                  :     399,998,938 bytes
Output Document Count                    :               7
Output Document Size                     :             510 bytes
Index Utilization                        :            0.00 %
Total Query Execution Time               :        4,500.34 milliseconds
  Query Preparation Times
    Query Compilation Time               :            0.09 milliseconds
    Logical Plan Build Time              :            0.05 milliseconds
    Physical Plan Build Time             :            0.04 milliseconds
    Query Optimization Time              :            0.01 milliseconds
  Index Lookup Time                      :            0.01 milliseconds
  Document Load Time                     :        4,177.66 milliseconds
  Runtime Execution Times
    Query Engine Times                   :          322.16 milliseconds
    System Function Execution Time       :           85.74 milliseconds
    User-defined Function Execution Time :            0.00 milliseconds
  Document Write Time                    :            0.01 milliseconds
Client Side Metrics
  Retry Count                            :               0
  Request Charge                         :        4,059.95 RUs
```

El recuento de documentos recuperados (60 951) es significativamente mayor que el de documentos de salida (7), por lo que esta consulta necesitaba realizar un examen. En este caso, la función del sistema [UPPER()](sql-query-upper.md) no utiliza el índice.

## <a name="include-necessary-paths-in-the-indexing-policy"></a>Inclusión de las rutas de acceso necesarias en la directiva de indexación

La directiva de indexación debe cubrir cualquier propiedad incluida en las cláusulas `WHERE`, `ORDER BY`, `JOIN` y la mayoría de las funciones del sistema. La ruta de acceso especificada en la directiva de indexación debe coincidir (distingue entre mayúsculas y minúsculas) con la propiedad de los documentos JSON.

Si ejecutamos una consulta simple en el conjunto de datos [nutrition](https://github.com/CosmosDB/labs/blob/master/dotnet/setup/NutritionData.json), observamos un cargo por RU mucho menor cuando la propiedad de la cláusula `WHERE` está indexada.

### <a name="original"></a>Original

Consulta:

```sql
SELECT * FROM c WHERE c.description = "Malabar spinach, cooked"
```

Directiva de indexación:

```json
{
    "indexingMode": "consistent",
    "automatic": true,
    "includedPaths": [
        {
            "path": "/*"
        }
    ],
    "excludedPaths": [
        {
            "path": "/description/*"
        }
    ]
}
```

**Cargo por unidad de solicitud:** 409,51 RU

### <a name="optimized"></a>Optimizado

Directiva de indexación actualizada:

```json
{
    "indexingMode": "consistent",
    "automatic": true,
    "includedPaths": [
        {
            "path": "/*"
        }
    ],
    "excludedPaths": []
}
```

**Cargo por unidad de solicitud:** 2,98 RU

Puede agregar propiedades adicionales a la directiva de indexación en cualquier momento, sin que ello afecte a la disponibilidad o el rendimiento de la escritura. Si agrega una nueva propiedad al índice, las consultas que usen esta propiedad utilizarán inmediatamente el nuevo índice disponible. La consulta utilizará el nuevo índice mientras se está generando. En consecuencia, los resultados de la consulta pueden ser incoherentes con la recompilación del índice en curso. Si se indexa una nueva propiedad, las consultas que solo utilicen índices existentes no se verán afectadas durante la recompilación del índice. Puede [realizar el seguimiento del progreso de transformación del índice](https://docs.microsoft.com/azure/cosmos-db/how-to-manage-indexing-policy#use-the-net-sdk-v3).

## <a name="understand-which-system-functions-utilize-the-index"></a>Conocimiento de las funciones del sistema que usan el índice

Si la expresión se puede traducir en un intervalo de valores de cadena, puede usar el índice; de lo contrario, no puede.

Esta es la lista de funciones de cadena que pueden utilizar el índice:

- STARTSWITH(str_expr, str_expr)
- LEFT(str_expr, num_expr) = str_expr
- SUBSTRING(str_expr, num_expr, num_expr) = str_expr, pero solo si el primer valor de num_expr es 0

A continuación se indican algunas funciones del sistema comunes que no usan el índice y que deben cargar cada documento:

| **Función del sistema**                     | **Ideas de optimización**             |
| --------------------------------------- |------------------------------------------------------------ |
| CONTAINS                                | Use Azure Search para buscar texto completo                        |
| UPPER/LOWER                             | En lugar de usar la función del sistema para normalizar los datos cada vez que realice comparaciones, puede normalizar el uso de mayúsculas y minúsculas durante la inserción. Después, una consulta como ```SELECT * FROM c WHERE UPPER(c.name) = 'BOB'``` simplemente se convierte en ```SELECT * FROM c WHERE c.name = 'BOB'``` |
| Funciones matemáticas (sin agregados) | Si necesita calcular frecuentemente un valor en la consulta, considere la posibilidad de almacenarlo como propiedad en el documento JSON. |

------

Otras partes de la consulta pueden seguir utilizando el índice aunque las funciones del sistema no lo usen.

## <a name="queries-with-both-a-filter-and-an-order-by-clause"></a>Consultas con filtro y una cláusula ORDER BY

Aunque normalmente las consultas con filtro y una cláusula `ORDER BY` utilizan un índice de intervalo, serán más eficientes si se pueden servir desde un índice compuesto. Además de modificar la directiva de indexación, debe agregar todas las propiedades del índice compuesto a la cláusula `ORDER BY`. Esta modificación de la consulta garantizará que utiliza el índice compuesto.  Puede observar el impacto mediante la ejecución de una consulta en el conjunto de datos [nutrition](https://github.com/CosmosDB/labs/blob/master/dotnet/setup/NutritionData.json).

### <a name="original"></a>Original

Consulta:

```sql
SELECT * FROM c WHERE c.foodGroup = "Soups, Sauces, and Gravies" ORDER BY c._ts ASC
```

Directiva de indexación:

```json
{

        "automatic":true,
        "indexingMode":"Consistent",
        "includedPaths":[  
            {  
                "path":"/*"
            }
        ],
        "excludedPaths":[]
}
```

**Cargo por unidad de solicitud:** 44,28 RU

### <a name="optimized"></a>Optimizado

Consulta actualizada (incluye ambas propiedades en la cláusula `ORDER BY`):

```sql
SELECT * FROM c
WHERE c.foodGroup = “Soups, Sauces, and Gravies”
ORDER BY c.foodGroup, c._ts ASC
```

Directiva de indexación actualizada:

```json
{  
        "automatic":true,
        "indexingMode":"Consistent",
        "includedPaths":[  
            {  
                "path":"/*"
            }
        ],
        "excludedPaths":[],
        "compositeIndexes":[  
            [  
                {  
                    "path":"/foodGroup",
                    "order":"ascending"
        },
                {  
                    "path":"/_ts",
                    "order":"ascending"
                }
            ]
        ]
    }

```

**Cargo por unidad de solicitud:** 8,86 RU

## <a name="optimize-join-expressions-by-using-a-subquery"></a>Optimización de las expresiones JOIN mediante una subconsulta
Las subconsultas multivalor pueden optimizar las expresiones `JOIN` mediante la inserción de predicados después de cada expresión select-many, en lugar de hacerlo después de todas las combinaciones cruzadas en la cláusula `WHERE`.

Considere la siguiente consulta:

```sql
SELECT Count(1) AS Count
FROM c
JOIN t IN c.tags
JOIN n IN c.nutrients
JOIN s IN c.servings
WHERE t.name = 'infant formula' AND (n.nutritionValue > 0
AND n.nutritionValue < 10) AND s.amount > 1
```

**Cargo por unidad de solicitud:** 167,62 RU

Para esta consulta, el índice coincidirá con cualquier documento que tenga una etiqueta con el nombre "infant formula", un valor de nutritionValue mayor que 0 y sirva una cantidad mayor que 1. La expresión `JOIN` aquí generará el producto cruzado de todos los elementos de las matrices tags, nutrients y servings para cada documento coincidente antes de aplicar cualquier filtro. Luego, la cláusula `WHERE` aplicará el predicado de filtro en cada tupla `<c, t, n, s>`.

Por ejemplo, si un documento coincidente tenía 10 elementos en cada una de las tres matrices, se expandirá a 1×10×10×10 (es decir, 1000) tuplas. Usar subconsultas aquí puede ayudar a filtrar los elementos de matrices combinadas antes de unirse a la siguiente expresión.

Esta consulta es equivalente a la anterior, pero usa subconsultas:

```sql
SELECT Count(1) AS Count
FROM c
JOIN (SELECT VALUE t FROM t IN c.tags WHERE t.name = 'infant formula')
JOIN (SELECT VALUE n FROM n IN c.nutrients WHERE n.nutritionValue > 0 AND n.nutritionValue < 10)
JOIN (SELECT VALUE s FROM s IN c.servings WHERE s.amount > 1)
```

**Cargo por unidad de solicitud:** 22,17 RU

Se supone que solo un elemento de la matriz tags coincide con el filtro, y hay cinco elementos tanto para la matriz de nutrients como la de servings. Las expresiones `JOIN`luego se expandirán a 1 × 1 × 5 × 5 = 25 elementos, en lugar de los 1000 elementos de la primera consulta.

## <a name="queries-where-retrieved-document-count-is-equal-to-output-document-count"></a>Consultas en las que el recuento de documentos recuperados es igual que el de documentos de salida

Si el recuento de documentos recuperados es aproximadamente igual al de documentos de salida, significa que la consulta no tuvo que examinar muchos documentos innecesarios. Para muchas consultas, como las que usan la palabra clave TOP, el recuento de documentos recuperados puede superar el de documentos de salida en una unidad. Esto no debe causar problemas.

## <a name="avoid-cross-partition-queries"></a>Cómo evitar las particiones cruzadas

Azure Cosmos DB usa [particiones](partitioning-overview.md) para escalar contenedores individuales a medida que aumentan las necesidades de almacenamiento de datos y las unidades de solicitud. Cada partición física tiene un índice independiente. Si la consulta tiene un filtro de igualdad que coincide con la clave de partición del contenedor, solo tendrá que comprobar el índice de la partición pertinente. Esta optimización reduce el número total de RU que requiere la consulta.

Si tiene un gran número de RU aprovisionadas (más de 30 000) o una gran cantidad de datos almacenados (más de aproximadamente 100 GB), es probable que tenga un contenedor lo suficientemente grande como para ver una reducción significativa en los cargos de las RU de consulta.

Por ejemplo, si creamos un contenedor con la clave de partición foodGroup, las siguientes consultas solo tendrían que comprobar una única partición física:

```sql
SELECT * FROM c
WHERE c.foodGroup = "Soups, Sauces, and Gravies" and c.description = "Mushroom, oyster, raw"
```

Estas consultas también se optimizarían al incluir la clave de partición en la consulta:

```sql
SELECT * FROM c
WHERE c.foodGroup IN("Soups, Sauces, and Gravies", "Vegetables and Vegetable Products") and c.description = "Mushroom, oyster, raw"
```

Las consultas que tienen filtros de intervalo en la clave de partición o que no tienen ningún filtro en la clave de partición, deberán desplegarse y comprobar los resultados de los índices de todas las particiones físicas.

```sql
SELECT * FROM c
WHERE c.description = "Mushroom, oyster, raw"
```

```sql
SELECT * FROM c
WHERE c.foodGroup > "Soups, Sauces, and Gravies" and c.description = "Mushroom, oyster, raw"
```

## <a name="filters-on-multiple-properties"></a>Filtros en varias propiedades

Aunque las consultas con filtros en varias propiedades suelen utilizar un índice de intervalo, serán más eficientes si se pueden servir desde un índice compuesto. Esta optimización no afectará considerablemente a cantidades pequeñas de datos. No obstante, puede resultar útil para grandes cantidades de datos. Como máximo puede optimizar un filtro de distinto del de igualdad por índice compuesto. Si la consulta tiene varios filtros distintos del de igualdad, debe elegir uno de ellos para que utilice el índice compuesto. El resto continuará usando índices de intervalo. El filtro distinto del de igualdad debe definirse en último lugar en el índice compuesto. [Más información sobre los índices compuestos](index-policy.md#composite-indexes)

Estos son algunos ejemplos de consultas que se podrían optimizar con un índice compuesto:

```sql
SELECT * FROM c
WHERE c.foodGroup = "Vegetables and Vegetable Products" AND c._ts = 1575503264
```

```sql
SELECT * FROM c
WHERE c.foodGroup = "Vegetables and Vegetable Products" AND c._ts > 1575503264
```

Este es el índice compuesto pertinente:

```json
{  
        "automatic":true,
        "indexingMode":"Consistent",
        "includedPaths":[  
            {  
                "path":"/*"
            }
        ],
        "excludedPaths":[],
        "compositeIndexes":[  
            [  
                {  
                    "path":"/foodGroup",
                    "order":"ascending"
                },
                {  
                    "path":"/_ts",
                    "order":"ascending"
                }
            ]
        ]
}
```

## <a name="optimizations-that-reduce-query-latency"></a>Optimizaciones que reducen la latencia de las consultas:

En muchos casos, el cargo por RU puede ser aceptable pero la latencia de consulta sigue siendo demasiado elevada. En las secciones siguientes se ofrece información general con sugerencias para reducir la latencia de las consultas. Si ejecuta la misma consulta varias veces en el mismo conjunto de datos, tendrá el mismo cargo por RU cada vez. Sin embargo, la latencia de las consultas puede variar entre una ejecución y otra.

## <a name="improve-proximity"></a>Mejora de la proximidad

Las consultas que se ejecutan desde una región distinta de aquella de la cuenta de Azure Cosmos DB tendrán una latencia mayor que si se ejecutaran desde la misma región. Por ejemplo, si ejecutara código en el equipo de escritorio, debería esperar que la latencia fuera decenas o cientos (o más) de milisegundos más que si la consulta proviniera de una máquina virtual dentro de la misma región de Azure que Azure Cosmos DB. Es fácil [distribuir datos globalmente en Azure Cosmos DB](distribute-data-globally.md) para asegurarse de que puede acercar los datos a la aplicación.

## <a name="increase-provisioned-throughput"></a>Aumento de la capacidad de proceso aprovisionada

En Azure Cosmos DB, la capacidad de proceso aprovisionada se mide en unidades de solicitud (RU). Supongamos que tiene una consulta que consume 5 RU de capacidad de proceso. Por ejemplo, si aprovisiona 1000 RU, podrá ejecutar esa consulta 200 veces por segundo. Si intentó ejecutar la consulta cuando no había suficiente capacidad de proceso disponible, Azure Cosmos DB devolvería un error HTTP 429. Cualquiera de los SDK de Core (SQL) API actuales volverá a intentar automáticamente esta consulta tras una breve espera. Las solicitudes limitadas tardan más, por lo que aumentar la capacidad de proceso aprovisionada puede mejorar la latencia de las consultas. Puede observar el [número total de solicitudes limitadas](use-metrics.md#understand-how-many-requests-are-succeeding-or-causing-errors) en la hoja Métrica de Azure Portal.

## <a name="increase-maxconcurrency"></a>Aumento de MaxConcurrency

Las consultas paralelas funcionan creando consultas en varias particiones en paralelo. Sin embargo, los datos de una recopilación con particiones individual se capturan en serie con respecto a la consulta. Establecer MaxConcurrency en el número de particiones tiene la máxima probabilidad de conseguir el mejor rendimiento de consulta, siempre y cuando el resto de condiciones del sistema permanezcan invariables. Si no conoce el número de particiones, puede establecer MaxConcurrency (o MaxDegreesOfParallelism en versiones anteriores del SDK) en un número alto y el sistema elegirá el mínimo (número de particiones, entrada proporcionada por el usuario) como el grado máximo de paralelismo.

## <a name="increase-maxbuffereditemcount"></a>Aumento de MaxBufferedItemCount

Las consultas están diseñadas para capturar previamente los resultados mientras el cliente procesa el lote actual de resultados. La captura previa ayuda a mejorar la latencia general de una consulta. La configuración de MaxBufferedItemCount limita el número de resultados capturados previamente. La configuración de este valor en el número esperado de resultados devueltos (o un número más alto) permite que la consulta reciba el máximo beneficio de la captura previa. Establecer este valor en -1 permite que el sistema decida automáticamente el número de elementos que se van a almacenar en el búfer.

## <a name="next-steps"></a>Pasos siguientes
Consulte los siguientes documentos sobre cómo medir las RU por consulta, cómo obtener estadísticas de ejecución para optimizar las consultas y mucho más:

* [Obtención de métricas de ejecución de consultas SQL mediante el SDK de .NET](profile-sql-api-query.md)
* [Optimización del rendimiento de consultas con Azure Cosmos DB](sql-api-sql-query-metrics.md)
* [Sugerencias de rendimiento para el SDK de .NET](performance-tips.md)
