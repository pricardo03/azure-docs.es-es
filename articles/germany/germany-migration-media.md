---
title: Migración de recursos multimedia de Azure desde Azure Alemania a Azure global
description: En este artículo se proporciona información sobre cómo migrar los recursos multimedia de Azure desde Azure Alemania a Azure global.
author: gitralf
services: germany
cloud: Azure Germany
ms.author: ralfwi
ms.service: germany
ms.date: 08/15/2018
ms.topic: article
ms.custom: bfmigrate
ms.openlocfilehash: 9c1426391edb42ecc3e74ee84649bb69196c45dd
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "67033658"
---
# <a name="migrate-media-resources-to-global-azure"></a>Migración de recursos multimedia a Azure global

En este artículo se proporciona información que puede ayudarle a migrar los recursos multimedia de Azure desde Azure Alemania a Azure global.

## <a name="media-services"></a>Media Services

En Azure Media Services, puede configurar su propia cuenta de almacenamiento y todos los recursos multimedia. En primer lugar, cree una nueva cuenta de Media Services en Azure global. A continuación, vuelva a cargar los artefactos multimedia correspondientes y realice la codificación y el streaming en la nueva cuenta de Media Services.

Para obtener más información:

- Actualice sus conocimientos completando los [tutoriales de Media Services](https://docs.microsoft.com/azure/media-services/#step-by-step-tutorials).
- Revise la [introducción a Media Services](../media-services/previous/media-services-overview.md).
- Aprenda a [crear una cuenta de Media Services](../media-services/previous/media-services-portal-create-account.md).

## <a name="media-player"></a>Media Player

Puede seleccionar varios puntos de conexión en Azure Media Player. Puede transmitir el contenido desde los puntos de conexión de Azure Alemania o Azure global.

Para más información, consulte [Azure Media Player](https://ampdemo.azureedge.net/azuremediaplayer.html).

## <a name="next-steps"></a>Pasos siguientes

Obtenga información sobre herramientas, técnicas y recomendaciones para migrar los recursos en las siguientes categorías de servicio:

- [Proceso](./germany-migration-compute.md)
- [Redes](./germany-migration-networking.md)
- [Storage](./germany-migration-storage.md)
- [Web](./germany-migration-web.md)
- [Bases de datos](./germany-migration-databases.md)
- [Analytics](./germany-migration-analytics.md)
- [IoT](./germany-migration-iot.md)
- [Integración](./germany-migration-integration.md)
- [Identidad](./germany-migration-identity.md)
- [Seguridad](./germany-migration-security.md)
- [Herramientas de administración](./germany-migration-management-tools.md)
