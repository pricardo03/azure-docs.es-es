---
title: Introducción a Microsoft Speech Recognition API con bibliotecas de cliente de Bing Speech | Microsoft Docs
description: Utilice las bibliotecas cliente del servicio de voz de Microsoft en Microsoft Cognitive Services para desarrollar aplicaciones que conviertan el audio hablado en texto.
services: cognitive-services
author: zhouwangzw
manager: wolfma
ms.service: cognitive-services
ms.component: bing-speech
ms.topic: article
ms.date: 09/15/2017
ms.author: zhouwang
ms.openlocfilehash: f4b6a97260c6dc176600af8844001e4de819ff7c
ms.sourcegitcommit: fab878ff9aaf4efb3eaff6b7656184b0bafba13b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/22/2018
ms.locfileid: "42357517"
---
# <a name="get-started-with-bing-speech-service-client-libraries"></a>Introducción a las bibliotecas de cliente del servicio Bing Speech

Además de realizar solicitudes HTTP directas por medio de una API REST, el servicio Bing Speech proporciona a los desarrolladores bibliotecas de cliente de Voz en distintos idiomas. Bibliotecas cliente de voz:

- Son compatibles con las funciones más avanzadas de reconocimiento de voz, como los resultados intermedios en tiempo real, el flujo de audio de larga duración (hasta 10 minutos) y el reconocimiento continuado.
- Ofrecen una API simple e idiomática en el idioma de su preferencia.
- Ocultan los detalles de comunicaciones de nivel inferior.

Actualmente, están disponibles las siguientes bibliotecas de cliente de Bing Speech:

- [Biblioteca de escritorio de C#](GetStartedCSharpDesktop.md)
- [Biblioteca de servicios C#](GetStartedCSharpServiceLibrary.md)
- [Biblioteca de JavaScript](GetStartedJSWebsockets.md)
- [Biblioteca de Java para Android](GetStartedJavaAndroid.md)
- [Biblioteca de Objective-C para iOS](Get-Started-ObjectiveC-iOS.md)

> [!NOTE] 
En mayo de 2018, lanzamos también el nuevo [servicio de Voz](/speech-service/overview.md) en versión preliminar pública. Le animamos a que [lo pruebe gratuitamente](/speech-service/get-started.md). 

## <a name="additional-resources"></a>Recursos adicionales

- La página de [ejemplos](../samples.md) proporciona ejemplos completos para utilizar las bibliotecas cliente de voz.
- Si necesita una biblioteca cliente que aún no es compatible, puede crear su propio SDK. Implemente el [protocolo WebSocket de voz](../API-Reference-REST/websocketprotocol.md) en la plataforma y use el idioma de su elección.

## <a name="license"></a>Licencia

Todos los SDK de Cognitive Services y los ejemplos tienen una licencia de MIT. Para más información, consulte [Licencia](https://github.com/Microsoft/Cognitive-Speech-STT-JavaScript/blob/master/LICENSE.md).
