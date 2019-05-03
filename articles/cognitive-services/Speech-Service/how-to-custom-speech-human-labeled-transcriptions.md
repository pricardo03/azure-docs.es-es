---
title: Instrucciones con etiqueta humanos transcripciones - servicios de voz
titlesuffix: Azure Cognitive Services
description: Si está pensando para mejorar la precisión del reconocimiento, especialmente los problemas que se producen cuando se elimina o sustituye de forma incorrecta, palabras desea usar la etiqueta humanos transcripciones junto con los datos de audio. ¿Qué etiqueta humanos transcripciones? Que es fácil, son transcripciones palabra por palabra, textuales de un archivo de audio.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: erhopf
ms.openlocfilehash: 7f0b467284872f3d936984741c6d092705008a5a
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/02/2019
ms.locfileid: "65025927"
---
# <a name="how-to-create-human-labeled-transcriptions"></a>Creación de la etiqueta humanos transcripciones

Si está pensando para mejorar la precisión del reconocimiento, especialmente los problemas que se producen cuando se elimina o sustituye de forma incorrecta, palabras desea usar la etiqueta humanos transcripciones junto con los datos de audio. ¿Qué etiqueta humanos transcripciones? Que es fácil, son transcripciones palabra por palabra, textuales de un archivo de audio.

Un ejemplo grande de datos de transcripción se necesita para mejorar el reconocimiento, se recomienda proporcionar entre 10 y 1.000 horas de datos de transcripción. En esta página, revisaremos las directrices diseñadas para ayudarle a crear las transcripciones de alta calidad. Esta guía se divide por la configuración regional, con secciones para chino mandarín, inglés de Estados Unidos y Alemania.

## <a name="us-english-en-us"></a>Inglés de Estados Unidos (en-US)

Deben proporcionarse con etiqueta humanos transcripciones de audio inglés como texto sin formato, usar solo caracteres ASCII. Evite el uso de caracteres de puntuación Latín-1 o Unicode. Estos caracteres se agregan a menudo involuntariamente al copiar texto desde una aplicación de procesamiento de texto o el barrido de datos de páginas web. Si estos caracteres están presentes, asegúrese de que se actualizan con la sustitución de ASCII correspondiente.

Estos son algunos ejemplos:

| Caracteres para evitar | Sustitución | Notas |
|---------------------|--------------|-------|
| "Hola mundo" | "Hello world" | Las comillas de apertura y cierre se sustituyeron por caracteres ASCII adecuados. |
| Día de John | Día de John | El apóstrofo se ha sustituido por el carácter ASCII adecuado. |
| it was good—no, it was great! | it was good--no, it was great! | Se ha sustituido por el guión largo con dos guiones. |

### <a name="text-normalization-for-us-english"></a>Normalización de texto para inglés de Estados Unidos

Normalización de texto es la transformación de palabras en un formato coherente que utilizó al entrenar un modelo. Algunas reglas de normalización se aplican al texto automáticamente, sin embargo, se recomienda usar estas directrices cuando prepare los datos etiquetados humanos transcripción:

* Escribe las abreviaturas en palabras.
* Escribe cadenas numéricas no estándares en palabras (por ejemplo, en términos de Contabilidad).
* Caracteres no alfabéticos o mixto alfanuméricos debe ser transcripción como pronunciado.
* No se puede editar las abreviaturas que se pronunciación como palabras (por ejemplo, "radial", "láser", "RAM" o "OTAN").
* Escritura de salida de las abreviaturas que se pronuncia como cartas independientes por cada letra separado por un espacio.

Estos son algunos ejemplos de normalización que debe realizar en la transcripción:

| Texto original | Texto después de la normalización |
|---------------|--------------------------|
| Dr. Bruce Banner | Doctor Bruce Banner |
| James Bond, 007 | James Bond double oh seven |
| Ke$ha | Kesha |
| How long is the 2x4 | How long is the two by four |
| The meeting goes from 1-3pm | The meeting goes from one to three pm |
| Mi sangre es del tipo O + | My blood type is O positive |
| el agua es H20 | 2 H O el agua es |
| reproducir OU812 por Van Halen | reproducir O U 8 1 2 por Van Halen |
| UTF-8 with BOM | U T F 8 with BOM |

Las siguientes reglas de normalización se aplican automáticamente a transcripciones:

* Utilice letras minúsculas.
* Quite todos los signos de puntuación, excepto los apóstrofos dentro de las palabras.
* Expanda los números en formulario/habla de palabras, como importes en dólares.

Estos son algunos ejemplos de normalización que se realiza automáticamente en la transcripción:

| Texto original | Texto después de la normalización |
|---------------|--------------------------|
| "Holy cow!" said Batman. | holy cow said batman |
| "What?" said Batman's sidekick, Robin. | what said batman's sidekick robin |
| Go get -em! | go get em |
| I'm double-jointed | i’m double jointed |
| 104 Elm Street | one oh four Elm street |
| Tune to 102.7 | tune to one oh two seven |
| Pi is about 3.14 | pi is about three point one four |
It costs $3.14| it costs three fourteen |

## <a name="mandarin-chinese-zh-cn"></a>Chino mandarín (zh-cn)

Etiqueta humanos transcripciones de audio chino mandarín deben estar codificado con un marcador de orden de bytes UTF-8. Evite el uso de caracteres de puntuación de ancho medio. Estos caracteres pueden incluidos accidentalmente al preparar los datos en un procesador de textos o puede extraer datos desde las páginas web. Si estos caracteres están presentes, asegúrese de que se actualizan con la sustitución de ancho completo correspondiente.

