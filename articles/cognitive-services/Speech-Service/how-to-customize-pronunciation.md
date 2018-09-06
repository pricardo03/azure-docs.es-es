---
title: Servicio Speech de Azure Cognitive Services
description: Obtenga información sobre cómo personalizar la pronunciación con Speech Service para Cognitive Services.
services: cognitive-services
author: PanosPeriorellis
ms.service: cognitive-services
ms.component: custom-speech
ms.topic: article
ms.date: 07/02/2018
ms.author: panosper
ms.openlocfilehash: 93fec1ea78263798588a43b2314ffdea736cdbbc
ms.sourcegitcommit: b5ac31eeb7c4f9be584bb0f7d55c5654b74404ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/23/2018
ms.locfileid: "42745358"
---
# <a name="enable-custom-pronunciation"></a>Habilitar la pronunciación personalizada
Con el uso de la pronunciación personalizada, puede definir el formato fonético y mostrar una palabra o un término. Es útil para controlar términos personalizados, como nombres de producto o acrónimos. Basta con un archivo de pronunciación (un archivo .txt simple).

Aquí le mostramos cómo funciona: En un único archivo .txt, puede especificar varias entradas de pronunciación personalizada. La estructura es como sigue:

```
Display form <Tab> Spoken form <Newline>
```

En la tabla siguiente se muestran varios ejemplos:

| Formato mostrado | Formato hablado |
|----------|-------|
| C3PO | ce tres pe o |
| L8R | ele ocho erre |
| CNTK | ce ene té k|

## <a name="requirements-for-the-spoken-form"></a>Requisitos del formato hablado
El formato hablado debe estar en minúsculas, que se puede forzar durante la importación. Además, debe proporcionar comprobaciones en el importador de datos. No se permite ninguna pestaña en el formato hablado o el formato mostrado. Sin embargo, puede haber más caracteres prohibidos en el formato mostrado (por ejemplo, ~ y ^).

Cada archivo .txt puede tener varias entradas, como se muestra en la siguiente imagen:

![Ejemplos de pronunciación de acrónimos](media/stt/custom-speech-pronunciation-file.png)

El formato hablado es la secuencia fonética del formato mostrado. Se compone de letras, palabras o sílabas. Actualmente, no hay instrucciones adicionales ni un conjunto de estándares que ayuden a formular el formato hablado. 

## <a name="supported-pronunciation-characters"></a>Caracteres de pronunciación admitidos
La pronunciación personalizada solo se admite en inglés (en-US) y en alemán (de-de) en este momento. Los juegos de caracteres que pueden usarse para expresar el formato hablado de un término (en el archivo de pronunciación personalizada) se muestran en la tabla siguiente: 

| Idioma | Caracteres |
|---------- |----------|
| Inglés (en-US) | a, b, c, d, e, f, g, h, i, j, k, l, o, p, q, r, s, t, u, v, w, x, y, z |
| Alemán (de-de) | ä, ö, ü, ẞ, a, b, c, d, e, f, g, h, i, j, k, l, o, p, q, r, s, t, u, v, w, x, y, z |

> [!NOTE]
> El formato mostrado de un término (en un archivo pronunciación) debe escribirse igual en un conjunto de datos de adaptación de lenguaje.

## <a name="requirements-for-the-display-form"></a>Requisitos para el formato mostrado
Un formato mostrado solo puede ser una palabra personalizada, un término, un acrónimo o palabras compuestas que combinen palabras existentes. También puede especificar pronunciaciones alternativas para palabras comunes. 

>[!NOTE]
>No se recomienda usar esta característica para volver a formular palabras comunes o para modificar el formato hablado. Es mejor ejecutar el descodificador para ver si algunas palabras poco habituales (por ejemplo, abreviaturas, palabras técnicas o palabras en otros idiomas) no se han descodificado correctamente. Si es así, agréguelas al archivo pronunciación personalizada. En el modelo de lenguaje, debe usar siempre el formato mostrado de una palabra y solo ese formato. 

## <a name="requirements-for-the-file-size"></a>Requisitos para el tamaño de archivo
El tamaño del archivo .txt que contiene las entradas de pronunciación está limitado a 1 megabyte (MB). Por lo general, no es necesario cargar grandes cantidades de datos con este archivo. La mayoría de los archivos de pronunciación personalizada suelen tener un tamaño de varios kilobytes (KB). La codificación del archivo .txt para todas las configuraciones regionales debería ser BOM UTF-8. En la configuración regional en inglés, ANSI también es aceptable.

## <a name="next-steps"></a>Pasos siguientes
* Mejorar la precisión del reconocimiento mediante la creación de un [modelo acústico personalizado](how-to-customize-acoustic-models.md).
* Mejorar la precisión del reconocimiento mediante la creación de un [modelo de lenguaje personalizado](how-to-customize-language-model.md).
 