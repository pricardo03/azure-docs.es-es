---
title: Evaluar las consultas con la función de perfil de ejecución para la API de Gremlin de Azure Cosmos DB
description: Obtenga información sobre cómo solucionar problemas y mejorar sus consultas de Gremlin con el paso de perfil de ejecución.
services: cosmos-db
author: luisbosquez
manager: kfile
ms.service: cosmos-db
ms.component: cosmosdb-graph
ms.topic: conceptual
ms.date: 03/27/2019
ms.author: lbosq
ms.openlocfilehash: 2f3967c64e79b2bc7b01b35eff26f5ac0d4e3db4
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/18/2019
ms.locfileid: "59288612"
---
# <a name="how-to-use-the-execution-profile-step-to-evaluate-your-gremlin-queries"></a>Cómo usar el paso de perfil de ejecución para evaluar las consultas de Gremlin

En este artículo se proporciona información general de cómo usar el paso de perfil de ejecución para las bases de datos del gráfico de Gremlin API de Azure Cosmos DB. Este paso proporciona información relevante para solucionar problemas y las optimizaciones de consultas y es compatible con cualquier consulta de Gremlin que se puede ejecutar en una cuenta de API de Gremlin de Cosmos DB.

Para usar este paso, solo tiene que anexar el `executionProfile()` llamada al final de la consulta de Gremlin de función. **Se ejecutará la consulta de Gremlin** y el resultado de la operación devolverá un objeto de respuesta JSON con el perfil de ejecución de consulta.

Por ejemplo: 

```java
    // Basic traversal
    g.V('mary').out()

    // Basic traversal with execution profile call
    g.V('mary').out().executionProfile()
```

Después de llamar a la `executionProfile()` paso, la respuesta será un objeto JSON que incluye el paso de Gremlin ejecutado, el tiempo total que tardó y una matriz de los operadores de tiempo de ejecución de Cosmos DB que dieron lugar a la instrucción.

> [!NOTE]
> Esta implementación para el perfil de ejecución no está definida en la especificación de Apache Tinkerpop. Es específico de la implementación de la API de Gremlin de Azure Cosmos DB.


## <a name="response-example"></a>Ejemplo de respuesta

Este es un ejemplo con anotaciones de la salida que se devolverá:

> [!NOTE]
> En este ejemplo se marcará con comentarios que explican la estructura general de la respuesta. Una respuesta executionProfile real no contener ningún comentario.

```json
[
  {
    // The Gremlin statement that was executed.
    "gremlin": "g.V('mary').out().executionProfile()",

    // Amount of time in milliseconds that the entire operation took.
    "totalTime": 28,

    // An array containing metrics for each of the steps that were executed. Each Gremlin step will translate to one or more of these steps.
    // This list is sorted in order of execution.
    "metrics": [
      {
        // This operation obtains a set of Vertex objects.
        // The metrics include: time, percentTime of total execution time, resultCount, fanoutFactor, count, size (in bytes) and time.
        "name": "GetVertices",
        "time": 24,
        "annotations": {
          "percentTime": 85.71
        },
        "counts": {
          "resultCount": 2
        },
        "storeOps": [
          {
            "fanoutFactor": 1,
            "count": 2,
            "size": 696,
            "time": 0.4
          }
        ]
      },
      {
        // This operation obtains a set of Edge objects. Depending on the query, these might be directly adjacent to a set of vertices, or separate, in the case of an E() query.
        // The metrics include: time, percentTime of total execution time, resultCount, fanoutFactor, count, size (in bytes) and time.
        "name": "GetEdges",
        "time": 4,
        "annotations": {
          "percentTime": 14.29
        },
        "counts": {
          "resultCount": 1
        },
        "storeOps": [
          {
            "fanoutFactor": 1,
            "count": 1,
            "size": 419,
            "time": 0.67
          }
        ]
      },
      {
        // This operation obtains the vertices that a set of edges point at.
        // The metrics include: time, percentTime of total execution time and resultCount.
        "name": "GetNeighborVertices",
        "time": 0,
        "annotations": {
          "percentTime": 0
        },
        "counts": {
          "resultCount": 1
        }
      },
      {
        // This operation represents the serialization and preparation for a result from the preceding graph operations.
        // The metrics include: time, percentTime of total execution time and resultCount.
        "name": "ProjectOperator",
        "time": 0,
        "annotations": {
          "percentTime": 0
        },
        "counts": {
          "resultCount": 1
        }
      }
    ]
  }
]
```

> [!NOTE]
> El paso executionProfile ejecutará la consulta de Gremlin. Esto incluye la `addV` o `addE`pasos, que dará como resultado la creación y confirmación los cambios especificados en la consulta. Como resultado, también se cobrará a las unidades de solicitud generado por la consulta de Gremlin.

