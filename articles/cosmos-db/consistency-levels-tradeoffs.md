---
title: Compromisos entre rendimiento y disponibilidad en los distintos niveles de coherencia en Azure Cosmos DB | Microsoft Docs
description: Compromisos entre rendimiento y disponibilidad en los distintos niveles de coherencia en Azure Cosmos DB.
keywords: coherencia, rendimiento, azure cosmos db, azure, Microsoft azure
services: cosmos-db
author: markjbrown
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 10/20/2018
ms.author: mjbrown
ms.openlocfilehash: 0e4105d6f56a8eb45a83e970c85319cf25041781
ms.sourcegitcommit: 5a1d601f01444be7d9f405df18c57be0316a1c79
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2018
ms.locfileid: "51514781"
---
# <a name="availability-and-performance-tradeoffs-for-various-consistency-levels-in-azure-cosmos-db"></a>Contrapartidas entre rendimiento y disponibilidad en los distintos niveles de coherencia en Azure Cosmos DB

Las bases de datos distribuidas que dependen de la replicación para su alta disponibilidad, su baja latencia o ambas, constituyen el compromiso fundamental entre la coherencia de lectura y la disponibilidad, la latencia y el rendimiento. Azure Cosmos DB se aproxima a la coherencia de datos como un espectro de opciones en lugar de los dos extremos de coherencia, alta y posible. Cosmos DB permite a los desarrolladores elegir entre cinco modelos de coherencia bien definidos dentro del espectro de coherencia (del más fuerte al más débil): **fuerte**, **de obsolescencia limitada**, **de sesión**, **de prefijo coherente** y **posible**. Cada uno de los cinco modelos de coherencia ofrece contrapartidas entre rendimiento y disponibilidad, y están respaldados por Acuerdos de Nivel de Servicio.

## <a name="consistency-levels-and-latency"></a>Latencia y niveles de coherencia

- Se garantiza que la **latencia de lectura** de todos los niveles de coherencia siempre es inferior a 10 milisegundos en el percentil 99 y está respaldada por el contrato de nivel de servicio. La latencia media de lectura (en el percentil 50) es normalmente de dos milisegundos o menos.

- Excepto para las cuentas de Cosmos que abarcan varias regiones y están configuradas con coherencia fuerte, se garantiza que la **latencia de escritura** de los restantes niveles de coherencia siempre es inferior a 10 milisegundos en el percentil 99. Esta latencia de escritura está respaldada por el Acuerdo de Nivel de Servicio. La latencia media de escritura (en el percentil 50) es normalmente de cinco milisegundos o menos.

- En el caso de las cuentas de Cosmos que tienen varias regiones configuradas con consistencia fuerte (actualmente en versión preliminar), se garantiza que la **latencia de escritura** es inferior a < (2 * Tiempo de ida y vuelta/RTT) + 10 milisegundos en el percentil 99. El RTT entre cualquiera de las dos regiones más alejadas asociadas a su cuenta de Cosmos. La latencia de RTT exacta depende de la distancia a la velocidad de la luz y la topología de red exacta de Azure. Redes de Azure no proporciona ningún contrato de nivel de servicio de latencia para el RTT entre dos regiones de Azure. Las latencias de replicación de Cosmos DB se muestran en su cuenta de Cosmos en Azure Portal, lo que le permite supervisar las latencias de replicación entre diversas regiones asociadas a su cuenta de Cosmos.

## <a name="consistency-levels-and-throughput"></a>Rendimiento y niveles de coherencia

- Para el mismo número de unidades de solicitud, los niveles de coherencia de sesión, de prefijo coherente y posible proporcionan aproximadamente el doble de rendimiento de lectura en comparación con la coherencia fuerte o de obsolescencia limitada.

- Para un tipo determinado de operación de escritura (por ejemplo, Insert, Replace, Upsert, Delete, etc.), el rendimiento de escritura de las unidades de solicitud es idéntico para todos los niveles de coherencia.

## <a name="next-steps"></a>Pasos siguientes

En los artículos siguientes puede obtener más información sobre los compromisos entre distribución global y coherencia general en sistemas distribuidos:

* [Consistency Tradeoffs in Modern Distributed Database Systems Design](https://www.computer.org/web/csdl/index/-/csdl/mags/co/2012/02/mco2012020037-abs.html) (Compromisos de coherencia en el diseño de sistemas modernos de bases de datos distribuidas)
* [Alta disponibilidad](high-availability.md)
* [Acuerdo de Nivel de Servicio de Azure Cosmos DB](https://azure.microsoft.com/support/legal/sla/cosmos-db/v1_2/)
