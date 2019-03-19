---
title: Introducción a Bing Speech Recognition API en Objective-C en iOS | Microsoft Docs
titlesuffix: Azure Cognitive Services
description: Use Bing Speech Recognition API para desarrollar aplicaciones iOS que conviertan el audio hablado en texto.
services: cognitive-services
author: zhouwangzw
manager: wolfma
ms.service: cognitive-services
ms.subservice: bing-speech
ms.topic: article
ms.date: 09/18/2018
ms.author: zhouwang
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 09b7e8961e59bd6fad49408c28e9ee9a4a209cae
ms.sourcegitcommit: 8ca6cbe08fa1ea3e5cdcd46c217cfdf17f7ca5a7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/22/2019
ms.locfileid: "56669439"
---
# <a name="quickstart-use-the-bing-speech-recognition-api-in-objective-c-on-ios"></a>Inicio rápido: Uso de Bing Speech Recognition API en Objective-C en iOS

[!INCLUDE [Deprecation note](../../../../includes/cognitive-services-bing-speech-api-deprecation-note.md)]

Con Speech Recognition API, puede desarrollar aplicaciones iOS que usen el servicio Voz basado en la nube para convertir el audio hablado en texto. La API admite el streaming en tiempo real, por lo que la aplicación puede recibir simultánea y asincrónicamente resultados parciales del reconocimiento al mismo tiempo que envía audio al servicio.

