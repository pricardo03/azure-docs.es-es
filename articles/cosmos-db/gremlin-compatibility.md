---
title: Compatibilidad de Gremlin de Azure Cosmos DB con las características de TinkerPop
description: Documentación de referencia para problemas de compatibilidad del motor Graph
author: olignat
ms.service: cosmos-db
ms.subservice: cosmosdb-graph
ms.topic: reference
ms.date: 09/10/2019
ms.author: olignat
ms.openlocfilehash: d58d1ea50251c3568161ce008db2672ff0fd6372
ms.sourcegitcommit: d70c74e11fa95f70077620b4613bb35d9bf78484
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/11/2019
ms.locfileid: "70910700"
---
# <a name="azure-cosmos-db-gremlin-compatibility"></a>Compatibilidad de Gremlin de Azure Cosmos DB
El motor Graph de Azure Cosmos DB sigue estrechamente la especificación de pasos transversales de [Apache TinkerPop](https://tinkerpop.apache.org/docs/current/reference/#graph-traversal-steps), pero con algunas diferencias.

## <a name="behavior-differences"></a>Diferencias de comportamiento

* El motor Graph de Azure Cosmos DB ejecuta el recorrido centrando la ***prioridad en la amplitud***, mientras que Gremlin de TinkerPop lo hace centrando la prioridad en la profundidad. Este comportamiento logra un mejor rendimiento en un sistema escalable horizontalmente como Cosmos DB. 

## <a name="unsupported-features"></a>Características no admitidas

* ***[Gremlin Bytecode](http://tinkerpop.apache.org/docs/current/tutorials/gremlin-language-variants/)*** es una especificación independiente del lenguaje de programación para recorridos de grafos. Cosmos DB Graph todavía no lo admite. Use ```GremlinClient.SubmitAsync()``` y pase el recorrido como una cadena de texto.

* Actualmente, no se admite el establecimiento de la cardinalidad ***```property(set, 'xyz', 1)```***. En su lugar, use ```property(list, 'xyz', 1)```.

* ***```match()```*** permite consultar grafos mediante la coincidencia de patrones declarativos. Esta funcionalidad no está disponible.

* No se admiten los ***objetos como propiedades*** en bordes ni vértices. Las propiedades solo pueden ser tipos primitivos o matrices.

* No se admite la ***ordenación por propiedades de la matriz*** ```.order().by(<array property>)```. Solo se admite la ordenación por tipos primitivos.

* No se admiten los ***tipos JSON no primitivos***. Use los tipos ```string```, ```number``` o ```true```/```false```. No se admiten los valores ```null```. 

* El serializador ***GraphSONv3*** no está disponible actualmente.

* No se admiten las ***transacciones*** debido a la naturaleza distribuida del sistema.  Configure el modelo de coherencia adecuado en la cuenta de Gremlin para "leer sus propias escrituras", y use la simultaneidad optimista para resolver las escrituras conflictivas.

## <a name="next-steps"></a>Pasos siguientes
* Visite la página [Voz del usuario de Cosmos DB](https://feedback.azure.com/forums/263030-azure-cosmos-db) para compartir comentarios y ayudar al equipo a centrarse en las características que son importantes para usted.
