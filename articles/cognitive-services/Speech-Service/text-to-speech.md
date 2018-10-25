---
title: 'Acerca de Text to Speech: servicio Voz'
titleSuffix: Azure Cognitive Services
description: El servicio de Microsoft Text-to-Speech ofrece más de 75 voces en más de 45 idiomas y configuraciones regionales. Para utilizar estas fuentes de voz estándar, basta con especificar el nombre de voz con algunos otros parámetros al llamar al servicio Voz.
services: cognitive-services
author: erhopf
ms.service: cognitive-services
ms.component: speech-service
ms.topic: conceptual
ms.date: 10/16/2018
ms.author: erhopf
ms.openlocfilehash: 2f79ecbb42ba6453c7fb615114c0b7e56402ada3
ms.sourcegitcommit: 6361a3d20ac1b902d22119b640909c3a002185b3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2018
ms.locfileid: "49363070"
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

El servicio de Microsoft **Text-to-Speech** ofrece más de 75 voces en más de 45 idiomas y configuraciones regionales. Para utilizar estas "fuentes de voz" estándar, basta con especificar el nombre de voz con algunos otros parámetros al llamar a la API REST del servicio. Para obtener más información acerca de las voces admitidas, consulte [Supported languages](language-support.md#text-to-speech) (Idiomas admitidos).

Si desea una voz única para la aplicación, puede crear [fuentes de voz personalizadas](how-to-customize-voice-font.md) desde sus propios ejemplos de voz.

## <a name="api-capabilities"></a>Funcionalidades de la API

Muchas de estas funcionalidades de **Speech to Text** API, especialmente en relación con la personalización, están disponibles a través de REST. En la tabla siguiente se resumen las funcionalidades de cada método de acceso a la API. Para obtener una lista completa de las funcionalidades y los detalles de la API, consulte la [referencia de Swagger](https://westus.cris.ai/swagger/ui/index).

| Caso de uso | REST | SDK |
|-----|-----|-----|----|
| Carga de conjuntos de datos para la adaptación de voces | SÍ | Sin  |
| Creación y administración de modelos de fuentes de voz | SÍ | Sin  |
| Creación y administración de implementaciones de fuentes de voz | SÍ | Sin  |
| Creación y administración de pruebas de fuentes de voz| SÍ | Sin  |
| Administrar suscripciones | SÍ | Sin  |

> [!NOTE]
> La API implementa una medida que limita las solicitudes de API a 25 por cada 5 segundos. Los encabezados de los mensajes le informarán sobre los límites.

## <a name="next-steps"></a>Pasos siguientes

* [Obtenga su suscripción de prueba a Voz](https://azure.microsoft.com/try/cognitive-services/)
* [Conozca cómo sintetizar voz con la API REST](how-to-text-to-speech.md)
