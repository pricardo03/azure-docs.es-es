---
title: Recuperación geográfica ante desastres en Azure Event Grid | Microsoft Docs
description: Describe cómo Azure Event Grid admite recuperación geográfica ante desastres (GeoDR) automáticamente.
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: conceptual
ms.date: 05/24/2019
ms.author: spelluru
ms.openlocfilehash: 5b5c973a8daa8776efb0909092c569ea46902265
ms.sourcegitcommit: 009334a842d08b1c83ee183b5830092e067f4374
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2019
ms.locfileid: "66307323"
---
# <a name="server-side-geo-disaster-recovery-in-azure-event-grid"></a>Recuperación de desastres geográfica del lado servidor en Azure Event Grid
Event Grid tiene ahora una recuperación de desastres geográfica automática (GeoDR) de metadatos no solo para el nuevo, pero todos los dominios existentes, temas y suscripciones a eventos. Si deja de funcionar toda una región de Azure, Event Grid ya tiene todos los metadatos de infraestructura relacionados con eventos sincronizados en una región emparejada. Los nuevos eventos se empezarán a fluir de nuevo sin intervención del usuario. 

Recuperación ante desastres se mide con dos métricas:

- [Objetivo de punto de recuperación (RPO)](https://en.wikipedia.org/wiki/Disaster_recovery#Recovery_Point_Objective): los minutos u horas de datos que se pueden perder.
- [Objetivo de tiempo de recuperación (RTO)](https://en.wikipedia.org/wiki/Disaster_recovery#Recovery_time_objective): el detalle de los minutos de horas puede ser el servicio.

Conmutación automática por error de la cuadrícula de eventos tiene diferentes RPO y RTO para los metadatos (suscripciones de eventos, etc.) y datos (eventos). Si tiene una especificación diferente de las siguientes, puede implementar su propio [conmutar por error del lado cliente utilizando el tema de las API de estado](custom-disaster-recovery.md).

## <a name="recovery-point-objective-rpo"></a>Objetivo de punto de recuperación (RPO)
- **Metadata RPO**: cero minutos. Cada vez que se crea un recurso de Event Grid, se replica al instante entre regiones. Cuando se produce una conmutación por error, no se pierde ningún metadato.
- **Datos RPO**: Si el sistema está en buen estado y el RPO para eventos al día en tráfico existente en el momento de la conmutación por error regional, es de aproximadamente 5 minutos.

## <a name="recovery-time-objective-rto"></a>Objetivo de tiempo de recuperación (RTO)
- **RTO metadatos**: Aunque generalmente se produce mucho más rápido, dentro de 60 minutos, Event Grid se empezará a aceptar las llamadas de creación/actualización/eliminación de temas y suscripciones.
- **Datos RTO**: Al igual que los metadatos, suele deberse a mucho más rápidamente, sin embargo en 60 minutos, Event Grid empezará a aceptar tráfico nuevo después de una conmutación por error regional.

> [!NOTE]
> El costo de los metadatos GeoDR en Event Grid es: 0 $.


## <a name="next-steps"></a>Pasos siguientes
Si desea implementar la lógica de conmutación por error de cliente propios, consulte [# crear su propios recuperación ante desastres para temas personalizados de Event Grid](custom-disaster-recovery.md)
