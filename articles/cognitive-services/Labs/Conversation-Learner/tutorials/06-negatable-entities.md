---
title: 'Uso de entidades negables con un modelo de Conversation Learner: Microsoft Cognitive Services | Microsoft Docs'
titleSuffix: Azure
description: Obtenga información sobre cómo usar entidades negables con un modelo de Conversation Learner.
services: cognitive-services
author: nitinme
manager: nolachar
ms.service: cognitive-services
ms.subservice: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: nitinme
ms.openlocfilehash: 2c758d1dd5d4d1e7ab25faccd5280963211181d1
ms.sourcegitcommit: 3d4121badd265e99d1177a7c78edfa55ed7a9626
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/30/2019
ms.locfileid: "66388803"
---
# <a name="how-to-use-negatable-entities-with-a-conversation-learner-model"></a>Uso de entidades negables con un modelo de Conversation Learner

En este tutorial se explica la propiedad "Negatable" (Negable) de las entidades.

## <a name="video"></a>Vídeo

[![Vista preliminar del tutorial de entidades negables](https://aka.ms/cl_Tutorial_v3_NegatableEntities_Preview)](https://aka.ms/cl_Tutorial_v3_NegatableEntities)

## <a name="requirements"></a>Requisitos
Para poder realizar este tutorial, el bot de tutorial general debe estar en ejecución.

    npm run tutorial-general

## <a name="details"></a>Detalles
La propiedad "Negatable" de una entidad permite etiquetar ambos normal (positivo) y negativos de instancias de la entidad, enseñar basados en modelos positivos y negativos y borrar el valor de una entidad existente. Las entidades que tienen su propiedad "Negatable" establecida se denominan entidades negables en Conversation Leaner.

## <a name="steps"></a>Pasos

Empiece en la página principal de la interfaz de usuario web.

### <a name="create-the-model"></a>Crear el modelo

1. Seleccione **nuevo modelo**.
2. Escriba **NegatableEntity** para **nombre**.
3. Seleccione **Crear**.

### <a name="entity-creation"></a>Crear la entidad

1. Seleccione **entidades** en el panel izquierdo, a continuación, **nueva entidad**.
2. Seleccione **personalizado entrenado** para **tipo de entidad**.
3. Escriba **nombre** para **nombre de entidad**.
4. Comprobar **Negatable** para permitir que los usuarios proporcionar un valor de la entidad o diga algo es *no* valor de una entidad, por tanto, debe eliminar el valor de la entidad coincidente.
5. Seleccione **Crear**.

![](../media/T06_entity_create.png)

### <a name="create-the-first-action"></a>Crear la primera acción

1. Seleccione **acciones** en el panel izquierdo, a continuación, **nueva acción**.
2. Escriba **no sé su nombre.** para **respuesta del Bot...** .
3. Escriba **nombre** para **descalificación le da derecho al**.
4. Seleccione **Crear**.

![](../media/T06_action_create_1.png)

### <a name="create-the-second-action"></a>Crear la segunda acción

1. Seleccione **acciones** en el panel izquierdo, a continuación, **nueva acción**.
2. Escriba **sé su nombre. Es $name.** para **respuesta del Bot...** .
3. Seleccione **Crear**.

> [!NOTE]
> El **nombre** entidad se agregó automáticamente como un **necesario entidades** por referencia en la declaración de la respuesta.

Ahora tiene dos acciones.

![](../media/T06_action_create_2.png)

### <a name="train-the-model"></a>Entrenar el modelo

1. Seleccione **cuadros de diálogo "Train"** en el panel izquierdo, a continuación, **el cuadro de diálogo de nuevo "Train"** .
2. Escriba **hello** para utterance del usuario en el panel izquierdo del chat.
3. Seleccione **puntuar acciones**.
4. Seleccione **no sé su nombre.** en la lista de acciones. El percentil es 100% como la única acción válida según las restricciones.
5. Escriba **me llamo Frank** para utterance del usuario en el panel izquierdo del chat.
6. Resaltar **Frank** , a continuación, seleccione **+ nombre**. Entidades negable tienen dos instancias: (+) y agrega o sobrescribe el valor; (-) menos quita el valor.
7. Seleccione **puntuar acciones**. El **nombre** entidad ahora se define como **Frank** en la memoria del modelo, por lo que el **sé su nombre. Es $name** acción está disponible.
8. Seleccione **sé su nombre. Es $name.** en la lista de acciones.
9. Escriba **Frank, mi nombre no es.** para la declaración del usuario en el panel izquierdo del chat.
10. Resaltar **Frank** , a continuación, seleccione **-nombre** para borrar el valor de la **nombre** entidad.
11. Seleccione **puntuar acciones**.
12. Seleccione **no sé su nombre.** en la lista de acciones.
13. Escriba **Susan, mi nombre es.** para la declaración terceros del usuario en el panel izquierdo del chat.
14. Resaltar **Susan** , a continuación, **+ nombre** 

![](../media/T06_training.png)

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Entidades de varios valores](./07-multi-value-entities.md)
