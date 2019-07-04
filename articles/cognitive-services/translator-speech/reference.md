---
title: Referencia de Translator Speech API
titleSuffix: Azure Cognitive Services
description: Documentación de referencia para Translator Speech API.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-speech
ms.topic: reference
ms.date: 05/18/2018
ms.author: swmachan
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 0f083a6ca3079128aad4aba3a53013df378a6106
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/28/2019
ms.locfileid: "67446896"
---
# <a name="translator-speech-api"></a>Translator Speech API

[!INCLUDE [Deprecation note](../../../includes/cognitive-services-translator-speech-deprecation-note.md)]

Este servicio ofrece una API de transmisión por secuencias para transcribir voz conversacional de un idioma en texto de otro idioma. La API también integra las funcionalidades de texto a voz para volver a hablar el texto traducido. Translator Speech API permite escenarios como la traducción de conversaciones en tiempo real, tal como se muestra en Skype Translator.

Con Translator Speech API, las aplicaciones cliente transmiten audio de voz al servicio y reciben de vuelta una secuencia de resultados de texto que incluyen el texto reconocido en el idioma de origen y su traducción en el idioma de destino. Para producir los resultados, se aplica el reconocimiento de voz automático (ASR) con la tecnología de redes neuronales profundas a la secuencia de audio entrante. La salida de ASR sin formato se mejora aún más mediante una técnica nueva denominada TrueText para reflejar mejor la intención del usuario. Por ejemplo, TrueText quita las faltas de fluidez (murmullos y toses) y restaura correctamente la puntuación y las mayúsculas y minúsculas. También se incluye la capacidad de enmascarar o excluir las palabras soeces. Los motores de reconocimiento y traducción están entrenados específicamente para controlar la voz conversacional. El servicio de traducción de voz utiliza la detección de silencios para determinar el final de una expresión. Después de una pausa en la actividad de voz, el servicio devuelve el resultado final de la expresión completa. El servicio también puede devolver resultados parciales volver, lo que proporcionará traducciones y reconocimientos intermedios de una expresión en curso. Para obtener resultados finales, el servicio proporciona la capacidad de sintetizar voz (texto a voz) del texto hablado en los idiomas de destino. El audio de texto a voz se crea en el formato especificado por el cliente. Están disponibles los formatos WAV y MP3.

Translator Speech API aprovecha el protocolo WebSocket para proporcionar un canal de comunicación dúplex completo entre el cliente y el servidor. Una aplicación requerirá estos pasos para usar el servicio:

## <a name="1-getting-started"></a>1. Introducción
Para acceder a Translator Text API, debe [suscribirse a Microsoft Azure](translator-speech-how-to-signup.md).

## <a name="2-authentication"></a>2. Authentication

Use la clave de suscripción para realizar la autenticación. Translator Speech API admite dos modos de autenticación:

* **Con un token de acceso:** en la aplicación, obtenga un token de acceso desde el servicio de token. Use la clave de suscripción de Translator Speech API para obtener un token de acceso del servicio de autenticación de Azure Cognitive Services. Cada token de acceso tiene una validez de 10 minutos. Obtenga un nuevo token de acceso cada 10 minutos y siga usando el mismo para solicitudes repetidas durante esos 10 minutos.

* **Con una clave de suscripción directamente:** en la aplicación, pase la clave de suscripción como un valor del encabezado `Ocp-Apim-Subscription-Key`.

Trate la clave de suscripción y el token de acceso como secretos que deben ocultarse.

## <a name="3-query-languages"></a>3. Lenguajes de consulta
**Consulte los recursos de idiomas del conjunto actual de idiomas admitidos.** Los [recursos de idiomas](languages-reference.md) muestran el conjunto de idiomas y voces disponibles para el reconocimiento de voz, la traducción de texto y texto a voz. Cada idioma o voz recibe un identificador que Translator Speech API usa para identificarlos.

## <a name="4-stream-audio"></a>4. Secuencia de audio
**Abra una conexión y empiece a transmitir audio al servicio.** La dirección URL de servicio es `wss://dev.microsofttranslator.com/speech/translate`. Los formatos de audio y parámetros esperados por el servicio se describen a continuación, en la operación `/speech/translate`. Uno de los parámetros se utiliza para pasar el token de acceso del paso 2 anterior.

