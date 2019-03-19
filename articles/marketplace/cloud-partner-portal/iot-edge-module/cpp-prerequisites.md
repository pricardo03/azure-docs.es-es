---
title: Requisitos previos de los módulos IoT Edge de Azure | Microsoft Docs
description: Requisitos previos para publicar un módulo IoT Edge.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: v-miclar
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 03/13/2019
ms.author: pbutlerm
ms.openlocfilehash: a4f1023bdf8a49fccbbda1fd0dc537f83a3acee1
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/18/2019
ms.locfileid: "58013801"
---
# <a name="iot-edge-module-publishing-prerequisites"></a>Requisitos previos de publicación de módulos IoT Edge

En este artículo se describen los requisitos previos para publicar una oferta de módulo IoT Edge.  Si aún no lo ha hecho, revise el [Guía de publicación de módulos de IoT Edge](../..//iot-edge-module.md).


## <a name="publishing-prerequisites"></a>Requisitos previos de publicación

Para publicar un módulo IoT Edge en Azure Marketplace, es preciso cumplir los siguientes requisitos previos:

<!-- P2: It would be great to point to the terms of use of CPP here. This can often be a blocker for big companies and these terms of use are not anonymously visible yet.-->
- Acceso a [Cloud Partner Portal](https://cloudpartner.azure.com/). Para obtener más información, vea [Guía de publicación de Azure Marketplace y AppSource](https://docs.microsoft.com/azure/marketplace/marketplace-publishers-guide).
- Aceptación de los [Términos de Microsoft Azure Marketplace](https://azure.microsoft.com/support/legal/marketplace-terms/)
- Hospedaje de los recursos técnicos del módulo IoT Edge en Azure Container Registry.  Para obtener más información, vea [Preparar los recursos técnicos del módulo IoT Edge](./cpp-create-technical-assets.md)
- Metadatos del módulo IoT Edge listos para usar. Por ejemplo, prepare los recursos siguientes:
    - Un título
    - Una descripción (en formato HTML)
    - Una imagen de logotipo (formato PNG y tamaños de imagen fijos, como 40 x 40 px, 90 x 90 px, 115 x 115 px, 255 x 115 px)
    - Un término de uso y directiva de privacidad
    - Una configuración predeterminada del módulo que incluya: rutas, propiedades deseadas gemelas, createOptions y variables de entorno.
    - Documentación del módulo
    - Contactos de soporte técnico


## <a name="next-steps"></a>Pasos siguientes

Una vez que tenga [preparado el recurso técnico de módulo IoT Edge](./cpp-create-technical-assets.md), estará listo para [crear su oferta de módulo IoT Edge](./cpp-create-offer.md). 
