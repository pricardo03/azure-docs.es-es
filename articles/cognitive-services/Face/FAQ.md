---
title: Preguntas frecuentes sobre Face API
titlesuffix: Azure Cognitive Services
description: Estas son las respuestas a las preguntas más habituales sobre el servicio Face API.
services: cognitive-services
author: SteveMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: conceptual
ms.date: 01/26/2017
ms.author: sbowles
ms.openlocfilehash: ceaffebde65402db385ff8b906d77c2ddadc6f97
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/07/2019
ms.locfileid: "55860957"
---
# <a name="face-api-frequently-asked-questions"></a>Preguntas frecuentes sobre Face API

### <a name="if-you-cant-find-answers-to-your-questions-in-this-faq-try-asking-the-face-api-community-on-stackoverflowhttpsstackoverflowcomquestionstaggedproject-oxfordormicrosoft-cognitive-or-contact-help-and-support-on-uservoicehttpscognitiveuservoicecom"></a>Si no puede encontrar respuestas a sus preguntas en estas P+F, puede plantearlas en la comunidad de Face API en [StackOverflow](https://stackoverflow.com/questions/tagged/project-oxford+or+microsoft-cognitive) o ponerse en contacto con Ayuda y soporte técnico en [UserVoice](https://cognitive.uservoice.com/).

-----
**Pregunta**: ¿Qué factores pueden reducir la precisión de Face API en el reconocimiento, la verificación y la búsqueda de similitudes?

**Respuesta**: Por lo general, son los mismos casos en los que los humanos tienen dificultades para identificar a alguien, por ejemplo:
* Obstáculos que bloquean uno o los dos ojos
* Retroiluminación, por ejemplo, fuerte contraluz
* Cambios en el peinado o vello facial
* Cambios debidos a la edad
* Expresiones faciales extremas (p. ej. chillidos)

Face API funciona bien normalmente en casos como estos, pero la precisión puede ser menor. Para que el reconocimiento sea más fiable y solucionar estas dificultades, entrene a sus personas con fotos que incluyan diversos ángulos e iluminación.

-----
**Pregunta**:  Estoy pasando los datos de imagen binarios, pero recibo un error "imagen de cara no válida".

**Respuesta**:  Esto implica que el algoritmo ha tenido un problema al analizar la imagen. Las causas son:
* Los formatos de imagen de entrada admitidos son JPEG, PNG, GIF(el primer fotograma) y BMP.
* El tamaño de archivo de imagen no debe ser superior a 4 MB.
* El intervalo de tamaños de cara detectable es 36 x 36 a 4096 x 4096 píxeles. Las caras que se encuentren fuera de este intervalo no se detectarán.
* Algunas caras podrían no detectarse debido a dificultades técnicas; por ejemplo, ángulos de cara muy grandes (poses), oclusión grande. Las caras de frente y casi de frente obtienen los mejores resultados.

-----
