---
title: Niveles de coherencia en Azure Cosmos DB
description: Azure Cosmos DB tiene cinco niveles de coherencia para ayudar a equilibrar la coherencia, la disponibilidad y la latencia finales.
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/20/2019
ms.openlocfilehash: dcd51756a9c5a5a24a082862bb911cc2d2605d61
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "65954359"
---
# <a name="consistency-levels-in-azure-cosmos-db"></a>Niveles de coherencia en Azure Cosmos DB

Las bases de datos distribuidas que dependen de la replicación para su alta disponibilidad, su baja latencia o ambas, constituyen el compromiso fundamental entre la coherencia de lectura y la disponibilidad, la latencia y el rendimiento. La mayoría de las bases de datos distribuidas disponibles comercialmente solicitan a los desarrolladores que elijan entre los dos modelos de coherencia extrema: coherencia *fuerte* y *posible* coherencia. La  [linealidad](https://cs.brown.edu/~mph/HerlihyW90/p463-herlihy.pdf) o el modelo de coherencia fuerte es el estándar de oro de la programación de datos, pero obliga a pagar el precio de una latencia más alta (en estado estable) y una menor disponibilidad (durante los errores). Por otro lado, la posible coherencia ofrece una mayor disponibilidad y un mejor rendimiento, pero es difícil programar aplicaciones. 

Azure Cosmos DB se aproxima a la coherencia de datos como un espectro de opciones en lugar de como dos extremos. La coherencia fuerte y la posible coherencia están en los extremos del espectro, pero existen muchas opciones de coherencia en todo el espectro. Los desarrolladores pueden usar estas opciones para elegir opciones precisas y compensaciones pormenorizadas con respecto a la alta disponibilidad y al rendimiento. 

Con Azure Cosmos DB, los desarrolladores pueden elegir entre cinco modelos de coherencia bien definidos en el espectro de coherencia. Desde la coherencia más sólida a una más relajada, los modelos incluyen una coherencia *fuerte*, de *obsolescencia limitada*, de *sesión*, *de prefijo coherente* y *posible* coherencia. Los modelos están bien definidos, son intuitivos y se pueden usar en escenarios reales específicos. Cada modelo proporciona [compensaciones entre la disponibilidad y el rendimiento](consistency-levels-tradeoffs.md) y cuenta con el respaldo de Acuerdos de Nivel de Servicio. En la imagen siguiente se muestran los distintos niveles de coherencia como un espectro.

![Coherencia como un espectro](./media/consistency-levels/five-consistency-levels.png)

Los niveles de coherencia son independientes de la región y están garantizados para todas las operaciones, independientemente de la región desde la que se realizan las operaciones de lectura y escritura, del número de regiones asociadas con la cuenta de Azure Cosmos o de si la cuenta está configurada para una o varias regiones de escritura.

## <a name="scope-of-the-read-consistency"></a>Ámbito de la coherencia de lectura

La coherencia de lectura se aplica a una operación de lectura limitada a un intervalo de claves de partición o una partición lógica. Un cliente remoto o un procedimiento almacenado pueden emitir la operación de lectura.

## <a name="configure-the-default-consistency-level"></a>Configuración del nivel de coherencia predeterminado

Puede configurar el nivel de coherencia predeterminado de su cuenta de Azure Cosmos en cualquier momento. El nivel de coherencia predeterminado configurado en su cuenta se aplica a todas las bases de datos y contenedores de bases de datos de Azure Cosmos de esa cuenta. Todas las operaciones de lectura y consulta que se emitan con arreglo a un contenedor o una base de datos usan el nivel de coherencia especificado de forma predeterminada. Para más información, consulte cómo [configurar el nivel de coherencia predeterminado](how-to-manage-consistency.md#configure-the-default-consistency-level).

## <a name="guarantees-associated-with-consistency-levels"></a>Garantías asociadas a los niveles de coherencia

Los Acuerdos de Nivel de Servicio que proporciona Azure Cosmos DB garantizan que el 100 por ciento de las solicitudes de lectura cumplan con la garantía de coherencia en cualquier nivel de coherencia que elija. Una solicitud de lectura cumple el Acuerdo de Nivel de Servicio de coherencia si se satisfacen todas las garantías de coherencia asociadas al nivel de coherencia. Las definiciones precisas de los cinco niveles de coherencia de Azure Cosmos DB que usan el [lenguaje de especificación TLA+](https://lamport.azurewebsites.net/tla/tla.html) se proporcionan en el repositorio [azure-cosmos-tla](https://github.com/Azure/azure-cosmos-tla) de GitHub. 

Aquí se describe la semántica de los cinco niveles de coherencia:

- **Alta**: La coherencia fuerte ofrece una garantía de [linealidad](https://aphyr.com/posts/313-strong-consistency-models). Se garantiza que las lecturas devuelven la versión más reciente de un elemento. Un cliente nunca ve una escritura no confirmada ni parcial. Se garantiza que los usuarios siempre leerán la escritura confirmada más reciente.

- **De obsolescencia entrelazada**: Se garantiza que las lecturas respetan la garantía de prefijo coherente. Las lecturas pueden ir con retraso respecto a las escrituras en un máximo de versiones *"K"* (es decir, "actualizaciones") de un elemento o el intervalo de tiempo *"T"* . En otras palabras, cuando elige la obsolescencia limitada, la "obsolescencia" se puede configurar de dos maneras: 

  * El número de versiones (*K*) del elemento
  * El intervalo de tiempo (*T*) que las lecturas pueden retrasarse con respecto a las escrituras 

  La obsolescencia limitada ofrece un orden global total, excepto dentro de la "ventana de obsolescencia". La garantía de lectura monotónica existe dentro de una región, tanto dentro como fuera de la ventana de obsolescencia. La coherencia fuerte tiene la misma semántica que la que ofrece la obsolescencia limitada. La ventana de obsolescencia es igual a cero. La obsolescencia limitada también se conoce como linealidad retardada. Cuando un cliente realiza operaciones de lectura en una región que acepta las escrituras, las garantías que proporciona la obsolescencia limitada son idénticas a las garantías por la coherencia fuerte.

- **Sesión**: Se garantiza que las lecturas respetan las garantías de prefijo coherente (suponiendo que hay una sesión de "escritor" única), lecturas monotónicas, escrituras monotónicas, lectura de la escritura y escritura tras las lecturas. La coherencia de sesión se limita a una sesión de cliente.

- **De prefijo coherente**: Las actualizaciones que se devuelven contienen prefijos para todas las actualizaciones, sin espacios. El nivel de coherencia del prefijo coherente garantiza que las lecturas nunca vean escrituras desordenadas.

- **Ocasional**: No hay ninguna garantía de ordenación para las lecturas. En ausencia de escrituras adicionales, las réplicas terminarán por converger.

## <a name="consistency-levels-explained-through-baseball"></a>Niveles de coherencia explicados a través del béisbol

Tomemos como ejemplo un escenario de un juego de béisbol. Imagine una secuencia de escrituras que representa el marcador de un juego de béisbol. El típico marcador de dos líneas con el resultado de cada entrada se describe en el documento [Replicated data consistency through baseball](https://www.microsoft.com/en-us/research/wp-content/uploads/2011/10/ConsistencyAndBaseballReport.pdf) (Descripción de la coherencia de datos mediante el béisbol). Este juego de béisbol hipotético actualmente está en la mitad de la séptima entrada, es decir, la entrada de la suerte. Los visitantes pierden por un marcador de 2 a 5, como se muestra a continuación:

| | **1** | **2** | **3** | **4** | **5** | **6** | **7** | **8** | **9** | **Carreras** |
| - | - | - | - | - | - | - | - | - | - | - |
| **Visitante** | 0 | 0 | 1 | 0 | 1 | 0 | 0 |  |  | 2 |
| **Local** | 1 | 0 | 1 | 1 | 0 | 2 |  |  |  | 5 |

Un contenedor de Azure Cosmos guarda el número total de carreras del equipo visitante y del equipo local. Mientras el partido está en curso, las diferentes garantías de lectura pueden tener como resultado que los clientes lean diferentes marcadores. En la siguiente tabla se enumera el conjunto completo de marcadores que se puede devolver con la lectura de los marcadores del equipo visitante y el equipo local con cada uno de los cinco niveles de garantía de coherencia. El marcador del equipo visitante aparece primero. Los distintos valores devueltos posibles están separados por comas.

| **Nivel de coherencia** | **Marcadores (equipo visitante, equipo local)** |
| - | - |
| **Fuerte** | 2-5 |
| **Obsolescencia limitada** | Loos marcadores van como máximo con una entrada de retraso: 2-3, 2-4, 2-5 |
| **De sesión** | <ul><li>Para el agente de escritura: 2-5</li><li> Para cualquier otro que no sea el agente de escritura: 0-0, 0-1, 0-2, 0-3, 0-4, 0-5, 1-0, 1-1, 1-2, 1-3, 1-4, 1-5, 2-0, 2-1, 2-2, 2-3, 2-4, 2-5</li><li>Después de la lectura de 1-3: 1-3, 1-4, 1-5, 2-3, 2-4, 2-5</li> |
| **De prefijo coherente** | 0-0, 0-1, 1-1, 1-2, 1-3, 2-3, 2-4, 2-5 |
| **Posible** | 0-0, 0-1, 0-2, 0-3, 0-4, 0-5, 1-0, 1-1, 1-2, 1-3, 1-4, 1-5, 2-0, 2-1, 2-2, 2-3, 2-4, 2-5 |

## <a name="additional-reading"></a>Lecturas adicionales

Para más información sobre los conceptos de coherencia, lea los artículos siguientes:

- [High-level TLA+ specifications for the five consistency levels offered by Azure Cosmos DB](https://github.com/Azure/azure-cosmos-tla) (Especificaciones TLA+ de alto nivel de los cinco niveles de coherencia que ofrece Azure Cosmos DB)
- [Replicated Data Consistency Explained Through Baseball (video)](https://www.youtube.com/watch?v=gluIh8zd26I) (Vídeo sobre Coherencia de datos replicados explicada mediante el béisbol), por Doug Terry
- [Replicated Data Consistency Explained Through Baseball (whitepaper)](https://www.microsoft.com/en-us/research/publication/replicated-data-consistency-explained-through-baseball/?from=http%3A%2F%2Fresearch.microsoft.com%2Fpubs%2F157411%2Fconsistencyandbaseballreport.pdf) (Coherencia de datos replicados explicada mediante el béisbol [notas del producto]), por Doug Terry
- [Session guarantees for weakly consistent replicated data](https://dl.acm.org/citation.cfm?id=383631) (Garantías de sesión para datos replicados con coherencia débil)
- [Consistency Tradeoffs in Modern Distributed Database Systems Design: CAP is Only Part of the Story](https://www.computer.org/csdl/magazine/co/2012/02/mco2012020037/13rRUxjyX7k) (Compromisos de coherencia en el diseño de sistemas modernos de bases de datos distribuidas: CAP es solo parte de la historia)
- [Probabilistic Bounded Staleness (PBS) for Practical Partial Quorums](https://vldb.org/pvldb/vol5/p776_peterbailis_vldb2012.pdf) [Obsolescencia limitada probabilística (PBS) para cuórums parciales prácticos].
- [Eventual Consistent - Revisited (Coherencia posible: revisión)](https://www.allthingsdistributed.com/2008/12/eventually_consistent.html)

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre los niveles de coherencia de Azure Cosmos DB, lea los artículos siguientes:

* [Elección del nivel de coherencia adecuado para la aplicación](consistency-levels-choosing.md)
* [Niveles de coherencia en Azure Cosmos DB](consistency-levels-across-apis.md)
* [Availability and performance tradeoffs for various consistency levels](consistency-levels-tradeoffs.md) (Compromisos entre rendimiento y disponibilidad en los distintos niveles de coherencia)
* [Configuración del nivel de coherencia predeterminado](how-to-manage-consistency.md#configure-the-default-consistency-level)
* [Invalidación del nivel de coherencia predeterminado](how-to-manage-consistency.md#override-the-default-consistency-level)

