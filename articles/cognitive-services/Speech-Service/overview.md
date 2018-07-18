---
title: ¿Qué es el servicio Voz (versión preliminar)? | Microsoft Docs
description: 'El servicio Voz, que forma parte de Microsoft Cognitive Services, agrupa varios servicios de voz de Azure que anteriormente estaban disponibles por separado: Bing Speech (que comprende el reconocimiento de voz y texto a voz), Custom Speech y Traducción de voz.'
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
manager: noellelacharite
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 05/07/2018
ms.author: v-jerkin
ms.openlocfilehash: e7c09eee1634c52e78a523a7cc65641ea99f23e6
ms.sourcegitcommit: b7290b2cede85db346bb88fe3a5b3b316620808d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/05/2018
ms.locfileid: "35383331"
---
# <a name="what-is-the-speech-service-preview"></a>¿Qué es el servicio Voz (versión preliminar)?

El servicio Voz, que forma parte de Microsoft Cognitive Services, agrupa varios servicios de voz de Azure que anteriormente estaban disponibles por separado: Bing Speech (que comprende el reconocimiento de voz y texto a voz), Custom Speech y Traducción de voz. Al igual que sus precursores, el servicio Voz funciona con las tecnologías que se usan en otros productos de Microsoft, incluidos Cortana y Microsoft Office.

> [!NOTE]
> El servicio Voz está actualmente en versión preliminar pública. Vuelva aquí con regularidad para obtener actualizaciones de la documentación, ejemplos de código adicionales y mucho más.

Con una suscripción, el servicio Voz unificado ofrece a los desarrolladores una manera fácil de proporcionar a las aplicaciones características eficaces habilitadas para la voz. Ahora las aplicaciones pueden incluir comandos de voz, transcripción, dictado, síntesis de voz y traducción.

|Función|DESCRIPCIÓN|
|-|-|
|Conversión de voz en texto|Convierte la voz humana continua en texto para que se pueda usar como entrada para la aplicación. Se puede integrar con el [servicio Language Understanding](https://docs.microsoft.com/azure/cognitive-services/luis/) (LUIS) para derivar la intención del usuario de las expresiones.|
|Texto a voz|Convierte el texto en archivos de audio con una voz sintetizada con un sonido natural.|
|Traducción&nbsp; de voz|Proporciona traducciones de voz a otros idiomas, con salida de texto o voz.|

## <a name="using-the-speech-service"></a>Uso del servicio Voz

El servicio Voz se proporciona de dos maneras. [El SDK](speech-sdk.md) abstrae los detalles de los protocolos de red. La [API REST](rest-apis.md) funciona con cualquier lenguaje de programación, pero no ofrece todas las funciones que ofrece el SDK.

|<br>Método|Voz<br>en texto|Texto a<br>Voz|Voz<br>Traducción|<br>DESCRIPCIÓN|
|-|-|-|-|-|
|[SDK](speech-sdk.md)|Sí|Sin |Sí|Bibliotecas para lenguajes de programación concretos que simplifican el desarrollo.|
|[REST](rest-apis.md)|Sí|Sí|Sin |Una API sencilla basada en HTTP que facilita la incorporación de la voz a la aplicación.|

## <a name="speech-to-text"></a>Conversión de voz en texto

La API de [Conversión de voz en texto](speech-to-text.md) (STT), o de reconocimiento de voz, transcribe las secuencias de audio en texto que la aplicación puede aceptar como entrada. La aplicación puede, por ejemplo, escribir el texto en un documento o actuar sobre él como un comando.

Conversión de voz en texto se ha optimizado por separado para escenarios interactivos, de conversación y de dictado. Los siguientes son los casos de uso comunes para la API de conversión de voz en texto. 

* Reconocer una expresión breve, por ejemplo, un comando, sin resultados provisionales.
* Transcribir una expresión larga, grabada previamente, como un mensaje de correo de voz.
* Transcribir voz en streaming en tiempo real, con resultados parciales, para dictado.
* Determinar qué quieren hacer los usuarios en función de una solicitud de lenguaje natural hablada.

La API de conversión de voz en texto admite la transcripción de voz interactiva con la conversión continua en tiempo real y resultados provisionales. También admite la detección de fin de voz, la capitalización y puntuación automática opcional, el enmascaramiento de palabras soeces y la normalización de texto.

Se pueden personalizar modelos de lenguaje y acústicos de Conversión de voz en texto para acomodar vocabulario especializado, entornos ruidosos y diferentes formas de hablar.

## <a name="text-to-speech"></a>Texto a voz

La API [Text to Speech](text-to-speech.md) (TTS), o síntesis de voz, convierte texto sin formato a voz que parezca natural, que se entrega a la aplicación en un archivo de audio. Hay disponibles varias voces, con distinción de género o acentos, para varios idiomas admitidos.

La API admite las etiquetas del Lenguaje de marcado de síntesis de voz (SSML), para que pueda especificar la pronunciación fonética exacta de palabras problemáticas. SSML también puede indicar características de la voz (como énfasis, velocidad, volumen, género y tono) directamente en el texto.

Lo siguiente son casos de uso comunes para Text to Speech API.

* Salida de voz como una salida de pantalla alternativa para usuarios con discapacidad visual.
* Avisos por voz para aplicaciones de coche, como navegación.
* Interfaces de usuario conversacionales junto con la API de conversión de voz en texto.

Si necesita un dialecto no admitido o simplemente quiere una voz única para la aplicación, Text to Speech API admite modelos de voz personalizados.

## <a name="speech-translation"></a>Traducción de voz

[Speech Translation](speech-translation.md) API se puede usar para traducir el audio en streaming en tiempo casi real o procesar la voz grabada. Con la traducción en streaming, el servicio devuelve resultados provisionales que se pueden mostrar al usuario para indicar el progreso de la traducción. Los resultados se pueden devolver como texto o como voz.

Los casos de uso de Traducción de voz son los siguientes.

* Implementar una aplicación móvil de traducción "conversacional" o un dispositivo para personas que viajan. 
* Proporcionar traducciones automáticas para subtítulos de grabaciones de audio y vídeo.

## <a name="speech-devices-sdk"></a>SDK de dispositivos de voz

Con la introducción del servicio Voz unificado, Microsoft y sus asociados ahora ofrecen una plataforma integrada de hardware y software optimizada para el desarrollo de dispositivos habilitados para voz: el [SDK de dispositivos de voz](speech-devices-sdk.md). Este SDK es adecuado para el desarrollo de dispositivos inteligentes de voz para todos los tipos de aplicaciones.

El SDK de dispositivos de voz permite crear dispositivos ambientales propios con una palabra de reactivación personalizada, de modo que la indicación que inicia la captura de audio sea única para su marca. También proporciona un procesamiento de audio superior a partir de orígenes multicanal para un reconocimiento de voz más preciso, incluso la supresión de ruido, voz de campo lejano y formación de haz.

## <a name="next-steps"></a>Pasos siguientes

Obtengan una clave de suscripción gratuita para el servicio Voz.

> [!div class="nextstepaction"]
> [Try the Speech service for free](get-started.md) (Prueba gratuita del servicio Voz)
