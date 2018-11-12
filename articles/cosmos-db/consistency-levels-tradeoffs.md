---
title: Compromisos entre rendimiento y disponibilidad en los distintos niveles de coherencia | Microsoft Docs
description: Compromisos entre rendimiento y disponibilidad en los distintos niveles de coherencia en Azure Cosmos DB.
keywords: consistency, performance, azure cosmos db, azure, Microsoft azure
services: cosmos-db
author: markjbrown
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 10/20/2018
ms.author: mjbrown
ms.openlocfilehash: 061a7c223d0e1c6fa7384d7defe9f84f470236ce
ms.sourcegitcommit: dbfd977100b22699823ad8bf03e0b75e9796615f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2018
ms.locfileid: "50243830"
---
# <a name="availability-and-performance-tradeoffs-for-various-consistency-levels"></a>Compromisos entre rendimiento y disponibilidad en los distintos niveles de coherencia

Las bases de datos distribuidas que dependen de la replicación para su alta disponibilidad, su baja latencia o ambas, constituyen el compromiso fundamental entre la coherencia de lectura y la disponibilidad1, la latencia2 y el rendimiento. Azure Cosmos DB se aproxima a la coherencia de datos como un espectro de opciones en lugar de los dos extremos de coherencia, alta y posible. Cosmos DB permite a los desarrolladores elegir entre cinco modelos de coherencia bien definidos dentro del espectro de coherencia (del más fuerte al más débil): **alta**, **de obsolescencia limitada**, **de sesión**, **de prefijo coherente** y **posible**. Cada uno de los cinco modelos de coherencia ofrece compromisos claros entre rendimiento y disponibilidad, y están respaldados por contratos de nivel de servicio.

## <a name="consistency-levels-and-latency"></a>Latencia y niveles de coherencia

- Se garantiza que la **latencia de lectura** de todos los niveles de coherencia siempre es inferior a 10 milisegundos en el percentil 99 y está respaldada por el contrato de nivel de servicio. La latencia media de lectura (en el percentil 50) es normalmente de dos milisegundos o menos.
- Excepto para las cuentas de Cosmos que abarcan varias regiones y están configuradas con la coherencia alta, se garantiza que la **latencia de escritura** de todos los niveles de coherencia siempre es inferior a 10 milisegundos en el percentil 99 y está respaldada por el contrato de nivel de servicio. La latencia media de escritura (en el percentil 50) es normalmente de cinco milisegundos o menos.
- En el caso de las cuentas de Cosmos con varias regiones configuradas con consistencia alta (actualmente en versión preliminar), se garantiza que la **latencia de escritura** es inferior a < (2 * RTT) + 10 milisegundos en el percentil 99. El RTT de la más alejada de las dos regiones asociadas a su cuenta de Cosmos. La latencia de RTT exacta depende de la distancia a la velocidad de la luz y la topología de red exacta de Azure. Redes de Azure no proporciona ningún contrato de nivel de servicio de latencia para el RTT entre dos regiones de Azure. Las latencias de replicación de Cosmos DB se muestran en su cuenta de Cosmos en Azure Portal, lo que le permite supervisar las latencias de replicación entre diversas regiones asociadas a su cuenta de Cosmos.

## <a name="consistency-levels-and-throughput"></a>Rendimiento y niveles de coherencia

- Para la misma cantidad de unidades de solicitud (RU), la coherencia de sesión, de prefijo coherente y posible proporcionan aproximadamente el doble de rendimiento de lectura en comparación con la coherencia alta o de obsolescencia limitada.
- Para un tipo determinado de operación de escritura (por ejemplo, Insert, Replace, Upsert, Delete, etc.), el rendimiento de escritura de las RU es idéntico para todos los niveles de coherencia.

## <a name="consistency-levels-and-durability"></a>Durabilidad y niveles de coherencia

Antes de que una operación de escritura sea reconocida por el cliente, un quórum de réplicas dentro de la región que acepta las escrituras confirma permanentemente los datos. Además, si el contenedor está configurado con la directiva de indexación coherente, el quórum de réplicas también actualiza, replica y confirma permanentemente el índice de forma sincrónica antes de que la escritura sea reconocida por el cliente. 

En la tabla siguiente se resume la posible ventana de pérdida de datos en caso de un desastre regional para las cuentas de Cosmos que abarcan varias regiones.

| **Nivel de coherencia** | **Ventana de la posible pérdida de datos en caso de un desastre regional** |
| - | - |
| Alta | Cero |
| De obsolescencia limitada | Se limita a la "ventana de obsolescencia" configurada por el desarrollador en la cuenta de Cosmos. |
| Sesión | Hasta cinco segundos |
| De prefijo coherente | Hasta cinco segundos |
| Ocasional | Hasta cinco segundos |

## <a name="next-steps"></a>Pasos siguientes

En los artículos siguientes puede obtener más información sobre los compromisos entre distribución global y coherencia general en sistemas distribuidos:

* [Consistency Tradeoffs in Modern Distributed Database Systems Design](https://www.computer.org/web/csdl/index/-/csdl/mags/co/2012/02/mco2012020037-abs.html) (Compromisos de coherencia en el diseño de sistemas modernos de bases de datos distribuidas)
* [Alta disponibilidad](high-availability.md)
* [Acuerdo de Nivel de Servicio de Azure Cosmos DB](https://azure.microsoft.com/support/legal/sla/cosmos-db/v1_2/)
