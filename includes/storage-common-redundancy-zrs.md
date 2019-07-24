---
title: archivo de inclusión
description: archivo de inclusión
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 11/04/2018
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: d25efa8c666fa91c7bd652a7cf931ea6ca379aa2
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "67132982"
---
El almacenamiento con redundancia de zona (ZRS) replica los datos de manera sincrónica en tres clústeres de almacenamiento en una sola región. Cada clúster de almacenamiento está separado físicamente de los demás y está ubicado su propia zona de disponibilidad (AZ). Cada zona de disponibilidad, y el clúster ZRS dentro de ella, es autónoma, e incluye distintas herramientas y funcionalidades de red. Una solicitud de escritura en una cuenta de almacenamiento de ZRS se devuelve correctamente solo después de que los datos se escriben en todas las réplicas de los tres clústeres.

Al almacenar los datos en una cuenta de almacenamiento con replicación ZRS, aún puede tener acceso a los datos y administrarlos si una zona de disponibilidad no está disponible. ZRS proporciona un excelente rendimiento y baja latencia. ZRS ofrece los mismos [objetivos de escalabilidad](../articles/storage/common/storage-scalability-targets.md) que el [almacenamiento con redundancia local (LRS)](../articles/storage/common/storage-redundancy-lrs.md).

Considere la posibilidad de usar ZRS para escenarios que requieren coherencia, durabilidad y alta disponibilidad. Incluso si una interrupción o un desastre natural hace que una zona de disponibilidad no esté disponible, ZRS ofrece la durabilidad de los objetos de almacenamiento de al menos el 99,9999999999 % (doce nueves) durante un año determinado.

Para más información sobre las zonas de disponibilidad, consulte [Introducción a las zonas de disponibilidad](https://docs.microsoft.com/azure/availability-zones/az-overview).