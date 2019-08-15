---
title: 'Solución de problemas: Personalizer'
titleSuffix: Azure Cognitive Services
description: En este artículo se pueden encontrar preguntas para solucionar problemas sobre Personalizer.
author: diberry
manager: nitinme
services: cognitive-services
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: conceptual
ms.date: 06/15/2019
ms.author: diberry
ms.openlocfilehash: 9f4c4129217923f7fb32996f7447ed09a034f888
ms.sourcegitcommit: 0f54f1b067f588d50f787fbfac50854a3a64fff7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/12/2019
ms.locfileid: "68955221"
---
# <a name="personalizer-troubleshooting"></a>Solución de problemas de Personalizer

En este artículo se incluyen respuestas a preguntas para solucionar problemas sobre Personalizer.

## <a name="learning-loop"></a>Bucle de aprendizaje

### <a name="the-learning-loop-doesnt-seem-to-learn-how-do-i-fix-this"></a>El bucle de aprendizaje parece que no aprende. ¿Cómo puedo corregirlo?

El bucle de aprendizaje necesita unas pocas miles de llamadas a Reward antes de que las llamadas a Rank se prioricen eficazmente. 

Si no está seguro de cómo se está comportando el bucle de aprendizaje actualmente, ejecute una [evaluación sin conexión](concepts-offline-evaluation.md) y aplique la directiva de aprendizaje corregido. 

## <a name="next-steps"></a>Pasos siguientes

[Configuración de la frecuencia de actualización del modelo](how-to-settings.md#model-update-frequency)