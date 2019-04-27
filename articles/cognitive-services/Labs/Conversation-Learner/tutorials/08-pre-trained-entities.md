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
ms.openlocfilehash: dade442cb04f79ef75e65d6eb29128b105e72b40
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60707756"
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

1. Seleccione **nuevo modelo**.
2. Escriba **PretrainedEntities** para **nombre**.
3. Seleccione **Crear**.

### <a name="entity-creation"></a>Crear la entidad

1. Seleccione **entidades** en el panel izquierdo, a continuación, **nueva entidad**.
2. Seleccione **Trained Pre/datetimeV2** para **tipo de entidad**.
3. Comprobar **multivalor** para habilitar la entidad se acumulan uno o varios valores. Tenga en cuenta que las entidades Pre-Trained no puede ser negable.
4. Seleccione **Crear**.

![](../media/T08_entity_create.png)

1. Seleccione **acciones** en el panel izquierdo, a continuación, **nueva acción**.
2. Escriba **la fecha es $builtin-datetimev2** para **respuesta del Bot...** .
3. Seleccione **Crear**.

![](../media/T08_action_create_1.png)

### <a name="create-the-second-action"></a>Crear la segunda acción

1. Seleccione **acciones** en el panel izquierdo, a continuación, **nueva acción**.
2. Escriba **¿qué es la fecha?** para **respuesta del Bot...** . No pueden ser entidades previamente entrenadas **necesario entidades** tal como se reconocen de forma predeterminada para todas las grabaciones de voz.
3. Escriba **builtin datetimev2** para **descalificación entidades**.
4. Seleccione **Crear**.

![](../media/T08_action_create_2.png)

### <a name="train-the-model"></a>Entrenar el modelo

1. Seleccione **cuadros de diálogo "Train"** en el panel izquierdo, a continuación, **el cuadro de diálogo de nuevo "Train"**.
2. Escriba **hello** para utterance del usuario en el panel izquierdo del chat.
3. Seleccione **puntuar acciones**.
4. Seleccione **¿qué es la fecha?** en la lista de acciones
5. Escriba **hoy** para utterance del usuario en el panel izquierdo del chat.
    - El **hoy** modelos previamente entrenados en LUIS reconoce automáticamente utterance (dictado).
    - Al mantener el puntero sobre los valores de las entidades previamente entrenadas, se muestran datos adicionales proporcionados por LUIS.

![](../media/T08_training.png)

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Resoluciones de entidad](./09-entity-resolvers.md)
