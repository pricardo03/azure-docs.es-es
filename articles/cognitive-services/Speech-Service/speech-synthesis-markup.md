---
title: 'Lenguaje de marcado de síntesis de voz (SSML): servicio de Voz'
titleSuffix: Azure Cognitive Services
description: Uso del lenguaje de marcado de síntesis de voz para controlar la pronunciación y la prosodia en la conversión de texto a voz.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 07/05/2019
ms.author: erhopf
ms.openlocfilehash: 89b8b5f8c574de033fabf6861e24fc7d2b31e171
ms.sourcegitcommit: 670c38d85ef97bf236b45850fd4750e3b98c8899
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/08/2019
ms.locfileid: "68855096"
---
# <a name="speech-synthesis-markup-language-ssml"></a>Lenguaje de marcado de síntesis de voz (SSML)

El lenguaje de marcado de síntesis de voz (SSML) es un lenguaje de marcado basado en XML que permite a los desarrolladores especificar cómo se convierte el texto de entrada en una voz sintetizada mediante el servicio de texto a voz. En comparación con el texto sin formato, SSML permite a los desarrolladores ajustar el tono, la pronunciación, la velocidad del habla, el volumen y muchas cosas más en la salida de texto a voz. La puntuación normal, como hacer una pausa después de un punto o usar la entonación correcta cuando una oración termina con un signo de interrogación, se administra automáticamente.

