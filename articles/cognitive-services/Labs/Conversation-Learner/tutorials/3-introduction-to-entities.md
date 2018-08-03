---
title: 'Uso de entidades con un modelo de Conversation Learner: Microsoft Cognitive Services | Microsoft Docs'
titleSuffix: Azure
description: Obtenga información sobre cómo usar entidades con un modelo de Conversation Learner.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: f851d43d69999a848dea01c9457a379adb63353b
ms.sourcegitcommit: 4e5ac8a7fc5c17af68372f4597573210867d05df
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/20/2018
ms.locfileid: "39172388"
---
# <a name="introduction-to-entities"></a>Introducción a las entidades

En este tutorial se presentan las entidades y se explica cómo usar los campos "Disqualifying entities" (Entidades descalificadas) y "Required entities" (Entidades requeridas) en las acciones.

## <a name="video"></a>Vídeo

[![Vista previa del tutorial 3](http://aka.ms/cl-tutorial-03-preview)](http://aka.ms/blis-tutorial-03)

## <a name="requirements"></a>Requisitos

Para poder realizar este tutorial debe ejecutar el bot del tutorial general

    npm run tutorial-general

## <a name="details"></a>Detalles

En este tutorial se ilustran dos usos comunes de las entidades.  En primer lugar, las entidades pueden extraer subcadenas de un mensaje de usuario, como la identificación de una ciudad en "what's the weather in Seattle?" (¿Qué tiempo hace en Seattle?).  En segundo lugar, las entidades restringir la disponibilidad de las acciones.  En concreto, una acción puede enumerar una entidad como "requerida" o "descalificada".
- Las entidades requeridas de una acción deben estar presentes en la memoria del bot para que la acción esté disponible.
- Las entidades descalificadas *no* deben estar presentes en la memoria del bot para que la acción esté disponible.

En otros tutoriales se tratan otros aspectos de las entidades, como las entidades compiladas previamente, las entidades de varios valores y negables, las entidades de programación y la manipulación de entidades en el código.

## <a name="steps"></a>Pasos

### <a name="create-the-model"></a>Crear el modelo

1. En la interfaz de usuario web, haga clic en New Model (Modelo nuevo).
2. En Name (Nombre), escriba IntroToEntities. A continuación, haga clic en Crear.

### <a name="create-entity"></a>Crear entidad

1. Haga clic en Entidades y, a continuación, en Nueva entidad.
2. En Entity Name (Nombre de entidad), escriba la ciudad.
3. Click Create

> [!NOTE]
> El tipo de entidad es "custom" (personalizada), lo que significa que se puede entrenar la entidad.  Hay entidades compiladas previamente, lo que significa que su comportamiento no se puede ajustar; se tratan en otro tutorial.

### <a name="create-two-actions"></a>Crear dos acciones

1. Haga clic en Actions (Acciones) y, a continuación, en New Action (Nueva acción).
2. En Response (Respuesta), escriba "I don't know what city you want" (No sé qué ciudad desea).
3. En Disqualifying Entities (Entidades descalificadas), escriba $city. Haga clic en Guardar.
    - Esto significa que si esta entidad está definida en la memoria del bot, entonces esta acción *no* estará disponible.
2. Haga clic en Actions (Acciones) y en New Action (Nueva acción) para crear una segunda acción.
3. En Response (Respuesta), escriba "The weather in $city is probably sunny" (Parece que está soleado en $city).
4. En Required Entities (Entidades requeridas), la entidad de ciudad se ha agregado automáticamente, ya que se ha hecho referencia a ella.
5. Haga clic en Guardar

Ahora tiene dos acciones.

![](../media/tutorial3_actions.PNG)

### <a name="train-the-bot"></a>Entrenamiento del bot

1. Haga clic en Train Dialogs (Diálogos de entrenamiento) y, a continuación, en New Train Dialog (Nuevo diálogo de entrenamiento).
2. Escriba "hello" (hola).
3. Haga clic en Score Actions (Acciones de puntuación) y seleccione "I don't know what city you want?" (No sé qué ciudad desea).
    - No se puede seleccionar la respuesta en que la entidad de ciudad es necesaria, porque la entidad de ciudad no está definida en la memoria del bot.
2. Seleccione "I don't know what city you want" (No sé qué ciudad desea).
4. Escriba "seattle". Resalte seattle y luego haga clic en la ciudad.
5. Haga clic en Score Actions (Acciones de puntuación)
    - El valor de ciudad ahora se encuentra en la memoria del bot.
    - "Weather in $city is probably sunny" (Probablemente esté soleado en $city) ahora está disponible como respuesta. 
6. Seleccione "Weather in $city is probably sunny" (Probablemente esté soleado en $city).

Supongamos que el usuario escribe "repeat that" (Repetir eso). 
1. Escriba eso y haga clic en ENTRAR. La entidad de ciudad y su valor están en memoria y disponibles.
2. Seleccione "Weather in $city is probably sunny" (Probablemente esté soleado en $city).

![](../media/tutorial3_entities.PNG)

Ahora ha creado una entidad y ha etiquetado instancias de ella en los mensajes de usuario.  También ha usado la presencia o ausencia de la entidad en la memoria del bot para controlar qué acciones están disponibles, mediante los campos de entidades descalificadas y requeridas de la acción.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Entidad esperada](./4-expected-entity.md)
