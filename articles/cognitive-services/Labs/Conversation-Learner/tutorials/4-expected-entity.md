---
title: 'Uso de la propiedad "Entidad esperada" de las acciones de Conversation Learner: Microsoft Cognitive Services | Microsoft Docs'
titleSuffix: Azure
description: Obtenga información sobre cómo usar la propiedad "Entidad esperada" de un modelo de Conversation Learner.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: d0ad7093249bea761e0a36c6fffab8cdd151a662
ms.sourcegitcommit: f58fc4748053a50c34a56314cf99ec56f33fd616
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/04/2018
ms.locfileid: "48268028"
---
# <a name="how-to-use-the-expected-entity-property-of-actions"></a>Uso de la propiedad "Entidad esperada" de las acciones

En este tutorial se muestra el campo "Entidad esperada" de las acciones.

## <a name="video"></a>Vídeo

[![Vista previa del tutorial 4](http://aka.ms/cl-tutorial-04-preview)](http://aka.ms/blis-tutorial-04)

## <a name="requirements"></a>Requisitos
Para poder realizar este tutorial debe ejecutar el bot del tutorial general

    npm run tutorial-general

## <a name="details"></a>Detalles
Use el campo "Entidad esperada" de una acción para comunicarse con el sistema en el que espera que la respuesta del usuario a una acción adopte el estado de una entidad.

Concretamente, si el campo "entidad esperada" de una acción está establecido en $entity, entonces en la siguiente expresión de usuario, el sistema hará lo siguiente:

1. En primer lugar, como es habitual, intenta encontrar las entidades que usan el modelo de extracción de entidades basado en el aprendizaje automático.
2. Si no se encuentra ninguna entidad en el paso 1, como método heurístico, asigne la expresión de usuario completa a $entity.
3. Llame a `EntityDetectionCallback` de la forma habitual y proceda con la selección de acciones.

## <a name="steps"></a>Pasos

### <a name="create-the-model"></a>Crear el modelo

1. En la interfaz de usuario web, haga clic en New Model (Modelo nuevo).
2. En Nombre, escriba ExpectedEntities. A continuación, haga clic en Crear.

### <a name="create-an-entity"></a>Crear una entidad

1. Haga clic en Entidades y, a continuación, en Nueva entidad.
2. En Entity Name (Nombre de entidad), escriba el nombre.
3. Haga clic en Crear.

> [!NOTE]
> El tipo de entidad es "custom". Este valor significa que la entidad se puede entrenar.  También hay entidades precompiladas, lo que significa que no se puede ajustar su comportamiento.  Estas entidades se tratan en el [tutorial sobre las entidades precompiladas](./7-built-in-entities.md).

![](../media/tutorial4_entities.PNG)

### <a name="create-two-actions"></a>Crear dos acciones

1. Haga clic en Actions (Acciones) y, después, en New Action (Nueva acción).
2. En Respuesta, escriba "What's your name?" (¿Cómo se llama?).
3. En Expected Entities (Entidades esperadas), escriba $name. Haga clic en Guardar.
    - Este valor significa que si se plantea esta pregunta y la respuesta del usuario no tienen ninguna entidad detectada, el bot asumiría la respuesta completa del usuario en esta entidad.
    - La entidad se agrega automáticamente como una entidad descalificadora. 
2. Haga clic en Actions (Acciones) y en New Action (Nueva acción) para crear una segunda acción.
3. En Response (Respuesta), escriba "Hello $name" (Hola, $name).
    - La entidad se agrega automáticamente como una entidad obligatoria.
4. Haga clic en Guardar.

Ahora tiene dos acciones.

![](../media/tutorial4_actions.PNG)

### <a name="train-the-bot"></a>Entrenamiento del bot

1. Haga clic en Train Dialogs (Diálogos de entrenamiento) y, a continuación, en New Train Dialog (Nuevo diálogo de entrenamiento).
2. Escriba "hello" (hola).
3. Haga clic en Score Actions (Acciones de puntuación) y seleccione "What's your name?" (¿Cómo se llama?).
    - No se puede seleccionar la respuesta "Hello $name" (Hola, $name), ya que requiere que se defina la entidad $name y $name no está en la memoria del bot.
2. Escriba "david". 
    - El nombre se resalta como una entidad. Esto se debe a la heurística configurada anteriormente para seleccionar la respuesta como la entidad.
5. Haga clic en Score Actions (Acciones de puntuación)
    - El valor de nombre ahora se encuentra en la memoria del bot.
    - "Hello $name" (Hola, $name) ahora está disponible como una respuesta. 
6. Seleccione "Hello $name" (Hola, $name).
7. Haga clic en Done Teaching (Aprendizaje completado).

Estos son dos ejemplos en los que el modelo de extracción de entidades de aprendizaje automático identifica un nombre, por lo que no se desencadena la heurística de "Entidad esperada".

1. Haga clic en New Train Dialog (Haga clic en diálogo de entrenamiento).
2. Escriba "my name is david" (Me llamo David).
    - El modelo identifica a David como la entidad de nombre, porque ha visto esta palabra antes.
2. Haga clic en Score Actions (Acciones de puntuación)
3. Seleccione "Hello $name" (Hola, $name).
4. Escriba "my name is susan" (Me llamo Susan).
    - El modelo identifica a Susan como el nombre, porque ya ha visto este patrón.
2. Haga clic en Score Actions (Acciones de puntuación).
2. Seleccione "Hello susan" (Hola, Susan).
3. Haga clic en Done Teaching (Aprendizaje completado).

En los ejemplos siguientes, se desencadena la heurística de la "entidad esperada", pero es incorrecta. Los ejemplos muestran entonces cómo hacer una corrección.

1. Escriba "call me jose" (Llámeme Jose).
    - El modelo no reconoce el nombre como una entidad.
2. Haga clic en jose y seleccione el nombre.
3. Haga clic en Score Actions (Acciones de puntuación).
4. Seleccione "Hello $name" (Hola, $name).
5. Haga clic en Done Teaching (Aprendizaje completado).
1. Haga clic en New Train Dialog (Haga clic en diálogo de entrenamiento).
2. Escriba "hello" (Hola).
3. En la respuesta a "what's your name" (¿Cómo se llama?), escriba "I am called frank" (Me llaman Frank).
    - Se resalta la frase completa. Esto se debe a que el modelo estadístico no encontró un nombre, por lo que la heurística desencadenó y seleccionó la respuesta completa como la entidad de nombre.
2. Para corregirlo, haga clic en la frase resaltada y luego haga clic en el icono de la papelera de color rojo. 
3. Haga clic para seleccionar frank y, a continuación, haga clic en el nombre.
2. Haga clic en Score Actions (Acciones de puntuación).
3. Seleccione "Hello $name" (Hola, $name).
4. Haga clic en Done Teaching (Aprendizaje completado).

![](../media/tutorial4_dialogs.PNG)

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Entidades negables](./5-negatable-entities.md)
