---
title: Requisitos previos de los módulos IoT Edge de Azure | Microsoft Docs
description: Requisitos previos para publicar un módulo IoT Edge.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: dan-wesley
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 10/18/2018
ms.author: pbutlerm
ms.openlocfilehash: 7271a97dc7ab9de1840d809ded0ba1c2940ed83f
ms.sourcegitcommit: 707bb4016e365723bc4ce59f32f3713edd387b39
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/19/2018
ms.locfileid: "49433547"
---
# <a name="iot-edge-module-publishing-prerequisites"></a>Requisitos previos de publicación de módulos IoT Edge

En este artículo se describen los requisitos previos para publicar una oferta de módulo IoT Edge.

Para obtener más información sobre los módulos IoT Edge y las ventajas de publicar un módulo en Azure Marketplace, vea la [guía de publicación de módulos IoT Edge](https://docs.microsoft.com/azure/marketplace/iot-edge-module).

## <a name="publishing-prerequisites"></a>Requisitos previos de publicación

Para publicar un módulo IoT Edge en Azure Marketplace, es preciso cumplir los siguientes requisitos previos:

<!-- P2: It would be great to point to the terms of use of CPP here. This can often be a blocker for big companies and these terms of use are not anonymously visible yet.-->
- Acceso a [Cloud Partner Portal](https://cloudpartner.azure.com/). Para obtener más información, vea [Guía de publicación de Azure Marketplace y AppSource](https://docs.microsoft.com/azure/marketplace/marketplace-publishers-guide).
- Aceptación de los [Términos de Microsoft Azure Marketplace](https://azure.microsoft.com/support/legal/marketplace-terms/)
- Hospedaje de los recursos técnicos del módulo IoT Edge en Azure Container Registry.  Para obtener más información, vea [Preparar los recursos técnicos del módulo IoT Edge](./cpp-create-technical-assets.md)
- Metadatos del módulo IoT Edge listos para usar. Por ejemplo, (lista no exhaustiva):
    - Un título
    - Una descripción (en formato HTML)
    - Una imagen de logotipo (formato PNG y tamaños de imagen fijos, como 40 x 40 px, 90 x 90 px, 115 x 115 px, 255 x 115 px)
    - Un término de uso y directiva de privacidad
    - Una configuración predeterminada del módulo que incluya: rutas, propiedades deseadas gemelas, createOptions y variables de entorno.
    - Documentación
    - Contactos de soporte técnico

## <a name="next-steps"></a>Pasos siguientes

- [Preparar los recursos técnicos del módulo IoT Edge](./cpp-create-technical-assets.md)
- [Creación de una oferta del módulo de IoT Edge](./cpp-create-offer.md)
