---
title: 'Uso de la propiedad "Expected Entity" (Entidad esperada) de las acciones de Conversation Learner: Microsoft Cognitive Services | Microsoft Docs'
titleSuffix: Azure
description: Obtenga información sobre cómo usar la propiedad "Expected Entity" (Entidad esperada) de un modelo de Conversation Learner.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: c6696d0f22026ac333c526b505732d15a4b01be7
ms.sourcegitcommit: 295babdcfe86b7a3074fd5b65350c8c11a49f2f1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/27/2018
ms.locfileid: "53796686"
---
# <a name="how-to-use-the-expected-entity-property-of-actions"></a>Uso de la propiedad "Expected Entity" (Entidad esperada) de las acciones

En este tutorial se muestra la propiedad "Expected Entity" (Entidad esperada) de las acciones.

## <a name="video"></a>Vídeo

[![Vista previa del tutorial de entidad esperada](https://aka.ms/cl_Tutorial_v3_ExpectedEntity_Preview)](https://aka.ms/cl_Tutorial_v3_ExpectedEntity)

## <a name="requirements"></a>Requisitos
Para poder realizar este tutorial debe ejecutar el bot del tutorial general

    npm run tutorial-general

## <a name="details"></a>Detalles
Use la propiedad "Expected Entity" (Entidad esperada) de una acción para guardar la respuesta del usuario a esta acción en una entidad.

Al agregar entidades a la propiedad "Expected Entity" (Entidad esperada) de una acción, el sistema hará lo siguiente:

1. Para empezar, intentará emparejar las entidades mediante el modelo de extracción de entidades basado en el aprendizaje automático.
2. Si no se encuentra ninguna entidad, asignará la expresión de usuario completa a $entity de acuerdo con el método heurístico.
3. Llamará a `EntityDetectionCallback` y continuará con la selección de acciones.

## <a name="steps"></a>Pasos

### <a name="create-the-model"></a>Creación del modelo

1. En la interfaz de usuario web, haga clic en "New Model" (Nuevo modelo).
2. En el campo "Name" (Nombre), escriba "ExpectedEntities" y presione Entrar.
3. Haga clic en el botón "Create" (Crear).

### <a name="entity-creation"></a>Creación de la entidad

1. En el panel izquierdo, haga clic en "Entities" (Entidades) y después en el botón "New Entity" (Nueva entidad).
2. Seleccione "Custom Trained" (Entrenada de forma personalizada) en "Entity Type" (Tipo de entidad).
3. Escriba "nombre" en "Entity Name" (Nombre de entidad).
4. Haga clic en el botón "Create" (Crear).

> [!NOTE]
> El tipo de entidad "Custom Trained" (Entrenada de forma personalizada) implica que esta entidad se puede entrenar, a diferencia de otros tipos de entidades.

![](../media/tutorial4_entities.PNG)

### <a name="create-the-first-action"></a>Creación de la primera acción

1. En el panel izquierdo, haga clic en "Actions" (Acciones) y, posteriormente, en el botón "New Action" (Nueva acción).
2. En el campo "Bot's response..." (Respuesta del bot), escriba "¿Cómo se llama?".
3. En el campo "Expected Entities" (Entidades esperadas), escriba "nombre".
4. Haga clic en el botón "Create" (Crear).

> [!NOTE]
> Las entidades que se detecten y extraigan de la respuesta del usuario se guardarán en la entidad "nombre" si se elige esta acción. Si no se detecta ninguna entidad, se guardará toda la respuesta en esta entidad.

### <a name="create-the-second-action"></a>Creación de la segunda acción

1. En el panel izquierdo, haga clic en "Actions" (Acciones) y, posteriormente, en el botón "New Action" (Nueva acción).
2. En el campo "Bot's response..." (Respuesta del bot…), escriba "Hola, $nombre:".
3. Haga clic en el botón "Create" (Crear).

> [!NOTE]
> La entidad "nombre" se ha agregado automáticamente como una de las "Required Entities" (Entidades necesarias) por referencia en la respuesta.

Ahora tiene dos acciones.

![](../media/tutorial4_actions.PNG)

### <a name="train-the-model"></a>Entrenamiento del modelo

1. En el panel izquierdo, haga clic en "Train Dialogs" (Diálogos de entrenamiento) y, después, en el botón "New Train Dialog" (Nuevo diálogo de entrenamiento).
2. En el panel del chat, donde aparece "Type your message..." (Escriba su mensaje…), escriba "hola".
    - Esto simula la parte del usuario de la conversación.
3. Haga clic en el botón "Score Actions" (Acciones de puntuación).
4. Seleccione la respuesta, "¿Cómo se llama?".
    - La respuesta "Hola, $nombre:" no se puede seleccionar ya que esta respuesta requiere que se defina ahora la entidad "nombre" en la memoria del modelo.
5. En el panel del chat, donde aparece "Type your message..." (Escriba su mensaje…), escriba "Frank".
    - Se resalta "Frank" como una entidad de acuerdo con la heurística que hemos configurado anteriormente para guardar la respuesta como la entidad.
6. Haga clic en el botón "Score Actions" (Acciones de puntuación).
    - La entidad "nombre" ahora se define como "Frank" en la memoria del modelo, por lo que la acción "Hola, $nombre:" se puede seleccionar como una acción.
7. Seleccione la respuesta "Hola, $nombre:".
8. Haga clic en el botón "Save" (Guardar).

Al agregar entradas alternativas, se entrena más el modelo.

1. En el campo "Add alternative input..." (Agregar entrada alternativa...), escriba "Me llamo Jose".
    - El modelo no reconoce el nombre como una entidad por lo que selecciona todo el bloque de texto como el valor de la entidad.
2. Haga clic en la frase "Me llamo Jose" y después en el icono de la papelera.
3. Haga clic en "Jose" y después en "nombre" en la lista de entidades.
4. Haga clic en Score Actions (Acciones de puntuación).
5. Seleccione la respuesta "Hola, Frank:".
6. Haga clic en el botón "Save" (Guardar).

![](../media/tutorial4_dialogs.PNG)

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Entidades negables](./06-negatable-entities.md)
