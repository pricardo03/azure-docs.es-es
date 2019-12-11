---
title: 'Preparación de los datos de prueba para Custom Speech: servicio de voz'
titleSuffix: Azure Cognitive Services
description: Tanto si prueba la precisión del reconocimiento de voz de Microsoft como si entrena sus propios modelos, necesitará datos (en forma de audio o texto). En esta página, veremos los tipos de datos, cómo se usan y cómo administrar cada uno de ellos.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 09/06/2019
ms.author: erhopf
ms.openlocfilehash: b2d02ed5a9fb2cb10e4cf18fe7d878da5b032fe0
ms.sourcegitcommit: 6c01e4f82e19f9e423c3aaeaf801a29a517e97a0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/04/2019
ms.locfileid: "74816406"
---
# <a name="prepare-data-for-custom-speech"></a>Preparación de los datos para Custom Speech

Tanto si prueba la precisión del reconocimiento de voz de Microsoft como si entrena sus propios modelos, necesitará datos en forma de audio y texto. En esta página, veremos los tipos de datos, cómo se usan y cómo administrar cada uno de ellos.

## <a name="data-types"></a>Tipos de datos

En esta tabla se enumeran los tipos de datos aceptados, cuándo se debe utilizar cada tipo de datos y la cantidad recomendada. No todos los tipos de datos son necesarios para crear un modelo. Los requisitos de datos variarán dependiendo de si crea una prueba o entrena un modelo.

