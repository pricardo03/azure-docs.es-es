---
title: Regiones del servicio Speech
description: Referencia de las regiones del servicio Voz.
services: cognitive-services
author: mahilleb-msft
ms.service: cognitive-services
ms.technology: speech
ms.topic: article
ms.date: 06/28/2018
ms.author: mahilleb
ms.openlocfilehash: 082002b25b02e1e496221f4686d0e636630dd438
ms.sourcegitcommit: 7ad9db3d5f5fd35cfaa9f0735e8c0187b9c32ab1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/27/2018
ms.locfileid: "39324398"
---
# <a name="regions-of-the-speech-service"></a>Regiones del servicio Voz

El servicio Voz está disponible en distintas regiones.
Al crear una suscripción, puede elegir una región disponible, según sus necesidades.

Al utilizar la suscripción, debe tener en cuenta la región que ha elegido.

## <a name="rest-api"></a>API DE REST

Con la API REST, elija los puntos de conexión específicos de la región.
Consulte las [API REST](rest-apis.md) para más información.

## <a name="speech-sdk"></a>SDK de voz

En las regiones de [SDK de Voz](speech-sdk.md), las regiones se especifican como una cadena (por ejemplo, como un parámetro [SpeechFactory.FromSubscription](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechfactory.fromsubscription) en el SDK de Voz para C#).

### <a name="regions-for-speech-recognition-and-translation"></a>Regiones con reconocimiento y traducción de voz

En la tabla siguiente se enumeran las regiones disponibles para el **reconocimiento de voz** y la **traducción**:

Region| Valor del parámetro de una región en el SDK de Voz
-|-
Oeste de EE. UU.| `westus`
Asia oriental| `eastasia`
Europa del Norte| `northeurope`

### <a name="regions-for-intent-recognition"></a>Regiones con reconocimiento de intenciones

Las regiones disponibles para el **reconocimiento de la intención** mediante el SDK de Voz se enumeran en la [página de la región del servicio Language Understanding](/azure/cognitive-services/luis/luis-reference-regions).
Para cada región de publicación enumerada, el parámetro de región del SDK de Voz correspondiente se determina como la primera parte del nombre de dominio del punto de conexión.
Por ejemplo, use `westus` para especificar la región de publicación Oeste de EE. UU.
