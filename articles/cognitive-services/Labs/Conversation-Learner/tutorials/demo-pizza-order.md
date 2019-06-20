---
title: 'Modelo de Conversation Learner de demostración, pedido de pizza: Microsoft Cognitive Services | Microsoft Docs'
titleSuffix: Azure
description: Obtenga información acerca de cómo crear un modelo de Conversation Learner de demostración.
services: cognitive-services
author: nitinme
manager: nolachar
ms.service: cognitive-services
ms.subservice: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: nitinme
ms.openlocfilehash: 010245480d8e1f59d5c1b92a9e717f73b5ba7f4c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "66389139"
---
# <a name="demo-pizza-order"></a>Demostración: Pedido de pizza
Esta demostración ilustra un bot de pedido de pizza, que ayuda al pedido de una pizza mediante:

- el reconocimiento de ingredientes de pizza en las expresiones del usuario
- la administración del inventario de ingredientes y la respuesta adecuada
- el recordar pedidos anteriores y acelerar el pedido de una pizza idéntica

## <a name="video"></a>Vídeo

[![Vista previa de pedido de pizza de demostración](https://aka.ms/cl_Tutorial_v3_DemoPizzaOrder_Preview)](https://aka.ms/cl_Tutorial_v3_DemoPizzaOrder)

## <a name="requirements"></a>Requisitos
Para poder realizar este tutorial debe ejecutar el bot de pedido de pizza.

    npm run demo-pizza

### <a name="open-the-demo"></a>Abrir la demostración

En la lista de modelos de la interfaz de usuario web, haga clic en TutorialDemo Pizza Order. 

## <a name="entities"></a>Entidades

El modelo contiene tres entidades:

- En "Toppings" se acumulan los ingredientes especificados por el usuario, si están disponibles.
- "OutofStock" indica que el ingrediente seleccionado por el usuario está agotado.
- "LastToppings" contiene los ingredientes históricos de su pedido anterior.

![](../media/tutorial_pizza_entities.PNG)

### <a name="actions"></a>Acciones

El modelo contiene un conjunto de acciones que solicitan al usuario su selección de ingredientes, los ingredientes acumulados y mucho más.

También se proporcionan dos llamadas a API:

- "FinalizeOrder" controla la finalización del pedido.
- "UseLastToppings" procesa la información de los ingredientes históricos.

![](../media/tutorial_pizza_actions.PNG)

### <a name="training-dialogs"></a>Diálogos de entrenamiento

Hay varios diálogos de entrenamiento en el modelo.

![](../media/tutorial_pizza_dialogs.PNG)

Vamos a entrenar el modelo un poco más mediante la creación de otro de diálogo de entrenamiento.

1. En el panel izquierdo, haga clic en "Train Dialogs" (Diálogos de entrenamiento) y, después, en el botón "New Train Dialog" (Nuevo diálogo de entrenamiento).
2. En el panel del chat, donde dice "Type your message..." (Escriba su mensaje…), escriba "Pedir una pizza con queso".
    - El extractor de entidades extrajo la palabra "queso".
3. Haga clic en el botón "Score Actions" (Puntuar acciones).
4. Seleccione la respuesta, "Tiene queso en su pizza".
5. Seleccione la respuesta "Would you like anything else?" (¿Quiere algo más?).
6. En el panel del chat, donde dice "Type your message..." (Escriba su mensaje…), escriba "agregar champiñones y pimientos".
7. Haga clic en el botón "Score Actions" (Puntuar acciones).
8. Seleccione la respuesta, "Tiene queso, champiñones y pimientos en su pizza".
9. Seleccione la respuesta "Would you like anything else?" (¿Quiere algo más?).
10. En el panel del chat, donde dice "Type your message..." (Escriba su mensaje…), escriba "quitar pimiento y agregar salchicha".
11. Haga clic en el botón "Score Actions" (Puntuar acciones).
12. Seleccione la respuesta, "Tiene queso, champiñones y salchicha en su pizza".
13. Seleccione la respuesta "Would you like anything else?" (¿Quiere algo más?).
14. En el panel del chat, donde dice "Type your message..." (Escriba su mensaje…), escriba "agregar boniato".
15. Haga clic en el botón "Score Actions" (Puntuar acciones).
    - El valor "boniato" se agregó a "OutofStock" por el código de devolución de llamada de detección de entidad, ya que el texto no coincidía con ningún ingrediente admitido.
16. Seleccione la respuesta, "OutOfStock"
17. Seleccione la respuesta "Would you like anything else?" (¿Quiere algo más?).
18. En el panel del chat, donde dice "Type your message..." (Escriba su mensaje…), escriba "no".
    - El "no" no se marca como un tipo de intención. En su lugar, seleccionaremos la acción correspondiente en función del contexto actual.
19. Haga clic en el botón "Score Actions" (Puntuar acciones).
20. Seleccione la respuesta, "FinalizeOrder"
    - Si se selecciona esta acción, el código de devolución de llamada FinalizeOrder guarda los ingredientes actuales del cliente en la entidad "LastToppings" y elimina la entidad "Toppings".
21. En el panel del chat, donde dice "Type your message..." (Escriba su mensaje…), escriba "otro pedido".
22. Haga clic en el botón "Score Actions" (Puntuar acciones).
23. Seleccione la respuesta, "¿Quiere queso, champiñones y salchicha?"
    - Esta acción está disponible ahora debido a que se ha establecido la entidad "LastToppings".
24. En el panel del chat, donde dice "Type your message..." (Escriba su mensaje…), escriba "sí".
25. Haga clic en el botón "Score Actions" (Puntuar acciones).
26. Seleccione la respuesta, "UseLastToppings".
27. Seleccione la respuesta, "Tiene queso, champiñones y salchicha en su pizza".
28. Seleccione la respuesta "Would you like anything else?" (¿Quiere algo más?).

![](../media/tutorial_pizza_callbackcode.PNG)

![](../media/tutorial_pizza_apicalls.PNG)

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Implementar un bot de Conversation Learner](../deploy-to-bf.md)
