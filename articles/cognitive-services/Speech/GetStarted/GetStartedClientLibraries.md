---
title: Introducción a Microsoft Speech Recognition API con bibliotecas cliente | Microsoft Docs
description: Utilice las bibliotecas cliente del servicio de voz de Microsoft en Microsoft Cognitive Services para desarrollar aplicaciones que conviertan el audio hablado en texto.
services: cognitive-services
author: zhouwangzw
manager: wolfma
ms.service: cognitive-services
ms.component: bing-speech
ms.topic: article
ms.date: 09/15/2017
ms.author: zhouwang
ms.openlocfilehash: 5abe5bc48c2bd73d0facf33e41a8076df2972153
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/23/2018
ms.locfileid: "35380543"
---
# <a name="get-started-with-speech-service-client-libraries"></a>Introducción a las bibliotecas cliente del servicio de voz

Además de realizar solicitudes HTTP directas a través de una API de REST, el servicio de voz proporciona a los desarrolladores bibliotecas cliente de voz en distintos idiomas. Bibliotecas cliente de voz:

- Son compatibles con las funciones más avanzadas de reconocimiento de voz, como los resultados intermedios en tiempo real, el flujo de audio de larga duración (hasta 10 minutos) y el reconocimiento continuado.
- Ofrecen una API simple e idiomática en el idioma de su preferencia.
- Ocultan los detalles de comunicaciones de nivel inferior.

Actualmente, están disponibles las siguientes bibliotecas cliente de voz:

- [Biblioteca de escritorio de C#](GetStartedCSharpDesktop.md)
- [Biblioteca de servicios C#](GetStartedCSharpServiceLibrary.md)
- [Biblioteca de JavaScript](GetStartedJSWebsockets.md)
- [Biblioteca de Java para Android](GetStartedJavaAndroid.md)
- [Biblioteca de Objective-C para iOS](Get-Started-ObjectiveC-iOS.md)

## <a name="additional-resources"></a>Recursos adicionales

- La página de [ejemplos](../samples.md) proporciona ejemplos completos para utilizar las bibliotecas cliente de voz.
- Si necesita una biblioteca cliente que aún no es compatible, puede crear su propio SDK. Implemente el [protocolo WebSocket de voz](../API-Reference-REST/websocketprotocol.md) en la plataforma y use el idioma de su elección.

## <a name="license"></a>Licencia

Todos los SDK de Cognitive Services y los ejemplos tienen una licencia de MIT. Para obtener más información, vea [Licencia](https://github.com/Microsoft/Cognitive-Speech-STT-JavaScript/blob/master/LICENSE.md).
