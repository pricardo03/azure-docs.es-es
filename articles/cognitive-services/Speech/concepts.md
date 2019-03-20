---
title: Conceptos de Bing Speech | Microsoft Docs
titlesuffix: Azure Cognitive Services
description: Conceptos básicos que se usan en el servicio de voz de Microsoft.
services: cognitive-services
author: zhouwangzw
manager: wolfma
ms.service: cognitive-services
ms.subservice: bing-speech
ms.topic: article
ms.date: 09/18/2018
ms.author: zhouwang
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: c114c726bea34465972a282acac6b8acbbf9a80f
ms.sourcegitcommit: 8ca6cbe08fa1ea3e5cdcd46c217cfdf17f7ca5a7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/22/2019
ms.locfileid: "56670441"
---
# <a name="basic-concepts"></a>Conceptos básicos

[!INCLUDE [Deprecation note](../../../includes/cognitive-services-bing-speech-api-deprecation-note.md)]

En esta página se describen algunos conceptos básicos del servicio de reconocimiento de voz de Microsoft. Se recomienda leer esta página antes de usar Microsoft Speech Recognition API en la aplicación.

## <a name="understanding-speech-recognition"></a>Información sobre el reconocimiento de voz

Si es la primera vez que va a crear una aplicación habilitada para voz o si es la primera vez que va a agregar capacidades de voz a una aplicación existente, esta sección le ayudará a empezar con estas tareas. Si ya tiene cierta experiencia con aplicaciones habilitadas para voz, puede optar por ojear simplemente esta sección o puede omitirla por completo si es ya es un veterano en voz y quiere pasar directamente a los detalles de protocolo.

### <a name="audio-streams"></a>Secuencias de audio

Ante todo, entre los conceptos básicos de la voz se encuentra la *secuencia de audio*. A diferencia de una pulsación de tecla, que se produce en un único punto en el tiempo y contiene un único fragmento de información, una solicitud hablada se extiende a cientos de milisegundos y contiene una gran cantidad de kilobytes de información. La duración de las expresiones habladas presenta algunas dificultades a los desarrolladores que desean proporcionar una experiencia de voz simplificada y elegante para su aplicación. Los equipos y algoritmos actuales realizan la transcripción de voz en aproximadamente la mitad de la duración de la expresión, por lo que una expresión de 2 segundos puede transcribirse en aproximadamente 1 segundo, pero cualquier aplicación que experimente un retraso de 1 segundo en el proceso de usuario no será simplificada ni elegante.

Afortunadamente, hay formas de "ocultar" el tiempo de transcripción realizando la transcripción de una parte de la expresión mientras el usuario dice la otra parte. Por ejemplo, si se divide una expresión de 1 segundo en 10 fragmentos de 100 milisegundos y se realiza la transcripción de cada fragmento, más de 450 de los 500 milisegundos totales necesarios para la transcripción pueden "ocultarse", de modo que el usuario no sea consciente de la transcripción que se realiza mientras habla. Al pensar en este ejemplo, recuerde que el servicio realiza la transcripción de los 100 milisegundos de audio anteriores mientras el usuario habla durante los 100 siguientes, por lo que cuando el usuario deje de hablar, el servicio solo tendrá que transcribir aproximadamente 100 milisegundos de audio para producir un resultado.

Para lograr esta experiencia de usuario, la información de audio hablada se recopila en fragmentos y se transcriben a medida que el usuario habla. Estos fragmentos de audio colectivos de la *secuencia de audio* y el proceso de envío de estos fragmentos de audio al servicio se denomina *streaming de audio.* Audio streaming es una parte importante de cualquier aplicación habilitada para voz; el ajuste del tamaño de fragmento y la optimización de la implementación del streaming son algunas de las formas más impactantes de mejorar la experiencia de usuario de su aplicación.

### <a name="microphones"></a>Micrófonos

Las personas procesan el audio hablado con sus oídos, pero el hardware inanimado usa los micrófonos. Para habilitar la voz en cualquier aplicación, debe integrar el micrófono que proporciona la secuencia de audio para la aplicación.

Las API del micrófono deben permitir al usuario iniciar y detener la recepción de bytes de audio del micrófono. Debe decidir qué acciones de usuario desencadenarán que el micrófono empiece a escuchar la voz. Puede elegir que se tenga que presionar un botón para desencadenar que se inicie la escucha o que un observador de *palabra clave* o *palabra de reactivación* escuche siempre el micrófono y utilizar la salida de ese módulo para desencadenar el envío de audio al servicio de voz de Microsoft.

