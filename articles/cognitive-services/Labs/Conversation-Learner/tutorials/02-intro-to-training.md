---
title: 'Introducción al entrenamiento de un modelo de Conversation Learner: Microsoft Cognitive Services | Microsoft Docs'
titleSuffix: Azure
description: Obtenga información sobre cómo entrenar un modelo que incluya ramificación y edición del entrenamiento anterior a través de Conversation Learner.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.subservice: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 1e6dea521207ec00efe948a879c705bd28a75d57
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2019
ms.locfileid: "55213203"
---
# <a name="introduction-to-training"></a>Introducción al entrenamiento

Este tutorial muestra los conceptos básicos del entrenamiento de un modelo, la ramificación del nuevo entrenamiento a partir de un entrenamiento anterior y la edición de una respuesta de bot para cambiarla.

## <a name="video"></a>Vídeo

[![Vista preliminar del tutorial de introducción al entrenamiento](https://aka.ms/cl_Tutorial_v3_IntroTraining_Preview)](https://aka.ms/cl_Tutorial_v3_IntroTraining)

## <a name="requirements"></a>Requisitos
Para poder realizar este tutorial debe ejecutar el bot del tutorial general

    npm run tutorial-general

## <a name="details"></a>Detalles

- Acciones: Una respuesta de bot a la entrada del usuario.
- Entrenar: Cómo enseñamos a un bot a responder a las entradas del usuario.
- Ramificación: La modificación de una entrada de usuario dentro de un diálogo de entrenamiento guardado con el fin de crear un nuevo diálogo de entrenamiento que empiece igual que el original pero lleve la conversación hacia una dirección diferente.

## <a name="steps"></a>Pasos

### <a name="create-a-new-model"></a>Creación de un modelo nuevo

1. En la interfaz de usuario web, haga clic en New Model (Modelo nuevo).
2. Para "Name" (Nombre), escriba "Bot de inspiración". A continuación, haga clic en Crear.

### <a name="create-an-action"></a>Creación de una acción

1. En el panel izquierdo, haga clic en "Actions" (Acciones) y, a continuación, en el botón "New Action" (Nueva acción).
2. En el campo "Bot's response..." (Respuesta del bot), escriba "Hola. ¿Le gustaría estar inspirado hoy?".
    - Deje todos los otros campos y las casillas en su configuración predeterminada.
3. Haga clic en Crear.

### <a name="first-training-and-creating-another-action-while-training"></a>Primer entrenamiento y creación de otra acción durante el entrenamiento

1. En el panel izquierdo, haga clic en "Train Dialogs" (Diálogos de entrenamiento) y, a continuación, en el botón "New Train Dialog" (Nuevo diálogo de entrenamiento).
2. En el panel del chat, donde se indica "Type your message..." (Escriba su mensaje…), escriba "hola". 
    - Esto simula la parte del usuario de la conversación.
3. Haga clic en "Score Actions" (Acciones de puntuación).
4. Seleccione la respuesta "Hola. ¿Le gustaría estar inspirado hoy?".
5. Como usuario, responda "sí".
6. Haga clic en "Score Actions" (Acciones de puntuación).
7. Haga clic en el botón "+Action" (+Acción). 
    - Esto le llevará al cuadro de diálogo conocido "Create an Action" (Crear una acción).
8. Indique la siguiente respuesta del bot: "Usted es una gran persona".
9. Haga clic en Crear.
10. Observe que el bot responde inmediatamente.
11. Haga clic en el botón "Save" (Guardar).

### <a name="branch-a-second-training-off-of-the-first-training"></a>Ramificación de un segundo entrenamiento externo al primer entrenamiento
1. Haga clic en la fila de cuadrícula que resume el primer entrenamiento. 
    - Esto le permite ver y editar el entrenamiento existente.
2. Haga clic en la respuesta del usuario, "sí". 
    - Se mostrarán los controles de edición.
3. Haga clic en el icono de rama. 
    - Se abrirá una solicitud de entrada de un usuario diferente para una conversación nueva.
4. Escriba "no" y pulse Entrar o haga clic en el botón "Create" (Crear). 
    - En este momento, tendrá una nueva instancia de un diálogo de entrenamiento, mientras que el original permanece sin cambios.
5. Haga clic en "Score Actions" (Acciones de puntuación).
6. Haga clic en la respuesta incorrecta del bot que acaba de aparecer.
7. Haga clic en el botón "+Action" (+Acción) 
    - para que podamos crear una nueva acción para que el bot responda con ella.
8. En la respuesta del bot, responda como "Ningún problema. ¡Que tenga un buen día!"
9. Haga clic en Crear.
10. Observe que el bot responde inmediatamente.
11. Haga clic en el botón "Save" (Guardar).

### <a name="test-the-trainings"></a>Prueba de los entrenamientos
1. En el panel izquierdo, haga clic en "Log Dialogs" (Diálogos de registro) y, a continuación, en "New Log Dialog" (Nuevo diálogo de registro).
2. Escriba el mensaje, "hola". 
3. Observe que el bot responde automáticamente tal y como lo hemos entrenado.
4. Escriba la respuesta de usuario, "sí".
5. Observe la respuesta de bot: muestra que está funcionando el primer entrenamiento.
6. Haga clic en el botón "Session Timeout" (Tiempo de espera de sesión). Esto indica a Conversation Learner que queremos empezar de nuevo, omitiendo los turnos de conversación que acaban de producirse.
7. Escriba el mensaje, "hola". 
8. Observe que el bot responde automáticamente tal y como lo hemos entrenado.
9. Escriba la respuesta del usuario, "no".
10. Observe la respuesta de bot: muestra que está funcionando el segundo entrenamiento.
11. Haga clic en el botón "Done Testing" (Finalizar la prueba).

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Acciones de espera y acciones de no espera](./03-wait-vs-nonwait-actions.md)
