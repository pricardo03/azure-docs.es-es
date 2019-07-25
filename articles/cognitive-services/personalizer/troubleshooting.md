---
title: 'Solución de problemas: Personalizer'
titleSuffix: Azure Cognitive Services
description: En este artículo se pueden encontrar preguntas para solucionar problemas sobre Personalizer.
author: edjez
manager: nitinme
services: cognitive-services
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: article
ms.date: 06/15/2019
ms.author: edjez
ms.openlocfilehash: be6119d96b89622f45db1099a47e858a5893c2cb
ms.sourcegitcommit: dad277fbcfe0ed532b555298c9d6bc01fcaa94e2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/10/2019
ms.locfileid: "67722253"
---
# <a name="personalizer-troubleshooting"></a>Solución de problemas de Personalizer

En este artículo se incluyen respuestas a preguntas para solucionar problemas sobre Personalizer.

## <a name="learning-loop"></a>Bucle de aprendizaje

### <a name="the-learning-loop-doesnt-seem-to-learn-how-do-i-fix-this"></a>El bucle de aprendizaje parece que no aprende. ¿Cómo puedo corregirlo?

El bucle de aprendizaje necesita unas pocas miles de llamadas a Reward antes de que las llamadas a Rank se prioricen eficazmente. 

Si no está seguro de cómo se está comportando el bucle de aprendizaje actualmente, ejecute una [evaluación sin conexión](concepts-offline-evaluation.md) y aplique la directiva de aprendizaje corregido. 

## <a name="next-steps"></a>Pasos siguientes

[Configuración de la frecuencia de actualización del modelo](how-to-settings.md#model-update-frequency)