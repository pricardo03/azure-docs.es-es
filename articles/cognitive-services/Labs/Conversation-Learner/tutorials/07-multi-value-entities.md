---
title: 'Uso de entidades de varios valores con un modelo de Conversation Learner: Microsoft Cognitive Services | Microsoft Docs'
titleSuffix: Azure
description: Obtenga información sobre cómo utilizar entidades de varios valores con un modelo de Conversation Learner.
services: cognitive-services
author: nitinme
manager: nolachar
ms.service: cognitive-services
ms.subservice: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: nitinme
ms.openlocfilehash: 1f62def5e498f3f744beaed0cda207e1a75bfdf2
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "66387961"
---
# <a name="how-to-use-multi-value-entities-with-a-conversation-learner-model"></a>Uso de entidades de varios valores con un modelo de Conversation Learner
Este tutorial muestra la propiedad de varios valores de las entidades.

## <a name="video"></a>Vídeo

[![Versión preliminar del tutorial de entidades multivalor](https://aka.ms/cl_Tutorial_v3_MultiValued_Preview)](https://aka.ms/cl_Tutorial_v3_MultiValued)

## <a name="requirements"></a>Requisitos
Para poder realizar este tutorial, el bot de tutorial general debe estar en ejecución.

    npm run tutorial-general

## <a name="details"></a>Detalles
Las entidades de varios valores acumulan valores en una lista, en lugar de almacenar un valor único.  Estas entidades son útiles cuando los usuarios pueden especificar más de un valor. Los ingredientes de una pizza, por ejemplo.

Las entidades marcadas como de varios valores tendrán cada instancia reconocida de la entidad anexada a una lista en la memoria del bot. El reconocimiento posterior se anexa al valor de la entidad, en lugar de sobrescribirse.

## <a name="steps"></a>Pasos

Empiece en la página principal de la interfaz de usuario web.

### <a name="create-the-model"></a>Crear el modelo

1. Seleccione **New Model** (Nuevo modelo).
2. Escriba **MultiValueEntities** en **Name** (Nombre).
3. Seleccione **Crear**.

### <a name="entity-creation"></a>Crear la entidad

1. Seleccione **Entities** (Entidades) en el panel izquierdo y, después, **New Entity** (Nueva entidad).
2. Seleccione **Custom Trained** (Entrenamiento personalizado) en **Entity Type** (Tipo de entidad).
3. Escriba **Toppings** (Ingredientes) en **Entity Name** (Nombre de entidad).
4. Active **Multi-valued** (Multivalor) para permitir que la entidad acumule uno o varios valores.
5. Active **Negatable** (Negable).
6. Seleccione **Crear**.

![](../media/T07_entity_create.png)

### <a name="create-the-first-action"></a>Crear la primera acción

1. Seleccione **Actions** (Acciones) en el panel izquierdo y, a continuación, **New Action** (Nueva acción).
2. Escriba **Estos son sus ingredientes: $ingredientes**  en el campo **Bot's response...** (Respuesta del bot). El signo del dólar inicial indica una referencia de entidad.
3. Seleccione **Crear**.

![](../media/T07_action_create_1.png)

### <a name="create-the-second-action"></a>Crear la segunda acción

1. Seleccione **Actions** (Acciones) en el panel izquierdo y, a continuación, **New Action** (Nueva acción).
2. Escriba **What toppings would you like?** (¿Qué ingredientes quiere?) en el campo **Bot's response...** (Respuesta del bot).
3. En **Disqualifying Entities** (Entidades descalificadas), escriba **Toppings** (Ingredientes).
4. Seleccione **Crear**.

Ahora tiene dos acciones.

![](../media/T07_action_create_2.png)

### <a name="train-the-model"></a>Entrenar el modelo

1. Seleccione **Train Dialogs** (Diálogos de entrenamiento) en el panel izquierdo y, a continuación, **New Train Dialog** (Nuevo diálogo de entrenamiento).
2. Escriba **hi** (hola) como expresión del usuario en el panel izquierdo del chat.
3. Seleccione **Score Actions** (Acciones de puntuación).
4. Seleccione **What toppings would you like?** (¿Qué ingredientes quiere?) en la lista de acciones. El percentil es 100 %, ya que la única acción válida se basa en las restricciones.
5. Escriba **cheese and mushrooms** (queso y champiñones) como expresión del usuario en el panel izquierdo del chat.
6. Resalte **cheese** (queso) y, a continuación, seleccione **+toppings** (+ingredientes).
7. Resalte  **mushrooms** (champiñones) y, a continuación, seleccione **+toppings** (+ingredientes).
8. Seleccione **Score Actions** (Acciones de puntuación).
9. Seleccione **Here are your toppings: $Toppings** (Estos son sus ingredientes: $Ingredientes).
10. Escriba **add pepper** (agregar pimienta) como siguiente expresión del usuario en el panel izquierdo del chat.
11. Resalte **pepper** (pimienta) y, a continuación, seleccione **+toppings** (ingredientes).
12. Seleccione **Score Actions** (Acciones de puntuación).
13. Seleccione **Here are your toppings: $Toppings** (Estos son sus ingredientes: $Ingredientes).
14. Escriba **remove cheese** (quitar el queso) como tercera expresión del usuario en el panel izquierdo del chat.
15. Resalte **cheese** (queso) y, a continuación, seleccione **-toppings** (-ingredientes).
16. Seleccione **Score Actions** (Acciones de puntuación).
17. Seleccione **Here are your toppings: $Toppings** (Estos son sus ingredientes: $Ingredientes).

![](../media/T07_training.png)

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Entidades previamente entrenadas](./08-pre-trained-entities.md)