## <a name="5-process-the-results"></a>5. Procesamiento de los resultados
**Procese los resultados transmitidos desde el servicio.** El formato de los segmentos de audio de texto a voz, los resultados parciales y los resultados finales se describen en la documentación de la operación `/speech/translate` que se indica a continuación.

Los ejemplos de código que demuestran el uso de Translator Speech API están disponibles en el [sitio de GitHub de Microsoft Translator](https://github.com/MicrosoftTranslator).

## <a name="implementation-notes"></a>Notas de implementación

GET /speech/translate establece una sesión para la traducción de voz.

### <a name="connecting"></a>Conexión
Antes de conectarse al servicio, revise la lista de parámetros que se proporcionan más adelante en esta sección. A continuación, se muestra una solicitud de ejemplo:

`GET wss://dev.microsofttranslator.com/speech/translate?from=en-US&to=it-IT&features=texttospeech&voice=it-IT-Elsa&api-version=1.0`
`Ocp-Apim-Subscription-Key: {subscription key}`
`X-ClientTraceId: {GUID}`

La solicitud especifica que el inglés hablado se transmitirá al servicio y se traducirá al italiano. Cada resultado de reconocimiento final generará una respuesta de audio de texto a voz con una voz femenina denominada Elsa. Tenga en cuenta que la solicitud incluye las credenciales en el `Ocp-Apim-Subscription-Key header`. La solicitud también sigue un procedimiento recomendado estableciendo un identificador único global en el encabezado `X-ClientTraceId`. Una aplicación cliente debe registrar el identificador de seguimiento para que pueda utilizarse para solucionar problemas cuando se produzcan.

### <a name="sending-audio"></a>Envío de audio
Una vez establecida la conexión, el cliente comienza a transmitir audio al servicio. El cliente envía audio en fragmentos. Cada fragmento se transmite mediante un mensaje de Websocket del tipo Binary.

La entrada de audio está en el formato de archivo de audio de forma de onda (WAVE o, más comúnmente conocido como WAV, debido a su extensión de nombre de archivo). La aplicación cliente debe transmitir un audio de PCM firmado de 16 bits de canal único muestreado a 16 kHz. El primer conjunto de bytes que transmita el cliente incluirá el encabezado WAV. Un encabezado de 44 bytes para una secuencia de PCM firmada de 16 bits firmada de canal único muestreada a 16 kHz es:

|Offset|Valor|
|:---|:---|
|0 - 3|"RIFF"|
|4 - 7|0|
|8 - 11|"WAVE"|
|12 - 15|"fmt"|
|16 - 19|16|
|20 - 21|1|
|22 - 23|1|
|24 - 27|16000|
|28 - 31|32000|
|32 - 33|2|
|34 - 35|16|
|36 - 39|"data"|
|40 - 43|0|

Tenga en cuenta que el tamaño total del archivo (de 4 a 7 bytes) y el tamaño de los "datos" (de 40 a 43 bytes) se establecen en cero. Esto es correcto para el escenario de transmisión en el que el tamaño total no se conoce necesariamente por adelantado.

Después de enviar el encabezado WAV (RIFF), el cliente envía fragmentos de los datos de audio. El cliente normalmente transmite fragmentos de tamaño fijo que representan una duración fija (por ejemplo, secuencia de audio de 100 ms cada vez).

### <a name="signal-the-end-of-the-utterance"></a>Indicación del final de la expresión
Translator Speech API devuelve la transcripción y la traducción de la transmisión de la secuencia de audio según se envía. La transcripción final, la traducción final y la secuencia de audio traducida se le devolverán una vez finalizada la expresión. En algunos caso podría querer forzar el final de la expresión. Para ello, envíe 2,5 segundos de silencio.

### <a name="final-result"></a>Resultado final
Se genera un resultado final de reconocimiento de voz al final de una expresión. Se transmite un resultado desde el servicio al cliente mediante un mensaje de WebSocket de tipo texto. El contenido del mensaje es la serialización JSON de un objeto con las siguientes propiedades:

* `type`: constante de cadena para identificar el tipo de resultado. El valor es el final para los resultados finales.
* `id`: identificador de cadena asignado al resultado del reconocimiento.
* `recognition`: texto reconocido en el idioma de origen. El texto puede ser una cadena vacía en el caso de un reconocimiento falso.
* `translation`: texto reconocido traducido en el idioma de destino.
* `audioTimeOffset`: desfase horario del inicio del reconocimiento en tics (1 tic = 100 nanosegundos). El desfase es relativo al principio de la transmisión.
* `audioTimeSize`: duración en tics (100 nanosegundos) del reconocimiento.
* `audioStreamPosition`: desfase de bytes del principio del reconocimiento. El desfase es relativo al principio de la secuencia.
* `audioSizeBytes`: tamaño en bytes del reconocimiento.

Tenga en cuenta que la posición del reconocimiento de la secuencia de audio no se incluye en los resultados de forma predeterminada. El cliente debe seleccionar la característica `TimingInfo` (consulte el parámetro `features`).

A continuación, se muestra un ejemplo de resultado final:

```
{
  type: "final"
  id: "23",
  recognition: "what was said",
  translation: "translation of what was said",
  audioStreamPosition: 319680,
  audioSizeBytes: 35840,
  audioTimeOffset: 2731600000,
  audioTimeSize: 21900000
}
```

### <a name="partial-result"></a>Resultado parcial
Los resultados del reconocimiento de voz parciales o intermedios no se transmiten al cliente de forma predeterminada. El cliente puede utilizar el parámetro de consulta de características para solicitarlos.

Un resultado parcial se transmite desde el servicio al cliente mediante un mensaje de WebSocket de tipo texto. El contenido del mensaje es la serialización JSON de un objeto con las siguientes propiedades:

* `type`: constante de cadena para identificar el tipo de resultado. El valor es parcial para los resultados parciales.
* `id`: identificador de cadena asignado al resultado del reconocimiento.
* `recognition`: texto reconocido en el idioma de origen.
* `translation`: texto reconocido traducido en el idioma de destino.
* `audioTimeOffset`: desfase horario del inicio del reconocimiento en tics (1 tic = 100 nanosegundos). El desfase es relativo al principio de la transmisión.
* `audioTimeSize`: duración en tics (100 nanosegundos) del reconocimiento.
* `audioStreamPosition`: desfase de bytes del principio del reconocimiento. El desfase es relativo al principio de la secuencia.
* `audioSizeBytes`: tamaño en bytes del reconocimiento.

Tenga en cuenta que la posición del reconocimiento de la secuencia de audio no se incluye en los resultados de forma predeterminada. El cliente debe seleccionar la característica TimingInfo (consulte el parámetro de características).

A continuación, se muestra un ejemplo de resultado final:

```
{
  type: "partial"
  id: "23.2",
  recognition: "what was",
  translation: "translation of what was",
  audioStreamPosition: 319680,
  audioSizeBytes: 25840,
  audioTimeOffset: 2731600000,
  audioTimeSize: 11900000
}
```

### <a name="text-to-speech"></a>Texto a voz
Cuando se habilita la característica de texto a voz (consulte el parámetro `features` más adelante), al resultado final le sigue el audio del texto traducido hablado. Los datos de audio están fragmentados y se envían desde el servicio al cliente como una secuencia de mensajes de Websocket de tipo binario. Un cliente puede detectar el final de la secuencia comprobando el bit FIN de cada mensaje. El último mensaje binario tendrá su bit FIN establecido en uno para indicar el final de la secuencia. El formato de la secuencia depende del valor del parámetro `format`.

### <a name="closing-the-connection"></a>Cierre de la conexión
Cuando una aplicación cliente ha terminado de transmitir el audio y ha recibido el último resultado final, debe cerrar la conexión mediante el inicio del protocolo de enlace de cierre de WebSocket. Hay condiciones que provocan que el servidor finalice la conexión. El cliente puede recibir los siguientes códigos de cierre de WebSocket:

* `1003 - Invalid Message Type`: el servidor finaliza la conexión porque no puede aceptar el tipo de datos que recibió. Esto suele ocurrir cuando el audio de entrada no inicia con un encabezado adecuado.
* `1000 - Normal closure`: la conexión se cerró después de llevar a cabo la solicitud. El servidor cerrará la conexión: cuando no se reciba ningún audio del cliente durante un largo período de tiempo; cuando se transmita silencio durante un largo período de tiempo; cuando una sesión alcance la duración máxima permitida (aproximadamente 90 minutos).
* `1001 - Endpoint Unavailable`: indica que el servidor dejará de estar disponible. La aplicación cliente puede intentar volver a conectarse con un límite de número de reintentos.
* `1011 - Internal Server Error`: el servidor cerrará la conexión debido a un error en el servidor.

### <a name="parameters"></a>Parámetros

|Parámetro|Valor|DESCRIPCIÓN|Tipo de parámetro|Tipo de datos|
|:---|:---|:---|:---|:---|
|api-version|1.0|Versión de la API que el cliente solicitó. Los valores permitidos son: `1.0`.|query   |string|
|De|(vacío)   |Especifica el idioma de la voz de entrada. El valor es uno de los identificadores de idioma del ámbito `speech` de la respuesta de la API de idiomas.|query|string|
|to|(vacío)|Especifica el idioma al que se va a traducir el texto transcrito. El valor es uno de los identificadores de idioma del ámbito `text` de la respuesta de la API de idiomas.|query|string|
|features|(vacío)   |Conjunto separados por comas de las características seleccionadas por el cliente. Las características disponibles son:<ul><li>`TextToSpeech`: especifica que el servicio debe devolver el audio traducido de la última oración traducida.</li><li>`Partial`: especifica que el servicio debe devolver los resultados intermedios del reconocimiento mientras el audio transmite al servicio.</li><li>`TimingInfo`: especifica que el servicio debe devolver información de tiempo asociada a cada reconocimiento.</li></ul>Por ejemplo, un cliente debe especificar `features=partial,texttospeech` para recibir los resultados parciales y texto a voz, pero no información de tiempo. Tenga en cuenta que los resultados finales siempre se transmiten al cliente.|query|string|
|voice|(vacío)|Identifica qué voz utilizar para la representación de texto a voz del texto traducido. El valor es uno de los identificadores de voz del ámbito de tts de la respuesta de la API de idiomas. Si no se especifica una voz, el sistema elegirá una automáticamente cuando esté habilitada la característica de texto a voz.|query|string|
|format|(vacío)|Especifica el formato de la secuencia de audio de texto a voz devuelta por el servicio. Las opciones disponibles son la siguientes:<ul><li>`audio/wav`: secuencia de audio Waveform. El cliente debe utilizar el encabezado WAV para interpretar correctamente el formato de audio. El audio WAV para texto a voz es de PCM de canal único de 16 bits con una velocidad de muestreo de 24 kHz o 16 kHz.</li><li>`audio/mp3`: secuencia de audio MP3.</li></ul>El valor predeterminado es `audio/wav`.|query|string|
|ProfanityAction    |(vacío)    |Especifica cómo el servicio debe tratar las palabras soeces reconocidas en la voz. Las acciones válidas son las siguientes:<ul><li>`NoAction`: las palabras soeces se dejan tal cual.</li><li>`Marked`: las palabras soeces se reemplazan por un marcador. Consulte el parámetro `ProfanityMarker`.</li><li>`Deleted`: las palabras soeces se eliminan. Por ejemplo, si la palabra `"jackass"` se trata como una palabra soez, la frase `"He is a jackass."` se convertirá en `"He is a .".`</li></ul>El valor predeterminado es Marcado.|query|string|
|ProfanityMarker|(vacío)    |Especifica cómo se tratan las palabras soeces detectadas cuando `ProfanityAction` se establece en `Marked`. Las opciones válidas son las siguientes:<ul><li>`Asterisk`: las palabras soeces se reemplazan por la cadena `***`. Por ejemplo, si la palabra `"jackass"` se trata como una palabra soez, la frase `"He is a jackass."` se convertirá en `"He is a ***.".`</li><li>`Tag`: las palabras soeces están rodeadas por una etiqueta XML de palabras soeces. Por ejemplo, si la palabra `"jackass"` se trata como una palabra soez, la frase `"He is a jackass."` se convertirá en `"He is a <profanity>jackass</profanity>."`.</li></ul>El valor predeterminado es `Asterisk`.|query|string|
|Authorization|(vacío)  |Especifica el valor del token de portador del cliente. Use el prefijo `Bearer` seguido del valor de `access_token` devuelto por el servicio de token de autenticación.|encabezado   |string|
|Ocp-Apim-Subscription-Key|(vacío)|Es necesario si no se ha especificado el encabezado `Authorization`.|encabezado|string|
|access_token|(vacío)   |Alternativa para pasar un token de acceso de OAuth válido. El token de portador normalmente se proporciona con el encabezado `Authorization`. Algunas bibliotecas de WebSocket no permiten que el código de cliente establezca los encabezados. En este caso, el cliente puede utilizar el parámetro de consulta `access_token` para pasar un token válido. Al utilizar un token de acceso para realizar la autenticación, si el encabezado `Authorization` no se establece, debe establecerse `access_token`. Si se establecen el encabezado y el parámetro de consulta, se omite el parámetro de consulta. Los clientes solo deben usar un método para pasar el token.|query|string|
|subscription-key|(vacío)   |Alternativa para pasar la clave de la suscripción. Algunas bibliotecas de WebSocket no permiten que el código de cliente establezca los encabezados. En este caso, el cliente puede utilizar el parámetro de consulta `subscription-key` para pasar una clave de suscripción válida. Al utilizar una clave de suscripción para realizar la autenticación, si el encabezado `Ocp-Apim-Subscription-Key`  no se establece, se debe establecer la clave de la suscripción. Si se establecen el encabezado y el parámetro de consulta, se omite el parámetro de consulta. Los clientes solo deben usar un método para pasar la `subscription key`.|query|string|
|X-ClientTraceId    |(vacío)    |GUID generado por el cliente que se utiliza para realizar un seguimiento de una solicitud. Para una solución de problemas correcta, los clientes deben proporcionar y registrar un nuevo valor con cada solicitud.<br/>En lugar de utilizar un encabezado, este valor puede pasarse con el parámetro de consulta `X-ClientTraceId`. Si se establecen el encabezado y el parámetro de consulta, se omite el parámetro de consulta.|encabezado|string|
|X-CorrelationId|(vacío)    |Un identificador generado por el cliente utilizado para poner en correlación varios canales en una conversación. Pueden crearse varias sesiones de traducción de voz para habilitar las conversaciones entre usuarios. En tal caso, todas las sesiones de traducción de voz usan el mismo identificador de correlación para unir los canales. Esto facilita el seguimiento y diagnóstico. El identificador debe ajustarse a: `^[a-zA-Z0-9-_.]{1,64}$`<br/>En lugar de utilizar un encabezado, este valor puede pasarse con el parámetro de consulta `X-CorrelationId`. Si se establecen el encabezado y el parámetro de consulta, se omite el parámetro de consulta.|encabezado|string|
|X-ClientVersion|(vacío)    |Identifica la versión de la aplicación cliente. Ejemplo: "2.1.0.123".<br/>En lugar de utilizar un encabezado, este valor puede pasarse con el parámetro de consulta `X-ClientVersion`. Si se establecen el encabezado y el parámetro de consulta, se omite el parámetro de consulta.|encabezado|string|
|X-OsPlatform|(vacío)   |Identifica el nombre y la versión del sistema operativo que se ejecuta en la aplicación cliente. Ejemplos: "Android 5.0", "iOS 8.1.3" y "Windows 8.1".<br/>En lugar de utilizar un encabezado, este valor puede pasarse con el parámetro de consulta `X-OsPlatform`. Si se establecen el encabezado y el parámetro de consulta, se omite el parámetro de consulta.|encabezado|string|

### <a name="response-messages"></a>Mensajes de respuesta

|Código de estado HTTP|Motivo|Modelo de respuesta|encabezados|
|:--|:--|:--|:--|
|101    |Actualización de WebSocket.|Objeto de valor de ejemplo del modelo <br/> {}|X-RequestId<br/>Valor que identifica la solicitud para solucionar problemas.<br/>string|
|400    |Solicitud incorrecta. Compruebe los parámetros de entrada para asegurarse de que son válidos. El objeto de respuesta incluye una descripción más detallada del error.|||
|401    |No autorizado. Asegúrese de que se establecen las credenciales, que son válidas y que la suscripción de Azure Data Market es correcta y tiene saldo disponible.|||
|500    |Se produjo un error. Si el error persiste, notifíquelo junto con el identificador de seguimiento de cliente (X-ClientTraceId) o el identificador de solicitud (X-ID).|||
|503    |Servidor no disponible temporalmente. Vuelva a intentar realizar la solicitud. Si el error persiste, notifíquelo junto con el identificador de seguimiento de cliente (X-ClientTraceId) o el identificador de solicitud (X-ID).|||
