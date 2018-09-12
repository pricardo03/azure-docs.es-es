---
title: archivo de inclusión
description: archivo de inclusión
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 07/11/2018
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: 4a43966180850645584043690b1be9d6ae232f6e
ms.sourcegitcommit: d211f1d24c669b459a3910761b5cacb4b4f46ac9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/06/2018
ms.locfileid: "44027454"
---
El almacenamiento con redundancia de zona (ZRS) replica los datos de manera sincrónica en tres clústeres de almacenamiento en una sola región. Cada clúster de almacenamiento está separado físicamente de los demás y reside en su propia zona de disponibilidad (AZ). Cada zona de disponibilidad, y el clúster ZRS dentro de ella, es autónoma, con distintas herramientas y funcionalidades de red.

Almacenar datos en una cuenta de ZRS garantiza que podrá acceder a los datos y administrarlos en caso de que una zona deje de estar disponible. ZRS proporciona un excelente rendimiento y baja latencia. ZRS ofrece los mismos [objetivos de escalabilidad](../articles/storage/common/storage-scalability-targets.md) que el [almacenamiento con redundancia local (LRS)](../articles/storage/common/storage-redundancy-lrs.md).

Considere a ZRS para los escenarios que requieren coherencia máxima, durabilidad sólida y alta disponibilidad, incluso si una interrupción o un desastre natural hace que un centro de datos de zona deje de estar disponible. ZRS proporciona a los objetos de almacenamiento una durabilidad de al menos el 99,9999999999 % (doce nueves) durante un año determinado.

Para más información sobre las zonas de disponibilidad, consulte [Introducción a las zonas de disponibilidad](https://docs.microsoft.com/azure/availability-zones/az-overview).