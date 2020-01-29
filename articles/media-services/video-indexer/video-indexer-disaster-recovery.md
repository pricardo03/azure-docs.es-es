---
title: Continuidad empresarial y recuperación ante desastres en Video Indexer (Azure)
description: Aprenda a conmutar por error a una cuenta secundaria de Video Indexer si se produce una interrupción o un error en un centro de datos regional.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.service: media-services
ms.subservice: video-indexer
ms.workload: ''
ms.topic: article
ms.custom: ''
ms.date: 07/29/2019
ms.author: juliako
ms.openlocfilehash: 2f54c340226a9ea78643df8e0a984c8ed8475c94
ms.sourcegitcommit: 38b11501526a7997cfe1c7980d57e772b1f3169b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/22/2020
ms.locfileid: "76513582"
---
# <a name="handle-video-indexer-business-continuity-and-disaster-recovery"></a>Control de la continuidad empresarial y la recuperación ante desastres en Video Indexer

Video Indexer de Azure Media Services no proporciona una conmutación por error instantánea del servicio si se produce una interrupción o un error en un centro de centros regional. En este artículo se explica cómo configurar el entorno para una conmutación por error con el fin de garantizar la disponibilidad óptima de las aplicaciones y minimizar el tiempo de recuperación en caso de que se produzca un desastre.

Se recomienda que configure la continuidad empresarial y recuperación ante desastres entre las parejas regionales para beneficiarse de las directivas de aislamiento y disponibilidad de Azure. Para más información, consulte [Regiones emparejadas de Azure](https://docs.microsoft.com/azure/best-practices-availability-paired-regions).

## <a name="prerequisites"></a>Prerequisites 

Suscripción a Azure. Si aún no tiene una suscripción de Azure, regístrese para una [evaluación gratuita de Azure](https://azure.microsoft.com/free/).

## <a name="failover-to-a-secondary-account"></a>Conmutación por error a una cuenta secundaria

Si desea implementar BCDR, debe tener dos cuentas de Video Indexer para administrar la redundancia.

1. Cree dos cuentas de Video Indexer conectadas a Azure (consulte el artículo sobre [creación de cuentas](connect-to-azure.md)). Una será para la región primaria y otra para la región de Azure emparejada. 
1. Si se produce un error en la región primaria, cambie a la indexación mediante la cuenta secundaria.

> [!TIP]
> Puede automatizar los procesos de BCDR mediante la configuración de alertas de registro de actividad para las notificaciones de mantenimiento del servicio, según se explica en [Creación de alertas del registro de actividad en notificaciones del servicio](../../service-health/alerts-activity-log-service-notifications.md).

Para información sobre el uso de varios inquilinos, consulte [Administración de varios inquilinos](manage-multiple-tenants.md). Para implementar BCDR, elija una de estas dos opciones: [una cuenta de Video Indexer por cada inquilino](manage-multiple-tenants.md#video-indexer-account-per-tenant) o [una suscripción de Azure por cada inquilino](manage-multiple-tenants.md#azure-subscription-per-tenant).

## <a name="next-steps"></a>Pasos siguientes

[Administración de una cuenta de Video Indexer conectada a Azure](manage-account-connected-to-azure.md)
