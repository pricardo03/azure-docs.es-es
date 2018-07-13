---
title: 'Flujo de control de Conversation Learner: Microsoft Cognitive Services | Microsoft Docs'
titleSuffix: Azure
description: Obtenga información sobre el flujo de control de Conversation Learner.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 592ca82db7e0ab3ff89d25b61f38f8b226f3bc86
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/23/2018
ms.locfileid: "35381714"
---
## <a name="control-flow"></a>Flujo de control

En este documento se describe el flujo de control de Conversation Learner (CL) como se muestra en el diagrama siguiente.

![](media/controlflow.PNG)

1. El usuario escribe un término o frase en el bot, por ejemplo, "¿Qué tiempo hace en Seattle?".
1. CL pasa la entrada del usuario a un modelo de aprendizaje automático que extrae las entidades.
    - Este modelo lo compila Conversation Learner y se hospeda en www.luis.ai.
2. Todas las entidades extraídas y el texto de entrada del usuario se pasan al método Devolución de llamada de detección de entidades en el código del bot.
    - Este código puede establecer/borrar/manipular valores de entidades.
1. La red neuronal de CL adopta la salida de la extracción de entidades y la entrada del usuario y puntúa todas las acciones definidas en el bot.
    - En este ejemplo, acción de máxima probabilidad es ofrecer la previsión meteorológica.

![](media/controlflow_forecast.PNG)

5. La acción seleccionada, en este caso, requiere una llamada API para recuperar la previsión meteorológica. 
6. Luego se invoca esta API, que se ha registrado con el método CL.AddAPICallback.  El resultado de esta API se devuelve después al usuario como mensaje; por ejemplo, "Soleado con un valor alto de 67".
7. A continuación, se realiza la llamada a la red neuronal para determinar la acción siguiente en función del paso anterior.
8. La red neuronal después predice el siguiente conjunto de posibles acciones y la acción seleccionada se presenta al usuario, en este caso, "¿Algo más?".

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Enseñar con Conversation Learner ](./how-to-teach-cl.md)
