---
title: Text to Speech API de Servicio de voz de Microsoft | Microsoft Docs
description: Use la API de texto a voz para proporcionar una conversión de texto a voz en tiempo real en una variedad de idiomas y voces
services: cognitive-services
author: priyaravi20
manager: yanbo
ms.service: cognitive-services
ms.component: bing-speech
ms.topic: article
ms.date: 03/16/2017
ms.author: priyar
ms.openlocfilehash: 6236bf1c4d9ac7ba672939543c16b8d02a79892e
ms.sourcegitcommit: 744747d828e1ab937b0d6df358127fcf6965f8c8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41929780"
---
# <a name="bing-text-to-speech-api"></a>Bing text to speech API

## <a name="Introduction"></a>Introducción

Con Bing text to speech API, su aplicación puede enviar solicitudes HTTP a un servidor en la nube, donde el texto se sintetiza al instante en voz con un sonido humano y se devuelve como archivo de audio. Esta API se puede usar en muchos contextos diferentes para proporcionar una conversión de texto a voz en tiempo real en una variedad de idiomas y voces diferentes.

## <a name="VoiceSynReq"></a>Solicitud de síntesis de voz

### <a name="Subscription"></a>Token de autorización

Cada solicitud de síntesis de voz requiere un token de acceso JSON Web Token (JWT). El token de acceso JWT se pasa en el encabezado de la solicitud de voz. El token tiene un tiempo de expiración de 10 minutos. Para obtener información sobre cómo suscribirse y obtener claves de API que se usan para recuperar los tokens de acceso JWT válidos, consulte la página de [suscripción a Cognitive Services](https://azure.microsoft.com/try/cognitive-services/).

La clave de API se pasa al servicio de token. Por ejemplo: 

```HTTP
POST https://api.cognitive.microsoft.com/sts/v1.0/issueToken
Content-Length: 0
```

La información de encabezados requerida para el acceso del token es la siguiente.

NOMBRE| Formato | DESCRIPCIÓN
----|----|----
Ocp-Apim-Subscription-Key | ASCII | Su clave de suscripción

El servicio de token devuelve el token de acceso JWT como `text/plain`. A continuación, el JWT se pasa como `Base64 access_token` al punto de conexión de voz como un encabezado de autorización prefijado con la cadena `Bearer`. Por ejemplo: 

`Authorization: Bearer [Base64 access_token]`

Los clientes deben utilizar el siguiente punto de conexión para tener acceso al servicio de texto a voz:

`https://speech.platform.bing.com/synthesize`

>[!NOTE]
>Hasta que no haya adquirido un token de acceso con la clave de suscripción como se describió anteriormente, este vínculo genera un error de respuesta `403 Forbidden`.

### <a name="Http"></a>Encabezados HTTP

En la tabla siguiente se muestran los encabezados HTTP que se utilizan para las solicitudes de síntesis de voz.

Encabezado |Valor |Comentarios
----|----|----
Content-Type | application/ssml+xml | El tipo de contenido de entrada.
X-Microsoft-OutputFormat | **1.** ssml-16khz-16bit-mono-tts <br> **2.** raw-16khz-16bit-mono-pcm <br>**3.** audio-16khz-16kbps-mono-siren <br> **4.** riff-16khz-16kbps-mono-siren <br> **5.** riff-16khz-16bit-mono-pcm <br> **6.** audio-16khz-128kbitrate-mono-mp3 <br> **7.** audio-16khz-64kbitrate-mono-mp3 <br> **8.** audio-16khz-32kbitrate-mono-mp3 | El formato de audio de salida.
X-Search-AppId | Un GUID (solo hexadecimal, sin guiones) | Un identificador que identifica la aplicación cliente de forma exclusiva. Puede ser el identificador del almacén de aplicaciones. Si no hay uno disponible, el usuario puede generar el identificador para una aplicación.
X-Search-ClientID | Un GUID (solo hexadecimal, sin guiones) | Un identificador que identifica de forma única una instancia de la aplicación para cada instalación.
User-Agent | Nombre de la aplicación | El nombre de la aplicación es necesario y debe tener menos de 255 caracteres.
Autorización | Token de autorización |  Consulte la sección <a href="#Subscription">Token de autorización</a>.

### <a name="InputParam"></a>Parámetros de entrada

Las solicitudes a Bing text to speech API se realizan utilizando llamadas POST HTTP. Los encabezados se especifican en la sección anterior. El cuerpo contiene una entrada Speech Synthesis Markup Language (SSML) que representa el texto que se va a sintetizar. Para obtener una descripción del marcado que se usa para controlar aspectos de la voz como el idioma y sexo del orador, consulte la [especificación SSML de W3C](http://www.w3.org/TR/speech-synthesis/).

>[!NOTE]
>El tamaño máximo de la entrada SSML que se admite es de 1024 caracteres, incluidas todas las etiquetas.

###  <a name="SampleVoiceOR"></a>Ejemplo: solicitud de salida de voz

El siguiente es un ejemplo de una solicitud de salida de voz:

```HTTP
POST /synthesize
HTTP/1.1
Host: speech.platform.bing.com

X-Microsoft-OutputFormat: riff-8khz-8bit-mono-mulaw
Content-Type: application/ssml+xml
Host: speech.platform.bing.com
Content-Length: 197
Authorization: Bearer [Base64 access_token]

<speak version='1.0' xml:lang='en-US'><voice xml:lang='en-US' xml:gender='Female' name='Microsoft Server Speech Text to Speech Voice (en-US, ZiraRUS)'>Microsoft Bing Voice Output API</voice></speak>
```

## <a name="VoiceOutResponse"></a>Respuesta de salida de voz

Bing text to speech API usa POST HTTP para devolver el audio al cliente. La respuesta de API contiene la secuencia de audio y el códec, y coincide con el formato de salida solicitado. El audio devuelto para una solicitud determinada no debe superar los 15 segundos.

### <a name="SuccessfulRecResponse"></a>Ejemplo: respuesta de síntesis correcta

El código siguiente es un ejemplo de una respuesta JSON a una solicitud de síntesis de voz correcta. Los comentarios y el formato del código son para fines de este ejemplo únicamente y se omiten de la respuesta real.

```HTTP
HTTP/1.1 200 OK
Content-Length: XXX
Content-Type: audio/x-wav

Response audio payload
```

### <a name="RecFailure"></a>Ejemplo: error de síntesis

El código de ejemplo siguiente muestra una respuesta JSON a un error de consulta de síntesis de voz:

```HTTP
HTTP/1.1 400 XML parser error
Content-Type: text/xml
Content-Length: 0
```

### <a name="ErrorResponse"></a>Respuestas de error

Error | DESCRIPCIÓN
----|----
HTTP/400 Solicitud incorrecta | Falta un parámetro obligatorio, está vacío o es nulo, o bien el valor pasado a un parámetro obligatorio u opcional no es válido. Un motivo para obtener la respuesta "no válida" es pasar un valor de cadena que supera la longitud permitida. Se incluye una breve descripción del parámetro problemático.
HTTP/401 No autorizado | La solicitud no está autenticada.
HTTP/413 Entidad de solicitud demasiado grande  | La entrada SSML es mayor que lo admitido.
HTTP/502 Puerta de enlace no válida | Hay un problema relacionado con la red o un problema en el servidor.

Un ejemplo de una respuesta de error es el siguiente:

```HTTP
HTTP/1.0 400 Bad Request
Content-Length: XXX
Content-Type: text/plain; charset=UTF-8

Voice name not supported
```

## <a name="ChangeSSML"></a>Cambio de la salida de voz a través de SSML

Microsoft Text-to-Speech API admite SSML 1.0, tal como se define en W3C [Speech Synthesis Markup Language (SSML) versión 1.0](http://www.w3.org/TR/2009/REC-speech-synthesis-20090303/). En esta sección se muestran ejemplos de cómo cambiar ciertas características de la salida de voz generada, como la velocidad de habla, la pronunciación, etc., mediante etiquetas SSML.

1. Adición de una pausa

  ```
  <speak version='1.0' xmlns="http://www.w3.org/2001/10/synthesis" xml:lang='en-US'><voice  name='Microsoft Server Speech Text to Speech Voice (en-US, BenjaminRUS)'> Welcome to use Microsoft Cognitive Services <break time="100ms" /> Text-to-Speech API.</voice> </speak>
  ```

2. Cambio de la velocidad de habla

  ```
  <speak version='1.0' xmlns="http://www.w3.org/2001/10/synthesis" xml:lang='en-US'><voice  name='Microsoft Server Speech Text to Speech Voice (en-US, JessaRUS)'><prosody rate="+30.00%">Welcome to use Microsoft Cognitive Services Text-to-Speech API.</prosody></voice> </speak>
  ```

3. Pronunciación

  ```
  <speak version='1.0' xmlns="http://www.w3.org/2001/10/synthesis" xml:lang='en-US'><voice  name='Microsoft Server Speech Text to Speech Voice (en-US, JessaRUS)'> <phoneme alphabet="ipa" ph="t&#x259;mei&#x325;&#x27E;ou&#x325;"> tomato </phoneme></voice> </speak>
  ```

4. Cambio de volumen

  ```
  <speak version='1.0' xmlns="http://www.w3.org/2001/10/synthesis" xml:lang='en-US'><voice  name='Microsoft Server Speech Text to Speech Voice (en-US, JessaRUS)'><prosody volume="+20.00%">Welcome to use Microsoft Cognitive Services Text-to-Speech API.</prosody></voice> </speak>
  ```

5. Cambio de tono

  ```
  <speak version='1.0' xmlns="http://www.w3.org/2001/10/synthesis" xml:lang='en-US'><voice  name='Microsoft Server Speech Text to Speech Voice (en-US, JessaRUS)'>Welcome to use <prosody pitch="high">Microsoft Cognitive Services Text-to-Speech API.</prosody></voice> </speak>
  ```

6. Cambio de contorno de prosodia

  ```
  <speak version='1.0' xmlns="http://www.w3.org/2001/10/synthesis" xml:lang='en-US'><voice  name='Microsoft Server Speech Text to Speech Voice (en-US, JessaRUS)'><prosody contour="(80%,+20%) (90%,+30%)" >Good morning.</prosody></voice> </speak>
  ```

> [!NOTE]
> Tenga en cuenta que los datos de audio deben ser un archivo wav de 8k o 16k en el formato siguiente: **código CRC** (CRC-32): 4 bytes (DWORD) con rango válido 0x00000000 ~ 0xFFFFFFFF; **Marca de formato de audio**: 4 bytes (DWORD) con rango válido 0x00000000 ~ 0xFFFFFFFF; **Cuenta de muestras**: 4 bytes (DWORD) con rango válido 0x00000000 ~ 0x7FFFFFFF; **Tamaño de cuerpo binario**: 4 bytes (DWORD) con rango válido 0x00000000 ~ 0x7FFFFFFF; **Cuerpo binario**: n bytes.

## <a name="SampleApp"></a>Aplicación de ejemplo

Para obtener detalles de implementación, consulte la [aplicación de ejemplo de texto a voz de Visual C# .NET](https://github.com/Microsoft/Cognitive-Speech-TTS/blob/master/Samples-Http/CSharp/TTSProgram.cs).

## <a name="SupLocales"></a>Configuraciones regionales y fuentes de voz admitidas

En la tabla siguiente es identifican algunas de las configuraciones regionales admitidas y las fuentes de voz relacionadas.

Configuración regional | Sexo | Asignación de nombre de servicio
---------|--------|------------
ar-EG* | Mujer | "Microsoft Server Speech Text to Speech Voice (ar-EG, Hoda)"
ar-SA | Hombre | "Microsoft Server Speech Text to Speech Voice (ar-SA, Naayf)"
bg-BG | Hombre | "Microsoft Server Speech Text to Speech Voice (bg-BG, Ivan)"
ca-ES | Mujer | "Microsoft Server Speech Text to Speech Voice (ca-ES, HerenaRUS)"
cs-CZ | Hombre | "Microsoft Server Speech Text to Speech Voice (cs-CZ, Jakub)"
da-DK | Mujer | "Microsoft Server Speech Text to Speech Voice (da-DK, HelleRUS)"
de-AT | Hombre | "Microsoft Server Speech Text to Speech Voice (de-AT, Michael)"
de-CH | Hombre | "Microsoft Server Speech Text to Speech Voice (de-CH, Karsten)"
de-DE | Mujer | "Microsoft Server Speech Text to Speech Voice (de-DE, Hedda)"
de-DE | Mujer | "Microsoft Server Speech Text to Speech Voice (de-DE, HeddaRUS)"
de-DE | Hombre | "Microsoft Server Speech Text to Speech Voice (de-DE, Stefan, Apollo)"
el-GR | Hombre | "Microsoft Server Speech Text to Speech Voice (el-GR, Stefanos)"
en-AU | Mujer | "Microsoft Server Speech Text to Speech Voice (en-AU, Catherine)"
en-AU | Mujer | "Microsoft Server Speech Text to Speech Voice (en-AU, HayleyRUS)"
en-CA | Mujer | "Microsoft Server Speech Text to Speech Voice (en-CA, Linda)"
en-CA | Mujer | "Microsoft Server Speech Text to Speech Voice (en-CA, HeatherRUS)"
en-GB | Mujer | "Microsoft Server Speech Text to Speech Voice (en-GB, Susan, Apollo)"
en-GB | Mujer | "Microsoft Server Speech Text to Speech Voice (en-GB, HazelRUS)"
en-GB | Hombre | "Microsoft Server Speech Text to Speech Voice (en-GB, George, Apollo)"
en-IE | Hombre | "Microsoft Server Speech Text to Speech Voice (en-IE, Sean)"
en-IN | Mujer | "Microsoft Server Speech Text to Speech Voice (en-IN, Heera, Apollo)"
en-IN | Mujer | "Microsoft Server Speech Text to Speech Voice (en-IN, PriyaRUS)"
en-IN | Hombre | "Microsoft Server Speech Text to Speech Voice (en-IN, Ravi, Apollo)"
es-ES | Mujer | "Microsoft Server Speech Text to Speech Voice (en-US, ZiraRUS)"
es-ES | Mujer | "Microsoft Server Speech Text to Speech Voice (en-US, JessaRUS)"
es-ES | Hombre | "Microsoft Server Speech Text to Speech Voice (en-US, BenjaminRUS)"
es-ES | Mujer | "Microsoft Server Speech Text to Speech Voice (es-ES, Laura, Apollo)"
es-ES | Mujer | "Microsoft Server Speech Text to Speech Voice (es-ES, HelenaRUS)"
es-ES | Hombre | "Microsoft Server Speech Text to Speech Voice (es-ES, Pablo, Apollo)"
es-MX | Mujer | "Microsoft Server Speech Text to Speech Voice (es-MX, HildaRUS)"
es-MX | Hombre | "Microsoft Server Speech Text to Speech Voice (es-MX, Raul, Apollo)"
fi-FI | Mujer | "Microsoft Server Speech Text to Speech Voice (fi-FI, HeidiRUS)"
fr-CA | Mujer | "Microsoft Server Speech Text to Speech Voice (fr-CA, Caroline)"
fr-CA | Mujer | "Microsoft Server Speech Text to Speech Voice (fr-CA, HarmonieRUS)"
fr-CH | Hombre | "Microsoft Server Speech Text to Speech Voice (fr-CH, Guillaume)"
fr-FR | Mujer | "Microsoft Server Speech Text to Speech Voice (fr-FR, Julie, Apollo)"
fr-FR | Mujer | "Microsoft Server Speech Text to Speech Voice (fr-FR, HortenseRUS)"
fr-FR | Hombre | "Microsoft Server Speech Text to Speech Voice (fr-FR, Paul, Apollo)"
he-IL| Hombre| "Microsoft Server Speech Text to Speech Voice (he-IL, Asaf)"
hi-IN | Mujer | "Microsoft Server Speech Text to Speech Voice (hi-IN, Kalpana, Apollo)"
hi-IN | Mujer | "Microsoft Server Speech Text to Speech Voice (hi-IN, Kalpana)"
hi-IN | Hombre | "Microsoft Server Speech Text to Speech Voice (hi-IN, Hemant)"
hr-HR | Hombre | "Microsoft Server Speech Text to Speech Voice (hr-HR, Matej)"
hu-HU | Hombre | "Microsoft Server Speech Text to Speech Voice (hu-HU, Szabolcs)"
id-ID | Hombre | "Microsoft Server Speech Text to Speech Voice (id-ID, Andika)"
it-IT | Hombre | "Microsoft Server Speech Text to Speech Voice (it-IT, Cosimo, Apollo)"
it-IT | Mujer | "Microsoft Server Speech Text to Speech Voice (it-IT, LuciaRUS)"
ja-JP | Mujer | "Microsoft Server Speech Text to Speech Voice (ja-JP, Ayumi, Apollo)"
ja-JP | Hombre | "Microsoft Server Speech Text to Speech Voice (ja-JP, Ichiro, Apollo)"
ja-JP | Mujer | "Microsoft Server Speech Text to Speech Voice (ja-JP, HarukaRUS)"
ko-KR | Mujer | "Microsoft Server Speech Text to Speech Voice (ko-KR, HeamiRUS)"
ms-MY | Hombre | "Microsoft Server Speech Text to Speech Voice (ms-MY, Rizwan)"
nb-NO | Mujer | "Microsoft Server Speech Text to Speech Voice (nb-NO, HuldaRUS)"
nl-NL | Mujer | "Microsoft Server Speech Text to Speech Voice (nl-NL, HannaRUS)"
pl-PL | Mujer | "Microsoft Server Speech Text to Speech Voice (pl-PL, PaulinaRUS)"
pt-BR | Mujer | "Microsoft Server Speech Text to Speech Voice (pt-BR, HeloisaRUS)"
pt-BR | Hombre | "Microsoft Server Speech Text to Speech Voice (pt-BR, Daniel, Apollo)"
pt-PT | Mujer | "Microsoft Server Speech Text to Speech Voice (pt-PT, HeliaRUS)"
ro-RO | Hombre | "Microsoft Server Speech Text to Speech Voice (ro-RO, Andrei)"
ru-RU | Mujer | "Microsoft Server Speech Text to Speech Voice (ru-RU, Irina, Apollo)"
ru-RU | Hombre | "Microsoft Server Speech Text to Speech Voice (ru-RU, Pavel, Apollo)"
ru-RU | Mujer | "Microsoft Server Speech Text to Speech Voice (ru-RU, EkaterinaRUS)"
sk-SK | Hombre | "Microsoft Server Speech Text to Speech Voice (sk-SK, Filip)"
sl-SI | Hombre | "Microsoft Server Speech Text to Speech Voice (sl-SI, Lado)"
sv-SE | Mujer | "Microsoft Server Speech Text to Speech Voice (sv-SE, HedvigRUS)"
ta-IN | Hombre | "Microsoft Server Speech Text to Speech Voice (ta-IN, Valluvar)"
th-TH | Hombre | "Microsoft Server Speech Text to Speech Voice (th-TH, Pattara)"
tr-TR | Mujer | "Microsoft Server Speech Text to Speech Voice (tr-TR, SedaRUS)"
vi-VN | Hombre | "Microsoft Server Speech Text to Speech Voice (vi-VN, An)"
zh-CN | Mujer | "Microsoft Server Speech Text to Speech Voice (zh-CN, HuihuiRUS)"
zh-CN | Mujer | "Microsoft Server Speech Text to Speech Voice (zh-CN, Yaoyao, Apollo)"
zh-CN | Hombre | "Microsoft Server Speech Text to Speech Voice (zh-CN, Kangkang, Apollo)"
zh-HK | Mujer | "Microsoft Server Speech Text to Speech Voice (zh-HK, Tracy, Apollo)"
zh-HK | Mujer | "Microsoft Server Speech Text to Speech Voice (zh-HK, TracyRUS)"
zh-HK | Hombre | "Microsoft Server Speech Text to Speech Voice (zh-HK, Danny, Apollo)"
zh-TW | Mujer | "Microsoft Server Speech Text to Speech Voice (zh-TW, Yating, Apollo)"
zh-TW | Mujer | "Microsoft Server Speech Text to Speech Voice (zh-TW, HanHanRUS)"
zh-TW | Hombre | "Microsoft Server Speech Text to Speech Voice (zh-TW, Zhiwei, Apollo)"
 *ar-EG admite el árabe estándar moderno (MSA).

> [!NOTE]
> Tenga en cuenta que los nombres de servicio anteriores **Microsoft Server Speech Text to Speech Voice (cs-CZ, Vit)** and **Microsoft Server Speech Text to Speech Voice (en-IE, Shaun)** entrarán en desuso después del 31/3/2018, con el fin de optimizar las funcionalidades de Bing Speech API. Actualice el código con los nombres actualizados.

## <a name="TrouNSupport"></a>Solución de problemas y soporte técnico

Publique todas las preguntas y problemas en el foro de [Bing Speech Service](https://social.msdn.microsoft.com/Forums/en-US/home?forum=SpeechService) de MSDN. Incluya detalles completos, tales como:

* Un ejemplo de la cadena de solicitud completa.
* Si procede, el resultado completo de una solicitud errónea, lo que incluye los identificadores de registro.
* El porcentaje de solicitudes que generan error.
