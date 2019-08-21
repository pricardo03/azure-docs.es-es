---
title: archivo de inclusión
description: archivo de inclusión
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 06/28/2019
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: 9326e6dac88fa23a7bb2bc489064aca5ef52980b
ms.sourcegitcommit: df7942ba1f28903ff7bef640ecef894e95f7f335
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/14/2019
ms.locfileid: "69015941"
---
El almacenamiento con redundancia de zona (ZRS) replica los datos de manera sincrónica en tres clústeres de almacenamiento en una sola región. Cada clúster de almacenamiento está separado físicamente de los demás y está ubicado su propia zona de disponibilidad (AZ). Cada zona de disponibilidad, y el clúster ZRS dentro de ella, es autónoma, e incluye distintas herramientas y funcionalidades de red. Una solicitud de escritura en una cuenta de almacenamiento de ZRS se devuelve correctamente solo después de que los datos se escriben en todas las réplicas de los tres clústeres.

Al almacenar los datos en una cuenta de almacenamiento con replicación ZRS, aún puede tener acceso a los datos y administrarlos si una zona de disponibilidad no está disponible. ZRS proporciona un excelente rendimiento y baja latencia. ZRS ofrece los mismos [objetivos de escalabilidad](../articles/storage/common/storage-scalability-targets.md) que el [almacenamiento con redundancia local (LRS)](../articles/storage/common/storage-redundancy-lrs.md).

Considere la posibilidad de usar ZRS para escenarios que requieren coherencia, durabilidad y alta disponibilidad. Incluso si una interrupción o un desastre natural hace que una zona de disponibilidad no esté disponible, ZRS ofrece la durabilidad de los objetos de almacenamiento de al menos el 99,9999999999 % (doce nueves) durante un año determinado.

El almacenamiento con redundancia de zona geográfica (GZRS) (versión preliminar) replica los datos sincrónicamente en tres zonas de disponibilidad de Azure en la región primaria y, a continuación, replica los datos de forma asincrónica en la región secundaria. GZRS proporciona alta disponibilidad junto con la durabilidad máxima. El almacenamiento con redundancia de zona geográfica (GZRS) está diseñado para proporcionar una durabilidad mínima del 99,99999999999999 % (16 nueves) de los objetos en un año determinado. Para obtener acceso de lectura a los datos de la región secundaria, habilite el almacenamiento con redundancia de zona geográfica con acceso de lectura (RA-GZRS). Para obtener más información acerca de GZRS, consulte [Almacenamiento con redundancia de zona geográfica para obtener alta disponibilidad y durabilidad máxima (versión preliminar)](../articles/storage/common/storage-redundancy-lrs.md).

Para más información sobre las zonas de disponibilidad, consulte [Introducción a las zonas de disponibilidad](https://docs.microsoft.com/azure/availability-zones/az-overview).
