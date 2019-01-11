---
title: 'Resoluciones de entidad en un modelo de Conversation Learner: Microsoft Cognitive Services | Microsoft Docs'
titleSuffix: Azure
description: Obtenga información sobre cómo usar resoluciones de entidad en Conversation Learner.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: cc1f9ea18d425679a4db9c7b91a1c0a3c4451d4b
ms.sourcegitcommit: 295babdcfe86b7a3074fd5b65350c8c11a49f2f1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/27/2018
ms.locfileid: "53796702"
---
# <a name="entity-resolvers"></a>Resoluciones de entidad

En este tutorial se muestra cómo usar resoluciones de entidad en Conversation Learner.

## <a name="video"></a>Vídeo

[![Vista previa del tutorial de resoluciones de entidad](https://aka.ms/cl_Tutorial_v3_EntityResolvers_Preview)](https://aka.ms/cl_Tutorial_v3_EntityResolvers)

## <a name="requirements"></a>Requisitos
Para poder realizar este tutorial debe ejecutar el bot del tutorial general

    npm run tutorial-general

## <a name="details"></a>Detalles

- El tipo de resolución es una propiedad opcional de las entidades personalizadas.
- Las resoluciones de entidad usan la eficacia de los reconocedores de entidad previamente entrenados de LUIS para proporcionar detalles adicionales y precisión a la entidad personalizada.

## <a name="steps"></a>Pasos

### <a name="create-a-new-model"></a>Creación de un modelo

1. En la interfaz de usuario web, haga clic en el botón "New Model" (Nuevo modelo).
2. En el campo "Name" (Nombre), escriba "Entity Resolvers" (Resoluciones de entidad) y pulse Entrar o haga clic en el botón "Crear" (Create).

### <a name="create-a-pair-of-entities"></a>Creación de un par de entidades

1. En el panel izquierdo, haga clic en "Entities" (Entidades) y después en el botón "New Entity" (Nueva entidad).
2. En el campo "Entity Name" (Nombre de entidad), escriba "salida".
3. En la lista desplegable "Resolver Type" (Tipo de resolución), seleccione "datetimeV2".
4. Haga clic en el botón "Create" (Crear).
5. Haga clic en el botón "OK" (Aceptar) después de haber leído el elemento emergente de información.
6. Siga los mismos pasos y cree otra entidad denominada "vuelta" que también tenga un tipo de resolución "datetimeV2".

### <a name="create-a-pair-of-actions"></a>Creación de un par de acciones

1. En el panel izquierdo, haga clic en "Actions" (Acciones) y después en el botón "New Action" (Nueva acción).
2. En el campo "Bot's response" (Respuesta del bot), escriba "Sale el $salida y vuelve el $vuelta".
    - IMPORTANTE: Al escribir "$[nombreEntidad]", tiene que presionar Entrar o hacer clic en la entidad en la lista desplegable; de lo contrario, Conversation Learner considerará que esto es texto en lugar de una entidad.
    - Fíjese en que el campo "Required Entities" (Entidades necesarias) también obtendrá estas entidades y que no se pueden quitar. De esta forma, se impide que esta acción esté disponible hasta que las dos entidades necesarias estén presentes.
3. Haga clic en el botón "Create" (Crear).
4. Vuelva a hacer clic en el botón "New Action" (Nueva acción) para crear una segunda acción.
5. En el campo "Bot's response" (Respuesta del bot), escriba "¿Cuándo tiene pensado viajar?".
6. En el cuadro "Disqualifying Entities" (Entidades descalificadoras), escriba "salida" y "vuelta".
    - Estas indican a nuestro bot que NO realice esta acción si alguna de estas entidades contiene un valor.
7. Haga clic en el botón "Create" (Crear).


### <a name="training"></a>Cursos

1. Vea el mensaje "Training: [Status]" (Entrenamiento: [Estado]) situado en la parte superior izquierda de la página y espere a que sea "Completed" (Completado).
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
    - Mantenga el puntero sobre cada una y observe cómo las entidades son objetos de fecha que capturan claramente la fecha del calendario real en lugar de "domingo" o "mañana".
10. Seleccione la respuesta del bot "Sale el…" .
11. Haga clic en el botón "Save" (Guardar).

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Entradas alternativas](./10-alternative-inputs.md)
