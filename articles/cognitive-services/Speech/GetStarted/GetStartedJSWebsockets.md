---
title: Introducción a Bing Speech Recognition API en JavaScript | Microsoft Docs
titlesuffix: Azure Cognitive Services
description: Utilice Bing Speech Recognition API en Cognitive Services para desarrollar aplicaciones que conviertan continuamente el audio hablado en texto.
services: cognitive-services
author: zhouwangzw
manager: wolfma
ms.service: cognitive-services
ms.subservice: bing-speech
ms.topic: article
ms.date: 09/18/2018
ms.author: zhouwang
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 17901ad40a48e9ee8d1a8b872b04ad52b75b3a52
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60515214"
---
# <a name="get-started-with-the-speech-recognition-api-in-javascript"></a>Introducción a Speech Recognition API en JavaScript

[!INCLUDE [Deprecation note](../../../../includes/cognitive-services-bing-speech-api-deprecation-note.md)]

Puede desarrollar aplicaciones que conviertan el audio hablado en texto con Speech Recognition API. La biblioteca cliente de JavaScript usa el [protocolo WebSocket del servicio de voz](../API-Reference-REST/websocketprotocol.md), que permite hablar y recibir el texto transcrito simultáneamente. Este artículo sirve de ayuda para empezar a usar Speech Recognition API en JavaScript.

## <a name="prerequisites"></a>Requisitos previos

### <a name="subscribe-to-the-speech-recognition-api-and-get-a-free-trial-subscription-key"></a>Suscripción a Speech Recognition API y obtención de una clave de suscripción de prueba gratuita

Speech API forma parte de Cognitive Services. Puede obtener claves de suscripción de prueba gratuitas en la página de [suscripción a Cognitive Services](https://azure.microsoft.com/try/cognitive-services/). Después de seleccionar Speech API, seleccione **Obtener clave de API** para obtener la clave. Devuelve una clave principal y una clave secundaria. Las dos claves están asociadas a la misma cuota, por lo que puede usar cualquiera de las claves.

> [!IMPORTANT]
> Obtenga una clave suscripción. Para poder usar las bibliotecas cliente de Speech, debe tener una [clave de suscripción](https://azure.microsoft.com/try/cognitive-services/).

## <a name="get-started"></a>Introducción

En esta sección se le guiará por los pasos necesarios para cargar una página HTML de ejemplo. El ejemplo se encuentra en nuestro [repositorio de Github](https://github.com/Azure-Samples/SpeechToText-WebSockets-Javascript). Puede **abrir directamente el ejemplo** desde el repositorio o **abrir el ejemplo desde una copia local** del repositorio.

> [!NOTE]
> Algunos exploradores bloquean el acceso del micrófono de los orígenes no seguros. Por lo tanto, se recomienda hospedar "ejemplo"/"aplicación" en https para que funcione en todos los exploradores compatibles.

### <a name="open-the-sample-directly"></a>Abrir el ejemplo directamente

Adquiera una clave de suscripción, tal como se describe arriba. Luego abra el [vínculo al ejemplo](https://htmlpreview.github.io/? https://github.com/Azure-Samples/SpeechToText-WebSockets-Javascript/blob/preview/samples/browser/Sample.html). Esto cargará la página en el explorador predeterminado (representado mediante [htmlPreview](https://github.com/htmlpreview/htmlpreview.github.com)).

### <a name="open-the-sample-from-a-local-copy"></a>Abrir el ejemplo desde una copia local

Pruebe el ejemplo localmente, clone este repositorio:

```
git clone https://github.com/Azure-Samples/SpeechToText-WebSockets-Javascript
```

Compile los orígenes de TypeScript y agrúpelos en un único archivo JavaScript (es necesario que se haya instalado [npm](https://www.npmjs.com/) en el equipo). Vaya a la raíz del repositorio clonado y ejecute los comandos:

```
cd SpeechToText-WebSockets-Javascript && npm run bundle
```

Abra `samples\browser\Sample.html` en el explorador que prefiera.

## <a name="next-steps"></a>Pasos siguientes

Encontrará más información sobre cómo incluir el SDK en su propia página web [aquí](https://github.com/Azure-Samples/SpeechToText-WebSockets-Javascript).

## <a name="remarks"></a>Comentarios

- Speech Recognition API admite tres [modos de reconocimiento](../concepts.md#recognition-modes). Puede cambiar el modo actualizando la función **Setup()** que se encentra el archivo Sample.html. El ejemplo establece el modo en `Interactive` de forma predeterminada. Para cambiar el modo, actualice el parámetro `SR.RecognitionMode.Interactive` a otro modo. Por ejemplo, cambie el parámetro a `SR.RecognitionMode.Conversation`.
- Para obtener una lista completa de los exploradores compatibles, consulte la sección [Supported languages](../API-Reference-REST/supportedlanguages.md) (Idiomas admitidos).

## <a name="related-topics"></a>Temas relacionados

- [Repositorio de ejemplo de JavaScript Speech Recognition API](https://github.com/Azure-Samples/SpeechToText-WebSockets-Javascript)
- [Introducción a la API de REST](GetStartedREST.md)
