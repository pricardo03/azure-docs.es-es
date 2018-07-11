---
title: Preguntas frecuentes sobre Emotion API | Microsoft Docs
description: Obtenga respuestas a las preguntas más frecuentes sobre Emotion API en Cognitive Services.
services: cognitive-services
author: anrothMSFT
manager: corncar
ms.service: cognitive-services
ms.component: emotion-api
ms.topic: article
ms.date: 01/26/2017
ms.author: anroth
ms.openlocfilehash: 8532d7c00fd8d7b01d84b5e55cb9bbc60241789c
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/23/2018
ms.locfileid: "35380142"
---
# <a name="emotion-api-frequently-asked-questions"></a>Preguntas frecuentes sobre Emotion API
 
> [!IMPORTANT]
> La versión preliminar de Video API terminará el 30 de octubre de 2017. Pruebe la nueva [versión preliminar de Video Indexer API](https://azure.microsoft.com/services/cognitive-services/video-indexer/) para extraer información de los vídeos con facilidad y mejorar las experiencias de detección de contenido, como los resultados de la búsqueda, gracias al reconocimiento de texto oral, caras, caracteres y emociones. [Más información](https://docs.microsoft.com/azure/cognitive-services/video-indexer/video-indexer-overview).

### <a name="if-you-cant-find-answers-to-your-questions-in-this-faq-try-asking-the-emotion-api-community-on-stackoverflowhttpsstackoverflowcomquestionstaggedproject-oxfordormicrosoft-cognitive-or-contact-help-and-support-on-uservoicehttpscognitiveuservoicecom"></a>Si no puede encontrar las respuestas a sus preguntas en estas Preguntas más frecuentes, pregúntelas en la comunidad de Emotion API en [StackOverflow](https://stackoverflow.com/questions/tagged/project-oxford+or+microsoft-cognitive) o póngase en contacto con Ayuda y soporte técnico en [UserVoice](https://cognitive.uservoice.com/).  

-----

**Pregunta**: *¿Qué tipos de imágenes consiguen los mejores resultados de Emotion API?*

**Respuesta**: Use imágenes faciales frontales completas y despejadas para obtener los mejores resultados. La confiabilidad disminuye con caras frontales parciales y Emotion API podría no reconocer emociones en imágenes de caras giradas 45 grados o más.

-----

**Pregunta**: *¿Cuántas emociones puede identificar Emotion API?*

**Respuesta**: Emotion API reconoce ocho emociones distintas aceptadas universalmente: 
* Felicidad
* Tristeza
* Sorpresa
* Enfado
* Miedo
* Desprecio
* Desagrado 
* Neutra 

-----

**Pregunta**: *¿Hay alguna forma de pasar una secuencia de vídeo en directo a la API y obtener el resultado simultáneamente?*

**Respuesta**: Use una instancia de Emotion API basada en imágenes y llámela en cada fotograma u omita fotogramas para mejor rendimiento.  Hay ejemplos disponibles del análisis de fotograma a fotograma.

-----

**Pregunta**: *Paso los datos de imagen binarios, pero me devuelve un mensaje de imagen no válida.**

**Respuesta**: Esto implica que el algoritmo ha tenido un problema al analizar la imagen.  
* Los formatos de imagen de entrada admitidos son JPEG, PNG, GIF(el primer fotograma) y BMP. 
* El tamaño de archivo de imagen no debe ser superior a 4 MB.
* El intervalo de tamaños de cara detectable es 36x36 a 4096x4096 píxeles. Las caras que se encuentren fuera de este intervalo no se detectarán.
* Algunas caras podrían no detectarse debido a dificultades técnicas; por ejemplo, ángulos de cara muy grandes (poses), oclusión grande. Las caras de frente y casi de frente obtienen los mejores resultados.

-----
