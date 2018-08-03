---
title: 'Incorporación de entidades precompiladas a un modelo de Conversation Learner: Microsoft Cognitive Services | Microsoft Docs'
titleSuffix: Azure
description: Obtenga información sobre cómo agregar entidades precompiladas a un modelo de Conversation Learner.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 84d73add5586aaaf130253a8122a4152e39bcbe9
ms.sourcegitcommit: 4e5ac8a7fc5c17af68372f4597573210867d05df
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/20/2018
ms.locfileid: "39171374"
---
# <a name="how-to-add-pre-built-entities"></a>Cómo agregrar entidades pregeneradas
En este tutorial se muestra cómo agregar entidades "precompiladas" a un modelo de Conversation Learner.

## <a name="video"></a>Vídeo

[![Vista previa del tutorial 7](http://aka.ms/cl-tutorial-07-preview)](http://aka.ms/blis-tutorial-07)

## <a name="requirements"></a>Requisitos
Para poder realizar este tutorial debe ejecutar el bot del tutorial general

    npm run tutorial-general

## <a name="details"></a>Detalles

Las entidades pregeneradas reconocen tipos comunes de entidades, tales como números, fechas, importes monetarios y otros.  A diferencia de las entidades personalizadas, estas funcionan de forma "integrada" y no requieren ningún tipo de aprendizaje.  Además, a diferencia de las entidades personalizadas, su comportamiento no se puede cambiar.  De forma predeterminada, las entidades pregeneradas tienen varios valores, es decir, la memoria del bot se acumulará en cada instancia identificada de la entidad.

## <a name="steps"></a>Pasos

### <a name="create-the-model"></a>Crear el modelo

1. En la interfaz de usuario web, haga clic en New Model (Modelo nuevo).
2. En Nombre, escriba BuiltInEntities. A continuación, haga clic en Crear.

### <a name="create-an-entity"></a>Crear una entidad

1. Haga clic en Entidades y, a continuación, en Nueva entidad.
2. Haga clic en la lista desplegable de EntityType y seleccione datetimev2.
    - Se deshabilitarán las opciones programables de tipo negable, ya que no se aplican a las entidades precompiladas.
3. Haga clic en Crear.

![](../media/tutorial7_entities.PNG)

### <a name="create-two-actions"></a>Crear dos acciones

1. Haga clic en Acciones y, a continuación, en Nueva acción.
2. En Respuesta, escriba "la fecha es $luis-datetimev2".
3. Haga clic en Crear.

![](../media/tutorial7_actions.PNG)

A continuación, cree la segunda acción:

1. Haga clic en Acciones y en Nueva acción para crear una segunda acción.
3. En Respuesta, escriba "¿Cuál es la fecha?".
4. En Disqualifying Entities (Entidades descalificadas), escriba "luis datetimev2".
4. Click Create

![](../media/tutorial7_actions2.PNG)

Ahora tiene dos acciones.

### <a name="train-the-bot"></a>Entrenamiento del bot

1. Haga clic en Train Dialogs (Diálogos de entrenamiento) y, a continuación, en New Train Dialog (Nuevo diálogo de entrenamiento).
2. Escriba "hola".
3. Haga clic en Score Actions (Acciones de puntuación) y seleccione "What's the date?" (¿Cuál es la fecha?).
2. Escriba "hoy". 
    - Tenga en cuenta que "hoy" está etiquetado y que se muestra en la segunda línea, ya que es una entidad pregenerada y no es editable.
5. Haga clic en Score Actions (Acciones de puntuación).
    - La fecha ahora aparece en la sección Entity Memory (Memoria de la entidad). 
    - Si pasa el mouse sobre la fecha, verá los datos adicionales que proporcionó LUIS; puede usar estos datos y editarlos en el código. 
6. Seleccione "la fecha es $luis-datetimev2".
7. Haga clic en Done Teaching (Aprendizaje completado)

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Entradas alternativas](./8-alternative-inputs.md)
