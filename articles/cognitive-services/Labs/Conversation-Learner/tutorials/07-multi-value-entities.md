---
title: 'Uso de entidades de varios valores con un modelo de Conversation Learner: Microsoft Cognitive Services | Microsoft Docs'
titleSuffix: Azure
description: Obtenga información sobre cómo utilizar entidades de varios valores con un modelo de Conversation Learner.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 2363dde7dc2462adde730fa9a4883ffb6c558f3f
ms.sourcegitcommit: 295babdcfe86b7a3074fd5b65350c8c11a49f2f1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/27/2018
ms.locfileid: "53796666"
---
# <a name="how-to-use-multi-value-entities-with-a-conversation-learner-model"></a>Uso de entidades de varios valores con un modelo de Conversation Learner
Este tutorial muestra la propiedad de varios valores de las entidades.

## <a name="video"></a>Vídeo

[![Vista preliminar del tutorial de entidades de varios valores](https://aka.ms/cl_Tutorial_v3_MultiValued_Preview)](https://aka.ms/cl_Tutorial_v3_MultiValued)

## <a name="requirements"></a>Requisitos
Para poder realizar este tutorial debe ejecutar el bot del tutorial general

    npm run tutorial-general

## <a name="details"></a>Detalles
Las entidades de varios valores acumulan valores en una lista, en lugar de almacenar un valor único.  Estas entidades son útiles cuando los usuarios pueden especificar más de un valor. Los ingredientes de una pizza, por ejemplo.

Las entidades marcadas como de varios valores tendrán cada instancia reconocida de la entidad anexada a una lista en la memoria del bot. El reconocimiento posterior se anexa al valor de la entidad, en lugar de sobrescribirse.

## <a name="steps"></a>Pasos

### <a name="create-the-model"></a>Creación del modelo

1. En la interfaz de usuario web, haga clic en "New Model" (Modelo nuevo).
2. En el campo "Name" (Nombre), escriba "EntidadesMultivalor" y pulse Entrar.
3. Haga clic en el botón "Create" (Crear).

### <a name="entity-creation"></a>Creación de la entidad

1. En el panel izquierdo, haga clic en "Entities" (Entidades) y, a continuación, en el botón "New Entity" (Nueva entidad).
2. Seleccione "Custom Trained" (Entrenada de forma personalizada) para "Entity Type" (Tipo de entidad).
3. Escriba "ingredientes" en "Entity Name" (Nombre de entidad).
4. Active la casilla "Multi-valued" (Varios valores).
    - Las entidades de varios valores acumulan uno o más valores en la entidad.
5. Active la casilla "Negatable" (Negable).
    - La propiedad "Negatable" se ha tratado en otro tutorial.
6. Haga clic en el botón "Create" (Crear).

![](../media/tutorial6_entities.PNG)

### <a name="create-the-first-action"></a>Creación de la primera acción

1. En el panel izquierdo, haga clic en "Actions" (Acciones) y, a continuación, en el botón "New Action" (Nueva acción).
2. En el campo "Bot's response..." (Respuesta del bot), escriba "Estos son sus ingredientes: $ingredientes".
    - El signo del dólar inicial indica una referencia de entidad
3. Haga clic en el botón "Create" (Crear).

### <a name="create-the-second-action"></a>Creación de la segunda acción

1. En el panel izquierdo, haga clic en "Actions" (Acciones) y, a continuación, en el botón "New Action" (Nueva acción).
2. En el campo "Bot's response..." (Respuesta del bot), escriba "¿Qué ingredientes quiere?"
3. En el campo "Disqualifying Entities" (Entidades descalificadoras), escriba "ingredientes".
4. Haga clic en el botón "Create" (Crear).

Ahora tiene dos acciones.

![](../media/tutorial6_actions.PNG)

### <a name="train-the-model"></a>Entrenamiento del modelo

1. En el panel izquierdo, haga clic en "Train Dialogs" (Diálogos de entrenamiento) y, a continuación, en el botón "New Train Dialog" (Nuevo diálogo de entrenamiento).
2. En el panel del chat, donde se indica "Type your message..." (Escriba su mensaje…), escriba "hola".
3. Haga clic en el botón "Score Actions" (Puntuar acciones).
4. Seleccione la respuesta, "¿Qué ingredientes quiere?"
    - El percentil es 100 %, ya que la única acción válida se basa en las restricciones.
5. En el panel del chat, donde se indica "Type your message..." (Escriba su mensaje…), escriba "queso y champiñones".
6. Haga clic en "queso" y elija la etiqueta "+ ingredientes"
7. Haga clic en "champiñones" y elija la etiqueta "+ ingredientes"
8. Haga clic en el botón "Score Actions" (Puntuar acciones).
9. Seleccione la respuesta, "Here are your toppings: $toppings" (Estos son sus ingredientes: $ingredientes).
10. En el panel del chat, donde se indica "Type your message..." (Escriba su mensaje…), escriba "agregar pimienta".
11. Haga clic en "pimienta" y elija la etiqueta "+ ingredientes"
12. Haga clic en el botón "Score Actions" (Puntuar acciones).
13. Seleccione la respuesta, "Here are your toppings: $toppings" (Estos son sus ingredientes: $ingredientes).
14. En el panel del chat, donde se indica "Type your message..." (Escriba su mensaje…), escriba "quitar queso".
15. Haga clic en "queso" y elija la etiqueta "- ingredientes"
16. Haga clic en el botón "Score Actions" (Puntuar acciones).
17. Seleccione la respuesta, "Here are your toppings: $toppings" (Estos son sus ingredientes: $ingredientes).

![](../media/tutorial5_dialogs.PNG)

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Entidades previamente entrenadas](./08-pre-trained-entities.md)
