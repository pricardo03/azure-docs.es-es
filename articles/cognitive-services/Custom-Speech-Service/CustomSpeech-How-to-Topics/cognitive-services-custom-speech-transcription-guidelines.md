---
title: Directrices para la transcripción en Custom Speech Service en Azure | Microsoft Docs
description: Aprenda a preparar los datos para Custom Speech Service en Cognitive Services.
services: cognitive-services
author: PanosPeriorellis
manager: onano
ms.service: cognitive-services
ms.component: custom-speech
ms.topic: article
ms.date: 02/08/2017
ms.author: panosper
ms.openlocfilehash: 2785a35ac7583ac3d9503cb721d10078d86aa365
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/23/2018
ms.locfileid: "35380311"
---
# <a name="transcription-guidelines"></a>Directrices de transcripción
Para garantizar el mejor uso de los datos de texto para la personalización de los modelos acústico y de lenguaje, deben seguirse las siguientes directrices de transcripción. Estas directrices son específicas del idioma.

## <a name="text-normalization"></a>Normalización de texto

Para un uso óptimo en la personalización de los modelos acústico o de lenguaje, los datos de texto se deben normalizar, lo que significa que se deben transformar a un formato estándar y no ambiguo, que el sistema pueda leer. En esta sección se describe la normalización de texto realizada por Custom Speech Service cuando se importan datos y la normalización de texto que el usuario debe llevar a cabo antes de la importación de datos.

## <a name="inverse-text-normalization"></a>Normalización inversa de texto

El proceso de conversión de texto sin formato "sin procesar" de nuevo a texto con formato, es decir, con mayúsculas y signos de puntuación, se denomina normalización inversa de texto (ITN). La ITN se realiza en los resultados devueltos por Microsoft Cognitive Services Speech API. Un punto de conexión personalizado implementado con Custom Speech Service usa la misma ITN que Microsoft Cognitive Services Speech API. Sin embargo, este servicio no admite actualmente la ITN personalizada, por lo que los términos nuevos introducidos por un modelo de lenguaje personalizado no se formatearán en los resultados de reconocimiento.

## <a name="transcription-guidelines-for-en-us"></a>Directrices de transcripción para en-US

Los datos de texto que se cargan a este servicio se deben escribir en texto sin formato usando únicamente el juego de caracteres imprimible ASCII. Cada línea del archivo únicamente debe contener el texto para una sola expresión.

Es importante evitar el uso de caracteres de puntuación Unicode. Esto puede ocurrir inadvertidamente al preparar los datos en un programa de procesamiento de texto o al extraerlos de páginas web. Reemplace estos caracteres por las sustituciones ASCII apropiadas. Por ejemplo: 

| Unicode que se debe evitar | Sustitución ASCII |
|----- | ----- |
| “Hello world” (comillas dobles de apertura y cierre) | "Hello world" (comillas dobles) |
| John’s day (comilla simple derecha) | John's day (apóstrofo) |

### <a name="text-normalization-performed-by-the-custom-speech-service"></a>Normalización de texto realizada por Custom Speech Service

Este servicio llevará a cabo la normalización de texto siguiente en los datos importados como conjunto de datos de idioma o las transcripciones de un conjunto de datos acústico. Esto incluye:

*   Aplicar minúsculas a todo el texto.
*   Quitar toda la puntuación menos los apóstrofos dentro de las palabras.
*   Expandir los números a formato hablado, incluidas las cantidades en dólares.

Estos son algunos ejemplos

| Texto original | Después de la normalización |
|----- | ----- |
| Starbucks Coffee | starbucks coffee |
| “Holy cow!” said Batman. | holy cow said batman |
| “What?” said Batman’s sidekick, Robin. | what said batman’s sidekick robin |
| Go get -em! | go get em |
| I’m double-jointed | i’m double jointed |
| 104 Main Street | one oh four main street |
| Tune to 102.7 | tune to one oh two point seven |
| Pi is about 3.14 | pi is about three point one four |
| It costs $3.14 | it costs three fourteen |

### <a name="text-normalization-required-by-users"></a>Normalización de texto que deben realizar los usuarios

Para garantizar el mejor uso de los datos, se deben aplicar las siguientes reglas de normalización a los datos antes de importarlos.

*   Las abreviaturas se deben escribir en palabras para reflejar el formato hablado.
*   Las cadenas numéricas no estándar se deben escribir en palabras.
*   Las palabras con caracteres que no son alfabéticos o combinados con caracteres alfanuméricos se deben transcribir tal como se pronuncian.
*   Los acrónimos comunes pueden dejarse como una sola entidad sin puntos ni espacios entre las letras, pero todos los demás acrónimos se deben escribir con letras separadas por un único espacio.

Estos son algunos ejemplos

| Texto original | Después de la normalización |
|----- | ----- |
| 14 NE 3rd Dr. | fourteen northeast third drive |
| Dr. Strangelove | Doctor Strangelove |
| James Bond 007 | James Bond double oh seven |
| Ke$ha | Kesha |
| How long is the 2x4 | How long is the two by four |
| The meeting goes from 1-3pm | The meeting goes from one to three pm |
| my blood type is O+ | My blood type is O positive |
| water is H20 | water is H 2 O |
| play OU812 by Van Halen | play O U 8 1 2 by Van Halen |

## <a name="transcription-guidelines-for-zh-cn"></a>Directrices de la transcripción para zh-CN

Los datos de texto que se cargan a Custom Speech Service deben usar la **codificación UTF-8 (incl. BOM)**. Cada línea del archivo únicamente debe contener el texto para una sola expresión.

