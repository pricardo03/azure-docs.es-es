---
title: 'Cómo usar entidades de varios valores con una aplicación Conversation Learner: Microsoft Cognitive Services | Microsoft Docs'
titleSuffix: Azure
description: Obtenga información sobre cómo utilizar entidades de varios valores con una aplicación Conversation Learner.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 865b50747b2c9574b5f88d4902bea9e4c8e0e032
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/23/2018
ms.locfileid: "35381763"
---
# <a name="how-to-use-multi-value-entities-with-a-conversation-learner-application"></a>Cómo utilizar entidades de varios valores con una aplicación Conversation Learner
Este tutorial muestra la propiedad "Multi-Value" (Varios valores) de las entidades.

##<a name="requirements"></a>Requisitos
Para poder realizar este tutorial debe ejecutar el bot de tutorial general.

    npm run tutorial-general

## <a name="details"></a>Detalles
Una entidad de varios valores acumula valores en una lista, en lugar de almacenar un valor único.  Esto es útil para las entidades en que el usuario puede especificar más de un valor, como los ingredientes de una pizza.

En concreto, si una entidad está marcada como "Multi-Value" (Varios valores), cada instancia reconocida de la entidad se anexará a una lista en la memoria del bot (en lugar de sobrescribir un valor de entidad único).

## <a name="steps"></a>Pasos

### <a name="create-the-application"></a>Creación de la aplicación

1. En la interfaz de usuario web, haga clic en Nueva aplicación.
2. En Nombre, escriba MultiValueEntities. A continuación, haga clic en Crear.

### <a name="create-an-entity"></a>Crear una entidad

1. Haga clic en Entidades y, a continuación, en Nueva entidad.
2. En Entity Name (Nombre de entidad), escriba Toppings (Ingredientes).
3. Active Multi-Value (Varios valores).
    - Las entidades de varios valores acumulan uno o más valores en la entidad.
2. Active Negatable (Negable).  
    - De este modo, el usuario podrá quitar ingredientes de la lista de ingredientes de pizza acumulados.
3. Haga clic en Crear.

![](../media/tutorial6_entities.PNG)

### <a name="create-two-actions"></a>Crear dos acciones

1. Haga clic en Actions (Acciones) y, a continuación, en New Action (Nueva acción).
2. En respuesta, escriba "What toppings do you want" (¿Qué ingredientes quiere?).
3. En Disqualifying Entities (Entidades descalificadas), escriba Toppings (Ingredientes).
3. Click Create

A continuación, cree la segunda acción.

1. Haga clic en Actions (Acciones) y en New Action (Nueva acción) para crear una segunda acción.
3. En Response (Respuesta), escriba "Here are your toppings: $Toppings" (Estos son sus ingredientes: $Toppings).
4. Click Create

Ahora tiene dos acciones.

![](../media/tutorial6_actions.PNG)

### <a name="train-the-bot"></a>Entrenamiento del bot

1. Haga clic en Train Dialogs (Diálogos de entrenamiento) y, a continuación, en New Train Dialog (Nuevo diálogo de entrenamiento).
2. Escriba "hello" (hola).
3. Haga clic en Score Actions (Acciones de puntuación) y seleccione "What toppings do you want?" (¿Qué ingredientes quiere?).
2. Escriba "mushrooms and cheese" (setas y queso). 
    - Puede etiquetar ninguna, una o varias entidades.
3. Haga clic en "mushrooms" (setas) y seleccione Toppings (Ingredientes).
4. Haga clic en "mushrooms" (setas) y seleccione Toppings (Ingredientes).
5. Haga clic en Score Actions (Acciones de puntuación).
    - Tenga en cuenta que los dos valores están ahora presentes en la entidad Toppings (Ingredientes). 
6. Seleccione "Here are your toppings: $Toppings" (Estos son sus ingredientes: $Toppings).

Podemos agregar más:

7. Escriba "add peppers" (agregar pimientos).
    - Haga clic en "peppers" (pimientos) en Entity Detection (Detección de entidades) y seleccione Toppings (Ingredientes).
3. Haga clic en Score Actions (Acciones de puntuación).
    - Observe que "peppers" (pimientos) se muestra ahora como un valor adicional en Toppings (Ingredientes).
6. Seleccione "Here are your toppings: $Toppings" (Estos son sus ingredientes: $Toppings).

Vamos a quitar un ingrediente y a agregar otro:

2. Escriba "remove peppers and add sausage" (quitar pimientos y agregar salchicha).
1. Haga clic en "peppers" (pimientos) y luego en la x roja para quitar el ingrediente.
2. Haga clic en "peppers" (pimientos) y seleccione "-Toppings" (-Ingredientes).
3. Haga clic en Score Actions (Acciones de puntuación).
    - Observe que "peppers" (pimientos) se ha eliminado y "sausage" (salchicha) se ha agregado.
6. Seleccione "Here are your toppings: $Toppings" (Estos son sus ingredientes: $Toppings).

Ahora vamos a intentar quitarlo todo:

6. Escriba "remove mushrooms, remove cheese, and remove sausage " (Quitar setas, quitar quero y quitar salchicha).
7. Haga clic en los tres elementos y seleccione "-Toppings " (-Ingredientes).
7. Haga clic en Score Actions (Acciones de puntuación).
    - Observe que se todos los ingredientes se han borrado.
2. Seleccione "What toppings do you want" (¿Qué ingredientes quiere?).
3. Haga clic en Done Teaching (Aprendizaje completado).

![](../media/tutorial6_dialogs.PNG)

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Entidades integradas](./7-built-in-entities.md)
