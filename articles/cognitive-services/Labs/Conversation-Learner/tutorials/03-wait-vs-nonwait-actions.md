---
title: 'Uso de acciones de espera y de no espera con un modelo de Conversation Learner: Microsoft Cognitive Services | Microsoft Docs'
titleSuffix: Azure
description: Obtenga información sobre cómo usar las acciones de espera y de no espera con un modelo de Conversation Learner.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.subservice: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 2869e6c59388adf548e5f239d3a831a5a3f060dd
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60707621"
---
# <a name="wait-and-non-wait-actions"></a>Acciones de espera y acciones de no espera

En este tutorial se muestra la diferencia entre las acciones de espera y las acciones de no espera en Conversation Learner.

## <a name="video"></a>Vídeo

[![Vista preliminar del tutorial de espera y de no espera ](https://aka.ms/cl_Tutorial_v3_WaitnonWait_Preview)](https://aka.ms/cl_Tutorial_v3_WaitnonWait)

## <a name="requirements"></a>Requisitos
Para poder realizar este tutorial debe ejecutar el bot del tutorial general

    npm run tutorial-general

## <a name="details"></a>Detalles

- Acción de espera: Después que el sistema haya realizado una acción de "espera", dejará de realizar acciones y esperará la entrada del usuario.
- Acción de no espera: Después que el sistema haya realizado una acción de "no espera", elegirá de inmediato otra acción (sin esperar la entrada por parte del usuario).

## <a name="steps"></a>Pasos

### <a name="create-a-new-model"></a>Creación de un modelo nuevo

1. En la interfaz de usuario web, haga clic en New Model (Modelo nuevo).
2. En el campo "Name" (Nombre), escriba "Esperar o no esperar" y pulse Entrar o haga clic en el botón "Crear" (Create).

### <a name="create-the-first-two-wait-actions"></a>Creación de las primeras dos acciones de espera

1. En el panel izquierdo, haga clic en "Actions" (Acciones) y, a continuación, en el botón "New Action" (Nueva acción).
2. En el campo "Bot's response..." (Respuesta del bot), escriba "¿Qué pizza quiere?".
    - Esta es una acción de espera, así que deje activada la casilla "Wait for Response" (Esperar respuesta).
3. Haga clic en el botón "Create" (Crear).
4. Repita esos pasos y cree otra acción con "¡Marchando una pizza!" como respuesta del bot.

### <a name="train-using-those-wait-actions"></a>Entrenamiento con las acciones de espera

1. En el panel izquierdo, haga clic en "Train Dialogs" (Diálogos de entrenamiento) y, a continuación, en el botón "New Train Dialog" (Nuevo diálogo de entrenamiento).
2. En el panel del chat, donde se indica "Type your message..." (Escriba su mensaje…), escriba "Hola". 
    - Esto simula la parte del usuario de la conversación.
3. Haga clic en el botón "Score Actions" (Puntuar acciones).
4. Seleccione la respuesta, "¿Qué pizza quiere?".
5. Como usuario, responda "Margarita".
6. Haga clic en el botón "Score Actions" (Puntuar acciones).
7. Seleccione la respuesta, "¡Marchando una pizza!".
8. Haga clic en el botón "Save" (Guardar).

### <a name="create-a-non-wait-action-while-training"></a>Creación de una acción de no espera durante el entrenamiento
Aunque puede crear la acción de no espera tal como lo hizo anteriormente, también puede crearla desde dentro de una sesión de entrenamiento.
1. Haga clic en el botón "New Train Dialog" (Nuevo diálogo de entrenamiento).
2. Como usuario, escriba "Hola".
3. Haga clic en el botón "Score Actions" (Puntuar acciones).
4. Haga clic en el botón "+Action" (+Acción). 
    - Esto le llevará al cuadro de diálogo conocido "Create an Action" (Crear una acción).
5. Escriba lo siguiente como respuesta del bot: "¡Bienvenido a Pizza Bot!"
6. Desactive la casilla "Wait for Response" (Esperar respuesta).
7. Haga clic en el botón "Create" (Crear).
    - Observe que el bot responde inmediatamente con "¡Bienvenido a Pizza Bot!" y que se le vuelve a solicitar otra respuesta del bot. Esto es porque la respuesta del bot era la acción de no espera que acabamos de crear.
9. Seleccione la respuesta, "¿Qué pizza quiere?".
10. Como usuario, responda "Margarita".
11. Haga clic en el botón "Score Actions" (Puntuar acciones).
12. Seleccione la respuesta, "¡Marchando una pizza!".
13. Haga clic en el botón "Save" (Guardar).

> [!NOTE]
> La secuencia de las respuestas de bot con respecto a las acciones de espera y de no espera.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Introducción a las entidades](./04-introduction-to-entities.md)