Estos son algunos ejemplos:

| Caracteres para evitar | Sustitución | Notas |
|---------------------|--------------|-------|
| "你好" | "你好" | Las comillas de apertura y cierre se ha sustituido por caracteres correspondientes. |
| 需要什么帮助? | 需要什么帮助？ | El signo de interrogación se ha sustituido por caracteres adecuado. |

### <a name="text-normalization-for-mandarin-chinese"></a>Normalización de texto en chino mandarín

Normalización de texto es la transformación de palabras en un formato coherente que utilizó al entrenar un modelo. Algunas reglas de normalización se aplican al texto automáticamente, sin embargo, se recomienda usar estas directrices cuando prepare los datos etiquetados humanos transcripción:

* Escribe las abreviaturas en palabras.
* Escriba cadenas numéricas en forma hablada.

Estos son algunos ejemplos de normalización que debe realizar en la transcripción:

| Texto original | Texto después de la normalización |
|---------------|--------------------------|
| 我今年21 | 我今年二十一 |
| 3号楼504 | 三号 楼 五 零 四 |

Las siguientes reglas de normalización se aplican automáticamente a transcripciones:

* Quitar todos los signos de puntuación
* Expanda los números en forma de voz
* Convertir las letras de ancho completo en letras de ancho medio
* Uso de letras mayúsculas para todas las palabras en inglés

Estos son algunos ejemplos de normalización que se realiza automáticamente en la transcripción:

| Texto original | Texto después de la normalización |
|---------------|--------------------------|
| 3.1415 | 三 点 一 四 一 五 |
| ￥3.5 | 三 元 五 角 |
| w f y z |W F Y Z |
| 1992年8月8日 | 一 九 九 二 年 八 月 八 日 |
| 你吃饭了吗? | 你 吃饭 了 吗 |
| 下午5:00的航班 | 下午 五点 的 航班 |
| 我今年21岁 | 我 今年 二十 一 岁 |

## <a name="german-de-de-and-other-languages"></a>Alemán (de-DE) y otros lenguajes.

Etiqueta humanos transcripciones de audio alemán (y otros distintos del inglés o idiomas chino mandarín) deben estar codificado con un marcador de orden de bytes UTF-8. Debe proporcionarse una transcripción de la etiqueta humanos para cada archivo de audio.

### <a name="text-normalization-for-german"></a>Normalización de texto para alemán

Normalización de texto es la transformación de palabras en un formato coherente que utilizó al entrenar un modelo. Algunas reglas de normalización se aplican al texto automáticamente, sin embargo, se recomienda usar estas directrices cuando prepare los datos etiquetados humanos transcripción:

*   Escribir puntos decimales como ","y no".".
*   Escribir los separadores de hora como ":"y no"." (por ejemplo: 12:00 Uhr).
*   Las abreviaturas, como "ca." no se reemplazan. Se recomienda que utilice la forma hablada completa.
*   Se quitan los cuatro operadores matemáticos principales: +, -, \*, /. Se recomienda reemplazarlos con la forma escrita: "plus" "menos," "mal" y "geteilt."
*   Se quitan los operadores de comparación (=, <, y >). Se recomienda sustituirlos con "gleich", "kleiner als," y "grösser als".
*   Escribir fracciones, como 3/4, en forma escrita (por ejemplo: "drei viertel" en lugar de 3/4).
*   Reemplazar el símbolo de "€" con su formulario escrito "Euro".

Estos son algunos ejemplos de normalización que debe realizar en la transcripción:

| Texto original | Texto después de la normalización de usuario | Texto después de la normalización del sistema |
|---------------|-------------------------------|---------------------------------|
| Es ist 12.23 Uhr | Es ist 12:23 Uhr | es ist zwölf uhr drei und zwanzig uhr |
| {12.45} | {12,45} | zwölf komma vier fünf |
| 2 + 3 - 4 | 2 plus 3 minus 4 | zwei plus drei minus vier |

Las siguientes reglas de normalización se aplican automáticamente a transcripciones:

* Usar minúsculas en todo el texto.
* Quitar todos los signos de puntuación, incluidos los distintos tipos de comillas ("test", "test", "test" y «de prueba» es correctos).
* Descartar las filas con todos los caracteres especiales de este conjunto:™ ¤ ¥; § © ª ¬­® ° + ² entonces × ÿ Ø¬¬.
* Expanda números al formulario hablada, incluyendo dólar o cantidades Euro.
* Acepte metafonías solo para una, o y usted. Otros se reemplazará por "th" o se descartan.

Estos son algunos ejemplos de normalización que se realiza automáticamente en la transcripción:

| Texto original | Texto después de la normalización |
|---------------|--------------------------|
| Frankfurter Ring | frankfurter ring |
| ¡Eine Frage! | eine frage |
| wir, haben | wir haben |

## <a name="next-steps"></a>Pasos siguientes

* [Preparar y probar los datos](how-to-custom-speech-test-data.md)
* [Inspeccione los datos](how-to-custom-speech-inspect-data.md)
* [Evaluar los datos](how-to-custom-speech-evaluate-data.md)
* [Entrenar el modelo.](how-to-custom-speech-train-model.md)
* [Implementar el modelo](how-to-custom-speech-deploy-model.md)
