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
ms.openlocfilehash: 60ff2f71766a14af17ebb1cb9d20825976471296
ms.sourcegitcommit: 1aedb52f221fb2a6e7ad0b0930b4c74db354a569
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/17/2018
ms.locfileid: "41929858"
---
# <a name="what-is-the-speech-service-preview"></a>¿Qué es el servicio Voz (versión preliminar)?

Con una suscripción, el servicio Voz brinda a los desarrolladores un sencillo mecanismo para proporcionar a las aplicaciones eficaces características de voz. Ahora, las aplicaciones pueden incluir comandos de voz, transcripción, dictado, síntesis y traducción.

El servicio Voz funciona con las tecnologías que se usan en otros productos de Microsoft, incluidos Cortana y Microsoft Office.

> [!NOTE]
> El servicio Voz está actualmente en versión preliminar pública. Vuelva aquí con regularidad para obtener actualizaciones de la documentación, ejemplos de código adicionales y mucho más.

## <a name="speech-service-features"></a>Características del servicio Voz

|Función|DESCRIPCIÓN|
|-|-|
|[Voz a texto](speech-to-text.md)| Transcribe secuencias de audio en texto que la aplicación puede aceptar como entrada. Se puede integrar también con el [servicio Language Understanding](https://docs.microsoft.com/azure/cognitive-services/luis/) (LUIS) para extraer las intenciones del usuario a partir de expresiones.|
|[Texto a voz](text-to-speech.md)| Convierte texto sin formato a una voz que parece natural, que se entrega a la aplicación en un archivo de audio. Hay disponibles varias voces, con distinción de género o acentos, para varios idiomas admitidos. |
|[Traducción de voz](speech-translation.md)| Se puede usar para traducir el audio en streaming en tiempo casi real o para procesar la voz grabada. |
|Conversión de voz a texto personalizada|Puede personalizar la conversión de texto a voz creando sus propios modelos [acústicos](how-to-customize-acoustic-models.md) y de [idioma](how-to-customize-language-model.md) y especificando reglas de [pronunciación](how-to-customize-pronunciation.md) personalizadas. |
|[Conversión de texto a voz personalizada](how-to-customize-voice-font.md)|Puede crear sus propias voces para la conversión de texto a voz.|
|[Speech Devices SDK](speech-devices-sdk.md)| Con la introducción del servicio Voz unificado, Microsoft y sus asociados ahora ofrecen una plataforma integrada de hardware y software optimizada para el desarrollo de dispositivos habilitados para voz. |

## <a name="access-to-the-speech-service"></a>Acceso al servicio Voz

El servicio Voz se proporciona de dos maneras. [El SDK](speech-sdk.md) abstrae los detalles de los protocolos de red para facilitar el desarrollo en plataformas compatibles. La [API REST](rest-apis.md) funciona con cualquier lenguaje de programación, pero no dispone de todas las funciones que tiene el SDK.

|<br>Método|Voz<br>en texto|Texto a<br>Voz|Voz<br>Traducción|<br>DESCRIPCIÓN|
|-|-|-|-|-|
|[SDK](speech-sdk.md)|SÍ|Sin |SÍ|Bibliotecas de lenguajes de programación específicos; utilizan un protocolo basado en Websocket que simplifica el desarrollo.|
|[REST](rest-apis.md)|SÍ|SÍ|Sin |Una API sencilla basada en HTTP que facilita la incorporación de la voz a la aplicación.|

## <a name="speech-scenarios"></a>Escenarios de voz

A continuación, se describen algunos usos frecuente de la tecnología de voz. El [SDK de Voz](speech-sdk.md) es fundamental para la mayoría de estos escenarios.

> [!div class="checklist"]
> * Creación de aplicaciones que se activan por voz
> * Transcripción de las grabaciones de los centros de llamadas
> * Implementación de bots de voz

### <a name="voice-triggered-apps"></a>Aplicaciones que se activan por voz

La entrada de voz es un mecanismo excelente para hacer que la aplicación resulte flexible, sea rápida de usar y pueda utilizarse con la funcionalidad de manos libres. En una aplicación con la voz habilitada, los usuarios solo tienen que pedir la información que necesitan en lugar de navegar hasta ella.

Si la aplicación está diseñada para el público general, puede utilizar el modelo de reconocimiento de voz de referencia que proporciona el servicio Voz. Este servicio funciona bien y reconoce una gran variedad de hablantes en entornos comunes.

Si la aplicación se va a usar en un campo concreto, como medicina o TI, puede crear un [modelo de lenguaje](how-to-customize-language-model.md) para enseñar al servicio la terminología particular que utiliza la aplicación.

Si la aplicación se va a utilizar en un entorno con ruido, como una fábrica, puede crear un [modelo acústico](how-to-customize-acoustic-models.md) personalizado para que el servicio pueda distinguir mejor la voz del ruido.

Para empezar, solo tiene que descargar el [SDK de Voz](speech-sdk.md) y seguir la [guía de inicio rápido](quickstart-csharp-dotnet-windows.md) que corresponda.

### <a name="transcribe-call-center-recordings"></a>Transcripción de las grabaciones de los centros de llamadas

Normalmente, las grabaciones de los centros de llamadas solo se consultan si se produce algún problema con una llamada. Con el servicio Voz, resulta fácil transcribir todas las grabaciones. Una vez convertidas en texto, pueden indexarse fácilmente para realizar [búsquedas de texto completo](https://docs.microsoft.com/azure/search/search-what-is-azure-search) o aplicar [análisis de texto](https://docs.microsoft.com/azure/cognitive-services/Text-Analytics/) y detectar opiniones, idiomas y frases clave.

Si las grabaciones del centro de llamadas suelen contener terminología especializada (como nombres de producto o jerga de TI), puede crear un [modelo de lenguaje](how-to-customize-language-model.md) para enseñar al servicio Voz ese vocabulario. Los [modelos acústicos](how-to-customize-acoustic-models.md) personalizados pueden ayudar al servicio Voz a comprender las llamadas telefónicas que no tienen buena calidad.

Para más información acerca de este escenario, consulte documentación sobre [batch transcription](batch-transcription.md) (Transcripción en bloque) con el servicio Voz.///

### <a name="voice-bots"></a>Bots de voz

Los [bots](https://dev.botframework.com/) son un mecanismo cada vez más popular de conectar a los usuarios con la información que quieren y a los clientes con las empresas que les gustan. Si agrega una interfaz de usuario conversacional a una aplicación o a un sitio, será más fácil y rápido encontrar las funciones y acceder a ellas. Gracias al servicio Voz, la fluidez de la conversación alcanza una nueva dimensión, ya que, con la síntesis de voz, es posible responder a consultas habladas.

Para agregar una personalidad única al bot con voz (y reforzar su imagen de marca), puede utilizar una voz propia. El proceso de crear una voz personalizada se compone de dos pasos. En primer lugar, debe [realizar grabaciones](record-custom-voice-samples.md) de la voz que desee utilizar. Después, tiene que [enviar estas grabaciones](how-to-customize-voice-font.md) (junto con una transcripción de texto) al [portal de personalización de voz](https://cris.ai/Home/CustomVoice) del servicio, que se encargará de todo lo demás. Una vez que haya creado la voz personalizada, le resultará muy sencillo usarla en su aplicación.

## <a name="next-steps"></a>Pasos siguientes

Consiga una clave de suscripción del servicio Voz.

> [!div class="nextstepaction"]
> [Try the Speech service for free](get-started.md) (Prueba gratuita del servicio Voz)