## <a name="execution-profile-response-objects"></a>Objetos de respuesta de perfil de ejecución

La respuesta de una función executionProfile() brindará una jerarquía de objetos JSON con la estructura siguiente:
  - **Objeto de la operación de gremlin**: Representa la operación completa de Gremlin que se ejecutó. Contiene las siguientes propiedades.
    - `gremlin`: La instrucción de Gremlin explícita que se ha ejecutado.
    - `totalTime`: El tiempo, en milisegundos, que ha utilizado la ejecución del paso. 
    - `metrics`: Una matriz que contiene cada uno de los operadores de tiempo de ejecución de Cosmos DB que se ejecutaron para satisfacer la consulta. Esta lista se ordena en orden de ejecución.
    
  - **Operadores de tiempo de ejecución de COSMOS DB**: Representa cada uno de los componentes de toda la operación de Gremlin. Esta lista se ordena en orden de ejecución. Cada objeto contiene las siguientes propiedades:
    - `name`: Nombre del operador. Este es el tipo de paso que se evalúa y se ejecuta. Lea más en la tabla siguiente.
    - `time`: Cantidad de tiempo, en milisegundos, que tardó un operador determinado.
    - `annotations`: Contiene información adicional específica para el operador que se ha ejecutado.
    - `annotations.percentTime`: Porcentaje del tiempo total que tardó en ejecutar el operador específico.
    - `counts`: Número de objetos que se devolvieron desde la capa de almacenamiento de este operador. Esto se encuentra en la `counts.resultCount` valor escalar dentro.
    - `storeOps`: Representa una operación de almacenamiento que puede abarcar una o varias particiones.
    - `storeOps.fanoutFactor`: Representa el número de particiones que tiene acceso esta operación de almacenamiento específica.
    - `storeOps.count`: Representa el número de resultados que devuelve esta operación de almacenamiento.
    - `storeOps.size`: Representa el tamaño en bytes del resultado de una operación de almacenamiento determinado.

Operador de tiempo de ejecución de Gremlin de COSMOS DB|DESCRIPCIÓN
---|---
`GetVertices`| Este paso Obtiene un conjunto de objetos de predicado de la capa de persistencia. 
`GetEdges`| Este paso obtiene los bordes que están en posición adyacentes a un conjunto de vértices. Este paso puede dar lugar a una o varias operaciones de almacenamiento.
`GetNeighborVertices`| Este paso obtiene los vértices que están conectados a un conjunto de bordes. Los bordes contienen la partición de claves y el Id. de vértices de su origen y de destino.
`Coalesce`| Cuentas de este paso para la evaluación de dos operaciones cada vez que el `coalesce()` se ejecuta el paso de Gremlin.
`CartesianProductOperator`| Este paso calcula un producto cartesiano entre dos conjuntos de datos. Normalmente se ejecuta cada vez que los predicados `to()` o `from()` se usan.
`ConstantSourceOperator`| Este paso calcula una expresión para producir como resultado un valor constante.
`ProjectOperator`| Este paso se prepara y serializa una respuesta con el resultado de operaciones anteriores.
`ProjectAggregation`| Este paso se prepara y serializa una respuesta para una operación de agregado.

> [!NOTE]
> Esta lista seguirá actualizarse cuando se agregan nuevos operadores.

## <a name="examples-on-how-to-analyze-an-execution-profile-response"></a>Ejemplos de cómo analizar una respuesta de perfil de ejecución

Los siguientes son ejemplos de optimizaciones comunes que se pueden observar con la respuesta de perfil de ejecución:
  - Consulta de distribución ramificada ciego.
  - Consulta sin filtrar.

### <a name="blind-fan-out-query-patterns"></a>Patrones de consulta de distribución ramificada ciego

Suponga la siguiente respuesta de perfil de ejecución de un **grafo con particiones**:

```json
[
  {
    "gremlin": "g.V('tt0093640').executionProfile()",
    "totalTime": 46,
    "metrics": [
      {
        "name": "GetVertices",
        "time": 46,
        "annotations": {
          "percentTime": 100
        },
        "counts": {
          "resultCount": 1
        },
        "storeOps": [
          {
            "fanoutFactor": 5,
            "count": 1,
            "size": 589,
            "time": 75.61
          }
        ]
      },
      {
        "name": "ProjectOperator",
        "time": 0,
        "annotations": {
          "percentTime": 0
        },
        "counts": {
          "resultCount": 1
        }
      }
    ]
  }
]
```

