---
title: Uso de un punto de conexión personalizado de voz con Custom Speech Service en Azure | Microsoft Docs
description: Aprenda a usar un punto de conexión personalizado de voz a texto con Custom Speech Service en Cognitive Services.
services: cognitive-services
author: PanosPeriorellis
manager: onano
ms.service: cognitive-services
ms.component: custom-speech
ms.topic: article
ms.date: 02/08/2017
ms.author: panosper
ROBOTS: NOINDEX
ms.openlocfilehash: 55583952df3b83331f1f622a4fce269713ecf2a6
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46966527"
---
# <a name="use-a-custom-speech-to-text-endpoint"></a>Uso de un punto de conexión personalizado de voz a texto
Puede enviar solicitudes a un punto de conexión de voz a texto de Azure Custom Speech Service de forma similar a la que se envían al punto de conexión de voz predeterminado de Cognitive Services. Estos puntos de conexión son funcionalmente idénticos a los puntos de conexión predeterminados de Speech API. Por lo tanto, la misma funcionalidad disponible a través de la biblioteca cliente o la API REST para Speech API también está disponible para el punto de conexión personalizado.

Los puntos de conexión que cree mediante el uso de este servicio pueden procesar un número diferente de solicitudes simultáneas. El volumen depende del plan de tarifa asociado a la suscripción. Si se reciben demasiadas solicitudes, se produce un error. El plan gratuito tiene un límite mensual de solicitudes.

El servicio supone que los datos se transmiten en tiempo real. Si se envía con mayor rapidez, la solicitud se considerará en ejecución hasta que transcurra la duración del audio en tiempo real.

