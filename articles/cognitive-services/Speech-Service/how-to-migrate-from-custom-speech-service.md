---
title: Migración del servicio Custom Speech al servicio de voz
titleSuffix: Azure Cognitive Services
description: El servicio Custom Speech ahora forma parte del servicio de voz. Cambie al servicio de voz para aprovechar las actualizaciones más recientes de características y calidad.
services: cognitive-services
author: PanosPeriorellis
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 10/01/2018
ms.author: panosper
ms.custom: seodec18
ms.openlocfilehash: 9c93286329316d081f8fd99ebd360195931b7b09
ms.sourcegitcommit: 5aefc96fd34c141275af31874700edbb829436bb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/04/2019
ms.locfileid: "74805933"
---
# <a name="migrate-from-the-custom-speech-service-to-the-speech-service"></a>Migración del servicio Custom Speech al servicio de voz

Use este artículo para migrar las aplicaciones del servicio Custom Speech al servicio de voz.

El servicio Custom Speech ahora forma parte del servicio de voz. Cambie al servicio de voz para aprovechar las actualizaciones más recientes de características y calidad.

## <a name="migration-for-new-customers"></a>Migración para los nuevos clientes

El modelo de precios es más sencillo, al pasar a un modelo de precios por hora para el servicio de voz.  

1. Cree un recurso de Azure en cada región donde la aplicación esté disponible. El nombre del recurso de Azure es **Voz**. Puede usar un único recurso de Azure para los siguientes servicios en la misma región, en lugar de crear recursos independientes:

    * Voz a texto
    * Conversión de voz a texto personalizada
    * Texto a voz
    * Traducción de voz

2. Descargue el [SDK de Voz](speech-sdk.md).

3. Siga las guías de inicio rápido y los ejemplos del SDK para usar las API correctas. Si usa las API de REST, también debe usar los puntos de conexión y las claves de recursos correctos.

4. Actualice la aplicación cliente para usar las API y el servicio Voz.

## <a name="migration-for-existing-customers"></a>Migración para los clientes actuales

Migre sus claves de recursos existentes al servicio de voz en el portal de este servicio. Para ello, siga los pasos que se describen a continuación:

> [!NOTE]
> Las claves de recursos solo se pueden migrar en la misma región.

1. Inicie sesión en el portal [cris.ai](https://cris.ai/Home/CustomSpeech) y seleccione la suscripción en el menú superior derecho.

2. Seleccione **Migrate selected subscription** (Migrar suscripción seleccionada).

3. Escriba la clave de suscripción en el cuadro de texto y seleccione **Migrar**.

## <a name="next-steps"></a>Pasos siguientes

* [Prueba gratuita del servicio de voz](get-started.md).
* Obtenga información sobre los conceptos de [conversión de voz en texto](./speech-to-text.md).

## <a name="see-also"></a>Otras referencias

* [¿Qué es el servicio Voz?](overview.md)
* [Documentación del servicio de voz y del SDK de voz](speech-sdk.md#get-the-sdk)
