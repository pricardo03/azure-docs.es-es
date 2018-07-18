---
title: Acerca de Text to Speech | Microsoft Docs
description: Introducción a las funcionalidades de Text to Speech.
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
manager: noellelacharite
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 05/07/2018
ms.author: v-jerkin
ms.openlocfilehash: d7ec8648a8428558264c9bfd4d923523b90cce07
ms.sourcegitcommit: 0b4da003fc0063c6232f795d6b67fa8101695b61
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37855947"
---
# <a name="about-the-text-to-speech-api"></a>Acerca de Text to Speech API

**Text to Speech** (TTS) API del servicio Voz convierte la entrada de texto en voz natural (también denominada *síntesis de voz*).

Para generar voz, la aplicación envía solicitudes HTTP POST al servicio Voz. Allí, el texto se sintetiza como voz natural y se devuelve como archivo de audio. Se admite una variedad de voces e idiomas.

Entre los escenarios en los que se está adoptando la síntesis de voz se incluyen:

* *Mejora de la accesibilidad:* la tecnología de **Text to Speech** permite a los publicadores y propietarios de contenido responder a las distintas maneras en que los usuarios interactúan con el contenido. Las personas con discapacidad visual o problemas de lectura aprecian poder consumir contenido oralmente. La salida de voz también hace que los usuarios disfruten más fácilmente del contenido textual, como periódicos o blogs, en dispositivos móviles mientras viajan o hacen ejercicio.

* *Respuesta en escenarios de multitarea:* **Text to Speech** permite a los usuarios absorber información importante rápida y cómodamente mientras conducen o, en caso contrario, fuera de un entorno de lectura adecuado. La navegación es una aplicación común de esta área. 

* *Mejora del aprendizaje con varios modos:* los diversos usuaria¡os aprenden mejor de maneras diferentes. Los expertos en aprendizaje en línea han demostrado que proporcionar voz y texto a la vez puede ayudar a aprender y retener información más fácilmente.

* *Entrega de asistentes o bots intuitivos:* la capacidad de hablar puede ser una parte fundamental de un asistente virtual o un bot de chat inteligente. Cada vez más empresas desarrollan bots de chat para proporcionar experiencias de servicio de atención al cliente atractivas. Voz agrega otra dimensión al permitir que las respuestas del bot se reciban de forma oral (por ejemplo, por teléfono).

## <a name="voice-support"></a>Compatibilidad con Voz

El servicio de Microsoft **Text-to-Speech** ofrece más de 75 voces en más de 45 idiomas y configuraciones regionales. Para utilizar estas "fuentes de voz" estándar, basta con especificar el nombre de voz con algunos otros parámetros al llamar a la API REST del servicio. Para obtener más información acerca de las voces admitidas, consulte [Supported languages](https://docs.microsoft.com/en-us/azure/cognitive-services/speech-service/supported-languages#text-to-speech) (Idiomas admitidos). 

Si desea una voz única para la aplicación, puede crear [fuentes de voz personalizadas](how-to-customize-voice-font.md) desde sus propios ejemplos de voz.

## <a name="next-steps"></a>Pasos siguientes

* [Obtenga su suscripción de prueba a Voz](https://azure.microsoft.com/try/cognitive-services/)
* [Vea cómo funciona el reconocimiento de voz en C#](quickstart-csharp-windows.md)
