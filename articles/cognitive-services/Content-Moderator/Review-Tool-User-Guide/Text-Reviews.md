---
title: 'Revisión de texto en Azure Content Moderator: Content Moderator'
description: Aprenda a revisar texto en Content Moderator para ver su puntuación y las etiquetas detectadas. Use la información para determinar si el contenido es adecuado.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: article
ms.date: 01/10/2019
ms.author: sajagtap
ms.openlocfilehash: 896956653ed924b7e30e63c1ae5cfa90dd0cc94d
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2019
ms.locfileid: "55219794"
---
# <a name="let-human-reviewers-review-text"></a>Permitir que revisores humanos revisen texto

Puede usar Azure Content Moderator para revisar texto mediante puntuaciones y etiquetas detectadas. Use la información para determinar si el contenido es adecuado. 

## <a name="select-or-enter-the-text-to-review"></a>Seleccionar o escribir el texto para revisar

En Content Moderator, seleccione la pestaña **Try** (Probar). A continuación, seleccione la opción **Text** (Texto) para ir a la pantalla de inicio de moderación de texto. Escriba cualquier texto o envíe el texto de ejemplo predeterminado para la moderación automática de texto. Puede escribir hasta 1024 caracteres.

## <a name="get-ready-to-review-results"></a>Prepararse para revisar los resultados

La herramienta de revisión llama primero a Text Moderation API. A continuación, genera las revisiones de texto mediante las etiquetas detectadas. La herramienta de revisión busca coincidencias con los resultados de puntuación para consideración de su equipo.

## <a name="review-text-results"></a>Revisar los resultados de texto

Aparecen resultados detallados en las ventanas. Los resultados incluyen etiquetas detectadas y términos devueltos por Text Moderation API. Para alternar el estado de selección de una etiqueta, seleccione la etiqueta. También puede trabajar con etiquetas personalizadas que haya creado.

![Captura de pantalla de la herramienta de revisión que muestra el texto marcado en una ventana del explorador Chrome](../images/reviewresults_text.png)
