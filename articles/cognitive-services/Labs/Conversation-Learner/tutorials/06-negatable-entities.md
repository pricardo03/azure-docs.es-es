---
title: 'Uso de entidades negables con un modelo de Conversation Learner: Microsoft Cognitive Services | Microsoft Docs'
titleSuffix: Azure
description: Obtenga información sobre cómo usar entidades negables con un modelo de Conversation Learner.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 34806803c50308a288007bf3bad129075ca8bc6d
ms.sourcegitcommit: 295babdcfe86b7a3074fd5b65350c8c11a49f2f1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/27/2018
ms.locfileid: "53796701"
---
# <a name="how-to-use-negatable-entities-with-a-conversation-learner-model"></a>Uso de entidades negables con un modelo de Conversation Learner

En este tutorial se explica la propiedad "Negatable" (Negable) de las entidades.

## <a name="video"></a>Vídeo

[![Vista preliminar del tutorial de entidades negables](https://aka.ms/cl_Tutorial_v3_NegatableEntities_Preview)](https://aka.ms/cl_Tutorial_v3_NegatableEntities)

## <a name="requirements"></a>Requisitos
Para poder realizar este tutorial debe ejecutar el bot del tutorial general

    npm run tutorial-general

## <a name="details"></a>Detalles
La propiedad "Negatable" (Negable) de una entidad permite etiquetar las instancias normales (positivas) y negativas de la entidad, entrenar en base a los modelos positivos y negativos y borrar el valor de una entidad existente. Las entidades que tienen su propiedad "Negatable" establecida se denominan entidades negables en Conversation Leaner.

## <a name="steps"></a>Pasos

### <a name="create-the-model"></a>Creación del modelo

1. En la interfaz de usuario web, haga clic en "New Model" (Modelo nuevo).
2. En el campo "Name" (Nombre), escriba "EntidadNegable" y pulse Entrar.
3. Haga clic en el botón "Create" (Crear).

### <a name="entity-creation"></a>Creación de la entidad

1. En el panel izquierdo, haga clic en "Entities" (Entidades) y, a continuación, en el botón "New Entity" (Nueva entidad).
2. Seleccione "Custom" (Personalizada) para "Entity Type" (Tipo de entidad).
3. Escriba "nombre" para "Entity Name" (Nombre de entidad).
4. Active la casilla "Negatable" (Negable).
    - Al activar esta propiedad, se permite al usuario proporcionar un valor de la entidad o afirmar que algo *no* es un valor de entidad. En el último caso, el resultado es la eliminación del valor de entidad coincidente.
5. Haga clic en el botón "Create" (Crear).

![](../media/tutorial5_entities.PNG)

### <a name="create-the-first-action"></a>Creación de la primera acción

1. En el panel izquierdo, haga clic en "Actions" (Acciones) y, a continuación, en el botón "New Action" (Nueva acción).
2. En el campo "Bot's response..." (Respuesta del bot), escriba "No sé cómo se llama".
3. En el campo "Disqualifying Entities" (Entidades descalificadoras), escriba "nombre".
4. Haga clic en el botón "Create" (Crear).

### <a name="create-the-second-action"></a>Creación de la segunda acción

1. En el panel izquierdo, haga clic en "Actions" (Acciones) y, a continuación, en el botón "New Action" (Nueva acción).
2. En el campo "Bot's response..." (Respuesta del bot), escriba "Sé cómo se llama. Se llama $nombre".
3. Haga clic en el botón "Create" (Crear).

> [!NOTE]
> La entidad "nombre" se ha agregado automáticamente como una de las "Required Entities" (Entidades necesarias) por referencia en la respuesta.

Ahora tiene dos acciones.

![](../media/tutorial5_actions.PNG)

### <a name="train-the-model"></a>Entrenamiento del modelo

1. En el panel izquierdo, haga clic en "Train Dialogs" (Diálogos de entrenamiento) y, a continuación, en el botón "New Train Dialog" (Nuevo diálogo de entrenamiento).
2. En el panel del chat, donde se indica "Type your message..." (Escriba su mensaje…), escriba "hola".
3. Haga clic en el botón "Score Actions" (Puntuar acciones).
4. Seleccione la respuesta "No sé cómo se llama".
    - El percentil es 100 %, ya que la única acción válida se basa en las restricciones.
5. En el panel del chat, donde dice "Type your message..." (Escriba su mensaje…), escriba "Mi nombre es Francisco"
6. Seleccione "Francisco" y elija la etiqueta "+nombre"
    - Hay dos instancias de la entidad "nombre": "+nombre" y "-nombre".  El signo más (+) agrega o sobrescribe el valor. El signo menos (-) quita el valor.
7. Haga clic en el botón "Score Actions" (Puntuar acciones).
    - La entidad "nombre" ahora se define como "Francisco" en la memoria del modelo, por lo que acción "Sé cómo se llama. Se llama $nombre" está disponible.
8. Seleccione la respuesta "Sé cómo se llama. Se llama $nombre".
9. En el panel del chat, donde dice "Type your message..." (Escriba su mensaje…), escriba "Mi nombre no es Francisco".
10. Seleccione "Francisco" y elija la etiqueta "-nombre"
    - Seleccionamos "-nombre" para borrar el valor actual de la entidad.
11. Haga clic en el botón "Score Actions" (Puntuar acciones).
12. Seleccione la respuesta "No sé cómo se llama".
13. En el panel del chat, donde dice "Type your message..." (Escriba su mensaje…), escriba "Mi nombre es Susana".
14. Seleccione "Susana" y elija la etiqueta "+nombre".

![](../media/tutorial5_dialogs.PNG)

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Entidades de varios valores](./07-multi-value-entities.md)
