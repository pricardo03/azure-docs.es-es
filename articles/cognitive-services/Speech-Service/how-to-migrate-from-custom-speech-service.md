---
title: Migración de Custom Speech Service al servicio de Voz
titlesuffix: Azure Cognitive Services
description: Custom Speech Service ahora forma parte del servicio Voz. Cambie al servicio Voz para aprovechar las actualizaciones más recientes de calidad y características.
services: cognitive-services
author: PanosPeriorellis
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 10/01/2018
ms.author: panosper
ms.custom: seodec18
ms.openlocfilehash: 698962aa0e3d72b204c4e990aa1384b44bf3896f
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/07/2019
ms.locfileid: "55856904"
---
# <a name="migrate-from-the-custom-speech-service-to-the-speech-service"></a>Migrar de Custom Speech Service al servicio Voz

Use este artículo para obtener información sobre cómo migrar sus aplicaciones de Custom Speech Service al servicio Voz.

Custom Speech Service ahora forma parte del servicio Voz. Cambie al servicio Voz para aprovechar las actualizaciones más recientes de calidad y características.

## <a name="migration-for-new-customers"></a>Migración para los nuevos clientes

El modelo de precios es más sencillo, al pasar a un modelo de precios por hora para el servicio Voz.  

1. Cree un recurso de Azure en cada región donde la aplicación esté disponible. El nombre del recurso de Azure es **Voz**. Puede usar un único recurso de Azure para los siguientes servicios en la misma región, en lugar de crear recursos independientes:

    * Voz a texto
    * Conversión de voz a texto personalizada
    * Texto a voz
    * Traducción de voz

2. Descargue el [SDK de Voz](speech-sdk.md).

3. Siga las guías de inicio rápido y los ejemplos del SDK para usar las API correctas. Si usa las API de REST, también debe usar los puntos de conexión y las claves de recursos correctos.

4. Actualice la aplicación cliente para usar las API y el servicio Voz.

## <a name="migration-for-existing-customers"></a>Migración para los clientes actuales

Migre sus claves de recursos existentes con el servicio Voz en el portal del servicio Voz. Para ello, siga los pasos que se describen a continuación:

> [!NOTE]
> Las claves de recursos solo se pueden migrar en la misma región.

1. Inicie sesión en el portal [cris.ai](http://www.cris.ai) y seleccione la suscripción en el menú superior derecho.

2. Seleccione **Migrate selected subscription** (Migrar suscripción seleccionada).

3. Escriba la clave de suscripción en el cuadro de texto y seleccione **Migrar**.

## <a name="next-steps"></a>Pasos siguientes

* [Prueba gratuita del servicio Voz](get-started.md).
* Obtenga información sobre los conceptos de [conversión de voz en texto](./speech-to-text.md).

## <a name="see-also"></a>Otras referencias

* [¿Qué es el servicio Voz?](overview.md)
* [Documentación del servicio Voz y del SDK correspondiente](speech-sdk.md#get-the-sdk)
