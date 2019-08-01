---
title: 'Incorporación de entidades previamente entrenadas a un modelo de Conversation Learner: Microsoft Cognitive Services | Microsoft Docs'
titleSuffix: Azure
description: Obtenga información sobre cómo agregar entidades previamente entrenadas a un modelo de Conversation Learner.
services: cognitive-services
author: nitinme
manager: nolachar
ms.service: cognitive-services
ms.subservice: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: nitinme
ms.openlocfilehash: fb70983c2f9fd20368bb8c6803c9568b27141af7
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "66389270"
---
# <a name="how-to-add-pre-trained-entities"></a>Adición de entidades previamente entrenadas
En este tutorial se muestra cómo agregar entidades previamente entrenadas a un modelo de Conversation Learner.

## <a name="video"></a>Vídeo

[![Vista preliminar del tutorial de entidades previamente entrenadas](https://aka.ms/cl_Tutorial_v3_PreTrainedEntities_Preview)](https://aka.ms/cl_Tutorial_v3_PreTrainedEntities)

## <a name="requirements"></a>Requisitos
Para poder realizar este tutorial, el bot de tutorial general debe estar en ejecución.

    npm run tutorial-general

## <a name="details"></a>Detalles

Las entidades previamente entrenadas reconocen tipos comunes de entidades, tales como números, fechas, importes monetarios y otros.  Funcionan directamente, no requieren ningún entrenamiento y no se puede cambiar su comportamiento, a diferencia de las entidades personalizadas.  De forma predeterminada, las entidades previamente entrenadas tienen varios valores y acumulan todas las instancias identificadas de la entidad.

## <a name="steps"></a>Pasos

Empiece en la página principal de la interfaz de usuario web.

### <a name="create-the-model"></a>Crear el modelo

1. Seleccione **New Model** (Nuevo modelo).
2. Escriba **PretrainedEntities** en **Name** (Nombre).
3. Seleccione **Crear**.

### <a name="entity-creation"></a>Crear la entidad

1. Seleccione **Entities** (Entidades) en el panel izquierdo y, después, **New Entity** (Nueva entidad).
2. Seleccione **Pre-Trained/datetimeV2** (Previamente entrenada/fechahoraV2) como **Entity Type** (Tipo de entidad).
3. Active **Multi-valued** (Multivalor) para permitir que la entidad acumule uno o varios valores. Tenga en cuenta que las entidades previamente entrenadas no puede ser negables.
4. Seleccione **Crear**.

![](../media/T08_entity_create.png)

1. Seleccione **Actions** (Acciones) en el panel izquierdo y, a continuación, **New Action** (Nueva acción).
2. Escriba **La fecha es $builtin-datetimev2** en **Bot's Response...** (Respuesta del bot).
3. Seleccione **Crear**.

![](../media/T08_action_create_1.png)

### <a name="create-the-second-action"></a>Crear la segunda acción

1. Seleccione **Actions** (Acciones) en el panel izquierdo y, a continuación, **New Action** (Nueva acción).
2. Escriba **¿Qué día es?** en **Bot´s Response...**  (Respuesta del bot). Las entidades previamente entrenadas no pueden ser **Required Entities** (Entidades necesarias), ya que todas las expresiones del usuario las reconocen de forma predeterminada.
3. Escriba **builtin-datetimev2** en **Disqualifying Entities** (Entidades descalificadas).
4. Seleccione **Crear**.

![](../media/T08_action_create_2.png)

### <a name="train-the-model"></a>Entrenar el modelo

1. Seleccione **Train Dialogs** (Diálogos de entrenamiento) en el panel izquierdo y, a continuación, **New Train Dialog** (Nuevo diálogo de entrenamiento).
2. Escriba **hola** como expresión del usuario en el panel izquierdo del chat.
3. Seleccione **Score Actions** (Acciones de puntuación).
4. Seleccione **¿Qué día es?** en la lista de acciones.
5. Escriba **today** (hoy) como expresión del usuario en el panel izquierdo del chat.
    - La expresión **today** (hoy) la reconocen automáticamente los modelos previamente entrenados en LUIS.
    - Al mantener el puntero sobre los valores de las entidades previamente entrenadas, se muestran datos adicionales proporcionados por LUIS.

![](../media/T08_training.png)

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Resoluciones de entidad](./09-entity-resolvers.md)
