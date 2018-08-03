---
title: Instrucciones de transcripción para el aprendizaje de Voz | Microsoft Docs
description: Obtenga información sobre cómo preparar el texto para personalizar modelos de lenguaje y acústicos, y fuentes de voz para el servicio Voz.
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: PanosPeriorellis
manager: noellelacharite
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 07/01/2018
ms.author: panosper
ms.openlocfilehash: 6043d01e26d9f28f90e45c2cc194863b3a9cd017
ms.sourcegitcommit: 0b05bdeb22a06c91823bd1933ac65b2e0c2d6553
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/17/2018
ms.locfileid: "39072289"
---
# <a name="transcription-guidelines-for-using-speech-service"></a>Instrucciones de transcripción para usar el servicio Voz

Para personalizar **Conversión de voz en texto** o **Texto a voz**, debe proporcionar texto junto con la voz. Cada línea del texto se corresponde con una sola expresión. El texto debe coincidir con la voz lo más exactamente posible. El texto se denomina *transcripción* y se debe crear en un formato concreto.

El servicio de voz normaliza la entrada para mantener la coherencia del texto. 

En este artículo se describen ambos tipos de normalización. Las instrucciones varían ligeramente para los distintos idiomas.

## <a name="us-english-en-us"></a>Inglés de Estados Unidos (en-US)

Los datos de texto se deben escribir un enunciado por línea, en texto sin formato, con solo el juego de caracteres ASCII.

Es importante evitar el uso de caracteres de puntuación extendidos (Latino-1) o Unicode. Estos caracteres se pueden incluir de forma involuntaria al preparar los datos en un programa de procesamiento de texto o al extraerlos de páginas web. Reemplace estos caracteres por las sustituciones ASCII apropiadas. Por ejemplo: 

| Caracteres para evitar | Sustitución |
|----- | ----- |
| “Hello world” (comillas dobles de apertura y cierre) | "Hello world" (comillas dobles) |
| John’s day (comilla simple derecha) | John's day (apóstrofo) |
| it was good—no, it was great! (raya) | it was good--no, it was great! (guiones) |

### <a name="text-normalization-rules-for-english"></a>Reglas de normalización de texto para inglés

El servicio de voz lleva a cabo las siguientes reglas de normalización.

*   Aplicar minúsculas a todo el texto.
*   Quitar toda la puntuación menos los apóstrofos dentro de las palabras.
*   Expansión de los números a forma hablada, incluidas las cantidades en dólares.

Estos son algunos ejemplos

| Texto original | Después de la normalización |
|----- | ----- |
| "Holy cow!" said Batman. | holy cow said batman |
| "What?" said Batman's sidekick, Robin. | what said batman's sidekick robin |
| Go get -em! | go get em |
| I'm double-jointed | i’m double jointed |
| 104 Elm Street | one oh four Elm street |
| Tune to 102.7 | tune to one oh two seven |
| Pi is about 3.14 | pi is about three point one four |
| It costs $3.14 | it costs three fourteen |

Aplique la normalización siguiente a las transcripciones de texto.

*   Las abreviaturas se deben escribir con palabras
*   Las cadenas numéricas que no son estándar (por ejemplo, algunos formatos de fecha o contabilidad) se deben escribir en palabras.
*   Las palabras con caracteres que no son alfabéticos o con caracteres alfanuméricos mixtos se debe transcribir como se pronuncian.
*   No modifique las abreviaturas que se pronuncian como palabras. Por ejemplo, radar, laser, RAM, NATO.
*   Escriba las abreviaturas que se pronuncian como letras independientes y sepárelas mediante espacios. Por ejemplo, IBM, CPU, FBI, TBD, NaN. 

Estos son algunos ejemplos:

| Texto original | Después de la normalización |
|----- | ----- |
| 14 NE 3rd Dr. | fourteen northeast third drive |
| Dr. Bruce Banner | Doctor Bruce Banner |
| James Bond, 007 | James Bond double oh seven |
| Ke$ha | Kesha |
| How long is the 2x4 | How long is the two by four |
| The meeting goes from 1-3pm | The meeting goes from one to three pm |
| my blood type is O+ | My blood type is O positive |
| water is H20 | water is H 2 O |
| play OU812 by Van Halen | play O U 8 1 2 by Van Halen |
| UTF-8 with BOM | U T F 8 with BOM |

## <a name="chinese-zh-cn"></a>Chino (zh-CN)

Los datos de texto que se cargan a Custom Speech Service deben usar la codificación UTF-8 con marcador de orden de bytes. El archivo se debe escribir un enunciado por línea.

