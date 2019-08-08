---
title: 'Resoluciones de entidad en un modelo de Conversation Learner: Azure Cognitive Services | Microsoft Docs'
titleSuffix: Azure
description: Obtenga información sobre cómo usar resoluciones de entidad en Conversation Learner.
services: cognitive-services
author: nitinme
manager: nolachar
ms.service: cognitive-services
ms.subservice: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: nitinme
ROBOTS: NOINDEX
ms.openlocfilehash: 60c4abf1590cb91fd460cc6a2a5ba75a225ebd80
ms.sourcegitcommit: ad9120a73d5072aac478f33b4dad47bf63aa1aaa
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/01/2019
ms.locfileid: "68704039"
---
# <a name="entity-resolvers"></a>Resoluciones de entidad

En este tutorial se muestra cómo usar resoluciones de entidad en Conversation Learner.

## <a name="video"></a>Vídeo

[![Vista previa del tutorial de resoluciones de entidad](https://aka.ms/cl_Tutorial_v3_EntityResolvers_Preview)](https://aka.ms/cl_Tutorial_v3_EntityResolvers)

## <a name="requirements"></a>Requisitos
Para poder realizar este tutorial, el bot de tutorial general debe estar en ejecución.

    npm run tutorial-general

## <a name="details"></a>Detalles

- El tipo de resolución es una propiedad opcional de las entidades personalizadas.
- Las resoluciones de entidad usan la eficacia de los reconocedores de entidad previamente entrenados de LUIS para proporcionar detalles adicionales y precisión a la entidad personalizada.

## <a name="steps"></a>Pasos

Empiece en la página principal de la interfaz de usuario web.

### <a name="create-the-model"></a>Crear el modelo

1. Seleccione **New Model** (Nuevo modelo).
2. Escriba **Entity Resolvers** (Solucionadores de entidad) en **Name** (Nombre).
3. Seleccione **Crear**.

### <a name="create-a-pair-of-entities"></a>Creación de un par de entidades

1. Seleccione **Entities** (Entidades) en el panel izquierdo y, después, **New Entity** (Nueva entidad).
2. Escriba **partida** en **Entity Name** (Nombre de entidad).
3. Seleccione **datetimeV2** en **Resolver Type** (Tipo de resolución).
4. Seleccione **Crear**. Descarte el cuadro emergente informativo; para ello, seleccione **OK** (Aceptar).
5. Repita los pasos 1 a 4 para crear una segunda entidad denominada **regreso** con el tipo de resolución **datetimeV2**.

![](../media/T09_entities.png)

### <a name="create-a-pair-of-actions"></a>Creación de un par de acciones

1. Seleccione **Actions** (Acciones) en el panel izquierdo y, a continuación, **New Action** (Nueva acción).
2. Escriba **Sale el $partida y vuelve el $regreso** en el campo **Bot's response** (Respuesta del bot).
    - IMPORTANTE: Al escribir $[entityName], tiene que presionar Entrar o hacer clic en la entidad en la lista desplegable; de lo contrario, Conversation Learner considerará que es texto en lugar de una entidad.
    - Fíjese en que el campo **Required Entities** (Entidades necesarias) también obtendrá estas entidades y que no se pueden quitar. De esta forma, se impide que esta acción esté disponible hasta que las dos entidades necesarias estén presentes.
3. Seleccione **Crear**.
4. Seleccione **New Action** (Nueva acción) para crear una segunda acción.
5. Escriba **¿Cuándo tiene pensado viajar?** en **Bot's response** (Respuesta del bot).
6. Escriba **partida** y **regreso** en **Disqualifying Entities** (Entidades descalificadas). Estas indican a nuestro bot que NO realice esta acción si alguna de estas entidades contiene un valor.
7. Seleccione **Crear**.

![](../media/T09_actions.png)

### <a name="training"></a>Cursos

1. Examine el campo **Training: [Status]** (Entrenamiento: [Estado]) en la esquina superior izquierda para **Completed** (Completado).
    - Puede hacer clic en el vínculo "Refresh" (Actualizar) si tarda demasiado tiempo.
    - Es necesario que el estado del entrenamiento sea "Completado" para que nuestras resoluciones de entidades funcionen cuando entrenemos el modelo.

2. En el panel izquierdo, haga clic en "Train Dialogs" (Diálogos de entrenamiento) y, después, en el botón "New Train Dialog" (Nuevo diálogo de entrenamiento).
3. Escriba la primera expresión del usuario, "resérveme un vuelo". 
4. Haga clic en el botón "Score Actions" (Acciones de puntuación).
5. Seleccione la respuesta, "¿Cuándo tiene pensado viajar?".
6. Como el usuario, responda "salgo mañana y vuelvo el domingo de la próxima semana".
    - Observe cómo ha detectado Conversation Learner dos entidades "Pre-Trained date" (fecha entrenada previamente) en ese turno del usuario.
7. En el panel "Entity Detection" (Detección de entidad), seleccione el texto "mañana" y etiquételo como "salida".
8. Etiquete también el texto "domingo de la próxima semana" como "vuelta".
9. Haga clic en el botón "Score Actions" (Acciones de puntuación).
    - Observe cómo el panel "Memory" (Memoria) contiene las fechas de salida y de vuelta.
    - Mantenga el puntero sobre cada una y observe cómo las entidades son objetos de fecha que capturan claramente la fecha del calendario real en lugar de "Sunday" (Domingo) o "Tomorrow" (Mañana).
10. Seleccione la respuesta del bot "Sale el…" .
11. Haga clic en el botón "Save" (Guardar).

![](../media/T09_training.png)

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Entidades ENUM](./tutorial-enum-set-entity.md)
