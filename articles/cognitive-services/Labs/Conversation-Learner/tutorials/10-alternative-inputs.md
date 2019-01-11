---
title: 'Uso de entradas alternativas con Conversation Learner: Microsoft Cognitive Services | Microsoft Docs'
titleSuffix: Azure
description: Aprenda a usar entradas alternativas con Conversation Learner.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: b6ea1ee4eb8b55d2da4069ef19a268ec68f49cb4
ms.sourcegitcommit: 295babdcfe86b7a3074fd5b65350c8c11a49f2f1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/27/2018
ms.locfileid: "53796674"
---
# <a name="how-to-use-alternative-inputs"></a>Uso de entradas alternativas

En este tutorial, se explica cómo usar el campo “Entradas alternativas” para las expresiones de usuario en la interfaz de aprendizaje.

## <a name="video"></a>Vídeo

[![Vista previa del tutorial de entradas alternativas](https://aka.ms/cl_Tutorial_v3_AlternativeInputs_Preview)](https://aka.ms/cl_Tutorial_v3_AlternativeInputs)

## <a name="requirements"></a>Requisitos
Para poder realizar este tutorial debe ejecutar el bot del tutorial general

    npm run tutorial-general

## <a name="details"></a>Detalles
Las entradas alternativas son expresiones de usuario alternativas y equivalentes semánticamente que el usuario podría haber dicho en un momento específico en un diálogo de aprendizaje. Estas entradas alternativas le permiten especificar de forma más compacta variaciones de expresiones sin tener que especificar cada variación en cuadros de diálogo de aprendizaje separados.

## <a name="steps"></a>Pasos

### <a name="create-the-model"></a>Crear el modelo

1. En la interfaz de usuario web, haga clic en “New Model” (Modelo nuevo).
2. En el campo “Name” (Nombre), escriba “AlternativeInputs” y presione Entrar.
3. Haga clic en el botón “Crear”.

### <a name="entity-creation"></a>Crear la entidad

1. En el panel izquierdo, haga clic en “Entities” (Entidades) y, después, en el botón “New Entity” (Nueva entidad).
2. Seleccione “Custom Trained” (Entrenamiento personalizado) en “Entity Type” (Tipo de entidad).
3. Escriba “city” (ciudad) en “Entity Name” (Nombre de entidad).
4. Haga clic en el botón “Crear”.

Ahora, vamos a crear tres acciones.

### <a name="create-the-first-action"></a>Crear la primera acción

1. En el panel izquierdo, haga clic en “Actions” (Acciones) y, después, en el botón “New Action” (Nueva acción).
2. En el campo “Bot's response…” (Respuesta del bot…), escriba “Which city?” (¿Qué ciudad?).
3. En el campo “Expected Entity in User reply…” (Entidad esperada en la respuesta del usuario…), escriba “city” (ciudad).
4. En el campo “Disqualifying Entitles” (Entidades descalificadoras), escriba “city” (ciudad).
5. Haga clic en el botón “Crear”.

### <a name="create-the-second-action"></a>Crear la segunda acción

1. En el panel izquierdo, haga clic en “Actions” (Acciones) y, después, en el botón “New Action” (Nueva acción).
2. En el campo “Bot's response…” (Respuesta del bot…), escriba “The weather in $city is probably sunny” (Parece que está soleado en $city).
3. Haga clic en el botón “Crear”.

### <a name="create-the-third-action"></a>Crear la tercera acción

1. En el panel izquierdo, haga clic en “Actions” (Acciones) y, después, en el botón “New Action” (Nueva acción).
2. En el campo “Bot's response…” (Respuesta del bot…), escriba “Try asking for the weather” (Intente preguntar sobre el tiempo).
3. En el campo “Disqualifying Entitles” (Entidades descalificadoras), escriba “city” (ciudad).
4. Haga clic en el botón “Crear”.

Ahora dispone de tres acciones.

### <a name="train-the-model"></a>Entrenar el modelo

1. En el panel izquierdo, haga clic en “Train Dialogs” (Diálogos de entrenamiento) y, después, en el botón “New Train Dialog” (Nuevo diálogo de entrenamiento).
2. En el panel del chat, donde dice “Type your message…” (Escriba su mensaje…), escriba “What's the weather?” (¿Qué tiempo hace?).
3. Haga clic en el botón “Score Actions” (Puntuar acciones).
4. Seleccione la respuesta “Which city?” (¿Qué ciudad?).
5. En el panel del chat, donde dice “Type your message…” (Escriba su mensaje…), escriba “Denver”.
6. Haga clic en el botón “Score Actions” (Puntuar acciones).
7. Seleccione la respuesta, “The weather in Denver is probably sunny” (Es probable que esté soleado en Denver).
8. Haga clic en el botón “Save” (Guardar).

Vamos a entrenar un poco más el modelo mediante la creación de otro de diálogo de entrenamiento.

### <a name="second-model-train-dialog"></a>Segundo diálogo de entrenamiento del modelo

1. En el panel izquierdo, haga clic en “Train Dialogs” (Diálogos de entrenamiento) y, después, en el botón “New Train Dialog” (Nuevo diálogo de entrenamiento).
2. En el panel del chat, donde dice “Type your message…” (Escriba su mensaje…), escriba “What can you do?” (¿En qué puedo ayudarte?).
3. Haga clic en el botón “Score Actions” (Puntuar acciones).
4. Seleccione la respuesta “Try asking for weather” (Intente preguntar sobre el tiempo).
5. En el panel del chat, donde dice “Type your message…” (Escriba su mensaje…), escriba “What's the weather in Seattle?” (¿Qué tiempo hace en Seattle?).
6. Haga clic en “Seattle” y, después, seleccione “city” (ciudad) en la lista de entidades.
7. Haga clic en el botón “Score Actions” (Puntuar acciones).
8. Seleccione la respuesta, “The weather in Seattle is probably sunny” (Es probable que el tiempo en Seattle sea soleado).
9. Haga clic en el botón “Save” (Guardar).

### <a name="third-model-train-dialog-using-alternative-input"></a>Tercer diálogo de entrenamiento del modelo con entrada alternativa

1. En el panel izquierdo, haga clic en “Train Dialogs” (Diálogos de entrenamiento) y, después, en el botón “New Train Dialog” (Nuevo diálogo de entrenamiento).
2. En el panel del chat, donde dice “Type your message…” (Escriba su mensaje…), escriba “help” (ayuda).
3. Haga clic en el botón “Score Actions” (Puntuar acciones).
    - El modelo no está seguro de cuál es la mejor opción, por lo que elegiría de forma predeterminada el percentil más alto.
4. Haga clic en el botón “Abandon Teaching” (Salir del aprendizaje) y, después, seleccione el botón “Confirm” (Confirmar).

![](../media/tutorial8_closescores.png)

Ahora, vamos a optimizar el sistema con entradas alternativas. Puede agregar una entrada alternativa durante el proceso de aprendizaje o más tarde.

5. En el panel izquierdo, haga clic en “Train Dialogs” (Diálogos de entrenamiento) y, después, seleccione “What can you do?” (¿Qué puede hacer?) en la lista de diálogos de entrenamiento.
6. Haga clic en la expresión “What can you do?” (¿Qué puede hacer?) en el panel del chat.
7. En el campo “Add alternative input…” (Agregar entrada alternativa…), escriba “help” (ayuda) y presione Entrar.
8. Haga clic en el botón “Save Changes” (Guardar cambios).

![](../media/tutorial8_helpalternates.png)

Vamos a agregar otra entrada alternativa para controlar “Houston”.

9. Haga clic en la expresión “What's the weather in Seattle?” (¿Qué tiempo hace en Seattle?) en el panel del chat.
10. En el campo “Add alternative input…” (Agregar entrada alternativa…), escriba “forecast for Houston” (previsión para Houston) y presione Entrar.
    - El mensaje de error resalta el hecho de que las entradas alternativas tienen que ser equivalentes semánticamente y contener las mismas entidades que la expresión original (no solo los mismos valores de entidades). Se necesita la presencia de las mismas entidades.
11. Haga clic en “Houston” y seleccione “city” (ciudad) en la lista de entidades.
12. En el campo “Add alternative input…” (Agregar entrada alternativa…), escriba “forecast for Seattle” (previsión para Seattle) y presione Entrar.
13. Haga clic en “Seattle” y seleccione “city” (ciudad) en la lista de entidades.
14. Haga clic en el botón “Save Changes” (Guardar cambios).
15. Haga clic en el botón “Save Edit” (Guardar edición).

### <a name="testing-the-model"></a>Probar el modelo

1. En el panel izquierdo, haga clic en “Log Dialogs” (Diálogos de registro) y, después, en el botón “New Log Dialog” (Nuevo diálogo de registro).
2. En el panel del chat, donde dice “Type your message…” (Escriba su mensaje…), escriba “help me” (necesito ayuda).
3. En el panel del chat, donde dice “Type your message…” (Escriba su mensaje…), escriba “forecast for Denver” (previsión para Denver).

![](../media/tutorial8_altcities.png)

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Registrar diálogos](./11-log-dialogs.md)