En este artículo se usa una aplicación de ejemplo para demostrar los conceptos básicos del trabajo con Speech Recognition API para desarrollar una aplicación iOS. Para obtener una referencia completa de la API, consulte [Referencia de la biblioteca cliente del SDK de Speech](https://cdn.rawgit.com/Microsoft/Cognitive-Speech-STT-iOS/master/com.Microsoft.SpeechSDK-1_0-for-iOS.docset/Contents/Resources/Documents/index.html).

## <a name="prerequisites"></a>Requisitos previos

### <a name="platform-requirements"></a>Requisitos de la plataforma

Asegúrese de que el IDE de Mac XCode está instalado.

### <a name="get-the-client-library-and-examples"></a>Obtención de la biblioteca cliente y ejemplos

La biblioteca cliente de Speech y los ejemplos para iOS están disponibles en el [SDK de cliente de Speech para iOS](https://github.com/microsoft/cognitive-speech-stt-ios).

### <a name="subscribe-to-the-speech-recognition-api-and-get-a-free-trial-subscription-key"></a>Suscripción a Speech Recognition API y obtención de una clave de suscripción de prueba gratuita

Speech API forma parte de Cognitive Services (anteriormente Project Oxford). Puede obtener las claves de la suscripción de prueba gratuita en la página [Suscripción a Cognitive Services](https://azure.microsoft.com/try/cognitive-services/). Después de seleccionar Speech API, seleccione **Obtener clave de API** para obtener la clave. Devuelve una clave principal y una clave secundaria. Las dos claves están asociadas a la misma cuota, por lo que puede usar cualquiera de las claves.

Si desea usar *reconocimiento con intenciones*, también debe registrarse en [Language Understanding Intelligent Service (LUIS)](https://azure.microsoft.com/services/cognitive-services/language-understanding-intelligent-service/).

> [!IMPORTANT]
> * Obtenga una clave suscripción. Para poder usar las bibliotecas cliente de Speech, debe tener una [clave de suscripción](https://azure.microsoft.com/try/cognitive-services/).
>
> * Use la clave de su suscripción. Con la aplicación de ejemplo para iOS proporcionada, debe actualizar el archivo Samples/SpeechRecognitionServerExample/settings.plist con su clave de suscripción. Para más información, consulte [Compilar y ejecutar ejemplos](#build-and-run-samples).

## <a name="use-the-speech-client-library"></a>Uso de la biblioteca cliente de Speech

Para agregar la biblioteca cliente a proyecto de XCode, siga estas [instrucciones](https://github.com/Azure-Samples/Cognitive-Speech-STT-iOS#the-client-library).

Para encontrar la referencia de la biblioteca cliente de iOS, consulte esta [página web](https://cdn.rawgit.com/Microsoft/Cognitive-Speech-STT-iOS/master/com.Microsoft.SpeechSDK-1_0-for-iOS.docset/Contents/Resources/Documents/index.html).

## <a name="build-and-run-samples"></a>Compilación y ejecución de ejemplos

Para obtener información sobre cómo compilar y ejecutar ejemplos, consulte la [página Léame](https://github.com/Azure-Samples/Cognitive-Speech-STT-iOS#the-sample).

## <a name="samples-explained"></a>Ejemplos explicados

### <a name="create-recognition-clients"></a>Creación de clientes de reconocimiento

El código del ejemplo siguiente muestra cómo crear clases de cliente de reconocimiento basadas en escenarios de usuario:

```
{
    NSString* language = @"en-us";

    NSString* path = [[NSBundle mainBundle] pathForResource:@"settings" ofType:@"plist"];
    NSDictionary* settings = [[NSDictionary alloc] initWithContentsOfFile:path];

    NSString* primaryOrSecondaryKey = [settings objectForKey:(@"primaryKey")];
    NSString* luisAppID = [settings objectForKey:(@"luisAppID")];
    NSString* luisSubscriptionID = [settings objectForKey:(@"luisSubscriptionID")];

    if (isMicrophoneReco) {
        if (!isIntent) {
            micClient = [SpeechRecognitionServiceFactory createMicrophoneClient:(recoMode)
                                                                   withLanguage:(language)
                                                                        withKey:(primaryOrSecondaryKey)
                                                                   withProtocol:(self)];
        }
        else {
            MicrophoneRecognitionClientWithIntent* micIntentClient;
            micIntentClient = [SpeechRecognitionServiceFactory createMicrophoneClientWithIntent:(language)
                                                                                        withKey:(primaryOrSecondaryKey)
                                                                                  withLUISAppID:(luisAppID)
                                                                                 withLUISSecret:(luisSubscriptionID)
                                                                                   withProtocol:(self)];
            micClient = micIntentClient;
        }
    }
    else {
        if (!isIntent) {
            dataClient = [SpeechRecognitionServiceFactory createDataClient:(recoMode)
                                                              withLanguage:(language)
                                                                   withKey:(primaryOrSecondaryKey)
                                                              withProtocol:(self)];
        }
        else {
            DataRecognitionClientWithIntent* dataIntentClient;
            dataIntentClient = [SpeechRecognitionServiceFactory createDataClientWithIntent:(language)
                                                                                   withKey:(primaryOrSecondaryKey)
                                                                             withLUISAppID:(luisAppID)
                                                                            withLUISSecret:(luisSubscriptionID)
                                                                              withProtocol:(self)];
            dataClient = dataIntentClient;
        }
    }
}

```

La biblioteca cliente proporciona clases de cliente de reconocimiento implementadas previamente para los escenarios típicos de reconocimiento de voz:

* `DataRecognitionClient`: reconocimiento de voz con datos PCM (por ejemplo, desde un origen de archivo o audio). Los datos se dividen en búferes y cada búfer se envía al servicio Voz. No se realiza ninguna modificación en los búferes, por lo que el usuario puede aplicar su propia detección de silencio, si lo desea. Si se proporcionan datos desde archivos WAV, puede enviar datos desde el archivo directamente al servidor. Si tiene datos sin procesar, por ejemplo, audio procedente de Bluetooth, en primer lugar envíe un encabezado de formato al servidor seguido por los datos.
* `MicrophoneRecognitionClient`: reconocimiento de voz con audio procedente del micrófono. Asegúrese de que el micrófono esté encendido y de que los datos del micrófono se envían al servicio de reconocimiento de voz. Se aplica una "latencia de silencio" integrada a los datos de micrófono antes de que se envíen al servicio de reconocimiento.
* `DataRecognitionClientWithIntent` y `MicrophoneRecognitionClientWithIntent`: además del texto de reconocimiento, estos clientes devuelven información estructurada sobre la intención del orador, que las aplicaciones pueden usar para controlar las acciones adicionales. Para usar la "intención", primero debe entrenar un modelo mediante [LUIS](https://azure.microsoft.com/services/cognitive-services/language-understanding-intelligent-service/).

### <a name="recognition-language"></a>Idioma de reconocimiento

Cuando se usa `SpeechRecognitionServiceFactory` para crear el cliente, debe seleccionar un idioma. Para obtener la lista completa de los idiomas admitidos por el servicio Voz, consulte [Supported languages](../API-Reference-REST/supportedlanguages.md) (Idiomas admitidos).

### <a name="speechrecognitionmode"></a>SpeechRecognitionMode

También debe especificar `SpeechRecognitionMode` cuando cree el cliente con `SpeechRecognitionServiceFactory`:

* `SpeechRecognitionMode_ShortPhrase`: una expresión de hasta 15 segundos de duración. A medida que los datos se envían al servicio, el cliente recibe varios resultados parciales y un resultado final con n opciones mejores.
* `SpeechRecognitionMode_LongDictation`: una expresión de hasta dos minutos de duración. A medida que los datos se envían al servicio, el cliente recibe varios resultados parciales y varios resultados finales, en función de dónde identifica las pausas el servidor.

### <a name="attach-event-handlers"></a>Conexión de controladores de eventos

Puede conectar varios controladores de eventos al cliente que creó:

* **Eventos de resultados parciales**: este evento se llamará cada vez que el servicio de voz prediga lo que puede estar diciendo, incluso antes de que termine de hablar (si usa `MicrophoneRecognitionClient`) o termine de enviar datos (si usa `DataRecognitionClient`).
* **Eventos de error**: se llaman cuando el servicio detecta un error.
* **Eventos de intención**: se llaman en los clientes "WithIntent" (solo en el modo ShortPhrase) después de analizar el resultado final del reconocimiento en una intención JSON estructurada.
* **Eventos de resultados**:
  * En modo `SpeechRecognitionMode_ShortPhrase`, se llama a este evento y devuelve los n resultados mejores cuando termina de hablar.
  * En modo `SpeechRecognitionMode_LongDictation`, se llama al controlador de eventos varias veces, en función de dónde identifica el servicio las pausas en la frase.
  * **Para cada una de las n opciones mejores**, se devuelve un valor de confianza y unos cuantos formatos diferentes del texto reconocido. Para más información, consulte [Output format](../Concepts.md#output-format) (Formato de salida).

## <a name="related-topics"></a>Temas relacionados

* [Referencia a la biblioteca cliente de Java](https://cdn.rawgit.com/Microsoft/Cognitive-Speech-STT-iOS/master/com.Microsoft.SpeechSDK-1_0-for-iOS.docset/Contents/Resources/Documents/index.html)
* [Introducción al servicio de reconocimiento de voz de Microsoft y las intenciones en Java en Android](GetStartedJavaAndroid.md)
* [Introducción a Microsoft Speech API en JavaScript](GetStartedJSWebsockets.md)
* [Introducción a Microsoft Speech API mediante REST](GetStartedREST.md)
