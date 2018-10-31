---
title: Preguntas más frecuentes sobre Computer Vision API
titlesuffix: Azure Cognitive Services
description: Obtenga respuestas a las preguntas más frecuentes sobre Computer Vision API en Azure Cognitive Services.
services: cognitive-services
author: KellyDF
manager: cgronlun
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: conceptual
ms.date: 01/26/2017
ms.author: kefre
ms.openlocfilehash: 55b474d0edbb8dc01b9f35d4f8799e53e37862df
ms.sourcegitcommit: c282021dbc3815aac9f46b6b89c7131659461e49
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/12/2018
ms.locfileid: "49166379"
---
# <a name="computer-vision-api-frequently-asked-questions"></a>Preguntas frecuentes sobre Computer Vision API

### <a name="if-you-cant-find-answers-to-your-questions-in-this-faq-try-asking-the-computer-vision-api-community-on-stackoverflowhttpsstackoverflowcomquestionstaggedproject-oxfordormicrosoft-cognitive-or-contact-help-and-support-on-uservoicehttpscognitiveuservoicecom"></a>Si no puede encontrar respuestas a sus preguntas en estas P+F, puede plantearlas en la comunidad de Computer Vision API en [StackOverflow](https://stackoverflow.com/questions/tagged/project-oxford+or+microsoft-cognitive) o ponerse en contacto con [Ayuda y soporte técnico en UserVoice](https://cognitive.uservoice.com/).

-----

**Pregunta**: *¿Puedo entrenar a Computer Vision API para usar etiquetas personalizadas?  Por ejemplo, me gustaría introducir imágenes de razas de gatos para "entrenar" a AI y luego recibir el valor de raza en una solicitud de AI.*

**Respuesta**: Esta función no está disponible actualmente. Sin embargo, nuestros ingenieros están trabajando para incorporar esta funcionalidad a Computer Vision.

-----

**Pregunta**: *¿Se puede usar Computer Vision localmente sin una conexión a Internet?*

**Respuesta**: Actualmente no ofrecemos una solución local.

-----

**Pregunta**: *¿Qué idiomas se admiten con Computer Vision?*

**Respuesta**: Los idiomas admitidos son:

| | | Idiomas admitidos | | |
|---------------- |------------------ |------------------ |--------------------------- |--------------------
| Danés (da-DK)  | Neerlandés (nl-NL)     | English           | Finés (fi-FI)            |Francés (fr-FR)
| Alemán (de-de)  | Griego (el-GR)     | Húngaro (hu-HU) | Italiano (it-IT)            | Japonés (ja-JP)
| Coreano (ko-KR)  | Noruego (nb-NO) | Polaco (pl-PL)    | Portugués (pt-BR) (pt-PT) | Ruso (ru-RU)
| Español (es-ES)   | Sueco (sv-SV)     | Turco (tr-TR)   |                            |

-----

**Pregunta**: *¿Se puede usar Computer Vision para leer matrículas?*

**Respuesta**: Vision API ofrece una buena detección de texto con OCR, pero en este momento no está actualizada para matrículas. En nuestro esfuerzo constante por mejorar nuestros servicios, hemos agregado OCR para el reconocimiento automático de matrículas a nuestra lista de solicitudes de características.

-----

**Pregunta:** *¿Qué idiomas se admiten para el reconocimiento de escritura a mano?*

**Respuesta**: Actualmente, solo se admite inglés.

-----

**Pregunta**: *¿Qué tipos de superficies se admiten para el reconocimiento de escritura a mano?*

**Respuesta**: La tecnología funciona con distintas clases de superficies, como pizarras, notas del producto y notas rápidas.

-----

**Pregunta**: *¿Cuánto tarda la operación de reconocimiento de escritura a mano?*

**Respuesta**: Depende de la longitud del texto. Con textos más largos, puede tardar hasta varios segundos. Por lo tanto, una vez finalizada la operación de reconocimiento de texto escrito a mano, puede que deba esperar antes de poder recuperar los resultados mediante la operación de obtención de resultados de la operación de texto escrito a mano.

-----

**Pregunta**: *¿Cómo trata la tecnología de reconocimiento de escritura a mano el texto que se ha insertado mediante un signo de intercalación en medio de una línea?*

**Respuesta**: La operación de reconocimiento de escritura a mano devuelve dicho texto como una línea independiente.

-----

**Pregunta**: *¿Cómo trata la tecnología de reconocimiento de escritura a mano las palabras o líneas tachadas?*

**Respuesta**: Si las palabras están tachadas con varias líneas para presentarlas como irreconocibles, la operación de reconocimiento de escritura a mano no las selecciona. Sin embargo, si las palabras están tachadas con una sola línea, ese tachado se trata como ruido y la operación de reconocimiento de escritura a mano sigue seleccionando las palabras.

-----

**Pregunta**: *¿Qué orientación de texto se admite en la tecnología de reconocimiento de escritura a mano?*

**Respuesta**: La operación de reconocimiento de escritura a mano puede seleccionar texto orientado en ángulos de entre 30 y 40 grados.

-----
