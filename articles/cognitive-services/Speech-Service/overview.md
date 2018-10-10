---
title: ¿Qué es Speech Service?
description: 'El servicio Voz, que forma parte de Microsoft Cognitive Services, agrupa varios servicios de voz de Azure que anteriormente estaban disponibles por separado: Bing Speech (que comprende el reconocimiento de voz y texto a voz), Custom Speech y Traducción de voz.'
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: v-jerkin
ms.service: cognitive-services
ms.component: speech-service
ms.topic: overview
ms.date: 09/24/2018
ms.author: v-jerkin
ms.openlocfilehash: c12cd754d9979683fac4b819739d53e78ab9259c
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46990202"
---
# <a name="what-is-the-speech-service"></a>¿Qué es Speech Service?

Speech Service agrupa las funciones de voz de Azure que estaban disponibles a través de [Bing Speech API](https://docs.microsoft.com/azure/cognitive-services/speech/home), [Translator Speech](https://docs.microsoft.com/azure/cognitive-services/translator-speech/), [Custom Speech](https://docs.microsoft.com/azure/cognitive-services/custom-speech-service/cognitive-services-custom-speech-home) y los servicios de [Custom Voice](http://customvoice.ai/). Ahora, una sola suscripción proporciona acceso a todas estas funcionalidades.

Al igual que otros servicios de voz de Azure, el servicio Voz funciona con las tecnologías de voz usadas en productos como Cortana y Microsoft Office. Puede confiar en la calidad de los resultados y la confiabilidad de la plataforma en la nube.

## <a name="main-speech-service-functions"></a>Principales funciones de Speech Service

Las principales funciones de Speech Service son la conversión de voz en texto (también llamada reconocimiento de voz o transcripción), la conversión de texto a voz (síntesis de voz) y la traducción de voz.

|Función|Características|
|-|-|
|[Conversión de voz en texto](speech-to-text.md)| <ul><li>Permite transcribir en tiempo real voz en texto.<li>Puede transcribir por lotes voz a partir de grabaciones de audio. <li>Ofrece modos de reconocimiento para casos de uso interactivos, de conversación y dictado.<li>Admite resultados intermedios, detección de fin de voz, formato de texto automático y enmascaramiento de palabras soeces. <li>Puede llamar al servicio [Language Understanding](https://docs.microsoft.com/azure/cognitive-services/luis/) (LUIS) para extraer la intención del usuario de la transcripción de voz.\*|
|[Texto a voz](text-to-speech.md)| <ul><li>Convierte el texto a una voz que parece natural. <li>Ofrece varios géneros y dialectos para muchos idiomas compatibles. <li>Admite la entrada de texto sin formato o el lenguaje de marcado de síntesis de voz (SSML). |
|[Traducción de voz](speech-translation.md)| <ul><li>Traduce el audio en streaming casi en tiempo real.<li> También puede procesar voz grabada.<li>Puede proporcionar resultados como texto o como voz sintetizada. |

\* *El reconocimiento de intenciones requiere una suscripción a LUIS.*

## <a name="customize-speech-features"></a>Características de personalización de voz

Puede usar sus propios datos para entrenar los modelos subyacentes de las características de conversión de voz a texto y texto a voz del servicio Voz.

|Característica|Modelo|Propósito|
|-|-|-|
|Speech to Text|[Modelo acústico](how-to-customize-acoustic-models.md)|Ayuda a transcribir a oradores en entornos determinados como coches o fábricas.|
||[Modelo de lenguaje](how-to-customize-language-model.md)|Ayuda a transcribir un vocabulario y una gramática específicos de un sector, como la jerga especializada médica o de TI.|
||[Modelo de pronunciación](how-to-customize-pronunciation.md)|Ayuda a transcribir abreviaturas y acrónimos como, por ejemplo, "IOU" para "I owe you" (pagaré). |
|Texto a voz|[Fuente de voz](how-to-customize-voice-font.md)|Proporciona a su aplicación una voz propia mediante el entrenamiento del modelo basado en ejemplos de voz humana.|

Puede usar modelos personalizados en cualquier lugar en el que usaría los modelos estándares en la funcionalidad de conversión de voz en texto o texto a voz de la aplicación.

## <a name="use-the-speech-service"></a>Uso del servicio Voz

Para simplificar el desarrollo de aplicaciones habilitadas para voz, Microsoft proporciona [Speech SDK](speech-sdk.md) para su uso con el nuevo Speech Service. SDK de Voz proporciona API coherentes nativas de conversión de voz en texto y de traducción de voz para C#, C++ y Java. Si desarrolla en alguno de estos lenguajes, SDK de Voz facilita el desarrollo controlando la información de red en su lugar.

Speech Service también tiene una [API REST](rest-apis.md) que funciona con cualquier lenguaje de programación que pueda realizar solicitudes HTTP. La interfaz de REST no ofrece la funcionalidad de streaming en tiempo real del SDK.

|<br>Método|Voz<br>en texto|Texto a<br>Voz|Voz<br>Traducción|<br>DESCRIPCIÓN|
|-|-|-|-|-|
|[Speech SDK](speech-sdk.md)|SÍ|Sin |SÍ|API nativas para C#, C++ y Java para simplificar el desarrollo.|
|[REST](rest-apis.md)|SÍ|SÍ|Sin |Una API sencilla basada en HTTP que facilita la incorporación de la voz a la aplicación.|

### <a name="websockets"></a>Websocket

El servicio Voz también tiene protocolos WebSocket para hacer streaming de voz en texto y de traducción de voz. Speech SDK usa estos protocolos para comunicarse con Speech Service. Use el SDK de Voz en lugar de intentar implementar su propia comunicación de WebSockets con el servicio Voz.

Si ya tiene código que utiliza Bing Speech o Translator Speech a través de WebSockets, puede actualizarlo para que use el servicio Voz. Los protocolos WebSocket son compatibles; solo los puntos de conexión son diferentes.

### <a name="speech-devices-sdk"></a>SDK de dispositivos de voz

[Speech Devices SDK](speech-devices-sdk.md) es una plataforma integrada de hardware y software para desarrolladores de dispositivos habilitados para voz. Nuestros asociados de hardware proporcionan diseños de referencia y unidades de desarrollo. Microsoft ofrece un SDK optimizado para dispositivos que aprovecha plenamente las funcionalidades del hardware.

## <a name="speech-scenarios"></a>Escenarios de voz

Entre los casos de uso de Speech Service se incluyen:

> [!div class="checklist"]
> * Creación de aplicaciones que se activan por voz
> * Transcripción de las grabaciones de los centros de llamadas
> * Implementación de bots de voz

### <a name="voice-user-interface"></a>Interfaz de usuario de voz

La entrada de voz es un mecanismo excelente para hacer que la aplicación resulte flexible, sea rápida de usar y pueda utilizarse con la funcionalidad de manos libres. Con una aplicación con la voz habilitada, los usuarios solo tienen que pedir la información que necesitan.

Si la aplicación está diseñada para el público general, puede utilizar los modelos de reconocimiento de voz predeterminados. Estos reconocen una gran variedad de hablantes en entornos comunes.

Si se usa la aplicación en un dominio específico, por ejemplo, medicina o TI, puede crear un [modelo de lenguaje](how-to-customize-language-model.md). Puede usar este modelo para enseñar al servicio Voz la terminología especial que usa la aplicación.

Si se usa la aplicación en un entorno ruidoso como, por ejemplo, una fábrica, puede crear un [modelo acústico](how-to-customize-acoustic-models.md) personalizado. Este modelo ayuda al servicio Voz a distinguir la voz del ruido.

Comenzar es fácil. Simplemente descargue el [SDK de Voz](speech-sdk.md) y siga la [Guía de inicio rápido](quickstart-csharp-dotnet-windows.md) correspondiente.

### <a name="call-center-transcription"></a>Transcripción para los centros de llamadas

Normalmente, las grabaciones de los centros de llamadas solo se consultan si se produce algún problema con una llamada. Con el servicio Voz, resulta fácil transcribir todas las grabaciones. Puede indexar fácilmente el texto para realizar [búsquedas de texto completo](https://docs.microsoft.com/azure/search/search-what-is-azure-search) o aplicar [análisis de texto](https://docs.microsoft.com/azure/cognitive-services/Text-Analytics/) para detectar opiniones, idiomas y frases clave.

Si las grabaciones del centro de llamadas contienen terminología especializada (como nombres de producto o jerga de TI), puede crear un [modelo de lenguaje](how-to-customize-language-model.md) para enseñar al servicio Voz ese vocabulario. Los [modelos acústicos](how-to-customize-acoustic-models.md) personalizados pueden ayudar al servicio Voz a comprender las llamadas telefónicas que no tienen buena calidad.

Para más información acerca de este escenario, consulte documentación sobre [batch transcription](batch-transcription.md) (Transcripción en bloque) con el servicio Voz.

### <a name="voice-bots"></a>Bots de voz

Los [bots](https://dev.botframework.com/) son un mecanismo popular para conectar a los usuarios con la información que quieren y a los clientes con las empresas que les gustan. Si agrega una interfaz de usuario conversacional a una aplicación o a un sitio web, será más fácil y rápido encontrar las funciones y acceder a ellas. Con Speech Service, la fluidez de la conversación alcanza una nueva dimensión de influencia gracias a la capacidad para responder consultas habladas de Kind.

Para agregar una personalidad única al bot con voz, puede utilizar una voz propia. El proceso de crear una voz personalizada se compone de dos pasos. En primer lugar, debe [realizar grabaciones](record-custom-voice-samples.md) de la voz que desee utilizar. Después, tiene que [enviar estas grabaciones](how-to-customize-voice-font.md) (junto con una transcripción de texto) al [portal de personalización de voz](https://cris.ai/Home/CustomVoice) del servicio, que se encargará de todo lo demás. Después de crear su voz personalizada, los pasos para usarla en la aplicación son sencillos.

## <a name="next-steps"></a>Pasos siguientes

Consiga una clave de suscripción del servicio Voz.

> [!div class="nextstepaction"]
> [Try the Speech service for free](get-started.md) (Prueba gratuita del servicio Voz)