### <a name="end-of-speech"></a>Final del habla

Detectar *cuando* una persona ha *dejado* de hablar parece muy sencillo para los seres humanos, pero es un problema bastante difícil fuera de las condiciones de laboratorio. No basta con buscar simplemente el silencio tras una expresión, ya que a menudo el ruido del entorno complica las cosas. El servicio de voz de Microsoft realiza un excelente trabajo en la rápida detección de cuando un usuario deja de hablar y puede informar a la aplicación de este hecho, pero este mecanismo significa que la aplicación es la última en saber cuándo deja de hablar el usuario. No se parece en nada a los demás formatos de entrada en los que la aplicación es la *primera* en saber cuándo empieza la entrada del usuario *y* cuándo finaliza.

### <a name="asynchronous-service-responses"></a>Respuestas asincrónicas del servicio

El hecho de que la aplicación necesite estar informada de cuándo finaliza la entrada del usuario no significa ninguna disminución del rendimiento ni dificultades de programación para la aplicación, pero requiere que piense en las solicitudes de voz de manera diferente a los patrones de solicitud y respuesta con los que está familiarizado. Dado que la aplicación no sabrá cuando el usuario deje de hablar, la aplicación debe seguir transmitiendo audio al servicio mientras espera simultánea y asincrónicamente una respuesta del servicio. Este patrón es distinto de otros protocolos web de solicitud y respuesta, como HTTP. En estos protocolos, debe completar una solicitud antes de recibir cualquier respuesta; en el protocolo del servicio de voz de Microsoft, se reciben las respuestas *mientras aún se realiza el streaming de audio para la solicitud*.

> [!NOTE]
> Esta característica no se admite al usar la API de REST Speech HTTP.

### <a name="turns"></a>Turnos

La voz es portadora de información. Al hablar, se intenta expresar una información que se posee a alguien que escucha la información. Al expresar información, normalmente se producen turnos para hablar y escuchar. Del mismo modo, una aplicación habilitada para voz interactúa con los usuarios escuchando y respondiendo alternativamente, aunque la aplicación normalmente realiza la mayor parte de la escucha. La entrada de voz del usuario y la respuesta del servicio a esta entrada se llama *turno*. Un *turno* empieza cuando el usuario habla y termina cuando la aplicación ha completado la manipulación de la respuesta del servicio de voz.

### <a name="telemetry"></a>Telemetría

La creación de un dispositivo habilitado para voz o una aplicación puede resultar complicado, incluso para desarrolladores experimentados. Con frecuencia, los protocolos basados en secuencias parecen abrumadores a primera vista y los detalles importantes como la detección de silencios pueden ser completamente nuevos. Con la necesidad de enviar y recibir tantos mensajes correctamente para completar un solo par de solicitud y respuesta, es *muy* importante recopilar datos completos y precisos sobre los mensajes. El protocolo de servicio de voz de Microsoft proporciona la recopilación de estos datos. Debe hacer todo lo posible para proporcionar los datos necesarios con la mayor exactitud posible. Si suministra datos completos y precisos, se ayudará a sí mismo. Si alguna vez necesita ayuda del equipo del servicio de voz de Microsoft para solucionar problemas sobre la implementación del cliente, la calidad de los datos de telemetría recopilados será crítica para el análisis del problema.

> [!NOTE]
> Esta característica no se admite al usar la API de REST de reconocimiento de voz.

### <a name="speech-application-states"></a>Estados de la aplicación de voz

Los pasos necesarios para habilitar la entrada de voz en la aplicación son un poco diferentes de los pasos para otras formas de entrada como clics del mouse o pulsaciones de los dedos. Debe realizar un seguimiento de cuando la aplicación escucha el micrófono y envía datos al servicio de voz, cuando espera una respuesta del servicio y cuando se encuentra en un estado de inactividad. La relación entre estos estados se muestra en el diagrama siguiente.

![Diagrama de estados de la aplicación de voz](Images/speech-application-state-diagram.png)

Puesto que el servicio de voz de Microsoft participa de algunos de los estados, el protocolo de servicio define mensajes que ayudan a la aplicación en la transición entre estados. La aplicación necesita interpretar y actuar en estos mensajes de protocolo para realizar un seguimiento y administrar los estados de la aplicación de voz.

