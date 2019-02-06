---
title: Preguntas frecuentes sobre Emotion API
titlesuffix: Azure Cognitive Services
description: Proporciona respuestas a las preguntas más frecuentes sobre Emotion API.
services: cognitive-services
author: anrothMSFT
manager: cgronlun
ms.service: cognitive-services
ms.subservice: emotion-api
ms.topic: conceptual
ms.date: 01/26/2017
ms.author: anroth
ROBOTS: NOINDEX
ms.openlocfilehash: ded91c6de498b130cc26109a70e89955dd70d862
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2019
ms.locfileid: "55208999"
---
# <a name="emotion-api-frequently-asked-questions"></a>Preguntas frecuentes sobre Emotion API

> [!IMPORTANT]
> Emotion API dejará de usarse el 15 de febrero de 2019. La funcionalidad de reconocimiento de emociones está ahora disponible con carácter general como parte de [Face API](https://docs.microsoft.com/azure/cognitive-services/face/).

### <a name="if-you-cant-find-answers-to-your-questions-in-this-faq-try-asking-the-emotion-api-community-on-stackoverflowhttpsstackoverflowcomquestionstaggedproject-oxfordormicrosoft-cognitive-or-contact-help-and-support-on-uservoicehttpscognitiveuservoicecom"></a>Si no puede encontrar las respuestas a sus preguntas en estas Preguntas más frecuentes, pregúntelas en la comunidad de Emotion API en [StackOverflow](https://stackoverflow.com/questions/tagged/project-oxford+or+microsoft-cognitive) o póngase en contacto con Ayuda y soporte técnico en [UserVoice](https://cognitive.uservoice.com/).  

-----

**Pregunta**: *¿Qué tipos de imágenes consiguen los mejores resultados de Emotion API?*

**Respuesta**: Use imágenes faciales frontales completas y despejadas para obtener los mejores resultados. La confiabilidad disminuye con caras frontales parciales y Emotion API podría no reconocer emociones en imágenes de caras giradas más de 45 grados.

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

**Respuesta**: Use una instancia de Emotion API basada en imágenes y llámela en cada fotograma u omita fotogramas para un mejor rendimiento.  Hay ejemplos disponibles del análisis de fotograma a fotograma.

-----

**Pregunta**: *Paso los datos de imagen binarios, pero me devuelve el mensaje: "Imagen de rostro no válida.**

**Respuesta**: Este mensaje implica que el algoritmo ha tenido un problema al analizar la imagen.  
* Los formatos de imagen de entrada admitidos son JPEG, PNG, GIF(el primer fotograma) y BMP.
* El tamaño de archivo de imagen no debe ser superior a 4 MB.
* El intervalo de tamaño de cara detectable es 36 x 36 a 4096 × 4096 píxeles. Las caras que se encuentren fuera de este intervalo no se detectarán.
* Es posible que algunas caras no se detecten debido a los desafíos técnicos, como ángulos de cara grandes (poses) y oclusión grande. Las caras de frente y casi de frente obtienen los mejores resultados.

-----
