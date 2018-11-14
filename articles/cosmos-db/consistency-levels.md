---
title: Niveles de coherencia en Azure Cosmos DB | Documentos de Microsoft
description: Azure Cosmos DB tiene cinco niveles de coherencia para ayudar a equilibrar la coherencia, la disponibilidad y la latencia finales.
keywords: coherencia final, azure cosmos db, azure, Microsoft azure
services: cosmos-db
author: aliuy
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 03/27/2018
ms.author: andrl
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 2611c25764503551c4da918d06bcaabe315cbf7c
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2018
ms.locfileid: "50963088"
---
# <a name="consistency-levels-in-azure-cosmos-db"></a>Niveles de coherencia en Azure Cosmos DB

Las bases de datos distribuidas que dependen de la replicación para su alta disponibilidad, su baja latencia o ambas, constituyen el compromiso fundamental entre la coherencia de lectura y la disponibilidad, la latencia y el rendimiento. La mayoría de las bases de datos distribuidas disponibles comercialmente solicitan a los desarrolladores que elijan entre los dos modelos de coherencia extrema: coherencia fuerte y posible coherencia. Aunque la  [linealizabilidad](http://cs.brown.edu/~mph/HerlihyW90/p463-herlihy.pdf) o modelo de coherencia fuerte es el estándar de oro de la programación de datos, obliga a pagar el precio de una latencia más alta (en estado estable) y de una disponibilidad reducida (en caso de errores). Por otro lado, la posible coherencia ofrece una mayor disponibilidad y un mejor rendimiento, pero es difícil programar aplicaciones.

Cosmos DB se aproxima a la coherencia de datos como un espectro de opciones en lugar de como dos extremos. Aunque la coherencia fuerte y la posible coherencia son los dos extremos del espectro, hay muchas opciones de coherencia a lo largo del mismo. Estas opciones de coherencia permiten a los desarrolladores elegir opciones más precisas y contrapartidas de mayor granularidad con respecto a la alta disponibilidad o al rendimiento. Cosmos DB permite a los desarrolladores elegir entre cinco modelos de coherencia bien definidos dentro del espectro de coherencia (del más fuerte al más débil): **fuerte**, **de obsolescencia limitada**, **de sesión**, **de prefijo coherente** y **posible**. Cada uno de estos modelos de coherencia está bien definido, es intuitivo y puede utilizarse para escenarios específicos del mundo real. Cada uno de los cinco modelos de coherencia ofrece [contrapartidas entre rendimiento y disponibilidad](consistency-levels-tradeoffs.md), y están respaldados por Acuerdos de Nivel de Servicio.

![Coherencia como un espectro](./media/consistency-levels/five-consistency-levels.png)

Los niveles de coherencia son independientes de la región. El nivel de coherencia de la cuenta de Cosmos DB está garantizado para todas las operaciones de lectura con independencia de las siguientes propiedades:

- La región desde la que se proporcionan las operaciones de lectura y escritura
- El número de regiones asociadas con su cuenta de Cosmos
- Si su cuenta está configurada con una o varias regiones de escritura

## <a name="scope-of-the-read-consistency"></a>El ámbito de la coherencia de lectura

La coherencia de lectura se aplica a una sola operación de lectura limitada a un intervalo de claves de partición (que es una partición lógica). Un cliente remoto o un procedimiento almacenado pueden emitir la operación de lectura.

## <a name="configuring-the-default-consistency-level"></a>Configuración del nivel de coherencia predeterminado

Puede configurar el **nivel de coherencia predeterminado** de su cuenta de Cosmos DB en cualquier momento. El nivel de coherencia predeterminado configurado en su cuenta se aplica a todas las bases de datos (y contenedores) de Cosmos de esa cuenta. Todas las operaciones de lectura y consulta que se emitan con arreglo a un contenedor o una base de datos usarán el nivel de coherencia especificado de forma predeterminada. Para más información, consulte cómo [configurar el nivel de coherencia predeterminado](how-to-manage-consistency.md#configure-the-default-consistency-level).

## <a name="guarantees-associated-with-consistency-levels"></a>Garantías asociadas a los niveles de coherencia

Los Acuerdos de Nivel de Servicio que proporciona Azure Cosmos DB garantizan que el 100 % de las solicitudes de lectura cumplirán con la garantía de coherencia en cualquier nivel de coherencia que elija. Se considera que una solicitud de lectura ha cumplido el Acuerdo de Nivel de Servicio de coherencia si se satisfacen todas las garantías de coherencia asociadas al nivel de coherencia. Las definiciones precisas de los cinco niveles de coherencia de Cosmos DB que usan el [lenguaje de especificación TLA+](http://lamport.azurewebsites.net/tla/tla.html) se proporcionan en el repositorio [azure-cosmos-tla](https://github.com/Azure/azure-cosmos-tla) de GitHub. La semántica de los cinco niveles de coherencia se describe a continuación:

- **Nivel de coherencia = "fuerte"**: la coherencia fuerte ofrece una garantía de [linealizabilidad](https://aphyr.com/posts/313-strong-consistency-models) que asegura que las lecturas devuelvan la versión confirmada más reciente de un elemento. Un cliente nunca verá una solicitud de escritura no confirmada o parcial. Se garantiza que los usuarios siempre leerán la escritura confirmada más reciente.

- **Nivel de coherencia = "obsolescencia limitada"**: se garantiza que las lecturas respetan la garantía de prefijo coherente. Las lecturas puedan ir con retraso respecto a las escrituras en un máximo de versiones K (es decir, "actualizaciones") de un elemento o el intervalo de tiempo "t". Al elegir la obsolescencia limitada, la obsolescencia se puede configurar de dos maneras: 

  * Número de versiones (K) del elemento o
  * El intervalo de tiempo (t) que las lecturas pueden retrasarse con respecto a las escrituras. 

  La obsolescencia limitada ofrece un orden global total, excepto dentro de la "ventana de obsolescencia". La garantía de lectura monotónica existe dentro de una región, tanto dentro como fuera de la "ventana de obsolescencia". La coherencia fuerte tiene la misma semántica que la que ofrece la obsolescencia limitada pero con una "ventana de obsolescencia" igual a cero. La obsolescencia limitada también se conoce como **linealizabilidad retardada**. Cuando un cliente realiza operaciones de lectura en una región que acepta las escrituras, las garantías que proporciona la obsolescencia limitada son idénticas a las que proporciona la coherencia fuerte.

- **Nivel de coherencia = "de sesión"**: se garantiza que las lecturas respetan las garantías de prefijo coherente, lecturas monotónicas, escrituras monotónicas, lectura de la escritura y escritura tras las lecturas. La coherencia de sesión se limita a una sesión de cliente.

- **Nivel de coherencia = "de prefijo coherente"**: las actualizaciones devueltas contienen algunos prefijos de todas las actualizaciones, sin espacios. El prefijo coherente garantiza que las lecturas nunca vean escrituras desordenadas.

- **Nivel de coherencia = "posible"**: no hay ninguna garantía de ordenación para las lecturas. En ausencia de escrituras adicionales, las réplicas terminarán por converger.

## <a name="consistency-levels-explained-through-baseball"></a>Niveles de coherencia explicados a través del béisbol

Vamos a usar un partido de béisbol como ejemplo. Imagine una secuencia de escrituras que representan el marcador de un partido de béisbol con el típico marcador de dos líneas con el resultado de cada inning (entrada), como se indica en el documento [Replicated Data Consistency Through Baseball](https://www.microsoft.com/en-us/research/wp-content/uploads/2011/10/ConsistencyAndBaseballReport.pdf) (Descripción de la coherencia de datos mediante el béisbol). Este partido de béisbol hipotético se encuentra actualmente a la mitad del séptimo inning (el increible inning de la suerte o séptimo de la suerte) y el equipo de casa va ganando 2-5.

| | **1** | **2** | **3** | **4** | **5** | **6** | **7** | **8** | **9** | **Carreras** |
| - | - | - | - | - | - | - | - | - | - | - |
| **Visitante** | 0 | 0 | 1 | 0 | 1 | 0 | 0 |  |  | 2 |
| **Local** | 1 | 0 | 1 | 1 | 0 | 2 |  |  |  | 5 |

Un contenedor de Cosmos DB guarda el número total de carreras del equipo visitante y del equipo local. Mientras el partido está en curso, las diferentes garantías de lectura pueden tener como resultado que los clientes lean diferentes marcadores. En la siguiente tabla se enumera el conjunto completo de marcadores que se puede devolver con la lectura de los marcadores del equipo visitante y el equipo local con cada uno de los cinco niveles de garantía de coherencia. El marcador del equipo visitante aparece en primer lugar y los distintos valores devueltos posibles están separados por comas.

| **Nivel de coherencia** | **Marcadores** |
| - | - |
| **Fuerte** | 2-5 |
| **Obsolescencia limitada** | los marcadores van como máximo con un inning de retraso: 2-3, 2-4, 2-5 |
| **De sesión** | <ul><li>para el agente de escritura: 2-5</li><li> para cualquier otro que no sea el agente de escritura: 0-0, 0-1, 0-2, 0-3, 0-4, 0-5, 1-0, 1-1, 1-2, 1-3, 1-4, 1-5, 2-0, 2-1, 2-2, 2-3, 2-4, 2-5</li><li>después de la lectura de 1-3: 1-3, 1-4, 1-5, 2-3, 2-4, 2-5</li> |
| **De prefijo coherente** | 0-0, 0-1, 1-1, 1-2, 1-3, 2-3, 2-4, 2-5 |
| **Posible** | 0-0, 0-1, 0-2, 0-3, 0-4, 0-5, 1-0, 1-1, 1-2, 1-3, 1-4, 1-5, 2-0, 2-1, 2-2, 2-3, 2-4, 2-5 |

## <a name="additional-reading"></a>Lecturas adicionales

Para más información sobre los conceptos de coherencia, lea los artículos siguientes:

- [High-level TLA+ specifications for the five consistency levels offered by Azure Cosmos DB](https://github.com/Azure/azure-cosmos-tla) (Especificaciones TLA+ de alto nivel de los cinco niveles de coherencia que ofrece Azure Cosmos DB)
- [Replicated Data Consistency explained through Baseball (video)](https://www.youtube.com/watch?v=gluIh8zd26I) (Vídeo sobre Coherencia de datos replicados explicada mediante el béisbol), por Doug Terry
- [Replicated Data Consistency explained through baseball (Coherencia de datos replicados explicada mediante el béisbol) (notas del producto), por Doug Terry](https://www.microsoft.com/en-us/research/publication/replicated-data-consistency-explained-through-baseball/?from=http%3A%2F%2Fresearch.microsoft.com%2Fpubs%2F157411%2Fconsistencyandbaseballreport.pdf)
- [Session Guarantees for Weakly Consistent Replicated Data (Garantías de sesión para datos replicados con coherencia débil)](https://dl.acm.org/citation.cfm?id=383631)
- [Consistency Tradeoffs in Modern Distributed Database Systems Design: CAP is only part of the story (Compromisos de coherencia en el diseño de sistemas modernos de bases de datos distribuidas: CAP es solo una parte de la historia)](https://www.computer.org/web/csdl/index/-/csdl/mags/co/2012/02/mco2012020037-abs.html)
- [Probabilistic Bounded Staleness (PBS) for Practical Partial Quorums](http://vldb.org/pvldb/vol5/p776_peterbailis_vldb2012.pdf) [Obsolescencia limitada probabilística (PBS) para cuórums parciales prácticos].
- [Eventual Consistent - Revisited (Coherencia posible: revisión)](https://www.allthingsdistributed.com/2008/12/eventually_consistent.html)

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre los niveles de coherencia de Cosmos DB, lea los artículos siguientes:

* [Choosing the right consistency level for your application](consistency-levels-choosing.md) (Elección del nivel de coherencia adecuado para la aplicación)
* [Consistency levels across Cosmos DB APIs](consistency-levels-across-apis.md) (Niveles de coherencia en las API de Cosmos DB)
* [Availability and performance tradeoffs for various consistency levels](consistency-levels-tradeoffs.md) (Compromisos entre rendimiento y disponibilidad en los distintos niveles de coherencia)
* [How to configure the default consistency level](how-to-manage-consistency.md#configure-the-default-consistency-level) (Configuración del nivel de coherencia predeterminado)
* [How to configure the default consistency level](how-to-manage-consistency.md#override-the-default-consistency-level) (Reemplazar el nivel de coherencia predeterminado)

