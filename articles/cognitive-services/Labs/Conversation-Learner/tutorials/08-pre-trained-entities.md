---
title: 'Incorporación de entidades previamente entrenadas a un modelo de Conversation Learner: Microsoft Cognitive Services | Microsoft Docs'
titleSuffix: Azure
description: Obtenga información sobre cómo agregar entidades previamente entrenadas a un modelo de Conversation Learner.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.subservice: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: afa927009e684fa7f8c6217c91dcb589b331b5f5
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2019
ms.locfileid: "55224180"
---
# <a name="how-to-add-pre-trained-entities"></a>Adición de entidades previamente entrenadas
En este tutorial se muestra cómo agregar entidades previamente entrenadas a un modelo de Conversation Learner.

## <a name="video"></a>Vídeo

[![Vista preliminar del tutorial de entidades previamente entrenadas](https://aka.ms/cl_Tutorial_v3_PreTrainedEntities_Preview)](https://aka.ms/cl_Tutorial_v3_PreTrainedEntities)

## <a name="requirements"></a>Requisitos
Para poder realizar este tutorial debe ejecutar el bot del tutorial general

    npm run tutorial-general

## <a name="details"></a>Detalles

Las entidades previamente entrenadas reconocen tipos comunes de entidades, tales como números, fechas, importes monetarios y otros.  Funcionan directamente, no requieren ningún entrenamiento y no se puede cambiar su comportamiento, a diferencia de las entidades personalizadas.  De forma predeterminada, las entidades previamente entrenadas tienen varios valores y acumulan todas las instancias identificadas de la entidad.

## <a name="steps"></a>Pasos

### <a name="create-the-model"></a>Crear el modelo

1. En la interfaz de usuario web, haga clic en "New Model" (Modelo nuevo).
2. En el campo "Name" (Nombre), escriba "EntidadesPreviamenteEntrenadas" y pulse Entrar.
3. Haga clic en el botón "Create" (Crear).

### <a name="entity-creation"></a>Crear la entidad

1. En el panel izquierdo, haga clic en "Entities" (Entidades) y, a continuación, en el botón "New Entity" (Nueva entidad).
2. Seleccione "Pre-Trained/datetimeV2" (Previamente entrenada/fechahoraV2) para "Entity Type" (Tipo de entidad).
3. Active la casilla "Multi-valued" (Varios valores).
    - Las entidades de varios valores acumulan uno o más valores en la entidad.
    - Las propiedades negables están deshabilitadas para las entidades previamente entrenadas.
4. Haga clic en el botón "Create" (Crear).

![](../media/tutorial7_entities_a.PNG)

### <a name="create-the-first-action"></a>Creación de la primera acción

1. En el panel izquierdo, haga clic en "Actions" (Acciones) y, a continuación, en el botón "New Action" (Nueva acción).
2. En el campo "Bot's response..." (Respuesta del bot), escriba "La fecha es $builtin-datetimev2"
3. Haga clic en el botón "Create" (Crear).

![](../media/tutorial7_actions_a.PNG)

### <a name="create-the-second-action"></a>Crear la segunda acción

1. En el panel izquierdo, haga clic en "Actions" (Acciones) y, a continuación, en el botón "New Action" (Nueva acción).
2. En el campo "Bot's response..." (Respuesta del bot), escriba "¿Qué día es hoy?".
    - Las entidades previamente entrenadas no pueden ser entidades necesarias, ya que se reconocen de forma predeterminada para todas las expresiones del usuario.
3. En el campo "Disqualifying Entities" (Entidades descalificadoras), escriba "builtin-datetimev2".
4. Haga clic en el botón "Create" (Crear).

![](../media/tutorial7_actions2_a.PNG)

### <a name="train-the-model"></a>Entrenamiento del modelo

1. En el panel izquierdo, haga clic en "Train Dialogs" (Diálogos de entrenamiento) y, a continuación, en el botón "New Train Dialog" (Nuevo diálogo de entrenamiento).
2. En el panel del chat, donde se indica "Type your message..." (Escriba su mensaje…), escriba "hola".
3. Haga clic en el botón "Score Actions" (Puntuar acciones).
4. Seleccione la respuesta, "¿Qué día es hoy?"
5. En el panel del chat, donde se indica "Type your message..." (Escriba su mensaje…), escriba "today" (hoy).
    - La expresión "today" la reconocen automáticamente los modelos previamente entrenados en LUIS.
    - Al mantener el puntero sobre los valores de las entidades previamente entrenadas, se muestran datos adicionales proporcionados por LUIS.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Resoluciones de entidad](./09-entity-resolvers.md)
