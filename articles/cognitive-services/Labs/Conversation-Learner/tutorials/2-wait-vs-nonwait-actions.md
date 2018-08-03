---
title: 'Uso de acciones de espera y de no espera con un modelo de Conversation Learner: Microsoft Cognitive Services | Microsoft Docs'
titleSuffix: Azure
description: Obtenga información sobre cómo usar las acciones de espera y de no espera con un modelo de Conversation Learner.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: a8f7ccf79e750c9f3c21c25c50c3e275db7e4195
ms.sourcegitcommit: 4e5ac8a7fc5c17af68372f4597573210867d05df
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/20/2018
ms.locfileid: "39173091"
---
# <a name="wait-and-non-wait-actions"></a>Acciones de espera y acciones de no espera

En este tutorial se muestra la diferencia entre las acciones de espera y las acciones de no espera en Conversation Learner.

## <a name="video"></a>Vídeo

[![Vista previa del tutorial 2](http://aka.ms/cl-tutorial-02-preview)](http://aka.ms/blis-tutorial-02)

## <a name="requirements"></a>Requisitos
Para poder realizar este tutorial debe ejecutar el bot del tutorial general

    npm run tutorial-general

## <a name="details"></a>Detalles

- Acción de espera: después de que el sistema realiza una acción de "espera", dejará de realizar acciones y esperará la entrada del usuario.
- Acción de no espera: después de que el sistema realiza una acción de "no espera", elegirá de inmediato otra acción (sin esperar la entrada por parte del usuario).

## <a name="steps"></a>Pasos

### <a name="create-a-new-model"></a>Creación de un modelo nuevo

1. En la interfaz de usuario web, haga clic en New Model (Modelo nuevo).
2. En Name (Nombre), escriba WaitNonWait. A continuación, haga clic en Crear.

### <a name="create-the-first-wait-action"></a>Creación de la primera acción de espera

1. Haga clic en Actions (Acciones) y, después, en New Action (Nueva acción).
2. En Response (Respuesta), escriba "Which animal do you want" (¿Qué animal desea?).
    - Esta es una acción de espera, así que deje activada la casilla Wait for Response (Esperar respuesta).
3. Haga clic en Crear.

### <a name="create-a-non-wait-action"></a>Creación de una acción de no espera

1. Haga clic en New Action (Acción nueva)
2. En Response, escriba "Cows say moo" (La vaca hace mú).
3. Desactive la casilla Wait for Response (Esperar respuesta).
4. Click Create

### <a name="create-a-second-non-wait-action"></a>Creación de una segunda acción no espera

1. Haga clic en New Action (Acción nueva)
2. En respuesta, escriba "Ducks say quack" (El pato dice cuac).
3. Desactive la casilla Wait for Response (Esperar respuesta).
4. Click Create

![](../media/tutorial2_actions.PNG)

### <a name="train-the-bot"></a>Entrenamiento del bot

1. Haga clic en Train Dialogs (Diálogos de entrenamiento) y, a continuación, en New Train Dialog (Nuevo diálogo de entrenamiento).
2. Escriba "hello" (hola).
3. Haga clic en Score Actions (Acciones de puntuación) y seleccione "Which animal do you want?" (¿Qué animal desea?).
4. Escriba "cow" (vaca).
5. Haga clic en Score Actions (Acciones de puntuación) y seleccione "Cows say moo" (La vaca hace mú).
    - El bot no espera ninguna entrada y llevará a cabo la acción siguiente.
2. Seleccione "Which animal do you want?" (¿Qué animal desea?).
3. Escriba "duck" (pato).
5. Haga clic en Score Actions (Acciones de puntuación) y seleccione "Ducks say quack" (¿El pato hace cuac?).

![](../media/tutorial2_dialogs.PNG)

> [!NOTE]
> La secuencia de las respuestas de bot con respecto a las acciones de espera y de no espera.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Introducción a las entidades](./3-introduction-to-entities.md)
