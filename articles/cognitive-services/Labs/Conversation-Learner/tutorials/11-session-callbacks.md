---
title: 'Uso de devoluciones de llamada de sesión con un modelo de Conversation Learner: Microsoft Cognitive Services | Microsoft Docs'
titleSuffix: Azure
description: Obtenga información sobre cómo usar devoluciones de llamada de sesión con un modelo de Conversation Learner.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 73fb2a3f10476550bbe6bd20c387a1cc2a7727d8
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/07/2018
ms.locfileid: "51246487"
---
# <a name="how-to-use-session-callbacks-with-a-conversation-learner-model"></a>Uso de devoluciones de llamada de sesión con un modelo de Conversation Learner

En este tutorial se ilustran las devoluciones de llamada onSessionStart y onSessionEnd.

## <a name="video"></a>Vídeo

[![Vista previa del tutorial 11](https://aka.ms/cl-tutorial-11-preview)](https://aka.ms/blis-tutorial-11)

## <a name="requirements"></a>Requisitos
Para poder realizar este tutorial debe ejecutar el bot `tutorialSessionCallbacks`.

    npm run tutorial-session-callbacks

## <a name="details"></a>Detalles
En este tutorial se explica el concepto de una sesión, cómo se controlan las sesiones de forma predeterminada y cómo se puede invalidar este comportamiento.

Una sesión es una conversación con el bot. Puede tener varios turnos, pero no hay interrupciones largas durante la conversación (por ejemplo, 30 minutos).  Vea la página de ayuda sobre los "Límites" para consultar la duración del tiempo de espera de la sesión predeterminada.

Si hay interrupciones largas, el bot pasará a la sesión siguiente.  Si inicia una sesión nueva, la red neural recurrente vuelve a su estado inicial.  De forma predeterminada, se borran también todos los valores de entidad, aunque este comportamiento se puede cambiar (se explica a continuación).

### <a name="open-the-demo"></a>Abrir la demostración

En la lista de modelos, haga clic en Tutorial-11-SessionCallbacks. 

### <a name="entities"></a>Entidades

Hay cuatro entidades definidas en el modelo.

![](../media/tutorial11_entities.PNG)

Hay que destacar que BotName es una entidad de programación.  Esta entidad la definirá el bot en el momento de iniciar la sesión.

### <a name="actions"></a>Acciones

Hay cuatro acciones definidas en el modelo.

![](../media/tutorial11_actions.PNG)

En primer lugar, en este tutorial se mostrará cómo controlar los valores de entidades al inicio de la sesión; por ejemplo, configurar la entidad BotName antes de que el usuario diga algo.

En segundo lugar, en este tutorial se mostrará cómo conservar los valores de una sesión para la siguiente.  En este tutorial, se supone que el nombre y el número de teléfono del usuario siguen siendo los mismos de una sesión a la siguiente, pero que su ubicación puede cambiar.  Por tanto, persistimos el nombre y el número de teléfono entre sesiones, pero se borra la ubicación del usuario.

### <a name="train-dialog"></a>Diálogo de entrenamiento

Este es un diálogo de ejemplo. Se trata de una sesión, es decir, no hay interrupciones largas en este diálogo.

![](../media/tutorial11_traindialog.PNG)

### <a name="code-for-the-callbacks"></a>Código para las devoluciones de llamada

El código para los métodos de devolución de llamada está en el archivo: c:\<installedpath>\src\demos\tutorialSessionCallbacks.ts.

![](../media/tutorial11_code.PNG)

Ambos métodos son opcionales.

- OnSessionStartCallback: este método define la entidad BotName.
- OnSessionEndCallback: puede especificar lo que desea conservar. Borrará todas las entidades, excepto el nombre y el teléfono del usuario.

### <a name="try-the-bot"></a>Probar el bot

Cambie a la interfaz de usuario web y haga clic en los diálogos de registro.

1. Escriba "hello" (Hola).
2. Sistema: "Hi, I'm Botty. What's your name?" (Hola, soy Botty. ¿Cómo se llama?), que adopta el nombre Botty de OnSessionStartCallback.
3. Escriba "jason".
4. Sistema: "Hi jason. What's your phone number?" (Hola, Jason. ¿Cuál es su número de teléfono?).
5. Escriba "555-555-5555".
6. Sistema: "Can you tell Botty your location, jason?" (Jason, ¿puede indicar a Botty cuál es su ubicación?).
7. Escriba "Redmond".

Esta es una sesión. Para iniciar una nueva sesión, primero tenemos que finalizar esta. 

1. Haga clic en Session Timeout (Tiempo de espera de sesión). De esta forma, pasará a la siguiente sesión.
    - El botón "Session Timeout" (Tipo de espera de sesión) se proporciona para fines de depuración.  En una sesión real, necesitaría que se produjera una pausa larga de 30 minutos aproximadamente.  Vea la página de ayuda sobre los "Límites" para consultar la duración del tiempo de espera de la sesión predeterminada.
1. Escriba "hi" (hola).
2. Sistema: "Can you tell Botty your location, jason?" (Jason, ¿puede indicar a Botty cuál es su ubicación?).
    - El sistema recuerda el nombre y número de teléfono.
2. Escriba una nueva ubicación: "Seattle".
3. Sistema: "So, jason you are in Seattle" (Entonces, Jason, está en Seattle).
4. Haga clic en Done Testing (Prueba completada).

Vamos a cambiar a los diálogos de registro. Tenga en cuenta que la última conversación se ha dividido en dos porque cada diálogo de registro se corresponde con una sesión.  

![](../media/tutorial11_splitdialogs.PNG)

- En la primera interacción, se define Botty, pero no el nombre ni el número de teléfono.
- La segunda interacción muestra el nombre y número de teléfono.

Ya ha visto cómo se administran las sesiones de forma predeterminada y cómo puede invalidar el comportamiento predeterminado. 

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Llamadas API](./12-api-calls.md)
