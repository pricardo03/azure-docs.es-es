---
title: Cómo preparar los datos de voz personalizados - servicios de voz
titlesuffix: Azure Cognitive Services
description: Cree una voz personalizados para su marca con servicios de voz de Azure. Proporcionar las grabaciones de studio y los scripts asociados, el servicio genera un modelo de voz única atento a la voz grabada. Utilice esta voz para la síntesis de voz en sus productos, herramientas y aplicaciones.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 05/06/2019
ms.author: erhopf
ms.openlocfilehash: 18e1bb486c47baf7648a74e31451e2db73f72250
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65156864"
---
# <a name="prepare-data-to-create-a-custom-voice"></a>Preparar datos para crear una voz personalizada

Cuando esté listo para crear una voz personalizada para su aplicación, el primer paso es reunir las grabaciones de audio y los scripts asociados para iniciar el entrenamiento del modelo de voz. El servicio usa estos datos para crear una optimizado para que coincida con la voz en las grabaciones de voz única. Una vez que haya entrenado la voz, puede iniciar la síntesis de voz en sus aplicaciones.

Puede comenzar con una pequeña cantidad de datos para crear una prueba de concepto. Sin embargo, cuantos más datos que proporcione, más natural sonará su voz personalizada. Antes de que puede entrenar su propio modelo de voz, necesitará las grabaciones de audio y las transcripciones de texto asociado. En esta página, revisaremos los tipos de datos, cómo se usan y cómo administrar cada uno.

## <a name="data-types"></a>Tipos de datos

Un conjunto de datos de entrenamiento de voz incluye las grabaciones de audio y un archivo de texto con las transcripciones asociados. Cada archivo de audio debe contener una sola declaración (una frase única o una sola activar para un sistema de cuadro de diálogo) y ser menos de 15 segundos de duración.

En algunos casos, no puede tener el conjunto de datos derecho listo y deseará probar el entrenamiento de voz personalizados con archivos de audio disponibles, cortos o largos, con o sin las transcripciones. Proporcionamos herramientas (beta) para ayudarle a segmentar el audio en grabaciones de voz y preparar las transcripciones utilizando el [API de Batch transcripción](batch-transcription.md).

Esta tabla enumeran los tipos de datos y cómo cada uno se usa para crear un modelo de voz personalizados.

| Tipo de datos | DESCRIPCIÓN | Cuándo se deben usar | Servicio adicional necesario | Cantidad para entrenar un modelo | Configuraciones regionales |
| --------- | ----------- | ----------- | --------------------------- | ----------------------------- | --------- |
| **Grabaciones de voz individuales + transcripción coincidente** | Una colección (.zip) de los archivos de sonido (.wav) como declaraciones individuales. Cada archivo de audio debe ser de 15 segundos o menos de longitud, emparejado con una transcripción con formato (.txt). | Grabaciones de profesionales con coincidencia de transcripciones | Listo para el entrenamiento. | Ningún requisito de disco duro para en-US y zh-CN. Más de 2.000 + declaraciones diferentes para otras configuraciones regionales. | Todas las configuraciones regionales de voz personalizado |
| **Long audio + transcripciones (beta)** | Una colección (.zip) de largo, sin segmentar los archivos de audio (más de 20 segundos), se empareja con una transcripción (.txt) que contiene todas las palabras habladas. | Tiene archivos de audio y transcripciones coincidentes, pero no se segmentan en declaraciones. | Segmentación (mediante la transcripción de batch).<br>Transformación de formato de audio cuando sea necesario. | Ningún requisito de disco duro para en-US y zh-CN. | `en-US` y `zh-CN` |
| **Solo de audio (beta)** | Una colección (.zip) de los archivos de audio sin una transcripción. | Solo tiene archivos de audio disponibles sin las transcripciones. | Segmentación + generación de transcripción (mediante la transcripción de batch).<br>Transformación de formato de audio cuando sea necesario.| Sin necesidad de disco duro de `en-US` y `zh-CN`. | `en-US` y `zh-CN` |

Los archivos deben ser agrupados por tipo en un conjunto de datos y cargar como un archivo zip. Cada conjunto de datos solo puede contener un tipo de datos único.

> [!NOTE]
> El número máximo de conjuntos de datos permitida la importación por suscripción es 10 .zip gratuitamente archivos de usuarios de la suscripción (F0) y 500 para usuarios de la suscripción estándar (S0).

## <a name="individual-utterances--matching-transcript"></a>Grabaciones de voz individuales + transcripción coincidente

Puede preparar las grabaciones de grabaciones de voz individuales y la transcripción coincidente de dos maneras. Ya sea escribir un script y que se lee un talento de voz o usan audio disponible públicamente y transcribirlo al texto. En este último caso, deberá editar las disfluencias de los archivos de audio, como las muletillas ("em") y otros sonidos de relleno, tartamudeos, palabras entre dientes o pronunciaciones erróneas.

