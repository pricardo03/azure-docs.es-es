---
title: 'Flujo de control de Conversation Learner: Microsoft Cognitive Services | Microsoft Docs'
titleSuffix: Azure
description: Obtenga información sobre el flujo de control de Conversation Learner.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.subservice: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: e0a0a88e249c0a032e5afaeea14b9b3cfcbdc319
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60322946"
---
## <a name="control-flow"></a>Flujo de control

En este documento se describe el flujo de control de Conversation Learner (CL) como se muestra en el diagrama siguiente.

![](media/controlflow.PNG)

1. El usuario escribe un término o frase en el bot, por ejemplo, "¿Qué tiempo hace en Seattle?".
1. CL pasa la entrada del usuario a un modelo de aprendizaje automático que extrae las entidades.
   - Este modelo lo compila Conversation Learner y se hospeda en www.luis.ai.
1. Todas las entidades extraídas y el texto de entrada del usuario se pasan al método Devolución de llamada de detección de entidades en el código del bot.
    - Este código puede establecer/borrar/manipular valores de entidades.
1. La red neuronal de CL adopta la salida de la extracción de entidades y la entrada del usuario y puntúa todas las acciones definidas en el bot.
   - En este ejemplo, acción de máxima probabilidad es ofrecer la previsión meteorológica.

     ![](media/controlflow_forecast.PNG)

1. La acción seleccionada, en este caso, requiere una llamada API para recuperar la previsión meteorológica. 
1. Luego se invoca esta API, que se ha registrado con el método CL.AddCallback.  El resultado de esta API se devuelve después al usuario como mensaje; por ejemplo, "Soleado con un valor alto de 67".
1. A continuación, se realiza la llamada a la red neuronal para determinar la acción siguiente en función del paso anterior.
1. La red neuronal después predice el siguiente conjunto de posibles acciones y la acción seleccionada se presenta al usuario, en este caso, "¿Algo más?".

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Enseñar con factor de conversación](./how-to-teach-cl.md)
