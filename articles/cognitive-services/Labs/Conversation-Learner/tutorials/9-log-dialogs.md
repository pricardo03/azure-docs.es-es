---
title: 'Uso de diálogos de registro con un modelo de Conversation Learner: Microsoft Cognitive Services | Microsoft Docs'
titleSuffix: Azure
description: Obtenga información acerca de cómo usar diálogos de registro con un modelo de Conversation Learner.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 4663fc377e795e603bd2484ec4cf98578408501f
ms.sourcegitcommit: 609c85e433150e7c27abd3b373d56ee9cf95179a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/03/2018
ms.locfileid: "48248388"
---
# <a name="how-to-log-dialogs-in-a-conversation-learner-model"></a>Uso de diálogos de registro con un modelo de Conversation Learner

En este tutorial se explica cómo realizar pruebas de usuario final en la interfaz de Conversation Learner, cómo se registran los diálogos y cómo realizar correcciones en los diálogos registrados para mejorar el modelo.

## <a name="video"></a>Vídeo

[![Vista previa del tutorial 9](http://aka.ms/cl-tutorial-09-preview)](http://aka.ms/blis-tutorial-09)

## <a name="requirements"></a>Requisitos
Para poder realizar este tutorial debe ejecutar el bot del tutorial general

    npm run tutorial-general

## <a name="details"></a>Detalles
Puede usar los diálogos de registro para revisar los diálogos mantenidos con los usuarios finales y realizar correcciones.  En concreto, puede corregir etiquetas de entidad y selecciones de acciones para mejorar el rendimiento del modelo entrenado y del sistema en general. 

## <a name="steps"></a>Pasos

### <a name="create-the-model"></a>Crear el modelo

1. En la interfaz de usuario web, haga clic en New Model (Modelo nuevo).
2. En Name (Nombre), escriba LogDialogs. A continuación, haga clic en Crear.

### <a name="create-an-entity"></a>Crear una entidad

1. Haga clic en Entidades y, a continuación, en Nueva entidad.
2. En Entity Name (Nombre de entidad), escriba la ciudad.
3. Haga clic en Crear.

### <a name="create-two-actions"></a>Crear dos acciones

1. Haga clic en Actions (Acciones) y, a continuación, en New Action (Nueva acción).
2. En Response (Respuesta), escriba "Which city?" (¿Qué ciudad?).
3. En Disqualifying Entities (Entidades descalificadas), escriba $city.
3. Haga clic en Crear.

A continuación, cree la segunda acción:

1. Haga clic en Actions (Acciones) y, a continuación, en New Action (Nueva acción).
3. En Response (Respuesta), escriba "The weather in $city is probably sunny" (Parece que está soleado en $city).
4. En Required Entities (Entidades requeridas), escriba $city.
4. Haga clic en Crear.

Ahora tiene dos acciones.

### <a name="train-the-bot"></a>Entrenamiento del bot

1. Haga clic en Train Dialogs (Diálogos de entrenamiento) y, a continuación, en New Train Dialog (Nuevo diálogo de entrenamiento).
2. Escriba "what's the weather" (¿Cómo está el tiempo?).
3. Haga clic en Score Actions (Acciones de puntuación) y seleccione "Which city?" (¿Qué ciudad?).
2. Escriba "seattle".
3. Haga doble clic en "seattle" y seleccione la ciudad.
    - De esta forma, se marca como una entidad de ciudad.
5. Haga clic en Score Actions (Acciones de puntuación).
6. Seleccione "The weather in $city is probably sunny" (Parece que está soleado en $city).
7. Haga clic en Done Teaching (Aprendizaje completado).

Agregue otro diálogo de ejemplo:

1. Haga clic en Train Dialogs (Cuadros de diálogo de entrenamiento) y, después, en New Train Dialog (Nuevo cuadro de diálogo de entrenamiento).
2. Escriba "What's the weather in seattle" (¿Qué tiempo hace en Seattle?). Tenga en cuenta que Seattle se etiqueta como una entidad.
5. Haga clic en Score Actions (Acciones de puntuación). 
6. Seleccione "The weather in $city is probably sunny" (Parece que está soleado en $city).
7. Haga clic en Done Teaching (Aprendizaje completado).

### <a name="try-the-bot-as-the-user"></a>Pruebe el bot como el usuario
Imaginemos que hemos implementado este bot a los usuarios.

1. Haga clic en Log Dialogs (Diálogos de registro).
2. Haga clic en Nuevo cuadro de diálogo de registro.
    - Esto presenta el bot como el usuario lo experimentaría en el control de chat web en la parte izquierda de la interfaz del usuario. Puede ignorar el área del espacio en blanco a la derecha.
3. Escriba "hello" (hola).
4. Respuesta de bot: "which city?" (¿Qué ciudad?).
4. Escriba "Boston".
5. Respuesta de bot: "which city?" (¿Qué ciudad?).
    - Esto no parece correcto. Vamos a guardar este diálogo.
2. Haga clic en Done Testing (Prueba completada).

Vamos a iniciar una sesión nueva:

2. Haga clic en Nuevo cuadro de diálogo de registro.
3. Escriba "forecast for Boston" (Previsión de Boston).
4. Respuesta de bot: "which city?" (¿Qué ciudad?).
2. Haga clic en Done Testing (Prueba completada).

Ahora vamos a hacer correcciones en el segundo diálogo:

1. Haga clic en "forecast for Boston" (Previsión de Boston) en Log Dialogs (Diálogos de registro).
    - Se abrirá la conversación.
    - Si hace clic en el lado de usuario de la conversación, aquí en "forecast for Boston" (Previsión de Boston), puede cambiar las etiquetas de entidad.
    - Si hace clic en el lado del sistema, aquí en "which city" (¿Qué ciudad?), puede cambiar la acción que está seleccionada.
5. Haga clic en "forecast for Boston" (Previsión de Boston). 
    - La causa principal aquí es que Boston no se ha etiquetado como una entidad. Es necesario cambiar eso.
    - Haga doble clic en "Boston" y luego seleccione la ciudad.
    - Haga clic en Submit Changes (Enviar cambios) y en Save (Guardar). De esta forma, se creará un diálogo de entrenamiento basado en los cambios realizados y se le colocará en los diálogos de entrenamiento en el punto del cambio realizado.
6. Seleccione "The weather in $city is probably sunny" (Parece que está soleado en $city).
7. Haga clic en Done Teaching (Aprendizaje completado). Si va ahora a Train Dialogs (Diálogos de entrenamiento), verá que se ha agregado la nueva acción.

![](../media/tutorial9_logdiag1.PNG)

Ahora vamos a hacer correcciones en el otro diálogo:

1. Haga clic en "hello" (Hola) en Log Dialogs (Diálogos de registro).
    - Se abrirá la conversación.
3. La respuesta a "hello" (Hola) es "which city" (¿Qué ciudad?). Pero queremos cambiar a algo que resulte más conveniente. Una respuesta mejor sería algo como "hello, I'm the weather bot" (Hola, soy el bot del tiempo). Pero no hay ninguna acción que haga eso, por lo que tenemos que crear una.
4. Haga clic en Action (Acción).
    - En Response (Respuesta), escriba "I'm the weather bot. I can help with forecasts" (Soy el bot del tiempo. Puedo ayudarle con las previsiones).
6. Desactive la casilla Wait for Response (Esperar respuesta) para convertirla en una acción de no espera.
7. Haga clic en Crear.
8. Luego haga clic para seleccionar esta nueva acción. A continuación, haga clic en Guardar.
    - Esto permite volver a ese punto en la sesión de entrenamiento.
6. Haga clic para seleccionar "which city?" (¿Qué ciudad?).
7. Escriba "Boston". Haga doble clic para etiquetar Boston como una entidad, en caso de que aún no esté etiquetado.
8. Haga clic en Score Actions (Acciones de puntuación).
9. Haga clic para seleccionar "The weather in $city is probably sunny" (Parece que está soleado en $city).
10. Haga clic en Done Teaching (Aprendizaje completado).

![](../media/tutorial9_addnewaction.PNG)

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Devolución de llamada de detección de entidades](./10-entity-detection-callback.md)
