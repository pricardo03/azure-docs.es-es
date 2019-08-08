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
ROBOTS: NOINDEX
ms.openlocfilehash: 4edbfc396d4ab32b991b084a7b738f04b5205418
ms.sourcegitcommit: ad9120a73d5072aac478f33b4dad47bf63aa1aaa
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/01/2019
ms.locfileid: "68704104"
---
# <a name="how-to-use-negatable-entities-with-a-conversation-learner-model"></a>Uso de entidades negables con un modelo de Conversation Learner

En este tutorial se explica la propiedad "Negatable" (Negable) de las entidades.

## <a name="video"></a>Vídeo

[![Vista preliminar del tutorial de entidades negables](https://aka.ms/cl_Tutorial_v3_NegatableEntities_Preview)](https://aka.ms/cl_Tutorial_v3_NegatableEntities)

## <a name="requirements"></a>Requisitos
Para poder realizar este tutorial, el bot de tutorial general debe estar en ejecución.

    npm run tutorial-general

## <a name="details"></a>Detalles
La propiedad "Negatable" (Negable) de una entidad permite etiquetar las instancias normales (positivas) y negativas de la entidad, entrenar en función de los modelos positivos y negativos, y borrar el valor de una entidad existente. Las entidades que tienen su propiedad "Negatable" establecida se denominan entidades negables en Conversation Leaner.

## <a name="steps"></a>Pasos

Empiece en la página principal de la interfaz de usuario web.

### <a name="create-the-model"></a>Crear el modelo

1. Seleccione **New Model** (Nuevo modelo).
2. Escriba **NegatableEntity** en **Name** (Nombre).
3. Seleccione **Crear**.

### <a name="entity-creation"></a>Crear la entidad

1. Seleccione **Entities** (Entidades) en el panel izquierdo y, después, **New Entity** (Nueva entidad).
2. Seleccione **Custom Trained** (Entrenamiento personalizado) en **Entity Type** (Tipo de entidad).
3. Escriba el **nombre** en **Entity Name** (Nombre de entidad).
4. Active **Negatable** para permitir que los usuarios proporcionen un valor de entidad o indique que algo *no* es una entidad de valor y, por tanto, se elimina el valor de la entidad coincidente.
5. Seleccione **Crear**.

![](../media/T06_entity_create.png)

### <a name="create-the-first-action"></a>Crear la primera acción

1. Seleccione **Actions** (Acciones) en el panel izquierdo y, a continuación, **New Action** (Nueva acción).
2. Escriba **No sé su nombre.** como **Bot's response...** (Respuesta del bot).
3. En **Disqualifying Entities** (Entidades descalificadas), escriba el **nombre**.
4. Seleccione **Crear**.

![](../media/T06_action_create_1.png)

### <a name="create-the-second-action"></a>Crear la segunda acción

1. Seleccione **Actions** (Acciones) en el panel izquierdo y, a continuación, **New Action** (Nueva acción).
2. Escriba **Sé su nombre. Se llama $nombre.** como **Bot's response...** (Respuesta del bot).
3. Seleccione **Crear**.

> [!NOTE]
> La entidad **nombre** se ha agregado automáticamente como una de las **Required Entities** (Entidades necesarias) por referencia en la expresión de respuesta.

Ahora tiene dos acciones.

![](../media/T06_action_create_2.png)

### <a name="train-the-model"></a>Entrenar el modelo

1. Seleccione **Train Dialogs** (Diálogos de entrenamiento) en el panel izquierdo y, a continuación, **New Train Dialog** (Nuevo diálogo de entrenamiento).
2. Escriba **hola** como expresión del usuario en el panel izquierdo del chat.
3. Seleccione **Score Actions** (Acciones de puntuación).
4. Seleccione **No sé su nombre.** en la lista de acciones. El percentil es 100 %, ya que la única acción válida se basa en las restricciones.
5. Escriba **Me llamo Francisco** como expresión del usuario en el panel izquierdo del chat.
6. Resalte **Francisco** y, a continuación, seleccione **+nombre**. Las entidades negables tienen dos instancias: (+) más agrega o sobrescribe el valor; (-) menos quita el valor.
7. Seleccione **Score Actions** (Acciones de puntuación). La entidad **nombre** ahora se define como **Francisco** en la memoria del modelo, por lo que acción **Sé cómo se llama. Se llama $nombre** está disponible.
8. Seleccione **Sé su nombre. Se llama $nombre.** en la lista de acciones.
9. Escriba **No me llamo Francisco.** como expresión del usuario en el panel izquierdo del chat.
10. Resalte **Francisco** y, a continuación, seleccione **-nombre** para borrar el valor de la entidad **nombre**.
11. Seleccione **Score Actions** (Acciones de puntuación).
12. Seleccione **No sé su nombre.** en la lista de acciones.
13. Escriba **Mi nombre es Susana.** como expresión del tercer usuario en el panel izquierdo del chat.
14. Resalte **Susana** y, a continuación, **+nombre** 

![](../media/T06_training.png)

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Entidades de varios valores](./07-multi-value-entities.md)
