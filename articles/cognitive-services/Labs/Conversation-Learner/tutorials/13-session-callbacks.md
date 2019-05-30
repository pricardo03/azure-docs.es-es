---
title: 'Uso de devoluciones de llamada de sesión con un modelo de Conversation Learner: Microsoft Cognitive Services | Microsoft Docs'
titleSuffix: Azure
description: Obtenga información sobre cómo usar devoluciones de llamada de sesión con un modelo de Conversation Learner.
services: cognitive-services
author: nitinme
manager: nolachar
ms.service: cognitive-services
ms.subservice: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: nitinme
ms.openlocfilehash: eeed0718a1feb170dbbaa783ec0a840c7829c02e
ms.sourcegitcommit: 3d4121badd265e99d1177a7c78edfa55ed7a9626
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/30/2019
ms.locfileid: "66390011"
---
# <a name="how-to-use-session-callbacks-with-a-conversation-learner-model"></a>Uso de devoluciones de llamada de sesión con un modelo de Conversation Learner

En este tutorial, se proporciona información sobre las sesiones y cómo se controlan, y sobre las devoluciones de llamada onSessionStart y onSessionEnd de Conversation Learner.

## <a name="video"></a>Vídeo

[![Vista previa del tutorial devoluciones de llamada de sesión](https://aka.ms/cl_Tutorial_v3_SessionCallbacks_Preview)](https://aka.ms/cl_Tutorial_v3_SessionCallbacks)

## <a name="requirements"></a>Requisitos
Para usar este tutorial, es necesario ejecutar el bot “tutorialSessionCallbacks”.

    npm run tutorial-session-callbacks

## <a name="details"></a>Detalles
En este tutorial se explica el concepto de una sesión, cómo se controlan las sesiones de forma predeterminada y cómo se puede invalidar este comportamiento.

En Conversation Learner, una sesión representa un intercambio interactivo ininterrumpido con el bot. Las sesiones pueden tener varios turnos, pero se pueden finalizar mediante programación debido a inactividad si transcurren más de 30 minutos.  Para obtener información sobre cómo cambiar esta duración predeterminada del tiempo de espera de la sesión, vea la página de ayuda sobre “Límites”.

Este largo período de inactividad causará que el bot cree una sesión y restablezca la red neuronal recurrente a su estado inicial. De forma predeterminada, se borrarán todos los valores de entidad. Este comportamiento predeterminado de borrar los valores de entidad se puede cambiar, como se muestra a continuación.

### <a name="load-the-demo-model"></a>Cargar el modelo de demostración

En la interfaz de usuario web, haga clic en “Import Tutorials” (Importar tutoriales) y seleccione el modelo denominado “Tutorial-13-SessionCallbacks”.

### <a name="code-for-the-callbacks"></a>Código para las devoluciones de llamada

El código de ejemplo para las dos devoluciones de llamada de este modelo está disponible en: `c:\<installedpath>\src\demos\tutorialSessionCallbacks.ts`.

![](../media/tutorial11_code.PNG)

- OnSessionStartCallback: este método define la entidad BotName.
- OnSessionEndCallback: puede especificar lo que desea conservar. Borrará todas las entidades, excepto el nombre y el teléfono del usuario.

Cada devolución de llamada es opcional.

### <a name="actions"></a>Acciones

Hay cuatro acciones definidas en el modelo. Las acciones existentes se muestran en la vista de cuadrícula de “Acciones”.

![](../media/tutorial11_actions.PNG)

### <a name="creating-an-end-session-action-for-callback-invocation"></a>Crear una acción de sesión de finalización (para la invocación devolución de llamada)

1. En el panel izquierdo, haga clic en “Actions” (Acciones) y, después, en el botón “New Action” (Nueva acción).
2. Seleccione “ENDSESSION” como el “Entity Type” (Tipo de entidad).
3. En el campo “Data…” (Datos), escriba “Done” (Listo).
4. Haga clic en el botón “Crear”.

### <a name="edit-an-existing-action"></a>Editar una acción existente

1. Seleccione la acción “So, $UserName, you are in $UserLocation” ($UserName, está en $UserLocation) en la vista de cuadrícula.
2. Desactive la casilla “Wait for Response” (Esperar respuesta).
3. Haga clic en el botón “Save” (Guardar).

### <a name="chaining-actions"></a>Acciones encadenadas

1. En el panel izquierdo, haga clic en “Train Dialogs” (Diálogos de entrenamiento) y, después, en el botón “New Train Dialog” (Nuevo diálogo de entrenamiento).
2. En el panel del chat, donde se indica "Type your message..." (Escriba su mensaje…), escriba "hola".
3. Haga clic en el botón “Score Actions” (Puntuar acciones).
4. Seleccione la respuesta “Hi, I'm Botty. What's your name?” (Hola, soy Botty. ¿Cómo se llama?).
5. En el panel del chat, donde dice “Type your message…” (Escriba su mensaje…), escriba “Lars”.
6. Seleccione la respuesta “Hola, Lars. ¿Cuál es su número de teléfono?”.
7. En el panel del chat, donde dice “Type your message…” (Escriba su mensaje…), escriba “555-555-5555”.
8. Haga clic en el botón “Score Actions” (Puntuar acciones).
9. Seleccione la respuesta “Can you tell Botty your location, Lars?” (¿Puede decirle a Botty su ubicación, Lars?).
10. En el panel del chat, donde dice “Type your message…” (Escriba su mensaje…), escriba “Seattle”.
11. Haga clic en el botón “Score Actions” (Puntuar acciones).
12. Seleccione la respuesta “So, Lars, you are in Seattle” (Lars, se encuentra en Seattle).
13. Seleccione la respuesta “Done” (Listo).
14. Haga clic en el botón “Save” (Guardar).

### <a name="testing-the-model"></a>Probar el modelo

1. En el panel izquierdo, haga clic en “Log Dialogs” (Diálogos de registro) y, después, en el botón “New Log Dialog” (Nuevo diálogo de registro).
2. En el panel del chat, donde dice “Type your message…” (Escriba su mensaje…), escriba “hi” (hola).
3. En el panel del chat, donde dice “Type your message…” (Escriba su mensaje…), escriba “Lars”.
4. En el panel del chat, donde dice “Type your message…” (Escriba su mensaje…), escriba “555-555-5555”.
5. En el panel del chat, donde dice “Type your message…” (Escriba su mensaje…), escriba “Seattle”.
    - En este momento, se habrán conservado todos los valores de entidad, excepto la ubicación.
6. En el panel del chat, donde dice “Type your message…” (Escriba su mensaje…), escriba “hello” (hola).
7. En el panel del chat, donde dice “Type your message…” (Escriba su mensaje…), escriba “Detroit”.
8. Haga clic en el botón “Session Timeout” (Tiempo de espera de la sesión).
    - Al hacer clic en este botón, se ejecutará la respuesta del bot para períodos largos de inactividad.
9. Haga clic en el botón “OK” (Aceptar).
10. Haga clic en el botón “Done Testing” (Pruebas finalizadas).

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Llamadas API](./14-api-calls.md)
