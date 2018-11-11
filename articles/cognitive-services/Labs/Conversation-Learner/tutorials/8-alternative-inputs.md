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
ms.openlocfilehash: 3b38cc265b4adfb301dc1165e02b17a8aa7c9589
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/07/2018
ms.locfileid: "51252554"
---
# <a name="how-to-use-alternative-inputs"></a>Uso de entradas alternativas

En este tutorial se explica cómo usar el campo "Entradas alternativas" para la entrada de usuario en la interfaz de aprendizaje.

## <a name="video"></a>Vídeo

[![Vista previa del tutorial 8](https://aka.ms/cl-tutorial-08-preview)](https://aka.ms/blis-tutorial-08)

## <a name="requirements"></a>Requisitos
Para poder realizar este tutorial debe ejecutar el bot del tutorial general

    npm run tutorial-general

## <a name="details"></a>Detalles
Las "entradas alternativas" son expresiones de usuario alternativas que el usuario podría haber dicho en un punto concreto de un diálogo de aprendizaje. Las entradas alternativas permite especificar de forma más sólida las variaciones de lo que un usuario puede decir, sin tener que indicar cada variación en un diálogo de entrenamiento independiente.

## <a name="steps"></a>Pasos

### <a name="create-the-model"></a>Crear el modelo

1. En la interfaz de usuario web, haga clic en New Model (Modelo nuevo).
2. En Nombre, escriba AlternativeInputs. A continuación, haga clic en Crear.

### <a name="create-an-entity"></a>Crear una entidad

1. Haga clic en Entidades y, a continuación, en Nueva entidad.
2. En Entity Name (Nombre de entidad), escriba la ciudad.
3. Haga clic en Crear.

### <a name="create-three-actions"></a>Crear tres acciones

1. Haga clic en Actions (Acciones) y, a continuación, en New Action (Nueva acción).
2. En Response (Respuesta), escriba "Which city do you want?" (¿Qué ciudad desea?).
3. En Disqualifying Entities (Entidades descalificadas), escriba $city.
3. Haga clic en Crear.

A continuación, cree la segunda acción:

1. Haga clic en Actions (Acciones) y, a continuación, en New Action (Nueva acción).
3. En Response (Respuesta), escriba "The weather in $city is probably sunny" (Parece que está soleado en $city).
4. En Required Entities (Entidades requeridas), escriba $city.
4. Haga clic en Crear.

Cree la tercera acción:

1. Haga clic en Actions (Acciones) y, a continuación, en New Action (Nueva acción).
3. En Response (Respuesta), escriba "Try asking for the weather" (Intentar consultar el tiempo).
    - Esto sería para responder a la pregunta del usuario, como "what can the system do?" (¿Qué puede hacer el sistema?).
4. En Disqualifying Entities (Entidades descalificadas), escriba $city.
4. Haga clic en Crear.

Ahora dispone de tres acciones.

### <a name="train-the-bot"></a>Entrenamiento del bot

1. Haga clic en Train Dialogs (Diálogos de entrenamiento) y, a continuación, en New Train Dialog (Nuevo diálogo de entrenamiento).
2. Escriba "what's the weather" (¿Cómo está el tiempo?).
3. Haga clic en Score Actions (Acciones de puntuación) y seleccione "Which city do you want?" (¿Qué ciudad desea?).
2. Escriba "denver".
3. Haga doble clic en "denver" y seleccione la ciudad.
    - De esta forma, se marca como una entidad de ciudad.
5. Haga clic en Score Actions (Acciones de puntuación)
    - "denver" ahora está presente en la entidad de ciudad. 
6. Seleccione "The weather in $city is probably sunny" (Parece que está soleado en $city).
7. Haga clic en Done Teaching (Aprendizaje completado).

Agregue otro diálogo de ejemplo:

1. Haga clic en New Action (Nueva acción) y luego en New Train Dialog (Nuevo diálogo de entrenamiento).
2. Escriba "what can you do?" (¿Qué puede hacer?).
3. Haga clic en Score Actions (Acciones de puntuación) y seleccione "Try asking for the weather" (Intentar consultar el tiempo).
2. Escriba "What's the weather in seattle" (¿Qué tiempo hace en Seattle?).
3. Haga doble clic en "seattle" y seleccione la ciudad.
    - De esta forma, se marca como una entidad de ciudad.
5. Haga clic en Score Actions (Acciones de puntuación)
    - "seattle" ahora está presente en la entidad de ciudad. 
6. Seleccione "The weather in $city is probably sunny" (Parece que está soleado en $city).
7. Haga clic en Done Teaching (Aprendizaje completado).

Veamos qué sucede si el usuario indica algo semánticamente similar a lo anterior:

1. Haga clic en New Action (Nueva acción) y luego en New Train Dialog (Nuevo diálogo de entrenamiento).
2. Escriba "help" (Ayuda).
3. Haga clic en Score Actions (Acciones de puntuación).
    - Las puntuaciones de las dos respuestas posibles son muy similares. Esto nos indica que el modelo se confunde en el límite que separa las dos acciones.
6. Haga clic en Abandon Teaching (Abandonar entrenamiento) y en Confirm (Confirmar).

![](../media/tutorial8_closescores.png)

En este caso, sería útil agregar entradas alternativas a los diálogos. Puede agregarlas a medida que realiza el entrenamiento. También puede volver atrás y agregarlas más adelante.

2. Haga clic en "What can you do?" (¿Qué puede hacer?) en Train Dialogs (Diálogos de entrenamiento).
2. En el cuadro de diálogo, haga clic en "what can you do?" (¿Qué puede hacer?) para seleccionarlo.
    1. En el panel derecho, en Entity Detection (Detección de entidades), en Add alternative input (Agregar entrada alternativa), escriba un par de alternativas:
    1. Escriba "what are my choices?" (¿Cuáles son mis opciones?).
    2. Escriba "Tell me my choices" (Indicarme qué opciones tengo).
    3. Escriba "help" (Ayuda).
    1. Haga clic en Submit Changes (Enviar cambios).


![](../media/tutorial8_helpalternates.png)

2. Ahora haga clic en "what's the weather in seattle" (¿Qué tiempo hace en Seattle?).
    1. En Add alternative input (Agregar entrada alternativa), escriba "forecast for seattle" (Previsión para Seattle).
    2. Haga doble clic en "seattle" y seleccione la ciudad. Las entidades de las entradas alternativas deben estar presentes y tener el mismo conjunto de entidades. No hay problema si el contenido de las entidades es diferente.
    3. En Add alternative input (Agregar entrada alternativa), escriba "will it rain today in denver" (Hoy lloverá en Denver).
    4. Haga clic en "denver" y seleccione la ciudad.
    5. Haga clic en Submit Changes (Enviar cambios) y en Done (Listo).


Se van a agregar entradas alternativas al primer diálogo:

1. Haga clic en Train Dialogs (Diálogos de entrenamiento).
2. Haga clic en el diálogo que empieza con "what's the weather" (¿Qué tiempo hace?).
2. Haga clic para seleccionar "what's the weather" (¿Qué tiempo hace?) en el panel izquierdo:
    1. En Add alternative input (Agregar entrada alternativa), escriba "weather forecast" (Previsión meteorológica).
    2. Escriba "will it rain?" (¿Lloverá?).
    3. Haga clic en Submit Changes (Enviar cambios).
4. Haga clic para seleccionar "denver" en el panel izquierdo:
    1. En Add alternative input (Agregar entrada alternativa), escriba "for denver" (Para Denver).
    2. Escriba "forecast for austin" (Previsión para Austin).
        - Se resalta la frase completa. Haga clic en la frase y luego en la x de color rojo. Luego seleccione Austin y haga clic en la ciudad.
        - Haga clic en Submit Changes (Enviar cambios).
    1. Haga clic en Done (Listo), lo que provocará el reentrenamiento del modelo.

![](../media/tutorial8_altcities.png)

Probemos las variaciones:

1. Haga clic en New Train Dialog (Haga clic en diálogo de entrenamiento).
2. Escriba "what are you capabilities" (¿Qué habilidades tiene?).
3. Haga clic en Score Actions (Acciones de puntuación).
    - Las puntuaciones ahora son más decisivas en la siguiente acción, lo que indica la exactitud del modelo.
2. Seleccione "Try asking for weather" (Tratar de consultar el tiempo).
6. Haga clic en Done Teaching (Aprendizaje completado).

Ahora ya visto cómo se pueden usar las entradas alternativas para indicar otras cosas que el usuario puede haber dicho. Esto ayuda a evitar que se creen muchos diálogos, que a veces son los mismos, al contraerlos en un único diálogo y al enumerar lo que el usuario puede decir.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Registrar diálogos](./9-log-dialogs.md)
