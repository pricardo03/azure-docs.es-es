---
title: Contrapartidas entre rendimiento y disponibilidad en los distintos niveles de coherencia en Azure Cosmos DB
description: Compromisos entre rendimiento y disponibilidad en los distintos niveles de coherencia en Azure Cosmos DB.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/20/2018
ms.author: mjbrown
ms.reviewer: sngun
ms.openlocfilehash: 52f08511d16e216ced7e3d1de11eae960cdbaeb8
ms.sourcegitcommit: 8330a262abaddaafd4acb04016b68486fba5835b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/04/2019
ms.locfileid: "54041873"
---
# <a name="availability-and-performance-tradeoffs-for-various-consistency-levels-in-azure-cosmos-db"></a>Contrapartidas entre rendimiento y disponibilidad en los distintos niveles de coherencia en Azure Cosmos DB

Las bases de datos distribuidas que dependen de la replicación para la alta disponibilidad, la baja latencia o ambas deben realizar compensaciones. Tales compensaciones se realizan entre la coherencia y la disponibilidad de lectura, la latencia y el rendimiento. 

Azure Cosmos DB se aproxima a la coherencia de datos como un espectro de opciones. Este enfoque abarca opciones que van más allá de los dos extremos de coherencia (alta y ocasional). Puede elegir entre cinco modelos bien definidos en el espectro de la coherencia. De más fuerte a más débil, los modelos son:

- Alta 
- Uso vinculado 
- Sesión 
- Prefijo coherente 
- Ocasional 

Cada modelo proporciona compensaciones entre la disponibilidad y el rendimiento y cuenta con el respaldo de completos Acuerdos de Nivel de Servicio.

## <a name="consistency-levels-and-latency"></a>Latencia y niveles de coherencia

- Se garantiza que la latencia de lectura de todos los niveles de coherencia siempre es inferior a 10 milisegundos en el percentil 99. Esta latencia de escritura está respaldada por el Acuerdo de Nivel de Servicio. La latencia media de lectura (en el percentil 50) es normalmente de dos milisegundos o menos. Las cuentas de Azure Cosmos que abarcan varias regiones y están configuradas con una coherencia alta son una excepción a esta garantía.

-  Se garantiza que la latencia de escritura de los niveles de coherencia restantes siempre es inferior a 10 milisegundos en el percentil 99. Esta latencia de escritura está respaldada por el Acuerdo de Nivel de Servicio. La latencia media de escritura (en el percentil 50) es normalmente de cinco milisegundos o menos.

Algunas cuentas de Azure Cosmos podrían tener varias regiones configuradas con una coherencia alta. En este caso, se garantiza que la latencia de escritura sea inferior a dos veces el tiempo de ida y vuelta (RTT) más 10 milisegundos en el percentil 99. El RTT entre cualquiera de las dos regiones más alejadas se asocia a su cuenta de Azure Cosmos. Es igual al RTT entre cualquiera de las dos regiones más alejadas asociadas a su cuenta de Azure Cosmos. Esta opción se encuentra actualmente en versión preliminar. 

La latencia de RTT exacta depende de la distancia a la velocidad de la luz y la topología de red de Azure. Redes de Azure no proporciona ningún Acuerdo de Nivel de Servicio de latencia para el RTT entre dos regiones de Azure. Para la cuenta de Azure Cosmos, las latencias de replicación se muestran en Azure Portal. Puede usar Azure Portal para supervisar las latencias de replicación entre diversas regiones asociadas con su cuenta.

## <a name="consistency-levels-and-throughput"></a>Rendimiento y niveles de coherencia

- Para el mismo número de unidades de solicitud, los niveles de coherencia de sesión, de prefijo coherente y posible proporcionan aproximadamente el doble de rendimiento de lectura en comparación con la coherencia fuerte o de obsolescencia limitada.

- Para un tipo determinado de operación de escritura (por ejemplo, Insert, Replace, Upsert o Delete), el rendimiento de escritura de las unidades de solicitud es idéntico para todos los niveles de coherencia.

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre las compensaciones entre distribución global y coherencia general en sistemas distribuidos. Consulte los artículos siguientes:

* [Consistency Tradeoffs in Modern Distributed Database Systems Design](https://www.computer.org/web/csdl/index/-/csdl/mags/co/2012/02/mco2012020037-abs.html) (Compromisos de coherencia en el diseño de sistemas modernos de bases de datos distribuidas)
* [Alta disponibilidad](high-availability.md)
* [Acuerdo de Nivel de Servicio de Azure Cosmos DB](https://azure.microsoft.com/support/legal/sla/cosmos-db/v1_2/)
