---
title: ¿Qué es el servicio Voz (versión preliminar)?
description: 'El servicio Voz, que forma parte de Microsoft Cognitive Services, agrupa varios servicios de voz de Azure que anteriormente estaban disponibles por separado: Bing Speech (que comprende el reconocimiento de voz y texto a voz), Custom Speech y Traducción de voz.'
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 05/07/2018
ms.author: v-jerkin
ms.openlocfilehash: 0cd6d984ac9329112aa388e8d8ee808d4c3e6227
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/02/2018
ms.locfileid: "39445641"
---
# <a name="what-is-the-speech-service-preview"></a>¿Qué es el servicio Voz (versión preliminar)?

El servicio Voz funciona con las tecnologías que se usan en otros productos de Microsoft, incluidos Cortana y Microsoft Office.  Este mismo servicio está disponible para cualquier cliente como servicio de Cognitive Services. 

> [!NOTE]
> El servicio Voz está actualmente en versión preliminar pública. Vuelva aquí con regularidad para obtener actualizaciones de la documentación, ejemplos de código adicionales y mucho más.

Con una suscripción, el servicio Voz ofrece a los desarrolladores una manera fácil de proporcionar a las aplicaciones características eficaces habilitadas para la voz. Ahora las aplicaciones pueden incluir comandos de voz, transcripción, dictado, síntesis de voz y traducción.

## <a name="speech-service-features"></a>Características del servicio Voz

|Función|DESCRIPCIÓN|
|-|-|
|[Voz a texto](speech-to-text.md)| Transcribe secuencias de audio en texto que la aplicación puede aceptar como entrada. Se puede integrar también con el [servicio Language Understanding](https://docs.microsoft.com/azure/cognitive-services/luis/) (LUIS) para extraer las intenciones del usuario a partir de expresiones.|
|[Texto a voz](text-to-speech.md)| Convierte texto sin formato a una voz que parece natural, que se entrega a la aplicación en un archivo de audio. Hay disponibles varias voces, con distinción de género o acentos, para varios idiomas admitidos. |
|[Traducción de voz](speech-translation.md)| Se puede usar para traducir el audio en streaming en tiempo casi real o para procesar la voz grabada. |
|[Speech Devices SDK](speech-devices-sdk.md)| Con la introducción del servicio Voz unificado, Microsoft y sus asociados ahora ofrecen una plataforma integrada de hardware y software optimizada para el desarrollo de dispositivos habilitados para voz. |

## <a name="using-the-speech-service"></a>Uso del servicio Voz

El servicio Voz se proporciona de dos maneras. [El SDK](speech-sdk.md) abstrae los detalles de los protocolos de red. La [API REST](rest-apis.md) funciona con cualquier lenguaje de programación, pero no ofrece todas las funciones que ofrece el SDK.

|<br>Método|Voz<br>en texto|Texto a<br>Voz|Voz<br>Traducción|<br>DESCRIPCIÓN|
|-|-|-|-|-|
|[SDK](speech-sdk.md)|SÍ|Sin |SÍ|Bibliotecas para lenguajes de programación concretos que simplifican el desarrollo.|
|[REST](rest-apis.md)|SÍ|SÍ|Sin |Una API sencilla basada en HTTP que facilita la incorporación de la voz a la aplicación.|

## <a name="next-steps"></a>Pasos siguientes

Obtengan una clave de suscripción gratuita para el servicio Voz.

> [!div class="nextstepaction"]
> [Try the Speech service for free](get-started.md) (Prueba gratuita del servicio Voz)