Se pueden realizar las siguientes conclusiones de él:
- La consulta es una búsqueda de identificador única, ya que la instrucción de Gremlin sigue el patrón `g.V('id')`.
- A partir de la `time` métrica, la latencia de esta consulta parece ser alta, ya que es [más de 10 ms para una sola operación de lectura de punto](https://docs.microsoft.com/azure/cosmos-db/introduction#guaranteed-low-latency-at-99th-percentile-worldwide).
- Si observamos el `storeOps` objeto, podemos ver que el `fanoutFactor` es `5`, lo que significa que [5 particiones](https://docs.microsoft.com/azure/cosmos-db/partition-data) ha accedido a esta operación.

Como una conclusión de este análisis, podemos determinar que la primera consulta tiene acceso a las particiones de más de lo necesario. Esto puede solucionarse mediante la especificación de la clave de partición en la consulta como un predicado. Esto dará lugar a la menor latencia y menos costos por consulta. Obtenga más información sobre [graph particiones](graph-partitioning.md). Una consulta más óptima sería `g.V('tt0093640').has('partitionKey', 't1001')`.

### <a name="unfiltered-query-patterns"></a>Patrones de consulta sin filtrar

Compare las respuestas siguientes de perfil de ejecución de dos. Por motivos de simplicidad, estos ejemplos utilizan un único gráfico con particiones.

Esta primera consulta recupera todos los vértices con la etiqueta `tweet` y, a continuación, obtiene sus vértices vecinos:

```json
[
  {
    "gremlin": "g.V().hasLabel('tweet').out().executionProfile()",
    "totalTime": 42,
    "metrics": [
      {
        "name": "GetVertices",
        "time": 31,
        "annotations": {
          "percentTime": 73.81
        },
        "counts": {
          "resultCount": 30
        },
        "storeOps": [
          {
            "fanoutFactor": 1,
            "count": 13,
            "size": 6819,
            "time": 1.02
          }
        ]
      },
      {
        "name": "GetEdges",
        "time": 6,
        "annotations": {
          "percentTime": 14.29
        },
        "counts": {
          "resultCount": 18
        },
        "storeOps": [
          {
            "fanoutFactor": 1,
            "count": 20,
            "size": 7950,
            "time": 1.98
          }
        ]
      },
      {
        "name": "GetNeighborVertices",
        "time": 5,
        "annotations": {
          "percentTime": 11.9
        },
        "counts": {
          "resultCount": 20
        },
        "storeOps": [
          {
            "fanoutFactor": 1,
            "count": 4,
            "size": 1070,
            "time": 1.19
          }
        ]
      },
      {
        "name": "ProjectOperator",
        "time": 0,
        "annotations": {
          "percentTime": 0
        },
        "counts": {
          "resultCount": 20
        }
      }
    ]
  }
]
```

Tenga en cuenta el perfil de la misma consulta, pero ahora con un filtro adicional, `has('lang', 'en')`, antes de explorar los vértices adyacentes:

```json
[
  {
    "gremlin": "g.V().hasLabel('tweet').has('lang', 'en').out().executionProfile()",
    "totalTime": 14,
    "metrics": [
      {
        "name": "GetVertices",
        "time": 14,
        "annotations": {
          "percentTime": 58.33
        },
        "counts": {
          "resultCount": 11
        },
        "storeOps": [
          {
            "fanoutFactor": 1,
            "count": 11,
            "size": 4807,
            "time": 1.27
          }
        ]
      },
      {
        "name": "GetEdges",
        "time": 5,
        "annotations": {
          "percentTime": 20.83
        },
        "counts": {
          "resultCount": 18
        },
        "storeOps": [
          {
            "fanoutFactor": 1,
            "count": 18,
            "size": 7159,
            "time": 1.7
          }
        ]
      },
      {
        "name": "GetNeighborVertices",
        "time": 5,
        "annotations": {
          "percentTime": 20.83
        },
        "counts": {
          "resultCount": 18
        },
        "storeOps": [
          {
            "fanoutFactor": 1,
            "count": 4,
            "size": 1070,
            "time": 1.01
          }
        ]
      },
      {
        "name": "ProjectOperator",
        "time": 0,
        "annotations": {
          "percentTime": 0
        },
        "counts": {
          "resultCount": 18
        }
      }
    ]
  }
]
```

Estas dos consultas alcanza el mismo resultado, sin embargo, la primera de ellas requieren más unidades de solicitud dado que necesita para recorrer en iteración un conjunto de datos inicial mayor antes de consultar los elementos adyacentes. Podemos ver los indicadores de este comportamiento al comparar los siguientes parámetros de ambas respuestas:
- El `metrics[0].time` valor es mayor en la primera respuesta, que indica que este paso solo tardó más tiempo en resolverse.
- El `metrics[0].counts.resultsCount` es más alto también en la primera respuesta, lo que indica que el conjunto de datos de trabajo inicial era mayor valor.

## <a name="next-steps"></a>Pasos siguientes
* Obtenga información sobre la [admite las características de Gremlin](gremlin-support.md) en Azure Cosmos DB. 
* Obtenga más información sobre la [Gremlin API en Azure Cosmos DB](graph-introduction.md).
