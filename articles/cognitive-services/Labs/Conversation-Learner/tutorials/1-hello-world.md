---
title: 'Creación de un modelo "Hola mundo" de Conversation Learner: Microsoft Cognitive Services | Microsoft Docs'
titleSuffix: Azure
description: Obtenga información sobre cómo crear un modelo "Hola mundo" de Conversation Learner.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 271141f24ff729fc99210af67ad769a5ef83a65c
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/07/2018
ms.locfileid: "51242728"
---
# <a name="how-to-create-a-hello-world-model-with-conversation-learner"></a>Creación de un modelo "Hola mundo" de Conversation Learner

Este tutorial muestra cómo empezar a usar Conversation Learner, lo que incluye la creación de acciones, enseñanza interactiva y realización de correcciones de cuadros de diálogo registrados por parte de los usuarios finales.

## <a name="video"></a>Vídeo

[![Vista previa del tutorial 1](https://aka.ms/cl-tutorial-01-preview)](https://aka.ms/blis-tutorial-01)


## <a name="requirements"></a>Requisitos
Si no lo ha hecho aún, en primer lugar asegúrese de que se han completado todos los pasos de la instalación, incluida la creación de un archivo `.env` con clave de creación de LUIS.  Para más información, consulte la [guía de inicio rápido](https://github.com/Microsoft/ConversationLearner-Samples).

Para poder realizar este tutorial debe ejecutar el bot del tutorial general

    npm run tutorial-general

## <a name="steps"></a>Pasos

Empiece en la página principal de la interfaz de usuario web.

### <a name="create-the-model"></a>Crear el modelo
1. Haga clic en New Modelo (Modelo nuevo).
2. En el campo Name (Nombre), escriba "Hola mundo"
3. Haga clic en Crear.

### <a name="create-an-action"></a>Creación de una acción

1. Haga clic en la el modelo Hola mundo para iniciarlo
2. Haga clic en Actions (Acciones) y, después, en New Action (Nueva acción)
    - Una acción puede ser un mensaje de texto que Conversation Learner devuelve al usuario, una llamada a una API o una tarjeta.
3. En Response (Respuesta), escriba "Hola mundo"
    - Se trata de la respuesta que devolverá el bot
4. Haga clic en Crear.

Ha creado lo primero que el bot puede hacer, es decir, devolver una respuesta de texto.

### <a name="train-the-bot"></a>Entrenamiento del bot

#### <a name="create-the-first-dialog"></a>Creación del primer cuadro de diálogo

1. Haga clic en Train Dialogs (Cuadros de diálogo de entrenamiento) y, después, en New Train Dialog (Nuevo cuadro de diálogo de entrenamiento)
2. Escriba un ejemplo de lo que el usuario dirá al principio de la conversación, por ejemplo, "hola".
3. Haga clic en Score Actions (Acciones de puntuación)
4. Seleccione "Hola mundo"
    - Así se crea un cuadro de diálogo de ejemplo. 
2. Escriba "adiós"
3. Haga clic en Score Actions (Acciones de puntuación)
4. Haga clic en Agregar acción y después escriba "Adiós" En Response (Respuesta), haga clic en "Create" (Crear)
5. Haga clic en Done Teaching (Aprendizaje completado). Así finalizará este cuadro de diálogo de aprendizaje.

Ya tiene un cuadro de diálogo de entrenamiento en el sistema.

#### <a name="continue-teaching-the-bot"></a>Más entrenamiento del bot
Vamos a realizar un entrenamiento más y a ver cómo responde el bot.

1. Haga clic en New Train Dialog (Nuevo cuadro de diálogo de entrenamiento)
2. Escriba "Hola a todos"
    - Este cuadro de diálogo es similar al primero, por lo que esperamos obtener una buena puntuación del bot.
2. Haga clic en Score Action (Acción de puntuación)
    - Es posible que tanto la posición como la puntuación no sean suficientemente precisas y que requieran más entrenamiento.
3. Haga clic en Select (Seleccionar) junto a "Hola mundo"
4. Luego, escriba "adiós"
5. Haga clic en Score Actions (Acciones de puntuación)
6. Seleccione "Adiós"
7. Haga clic en Done Teaching (Aprendizaje completado)

![](../media/tutorial1_actions.PNG)

Realizaremos otra sesión de aprendizaje para ver cómo funciona el bot.

Repita los pasos anteriores con "holita" y "adiosito" y observe los cambios en la posición y puntuación de la respuesta de los bots al hacer clic en Score Actions (Acciones de puntuación).

Ahora puede repetir los pasos con "buenas" y "hasta luego" y observe que la puntuación muestra mejoras, lo que indica que el bot ha aprendido esta interacción.

![](../media/tutorial1_dialogs.PNG)

### <a name="test-the-bot-as-an-end-user"></a>Prueba del bot como usuario final

1. Haga clic Log Dialogs (Cuadros de diálogo de registro) y luego en New Log Dialog (Nuevo cuadro de diálogo de registro)
2. Escriba "hola a todo el mundo"
3. Luego "adiós"

También puede intentar iniciar una conversación con 'adiós' y observar la respuesta del bot.

### <a name="view-conversations-in-the-log-dialogs"></a>Visualización de conversaciones en los cuadros de diálogo de registro

En Log Dialogs (Cuadros de diálogo de registro) puede ver la lista de las conversaciones, así como actualizar y guardar las interacciones como cuadros de diálogo de entrenamiento. Para ello:

1. Haga clic en el registro de una conversación
2. Si la conversación parece correcta, haga clic en la última acción p. ej. "Adiós".
3. Haga clic para seleccionar la respuesta sugerida. 
    - También puede seleccionar o agregar otra acción.
4. Luego, haga clic en done (Listo) listo para guardarlo como cuadro de diálogo de entrenamiento.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Acciones de espera y acciones de no espera](./2-wait-vs-nonwait-actions.md)