Para generar una fuente de voz buena, cree las grabaciones en una sala silenciosa con un micrófono de alta calidad. Volumen coherente, habla de velocidad de habla el cabeceo y la presentación expresivo de voz son esenciales.

> [!TIP]
> Para crear una voz que se vaya a usar en una producción, le recomendamos que use un estudio de grabación profesional y un locutor. Para obtener más información, consulte [Cómo grabar ejemplos de voz para una voz personalizada](https://review.docs.microsoft.com/azure/cognitive-services/speech-service/record-custom-voice-samples).

### <a name="audio-files"></a>Archivos de audio

Cada archivo de audio debe contener una sola declaración (una frase única o una sola vuelven de un sistema de cuadro de diálogo), menos de 15 segundos de duración. Todos los archivos deben estar en el mismo idioma hablado. Con la excepción bilingües chino-inglés, multilingües voces de texto a voz personalizadas no se admiten. Cada archivo de audio debe tener un nombre de archivo numérico único con la extensión de nombre de archivo .wav.

Siga estas directrices cuando prepare el audio.

| Propiedad | `Value` |
| -------- | ----- |
| Formato de archivo | RIFF (.wav), que se agrupan en un archivo .zip |
| Frecuencia de muestreo | al menos 16 000 Hz |
| Formato de ejemplo | PCM, 16 bits |
| Nombre de archivo | Numérico, con la extensión. wav. No hay nombres de archivo duplicado permitidos. |
| Longitud de audio | Menos de 15 segundos |
| Formato de archivo | .zip |
| Tamaño de archivo máximo | 200 MB |

> [!NOTE]
> se rechazarán los archivos .wav con una frecuencia de muestreo inferior a 16.000 Hz. Si un archivo .zip contiene archivos .wav con distintas frecuencias de muestreo, se importará solo esos igual o superior a 16.000 Hz. Actualmente el portal importa archivos .zip de hasta 200 MB. Sin embargo, pueden cargarse varios archivos.

### <a name="transcripts"></a>Transcripciones

El archivo de transcripción es un archivo de texto sin formato. Use estas instrucciones para preparar sus transcripciones.

| Propiedad | `Value` |
| -------- | ----- |
| Formato de archivo | Texto sin formato (.txt) |
| Formato de codificación | ANSI/ASCII, UTF-8, UTF-8-BOM, UTF-16-LE o UTF-16-BE. Zh-CN, no se admiten las codificaciones ASCII/ANSI y UTF-8. |
| Número de expresiones por línea | **Una** -cada línea del archivo de transcripción debe contener el nombre de uno de los archivos de audio, seguidos por la transcripción correspondiente. El nombre de archivo y la transcripción deben estar separados por un carácter de tabulación (\t). |
| Tamaño de archivo máximo | 50 MB |

A continuación es un ejemplo de cómo las transcripciones se organizan utterance (dictado) mediante declaración en un archivo txt:

```
0000000001[tab] This is the waistline, and it's falling.
0000000002[tab] We have trouble scoring.
0000000003[tab] It was Janet Maslin.
```
Es importante que las transcripciones son 100%, precisa transcripciones de audio correspondiente. Errores en las transcripciones presentará la pérdida de calidad durante el entrenamiento.

> [!TIP]
> Al crear la producción texto a voz voces, seleccionadas grabaciones de voz (o escribir scripts) que tengan en cuenta fonético cobertura y eficacia. ¿Tiene problemas para obtener los resultados que desea? [Póngase en contacto con la voz personalizada](mailto:speechsupport@microsoft.com) equipo averiguar más sobre nosotros consulte.

## <a name="long-audio--transcript-beta"></a>Long audio + transcripciones (beta)

En algunos casos, se puede no haber segmentada audio disponible. Se proporcionan un servicio (beta) a través del portal de voz personalizados que le ayudarán a segmentar los archivos de sonido de larga duración y transcripciones de crear. Tenga en mente, este servicio se cobrará hacia el uso de la suscripción de voz a texto.

> [!NOTE]
> El servicio prolongada audio segmentación aprovechará la característica de lote transcripción de voz a texto, que solo admite usuarios de la suscripción estándar (S0). Durante el procesamiento de la segmentación, los archivos de audio y las transcripciones también se enviará al servicio de voz personalizados para refinar el modelo de reconocimiento por lo que se puede mejorar la precisión de los datos. No hay datos se conservarán durante este proceso. Después de realiza la segmentación, solo las declaraciones para segmentar y sus expedientes de asignación se almacenarán para su descarga y entrenamiento.

### <a name="audio-files"></a>Archivos de audio

Siga estas directrices cuando prepare audio para la segmentación.

| Propiedad | `Value` |
| -------- | ----- |
| Formato de archivo | RIFF (.wav) con una frecuencia de muestreo de al menos 16 khz 16 bits en PCM o. mp3 con una velocidad de bits de al menos 256 KBps, agrupado en un archivo .zip |
| Nombre de archivo | Sólo caracteres ASCII. Se producirá un error de caracteres Unicode en el nombre (por ejemplo, los caracteres de chino, o símbolos como ":"). No hay nombres duplicados permitidos. |
| Longitud de audio | Durante más de 20 segundos |
| Formato de archivo | .zip |
| Tamaño de archivo máximo | 200 MB |

Todos los archivos de audio se deben agrupar en un archivo zip. Está bien para poner archivos .wav y. mp3 en un archivo zip de audio, pero no se permite ninguna subcarpeta en el archivo zip. Por ejemplo, puede cargar un archivo zip que contiene un archivo de audio denominado 'kingstory.wav' segundo-45-larga y otra uno con nombre 'queenstory.mp3', 200-segundo-long, sin ninguna de las subcarpetas. Todos los archivos. mp3 se transformará en el formato .wav después del procesamiento.

### <a name="transcripts"></a>Transcripciones

Las transcripciones deben estar preparadas para las especificaciones enumeradas en esta tabla. Cada archivo de audio debe coincidir con una transcripción.

| Propiedad | `Value` |
| -------- | ----- |
| Formato de archivo | Texto sin formato (.txt), se agrupan en un archivo .zip |
| Nombre de archivo | Use el mismo nombre que el archivo de audio coincidente |
| Formato de codificación | UTF-8-BOM solo |
| Número de expresiones por línea | Sin límite |
| Tamaño de archivo máximo | 50 MILLONES |

Todos los archivos de las transcripciones de este tipo de datos deben estar agrupados en un archivo zip. No se permite ninguna subcarpeta en el archivo zip. Por ejemplo, ha cargado un archivo zip que contiene un archivo de audio denominado 'kingstory.wav', 45 segundos de duración, y otro uno con nombre 'queenstory.mp3', 200 segundos el tiempo. Deberá cargar otro archivo zip que contiene las transcripciones de dos, uno con nombre 'kingstory.txt', otro un 'queenstory.txt'. Dentro de cada archivo de texto sin formato, proporcionará la transcripción completas correcta para el audio coincidente.

Después de que el conjunto de datos se ha cargado correctamente, se le permitirá segmentar el archivo de audio en declaraciones basándose en la transcripción proporcionada. Puede comprobar las palabras pronunciadas segmentadas y las transcripciones coincidentes, descargue el conjunto de datos. Los identificadores únicos se asignará automáticamente a las grabaciones de voz segmentados. Es importante que asegurarse de que las transcripciones que proporciona son 100% preciso. Errores en las transcripciones pueden reducir la precisión durante la segmentación de audio y presentan aún más la pérdida de calidad en la fase de aprendizaje que se incluye más adelante.

## <a name="audio-only-beta"></a>Solo de audio (beta)

Si no tiene transcripciones para las grabaciones de audio, use el **solo Audio** opción para cargar los datos. Nuestro sistema puede ayudar a segmentar y transcribir los archivos de audio. Tenga en cuenta, este servicio contará para el uso de la suscripción de voz a texto.

Siga estas directrices cuando prepare el audio.

> [!NOTE]
> El servicio prolongada audio segmentación aprovechará la característica de lote transcripción de voz a texto, que solo admite usuarios de la suscripción estándar (S0).

| Propiedad | `Value` |
| -------- | ----- |
| Formato de archivo | RIFF (.wav) con una frecuencia de muestreo de al menos 16 khz 16 bits en PCM o. mp3 con una velocidad de bits de al menos 256 KBps, agrupado en un archivo .zip |
| Nombre de archivo | Sólo caracteres ASCII. Se producirá un error de caracteres Unicode en el nombre (por ejemplo, los caracteres de chino, o símbolos como ":"). Ningún nombre duplicado permitido. |
| Longitud de audio | Durante más de 20 segundos |
| Formato de archivo | .zip |
| Tamaño de archivo máximo | 200 MB |

Todos los archivos de audio se deben agrupar en un archivo zip. No se permite ninguna subcarpeta en el archivo zip. Una vez que el conjunto de datos se ha cargado correctamente, se le permitirá segmentar el archivo de audio en declaraciones en función de nuestro servicio de transcripción de voz por lotes. Los identificadores únicos se asignará automáticamente a las grabaciones de voz segmentados. Las transcripciones de coincidencia, se generará con el reconocimiento de voz. Todos los archivos. mp3 se transformará en el formato .wav después del procesamiento. Puede comprobar las palabras pronunciadas segmentadas y las transcripciones coincidentes, descargue el conjunto de datos.

## <a name="next-steps"></a>Pasos siguientes

- [Creación de una voz personalizada](how-to-custom-voice-create-voice.md)
- [Guía: Registre sus ejemplos de voz](record-custom-voice-samples.md)
