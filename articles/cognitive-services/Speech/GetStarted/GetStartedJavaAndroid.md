---
title: Introducción a Microsoft Speech Recognition API en Java en Android | Microsoft Docs
titlesuffix: Azure Cognitive Services
description: Use Microsoft Speech API para desarrollar aplicaciones Android que conviertan el audio hablado en texto.
services: cognitive-services
author: zhouwangzw
manager: wolfma
ms.service: cognitive-services
ms.component: bing-speech
ms.topic: article
ms.date: 09/18/2018
ms.author: zhouwang
ms.openlocfilehash: 4c5243ec14a4494222168bb33b3e840b96f8465e
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/16/2018
ms.locfileid: "49345261"
---
[!INCLUDE [Deprecation note](../../../../includes/cognitive-services-bing-speech-api-deprecation-note.md)]

# <a name="quickstart-use-the-bing-speech-recognition-api-in-java-on-android"></a>Guía de inicio rápido: Uso de Bing Speech Recognition API en Java en Android

Con Bing Speech Recognition API, puede desarrollar aplicaciones Android que usen el servicio Bing Speech basado en la nube para convertir el audio hablado en texto. La API admite el streaming en tiempo real, por lo que la aplicación puede recibir simultánea y asincrónicamente resultados parciales del reconocimiento al mismo tiempo que envía audio al servicio.

En este artículo se usa una aplicación de ejemplo para mostrar cómo utilizar la biblioteca cliente de voz para Android para desarrollar aplicaciones de conversión de voz en texto en Java para dispositivos Android.

## <a name="prerequisites"></a>Requisitos previos

### <a name="platform-requirements"></a>Requisitos de la plataforma