Evite el uso de caracteres de puntuación de ancho medio. Estos caracteres se pueden incluir de forma involuntaria al preparar los datos en un programa de procesamiento de texto o al extraerlos de páginas web. Reemplácelos con las sustituciones de ancho completo apropiadas. Por ejemplo: 

| Caracteres para evitar | Sustitución |
|----- | ----- |
| "你好" (comillas dobles de apertura y cierre) | "你好" (comillas dobles) |
| 需要什么帮助? (signo de interrogación) | 需要什么帮助？ |

### <a name="text-normalization-rules-for-chinese"></a>Reglas de normalización de texto para chino

El servicio de voz lleva a cabo las siguientes reglas de normalización.

*   Quitar todos los signos de puntuación.
*   Los números se expanden a la forma hablada.
*   Las letras de ancho completo se convierten a letras de ancho medio.
*   Convertir todas las palabras en inglés a mayúsculas.

Estos son algunos ejemplos.

| Texto original | Después de la normalización |
|----- | ----- |
| 3.1415 | 三 点 一 四 一 五 |
| ￥3.5 | 三 元 五 角 |
| w f y z | W F Y Z |
| 1992年8月8日 | 一 九 九 二 年 八 月 八 日 |
| 你吃饭了吗? | 你 吃饭 了 吗 |
| 下午5:00的航班 | 下午 五点 的 航班 |
| 我今年21岁 | 我 今年 二十 一 岁 |

Aplique la normalización siguiente al texto antes de importarlo.

*   Las abreviaturas se deben escribir con palabras (como en forma hablada).
*   Escriba cadenas numéricas en forma hablada.

Estos son algunos ejemplos.

| Texto original | Después de la normalización |
|----- | ----- |
| 我今年21 | 我今年二十一 |
| 3号楼504 | 三号 楼 五 零 四 |

## <a name="other-languages"></a>Otros lenguajes

Los datos de texto que se cargan al servicio **Conversión de voz en texto** deben usar la codificación UTF-8 con marcador de orden de bytes. El archivo se debe escribir un enunciado por línea.

> [!NOTE]
> En estos ejemplos se usa el alemán. Pero estas instrucciones se aplican a todos los idiomas que no sean el inglés de Estados Unidos o el chino.

### <a name="text-normalization-rules-for-german"></a>Reglas de normalización de texto para alemán

El servicio de voz lleva a cabo las siguientes reglas de normalización.

*   Aplicar minúsculas a todo el texto.
*   Se quitan todos los signos de puntuación incluidos los distintos tipos de comillas ("test", 'test', "test o «test» son correctos).
*   Se descartan todas las filas que contengan un carácter especial del juego ¢ ¤ ¥ ¦ § © ª ¬ ® ° ± ² µ × ÿ Ø¬¬
*   Expandir los números al formato de palabra, incluidas las cantidades en dólares o euros.
*   Solo se acepta la diéresis para las letras a, o, u; el resto se sustituye por "th" o se descarta.

Estos son algunos ejemplos.

| Texto original | Después de la normalización |
|----- | ----- |
| Frankfurter Ring | frankfurter ring |
| ¡Eine Frage! | eine frage |
| wir, haben | wir haben |

Aplique la normalización siguiente al texto antes de importarlo.

*   El separador decimal debe ser , (coma) y no . (punto).
*   El separador de hora entre las horas y los minutos debe ser ":" y no ".": 12:00 Uhr
*   Las abreviaturas, como "ca." no se reemplazan. Se recomienda usar la forma completa.
*   Se quitan los cuatro operadores matemáticos principales: +, -, \*, /. Se recomienda sustituirlos por su forma literal: plus, minus, mal, geteilt.
*   Esto mismo se aplica a los operadores de comparación (=, <, >): gleich, kleiner als, grösser als.
*   Las fracciones, como 3/4, se usan con formato de palabra (por ejemplo, "drei viertel" en lugar de ¾).
*   Reemplazar el símbolo € por el formato de palabra "Euro".

Estos son algunos ejemplos.

| Texto original | Después de la normalización del usuario | Después de la normalización del sistema
|--------  | ----- | -------- |
| Es ist 12.23Uhr | Es ist 12:23Uhr | es ist zwölf uhr drei und zwanzig uhr |
| {12.45} | {12,45} | zwölf komma vier fünf ||
| 2 + 3 - 4 | 2 plus 3 minus 4 | zwei plus drei minus vier|

## <a name="next-steps"></a>Pasos siguientes

- [Obtenga su suscripción de prueba a Voz](https://azure.microsoft.com/try/cognitive-services/)
- [Reconocer la voz en C#](quickstart-csharp-dotnet-windows.md)
