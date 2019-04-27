---
title: 'Uso de entidades con un modelo de Conversation Learner: Microsoft Cognitive Services | Microsoft Docs'
titleSuffix: Azure
description: Obtenga información sobre cómo usar entidades con un modelo de Conversation Learner.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.subservice: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: a42a2ec36d9ce4fb9c139dfddcde0fe0c188c888
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60707808"
---
# <a name="introduction-to-entities"></a>Introducción a las entidades

En este tutorial se presentan las entidades, las entidades descalificadoras, las entidades necesarias y su uso en Conversation Learner.

## <a name="video"></a>Vídeo

[![Vista previa del tutorial de presentación de las entidades](https://aka.ms/cl_Tutorial_v3_IntroEntities_Preview)](https://aka.ms/cl_Tutorial_v3_IntroEntities)

## <a name="requirements"></a>Requisitos

Para poder realizar este tutorial, el bot de tutorial general debe estar en ejecución.

    npm run tutorial-general

## <a name="details"></a>Detalles

Las entidades capturan la información que el bot necesita para realizar su tarea, ya sea a través de la extracción de expresiones del usuario o la asignación mediante código personalizado. Las propias entidades también pueden restringir la disponibilidad de las acciones al clasificarse explícitamente como "Required" (Necesarias) o "Disqualifying" (Descalificadoras).

- Las entidades requeridas deben estar presentes en la memoria del modelo para que la acción esté disponible.
- Las entidades descalificadoras *no* deben estar presentes en la memoria del modelo para que la acción esté disponible.

Este tutorial se centra en las entidades personalizadas. Las entidades entrenadas previamente, de varios valores, negables y de programación se presentan en otros tutoriales.

## <a name="steps"></a>Pasos

### <a name="create-the-model"></a>Crear el modelo

1. En la interfaz de usuario web, haga clic en "New Model" (Nuevo modelo).
2. En el campo "Name" (Nombre), escriba "IntroToEntities" y presione Entrar.
3. Haga clic en el botón "Create" (Crear).

### <a name="entity-creation"></a>Crear la entidad

1. En el panel izquierdo, haga clic en “Entities” (Entidades) y, después, en el botón “New Entity” (Nueva entidad).
2. Seleccione “Custom Trained” (Entrenamiento personalizado) en “Entity Type” (Tipo de entidad).
3. Escriba “city” (ciudad) en “Entity Name” (Nombre de entidad).
4. Haga clic en el botón "Create" (Crear).

> [!NOTE]
> El tipo de entidad "Custom Trained" (Entrenada de forma personalizada) implica que esta entidad se puede entrenar, a diferencia de otros tipos de entidades.

### <a name="create-the-actions"></a>Creación de acciones

1. En el panel izquierdo, haga clic en "Actions" (Acciones) y, posteriormente, en el botón "New Action" (Nueva acción).
2. En el campo "Bot's response..." (Respuesta del bot), escriba "No sé qué ciudad quiere".
3. En el campo "Disqualifying Entities" (Entidades descalificadoras), escriba "ciudad".
4. Haga clic en el botón "Create" (Crear).

> [!NOTE]
> Si agrega la entidad "ciudad" a "Disqualifying Entities" (Entidades descalificadoras), se descalificaría esta acción y el bot no la consideraría cuando se defina la entidad "ciudad" en la memoria del bot.

Ahora cree una segunda acción.

1. En el panel izquierdo, haga clic en "Actions" (Acciones) y, posteriormente, en el botón "New Action" (Nueva acción).
2. En el campo "Bot's response..." (Respuesta del bot...), escriba "Parece que está soleado en $ciudad".
3. Haga clic en el botón "Create" (Crear).

> [!NOTE]
> La entidad "ciudad" se ha agregado automáticamente a la lista Required Entities (Entidades necesarias) al hacerle referencia en la respuesta.

![](../media/tutorial3_actions.PNG)

### <a name="train-the-model"></a>Entrenamiento del modelo

1. En el panel izquierdo, haga clic en "Train Dialogs" (Diálogos de entrenamiento) y, a continuación, en el botón "New Train Dialog" (Nuevo diálogo de entrenamiento).
2. En el panel del chat, donde aparece "Type your message..." (Escriba su mensaje…), escriba "hola".
    - Esto simula la parte del usuario de la conversación.
3. Haga clic en el botón "Score Actions" (Acciones de puntuación).
4. Seleccione la respuesta "No sé qué ciudad quiere".
5. Como usuario, responda "Seattle".
6. Haga clic en el botón "Score Actions" (Acciones de puntuación).
7. Seleccione la respuesta "Parece que está soleado en $ciudad".
8. Haga clic en el botón "Save" (Guardar).

![](../media/tutorial3_entities.PNG)

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Entidad esperada](./05-expected-entity.md)