La implementación de los servicios de voz de SSML se basa en la [versión 1.0 del lenguaje de marcado de síntesis de voz](https://www.w3.org/TR/speech-synthesis) del World Wide Web Consortium.

> [!IMPORTANT]
> Los caracteres en chino, japonés y coreano se cuentan como dos caracteres para la facturación. Para obtener más información, consulte el apartado [Precios](https://azure.microsoft.com/pricing/details/cognitive-services/speech-services/).

## <a name="standard-neural-and-custom-voices"></a>Voces neuronales, estándares y personalizadas

Puede elegir entre voces estándar y neuronales, o puede crear su propia voz personalizada única para su producto o marca. Tiene más de 75 voces estándar disponibles en más de 45 idiomas y configuraciones regionales y 5 voces neuronales que están disponibles en 4 idiomas y configuraciones regionales. Para obtener una lista completa de los idiomas compatibles, las configuraciones regionales y las voces (neuronales y estándar), consulte [compatibilidad con idiomas](language-support.md).

Para más información sobre las voces estándar, neuronales y personalizadas, consulte la [introducción del texto a voz](text-to-speech.md).

## <a name="supported-ssml-elements"></a>Elementos SSML admitidos

Cada documento SSML se crea con los elementos SSML (o etiquetas). Estos elementos se utilizan para ajustar el tono, la prosodia, el volumen y mucho más. En las siguientes secciones se detallan cómo se utiliza cada elemento y cuándo es necesario u opcional.  

> [!IMPORTANT]
> No se olvide de utilizar comillas dobles alrededor de los valores de atributo. Las normas para un XML válido y bien formado requieren que los valores de los atributos estén rodeados de comillas dobles. Por ejemplo, `<prosody volume="90">` es un elemento válido y bien formado, pero `<prosody volume=90>` no. Es posible que SSML no reconozca valores de atributos que no estén entre comillas.

## <a name="create-an-ssml-document"></a>Creación de un documento SSML

`speak` es el elemento raíz y se **necesita** para todos los documentos SSML. El elemento `speak` contiene información importante, como la versión, el idioma y la definición del vocabulario de marcado.

**Sintaxis**

```xml
<speak version="1.0" xmlns="https://www.w3.org/2001/10/synthesis" xml:lang="string"></speak>
```

**Atributos**

| Atributo | DESCRIPCIÓN | Obligatorio u opcional |
|-----------|-------------|---------------------|
| version | Indica la versión de la especificación SSML utilizada para interpretar el marcado del documento. La versión actual es 1.0. | Obligatorio |
| xml:lang | Especifica el idioma del documento raíz. El valor puede contener un código de idioma en minúsculas y de dos letras (por ejemplo, **es**) o el código de idioma y el país o región en mayúscula (por ejemplo, **es-US**). | Obligatorio |
| xmlns | Especifica el URI del documento que define el vocabulario de marcado (los tipos de elementos y nombres de atributos) del documento SSML. El identificador URI actual es https://www.w3.org/2001/10/synthesis. | Obligatorio |

## <a name="choose-a-voice-for-text-to-speech"></a>Elección de una voz para la conversión de texto a voz

El elemento `voice` es obligatorio. Sirve para especificar la voz que se usa en la conversión de texto a voz.

**Sintaxis**

```xml
<voice name="string">
    This text will get converted into synthesized speech.
</voice>
```

**Atributos**

| Atributo | DESCRIPCIÓN | Obligatorio u opcional |
|-----------|-------------|---------------------|
| Nombre | Identifica la voz que se usa para la salida de texto a voz. Para ver una lista completa de voces compatibles, consulte [Compatibilidad con idiomas](language-support.md#text-to-speech). | Obligatorio |

**Ejemplo**

> [!NOTE]
> En este ejemplo se usa la voz `en-US-Jessa24kRUS`. Para ver una lista completa de voces compatibles, consulte [Compatibilidad con idiomas](language-support.md#text-to-speech).

```XML
<speak version="1.0" xmlns="https://www.w3.org/2001/10/synthesis" xml:lang="en-US">
    <voice  name="en-US-Jessa24kRUS">
        This is the text that is spoken.
    </voice>
</speak>
```

## <a name="use-multiple-voices"></a>Uso de varias voces

Dentro del elemento `speak`, puede especificar varias voces para la salida de texto a voz. Estas voces pueden estar en diferentes idiomas. Para cada voz, el texto se debe encapsular en un elemento `voice`.

**Atributos**

| Atributo | DESCRIPCIÓN | Obligatorio u opcional |
|-----------|-------------|---------------------|
| Nombre | Identifica la voz que se usa para la salida de texto a voz. Para ver una lista completa de voces compatibles, consulte [Compatibilidad con idiomas](language-support.md#text-to-speech). | Obligatorio |

**Ejemplo**

```xml
<speak version="1.0" xmlns="https://www.w3.org/2001/10/synthesis" xml:lang="en-US">
    <voice  name="en-US-Jessa24kRUS">
        Good morning!
    </voice>
    <voice  name="en-US-Guy24kRUS">
        Good morning to you too Jessa!
    </voice>
</speak>
```

## <a name="adjust-speaking-styles"></a>Ajuste de los estilos de habla

> [!IMPORTANT]
> Esta característica solo funciona con las voces neuronales.

De forma predeterminada, el servicio de texto a voz sintetiza el texto mediante un estilo de habla neutro tanto para voces estándar como neuronales. Con las voces neuronales, puede ajustar el estilo de habla para expresar alegría, empatía o sentimiento con el elemento `<mstts:express-as>`. Se trata de un elemento opcional único para los servicios de voz de Azure.

Actualmente, los ajustes de estilo de habla son compatibles con estas voces neuronales:
* `en-US-JessaNeural`
* `zh-CN-XiaoxiaoNeural`

Los cambios se aplican en el nivel de la oración y el estilo varía según la voz. Si no se admite un estilo, el servicio devolverá la voz con el estilo de habla neutro predeterminado.

**Sintaxis**

```xml
<mstts:express-as type="string"></mstts:express-as>
```

**Atributos**

| Atributo | DESCRIPCIÓN | Obligatorio u opcional |
|-----------|-------------|---------------------|
| Tipo | Especifica el estilo de habla. Actualmente, los estilos de habla son específicos de la voz. | Se necesita si se ajusta el estilo de habla para una voz neuronal. Si se usa `mstts:express-as`, se debe proporcionar el tipo. Si se proporciona un valor no válido, se omitirá este elemento. |

Utilice esta tabla para determinar qué estilos de habla son compatibles para cada voz neuronal.

| Voz | type | DESCRIPCIÓN |
|-------|------|-------------|
| `en-US-JessaNeural` | tipo =`cheerful` | Expresa una emoción que es positiva y feliz. |
| | tipo =`empathy` | Expresa un sentimiento de cuidado y comprensión. |
| `zh-CN-XiaoxiaoNeural` | tipo =`newscast` | Expresa un tono formal, similar a las retransmisiones de noticias. |
| | tipo =`sentiment` | Transmite un mensaje conmovedor o una historia. |

**Ejemplo**

Este fragmento de SSML ilustra cómo se utiliza el elemento `<mstts:express-as>` para cambiar el estilo de habla a `cheerful`.

```xml
<speak version="1.0" xmlns="https://www.w3.org/2001/10/synthesis" xmlns:mstts="https://www.w3.org/2001/mstts" xml:lang="en-US">
    <voice name="en-US-JessaNeural">
        <mstts:express-as type="cheerful">
            That'd be just amazing!
        </mstts:express-as>
    </voice>
</speak>
```

## <a name="add-or-remove-a-breakpause"></a>Adición o supresión de una pausa

Utilice el elemento `break` para insertar las pausas entre palabras, o para evitar pausas agregadas automáticamente por el servicio de texto a voz.

> [!NOTE]
> Utilice este elemento para invalidar el comportamiento predeterminado de texto a voz de una palabra o frase si el habla sintetizada de esa palabra o frase no suena natural. Establezca `strength` en `none` para evitar una pausa prosódica, que inserta automáticamente el servicio de texto a voz.

**Sintaxis**

```xml
<break strength="string" />
<break time="string" />
```

**Atributos**

| Atributo | DESCRIPCIÓN | Obligatorio u opcional |
|-----------|-------------|---------------------|
| intensidad | Especifica la duración relativa de una pausa mediante uno de los valores siguientes:<ul><li>None</li><li>x-weak</li><li>weak</li><li>medium (default)</li><li>strong</li><li>x-strong</li></ul> | Opcional |
| time | Especifica la duración absoluta de una pausa en segundos o milisegundos. Ejemplos de los valores válidos son 2 s y 500 | Opcional |

| Intensidad | DESCRIPCIÓN |
|----------|-------------|
| Ninguno, o si no se ha proporcionado ningún valor | 0 ms |
| x-weak | 250 ms |
| weak | 500 ms |
| medio | 750 ms |
| strong | 1000 ms |
| x-strong | 1250 ms |


**Ejemplo**

```xml
<speak version="1.0" xmlns="https://www.w3.org/2001/10/synthesis" xml:lang="en-US">
    <voice  name="en-US-Jessa24kRUS">
        Welcome to Microsoft Cognitive Services <break time="100ms" /> Text-to-Speech API.
    </voice>
</speak>
```

## <a name="specify-paragraphs-and-sentences"></a>Especificación de párrafos y oraciones

Los elementos `p` y `s` se utilizan para indicar párrafos y oraciones, respectivamente. En ausencia de estos elementos, el servicio de texto a voz determina automáticamente la estructura del documento SSML.

El elemento `p` puede contener texto y los siguientes elementos: `audio`, `break`, `phoneme`, `prosody`, `say-as`, `sub`, `mstts:express-as` y `s`.

El elemento `s` puede contener texto y los siguientes elementos: `audio`, `break`, `phoneme`, `prosody`, `say-as`, `mstts:express-as` y `sub`.

**Sintaxis**

```XML
<p></p>
<s></s>
```

**Ejemplo**

```XML
<speak version="1.0" xmlns="https://www.w3.org/2001/10/synthesis" xml:lang="en-US">
    <voice  name="en-US-Jessa24kRUS">
        <p>
            <s>Introducing the sentence element.</s>
            <s>Used to mark individual sentences.</s>
        </p>
        <p>
            Another simple paragraph.
            Sentence structure in this paragraph is not explicitly marked.
        </p>
    </voice>
</speak>
```

## <a name="use-phonemes-to-improve-pronunciation"></a>Uso de fonemas para mejorar la pronunciación

El elemento `ph` se utiliza para la pronunciación fonética en los documentos SSML. El elemento `ph` solo puede contener texto, no otros elementos. Proporcione siempre una voz natural como reserva.

Los alfabetos fonéticos se componen de segmentos acústicos, que se componen de letras, números o caracteres, a veces en combinación. Cada segmento acústico describe un sonido de voz único. Esto contrasta con el alfabeto latino, donde cualquier letra puede representar múltiples sonidos hablados. Tenga en cuenta las pronunciaciones diferentes de la letra "c" en las palabras "casa" y "cese" o, en inglés, las distintas pronunciaciones de la combinación de letras "th" en las palabras "thing" y "those".

**Sintaxis**

```XML
<phoneme alphabet="string" ph="string"></phoneme>
```

**Atributos**

| Atributo | DESCRIPCIÓN | Obligatorio u opcional |
|-----------|-------------|---------------------|
| alfabeto | Especifica el alfabeto fonético que se utilizará al sintetizar la pronunciación de la cadena en el atributo `ph`. La cadena que especifica el alfabeto debe especificarse en minúsculas. Estos son los posibles alfabetos que puede especificar.<ul><li>ipa: alfabeto fonético internacional</li><li>sapi: conjunto de segmentos acústicos de Speech API</li><li>ups: conjunto de segmentos acústicos universal</li></ul>El alfabeto solo se aplica al fonema del elemento. Para más información, consulte [Phonetic Alphabet Reference](https://msdn.microsoft.com/library/hh362879(v=office.14).aspx) (Referencia del alfabeto fonético). | Opcional |
| ph | Una cadena que contiene los segmentos acústicos que especifican la pronunciación de la palabra en el elemento `phoneme`. Si la cadena especificada contiene segmentos acústicos no reconocidos, el servicio de texto a voz rechaza todo el documento SSML y no produce ninguna de las salidas de voz especificadas en el documento. | Obligatorio si usa fonemas. |

**Ejemplos**

```XML
<speak version="1.0" xmlns="https://www.w3.org/2001/10/synthesis" xml:lang="en-US">
    <voice  name="en-US-Jessa24kRUS">
        <s>His name is Mike <phoneme alphabet="ups" ph="JH AU"> Zhou </phoneme></s>
    </voice>
</speak>
```

```xml
<speak version="1.0" xmlns="https://www.w3.org/2001/10/synthesis" xml:lang="en-US">
    <voice  name="en-US-Jessa24kRUS">
        <phoneme alphabet="ipa" ph="t&#x259;mei&#x325;&#x27E;ou&#x325;"> tomato </phoneme>
    </voice>
</speak>
```

## <a name="adjust-prosody"></a>Ajuste de la prosodia

El elemento `prosody` se utiliza para especificar los cambios en el tono, la curva melódica, el rango, la velocidad, la duración y el volumen de la salida de texto a voz. El elemento `prosody` puede contener texto y los siguientes elementos: `audio`, `break`, `p`, `phoneme`, `prosody`, `say-as`, `sub` y `s`.

Dado que los valores de los atributos prosódicos pueden variar en un amplio rango, el reconocedor de voz interpreta los valores asignados como una sugerencia de cuáles deben ser los valores prosódicos reales de la voz seleccionada. El servicio de texto a voz limita o sustituye valores que no son compatibles. Ejemplos de valores no compatibles son un tono de 1 MHz o un volumen de 120.

**Sintaxis**

```XML
<prosody pitch="value" contour="value" range="value" rate="value" duration="value" volume="value"></prosody>
```

**Atributos**

| Atributo | DESCRIPCIÓN | Obligatorio u opcional |
|-----------|-------------|---------------------|
| pitch | Indica el tono de la línea de referencia del texto. Puede expresar el tono como:<ul><li>Un valor absoluto, expresado como un número seguido de "Hz" (Hercios). Por ejemplo, 600 Hz.</li><li>Un valor relativo, expresado como un número precedido por "+" o "-" y seguido por "Hz" o "st", que especifica una cantidad para cambiar el tono. Por ejemplo: +80 Hz o -2st. La "st" indica que la unidad de cambio es un semitono, que es la mitad de un tono (medio paso) en la escala diatónica estándar.</li><li>Un valor constante:<ul><li>x-low</li><li>bajo</li><li>medio</li><li>alta</li><li>x-high</li><li>default</li></ul></li></ul>. | Opcional |
| contour | La curva melódica no es compatible con las voces neuronales. La curva melódica representa los cambios en el tono del contenido de la voz como una matriz de objetivos en posiciones de tiempo específicas en la salida de voz. Cada destino se define por conjuntos de pares de parámetros. Por ejemplo: <br/><br/>`<prosody contour="(0%,+20Hz) (10%,-2st) (40%,+10Hz)">`<br/><br/>El primer valor de cada conjunto de parámetros especifica la ubicación del cambio de tono como porcentaje de la duración del texto. El segundo valor especifica la cantidad para subir o bajar el tono, mediante un valor relativo o un valor de enumeración para el tono (consulte `pitch`). | Opcional |
| range  | Un valor que representa el rango del tono para el texto. Puede expresar `range` mediante los mismos valores absolutos, valores relativos o valores de enumeración usados para describir `pitch`. | Opcional |
| rate  | Indica la velocidad de habla del texto. Puede expresar `rate` como:<ul><li>Un valor relativo, expresado como un número que actúa como multiplicador del valor predeterminado. Por ejemplo, un valor de *1* no da como resultado ningún cambio en la velocidad. Un valor de *.5* da como resultado la mitad de la velocidad. Un valor de *3* da como resultado el triple de la velocidad.</li><li>Un valor constante:<ul><li>x-slow</li><li>lento</li><li>medio</li><li>fast</li><li>x-fast</li><li>default</li></ul></li></ul> | Opcional |
| duration  | El período de tiempo que debe transcurrir mientras el servicio de síntesis de voz lee el texto, en segundos o milisegundos. Por ejemplo, *2 s* o *1800 ms*. | Opcional |
| volumen  | Indica el nivel de volumen de la voz. Puede expresar el volumen como:<ul><li>Un valor absoluto, expresado como un número en el rango de 0,0 a 100,0, de *más silencioso* a *más alto*. Por ejemplo, 75. El valor predeterminado es 100.0.</li><li>Un valor relativo, expresado como un número precedido por "+" o "-" que especifica una cantidad para cambiar el volumen. Por ejemplo, +10 o -5.5.</li><li>Un valor constante:<ul><li>silent</li><li>x-soft</li><li>soft</li><li>medio</li><li>loud</li><li>x-loud</li><li>default</li></ul></li></ul> | Opcional |

### <a name="change-speaking-rate"></a>Cambio de la velocidad de habla

La velocidad de habla puede aplicarse a voces estándares en el nivel de palabra o frase. Sin embargo, solo puede aplicarse a voces neuronales en el nivel de oración.

**Ejemplo**

```xml
<speak version="1.0" xmlns="https://www.w3.org/2001/10/synthesis" xml:lang="en-US">
    <voice  name="en-US-Guy24kRUS">
        <prosody rate="+30.00%">
            Welcome to Microsoft Cognitive Services Text-to-Speech API.
        </prosody>
    </voice>
</speak>
```

### <a name="change-volume"></a>Cambio de volumen

Los cambios de volumen pueden aplicarse a voces estándar en el nivel de palabra o frase. Sin embargo, solo pueden aplicarse a voces neuronales en el nivel de frase.

**Ejemplo**

```xml
<speak version="1.0" xmlns="https://www.w3.org/2001/10/synthesis" xml:lang="en-US">
    <voice  name="en-US-Jessa24kRUS">
        <prosody volume="+20.00%">
            Welcome to Microsoft Cognitive Services Text-to-Speech API.
        </prosody>
    </voice>
</speak>
```

### <a name="change-pitch"></a>Cambio de tono

Los cambios de tono pueden aplicarse a voces estándar en el nivel de palabra o frase. Sin embargo, solo pueden aplicarse a voces neuronales en el nivel de frase.

**Ejemplo**

```xml
<speak version="1.0" xmlns="https://www.w3.org/2001/10/synthesis" xml:lang="en-US">
    <voice  name="en-US-Guy24kRUS">
        Welcome to <prosody pitch="high">Microsoft Cognitive Services Text-to-Speech API.</prosody>
    </voice>
</speak>
```

### <a name="change-pitch-contour"></a>Cambio de curva melódica

> [!IMPORTANT]
> Los cambios de curva melódica no se admiten con voces neuronales.

**Ejemplo**

```xml
<speak version="1.0" xmlns="https://www.w3.org/2001/10/synthesis" xml:lang="en-US">
    <voice  name="en-US-Jessa24kRUS">
        <prosody contour="(80%,+20%) (90%,+30%)" >
            Good morning.
        </prosody>
    </voice>
</speak>
```

## <a name="add-recorded-audio"></a>Adición del audio grabado

`audio` es un elemento opcional que le permite insertar audio MP3 en un documento SSML. El cuerpo del elemento de audio puede contener texto sin formato o marcado SSML que se lee en voz alta si el archivo de audio no está disponible o no se puede reproducir. Además, el elemento `audio` puede contener texto y los siguientes elementos: `audio`, `break`, `p`, `s`, `phoneme`, `prosody`, `say-as` y `sub`.

Cualquier audio incluido en el documento SSML debe cumplir estos requisitos:

* El archivo MP3 debe estar hospedado en un punto de conexión HTTPS accesible desde Internet. Se debe usar HTTPS y el dominio que hospeda el archivo MP3 debe presentar un certificado SSL válido y de confianza.
* El archivo MP3 debe ser válido (MPEG V2).
* La velocidad de bits debe ser de 48 kbps.
* La frecuencia de muestreo debe ser de 16 000 Hz.
* El tiempo total combinado de todos los archivos de texto y audio de una sola respuesta no puede superar los noventa (90) segundos.
* El archivo MP3 no debe contener ninguna información específica del cliente ni otra que sea confidencial.

**Sintaxis**

```xml
<audio src="string"/></audio>
```

**Atributos**

| Atributo | DESCRIPCIÓN | Obligatorio u opcional |
|-----------|-------------|---------------------|
| src | Especifica la ubicación o la URL del archivo de audio. | Es obligatorio si se usa el elemento de audio en el documento SSML. |

**Ejemplo**

```xml
<speak version="1.0" xmlns="https://www.w3.org/2001/10/synthesis" xml:lang="en-US">
    <p>
        <audio src="https://contoso.com/opinionprompt.wav"/>
        Thanks for offering your opinion. Please begin speaking after the beep.
        <audio src="https://contoso.com/beep.wav">
        Could not play the beep, please voice your opinion now. </audio>
    </p>
</speak>
```

## <a name="add-background-audio"></a>Adición de audio de fondo

El elemento `mstts:backgroundaudio` permite agregar audio de fondo a los documentos SSML (o mezclar un archivo de audio con el servicio de texto a voz). Con `mstts:backgroundaudio`, puede reproducir en bucle un archivo de audio de fondo, hacer que aparezca gradualmente al principio de la conversión de texto a voz y quitarlo gradualmente al final de la conversión de texto a voz.

Si el audio de fondo proporcionado dura menos que la conversión de texto a voz o el fundido de salida, se reproducirá en bucle. Si dura más que la conversión de texto a voz, se detendrá cuando finalice el fundido de salida.

Solo se permite un archivo de audio de fondo por cada documento SSML. Sin embargo, puede intercalar etiquetas `audio` dentro del elemento `voice` para agregar audio adicional al documento SSML.

**Sintaxis**

```XML
<mstts:backgroundaudio src="string" volume="string" fadein="string" fadeout="string"/>
```

**Atributos**

| Atributo | DESCRIPCIÓN | Obligatorio u opcional |
|-----------|-------------|---------------------|
| src | Especifica la ubicación o la URL del archivo de audio de fondo. | Es obligatorio si se usa el audio de fondo en el documento SSML. |
| volumen | Especifica el volumen del archivo de audio de fondo. **Valores aceptados**: de `0` a `100`, ambos incluidos. El valor predeterminado es `1`. | Opcional |
| fadein | Especifica la duración del fundido de entrada del audio de fondo en milisegundos. El valor predeterminado es `0`, que equivale a ningún fundido de entrada. **Valores aceptados**: de `0` a `10000`, ambos incluidos.  | Opcional |
| fadeout | Especifica la duración del fundido de salida del audio de fondo en milisegundos. El valor predeterminado es `0`, que equivale a ningún fundido de salida. **Valores aceptados**: de `0` a `10000`, ambos incluidos.  | Opcional |

**Ejemplo**

```xml
<speak version="1.0" xml:lang="en-US" xmlns:mstts="http://www.w3.org/2001/mstts">
    <mstts:backgroundaudio src="https://contoso.com/sample.wav" volume="0.7" fadein="3000" fadeout="4000"/>
    <voice name="Microsoft Server Speech Text to Speech Voice (en-US, Jessa24kRUS)">
        The text provided in this document will be spoken over the background audio.
    </voice>
</speak>
```

## <a name="next-steps"></a>Pasos siguientes

* [Compatibilidad de idiomas: voces, configuraciones regionales e idiomas](language-support.md)
