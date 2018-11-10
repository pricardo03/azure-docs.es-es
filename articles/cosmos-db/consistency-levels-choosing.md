---
title: Elección del nivel de coherencia adecuado para la aplicación | Microsoft Docs
description: Elija el nivel de coherencia adecuado para la aplicación en Azure Cosmos DB.
keywords: coherencia, rendimiento, azure cosmos db, azure, Microsoft azure
services: cosmos-db
author: markjbrown
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 10/23/2018
ms.author: mjbrown
ms.openlocfilehash: 4b438320ac75918d10880a4bcdccadca4eebec32
ms.sourcegitcommit: dbfd977100b22699823ad8bf03e0b75e9796615f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2018
ms.locfileid: "50243990"
---
# <a name="choose-the-right-consistency-level-for-your-application"></a>Elección del nivel de coherencia adecuado para la aplicación

Las bases de datos distribuidas que dependen de la replicación para su alta disponibilidad, su baja latencia o ambas, constituyen el compromiso fundamental entre la coherencia de lectura y la disponibilidad, la latencia y el rendimiento. La mayoría de las bases de datos distribuidas disponibles comercialmente solicitan a los desarrolladores que elijan entre los dos modelos de coherencia extrema: coherencia fuerte y posible coherencia. Azure Cosmos DB permite a los desarrolladores elegir entre cinco modelos de coherencia bien definidos: alta, de obsolescencia limitada, de sesión, de prefijo coherente y final. Cada uno de estos modelos de coherencia está bien definido, es intuitivo y puede utilizarse para escenarios específicos del mundo real. Cada uno de los cinco modelos de coherencia ofrece compromisos claros entre rendimiento y disponibilidad, y están respaldados por contratos de nivel de servicio. Las siguientes consideraciones sencillas le ayudarán a tomar la decisión correcta en muchos escenarios comunes.

## <a name="sql-api-or-table-api"></a>API de SQL o Table API

- En muchos escenarios del mundo real, la coherencia de la sesión es óptima y es la opción recomendada. Para obtener más información, vea [How-to manage session token for your application](how-to-manage-consistency.md#utilize-session-tokens) (Procedimientos para administrar el token de la sesión para la aplicación).
- Si la aplicación requiere una sólida coherencia, se recomienda usar el nivel de coherencia de obsolescencia limitada.
- Si necesita garantías de coherencia más estrictas que las proporcionadas por la coherencia de la sesión y la latencia de un solo dígito y milisegundo para las escrituras, se recomienda que utilice el nivel de coherencia de obsolescencia limitada.  
- Si la aplicación requiere una posible coherencia, se recomienda usar el nivel de coherencia de prefijo coherente.
- Si necesita garantías de coherencia menos estrictas que las que proporciona la coherencia de la sesión, se recomienda utilizar el nivel de coherencia de prefijo coherente.
- Si necesita la máxima disponibilidad y latencia más baja, use el nivel de posible coherencia.

## <a name="cassandra-mongodb-or-gremlin-api"></a>Cassandra, MongoDB o Gremlin API

- Para obtener más información sobre la asignación entre "Read Consistency Level" de Apache Cassandra y los niveles de coherencia de Cosmos DB, consulte [Consistency levels and Cosmos DB APIs](consistency-levels-across-apis.md#cassandra-mapping) (Niveles de coherencia y API de Cosmos DB).
- Para obtener más información sobre la asignación entre "Read Concern" de MongoDB y los niveles de coherencia de Azure Cosmos DB, consulte [Consistency levels and Cosmos DB APIs](consistency-levels-across-apis.md#mongo-mapping) (Niveles de coherencia y API de Cosmos DB).

## <a name="you-may-get-stronger-consistency-guarantees-in-practice"></a>Es posible que obtenga mayores garantías de coherencia en la práctica

Las garantías de coherencia para una operación de lectura se corresponden con la actualización y el orden del estado de la base de datos que se solicita. La coherencia de lectura está asociada con la ordenación y la propagación de las operaciones de escritura (actualización).  

Cuando se establece el nivel de coherencia en **obsolescencia limitada**, Cosmos DB garantiza que los clientes siempre leerán el valor de una escritura anterior, con un intervalo limitado por la ventana de obsolescencia.

Cuando se establece el nivel de coherencia en **alto**, la ventana de obsolescencia equivale a cero y se garantiza que los clientes leerán el último valor confirmado de la escritura.

Para los tres niveles de coherencia restantes, la ventana de obsolescencia depende en gran medida la carga de trabajo. Por ejemplo, si no se produce ninguna escritura en la base de datos, una operación de lectura con los niveles de coherencia **posible**, **sesión** o **prefijo coherente** es probable que produzca los mismos resultados que una operación de lectura con el nivel de coherencia alto.

Si su cuenta de Cosmos DB está configurada con un nivel de coherencia que no es el de coherencia alta, puede averiguar la probabilidad de que los clientes obtengan lecturas con coherencia alta (linearizable) para sus cargas de trabajo consultando la métrica de obsolescencia limitada de probabilidad (PBS) en el Azure Portal [Consulte aquí cómo usar la métrica PBS](how-to-manage-consistency.md#monitor-probabilistically-bounded-staleness-pbs-metric). La obsolescencia limitada de probabilidad muestra cómo de posible es la coherencia final. Esta métrica proporciona una visión general de la frecuencia con la que obtendrá una coherencia mayor que el nivel de coherencia que ha configurado en su cuenta de Cosmos DB. En otras palabras, puede ver la probabilidad (en milisegundos) de obtener lecturas con coherencia alta para una combinación de regiones de escritura y lectura.

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información acerca de los niveles de coherencia en los siguientes artículos:

* [Consistency level mapping across Cosmos DB APIs](consistency-levels-across-apis.md) (Asignación de niveles de coherencia en las API de Cosmos DB)
* [Availability and performance tradeoffs for various consistency levels](consistency-levels-tradeoffs.md) (Compromisos entre rendimiento y disponibilidad en los distintos niveles de coherencia)
* [How to manage the session token for your application](how-to-manage-consistency.md#utilize-session-tokens) (Cómo administrar el token de sesión de su aplicación)
* [How to monitor the Probabilistically Bounded Staleness (PBS) metric](how-to-manage-consistency.md#monitor-probabilistically-bounded-staleness-pbs-metric) (Cómo supervisar la métrica de obsolescencia limitada de manera probabilística (PBS))