---
title: 'Regiones: servicios de Voz'
titlesuffix: Azure Cognitive Services
description: Referencia de las regiones del servicio Voz.
services: cognitive-services
author: mahilleb-msft
manager: cgronlun
ms.service: cognitive-services
ms.component: speech-service
ms.topic: conceptual
ms.date: 12/06/2018
ms.author: mahilleb
ms.custom: seodec18
ms.openlocfilehash: d41213d72d40555d8dc5aeab76040fc556dae774
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/08/2018
ms.locfileid: "53091652"
---
# <a name="speech-service-supported-regions"></a>Regiones admitidas del servicio Voz

El servicio Voz permite que la aplicación convierta audio en texto, lleve a cabo la traducción de voz y convertir texto a voz. Este servicio está disponible en varias regiones con puntos de conexión únicos para SDK de Voz y API REST.

Asegúrese de usar el punto de conexión que coincida con la región de su suscripción.

## <a name="speech-sdk"></a>SDK de voz

En el [SDK del servicio Voz](speech-sdk.md), las regiones se especifican como una cadena (por ejemplo, como un parámetro `SpeechConfig.FromSubscription` en el SDK de Voz para C#).

### <a name="speech-recognition-and-translation"></a>Reconocimiento de voz y traducción

El SDK de Voz está disponible en estas regiones para el **reconocimiento de voz** y la **traducción**:

  Region | Parámetro del SDK de Voz | Portal de personalización de Voz
 ------|-------|--------
 Oeste de EE. UU. | `westus` | https://westus.cris.ai
 Oeste de EE. UU. 2 | `westus2` | https://westus2.cris.ai
 Este de EE. UU | `eastus` | https://eastus.cris.ai
 Este de EE. UU. 2 | `eastus2` | https://eastus2.cris.ai
 Asia oriental | `eastasia` | https://eastasia.cris.ai
 Sudeste de Asia | `southeastasia` | https://southeastasia.cris.ai
 Europa del Norte | `northeurope` | https://northeurope.cris.ai
 Europa occidental | `westeurope` | https://westeurope.cris.ai


### <a name="intent-recognition"></a>Reconocimiento de la intención

**Reconocimiento de la intención** para el SDK de Voz comparte las regiones admitidas con LUIS. Para obtener una lista completa de las regiones disponibles, consulte [Publicación de región y puntos de conexión: LUIS](https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions).

Las regiones disponibles para el **reconocimiento de la intención** mediante el SDK de Voz se enumeran en la [página de la región del servicio Language Understanding](/azure/cognitive-services/luis/luis-reference-regions).

Para cada región de publicación que se enumere, use el **nombre de la región de API** proporcionado. Por ejemplo, use `westus` para Oeste de EE. UU.

## <a name="rest-apis"></a>API de REST

El servicio Voz también expone puntos de conexión REST para las solicitudes de voz a texto y texto a voz.

### <a name="speech-to-text"></a>Voz a texto

Para obtener documentación de referencia sobre las solicitudes de voz a texto, vea las [API REST](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis#speech-to-text).

[!INCLUDE [](../../../includes/cognitive-services-speech-service-endpoints-speech-to-text.md)]

### <a name="text-to-speech"></a>Texto a voz

Para obtener documentación de referencia sobre las solicitudes de texto a voz, vea las [API REST](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis#speech-to-text).

[!INCLUDE [](../../../includes/cognitive-services-speech-service-endpoints-text-to-speech.md)]
