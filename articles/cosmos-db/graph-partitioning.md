---
title: Creación de particiones en la API de Gremlin de Azure Cosmos DB
description: Aprenda a usar Graph con particiones en Azure Cosmos DB. En este artículo también se describen los requisitos y procedimientos recomendados para un grafo con particiones.
author: luisbosquez
ms.author: lbosq
ms.service: cosmos-db
ms.subservice: cosmosdb-graph
ms.topic: conceptual
ms.date: 12/06/2018
ms.custom: seodec18
ms.openlocfilehash: f1e486a302b440d819e15ef86f8d76ea5e50d201
ms.sourcegitcommit: 8330a262abaddaafd4acb04016b68486fba5835b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/04/2019
ms.locfileid: "54036331"
---
# <a name="using-a-partitioned-graph-in-azure-cosmos-db"></a>Uso de Graph con particiones en Azure Cosmos DB

Una de las características clave de la API de Gremlin de Azure Cosmos DB es la posibilidad de tratar grafos a gran escala con escalado horizontal. El escalado horizontal se logra a través de las [funcionalidades de partición de Azure Cosmos DB](partition-data.md). Los contenedores pueden escalarse independientemente en términos de almacenamiento y rendimiento. Puede crear contenedores en Azure Cosmos DB que pueden escalarse automáticamente para almacenar los datos de un grafo. Los datos se equilibrarán automáticamente según la **clave de partición** especificada.

En este documento se describirán los detalles sobre cómo se crean particiones en las bases de datos de grafos junto con sus implicaciones para ambos vértices (o nodos) y bordes.

## <a name="requirements-for-partitioned-graph"></a>Requisitos para el grafo con particiones

Estos son los detalles que es necesario comprender para crear un contenedor de grafos con particiones:

- **Se requiere la creación de particiones** si se espera que el contenedor almacene más de 10 GB de tamaño o se quieren asignar más de 10 000 unidades de solicitud por segundo (RU).

- **Los vértices y los bordes se almacenan como documentos JSON**.

- Los **vértices requieren una clave de partición**. Esta clave determinará en qué partición se almacenará el vértice a través de un algoritmo hash. El nombre de esta clave de partición es una cadena de una sola palabra sin espacios ni caracteres especiales. La clave de partición se define al crear otro contenedor y tiene el siguiente formato: `/partitioning-key-name`.

- Los **bordes se almacenarán con sus vértices de origen**. En otras palabras, para cada vértice, su clave de partición define dónde se almacena junto con sus bordes salientes. Esto se hace para evitar consultas entre particiones cuando se usa la cardinalidad `out()` en las consultas de grafo.

- Las **consultas de grafo tienen que especificar una clave de partición**. Para aprovechar al máximo la partición horizontal en Azure Cosmos DB, la clave de partición debe especificarse al seleccionar un solo vértice, siempre que sea posible. Estas son las consultas para seleccionar uno o varios vértices en un grafo con particiones:

    - `/id` y `/label` no se admiten como claves de partición para un contenedor en la API de Gremlin.


    - Selección de un vértice por el identificador, **use el paso `.has()` para especificar la propiedad de clave de partición**: 
    
        ```
        g.V('vertex_id').has('partitionKey', 'partitionKey_value')
        ```
    
    - Selección de un vértice, **especifique una tupla con valor de clave de partición e identificador**: 
    
        ```
        g.V(['partitionKey_value', 'vertex_id'])
        ```
        
    - Especificación de una **matriz de tuplas de identificadores y valores de clave de partición**:
    
        ```
        g.V(['partitionKey_value0', 'verted_id0'], ['partitionKey_value1', 'vertex_id1'], ...)
        ```
        
    - Selección de un conjunto de vértices y **especificación de una lista de valores de clave de partición**: 
    
        ```
        g.V('vertex_id0', 'vertex_id1', 'vertex_id2', …).has('partitionKey', within('partitionKey_value0', 'partitionKey_value01', 'partitionKey_value02', …)
        ```

## <a name="best-practices-when-using-a-partitioned-graph"></a>Procedimientos recomendados con un grafo con particiones

Para garantizar la escalabilidad y el rendimiento al usar grafos con particiones con contenedores ilimitados, siga estas directrices:

- **Especifique siempre el valor de la clave de partición al consultar un vértice**. Obtener un vértice de una partición conocida es una forma de conseguir rendimiento.

- **Use la dirección saliente al consultar los bordes siempre que sea posible**. Tal y como se mencionó anteriormente, los bordes se almacenan con sus vértices de origen en la dirección saliente. Por lo que se minimizan las posibilidades de recurrir a las consultas entre particiones cuando los datos y las consultas están diseñados con este patrón en mente.

- **Elija una clave de partición que distribuya los datos uniformemente por las particiones**. Esta decisión depende mucho el modelo de datos de la solución. Más información sobre la creación de una clave de partición apropiada en [Partición y escalado en Azure Cosmos DB](partition-data.md).

- **Optimice las consultas para obtener los datos dentro de los límites de una partición**. Una estrategia de partición óptima se alinea con los patrones de consulta. Las consultas que obtienen datos de una sola partición proporcionan el mejor rendimiento.

## <a name="next-steps"></a>Pasos siguientes

Ahora puede pasar a la lectura los artículos siguientes:

* Información acerca de la [Partición y escalado en Azure Cosmos DB](partition-data.md).
* Información acerca de la [compatibilidad de Gremlin en la API de Gremlin](gremlin-support.md).
* Información acerca de la [introducción a la API de Gremlin](graph-introduction.md).
