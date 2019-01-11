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
ms.openlocfilehash: c08e3d2e8f712f5eb7c56585507a283c7fd040c1
ms.sourcegitcommit: 295babdcfe86b7a3074fd5b65350c8c11a49f2f1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/27/2018
ms.locfileid: "53796684"
---
# <a name="how-to-create-a-hello-world-model-with-conversation-learner"></a>Creación de un modelo "Hola mundo" de Conversation Learner

En este tutorial se muestra cómo empezar a usar Conversation Learner, lo que incluye crear acciones, enseñar al bot de forma interactiva y realizar correcciones de diálogos registrados que provienen de los usuarios finales.

## <a name="video"></a>Vídeo

[![Vista previa del tutorial de Hola mundo](https://aka.ms/cl_Tutorial_v3_HelloWorld_Preview)](https://aka.ms/cl_tutorial_v3_helloworld)


## <a name="requirements"></a>Requisitos
Si no lo ha hecho aún, en primer lugar asegúrese de que se han completado todos los pasos de la instalación, incluida la creación de un archivo `.env` con clave de creación de LUIS.  Para más información, consulte la [guía de inicio rápido](../quickstart.md).

Para poder realizar este tutorial, el bot de tutorial general debe estar en ejecución.

    npm run tutorial-general

## <a name="steps"></a>Pasos

Empiece en la página principal de la interfaz de usuario web.

### <a name="create-the-model"></a>Creación del modelo
1. Haga clic en el botón "New Model" (Nuevo modelo).
2. En el campo "Name" (Nombre), escriba "Hola mundo".
3. Haga clic en el botón "Create" (Crear).

Ahora debería ver la vista del modelo que ha creado.

### <a name="create-an-action"></a>Creación de una acción
1. En el panel izquierdo, haga clic en "Actions" (Acciones) y, posteriormente, en el botón "New Action" (Nueva acción).
    - Una acción puede ser un mensaje de texto que Conversation Learner devuelve al usuario, una llamada a una API o una tarjeta.
2. En el campo "Bot's response..." (Respuesta del bot…), escriba "Hola".
    - Esta es la respuesta que devolverá el bot.
3. Haga clic en el botón "Create" (Crear).

Ha creado la primera acción que puede llevar a cabo el bot, es decir, devolver una respuesta de texto.

### <a name="train-dialogs"></a>Diálogos de entrenamiento
Aquí es donde entrena el modelo sobre cómo responder a las expresiones del usuario.

#### <a name="first-training-dialog"></a>Primer diálogo de entrenamiento

1. En el panel izquierdo, haga clic en "Train Dialogs" (Diálogos de entrenamiento) y, después, en el botón "New Train Dialog" (Nuevo diálogo de entrenamiento).
2. Escriba "Hola" y presione Entrar.
    - A modo de ejemplo de lo que podría decir el usuario al principio de una conversación.
3. Haga clic en el botón "Score Actions" (Acciones de puntuación).
4. Seleccione "Hola".
    - Acaba de completar un turno completo en este diálogo de ejemplo. 
5. Escriba la respuesta del usuario: "Adiós".
6. Haga clic en el botón "Score Actions" (Acciones de puntuación).
7. Haga clic en el botón "+ Action" (+ Acción).
8. Escriba "¡Adiós!" en el campo "Bot's response..." (Respuesta del bot...) y después haga clic en el botón "Create" (Crear).
    - Fíjese en que el bot ha respondido con la acción que acaba de crear.
9. Haga clic en el botón "Save" (Guardar). 
    - Así finaliza y se guarda este diálogo de entrenamiento.

Ahora tiene un diálogo de entrenamiento en el modelo, junto con una única entidad y dos acciones.

#### <a name="second-training-dialog"></a>Segundo diálogo de entrenamiento
Realizaremos un entrenamiento más y veremos cómo responde el bot.

1. Haga clic en el botón "New Train Dialog" (Nuevo diálogo de entrenamiento).
2. Escriba "Hola".
    - Este diálogo es similar al primero, por lo que esperamos obtener una buena puntuación del bot.
3. Haga clic en el botón "Score Actions" (Acciones de puntuación).
    - Es posible que tanto la posición como la puntuación no sean suficientemente precisas y que requieran más entrenamiento.
4. Seleccione "Hola".
5. Escriba la respuesta del usuario: "Chao".
6. Haga clic en el botón "Score Actions" (Acciones de puntuación).
7. Seleccione "¡Adiós!".
8. Haga clic en el botón "Save" (Guardar).

### <a name="log-dialogs"></a>Diálogos de registro
Aquí es donde prueba, ve y corrige las conversaciones correctas que usted o los usuarios reales han tenido con el bot.

#### <a name="test-the-model-as-an-end-user"></a>Prueba del modelo como usuario final
1. En el panel izquierdo, haga clic en "Log Dialogs" (Diálogos de registro) y, después, en el botón "New Log Dialog" (Nuevo diálogo de registro).
2. Escriba "hola a todo el mundo".
3. Espere un momento, el bot debería responder automáticamente con "Hola".
4. Escriba "Hasta luego".
5. Espere un momento, de nuevo, el bot debería responder automáticamente con "Hola".
6. Haga clic en el botón "Done Testing" (Finalizar la prueba).

#### <a name="view-and-correct-a-user-conversation"></a>Consulta y corrección de una conversación con un usuario
Con los diálogos de registro, puede ver la lista de las conversaciones que mantienen los usuarios con el bot. También puede editarlas para corregir las respuestas del bot y guardar las interacciones como diálogos de entrenamiento. Para ello:
1. En la cuadrícula, haga clic en el registro de la conversación.
2. Haga clic en la última acción del bot (por ejemplo, "Hola").
3. Seleccione "¡Adiós!". para corregir el bot.
4. Haga clic en el botón "Save As Train Dialog" (Guardar como diálogo de entrenamiento).

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Introducción al entrenamiento](./02-intro-to-training.md)
