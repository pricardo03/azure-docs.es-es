---
title: Migración de Custom Speech Service al servicio de Voz
titlesuffix: Azure Cognitive Services
description: Custom Speech Service ahora forma parte de los servicios de voz. Cambiar a los servicios de voz para beneficiarse de las últimas actualizaciones de calidad y la característica.
services: cognitive-services
author: PanosPeriorellis
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 10/01/2018
ms.author: panosper
ms.custom: seodec18
ms.openlocfilehash: 501555c68b7b5922bbf5a74d2a7ed5e548b900c5
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/18/2019
ms.locfileid: "57833544"
---
# <a name="migrate-from-the-custom-speech-service-to-the-speech-service"></a>Migrar de Custom Speech Service al servicio Voz

Use este artículo para obtener información sobre cómo migrar sus aplicaciones de Custom Speech Service al servicio Voz.

Custom Speech Service ahora forma parte del servicio Voz. Cambiar a los servicios de voz para beneficiarse de las últimas actualizaciones de calidad y la característica.

## <a name="migration-for-new-customers"></a>Migración para los nuevos clientes

El modelo de precios es más sencillo, al pasar a un modelo de precios por hora para el servicio Voz.  

1. Cree un recurso de Azure en cada región donde la aplicación esté disponible. El nombre del recurso de Azure es **Voz**. Puede usar un único recurso de Azure para los siguientes servicios en la misma región, en lugar de crear recursos independientes:

    * Voz a texto
    * Conversión de voz a texto personalizada
    * Texto a voz
    * Traducción de voz

2. Descargue el [SDK de Voz](speech-sdk.md).

3. Siga las guías de inicio rápido y los ejemplos del SDK para usar las API correctas. Si usa las API de REST, también debe usar los puntos de conexión y las claves de recursos correctos.

4. Actualizar la aplicación cliente para usar las API y servicios de voz.

## <a name="migration-for-existing-customers"></a>Migración para los clientes actuales

Migrar las claves de recursos existentes a los servicios de voz en el portal de servicios de voz. Para ello, siga los pasos que se describen a continuación:

> [!NOTE]
> Las claves de recursos solo se pueden migrar en la misma región.

1. Inicie sesión en el portal [cris.ai](https://www.cris.ai) y seleccione la suscripción en el menú superior derecho.

2. Seleccione **Migrate selected subscription** (Migrar suscripción seleccionada).

3. Escriba la clave de suscripción en el cuadro de texto y seleccione **Migrar**.

## <a name="next-steps"></a>Pasos siguientes

* [Pruebe los servicios de voz de forma gratuita](get-started.md).
* Obtenga información sobre los conceptos de [conversión de voz en texto](./speech-to-text.md).

## <a name="see-also"></a>Vea también

* [¿Qué es el servicio Voz?](overview.md)
* [Documentación de servicios de voz y Speech SDK](speech-sdk.md#get-the-sdk)
