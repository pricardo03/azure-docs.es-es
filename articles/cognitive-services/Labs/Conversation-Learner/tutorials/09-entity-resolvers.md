---
title: Las resoluciones de entidad en un modelo de aprendiz conversación - Azure Cognitive Services | Microsoft Docs
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
ms.openlocfilehash: 51f74f504f0ad70c8c7f73be8ee6a05add685824
ms.sourcegitcommit: cababb51721f6ab6b61dda6d18345514f074fb2e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/04/2019
ms.locfileid: "66475743"
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

1. Seleccione **nuevo modelo**.
2. Escriba **solucionadores de entidad** para **nombre**.
3. Seleccione **Crear**.

### <a name="create-a-pair-of-entities"></a>Creación de un par de entidades

1. Seleccione **entidades** en el panel izquierdo, a continuación, **nueva entidad**.
2. Escriba **partida** para **nombre de entidad**.
3. Seleccione **datetimeV2** para **tipo de resolución**.
4. Seleccione **Crear**. Descartar el cuadro emergente informativo seleccionando **Aceptar**.
5. Repita los pasos 1 a 4 para crear una segunda entidad denominada **devolver** con **datetimeV2** tipo de resolución.

![](../media/T09_entities.png)

### <a name="create-a-pair-of-actions"></a>Creación de un par de acciones

1. Seleccione **acciones** en el panel izquierdo, a continuación, **nueva acción**.
2. Escriba **están dejando en $departure y devolver en $return** para **respuesta del Bot...** .
    - Importante: al escribir en $[entityName] deberá aciertos escriba o haga clic en la entidad en la lista desplegable aprendiz de conversación en caso contrario, considerará que el texto en lugar de una entidad.
    - Tenga en cuenta que el **necesario entidades** campo también obtendrá estas entidades y no puede quitarse. De esta forma, se impide que esta acción esté disponible hasta que las dos entidades necesarias estén presentes.
3. Seleccione **Crear**.
4. Seleccione **nueva acción** para crear una segunda acción.
5. Escriba **cuando va a viajar?** para **respuesta del Bot...** .
6. Escriba **partida** y **devolver** para **descalificación entidades**. Estas indican a nuestro bot que NO realice esta acción si alguna de estas entidades contiene un valor.
7. Seleccione **Crear**.

![](../media/T09_actions.png)

### <a name="training"></a>Cursos

1. Inspección del **entrenamiento: [Status]** en la esquina superior izquierda para **completado**.
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
    - Mantenga el mouse sobre cada uno de ellos y observe cómo las entidades son objetos de fecha que se capturan con claridad la fecha del calendario real en lugar de "Sunday" o "tomorrow".
10. Seleccione la respuesta del bot "Sale el…" .
11. Haga clic en el botón "Save" (Guardar).

![](../media/T09_training.png)

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Entidades de enum](./tutorial-enum-set-entity.md)
