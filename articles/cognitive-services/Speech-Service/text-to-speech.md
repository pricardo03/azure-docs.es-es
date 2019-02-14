---
title: 'Acerca de Text to Speech: servicio Voz'
titleSuffix: Azure Cognitive Services
description: La API de Text-to-Speech ofrece más de 75 voces en más de 45 idiomas y configuraciones regionales. Para utilizar estas fuentes de voz estándar, basta con especificar el nombre de voz con algunos otros parámetros al llamar al servicio Voz.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 12/13/2018
ms.author: erhopf
ms.custom: seodec18
ms.openlocfilehash: 0836ae4a9041db27cfed35dd0f1fc0df6e541aff
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/07/2019
ms.locfileid: "55859341"
---
# <a name="about-the-text-to-speech-api"></a>Acerca de Text-to-Speech API

**Text to Speech** (TTS) API convierte la entrada de texto en voz natural (también denominada *síntesis de voz*).

Para generar la voz, la aplicación envía solicitudes HTTP POST a Text-to-Speech API. Allí, el texto se sintetiza como voz natural y se devuelve como archivo de audio. Se admite una variedad de voces e idiomas.

Entre los escenarios en los que se está adoptando la síntesis de voz se incluyen:

* *Mejora de la accesibilidad:* la tecnología de **Text to Speech** permite a los publicadores y propietarios de contenido responder a las distintas maneras en que los usuarios interactúan con el contenido. Las personas con discapacidad visual o problemas de lectura aprecian poder consumir contenido oralmente. La salida de voz también hace que los usuarios disfruten más fácilmente del contenido textual, como periódicos o blogs, en dispositivos móviles mientras viajan o hacen ejercicio.

* *Respuesta en escenarios de multitarea:* **Text to Speech** permite a los usuarios absorber información importante de forma rápida y cómoda mientras conducen o en cualquier otro entorno de lectura que no sea adecuado. La navegación es una aplicación común de esta área.

* *Mejorar del aprendizaje con varios modos:* cada persona aprende mejor de una manera diferente. Los expertos en aprendizaje en línea han demostrado que proporcionar voz y texto a la vez puede ayudar a aprender y retener información más fácilmente.

* *Uso de bots o asistentes intuitivos:* la capacidad de hablar puede ser una parte fundamental de un asistente virtual o un bot de chat inteligente. Cada vez más empresas desarrollan bots de chat para proporcionar experiencias de servicio de atención al cliente atractivas. Voz agrega otra dimensión al permitir que las respuestas del bot se reciban de forma oral (por ejemplo, por teléfono).

## <a name="voice-support"></a>Compatibilidad con Voz

El servicio de Microsoft **Text-to-Speech** ofrece más de 75 voces en más de 45 idiomas y configuraciones regionales. Para utilizar estas "fuentes de voz" estándar, basta con especificar el nombre de voz con algunos otros parámetros al llamar a la API REST del servicio. Para más información acerca de los idiomas, las configuraciones regionales y las voces compatibles, consulte los [idiomas que se admiten](language-support.md#text-to-speech).

> [!IMPORTANT]
> Los costes varían para voces estándar, personalizadas y neuronales. Para obtener más información, consulte el apartado [Precios](https://azure.microsoft.com/pricing/details/cognitive-services/speech-services/).

### <a name="neural-voices"></a>Voces neuronales

Text to Speech neuronal se puede usar para que las interacciones con los bots de chat y los asistentes virtuales sean más naturales y atractivas, para convertir textos digitales, como los libros electrónicos, en audiolibros y para mejorar los sistemas de navegación de los automóviles. Con su prosodia natural similar a la humana y la clara articulación de las palabras, Text to Speech neuronal ha reducido considerablemente la fatiga de la escucha que aparece cuando se interactúa con sistemas de inteligencia artificial. Para más información acerca de las voces neuronales, consulte los [idiomas compatibles](language-support.md#text-to-speech).

### <a name="custom-voices"></a>Voces personalizadas

La personalización de voz de Text to Speech le permite crear una voz única y reconocible para su marca: una *fuente de voz*. Para crear su fuente de voz, haga que un estudio grabe y cargue los scripts asociados como datos de aprendizaje. A continuación, el servicio crea un modelo de voz único ajustado a la grabación. Dicha fuente de voz se puede usar para sintetizar la voz. Para obtener más información, consulte las [fuentes de voz personalizadas](how-to-customize-voice-font.md).

## <a name="api-capabilities"></a>Funcionalidades de la API

Muchas de las funcionalidades de **Speech to Text** API, sobre todo las relacionadas con la personalización, están disponibles a través de REST. En la tabla siguiente se resumen las funcionalidades de cada método de acceso a la API. Para obtener una lista completa de las funcionalidades y los detalles de la API, consulte la [referencia de Swagger](https://westus.cris.ai/swagger/ui/index).

| Caso de uso | REST | SDK |
|-----|-----|-----|----|
| Carga de conjuntos de datos para la adaptación de voces | Sí | Sin  |
| Creación y administración de modelos de fuentes de voz | Sí | Sin  |
| Creación y administración de implementaciones de fuentes de voz | Sí | Sin  |
| Creación y administración de pruebas de fuentes de voz| Sí | Sin  |
| Administrar suscripciones | Sí | Sin  |

> [!NOTE]
> La API implementa una medida que limita las solicitudes de API a 25 por cada 5 segundos. Los encabezados de los mensajes le informarán sobre los límites.

## <a name="next-steps"></a>Pasos siguientes

* [Obtención de una suscripción de gratuita a servicios de Voz](https://azure.microsoft.com/try/cognitive-services/)
* [Inicio rápido: Conversión de texto a voz con Python](quickstart-python-text-to-speech.md)
* [Inicio rápido: Conversión de texto a voz con .NET Core](quickstart-dotnet-text-to-speech.md)
* [Referencia de API de REST](rest-apis.md)
