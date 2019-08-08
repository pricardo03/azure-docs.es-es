---
title: 'Cómo usar tarjetas con una aplicación Conversation Learner, parte 1: Microsoft Cognitive Services | Microsoft Docs'
titleSuffix: Azure
description: Obtenga información sobre cómo usar tarjetas con una modelo de Conversation Learner.
services: cognitive-services
author: nitinme
manager: nolachar
ms.service: cognitive-services
ms.subservice: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: nitinme
ROBOTS: NOINDEX
ms.openlocfilehash: 426d7c8de29abeb88833e94962a7291a641702ac
ms.sourcegitcommit: ad9120a73d5072aac478f33b4dad47bf63aa1aaa
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/01/2019
ms.locfileid: "68703743"
---
# <a name="how-to-use-cards-part-1-of-2"></a>Cómo usar tarjetas (parte 1 de 2)

En este tutorial se muestra cómo agregar y usar una tarjeta simple en el bot.

> [!NOTE]
> Actualmente, Conversation Learner espera que los archivos de definición de tarjetas se encuentren en un directorio denominado "cards" que está en el directorio donde se ha iniciado el bot.

## <a name="video"></a>Vídeo

[![Vista previa del tutorial de tarjetas](https://aka.ms/cl_Tutorial_v3_Cards_Preview)](https://aka.ms/cl_Tutorial_v3_Cards)

## <a name="requirements"></a>Requisitos
Para poder realizar este tutorial debe ejecutar el bot del tutorial general

    npm run tutorial-general

## <a name="details"></a>Detalles

Las tarjetas son elementos de interfaz de usuario que permiten al usuario seleccionar una opción en la conversación. 

### <a name="open-the-demo"></a>Abrir la demostración

En la interfaz de usuario web, haga clic en "Import Tutorials" (Importar tutoriales) y seleccione el modelo denominado "Tutorial-15-Cards".

### <a name="the-card"></a>Tarjeta

La definición de la tarjeta está en la siguiente ubicación: C:\<rutainstalación\>\src\cards\prompt.json.

El sistema espera encontrar las definiciones de tarjeta en este directorio "cards".

![](../media/tutorial13_prompt.PNG)

> [!NOTE]
> Fíjese en el tipo de cuerpo "TextBlock" y el marcador de posición "{{question}}" en el campo de texto.
> Existen dos botones de envío y el texto que se envía para cada uno.

### <a name="actions"></a>Acciones

Se han creado tres acciones. Como se ve a continuación, la primera acción es una tarjeta.

![](../media/tutorial13_actions.PNG)

Veamos cómo se ha creado el tipo de acción de tarjeta:

![](../media/tutorial13_cardaction.PNG)

> [!NOTE]
> La tarjeta contiene tres parámetros diferentes: la entrada de pregunta y los botones 1 y 2. Estos elementos son referencias de plantilla de la tarjeta donde se escriben la pregunta y las respuestas correspondientes. También puede hacer referencia a entidades y usarlas, o bien una mezcla de texto y entidades.

El icono de ojo muestra el aspecto de la tarjeta.

### <a name="practicing-creating-card-actions"></a>Práctica de creación de las acciones de tarjeta

1. En el panel izquierdo, haga clic en "Actions" (Acciones) y, posteriormente, en el botón "New Action" (Nueva acción).
2. Seleccione "CARD" (TARJETA) en "Action Type" (Tipo de acción).
3. Seleccione "prompt" (aviso) en la lista "Template" (Plantilla).
4. En el campo "question" (pregunta), escriba "¿Ir a la izquierda o derecha?".
5. En el campo "button1" (botón1), escriba "izquierda".
6. En el campo "button2" (botón2), escriba "derecha".
7. Haga clic en el botón "Cancel" (Cancelar).

### <a name="train-dialog-using-an-adaptive-card"></a>Entrenamiento del diálogo con una tarjeta adaptable

1. En el panel izquierdo, haga clic en "Train Dialogs" (Diálogos de entrenamiento) y, después, en el botón "New Train Dialog" (Nuevo diálogo de entrenamiento).
2. En el panel del chat, donde se indica "Type your message..." (Escriba su mensaje…), escriba "hola".
3. Haga clic en el botón "Score Actions" (Acciones de puntuación).
4. Seleccione la respuesta, "prompt: question: ¿Ir a la izquierda o derecha?".
    - Se puede usar el icono de ojo para obtener una vista previa de la tarjeta.
5. En el panel del chat, haga clic en el botón "Izquierda" en el aviso representado.
6. Haga clic en el botón "Score Actions" (Acciones de puntuación).
7. Seleccione la respuesta "Izquierda".
8. Haga clic en el botón "Save" (Guardar).
9. Seleccione la respuesta, "prompt: question: ¿Ir a la izquierda o derecha?".
10. En el panel del chat, haga clic en el botón "Derecha" en el aviso representado.
11. Haga clic en el botón "Score Actions" (Acciones de puntuación).
12. Seleccione la respuesta "Derecha".

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Bots híbridos](./16-hybrid-bots.md)
