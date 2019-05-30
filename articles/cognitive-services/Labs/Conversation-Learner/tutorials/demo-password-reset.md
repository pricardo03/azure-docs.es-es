---
title: 'Modelo de Conversation Learner de demostración, restablecimiento de contraseña: Microsoft Cognitive Services | Microsoft Docs'
titleSuffix: Azure
description: Obtenga información acerca de cómo crear un modelo de Conversation Learner de demostración.
services: cognitive-services
author: nitinme
manager: nolachar
ms.service: cognitive-services
ms.subservice: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: nitinme
ms.openlocfilehash: 15aa3a8346087908cf77f1f68db916cc2c184448
ms.sourcegitcommit: 3d4121badd265e99d1177a7c78edfa55ed7a9626
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/30/2019
ms.locfileid: "66389875"
---
# <a name="demo-password-reset"></a>Demostración: Restablecimiento de contraseña
En este tutorial se muestra un bot simple de soporte técnico que puede ayudar a restablecer la contraseña, que utiliza la tecnología de Conversation Learner. El modelo de bot puede aprender flujos de diálogos no triviales y secuencias de varios turnos, incluidas clases fuera del dominio. La tarea puede realizarse sin código o entidades.

## <a name="video"></a>Vídeo

[![Vista previa de la demostración sobre contraseñas](https://aka.ms/cl_Tutorial_v3_DemoPassword_Preview)](https://aka.ms/cl_Tutorial_v3_DemoPassword)

## <a name="requirements"></a>Requisitos
Para poder utilizar este tutorial, debe ejecutar el bot de restablecimiento de contraseña.

    npm run demo-password

### <a name="open-the-demo"></a>Abrir la demostración

En la lista de modelos de la interfaz de usuario web, haga clic en Tutorial Demo Password Reset. 

### <a name="actions"></a>Acciones

El modelo contiene un conjunto de acciones diseñadas para ayudar a los usuarios a resolver problemas comunes con la contraseña.

![](../media/tutorial_pw_reset_actions.PNG)

### <a name="training-dialogs"></a>Diálogos de entrenamiento

El modelo también contiene varios diálogos de entrenamiento, incluidos algunos que muestran el entrenamiento de clases fuera del dominio. Por ejemplo, los usuarios pueden solicitar indicaciones de rutas. El bot de ejemplo se ha entrenado con algunos para la demostración y simplemente indicará algo parecido a "no puedo ayudar en esto". La lista de diálogos de entrenamiento existentes se encuentra en "Train Dialogs" (Diálogos de entrenamiento) en el panel izquierdo.

![](../media/tutorial_pw_reset_entities.PNG)

1. En el panel izquierdo, haga clic en "Train Dialogs" (Diálogos de entrenamiento) y, después, en el botón "New Train Dialog" (Nuevo diálogo de entrenamiento).
2. En el panel del chat, donde dice "Type your message..." (Escriba su mensaje…), escriba "he perdido mi contraseña".
3. Haga clic en el botón "Score Actions" (Puntuar acciones).
4. Seleccione la respuesta "Is that for your local account or Microsoft account?" (¿Es para su cuenta local o para la cuenta de Microsoft?).
5. En el panel del chat, donde dice "Type your message..." (Escriba su mensaje…), escriba "cuenta local".
6. Haga clic en el botón "Score Actions" (Puntuar acciones).
7. Seleccione la respuesta, "Which version of Windows do you have?" (¿Qué versión de Windows tiene?).
8. En el panel del chat, donde dice "Type your message..." (Escriba su mensaje…), escriba "windows xp".
9. Haga clic en el botón "Score Actions" (Puntuar acciones).
10. Haga clic en el botón "+Acción".
11. En el campo "Bot's response..." (Respuesta del bot…), escriba "SOLUCIÓN: Cómo restablecer la contraseña en Windows XP".
12. Haga clic en el botón "Crear".

### <a name="training-dialogs-for-out-of-domain-scenarios"></a>Cuadros de diálogo de entrenamiento para escenarios de fuera del dominio

1. En el panel izquierdo, haga clic en "Train Dialogs" (Diálogos de entrenamiento) y, después, en el cuadro de entrenamiento "tiendas de juguetes".
2. En el panel del chat, haga clic en la expresión "tiendas de juguetes".
3. En el campo "Add alternative input..." (Agregar entrada alternativa...), escriba "búsqueda en web" y presione Entrar.
4. En el campo "Add alternative input..." (Agregar entrada alternativa...), escriba "reserva de vuelo" y presione Entrar.
5. Haga clic en el botón "Guardar cambios".
6. Haga clic en el botón "Save Edit" (Guardar edición).
7. En el panel izquierdo, haga clic en "Log Dialogs" (Diálogos de registro) y, después, en el botón "New Train Dialog" (Nuevo diálogo de registro).
8. En el panel de chat, donde dice "Type your message..." (Escriba su mensaje…), escriba "no encuentro mi contraseña".
9. En el panel del chat, donde dice "Type your message..." (Escriba su mensaje…), escriba "cuenta de Microsoft".
10. En el panel del chat, donde dice "Type your message..." (Escriba su mensaje…), escriba "gracias".
11. Haga clic en el botón "Pruebas listas".
12. Haga clic en el cuadro de diálogo de registro de "no encuentro mi contraseña" en la vista de cuadrícula.
13. En el panel del chat, haga clic en la respuesta representada incorrectamente "Solución: Cómo restablecer la contraseña de la cuenta de Microsoft".
14. Haga clic en el botón "+Acción".
15. En el campo "Bot's response..." (Respuesta del bot…), escriba "Gracias"
16. Haga clic en el botón "Crear".
17. Haga clic en el botón "Save As Train Dialog" (Guardar como diálogo de entrenamiento).

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Demostración: orden de pizzas](./demo-pizza-order.md)