> [!NOTE]
> Aún se admiten los [nuevos sockets web](https://docs.microsoft.com/azure/cognitive-services/speech/api-reference-rest/websocketprotocol). Si tiene pensado usar sockets web con un punto de conexión de voz personalizado, siga estas instrucciones que aparecen a continuación.
>
> La compatibilidad con la nueva [API REST](https://docs.microsoft.com/azure/cognitive-services/speech/getstarted/getstartedrest) pronto estará disponible. Si tiene pensado llamar al punto de conexión de voz personalizado a través de HTTP, siga estas instrucciones que aparecen a continuación.
>

## <a name="send-requests-by-using-the-speech-client-library"></a>Envío de solicitudes mediante el uso de la biblioteca cliente de voz

Para enviar solicitudes a su punto de conexión personalizado mediante el uso de la biblioteca cliente de voz, inicie el cliente de reconocimiento. Use el SDK de Client Speech desde [NuGet](http://nuget.org/). Busque *reconocimiento de voz* y seleccione el paquete de reconocimiento de voz de Microsoft para su plataforma. El ejemplo de código se puede encontrar en [GitHub](https://github.com/Microsoft/Cognitive-Speech-STT-Windows). El SDK de Client Speech proporciona un generador de clases **SpeechRecognitionServiceFactory**, que ofrece los métodos siguientes:

  *   ```CreateDataClient(...)```: un cliente de reconocimiento de datos.
  *   ```CreateDataClientWithIntent(...)```: un cliente de reconocimiento de datos con intención.
  *   ```CreateMicrophoneClient(...)```: un cliente de reconocimiento de micrófono.
  *   ```CreateMicrophoneClientWithIntent(...)```: un cliente de reconocimiento de micrófono con intención.

Para obtener documentación detallada, consulte [Bing Speech API](https://docs.microsoft.com/azure/cognitive-services/speech/home). Los puntos de conexión de Custom Speech Service admiten el mismo SDK.

El cliente de reconocimiento de datos es adecuado para el reconocimiento de voz a partir de datos como, por ejemplo, un archivo u otro origen de audio. El cliente de reconocimiento de micrófono es adecuado para el reconocimiento de voz procedente de un micrófono. El uso de intención en cualquiera de los clientes puede devolver resultados de intención estructurados mediante [Language Understanding Intelligent Service](https://www.luis.ai/) (LUIS), si ha compilado una aplicación LUIS para su escenario.

Se pueden crear instancias de los cuatro tipos de clientes de dos maneras. El primer método usa Cognitive Services Speech API estándar. La segunda manera le permite especificar una dirección URL que se corresponde con el punto de conexión personalizado que se creó con Custom Speech Service.

Por ejemplo, puede crear un cliente **DataRecognitionClient** que envíe solicitudes a un punto de conexión personalizado mediante el método siguiente:

```csharp
public static DataRecognitionClient CreateDataClient(SpeeechRecognitionMode speechRecognitionMode, string language, string primaryOrSecondaryKey, **string url**);
```

**your_subscriptionId** y **endpointURL** hacen referencia a la clave de suscripción y a la dirección URL de los sockets web, respectivamente, en la página **Información de implementación**.

**AuthenticationUri** se utiliza para recibir un token del servicio de autenticación. Este identificador URI se debe establecer por separado, como se muestra en el siguiente ejemplo de código.

Este ejemplo de código muestra cómo usar el SDK del cliente:

```csharp
var dataClient = SpeechRecognitionServiceFactory.CreateDataClient(
  SpeechRecognitionMode.LongDictation,
  "en-us",
  "your_subscriptionId",
  "your_subscriptionId",
  "endpointURL");
// set the authorization Uri
dataClient.AuthenticationUri = "https://westus.api.cognitive.microsoft.com/sts/v1.0/issueToken";
```

> [!NOTE]
> Si usa métodos **Create** en el SDK, deberá proporcionar el identificador de suscripción dos veces debido a la sobrecarga de estos métodos.
>

Custom Speech Service usa dos direcciones URL diferentes para el reconocimiento de formato corto y el de formato largo. Ambas aparecen en la página **Implementaciones**. Use la dirección URL correcta del punto de conexión para el formato específico que desea usar.

Para más información sobre cómo invocar los distintos clientes de reconocimiento con el punto de conexión personalizado, consulte la clase [SpeechRecognitionServiceFactory](https://www.microsoft.com/cognitive-services/Speech-api/documentation/GetStarted/GetStartedCSharpDesktop). La documentación de esta página se refiere a la adaptación de modelos acústicos, pero se aplica a todos los puntos de conexión creados mediante Custom Speech Service.

## <a name="send-requests-by-using-the-speech-protocol"></a>Envío de solicitudes mediante Speech Protocol

Los puntos de conexión que se muestran para [Speech Protocol](https://docs.microsoft.com/azure/cognitive-services/speech/api-reference-rest/websocketprotocol) son puntos de conexión de Open Source Web Socket Speech Protocol.

Actualmente, la única implementación oficial de cliente es para [JavaScript](https://github.com/Azure-Samples/SpeechToText-WebSockets-Javascript). Si quiere empezar con el ejemplo que se proporciona aquí, realice los siguientes cambios en el código y compile el ejemplo de nuevo:

1. En _src\sdk\speech.browser\SpeechConnectionFactory.ts_, reemplace el nombre de host "wss://speech.platform.bing.com" por el nombre de host que aparece en la página de detalles de la implementación. No inserte el identificador URI completo aquí sino simplemente el esquema de protocolo *wss* y el nombre de host. Por ejemplo: 

    ```JavaScript
    private get Host(): string {
        return Storage.Local.GetOrAdd("Host", "wss://<your_key_goes_here>.api.cris.ai");
    }
    ```

2. Establezca el parámetro _recognitionMode_ en _samples\browser\Samples.html_ según sus requisitos:
    * _RecognitionMode.Interactive_ admite solicitudes de hasta 15 segundos.
    * _RecognitionMode.Conversation_ y _RecognitionMode.Dictation_ (ambos son equivalentes en Custom Speech Service) admite solicitudes de hasta 10 minutos.

3. Compile el ejemplo mediante la "compilación de Gulp" antes de utilizarlo.

> [!NOTE]
> Asegúrese de que usa el identificador URI correcto para este protocolo. El esquema obligatorio es *wss* (no *http* como en el protocolo de cliente). 

Para más información, consulte la documentación de [Bing Speech API](https://docs.microsoft.com/azure/cognitive-services/speech/getstarted/getstartedclientlibraries).

## <a name="send-requests-by-using-http"></a>Envío de solicitudes mediante HTTP

Enviar una solicitud al punto de conexión personalizado mediante el uso de un método HTTP Post es parecido a enviar una solicitud mediante HTTP a Cognitive Services Bing Speech API. Modifique la dirección URL para reflejar la dirección de la implementación personalizada.

Hay algunas restricciones en las solicitudes enviadas a través de HTTP al punto de conexión de Cognitive Services Speech y a los puntos de conexión de servicio personalizados que se crearon con este servicio. La solicitud HTTP no puede devolver resultados parciales durante el proceso de reconocimiento. Además, la duración de las solicitudes se limita a 10 segundos para el contenido de audio y a 14 segundos en total.

Para crear una solicitud POST, siga el mismo proceso que se usa para Cognitive Services Speech API.

1. Consigue un token de acceso mediante el identificador de suscripción. Este token es necesario para acceder al punto de conexión de reconocimiento. Se puede reutilizar durante 10 minutos.

    ```
    curl -X POST --header "Ocp-Apim-Subscription-Key:<subscriptionId>" --data "" "https://westus.api.cognitive.microsoft.com/sts/v1.0/issueToken"
    ```
      El **subscriptionId** se debe establecer en el identificador de suscripción que utiliza para esta implementación. La respuesta es el token sin formato que necesita para la siguiente solicitud.

2. Publique audio en el punto de conexión mediante el método POST de nuevo.

    ```
    curl -X POST --data-binary @example.wav -H "Authorization: Bearer <token>" -H "Content-Type: application/octet-stream" "<https_endpoint>"
    ```

    El **token** es el token de acceso que recibió con la llamada anterior. **https_endpoint** es la dirección completa del punto de conexión personalizado de voz a texto que aparece en la página **Información de implementación**.

Para más información sobre los parámetros de HTTP Post y el formato de respuesta, consulte [Microsoft Cognitive Services Bing Speech HTTP API](https://www.microsoft.com/cognitive-services/speech-api/documentation/API-Reference-REST/BingVoiceRecognition#SampleImplementation).

## <a name="send-requests-by-using-the-service-library"></a>Envío de solicitudes mediante la biblioteca de servicios
La biblioteca de servicios permite al servicio usar la nube de transcripción de Microsoft Speech para convertir lenguaje hablado en texto en tiempo real, de forma que la aplicación cliente puede enviar audio y recibir resultados de reconocimiento parciales de forma simultánea y asincrónica. Para más información sobre el SDK del servicio, vaya [aquí](https://docs.microsoft.com/azure/cognitive-services/speech/getstarted/getstartedcsharpservicelibrary)

> [!NOTE]
> Cuando se utiliza la biblioteca de servicios tiene que cambiar el identificador URI del proveedor de autorización en la implementación de **IAuthorizationProvider** a https://westus.api.cognitive.microsoft.com/sts/v1.0/issueToken.

## <a name="next-steps"></a>Pasos siguientes
* Mejore la precisión con el [modelo acústico personalizado](cognitive-services-custom-speech-create-acoustic-model.md).
* Mejore la precisión con un [modelo de lenguaje personalizado](cognitive-services-custom-speech-create-language-model.md).
