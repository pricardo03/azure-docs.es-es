---
title: 'Modelo de Conversation Learner de demostración, restablecimiento de contraseña: Microsoft Cognitive Services | Microsoft Docs'
titleSuffix: Azure
description: Obtenga información acerca de cómo crear un modelo de Conversation Learner de demostración.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: bd0bcd79bb21dc3973b34086f6dad21b47a95c2f
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/07/2018
ms.locfileid: "51240875"
---
# <a name="demo-password-reset"></a>Demostración: restablecimiento de contraseña
En esta demostración se muestra un bot de soporte técnico sencillo que puede facilitar los restablecimientos de contraseña. 

Muestra cómo Conversation Learner puede aprender flujos de diálogos no triviales y secuencias de varios turnos, incluida una clase fuera de dominio. Esta demostración no usa ningún código ni entidades.

## <a name="video"></a>Vídeo

[![Vista previa de la demostración sobre contraseñas](https://aka.ms/cl-demo-password-preview)](https://aka.ms/blis-demo-password)

## <a name="requirements"></a>Requisitos
Para poder utilizar este tutorial, debe ejecutar el bot de restablecimiento de contraseña.

    npm run demo-password

### <a name="open-the-demo"></a>Abrir la demostración

En la lista de modelos de la interfaz de usuario web, haga clic en Tutorial Demo Password Reset. 

### <a name="actions"></a>Acciones

Se ha creado un conjunto de acciones en que el usuario solicita ayuda con su contraseña, incluidas las soluciones.

![](../media/tutorial_pw_reset_actions.PNG)

### <a name="training-dialogs"></a>Diálogos de entrenamiento

Hay una serie de diálogo de entrenamiento. También existen demostraciones de una clase fuera de dominio; por ejemplo, las solicitudes de usuario como "itinerarios" están fuera de dominio; el bot ha dado ejemplos de algunas solicitudes fuera de dominio y puede responder con "No puedo ayudarle".

![](../media/tutorial_pw_reset_entities.PNG)

Por ejemplo, vamos a probar una sesión de instrucción.

1. Haga clic en Train Dialogs (Diálogos de entrenamiento) y, a continuación, en New Train Dialog (Nuevo diálogo de entrenamiento).
1. Escriba "He perdido mi contraseña".
2. Haga clic en Score Action (Acción de puntuación).
3. Haga clic para seleccionar "Is that for your local account or Microsoft account?" (¿Es para su cuenta local o para la cuenta de Microsoft?).
4. Escriba "Local account" (Cuenta local).
5. Haga clic en Score Actions (Acciones de puntuación).
3. Haga clic para seleccionar "Which version of Windows do you have?" (¿Qué versión de Windows tiene?).
4. Escriba "Windows 8".
5. Haga clic en Score Actions (Acciones de puntuación).
6. Seleccione "SOLUTION: how to reset password on Windows 8" (SOLUCIÓN: cómo restablecer la contraseña en Windows 8).
4. Haga clic en Done Teaching (Aprendizaje completado).

Vamos a probar cómo el bot puede aprender una clase fuera de dominio.

1. Haga clic en Train Dialogs (Diálogos de entrenamiento) y, a continuación, en New Train Dialog (Nuevo diálogo de entrenamiento).
1. Escriba "web search" (búsqueda web).
    - Este es un ejemplo de clase fuera de dominio. 
2. Haga clic en Score Action (Acción de puntuación).
3. Haga clic para seleccionar "Sorry, I can't help with that" (Lo sentimos, pero no puedo ayudarlo con eso).
    - Tenga en cuenta que la puntuación de esta opción es baja actualmente. Pero después de algo más de entrenamiento, la puntuación será mayor.
4. Haga clic en Done Teaching (Aprendizaje completado).

Ya ha visto cómo crear una demostración básica de soporte técnico y cómo puede aprender a ofrecer soluciones y también a administrar las consultas de ejemplo.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Demostración: orden de pizzas](./demo-pizza-order.md)
