---
title: 'Regiones: servicios de Voz'
titlesuffix: Azure Cognitive Services
description: Referencia de las regiones del servicio Voz.
services: cognitive-services
author: mahilleb-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 03/12/2019
ms.author: panosper
ms.custom: seodec18
ms.openlocfilehash: eb3697eefb5835305b2ec80ca1e77ccade19e44f
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60480670"
---
# <a name="speech-service-supported-regions"></a>Regiones admitidas del servicio Voz

El servicio Voz permite que la aplicación convierta audio en texto, lleve a cabo la traducción de voz y convertir texto a voz. Este servicio está disponible en varias regiones con puntos de conexión únicos para SDK de Voz y API REST.

Asegúrese de usar el punto de conexión que coincida con la región de su suscripción.

## <a name="speech-sdk"></a>SDK de voz

En las regiones de [SDK de Voz](speech-sdk.md), las regiones se especifican como una cadena (por ejemplo, como un parámetro `SpeechConfig.FromSubscription` en el SDK de Voz para C#).

### <a name="speech-recognition-and-translation"></a>Reconocimiento de voz y traducción

El SDK de Voz está disponible en estas regiones para el **reconocimiento de voz** y la **traducción**:

  Region | Parámetro del SDK de Voz | Portal de personalización de Voz
 ------|-------|--------
 Oeste de EE. UU. | `westus` | https://westus.cris.ai
 Oeste de EE. UU. 2 | `westus2` | https://westus2.cris.ai
 Este de EE. UU | `eastus` | https://eastus.cris.ai
 Este de EE. UU. 2 | `eastus2` | https://eastus2.cris.ai
 Centro de EE. UU. | `centralus` | https://centralus.cris.ai
 Centro-Norte de EE. UU | `northcentralus` | https://northcentralus.cris.ai
 Centro-Sur de EE. UU | `southcentralus` | https://southcentralus.cris.ai
 India Central | `centralindia` | https://centralindia.cris.ai
 Asia oriental | `eastasia` | https://eastasia.cris.ai
 Sudeste de Asia | `southeastasia` | https://southeastasia.cris.ai
 Este de Japón | `japaneast` | https://japaneast.cris.ai
 Corea Central | `koreacentral` | https://koreacentral.cris.ai
 Este de Australia | `australiaeast` | https://australiaeast.cris.ai
 Centro de Canadá | `canadacentral` | https://canadacentral.cris.ai
 Europa del Norte | `northeurope` | https://northeurope.cris.ai
 Europa occidental | `westeurope` | https://westeurope.cris.ai
 Sur de Reino Unido 2 | `uksouth` | https://uksouth.cris.ai
 Centro de Francia | `francecentral` | https://francecentral.cris.ai

### <a name="intent-recognition"></a>Reconocimiento de la intención

Las regiones disponibles para el **reconocimiento de la intención** mediante el SDK de Voz son las siguientes:

 Región global | Region | Parámetro del SDK de Voz
 ------|-------|--------
 Asia | Asia oriental | `eastasia`
 Asia | Sudeste asiático | `southeastasia`
 Australia | Este de Australia | `australiaeast`
 Europa | Europa del Norte | `northeurope`
 Europa | Europa occidental | `westeurope`
 Norteamérica | Este de EE. UU | `eastus`
 Norteamérica | Este de EE. UU. 2 | `eastus2`
 Norteamérica | Centro-Sur de EE. UU | `southcentralus`
 Norteamérica | Centro occidental de EE.UU. | `westcentralus`
 Norteamérica | Oeste de EE. UU. | `westus`
 Norteamérica | Oeste de EE. UU. 2 | `westus2`
 Sudamérica | Sur de Brasil | `brazilsouth`

Se trata de un subconjunto de las regiones de publicación compatibles con el [servicio Language Understanding (LUIS)](/azure/cognitive-services/luis/luis-reference-regions).

## <a name="rest-apis"></a>API de REST

El servicio Voz también expone puntos de conexión REST para las solicitudes de voz a texto y texto a voz.

### <a name="speech-to-text"></a>Voz a texto

Para obtener documentación de referencia sobre las solicitudes de voz a texto, vea las [API REST](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis).

[!INCLUDE [](../../../includes/cognitive-services-speech-service-endpoints-speech-to-text.md)]

### <a name="text-to-speech"></a>Texto a voz

Para obtener documentación de referencia sobre las solicitudes de texto a voz, vea las [API REST](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis).

[!INCLUDE [](../../../includes/cognitive-services-speech-service-endpoints-text-to-speech.md)]
