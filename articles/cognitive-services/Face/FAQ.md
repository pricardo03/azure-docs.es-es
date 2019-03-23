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
ms.openlocfilehash: 47ce80e1b0cefc01752d2445b751ebe1c2d65d08
ms.sourcegitcommit: 87bd7bf35c469f84d6ca6599ac3f5ea5545159c9
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/22/2019
ms.locfileid: "58351344"
---
# <a name="face-api-frequently-asked-questions"></a>Preguntas frecuentes sobre Face API

> [!TIP]
> Si no puede encontrar respuestas a sus preguntas en estas P+F, puede plantearlas en la comunidad de Face API en [StackOverflow](https://stackoverflow.com/questions/tagged/project-oxford+or+microsoft-cognitive) o ponerse en contacto con Ayuda y soporte técnico en [UserVoice](https://cognitive.uservoice.com/).

-----
**Pregunta**: ¿Qué factores pueden reducir la precisión de Face API de reconocimiento, comprobación o buscar similares?

**Respuesta**: Por lo general es los mismos casos donde los seres humanos tienen dificultades para identificar un usuario incluido:
* Obstáculos que bloquean uno o los dos ojos
* Iluminación fuerte (por ejemplo, retroiluminación graves)
* Cambios en el peinado o vello facial
* Cambios debidos a la edad
* Extremas expresiones faciales (por ejemplo, bajo protesta)

Face API a menudo es correcta en un desafío casos como el anterior, pero se puede reducir la precisión. Para que el reconocimiento sea más fiable y solucionar estas dificultades, entrene a sus personas con fotos que incluyan diversos ángulos e iluminación.

-----
**Pregunta**:  Estoy pasando los datos de imagen binarios, pero recibo un error "imagen de cara no válida".

**Respuesta**:  Este error implica que el algoritmo ha habido un problema al analizar la imagen. Las causas son:
* Los formatos de imagen de entrada admitidos son JPEG, PNG, GIF(el primer fotograma) y BMP.
* El tamaño de archivo de imagen no debe ser superior a 4 MB.
* El intervalo de tamaños de cara detectable es 36 x 36 a 4096 x 4096 píxeles. Las caras que se encuentren fuera de este intervalo no se detectarán.
* Algunas caras podrían no detectarse debido a desafíos técnicos, por ejemplo, ángulos de cara grandes (poses), oclusión grande. Las caras de frente y casi de frente obtienen los mejores resultados.

-----
