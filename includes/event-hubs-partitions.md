---
title: archivo de inclusión
description: archivo de inclusión
services: event-hubs
author: spelluru
ms.service: event-hubs
ms.topic: include
ms.date: 05/22/2019
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: b23f9532aa1ca6f7bae914ff8cb9d7566a0fec86
ms.sourcegitcommit: 64798b4f722623ea2bb53b374fb95e8d2b679318
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2019
ms.locfileid: "67841534"
---
Event Hubs proporciona streaming de mensajes mediante un patrón de consumidor con particiones en el que cada consumidor lee solo un subconjunto específico o una partición del flujo de mensajes. Este patrón permite un escalado horizontal para el procesamiento de eventos y ofrece otras características centradas en los flujos que no están disponibles en las colas y los temas.

Una partición es una secuencia ordenada de eventos que se mantiene en un centro de eventos. A medida que llegan eventos más recientes, se agregan al final de esta secuencia. Una partición puede considerarse como un "registro de confirmación".

![Event Hubs](./media/event-hubs-partitions/partition.png)

Event Hubs retiene datos durante un tiempo de retención configurado que se aplica a todas las particiones del centro de eventos. Los eventos expiran en función del tiempo; no se pueden eliminar explícitamente. Dado que las particiones son independientes y contienen sus propias secuencias de datos, a menudo crecen a velocidades diferentes.

![Event Hubs](./media/event-hubs-partitions/multiple-partitions.png)

El número de particiones se especifica en el momento de la creación y debe estar comprendido entre 2 y 32. El número de particiones no es modificable, por lo que debería tener en cuenta la escala a largo plazo a la hora de configurar este número. Las particiones son un mecanismo de organización de datos relacionado con el paralelismo de bajada necesario para consumir las aplicaciones. El número de particiones de un centro de eventos está directamente relacionado con el número de lectores simultáneos que espera tener. Puede aumentar el número de particiones más allá de 32 poniéndose en contacto con el equipo de Event Hubs.

Aunque las particiones son identificables y se pueden realizar envíos a estas directamente, no se recomienda hacerlo. En su lugar, puede usar las construcciones de nivel superior que se presentan en la sección [Publicador de eventos](../articles/event-hubs/event-hubs-features.md#event-publishers). 

Las particiones se rellenan con una secuencia de datos de eventos que contienen el cuerpo del evento, un contenedor de propiedades definidas por el usuario y diversos metadatos, como su desplazamiento en la partición y su número en la secuencia de streaming.

Se recomienda equilibrar las unidades de procesamiento y las particiones 1:1 para lograr una escalabilidad óptima. Una sola partición tiene una entrada y una salida garantizadas de hasta una unidad de procesamiento. Aunque puede lograr un mayor procesamiento en una partición, no se garantiza el rendimiento. Por este motivo, se recomienda encarecidamente que el número de particiones en un centro de eventos sea mayor o igual que el número de unidades de procesamiento.

Dado el procesamiento total que planea necesitar, conoce el número de unidades de procesamiento que precisa y el número mínimo de particiones, pero, ¿cuántas particiones debería tener? Elija el número de particiones en función del paralelismo de bajada que quiere conseguir, así como de las futuras necesidades de procesamiento. No hay ningún cargo por el número de particiones que tiene dentro de un centro de eventos.

Para más información acerca de particiones y el equilibrio entre disponibilidad y confiabilidad, consulte los artículos [Guía de programación de Event Hubs](../articles/event-hubs/event-hubs-programming-guide.md#partition-key) y [Disponibilidad y coherencia en Event Hubs](../articles/event-hubs/event-hubs-availability-and-consistency.md).
