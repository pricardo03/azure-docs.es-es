---
title: 'Uso de entidades de varios valores con un modelo de Conversation Learner: Microsoft Cognitive Services | Microsoft Docs'
titleSuffix: Azure
description: Obtenga información sobre cómo utilizar entidades de varios valores con un modelo de Conversation Learner.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.subservice: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 07550ed9c78aa584e55403d225eef4bcff33b057
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2019
ms.locfileid: "58167429"
---
# <a name="how-to-use-multi-value-entities-with-a-conversation-learner-model"></a>Uso de entidades de varios valores con un modelo de Conversation Learner
Este tutorial muestra la propiedad de varios valores de las entidades.

## <a name="video"></a>Vídeo

[![Vista previa de Tutorial de las entidades de varios valores](https://aka.ms/cl_Tutorial_v3_MultiValued_Preview)](https://aka.ms/cl_Tutorial_v3_MultiValued)

## <a name="requirements"></a>Requisitos
Para poder realizar este tutorial, el bot de tutorial general debe estar en ejecución.

    npm run tutorial-general

## <a name="details"></a>Detalles
Las entidades de varios valores acumulan valores en una lista, en lugar de almacenar un valor único.  Estas entidades son útiles cuando los usuarios pueden especificar más de un valor. Los ingredientes de una pizza, por ejemplo.

Las entidades marcadas como de varios valores tendrán cada instancia reconocida de la entidad anexada a una lista en la memoria del bot. El reconocimiento posterior se anexa al valor de la entidad, en lugar de sobrescribirse.

## <a name="steps"></a>Pasos

Empiece en la página principal de la interfaz de usuario web.

### <a name="create-the-model"></a>Crear el modelo

1. Seleccione **nuevo modelo**.
2. Escriba **MultiValueEntities** para **nombre**.
3. Seleccione **Crear**.

### <a name="entity-creation"></a>Crear la entidad

1. Seleccione **entidades** en el panel izquierdo, a continuación, **nueva entidad**.
2. Seleccione **personalizado entrenado** para **tipo de entidad**.
3. Escriba **ingredientes** para **nombre de entidad**.
4. Comprobar **multivalor** para habilitar la entidad se acumulan uno o varios valores.
5. Comprobar **negable**.
6. Seleccione **Crear**.

![](../media/T07_entity_create.png)

### <a name="create-the-first-action"></a>Crear la primera acción

1. Seleccione **acciones** en el panel izquierdo, a continuación, **nueva acción**.
2. Escriba **aquí están los ingredientes: $toppings** para **respuesta del Bot...** . Un signo de dólar indica una referencia de entidad.
3. Seleccione **Crear**.

![](../media/T07_action_create_1.png)

### <a name="create-the-second-action"></a>Crear la segunda acción

1. Seleccione **acciones** en el panel izquierdo, a continuación, **nueva acción**.
2. Escriba **qué ingredientes gustaría?** para **respuesta del Bot...** .
3. Escriba **ingredientes** para **descalificación le da derecho al**.
4. Seleccione **Crear**.

Ahora tiene dos acciones.

![](../media/T07_action_create_2.png)

### <a name="train-the-model"></a>Entrenar el modelo

1. Seleccione **cuadros de diálogo "Train"** en el panel izquierdo, a continuación, **el cuadro de diálogo de nuevo "Train"**.
2. Escriba **Hola** para utterance del usuario en el panel izquierdo del chat.
3. Seleccione **puntuar acciones**.
4. Seleccione **qué ingredientes gustaría?** en la lista de acciones. El percentil es 100% como la única acción válida según las restricciones.
5. Escriba **queso y hongos** para utterance del usuario en el panel izquierdo del chat.
6. Resaltar **queso** , a continuación, seleccione **+ ingredientes**.
7. Resaltar **hongos** , a continuación, seleccione **+ ingredientes**.
8. Seleccione **puntuar acciones**.
9. Seleccione **aquí están los ingredientes: $toppings** en la lista de acciones.
10. Escriba **agregar pimienta** para la declaración siguiente del usuario en el panel izquierdo del chat.
11. Resaltar **pimienta** , a continuación, seleccione **+ ingredientes**.
12. Seleccione **puntuar acciones**.
13. Seleccione **aquí están los ingredientes: $toppings** en la lista de acciones.
14. Escriba **quitar queso** para utterance terceros del usuario en el panel izquierdo del chat.
15. Resaltar **queso** , a continuación, seleccione **-ingredientes**.
16. Seleccione **puntuar acciones**.
17. Seleccione **aquí están los ingredientes: $toppings** en la lista de acciones.

![](../media/T07_training.png)

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Entidades previamente entrenadas](./08-pre-trained-entities.md)
