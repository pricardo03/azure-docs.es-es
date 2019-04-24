---
title: Protocolo WebSocket de Bing Speech | Microsoft Docs
titlesuffix: Azure Cognitive Services
description: Documentación del protocolo de Bing Speech basado en WebSocket
services: cognitive-services
author: zhouwangzw
manager: wolfma
ms.service: cognitive-services
ms.subservice: bing-speech
ms.topic: article
ms.date: 09/18/2018
ms.author: zhouwang
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: d6601f57d87b518b2061df64174818432b822755
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60515319"
---
# <a name="bing-speech-websocket-protocol"></a>Protocolo WebSocket de Bing Speech

[!INCLUDE [Deprecation note](../../../../includes/cognitive-services-bing-speech-api-deprecation-note.md)]

Bing Speech es una plataforma basada en la nube que incorpora los algoritmos más avanzados disponibles para convertir audio hablado en texto. El protocolo de Bing Speech define la [configuración de conexión](#connection-establishment) entre las aplicaciones cliente y el servicio, y los mensajes de reconocimiento de voz intercambiados entre homólogos ([mensajes originados en el cliente ](#client-originated-messages) y [mensajes originados en el servicio](#service-originated-messages)). Además, se describen los [mensajes de telemetría](#telemetry-schema) y el [control de errores](#error-handling).

## <a name="connection-establishment"></a>Establecimiento de la conexión

El protocolo Servicio de voz sigue la especificación estándar de WebSocket [IETF RFC 6455](https://tools.ietf.org/html/rfc6455). Una conexión de WebSocket se inicia como una solicitud HTTP que contiene los encabezados HTTP que indican el deseo del cliente de actualizar la conexión a WebSocket en lugar de usar la semántica HTTP. El servidor indica su voluntad de participar en la conexión de WebSocket devolviendo una respuesta `101 Switching Protocols` HTTP. Tras el intercambio de este protocolo de enlace, tanto el cliente como el servicio mantienen abierto el socket y empiezan a usar un protocolo basado en mensajes para enviar y recibir información.

Para iniciar el protocolo de enlace de WebSocket, la aplicación cliente envía una solicitud HTTPS GET al servicio. Incluye encabezados de actualización de WebSocket estándar, junto con otros encabezados que son específicos para voz.

```HTTP
GET /speech/recognition/interactive/cognitiveservices/v1 HTTP/1.1
Host: speech.platform.bing.com
Upgrade: websocket
Connection: Upgrade
Sec-WebSocket-Key: dGhlIHNhbXBsZSBub25jZQ==
Sec-WebSocket-Version: 13
Authorization: t=EwCIAgALBAAUWkziSCJKS1VkhugDegv7L0eAAJqBYKKTzpPZOeGk7RfZmdBhYY28jl&p=
X-ConnectionId: A140CAF92F71469FA41C72C7B5849253
Origin: https://speech.platform.bing.com
```

El servicio responde con:

```HTTP
HTTP/1.1 101 Switching Protocols
Upgrade: websocket
Connection: upgrade
Sec-WebSocket-Key: 2PTTXbeeBXlrrUNsY15n01d/Pcc=
Set-Cookie: SpeechServiceToken=AAAAABAAWTC8ncb8COL; expires=Wed, 17 Aug 2016 15:39:06 GMT; domain=bing.com; path="/"
Date: Wed, 17 Aug 2016 15:03:52 GMT
```

Todas las solicitudes de voz necesitan el cifrado [TLS](https://en.wikipedia.org/wiki/Transport_Layer_Security). No se admite el uso de las solicitudes de voz sin cifrar. Se admite la siguiente versión de TLS:

* TLS 1.2

### <a name="connection-identifier"></a>Identificador de la conexión

Servicio de voz requiere que todos los clientes incluyen un identificador único para identificar la conexión. Los clientes *deben* incluir el encabezado *X-ConnectionId* cuando inician un protocolo de enlace de WebSocket. El encabezado *X-ConnectionId* debe ser un valor de [identificador único universal](https://en.wikipedia.org/wiki/Universally_unique_identifier) (UUID). Las solicitudes de actualización de WebSocket que no incluyen *X-ConnectionId* no especifican un valor para el encabezado *X-ConnectionId*, o que no incluyen un valor UUID válido son rechazados por el servicio con una respuesta `400 Bad Request` HTTP.

### <a name="authorization"></a>Autorización

Además de los encabezados de protocolo de enlace de WebSocket estándar, las solicitudes de voz requieren un encabezado *Authorization*. El servicio rechaza las solicitudes de conexión sin este encabezado con una respuesta `403 Forbidden` HTTP.

El encabezado *Authorization* debe contener un token de acceso JSON Web Token (JWT).

Para obtener información acerca de cómo suscribirse y obtener claves de API que se usan para recuperar los tokens de acceso JWT válidos, consulte la página de [suscripción A Cognitive Services](https://azure.microsoft.com/try/cognitive-services/).

La clave de API se pasa al servicio de token. Por ejemplo: 

``` HTTP
POST https://api.cognitive.microsoft.com/sts/v1.0/issueToken
Content-Length: 0
```

La siguiente información de encabezado es obligatoria para el token de acceso.

| Name | Formato | DESCRIPCIÓN |
|----|----|----|
| Ocp-Apim-Subscription-Key | ASCII | Su clave de suscripción |

El servicio de token devuelve el token de acceso JWT como `text/plain`. A continuación, el JWT se pasa como `Base64 access_token` al protocolo de enlace como un encabezado *Authorization* prefijado con la cadena `Bearer`. Por ejemplo: 

`Authorization: Bearer [Base64 access_token]`

### <a name="cookies"></a>Cookies

Los clientes *deben* admiten cookies HTTP según se especifica en [RFC 6265](https://tools.ietf.org/html/rfc6265).

### <a name="http-redirection"></a>Redireccionamiento HTTP

Los clientes *deben* admite los mecanismos de redireccionamiento estándar indicados por la [especificación del protocolo HTTP](https://www.w3.org/Protocols/rfc2616/rfc2616.html).

### <a name="speech-endpoints"></a>Puntos de conexión de voz

Los clientes *deben* usar un punto de conexión adecuado de Servicio de voz. El punto de conexión se basa en el modo y el idioma de reconocimiento. En la tabla se muestran algunos ejemplos.

| Mode | Ruta de acceso | URI de servicio |
| -----|-----|-----|
| Interactive | /speech/recognition/interactive/cognitiveservices/v1 | https://speech.platform.bing.com/speech/recognition/interactive/cognitiveservices/v1?language=pt-BR |
| Conversation | /speech/recognition/conversation/cognitiveservices/v1 | https://speech.platform.bing.com/speech/recognition/conversation/cognitiveservices/v1?language=en-US |
| Dictation | /speech/recognition/dictation/cognitiveservices/v1 | https://speech.platform.bing.com/speech/recognition/dictation/cognitiveservices/v1?language=fr-FR |

Para obtener más información, consulte la página [Service URI](../GetStarted/GetStartedREST.md#service-uri) (URI de servicio).

### <a name="report-connection-problems"></a>Informar problemas de conexión

Los clientes deben informar inmediatamente todos los problemas encontrados al establecer una conexión. El protocolo de mensajes para informar de errores de conexión se describe en [Telemetría de errores de conexión](#connection-failure-telemetry).

### <a name="connection-duration-limitations"></a>Limitaciones de duración de la conexión

Las conexiones de WebSocket, cuando se comparan con las conexiones HTTP típicas al servicio web, duran *mucho* tiempo. Servicio de voz impone algunas limitaciones sobre la duración de las conexiones de WebSocket al servicio:

 * La duración máxima para cualquier conexión de WebSocket activa es de 10 minutos. Una conexión está activa si el servicio o el cliente envían los mensajes de WebSocket través de esa conexión. El servicio finaliza la conexión sin avisar cuando se alcanza el límite. Los clientes deben desarrollar escenarios de usuario que no requieran que la conexión permanezca activa durante la vigencia máxima de conexión o un valor próximo a esta.

 * La duración máxima para cualquier conexión de WebSocket inactiva es de 180 segundos. Una conexión está inactiva si ni el servicio ni el cliente envían un mensaje de WebSocket través de la conexión. Cuando se alcanza la vigencia máxima inactiva, el servicio finaliza la conexión de WebSocket inactiva.

## <a name="message-types"></a>Tipos de mensaje

Una vez establecida una conexión de WebSocket entre el cliente y el servicio, tanto el cliente como el servicio pueden enviar mensajes. En esta sección se describe el formato de estos mensajes de WebSocket.

[IETF RFC 6455](https://tools.ietf.org/html/rfc6455) especifica que los mensajes de WebSocket pueden transmitir datos mediante una codificación binaria o de texto. Las dos codificaciones utilizan distintos formatos de red. Cada formato está optimizado para una eficaz codificación, transmisión y descodificación de la carga del mensaje.

### <a name="text-websocket-messages"></a>Mensajes de texto de WebSocket

Los mensajes de texto de WebSocket llevan una carga de información textual que consta de una sección de encabezados y un cuerpo separados por el par conocido de nueva línea de doble retorno de carro usado para los mensajes HTTP. Y, al igual que los mensajes HTTP, los mensajes de texto de WebSocket especifican encabezados en formato *nombre:valor*, separado por un par de nueva línea de retorno de carro simple. Cualquier texto incluido en un mensaje de texto de WebSocket *debe* usar codificación [UTF-8](https://tools.ietf.org/html/rfc3629).

Los mensajes de texto de WebSocket deben especificar una ruta de acceso de mensaje en el encabezado *Path*. El valor de este encabezado debe ser uno de los tipos de mensaje de protocolo de voz definidos más adelante en este documento.

### <a name="binary-websocket-messages"></a>Mensajes binarios de WebSocket

Los mensajes binarios de WebSocket llevan una carga binaria. En el protocolo Servicio de voz, el audio se transmite hacia y se recibe desde el servicio a través de mensajes binarios de WebSocket. Todos los demás mensajes son mensajes de texto de WebSocket.

Al igual que los mensajes de texto de WebSocket, los mensajes binarios de WebSocket constan de una sección de encabezado y una de cuerpo. Los 2 primeros bytes especifican el mensaje binario de WebSocket, en orden [big-endian](https://en.wikipedia.org/wiki/Endianness), el tamaño de entero de 16 bits de la sección de encabezado. El tamaño mínimo de la sección de encabezado es de 0 bytes. El tamaño máximo es de 8192 bytes. El texto en los encabezados de los mensajes binarios de WebSocket *debe* usar codificación [US-ASCII](https://tools.ietf.org/html/rfc20).

Los encabezados de un mensaje binario de WebSocket se codifican con el mismo formato que en los mensajes de texto de WebSocket. El formato *nombre:valor* está separado por un par de nueva línea de retorno de carro simple. Los mensajes binarios de WebSocket deben especificar una ruta de acceso de mensaje en el encabezado *Path*. El valor de este encabezado debe ser uno de los tipos de mensaje de protocolo de voz definidos más adelante en este documento.

Los mensajes de texto y binarios de WebSocket se utilizan en el protocolo Servicio de voz.

## <a name="client-originated-messages"></a>Mensajes originados por el cliente

Una vez establecida la conexión, el cliente y el servicio pueden empezar a enviar mensajes. En esta sección se describe el formato y la carga de mensajes que las aplicaciones cliente envía a Servicio de voz. La sección [Mensajes originados por el servicio](#service-originated-messages) presenta los mensajes que se originan en Servicio de voz y se envían a las aplicaciones cliente.

Los mensajes principales enviados por el cliente a los servicios son `speech.config`, `audio`, y `telemetry`. Antes de analizar cada mensaje en detalle, se describen los encabezados de mensaje obligatorios comunes para todos estos mensajes.

### <a name="required-message-headers"></a>Encabezados de mensaje obligatorios

Los encabezados siguientes son obligatorios para todos los mensajes originados por el cliente.

| Encabezado | Value |
|----|----|
| Ruta de acceso | La ruta de acceso del mensaje según lo especificado en este documento |
| X-RequestId | UUID en formato "no-dash" |
| X-Timestamp | Marca de tiempo del reloj de cliente UTC en formato ISO 8601 |

#### <a name="x-requestid-header"></a>Encabezado X-RequestId

Las solicitudes originadas por el cliente se identifican de manera exclusiva mediante el encabezado del mensaje *X-RequestId*. Este encabezado es obligatorio para todos los mensajes originados por el cliente. El valor de encabezado *X-RequestId* debe ser un UUID en formato "no-dash", por ejemplo: *123e4567e89b12d3a456426655440000*. *No puede* estar en formato canónico: *123e4567-e89b-12d3-a456-426655440000*. Las solicitudes sin un encabezado *X-RequestId* o con un valor de encabezado que use un formato incorrecto para UUID provocan que el servicio finalice la conexión de WebSocket.

#### <a name="x-timestamp-header"></a>Encabezado X-Timestamp

Cada mensaje que envía una aplicación cliente a Servicio de voz *debe* incluir un encabezado *X-Timestamp*. El valor de este encabezado es la hora cuando el cliente envía el mensaje. Las solicitudes sin un encabezado *X-Timestamp* o con un valor de encabezado que use un formato incorrecto provocan que el servicio finalice la conexión de WebSocket.

El valor de encabezado *X-Timestamp* debe tener el formato 'aaaa'-'MM'-'dd'T'HH':'mm':'ss'.'fffffffZ', donde 'fffffff' es una fracción de segundo. Por ejemplo, '12,5' significa '12 + 5/10 segundos' y '12.526' significa '12 más 526/1000 segundos'. Este formato cumple con [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) y, a diferencia del encabezado *Date* HTTP estándar, puede proporcionar una resolución de milisegundos. Las aplicaciones cliente pueden redondear las marcas de tiempo al milisegundo más cercano. Las aplicaciones cliente tienen que asegurarse de que el reloj del dispositivo registra la hora con precisión a través de un [servidor de protocolo de tiempo de redes (NTP)](https://en.wikipedia.org/wiki/Network_Time_Protocol).

### <a name="message-speechconfig"></a>Mensaje `speech.config`

Servicio de voz necesita conocer las características de la aplicación para proporcionar el mejor reconocimiento de voz posible. Los datos de las características necesarias incluyen información sobre el dispositivo y el sistema operativo que se utilizan para la aplicación. Esta información se proporciona en el mensaje `speech.config`.

Los clientes *deben* enviar un mensaje `speech.config` inmediatamente después de establecer la conexión a Servicio de voz y antes de enviar cualquier mensaje `audio`. Debe enviar un mensaje `speech.config` una sola vez por conexión.

| Campo | DESCRIPCIÓN |
|----|----|
| Codificación de mensajes de WebSocket | Text |
| Cuerpo | La carga como estructura JSON |

#### <a name="required-message-headers"></a>Encabezados de mensaje obligatorios

| Nombre de encabezado | Value |
|----|----|
| Ruta de acceso | `speech.config` |
| X-Timestamp | Marca de tiempo del reloj de cliente UTC en formato ISO 8601 |
| Content-Type | application/json; charset=utf-8 |

Al igual que con todos los mensajes originados por el cliente en el protocolo Servicio de voz, el mensaje `speech.config` *debe* incluir un encabezado *X-Timestamp* que registre la hora del reloj UTC del cliente cuando se envió el mensaje al servicio. El mensaje `speech.config` *no* requiere un encabezado *X-RequestId* porque este mensaje no está asociado a una solicitud de voz determinada.

#### <a name="message-payload"></a>Carga del mensaje
La carga del mensaje `speech.config` es una estructura JSON que contiene información sobre la aplicación. En el siguiente ejemplo se muestra esta información. La información de contexto del cliente y del dispositivo se incluye en el elemento *context* de la estructura JSON.

```JSON
{
  "context": {
    "system": {
      "version": "2.0.12341",
    },
    "os": {
      "platform": "Linux",
      "name": "Debian",
      "version": "2.14324324"
    },
    "device": {
      "manufacturer": "Contoso",
      "model": "Fabrikan",
      "version": "7.341"
      }
    },
  }
}
```

##### <a name="system-element"></a>Elemento del sistema

El elemento system.version del mensaje `speech.config` contiene la versión del SDK del software de voz usado por la aplicación o el dispositivo cliente. El valor adopta el formato *major.minor.build.branch*. Puede omitir el componente *branch* si no corresponde.

##### <a name="os-element"></a>Elemento del sistema operativo

| Campo | DESCRIPCIÓN | Uso |
|-|-|-|
| os.platform | La plataforma del sistema operativo que aloja la aplicación, por ejemplo: Windows, Android, iOS o Linux. |Obligatorio |
| os.name | El nombre del producto del sistema operativo, por ejemplo: Debian o Windows 10 | Obligatorio |
| os.version | La versión del sistema operativo adopta el formato *major.minor.build.branch*. | Obligatorio |

##### <a name="device-element"></a>Elemento de dispositivo

| Campo | DESCRIPCIÓN | Uso |
|-|-|-|
| device.manufacturer | El fabricante del hardware del dispositivo. | Obligatorio |
| device.model | El modelo del dispositivo. | Obligatorio |
| device.version | La versión de software del dispositivo indicada por el fabricante del dispositivo. Este valor especifica una versión del dispositivo a la cual el fabricante puede hacer un seguimiento. | Obligatorio |

### <a name="message-audio"></a>Mensaje `audio`

Las aplicaciones cliente habilitadas para voz envían audio a Servicio de voz mediante la conversión de la secuencia de audio en una serie de fragmentos de audio. Cada fragmento de audio lleva un segmento de audio hablado que el servicio transcribe. El tamaño máximo de un único fragmento de audio es de 8192 bytes. Los mensajes de secuencia de audio son *mensajes binarios de WebSocket*.

Los clientes utilizan el mensaje `audio` para enviar un fragmento de audio al servicio. Los clientes leen el audio del micrófono en fragmentos y envían estos fragmentos a Servicio de voz para la transcripción. El primer mensaje `audio` debe contener un encabezado con formato correcto que especifique de manera adecuada que el audio se ajusta a uno de los formatos de codificación admitidos por el servicio. Los mensajes `audio` adicionales solo contienen los datos binarios de la secuencia de audio leídos desde el micrófono.

De manera opcional, los clientes pueden enviar un mensaje `audio` con un cuerpo de longitud cero. Este mensaje indica al servicio que el cliente sabe que el usuario ha dejado de hablar, se finaliza el enunciado y el micrófono se desactiva.

Servicio de voz utiliza el primer mensaje `audio` que contiene un identificador de solicitud único para señalar el inicio de un nuevo ciclo o *turno* de solicitud/respuesta. Una vez que el servicio recibe un mensaje `audio` con un nuevo identificador de solicitud, descarta los mensajes en cola o no enviados que están asociados a cualquier turno anterior.

| Campo | DESCRIPCIÓN |
|-------------|----------------|
| Codificación de mensajes de WebSocket | Binary |
| Cuerpo | Los datos binarios para el fragmento de audio. El tamaño máximo es de 8192 bytes. |

#### <a name="required-message-headers"></a>Encabezados de mensaje obligatorios

Los siguientes encabezados son obligatorios para todos los mensajes `audio`.

| Encabezado         |  Value     |
| ------------- | ---------------- |
| Ruta de acceso | `audio` |
| X-RequestId | UUID en formato "no-dash" |
| X-Timestamp | Marca de tiempo del reloj de cliente UTC en formato ISO 8601 |
| Content-Type | El tipo de contenido de audio. El tipo debe ser *audio/x-wav* (PCM) o *audio/silk* (SILK). |

#### <a name="supported-audio-encodings"></a>Codificaciones de audio compatibles

En esta sección se describen los códecs de audio compatibles con Servicio de voz.

##### <a name="pcm"></a>PCM

Servicio de voz acepta audio de modulación por impulsos codificados (PCM) sin comprimir. El audio se envía al servicio en formato [WAV](https://en.wikipedia.org/wiki/WAV), de modo que el primer fragmento de audio *debe* contener un encabezado en [formato de archivo para intercambio de recursos](https://en.wikipedia.org/wiki/Resource_Interchange_File_Format) (RIFF) válido. Si un cliente inicia un turno con un fragmento de audio que *no* incluye un encabezado RIFF válido, el servicio rechaza la solicitud y finaliza la conexión de WebSocket.

El audio PCM *debe* muestrearse a 16 kHz con 16 bits por muestra y un canal (*riff-16khz-16bit-mono-pcm*). Servicio de voz no es compatible con secuencias de audio estéreo y rechaza las secuencias de audio que no utilizan la velocidad de bits, la frecuencia de muestreo o el número de canales especificados.

##### <a name="opus"></a>Opus

Opus es un códec de audio abierto, libre de regalías, muy versátil. Servicio de voz admite Opus a una velocidad de bits constante de `32000` o `16000`. Actualmente solo se admite el contenedor `OGG` para Opus especificado por el tipo mime `audio/ogg`.

Para usar Opus, modifique el [ejemplo de JavaScript](https://github.com/Azure-Samples/SpeechToText-WebSockets-Javascript/blob/master/samples/browser/Sample.html#L101) y cambie el método `RecognizerSetup` que se devolverá.

```javascript
return SDK.CreateRecognizerWithCustomAudioSource(
            recognizerConfig,
            authentication,
            new SDK.MicAudioSource(
                     new SDK.OpusRecorder(
                     {
                         mimeType: "audio/ogg",
                         bitsPerSecond: 32000
                     }
              )
          ));
```

#### <a name="detect-end-of-speech"></a>Detección del final del habla

Las personas no señalan explícitamente una vez que han terminado de hablar. Cualquier aplicación que acepte voz como entrada tiene dos opciones para controlar el final del habla en una secuencia de audio: detección del final del habla por el servicio y detección del final del habla por el cliente. De estas dos opciones, la detección del final del habla por el servicio suele proporcionar una mejor experiencia de usuario.

##### <a name="service-end-of-speech-detection"></a>Detección del final del habla del servicio

Para compilar la experiencia de voz de manos libres ideal, las aplicaciones permite que el servicio detecte cuándo el usuario ha terminado de hablar. Los clientes envían audio del micrófono como fragmentos de *audio* hasta que el servicio detecta silencio y responde con un mensaje `speech.endDetected`.

##### <a name="client-end-of-speech-detection"></a>Detección del final del habla del cliente

Las aplicaciones cliente que permiten al usuario indicar el final del habla de alguna manera también pueden enviar esa señal al servicio. Por ejemplo, una aplicación cliente podría tener un botón "Parar" o "Silenciar" que el usuario puede presionar. Para indicar el final del habla, las aplicaciones de cliente enviar un mensaje de fragmento de *audio* con un cuerpo de longitud cero. Servicio de voz interpreta este mensaje como el final de la secuencia de audio entrante.

### <a name="message-telemetry"></a>Mensaje `telemetry`

Las aplicaciones cliente *deben* reconocer el final de cada turno mediante el envío de telemetría sobre el turno a Servicio de voz. La confirmación del final del turno permite a Servicio de voz asegurarse de que todos los mensajes necesarios para la finalización de la solicitud y de que el cliente recibió la respuesta correctamente. A su vez, la confirmación del final del turno permite a Servicio de voz comprobar que las aplicaciones cliente funcionan según lo previsto. Esta información es invaluable si necesita ayuda para solucionar problemas de una aplicación habilitada para voz.

Los clientes deben confirmar el final de un turno enviando un mensaje `telemetry` poco después de recibir un mensaje `turn.end`. Los clientes deberían intentar confirmar el mensaje `turn.end` lo antes posible. Si una aplicación cliente no confirma el final de un turno, Servicio de voz puede finalizar la conexión con un error. Los clientes deben enviar solo un mensaje `telemetry` a cada solicitud y respuesta identificadas por el valor *X-RequestId*.

| Campo | DESCRIPCIÓN |
| ------------- | ---------------- |
| Codificación de mensajes de WebSocket | Text |
| Ruta de acceso | `telemetry` |
| X-Timestamp | Marca de tiempo del reloj de cliente UTC en formato ISO 8601 |
| Content-Type | `application/json` |
| Cuerpo | Una estructura JSON que contiene información del cliente sobre el turno |

El esquema para el cuerpo del mensaje `telemetry` se define en la sección [Esquema de telemetría](#telemetry-schema).

#### <a name="telemetry-for-interrupted-connections"></a>Telemetría para conexiones interrumpidas

Si la conexión de red falla por algún motivo durante un turno y el cliente *no* recibe un mensaje `turn.end` desde el servicio, el cliente envía un mensaje `telemetry`. Este mensaje describe la solicitud con error la próxima vez que el cliente establece una conexión con el servicio. Los clientes no tienen que intentar una conexión inmediatamente para enviar el mensaje `telemetry`. El mensaje podría almacenarse en búfer en el cliente y enviarse por una conexión futuras solicitada por el usuario. El mensaje `telemetry` para las solicitudes con error *debe* usar el valor *X-RequestId* de la solicitud con error. Puede enviarse al servicio en cuanto se establece una conexión, sin tener que esperar para enviar o recibir otros mensajes.

## <a name="service-originated-messages"></a>Mensajes originados en el servicio

En esta sección se describen los mensajes que se originan en Servicio de voz y se envían al cliente. Servicio de voz mantiene un registro de las funcionalidades de cliente y genera los mensajes necesarios por cada cliente, por lo que no todos los clientes reciben todos los mensajes que se describen aquí. Para resumir, se hace referencia a los mensajes según el valor del encabezado *Path*. Por ejemplo, nos referimos a un mensaje de texto de WebSocket con el valor *Path* `speech.hypothesis` como un mensaje speech.hypothesis.

### <a name="message-speechstartdetected"></a>Mensaje `speech.startDetected`

El mensaje `speech.startDetected` indica que Servicio de voz ha detectado voz en la secuencia de audio.

| Campo | DESCRIPCIÓN |
| ------------- | ---------------- |
| Codificación de mensajes de WebSocket | Text |
| Ruta de acceso | `speech.startDetected` |
| Content-Type | application/json; charset=utf-8 |
| Cuerpo | La estructura JSON que contiene información sobre las condiciones cuando se detectó el inicio de la voz. El campo *Offset* de esta estructura especifica el desfase (en unidades de 100 nanosegundos) cuando se detecta la voz en la secuencia de audio, en relación con el inicio de la secuencia. |

#### <a name="sample-message"></a>Mensaje de ejemplo

```HTML
Path: speech.startDetected
Content-Type: application/json; charset=utf-8
X-RequestId: 123e4567e89b12d3a456426655440000

{
  "Offset": 100000
}
```

### <a name="message-speechhypothesis"></a>Mensaje `speech.hypothesis`

Durante el reconocimiento de voz, Servicio de voz genera periódicamente hipótesis sobre las palabras que el servicio ha reconocido. Servicio de voz envía estas hipótesis al cliente aproximadamente cada 300 milisegundos. El valor `speech.hypothesis` es adecuado *solo* para mejorar la experiencia de voz del usuario. No debe tener ninguna dependencia en el contenido ni la precisión del texto en estos mensajes.

 El mensaje `speech.hypothesis` corresponde a los clientes que tienen alguna funcionalidad de representación de texto y desean proporcionar comentarios casi en tiempo real del reconocimiento en curso a la persona que está hablando.

| Campo | DESCRIPCIÓN |
| ------------- | ---------------- |
| Codificación de mensajes de WebSocket | Text |
| Ruta de acceso | `speech.hypothesis` |
| X-RequestId | UUID en formato "no-dash" |
| Content-Type | application/json |
| Cuerpo | La estructura JSON de la hipótesis de voz |

#### <a name="sample-message"></a>Mensaje de ejemplo

```HTML
Path: speech.hypothesis
Content-Type: application/json; charset=utf-8
X-RequestId: 123e4567e89b12d3a456426655440000

{
  "Text": "this is a speech hypothesis",
  "Offset": 0,
  "Duration": 23600000
}
```

El elemento *Offset* especifica el desfase (en unidades de 100 nanosegundos) cuando se reconoció la frase, en relación con el inicio de la secuencia de audio.

El elemento *Duration* especifica la duración (en unidades de 100 nanosegundos) de esta frase de voz.

Los clientes no deben hacer ninguna suposición sobre la frecuencia, el tiempo o el texto contenido en una hipótesis de voz o la coherencia de texto en cualesquiera dos hipótesis de voz. Las hipótesis son tan solo instantáneas en el proceso de transcripción en el servicio. No representan una acumulación estable de transcripción. Por ejemplo, una primera hipótesis de voz podría contener las palabras "bien diversión", y la segunda hipótesis podría contener las palabras "encontrar divertido". Servicio de voz no realiza ningún procesamiento posterior (por ejemplo, mayúsculas, minúsculas, puntuación) en el texto de la hipótesis de voz.

### <a name="message-speechphrase"></a>Mensaje `speech.phrase`

Cuando Servicio de voz determina que tiene suficiente información para generar un resultado de reconocimiento que no cambiará, el servicio se genera un mensaje `speech.phrase`. Servicio de voz genera estos resultados después de detectar que el usuario ha terminado una oración o una frase.

| Campo | DESCRIPCIÓN |
| ------------- | ---------------- |
| Codificación de mensajes de WebSocket | Text |
| Ruta de acceso | `speech.phrase` |
| Content-Type | application/json |
| Cuerpo | La estructura JSON de la frase de voz |

El esquema JSON de la frase de voz incluye los siguientes campos: `RecognitionStatus`, `DisplayText`, `Offset` y `Duration`. Para obtener más información sobre estos campos, consulte [Transcription responses](../concepts.md#transcription-responses) (Respuestas de transcripción).

#### <a name="sample-message"></a>Mensaje de ejemplo

```HTML
Path: speech.phrase
Content-Type: application/json; charset=utf-8
X-RequestId: 123e4567e89b12d3a456426655440000

{
  "RecognitionStatus": "Success",
  "DisplayText": "Remind me to buy 5 pencils.",
  "Offset": 0,
  "Duration": 12300000
}
```

### <a name="message-speechenddetected"></a>Mensaje `speech.endDetected`

El mensaje `speech.endDetected` especifica que la aplicación cliente debe detener la transmisión de secuencias de audio al servicio.

| Campo | DESCRIPCIÓN |
| ------------- | ---------------- |
| Codificación de mensajes de WebSocket | Text |
| Ruta de acceso | `speech.endDetected` |
| Cuerpo | La estructura JSON que contiene el desfase cuando se ha detectado el final del habla. El desfase se representa en el unidades de 100 nanosegundos desde el principio del audio que se usó para el reconocimiento. |
| Content-Type | application/json; charset=utf-8 |

#### <a name="sample-message"></a>Mensaje de ejemplo

```HTML
Path: speech.endDetected
Content-Type: application/json; charset=utf-8
X-RequestId: 123e4567e89b12d3a456426655440000

{
  "Offset": 0
}
```

El elemento *Offset* especifica el desfase (en unidades de 100 nanosegundos) cuando se reconoció la frase, en relación con el inicio de la secuencia de audio.

### <a name="message-turnstart"></a>Mensaje `turn.start`

`turn.start` indica el inicio de un turno desde la perspectiva del servicio. El mensaje `turn.start` es siempre el primer mensaje de respuesta que recibirá para cualquier solicitud. Si no recibe un mensaje `turn.start`, se supone que el estado de la conexión de servicio no es válido.

| Campo | DESCRIPCIÓN |
| ------------- | ---------------- |
| Codificación de mensajes de WebSocket | Text |
| Ruta de acceso | `turn.start` |
| Content-Type | application/json; charset=utf-8 |
| Cuerpo | Estructura JSON |

#### <a name="sample-message"></a>Mensaje de ejemplo

```HTML
Path: turn.start
Content-Type: application/json; charset=utf-8
X-RequestId: 123e4567e89b12d3a456426655440000

{
  "context": {
    "serviceTag": "7B33613B91714B32817815DC89633AFA"
  }
}
```

El cuerpo del mensaje `turn.start` es una estructura JSON que contiene el contexto para el inicio del turno. El elemento *context* contiene una propiedad *serviceTag*. Esta propiedad especifica un valor de etiqueta que el servicio ha asociado con el turno. Microsoft puede utilizar este valor si usted necesita ayuda para solucionar los errores en la aplicación.

### <a name="message-turnend"></a>Mensaje `turn.end`

`turn.end` indica el final de un turno desde la perspectiva del servicio. El mensaje `turn.end` es siempre el último mensaje de respuesta que recibirá para cualquier solicitud. Los clientes pueden usar la recepción de este mensaje como señal para las actividades de limpieza y realizar la transición a un estado de inactividad. Si no recibe un mensaje `turn.end`, se supone que el estado de la conexión de servicio no es válido. En esos casos, cierre la conexión existente con el servicio y vuelva a conectarse.

| Campo | DESCRIPCIÓN |
| ------------- | ---------------- |
| Codificación de mensajes de WebSocket | Text |
| Ruta de acceso | `turn.end` |
| Cuerpo | None |

#### <a name="sample-message"></a>Mensaje de ejemplo

```HTML
Path: turn.end
X-RequestId: 123e4567e89b12d3a456426655440000
```

## <a name="telemetry-schema"></a>Esquema de telemetría

El cuerpo del mensaje de *telemetría* es una estructura JSON que contiene información del cliente sobre un turno o un intento de conexión. La estructura se compone de marcas de tiempo de cliente que registran cuándo se producen eventos de cliente. Cada marca de tiempo debe estar en formato ISO 8601, tal como se describe en la sección titulada "Encabezado X-Timestamp". Los mensajes de *telemetría* que no especifican todos los campos obligatorios en la estructura JSON o que no utilizan el formato de marca de hora correcto podrían provocar que el servicio terminara la conexión al cliente. Los clientes *deben* suministrar valores válidos para todos los campos obligatorios. Los clientes *deberían* suministrar valores para los campos opcionales siempre que corresponda. Los valores mostrados en los ejemplos de esta sección únicamente tienen fines ilustrativos.

El esquema de telemetría se divide en los siguientes elementos: marcas de tiempo de mensajes recibidos y métricas. El formato y el uso de cada elemento se especifica en las secciones siguientes.

### <a name="received-message-time-stamps"></a>Marcas de tiempo de mensajes recibidos

Los clientes deben incluir valores de hora de recepción para todos los mensajes que reciben después de conectarse correctamente al servicio. Estos valores deben registrar la hora cuando el cliente *recibió* cada mensaje desde la red. El valor no debería registrar ninguna otra hora. Por ejemplo, el cliente no debería registrar la hora cuando efectuó alguna *acción* en el mensaje. Las marcas de tiempo de mensajes recibidos se especifican en una matriz de pares *nombre:valor*. El nombre del par especifica el valor *Path* del mensaje. El valor del par especifica la hora del cliente cuando se recibió el mensaje. O bien, si se recibió más de un mensaje del nombre especificado, el valor del par es una matriz de marcas de tiempo que indica cuándo se recibieron los mensajes.

```JSON
  "ReceivedMessages": [
    { "speech.hypothesis": [ "2016-08-16T15:03:48.172Z", "2016-08-16T15:03:48.331Z", "2016-08-16T15:03:48.881Z" ] },
    { "speech.endDetected": "2016-08-16T15:03:49.721Z" },
    { "speech.phrase": "2016-08-16T15:03:50.001Z" },
    { "turn.end": "2016-08-16T15:03:51.021Z" }
  ]
```

Los clientes *deben* confirmar la recepción de todos los mensajes enviados por el servicio mediante la inclusión de marcas de tiempo para los mensajes en el cuerpo JSON. Si un cliente no confirma la recepción de un mensaje, el servicio puede finalizar la conexión.

### <a name="metrics"></a>Métricas

Los clientes deben incluir información sobre los eventos que se produjeron durante la vigencia de una solicitud. Se admiten las siguientes métricas: `Connection`, `Microphone` y `ListeningTrigger`.

### <a name="metric-connection"></a>Métrica `Connection`

La métrica `Connection` especifica los detalles sobre los intentos de conexión por parte del cliente. La métrica debe incluir marcas de tiempo de cuándo se inició y finalizó la conexión de WebSocket. La métrica `Connection` es obligatoria *solo para el primer turno de una conexión*. Los turnos subsiguientes no requieren la inclusión de esta información. Si un cliente realizar varios intentos de conexión antes de establecer una conexión, debe incluirse la información sobre *todos* los intentos de conexión. Para obtener más información, consulte: [Telemetría de errores de conexión](#connection-failure-telemetry).

| Campo | DESCRIPCIÓN | Uso |
| ----- | ----------- | ----- |
| Name | `Connection` | Obligatorio |
| Id | El valor de identificador de conexión que se utilizó en el encabezado *X-ConnectionId* para esta solicitud de conexión | Obligatorio |
| Iniciar | La hora en que el cliente envió la solicitud de conexión | Obligatorio |
| End | La hora en que el cliente recibió la notificación de que la conexión se estableció correctamente o, en casos de error, se rechazó, negó o falló | Obligatorio |
| Error | Una descripción del error producido, si lo hay. Si la conexión se estableció correctamente, los clientes deben omitir este campo. La longitud máxima de este campo es de 50 caracteres. | Obligatorio para los casos de error; en caso contrario, se omite |

La descripción del error debe tener 50 caracteres como máximo y lo ideal es que debe ser uno de los valores enumerados en la tabla siguiente. Si la condición de error no coincide con uno de estos valores, los clientes pueden usar una breve descripción de la condición de error usando [mayúsculas intercaladas](https://en.wikipedia.org/wiki/Camel_case) (CamelCasing) sin espacios en blanco. La capacidad de enviar un mensaje de *telemetría* requiere una conexión al servicio, por lo que solo pueden informarse las condiciones de error transitorias o temporales en el mensaje de *telemetría*. Las condiciones de error que impiden de manera *permanente* que un cliente establezca una conexión con el servicio impiden que el cliente envíe los mensajes al servicio, incluidos los mensajes de *telemetría*.

| Error | Uso |
| ----- | ----- |
| DNSfailure | El cliente no pudo conectarse al servicio debido a un error de DNS en la pila de red. |
| NoNetwork | El cliente intentó establecer una conexión, pero la pila de red informó que no había disponible ninguna red física. |
| NoAuthorization | No se pudo establecer la conexión de cliente al intentar adquirir un token de autorización para la conexión. |
| NoResources | El cliente se quedó sin algún recurso local (por ejemplo, memoria) al intentar establecer una conexión. |
| Prohibido | El cliente no pudo conectarse al servicio porque el servicio devolvió un código de estado HTTP `403 Forbidden` en la solicitud de actualización de WebSocket. |
| No autorizado | El cliente no pudo conectarse al servicio porque el servicio devolvió un código de estado HTTP `401 Unauthorized` en la solicitud de actualización de WebSocket. |
| BadRequest | El cliente no pudo conectarse al servicio porque el servicio devolvió un código de estado HTTP `400 Bad Request` en la solicitud de actualización de WebSocket. |
| ServerUnavailable | El cliente no pudo conectarse al servicio porque el servicio devolvió un código de estado HTTP `503 Server Unavailable` en la solicitud de actualización de WebSocket. |
| ServerError | El cliente no pudo conectarse al servicio porque el servicio devolvió un código de estado de error interno `HTTP 500` en la solicitud de actualización de WebSocket. |
| Tiempo de espera | La solicitud de conexión del cliente agotó el tiempo de espera sin una respuesta del servicio. El campo *End* contiene la hora en que el cliente agotó el tiempo de espera y dejó de esperar la conexión. |
| ClientError | El cliente finalizó la conexión debido a un error interno del cliente. |

### <a name="metric-microphone"></a>Métrica `Microphone`

La métrica `Microphone` es obligatoria para todos los turnos de voz. Esta métrica mide el tiempo en el cliente durante el cual la entrada de audio se usó de manera activa para una solicitud de voz.

Utilice los siguientes ejemplos como guía para los valores de tiempo *Start* de la grabación para la métrica `Microphone` en la aplicación cliente:

* Una aplicación cliente requiere que un usuario presione un botón físico para iniciar el micrófono. Después de presionar el botón, la aplicación cliente lee la entrada del micrófono y la envía a Servicio de voz. El valor *Start* de la métrica `Microphone` registra el tiempo después de la pulsación del botón cuando el micrófono está inicializado y listo para proporcionar la entrada. El valor *End* para la métrica `Microphone` registra la hora cuando la aplicación cliente detuvo la transmisión de la secuencia de audio al servicio después de haber recibido el mensaje `speech.endDetected` del servicio.

* Una aplicación cliente usa un observador de palabras clave que está escuchando "siempre". Solo una vez que el observador de palabras clave detecta una frase desencadenante hablada, la aplicación cliente recopila la entrada del micrófono y la envía a Servicio de voz. El valor *Start* para la métrica `Microphone` registra la hora cuando el observador de palabras clave notificó al cliente que empezara a usar la entrada del micrófono. El valor *End* para la métrica `Microphone` registra la hora cuando la aplicación cliente detuvo la transmisión de la secuencia de audio al servicio después de haber recibido el mensaje `speech.endDetected` del servicio.

* Una aplicación cliente tiene acceso a una secuencia de audio constante y realiza la detección de silencio/voz en esa secuencia de audio en un *módulo de detección de voz*. El valor *Start* para la métrica `Microphone` registra la hora cuando el *módulo de detección de voz* notificó al cliente que empezara a usar la entrada de la secuencia de audio. El valor *End* para la métrica `Microphone` registra la hora cuando la aplicación cliente detuvo la transmisión de audio al servicio después de haber recibido el mensaje `speech.endDetected` del servicio.

* Una aplicación cliente procesa el segundo turno de una solicitud de varios turnos y recibe un mensaje de respuesta de servicio para activar el micrófono para recopilar la entrada del segundo turno. El valor *Start* para la métrica `Microphone` registra la hora cuando la aplicación cliente habilita el micrófono y empieza a usar la entrada del origen de audio. El valor *End* para la métrica `Microphone` registra la hora cuando la aplicación cliente detuvo la transmisión de audio al servicio después de haber recibido el mensaje `speech.endDetected` del servicio.

El valor de hora *End* para la métrica `Microphone` registra la hora cuando la aplicación cliente detuvo la transmisión de la entrada de audio. En la mayoría de los casos, este evento se produce poco después de que el cliente recibe el mensaje `speech.endDetected` del servicio. Puede que las aplicaciones cliente comprueben el correcto cumplimiento con el protocolo; para ello, se aseguran de que el valor de hora *End* para la métrica `Microphone` se produce después del valor de hora de recepción del mensaje `speech.endDetected`. Y, dado que normalmente existe un retraso entre el final de un turno y el comienzo de otro turno, los clientes pueden comprobar el cumplimiento del protocolo al asegurarse de que la hora de *Start* de la métrica `Microphone` para cualquier turno posterior registra correctamente la hora a la que el cliente *comenzó* a usar el micrófono para transmitir la entrada de audio al servicio.

| Campo | DESCRIPCIÓN | Uso |
| ----- | ----------- | ----- |
| Name | Micrófono | Obligatorio |
| Iniciar | La hora en el cliente se comenzó a usar la entrada de audio del micrófono u otra secuencia de audio, o recibió un desencadenador del observador de palabras clave | Obligatorio |
| End | La hora en que el cliente dejó de usar el micrófono o la secuencia de audio | Obligatorio |
| Error | Una descripción del error producido, si lo hay. Si las operaciones de micrófono obtuvieron resultados satisfactorios, los clientes deben omitir este campo. La longitud máxima de este campo es de 50 caracteres. | Obligatorio para los casos de error; en caso contrario, se omite |

### <a name="metric-listeningtrigger"></a>Métrica `ListeningTrigger`
La métrica `ListeningTrigger` mide la hora cuando el usuario ejecuta la acción que inicia la entrada de voz. La métrica `ListeningTrigger` es opcional, pero se recomienda que los clientes que pueden proporcionar esta métrica lo hagan.

Utilice los siguientes ejemplos como guía para los valores de tiempo *Start* y *End* de la grabación para la métrica `ListeningTrigger` en la aplicación cliente.

* Una aplicación cliente requiere que un usuario presione un botón físico para iniciar el micrófono. El valor *Start* de esta métrica registra la hora de la pulsación del botón. El valor *End* registra la hora cuando finaliza la pulsación del botón.

* Una aplicación cliente usa un observador de palabras clave que está escuchando "siempre". Una vez que el observador de palabras clave detecta una frase desencadenante hablada, la aplicación cliente lee la entrada del micrófono y la envía a Servicio de voz. El valor *Start* de esta métrica registra la hora a la que el observador de palabras clave recibió audio que luego se detectó como la frase desencadenante. El valor *End* registra la hora cuando el usuario pronunció la última palabra de la frase desencadenante.

* Una aplicación cliente tiene acceso a una secuencia de audio constante y realiza la detección de silencio/voz en esa secuencia de audio en un *módulo de detección de voz*. El valor *Start* de esta métrica registra la hora a la que el *módulo de detección de voz* recibió audio que luego se detectó como voz. El valor *End* registra la hora cuando el *módulo de detección de voz* detectó voz.

* Una aplicación cliente procesa el segundo turno de una solicitud de varios turnos y recibe un mensaje de respuesta de servicio para activar el micrófono para recopilar la entrada del segundo turno. La aplicación cliente *no* debe incluir una métrica `ListeningTrigger` para este turno.

| Campo | DESCRIPCIÓN | Uso |
| ----- | ----------- | ----- |
| Name | ListeningTrigger | Opcional |
| Iniciar | La hora de inicio del desencadenador de escucha del cliente | Obligatorio |
| End | La hora final del desencadenador de escucha del cliente | Obligatorio |
| Error | Una descripción del error producido, si lo hay. Si la operación de desencadenador se estableció correctamente, los clientes deben omitir este campo. La longitud máxima de este campo es de 50 caracteres. | Obligatorio para los casos de error; en caso contrario, se omite |

#### <a name="sample-message"></a>Mensaje de ejemplo

En el ejemplo siguiente muestra un mensaje de telemetría con elementos ReceivedMessages y Metrics:

```HTML
Path: telemetry
Content-Type: application/json; charset=utf-8
X-RequestId: 123e4567e89b12d3a456426655440000
X-Timestamp: 2016-08-16T15:03:54.183Z

{
  "ReceivedMessages": [
    { "speech.hypothesis": [ "2016-08-16T15:03:48.171Z", "2016-08-16T15:03:48.331Z", "2016-08-16T15:03:48.881Z" ] },
    { "speech.endDetected": "2016-08-16T15:03:49.721Z" },
    { "speech.phrase": "2016-08-16T15:03:50.001Z" },
    { "turn.end": "2016-08-16T15:03:51.021Z" }
  ],
  "Metrics": [
    {
      "Name": "Connection",
      "Id": "A140CAF92F71469FA41C72C7B5849253",
      "Start": "2016-08-16T15:03:47.921Z",
      "End": "2016-08-16T15:03:48.000Z",
    },
    {
      "Name": "ListeningTrigger",
      "Start": "2016-08-16T15:03:48.776Z",
      "End": "2016-08-16T15:03:48.777Z",
    },
    {
      "Name": "Microphone",
      "Start": "2016-08-16T15:03:47.921Z",
      "End": "2016-08-16T15:03:51.921Z",
    },
  ],
}
```

## <a name="error-handling"></a>Control de errores

En esta sección se describen los tipos de mensajes de error y las condiciones que la aplicación debe controlar.

### <a name="http-status-codes"></a>Códigos de estado HTTP

Durante la solicitud de actualización de WebSocket, Servicio de voz puede devolver cualquiera de los códigos de estado HTTP estándar, como `400 Bad Request`, etc. La aplicación debe administrar correctamente estas condiciones de error.

#### <a name="authorization-errors"></a>Errores de autorización

Si se proporciona una autorización incorrecta durante la actualización de WebSocket, Servicio de voz devuelve un código de estado HTTP `403 Forbidden`. Entre las condiciones que pueden desencadenar este código de error se encuentran:

* Falta el encabezado *Authorization*

* Token de autorización no válido

* Token de autorización caducado

El mensaje de error `403 Forbidden` no indica un problema con Servicio de voz. Este mensaje de error indica un problema con la aplicación cliente.

### <a name="protocol-violation-errors"></a>Errores de infracción de protocolo

Si Servicio de voz detecta cualquier infracción del protocolo de parte de un cliente, el servicio finaliza la conexión de WebSocket después de devolver un *código de estado* y un *motivo* para la finalización. Las aplicaciones cliente pueden utilizar esta información para solucionar y corregir las infracciones.

#### <a name="incorrect-message-format"></a>Formato de mensaje incorrecto

Si un cliente envía un mensaje binario o de texto al servicio y no está codificado en el formato correcto indicado esta especificación, el servicio cierra la conexión con un código de estado *1007 Invalid Payload Data* (Datos de carga no válidos).

El servicio devuelve un código de estado para una variedad de motivos, como se muestra en los ejemplos siguientes:

* "Incorrect message format. Binary message has invalid header size prefix". El cliente envió un mensaje binario que tiene un prefijo de tamaño de encabezado no válido.

* "Incorrect message format. Binary message has invalid header size". El cliente envió un mensaje binario que especificó un tamaño de encabezado no válido.

* "Incorrect message format. Binary message headers decoding into UTF-8 failed". El cliente envió un mensaje binario que contiene encabezados que no se habían codificado correctamente en UTF-8.

* "Incorrect message format. Text message contains no data". El cliente envió un mensaje de texto que no contiene ningún dato de cuerpo.

* "Incorrect message format. Text message decoding into UTF-8 failed." El cliente envió un mensaje de texto que no se codificó correctamente en UTF-8.

* "Incorrect message format. Text message contains no header separator". El cliente envió un mensaje de texto que no contenía un separador de encabezado o utilizaba el separador de encabezado incorrecto.

#### <a name="missing-or-empty-headers"></a>Faltan encabezados o están vacíos

Si un cliente envía un mensaje que no tiene los encabezados obligatorios *X-RequestId* o *Path*, el servicio cierra la conexión con un código de estado *1002 Protocol Error* (Error de protocolo). El mensaje es "Missing/Empty header. {Header name}" (Falta el encabezado o está vacío. {Nombre del encabezado}).

#### <a name="requestid-values"></a>Valores de RequestId

Si un cliente envía un mensaje que especifica un encabezado *X-RequestId* con un formato incorrecto, el servicio cierra la conexión y devuelve un estado *1002 Protocol Error* (Error de protocolo). El mensaje es "Invalid request. X-RequestId header value was not specified in no-dash UUID format" (Solicitud no válida. El valor de encabezado X-RequestId no se especificó en formato no-dash de UUID).

#### <a name="audio-encoding-errors"></a>Errores de codificación de audio

Si un cliente envía un fragmento de audio que inicia un turno, y el formato o la codificación de audio no cumplen con la especificación necesaria, el servicio se cierra la conexión y devuelve un código de estado *1007 Invalid Payload Data* (Datos de carga no válidos). El mensaje indica el origen del error de codificación de formato.

#### <a name="requestid-reuse"></a>Reutilización de RequestId

Una vez finalizado un turno, si un cliente envía un mensaje que reutiliza el identificador de la solicitud del turno, el servicio cierra la conexión y devuelve un código de estado *1002 Protocol Error* (Error de protocolo). El mensaje es "Invalid request. Reutilización de identificadores de solicitud no está permitido" (Solicitud no válida. No se permite reutilizar identificadores de solicitud).

## <a name="connection-failure-telemetry"></a>Telemetría de errores de conexión

Para garantizar la mejor experiencia de usuario posible, los clientes deben informar a Servicio de voz de las marcas de tiempo para los puntos de control importantes dentro de una conexión mediante mensajes de *telemetría*. Es igualmente importante que los clientes informen al servicio sobre las conexiones que se intentaron establecer, pero no se pudieron establecer.

Por cada intento de conexión que no se pudo establecer, los clientes crean un mensaje de *telemetría* con un único valor de encabezado *X-RequestId*. Dado que el cliente no pudo establecer una conexión, el campo *ReceivedMessages* se puede omitir en el cuerpo JSON. Solo se incluye la entrada `Connection` en el campo *Metrics*. Esta entrada incluye las marcas de tiempo de inicio y final, así como la condición de error que se encontró.

### <a name="connection-retries-in-telemetry"></a>Reintentos de conexión en telemetría

Los clientes deben distinguir entre *reintentos* y *varios intentos de conexión* por el evento que desencadena el intento de conexión. Los intentos de conexión que se llevan a cabo mediante programación sin ninguna intervención del usuario son reintentos. Varios intentos de conexión que se llevan a cabo en respuesta a la entrada del usuario son varios intentos de conexión. Los clientes dan a cada intento de conexión activado por el usuario un *X-RequestId* exclusivo y un mensaje de *telemetría*. Los clientes reutilizan *X RequestId* para los reintentos mediante programación. Si se probaron varios reintentos para un único intento de conexión, cada intento de reintento se incluye como una entrada `Connection` en el mensaje de *telemetría*.

Por ejemplo, suponga que un usuario pronuncia el desencadenador de palabra clave para iniciar una conexión y no es posible establecer el primer intento de conexión debido a errores de DNS. Sin embargo, un segundo intento que el cliente realiza mediante programación se establece correctamente. Dado que el cliente reintentó la conexión sin necesidad de intervención adicional del usuario, el cliente utiliza un solo mensaje de *telemetría* con varias entradas `Connection` para describir la conexión.

Como otro ejemplo, suponga que un usuario pronuncia el desencadenador de palabra clave para iniciar una conexión y no es posible establecer la conexión después de tres reintentos. El cliente desiste, deja de intentar conectarse al servicio e informa al usuario que se produjo un error. A continuación, el usuario vuelve a pronunciar el desencadenador de palabra clave. En esta ocasión, suponga que el cliente se conecta al servicio. Después de conectarse, el cliente envía inmediatamente un mensaje de *telemetría* al servicio, que contiene tres entradas `Connection` que describen los errores de conexión. Después de recibir el mensaje `turn.end`, el cliente envía otro mensaje de *telemetría* que describe la conexión correcta.

## <a name="error-message-reference"></a>Referencia de mensajes de error

### <a name="http-status-codes"></a>Códigos de estado HTTP

| Código de estado HTTP | DESCRIPCIÓN | solución de problemas |
| - | - | - |
| 400 - Solicitud incorrecta | El cliente envió una solicitud de conexión de WebSocket que era incorrecta. | Compruebe haber proporcionado todos los parámetros y encabezados HTTP obligatorios, y que los valores son correctos. |
| 401 No autorizado | El cliente no incluyó la información de autorización obligatoria. | Compruebe que envía el encabezado de autorización *Authorization* en la conexión de WebSocket. |
| 403 Prohibido | El cliente envió información de autorización, pero no era válida. | Compruebe que no está enviando un valor caducado o no válido en el encabezado *Authorization*. |
| 404 No encontrado | El cliente intentó tener acceso a una ruta de acceso de dirección URL que no es compatible. | Compruebe que está usando la dirección URL correcta para la conexión de WebSocket. |
| 500 Error de servidor | El servicio encontró un error interno y no pudo satisfacer la solicitud. | En la mayoría de los casos, este error es transitorio. Vuelva a intentarlo. |
| Servicio no disponible 503 | El servicio no estaba disponible para atender la solicitud. | En la mayoría de los casos, este error es transitorio. Vuelva a intentarlo. |

### <a name="websocket-error-codes"></a>Códigos de error de WebSocket

| Código WebSocketsStatus | DESCRIPCIÓN | solución de problemas |
| - | - | - |
| 1000 Normal Closure (Cierre normal) | El servicio cerró la conexión de WebSocket sin indicar un error. | Si el cierre de WebSocket fue inesperado, vuelva a leer la documentación para asegurarse de comprender cómo y cuándo el servicio puede finalizar la conexión de WebSocket. |
| 1002 Protocol Error (Error de protocolo) | El cliente no pudo cumplir con requisitos del protocolo. | Asegúrese de entender la documentación del protocolo y tener claros los requisitos. Lea la documentación anterior acerca de los motivos del error para ver si está infringiendo requisitos del protocolo. |
| 1007 Invalid Payload Data (Datos de carga no válidos) | El cliente envió una carga no válido en un mensaje de protocolo. | Compruebe el último mensaje enviado al servicio para ver los errores. Lea la documentación anterior acerca de los errores de carga. |
| 1011 (Server Error) Error de servidor | El servicio encontró un error interno y no pudo satisfacer la solicitud. | En la mayoría de los casos, este error es transitorio. Vuelva a intentarlo. |

## <a name="related-topics"></a>Temas relacionados

Vea un [SDK de JavaScript](https://github.com/Azure-Samples/SpeechToText-WebSockets-Javascript) que es una implementación del protocolo de Servicio de voz basado en WebSocket.