Es importante evitar el uso de caracteres de puntuación de ancho medio. Esto puede ocurrir inadvertidamente al preparar los datos en un programa de procesamiento de texto o al extraerlos de páginas web. Reemplace estos caracteres por las sustituciones de ancho completo. Por ejemplo: 

| Unicode que se debe evitar | Sustitución ASCII |
|----- | ----- |
| “你好” (comillas dobles de apertura y cierre) | "你好" (comillas dobles) |
| 需要什么帮助? (signo de interrogación) | 需要什么帮助？ |

### <a name="text-normalization-performed-by-the-custom-speech-service"></a>Normalización de texto realizada por Custom Speech Service

Este servicio de voz llevará a cabo la normalización del texto siguiente en los datos importados como conjunto de datos de idioma o las transcripciones de un conjunto de datos acústico. Esto incluye:

*   Quitar todos los signos de puntuación.
*   Expandir los números al formato hablado.
*   Convertir las letras de ancho completo en letras de ancho medio.
*   Convertir todas las palabras en inglés a mayúsculas.

Estos son algunos ejemplos:

| Texto original | Después de la normalización |
|----- | ----- |
| 3.1415 | 三 点 一 四 一 五 |
| ￥3.5 | 三 元 五 角 |
| w f y z | W F Y Z |
| 1992年8月8日 | 一 九 九 二 年 八 月 八 日 |
| 你吃饭了吗 ? | 你 吃饭 了 吗 |
| 下午5:00的航班 | 下午 五点 的 航班 |
| 我今年21岁 | 我 今年 二十 一 岁 |

### <a name="text-normalization-required-by-users"></a>Normalización de texto que deben realizar los usuarios

Para garantizar el mejor uso de los datos, se deben aplicar las siguientes reglas de normalización a los datos _antes_ de importarlos.

*   Las abreviaturas se deben escribir en palabras para reflejar el formato hablado.
*   Este servicio no abarca todas las cantidades numéricas. Es más confiable escribir cadenas numéricas en formato hablado.

Estos son algunos ejemplos

| Texto original | Después de la normalización |
|----- | ----- |
| 我今年21 | 我今年二十一 |
| 3号楼504 | 三号 楼 五 零 四 |

## <a name="transcription-guidelines-for-de-de"></a>Directrices de la transcripción para de-DE

Los datos de texto que se cargan a Custom Speech Service deben usar solo la **codificación UTF-8 (incl. BOM)**. Cada línea del archivo únicamente debe contener el texto para una sola expresión.

### <a name="text-normalization-performed-by-the-custom-speech-service"></a>Normalización de texto realizada por Custom Speech Service

Este servicio llevará a cabo la normalización de texto siguiente en los datos importados como conjunto de datos de idioma o las transcripciones de un conjunto de datos acústico. Esto incluye:

*   Aplicar minúsculas a todo el texto.
*   Quitar todos los signos de puntuación, incluidas las comillas inglesas o alemanas ("test", 'test', “test o «test» son correctos).
*   Descartar todas las filas que contengan un carácter especial, incluidos: ^ ¢ £ ¤ ¥ ¦ § © ª ¬ ® ° ± ² µ × ÿ Ø¬¬.
*   Expandir los números al formato de palabra, incluidas las cantidades en dólares o euros.
*   Aceptar la diéresis solo para las letras a, o, u; el resto se sustituye por "th" o se descarta.

Estos son algunos ejemplos

| Texto original | Después de la normalización |
|----- | ----- |
| Frankfurter Ring | frankfurter ring |
| "Hallo, Mama!" sagt die Tochter. | hallo mama sagt die tochter |
| ¡Eine Frage! | eine frage |
| wir, haben | wir haben |
| Das macht $10 | das macht zehn dollars |


### <a name="text-normalization-required-by-users"></a>Normalización de texto que deben realizar los usuarios

Para garantizar el mejor uso de los datos, se deben aplicar las siguientes reglas de normalización a los datos antes de importarlos.

*   El separador decimal debe ser , (coma) y no . (punto). Por ejemplo, 2,3% y no 2.3%.
*   El separador entre las horas y los minutos debe ser : (dos puntos) y no . (punto). Por ejemplo, 12:00 Uhr
*   Las abreviaturas, como "ca.", "bzw." no se reemplazan. Se recomienda utilizar el formato completo para obtener la pronunciación correcta.
*   Quitar los cinco operadores matemáticos principales: +, -, \*, /.
 Se recomienda sustituirlos por su formato literal: plus, minus, mal, geteilt.
*   Esto mismo se aplica a los comparadores (=, <, >) - gleich, kleiner als, grösser als.
*   Usar fracciones, como 3/4, con formato de palabra, "drei viertel" en lugar de ¾.
*   Reemplazar el símbolo € por el formato de palabra "Euro".


Estos son algunos ejemplos

| Texto original | Después de la normalización del usuario | Después de la normalización del sistema
|--------  | ----- | -------- |
| Es ist 12.23Uhr | Es ist 12:23Uhr | es ist zwölf uhr drei und zwanzig uhr |
| {12.45} | {12,45} | zwölf komma vier fünf |
| 3 < 5 | 3 kleiner als 5 | drei kleiner als vier |
| 2 + 3 - 4 | 2 plus 3 minus 4 | zwei plus drei minus vier|
| Das macht 12€ | Das macht 12 Euros | das macht zwölf euros |



## <a name="next-steps"></a>Pasos siguientes
* [Cómo usar un punto de conexión personalizado de voz a texto](cognitive-services-custom-speech-create-endpoint.md)
* Mejorar la precisión con el [modelo acústico personalizado](cognitive-services-custom-speech-create-acoustic-model.md)
* Mejorar la precisión con un [modelo de lenguaje personalizado](cognitive-services-custom-speech-create-language-model.md)