## <a name="using-the-speech-recognition-service-from-your-apps"></a>Usar el servicio de reconocimiento de voz desde las aplicaciones

El servicio de reconocimiento de voz de Microsoft ofrece dos métodos para que los desarrolladores agreguen voz a sus aplicaciones.

- [API REST](GetStarted/GetStartedREST.md): los desarrolladores pueden usar llamadas HTTP desde sus aplicaciones hasta el servicio para el reconocimiento de voz.
- [Bibliotecas cliente](GetStarted/GetStartedClientLibraries.md): si necesitan características avanzadas, los desarrolladores pueden descargar las bibliotecas cliente de Voz de Microsoft y vincularlas en sus aplicaciones.  Las bibliotecas cliente están disponibles en distintas plataformas (Windows, Android, iOS) con distintos lenguajes (C#, Java, JavaScript, ObjectiveC).

| Casos de uso | [API de REST](GetStarted/GetStartedREST.md) | [Bibliotecas cliente](GetStarted/GetStartedClientLibraries.md) |
|-----|-----|-----|
| Convertir un audio hablado corto, por ejemplo, comandos (longitud audio < 15 s) sin resultados provisionales | Sí | Sí |
| Convertir un audio largo (> 15 s) | Sin  | Sí |
| Audio de secuencia con resultados provisionales deseados | Sin  | Sí |
| Comprender el texto convertido desde audio mediante LUIS | Sin  | Sí |

 Si el lenguaje o plataforma no tiene todavía un SDK, puede crear su propia implementación en función de la [documentación del protocolo](API-Reference-REST/websocketprotocol.md).

## <a name="recognition-modes"></a>Modos de reconocimiento

Hay tres modos de reconocimiento: `interactive`, `conversation` y `dictation`. El modo de reconocimiento ajusta el reconocimiento de voz en función de cómo suelen hablar los usuarios. Elija el modo de reconocimiento apropiado para la aplicación.

> [!NOTE]
> Los modos de reconocimiento pueden tener comportamientos diferentes en el protocolo REST del que tienen en el protocolo WebSocket. Por ejemplo, la API de REST no admite el reconocimiento continuado, incluso en el modo de conversación o dictado.
> [!NOTE]
> Estos modos son aplicables cuando se usa directamente el protocolo REST o WebSocket. Las [bibliotecas cliente](GetStarted/GetStartedClientLibraries.md) usan parámetros diferentes para especificar el modo de reconocimiento. Para obtener más información, consulte la biblioteca cliente de su elección.

El servicio de voz de Microsoft devuelve solo un único resultado de frase de reconocimiento para todos los modos de reconocimiento. Hay un límite de 15 segundos para cualquier expresión única.

### <a name="interactive-mode"></a>Modo interactivo

En modo `interactive`, un usuario realiza solicitudes cortas y espera a que la aplicación realice una acción como respuesta.

Las siguientes características son típicas de las aplicaciones en modo interactivo:

- Los usuarios saben que hablan a una máquina y no a otra persona.
- Los usuarios de la aplicación saben de antemano lo que van a decir, en función de lo que quieren que haga la aplicación.
- Las expresiones tardan normalmente de 2 a 3 segundos aproximadamente.

### <a name="conversation-mode"></a>Modo de conversación

En modo `conversation`, los usuarios entablan una conversación de persona a persona.

Las siguientes características son típicas de las aplicaciones en modo conversación:

- Los usuarios saben que están hablando a otra persona.
- El reconocimiento de voz mejora las conversaciones humanas permitiendo que uno o los dos participantes vean el texto hablado.
- Los usuarios no siempre tienen previsto lo que quieren decir.
- Con frecuencia, los usuarios usan jerga y otro lenguaje informal.

### <a name="dictation-mode"></a>Modo de dictado

En modo `dictation`, los usuarios recitan expresiones más largas a la aplicación para su posterior proceso.

Las siguientes características son típicas de las aplicaciones en modo dictado:

- Los usuarios saben que están hablando a una máquina.
- Los usuarios ven los resultados en texto del reconocimiento de voz.
- Con frecuencia, los usuarios tienen previsto lo que van a decir y usan un lenguaje más formal.
- Los usuarios emplean frases completas que duran entre 5 y 8 segundos.

> [!NOTE]
> En los modos de dictado y conversación, el servicio de voz de Microsoft no devuelve resultados parciales. En su lugar, el servicio devuelve resultados estables de frases después de los límites de silencio en la secuencia de audio. Microsoft puede mejorar el protocolo de voz para mejorar la experiencia de usuario en estos modos de reconocimiento continuo.

## <a name="recognition-languages"></a>Idiomas de reconocimiento

El *idioma de reconocimiento* especifica el idioma que habla el usuario de la aplicación. Especifique el *idioma de reconocimiento* con el parámetro de consulta de dirección URL *language* en la conexión. El valor del parámetro de consulta *language* usa la etiqueta de idioma IETF [BCP 47](https://en.wikipedia.org/wiki/IETF_language_tag) y **debe** ser uno de los idiomas admitidos en Speech Recognition API. La lista completa de los idiomas admitidos por el servicio de voz se puede encontrar en la página [Supported Languages](API-Reference-REST/supportedlanguages.md) (Idiomas admitidos).

El servicio de voz de Microsoft rechaza las solicitudes de conexión no válidas y muestra una respuesta `HTTP 400 Bad Request`. Una solicitud no válida es aquella que:

- No incluye un valor de parámetro de consulta *language*.
- Incluye un parámetro de consulta *language* con formato incorrecto.
- Incluye un parámetro de consulta *language* que no es uno de los idiomas admitidos.

Puede optar por crear una aplicación que admita uno o todos los idiomas admitidos por el servicio.

### <a name="example"></a>Ejemplo

En el siguiente ejemplo, una aplicación utiliza el modo de reconocimiento de voz de *conversación* para una persona que hable inglés de Estados Unidos.

```HTTP
https://speech.platform.bing.com/speech/recognition/conversation/cognitiveservices/v1?language=en-US
```

## <a name="transcription-responses"></a>Respuestas de transcripción

Las respuestas de transcripción devuelven el texto convertido en audio a los clientes. Una respuesta de transcripción contiene los siguientes campos:

- `RecognitionStatus` especifica el estado del reconocimiento. En la tabla siguiente se indican los valores posibles.

| Status | DESCRIPCIÓN |
| ------------- | ---------------- |
| Correcto | El reconocimiento es correcto y el campo DisplayText está presente. |
| NoMatch | Se detectó voz en la secuencia de audio, pero no se encontraron coincidencias de palabras en el idioma de destino. Consulte [Estado de reconocimiento NoMatch (#nomatch-recognition-status) para obtener más información.  |
| InitialSilenceTimeout | El inicio de la secuencia de audio contenía solo silencio y el servicio agotó el tiempo de espera al esperar voz. |
| BabbleTimeout | El inicio de la secuencia de audio contenía solo ruido y el servicio agotó el tiempo de espera al esperar voz. |
| Error | El servicio de reconocimiento encontró un error interno y no pudo continuar. |

- `DisplayText` representa la frase reconocida después de aplicar las mayúsculas y minúsculas, los signos de puntuación y la normalización inversa de texto, además de enmascarar las palabras soeces con asteriscos. El campo DisplayText está presente *solo* si el campo `RecognitionStatus` tiene el valor `Success`.

- `Offset` especifica el desfase (en unidades de 100 nanosegundos) con el que se reconoció la frase, en relación con el inicio de la secuencia de audio.

- `Duration` especifica la duración (en unidades de 100 nanosegundos) de esta frase de voz.

Una respuesta de transcripción devuelve más información si lo desea. Vea el [formato de salida](#output-format) para obtener información de cómo devolver salidas detalladas.

El servicio de voz de Microsoft es compatible con el proceso de transcripción adicional que incluye la adición de mayúsculas, minúsculas y signos de puntuación, el enmascaramiento de palabras soeces y la normalización del texto a formatos comunes. Por ejemplo, si un usuario dice una frase representada por las palabras "recordarme que compre seis iPhone", los servicios de voz de Microsoft devolverán el texto transcrito "Recordarme que compre 6 iPhone". El proceso que convierte la palabra "seis" en el número "6" se denomina *Normalización inversa de texto* (*ITN*).

### <a name="nomatch-recognition-status"></a>Estado de reconocimiento NoMatch

La respuesta de transcripción devuelve `NoMatch` en `RecognitionStatus` cuando el servicio de voz de Microsoft detecta voz en la secuencia de audio, pero no puede encontrar coincidencias de la voz con la gramática del idioma que se usa para la solicitud. Por ejemplo, puede producirse una condición *NoMatch* si un usuario dice algo en alemán cuando el reconocedor espera inglés de Estados Unidos como idioma hablado. El patrón en forma de onda de la expresión indica la presencia de voz humana, pero ninguna de las palabras coincide con el lexicón de inglés de Estados Unidos que el reconocedor usa.

Se produce otra condición *NoMatch* cuando el algoritmo de reconocimiento no encuentra una coincidencia exacta para los sonidos contenidos en la secuencia de audio. Cuando se produce esta situación, el servicio de voz de Microsoft puede generar mensajes *speech.hypothesis* que contengan *texto hipotético*, pero generará un mensaje *speech.phrase* en el que el estado de *RecognitionStatus* sea *NoMatch*. Esta condición es normal; no debe dar por supuesta la precisión o fidelidad del texto del mensaje *speech.hypothesis*. Tampoco debe dar por supuesto que, como el servicio de voz de Microsoft genera mensajes *speech.hypothesis*, el servicio es capaz de generar un mensaje *speech.phrase* con  *RecognitionStatus* *Success*.

## <a name="output-format"></a>Formato de salida

El servicio de voz de Microsoft puede devolver una variedad de formatos de carga en las respuestas de transcripción. Todas las cargas son estructuras JSON.

Puede controlar el formato del resultado de frase especificando el parámetro de consulta de dirección URL `format`. De forma predeterminada, el servicio devuelve resultados `simple`.

| Formato | DESCRIPCIÓN |
|-----|-----|
| `simple` | Un resultado de frase simplificada que contiene el estado de reconocimiento y el texto reconocido en formato mostrado. |
| `detailed` | Un estado de reconocimiento y la lista de resultados de las n frases mejores en las que cada resultado de frase contiene los cuatro formatos de reconocimiento y una puntuación de confianza. |

El formato `detailed` contiene los [n mejores valores](#n-best-values), además de `RecognitionStatus`, `Offset` y `duration`, en la respuesta.

### <a name="n-best-values"></a>N mejores valores

Los agentes de escucha, independientemente de si son humanos o máquinas, nunca pueden ser estar seguros de que oyen *exactamente* lo que se dice. Un agente de escucha puede asignar una *probabilidad* solo a una interpretación determinada de una expresión.

En condiciones normales, al hablar con otros usuarios con los que interactúan con frecuencia, las personas tienen una probabilidad alta de reconocer las palabras que se dicen. Los agentes de escucha de voz basados en máquina se esfuerzan por alcanzar niveles de precisión similares y, en condiciones adecuadas, [conseguir la paridad con las personas](https://blogs.microsoft.com/next/2016/10/18/historic-achievement-microsoft-researchers-reach-human-parity-conversational-speech-recognition/#sm.001ykosqs14zte8qyxj2k9o28oz5v).

Los algoritmos que se usan en el reconocimiento de voz exploran interpretaciones alternativas de una expresión como parte del proceso normal. Normalmente, estas alternativas se descartan cuando la evidencia en favor de una sola interpretación pasa a ser abrumadora. En condiciones menos óptimas, sin embargo, el reconocedor de voz termina con una lista de interpretaciones alternativas posibles. Las *N* primeras alternativas de esta lista se denominan la *lista de las n mejores*. Se asigna una [puntuación de confianza](#confidence) a cada alternativa. Las puntuaciones de confianza oscilan entre 0 y 1. Una puntuación de 1 representa el nivel más alto de confianza. Una puntuación de 0 representa el nivel más bajo de confianza.

> [!NOTE]
> El número de entradas en la lista de las mejores varía entre las distintas expresiones. El número de entradas puede variar entre distintos reconocimientos de la *misma* expresión. Esta variación es un resultado esperado y natural de la naturaleza probabilística del algoritmo de reconocimiento de voz.

Cada entrada que se devuelve en la lista de las n mejores contiene

- `Confidence`, que representa las [puntuaciones de confianza](#confidence) de esta entrada.
- `Lexical`, que es el [formato léxico](#lexical-form) del texto reconocido.
- `ITN`, que es el [formato ITN](#itn-form) del texto reconocido.
- `MaskedITN`, que es el [formato ITN enmascarado](#masked-itn-form) del texto reconocido.
- `Display`, que es el [formato mostrado](#display-form) del texto reconocido.

### Puntuación de confianza <a id="confidence"></a>

Las puntuaciones de confianza son parte integrante de los sistemas de reconocimiento de voz. El servicio de voz de Microsoft obtiene las puntuaciones de confianza de un *clasificador de confianza*. Microsoft entrena al clasificador de confianza a través de un conjunto de características que están diseñadas para discriminar al máximo entre el reconocimiento correcto e incorrecto. Se evalúan las puntuaciones de confianza con respecto a palabras individuales y expresiones completas.

Si decide utilizar las puntuaciones de confianza que el servicio devuelve, tenga en cuenta el siguiente comportamiento:

- Las puntuaciones de confianza se pueden comparar solo dentro del mismo modo de reconocimiento e idioma. No se pueden comparar puntuaciones entre diferentes idiomas o modos de reconocimiento diferentes. Por ejemplo, una puntuación de confianza en el modo reconocimiento interactivo *no* tiene ninguna correlación con una puntuación de confianza en modo de dictado.
- Las puntuaciones de confianza son más útiles en un conjunto restringido de expresiones. Naturalmente, hay un alto grado de variabilidad en las puntuaciones para un conjunto grande de expresiones.

Si decide usar un valor de puntuación de confianza como *umbral* en el que actúa la aplicación, use el reconocimiento de voz para establecer los valores de umbral.

- Ejecute el reconocimiento de voz en un ejemplo representativo de expresiones para la aplicación.
- Recopile las puntuaciones de confianza para cada reconocimiento en el conjunto de ejemplo.
- Base el valor de umbral en algunos percentiles de confianza para ese ejemplo.

Ningún valor de umbral único es adecuado para todas las aplicaciones. Una puntuación de confianza aceptable para una aplicación podría no ser aceptable para otra.

### <a name="lexical-form"></a>formato léxico

El formato léxico es el texto reconocido, exactamente como aparece en la expresión y sin signos de puntuación ni uso de mayúsculas. Por ejemplo, el formato léxico de la dirección "Calle Provenza 237" sería *calle provenza doscientos treinta y siete*, suponiendo que se dijera así. El formato léxico de la frase "Recordarme que compre 5 lápices" es *recordarme que compre cinco lapices*.

El formato léxico es más adecuado para las aplicaciones que necesitan realizar la normalización no estándar de texto. El formato léxico también es adecuado para las aplicaciones que necesitan palabras de reconocimiento sin procesar.

Las palabras soeces nunca se enmascaran en el formato léxico.

### <a name="itn-form"></a>Formato ITN

La normalización de texto es el proceso de conversión de texto de un formato a otro formato "canónico". Por ejemplo, el número de teléfono "555-1212" puede convertirse en el formato canónico *cinco cinco cinco uno dos uno dos*. La normalización *inversa* de texto (ITN) invierte este proceso y convierte las palabras "cinco cinco cinco uno dos uno dos" al formato canónico invertido *555-1212*. El formato ITN de un resultado de reconocimiento no incluye el uso de mayúsculas o signos de puntuación.

El formato ITN es más adecuado para las aplicaciones que actúan sobre el texto reconocido. Por ejemplo, una aplicación que permite a un usuario decir términos de búsqueda y luego usa estos términos en una consulta web utilizará el formato ITN. Las palabras soeces nunca se enmascaran en el formato ITN. Para enmascarar las palabras soeces, use el *formato ITN enmascarado*.

### <a name="masked-itn-form"></a>Formato ITN enmascarado

Como las palabras soeces son una parte natural del idioma hablado, el servicio de voz de Microsoft reconoce estas palabras y frases al decirlas. Sin embargo, puede que las palabras soeces no sean apropiadas para todas las aplicaciones, especialmente para las que son para un público restringido de usuarios no adultos.

El formato ITN enmascarado aplica el enmascaramiento de palabras soeces a la forma de normalización inversa de texto. Para enmascarar palabras soeces, establezca el valor del parámetro de palabras soeces en `masked`. Cuando se enmascaran las palabras soeces, las palabras que se reconocen como parte del lexicón de palabras soeces del idioma se reemplazan por asteriscos. Por ejemplo: *recordarme que compre 5 \*\*\*\* libretas*. El formato ITN enmascarado de un resultado de reconocimiento no incluye el uso de mayúsculas o signos de puntuación.

> [!NOTE]
> Si se establece el valor del parámetro de consulta de palabras soeces en `raw`, el formato ITN enmascarado es el mismo que el formato ITN. Las palabras soeces *no* se enmascaran.

### <a name="display-form"></a>Formato mostrado

Los signos de puntuación y el uso de mayúsculas indican dónde colocar el énfasis, hacer una pausa, etcétera, lo que hace más fácil de entender el texto. El formato mostrado agrega signos de puntuación y mayúsculas a los resultados de reconocimiento, lo que hace que el formato sea más adecuado para las aplicaciones que muestran el texto hablado.

Puesto que el formato mostrado amplía el formato ITN enmascarado, puede establecer el valor del parámetro de palabras soeces en `masked` o `raw`. Si el valor se establece en `raw`, los resultados del reconocimiento incluyen las palabras soeces que el usuario ha dicho. Si el valor se establece en `masked`, las palabras que se reconocen como parte del lexicón de palabras soeces del idioma se reemplazan por asteriscos.

### <a name="sample-responses"></a>Respuestas de ejemplo

Todas las cargas son estructuras JSON.

El formato de carga del resultado de frase `simple`:

```json
{
  "RecognitionStatus": "Success",
  "DisplayText": "Remind me to buy 5 pencils.",
  "Offset": "1236645672289",
  "Duration": "1236645672289"
}
```

El formato de carga del resultado de frase `detailed`:

```json
{
  "RecognitionStatus": "Success",
  "Offset": "1236645672289",
  "Duration": "1236645672289",
  "NBest": [
      {
        "Confidence" : "0.87",
        "Lexical" : "remind me to buy five pencils",
        "ITN" : "remind me to buy 5 pencils",
        "MaskedITN" : "remind me to buy 5 pencils",
        "Display" : "Remind me to buy 5 pencils.",
      },
      {
        "Confidence" : "0.54",
        "Lexical" : "rewind me to buy five pencils",
        "ITN" : "rewind me to buy 5 pencils",
        "MaskedITN" : "rewind me to buy 5 pencils",
        "Display" : "Rewind me to buy 5 pencils.",
      }
  ]
}
```

## <a name="profanity-handling-in-speech-recognition"></a>Control de palabras soeces en el reconocimiento de voz

El servicio de voz de Microsoft reconoce todas las formas de voz humana, incluidas las palabras y las frases que muchas personas pueden clasificar como "palabras soeces". Puede controlar la forma en que el servicio controla las palabras soeces utilizando el parámetro de consulta *profanity*. De forma predeterminada, el servicio enmascara las palabras soeces en los resultados *speech.phrase* y no devuelve mensajes *speech.hypothesis* que contienen palabras soeces.

| Valor de *profanity* | DESCRIPCIÓN |
| - | - |
| `masked` | Enmascara las palabras soeces con asteriscos. Este comportamiento es el predeterminado. |
| `removed` | Quita las palabras soeces de todos los resultados. |
| `raw` | Reconoce y devuelve las palabras soeces en todos los resultados. |

### <a name="profanity-value-masked"></a>Valor de profanity `Masked`

Para enmascarar las palabras soeces, establezca el parámetro de consulta *profanity* en el valor *masked*. Cuando el parámetro de consulta *profanity* tiene este valor o no se especifica para una solicitud, el servicio *enmascara* las palabras soeces. El servicio realiza el enmascaramiento reemplazando las palabras soeces en los resultados del reconocimiento por asteriscos. Cuando se especifica la utilización del enmascaramiento de palabras soeces, el servicio no devuelve mensajes *speech.hypothesis* que contienen palabras soeces.

### <a name="profanity-value-removed"></a>Valor de profanity `Removed`

Cuando el parámetro *profanity* tiene el valor de consulta *removed*, el servicio quita las palabras soeces de los mensajes *speech.phrase* y *speech.hypothesis*. Los resultados son los mismos *que si no se dijeran las palabras soeces*.

#### <a name="profanity-only-utterances"></a>Expresiones de solo palabras soeces

Un usuario puede decir *solo* palabras soeces en una aplicación que ha configurado el servicio quitar las palabras soeces. Para este escenario, si el modo de reconocimiento es *dictado* o *conversación*, el servicio no devuelve *speech.result*. Si el modo de reconocimiento es *interactivo*, el servicio devuelve *speech.result* con el código de estado *NoMatch*.

### <a name="profanity-value-raw"></a>Valor de profanity `Raw`

Cuando el parámetro de consulta *profanity* tiene el valor *raw*, el servicio no quita las palabras soeces de los mensajes *speech.phrase* y *speech.hypothesis*.