| Tipo de datos | Se utiliza en las pruebas | Cantidad recomendada | Se utiliza para el entrenamiento | Cantidad recomendada |
|-----------|-----------------|----------|-------------------|----------|
| [Audio](#audio-data-for-testing) | Sí<br>Se utiliza para la inspección visual | Más de cinco archivos de audio | Sin | N/a |
| [Transcripciones de audio con etiqueta humana](#audio--human-labeled-transcript-data-for-testingtraining) | Sí<br>Se utiliza para evaluar la precisión | De 0,5 a 5 horas de audio | Sí | De 1 a 1000 horas de audio |
| [Texto relacionado](#related-text-data-for-training) | Sin | N/a | Sí | De 1 a 200 MB de texto relacionado |

Los archivos deben agruparse por tipo en un conjunto de datos y cargarse como un archivo zip. Cada conjunto de datos solo puede contener un tipo de datos.

## <a name="upload-data"></a>Carga de datos

Cuando esté listo para cargar los datos, vaya al [portal de Custom Speech](https://speech.microsoft.com/customspeech) y haga clic en **Cargar datos** para iniciar el asistente y crear el primer conjunto de datos. Se le pedirá que seleccione un tipo de datos de voz para el conjunto de datos, antes de permitirle cargar los datos.

![Selección del audio desde el portal de Voz](./media/custom-speech/custom-speech-select-audio.png)

Cada conjunto de datos que cargue debe cumplir los requisitos del tipo de datos elegido. Es importante dar formato correctamente a los datos antes de cargarlos. Esto garantiza que el servicio Custom Speech procesará los datos con precisión. Los requisitos se muestran en las secciones siguientes.

Una vez cargado el conjunto de datos, tiene algunas opciones:

* Puede dirigirse a la pestaña **Testing** (Pruebas) e inspeccionar visualmente datos de transcripción solo de audio o de audio y con etiqueta humana.
* Puede dirigirse a la pestaña **Training** (Entrenamiento) y utilizar datos de transcripción de audio y humana o datos de texto relacionados para entrenar un modelo personalizado.

## <a name="audio-data-for-testing"></a>Datos de audio para pruebas

Los datos de audio son óptimos para probar la precisión del modelo de línea de base de Microsoft de voz a texto o de un modelo personalizado. Tenga en cuenta que los datos de audio se utilizan para inspeccionar la precisión de la voz con respecto al rendimiento de un modelo específico. Si busca cuantificar la precisión de un modelo, utilice los [datos de transcripción de audio y con etiqueta humana](#audio--human-labeled-transcript-data-for-testingtraining).

Utilice esta tabla para asegurarse de que los archivos de audio están formateados correctamente para su uso con Custom Speech:

| Propiedad | Valor |
|----------|-------|
| Formato de archivo | RIFF (WAV) |
| Velocidad de muestreo | 8000 o 16 000 Hz |
| Canales | 1 (mono) |
| Longitud máxima por audio | 2 horas |
| Formato de ejemplo | PCM, 16 bits |
| Formato de archivo | .zip |
| Tamaño de archivo máximo | 2 GB |

Si el audio no satisface estas propiedades o desea comprobar si lo hace, le sugerimos que descargue [sox](http://sox.sourceforge.net) para comprobar o convertir el audio. A continuación se muestran algunos ejemplos de cómo se puede realizar cada una de estas actividades mediante la línea de comandos:

| Actividad | DESCRIPCIÓN | Comando SOX |
|----------|-------------|-------------|
| Comprobar el formato de audio | Utilice este comando para comprobar el formato de archivo de audio. | `sox --i <filename>` |
| Convertir formato de audio | Use este comando para convertir el archivo de audio para el canal único, 16 bits, 16 KHz. | `sox <input> -b 16 -e signed-integer -c 1 -r 16k -t wav <output>.wav` |

## <a name="audio--human-labeled-transcript-data-for-testingtraining"></a>Datos de transcripción de audio y con etiqueta humanos para pruebas y entrenamiento

Para medir la precisión de la precisión de la conversión de voz a texto de Microsoft al procesar sus archivos de audio, debe proporcionar transcripciones con etiqueta humana (palabra por palabra) para su comparación. Aunque la transcripción con etiqueta humana a menudo requiere mucho tiempo, es necesario evaluar la precisión y entrenar al modelo para los casos de uso. Tenga en cuenta que las mejoras en el reconocimiento solo serán tan buenas como los datos proporcionados. Por ese motivo, es importante que solo se carguen las transcripciones de alta calidad.  

| Propiedad | Valor |
|----------|-------|
| Formato de archivo | RIFF (WAV) |
| Velocidad de muestreo | 8000 o 16 000 Hz |
| Canales | 1 (mono) |
| Longitud máxima por audio | 60 s |
| Formato de ejemplo | PCM, 16 bits |
| Formato de archivo | .zip |
| Tamaño máximo de archivo zip | 2 GB |

Para abordar problemas como la eliminación o sustitución de palabras, se necesita una cantidad significativa de datos para mejorar el reconocimiento. Por lo general, se recomienda proporcionar transcripciones palabra por palabra para aproximadamente de 10 a 1000 horas de audio. Las transcripciones para todos los archivos WAV deben incluirse en un único archivo de texto sin formato. Cada línea del archivo de transcripción debe contener el nombre de uno de los archivos de audio, seguido de la transcripción correspondiente. El nombre de archivo y la transcripción deben estar separados por un carácter de tabulación (\t).

  Por ejemplo:
```
  speech01.wav  speech recognition is awesome
  speech02.wav  the quick brown fox jumped all over the place
  speech03.wav  the lazy dog was not amused
```
> [!NOTE]
> La transcripción debería estar codificada como una marca BOM UTF-8.

Las transcripciones son texto normalizado para que el sistema pueda procesarlas. Sin embargo, hay algunas normalizaciones importantes que debe hacer el usuario _antes_ de cargar los datos en el estudio de voz. Para conocer el idioma que debe usar al preparar las transcripciones, consulte [How to create a human-labeled transcription](how-to-custom-speech-human-labeled-transcriptions.md) (Creación de una transcripción con etiqueta humana).

Cuando haya reunido los archivos de audio y las transcripciones correspondientes, deben empaquetarse como un único archivo .zip antes de cargarlos en el [portal de Custom Speech](https://speech.microsoft.com/customspeech). Este es un conjunto de datos de ejemplo con tres archivos de audio y un archivo de transcripción con etiqueta humana:

![Selección del audio desde el portal de Voz](./media/custom-speech/custom-speech-audio-transcript-pairs.png)

## <a name="related-text-data-for-training"></a>Datos de texto relacionados para el entrenamiento

Si tiene nombres de productos o características que son únicos y desea asegurarse de que se reconozcan correctamente, es importante incluir datos de texto relacionados para el entrenamiento. Se pueden proporcionar dos tipos de datos de texto relacionados para mejorar el reconocimiento:

| Tipo de datos | Mejora del reconocimiento por estos datos |
|-----------|------------------------------------|
| Expresiones o frases | Los datos pueden mejorar la precisión al reconocer nombres de productos o vocabulario específico del sector dentro del contexto de una frase. |
| Pronunciaciones | Los datos pueden mejorar la pronunciación de términos poco comunes, acrónimos u otras palabras con pronunciaciones indefinidas. |

Las expresiones se pueden proporcionar como archivos de texto individuales o múltiples. Cuanto más cerca se encuentren los datos de texto de lo que se va a decir, mayor será la probabilidad de que se mejore la precisión. Las pronunciaciones deben proporcionarse como un único archivo de texto. Todo se puede empaquetar como un único archivo zip y cargarse en el [portal de Custom Speech](https://speech.microsoft.com/customspeech).

### <a name="guidelines-to-create-an-utterances-file"></a>Directrices para crear un archivo de expresiones

Para crear un modelo personalizado mediante texto relacionado, deberá proporcionar una lista de expresiones de ejemplo. Estas expresiones no necesitan ser frases completas o gramaticalmente correctas, pero deben reflejar con precisión la entrada de voz que se espera en producción. Si desea que ciertos términos tengan mayor peso, puede agregar varias frases al archivo de datos relacionados que incluyan estos términos específicos.

Utilice esta tabla para asegurarse de que el archivo de datos relacionado con las expresiones esté formateado correctamente:

| Propiedad | Valor |
|----------|-------|
| Codificación de texto | BOM UTF-8 |
| Número de expresiones por línea | 1 |
| Tamaño de archivo máximo | 200 MB |

Además, querrá tener en cuenta las siguientes restricciones:

* Evite repetir los caracteres más de cuatro veces. Por ejemplo: "aaaa" o "uuuu".
* No utilice caracteres especiales o caracteres UTF-8 por encima de U+00A1.
* Se rechazarán los identificadores URI.

### <a name="guidelines-to-create-a-pronunciation-file"></a>Directrices para crear un archivo de pronunciación

Si hay términos poco comunes sin pronunciaciones estándar que los usuarios van a encontrar o utilizar, puede proporcionar un archivo de pronunciación personalizado para mejorar el reconocimiento.

> [!IMPORTANT]
> No se recomienda utilizar esta característica para alterar la pronunciación de palabras comunes.

Esto incluye ejemplos de una expresión hablada y una pronunciación personalizada para cada uno:

| Formulario reconocido o mostrado | Formato hablado |
|--------------|--------------------------|
| 3CPO | CI-TRI-PI-OU |  
| CNTK | CI EN TI KEI |
| IEEE | AI TRIPLE I |

La forma hablada es la secuencia fonética deletreada. Puede estar compuesta de letras, palabras, sílabas o una combinación de las tres.

La pronunciación personalizada está disponible en inglés (en-US) y en alemán (de-DE). En esta tabla se muestran los caracteres admitidos por idioma:

| Idioma | Configuración regional | Caracteres |
|----------|--------|------------|
| English | en-US | a, b, c, d, e, f, g, h, i, j, k, l, m, n, o, p, q, r, s, t, u, v, w, x, y, z |
| Alemán | de-DE | ä, ö, ü, a, b, c, d, e, f, g, h, i, j, k, l, m, n, o, p, q, r, s, t, u, v, w, x, y, z |

Utilice esta tabla para asegurarse de que el archivo de datos relacionado con las pronunciaciones esté formateado correctamente. Los archivos de pronunciación son pequeños y no deberían superar unos pocos KB.

| Propiedad | Valor |
|----------|-------|
| Codificación de texto | BOM UTF-8 (también se admite ANSI con el inglés) |
| Número de pronunciaciones por línea | 1 |
| Tamaño de archivo máximo | 1 MB (1 KB por cada nivel gratis) |

## <a name="next-steps"></a>Pasos siguientes

* [Inspección de los datos](how-to-custom-speech-inspect-data.md)
* [Evaluación de los datos](how-to-custom-speech-evaluate-data.md)
* [Entrenamiento del modelo](how-to-custom-speech-train-model.md)
* [Implementación del modelo](how-to-custom-speech-deploy-model.md)