El ejemplo se ha desarrollado por [Android Studio](http://developer.android.com/sdk/index.html) para Windows en Java.

### <a name="get-the-client-library-and-sample-application"></a>Obtener la biblioteca cliente y la aplicación de ejemplo

La biblioteca cliente de voz y los ejemplos para Android están disponibles en el [SDK de cliente voz para Android](https://github.com/microsoft/cognitive-speech-stt-android). Puede encontrar el ejemplo compilable en el directorio samples/SpeechRecoExample. Puede encontrar las dos bibliotecas que debe usar en sus propias aplicaciones en SpeechSDK/libs, en armeabi y la carpeta x86. El tamaño del archivo libandroid_platform.so es de 22 MB, pero se reduce a 4 MB en tiempo de implementación.

#### <a name="subscribe-to-the-speech-api-and-get-a-free-trial-subscription-key"></a>Suscribirse a Speech API y obtener una clave de suscripción de prueba gratuita

Speech API forma parte de Cognitive Services (anteriormente Project Oxford). Puede obtener las claves de la suscripción de prueba gratuita en la página [Suscripción a Cognitive Services](https://azure.microsoft.com/try/cognitive-services/). Después de seleccionar Speech API, seleccione **Obtener clave de API** para obtener la clave. Devuelve una clave principal y una clave secundaria. Las dos claves están asociadas a la misma cuota, por lo que puede usar cualquiera de las claves.

Si desea usar *reconocimiento con intenciones*, también debe registrarse en [Language Understanding Intelligent Service (LUIS)](https://azure.microsoft.com/services/cognitive-services/language-understanding-intelligent-service/).

> [!IMPORTANT]
>* Obtenga una clave suscripción. Para poder usar las bibliotecas cliente de Speech, debe tener una [clave de suscripción](https://azure.microsoft.com/try/cognitive-services/).
>
>* Use la clave de su suscripción. Con la aplicación de ejemplo Android proporcionada, actualice el archivo samples/SpeechRecoExample/res/values/strings.xml con las claves de suscripción. Para más información, vea [Compilar y ejecutar ejemplos](#build-and-run-samples).

## <a name="use-the-speech-client-library"></a>Usar la biblioteca cliente de voz

Para usar la biblioteca cliente en la aplicación, siga las [instrucciones](https://github.com/microsoft/cognitive-speech-stt-android#the-client-library).

Puede encontrar la referencia de biblioteca cliente para Android en la carpeta docs del [SDK de cliente de voz para Android](https://github.com/microsoft/cognitive-speech-stt-android).

## <a name="build-and-run-samples"></a>Compilar y ejecutar ejemplos

Para obtener información sobre cómo compilar y ejecutar ejemplos, vea la [página Léame](https://github.com/microsoft/cognitive-speech-stt-android#the-sample).

## <a name="samples-explained"></a>Ejemplos explicados

### <a name="create-recognition-clients"></a>Crear clientes de reconocimiento

El código del ejemplo siguiente muestra cómo crear clases de cliente de reconocimiento basadas en escenarios de usuario:

```java
void initializeRecoClient()
    {
        String language = "en-us";

        String subscriptionKey = this.getString(R.string.subscription_key);
        String luisAppID = this.getString(R.string.luisAppID);
        String luisSubscriptionID = this.getString(R.string.luisSubscriptionID);

        if (m_isMicrophoneReco && null == m_micClient) {
            if (!m_isIntent) {
                m_micClient = SpeechRecognitionServiceFactory.createMicrophoneClient(this,
                                                                                     m_recoMode,
                                                                                     language,
                                                                                     this,
                                                                                     subscriptionKey);
            }
            else {
                MicrophoneRecognitionClientWithIntent intentMicClient;
                intentMicClient = SpeechRecognitionServiceFactory.createMicrophoneClientWithIntent(this,
                                                                                                   language,
                                                                                                   this,
                                                                                                   subscriptionKey,
                                                                                                   luisAppID,
                                                                                                   luisSubscriptionID);
                m_micClient = intentMicClient;

            }
        }
        else if (!m_isMicrophoneReco && null == m_dataClient) {
            if (!m_isIntent) {
                m_dataClient = SpeechRecognitionServiceFactory.createDataClient(this,
                                                                                m_recoMode,
                                                                                language,
                                                                                this,
                                                                                subscriptionKey);
            }
            else {
                DataRecognitionClientWithIntent intentDataClient;
                intentDataClient = SpeechRecognitionServiceFactory.createDataClientWithIntent(this,
                                                                                              language,
                                                                                              this,
                                                                                              subscriptionKey,
                                                                                              luisAppID,
                                                                                              luisSubscriptionID);
                m_dataClient = intentDataClient;
            }
        }
    }

```

La biblioteca cliente proporciona clases de cliente de reconocimiento implementadas previamente para los escenarios típicos de reconocimiento de voz:

* `DataRecognitionClient`: reconocimiento de voz con datos PCM (por ejemplo, desde un origen de archivo o audio). Los datos se dividen en búferes y cada búfer se envía al servicio de voz. No se realiza ninguna modificación en los búferes, por lo que el usuario puede aplicar su propia detección de silencio, si lo desea. Si se proporcionan datos desde archivos WAV, puede enviar datos desde el archivo directamente al servicio de voz. Si tiene datos sin procesar, por ejemplo, audio procedente de Bluetooth, en primer lugar envíe un encabezado de formato al servicio de voz seguido por los datos.
* `MicrophoneRecognitionClient`: reconocimiento de voz con audio procedente del micrófono. Asegúrese de que el micrófono esté encendido y de que los datos del micrófono se envían al servicio de reconocimiento de voz. Se aplica una "latencia de silencio" integrada a los datos de micrófono antes de que se envíen al servicio de reconocimiento.
* `DataRecognitionClientWithIntent` y `MicrophoneRecognitionClientWithIntent`: estos clientes devuelven, además del texto de reconocimiento, información estructurada sobre la intención del orador, que puede usarse para controlar acciones adicionales de las aplicaciones. Para usar la "intención", primero debe entrenar un modelo mediante [LUIS](https://azure.microsoft.com/services/cognitive-services/language-understanding-intelligent-service/).

### <a name="recognition-language"></a>Idioma de reconocimiento

Cuando se usa `SpeechRecognitionServiceFactory` para crear el cliente, debe seleccionar un idioma. Para obtener la lista completa de los idiomas admitidos por el servicio de voz, consulte [Supported languages](../API-Reference-REST/supportedlanguages.md) (Idiomas admitidos).

### `SpeechRecognitionMode`

También debe especificar `SpeechRecognitionMode` cuando cree el cliente con `SpeechRecognitionServiceFactory`:

* `ShortPhrase`: una expresión de hasta 15 segundos de duración. A medida que los datos se envían al servicio, el cliente recibe varios resultados parciales y un resultado final con n opciones mejores.
* `LongDictation`: una expresión de hasta dos minutos de duración. A medida que los datos se envían al servicio, el cliente recibe varios resultados parciales y varios resultados finales, en función de dónde identifica las pausas el servicio.

### <a name="attach-event-handlers"></a>Conectar controladores de eventos

Puede conectar varios controladores de eventos al cliente que creó:

* **Eventos de resultados parciales**: se llama a este evento cada vez que el servicio de voz predice lo que puede estar diciendo, incluso antes de que termine de hablar (si usa `MicrophoneRecognitionClient`) o termine de enviar datos (si usa `DataRecognitionClient`).
* **Eventos de error**: se llama cuando el servicio detecta un error.
* **Eventos de intención**: se llama en los clientes "WithIntent" (solo en modo `ShortPhrase`) después de que se analice el resultado final del reconocimiento en una intención JSON estructurada.
* **Eventos de resultado**:
  * En modo `ShortPhrase`, se llama a este evento y devuelve los n resultados mejores cuando termina de hablar.
  * En modo `LongDictation`, se llama al controlador de eventos varias veces, en función de dónde identifica el servicio las pausas en la frase.
  * **Para cada una de las n opciones mejores**, se devuelve un valor de confianza y unos cuantos formatos diferentes del texto reconocido. Para más información, consulte [Output format](../Concepts.md#output-format) (Formato de salida).

## <a name="related-topics"></a>Temas relacionados

* [Referencia de la biblioteca de clientes para Android](https://github.com/Azure-Samples/Cognitive-Speech-STT-Android/tree/master/docs)
* [Introducción a Microsoft Speech API en C# para Windows en .NET](GetStartedCSharpDesktop.md)
* [Introducción a Microsoft Speech API en Objective-C en iOS](Get-Started-ObjectiveC-iOS.md)
* [Introducción a Microsoft Speech API en JavaScript](GetStartedJSWebsockets.md)
* [Introducción a Microsoft Speech API mediante REST](GetStartedREST.md)
