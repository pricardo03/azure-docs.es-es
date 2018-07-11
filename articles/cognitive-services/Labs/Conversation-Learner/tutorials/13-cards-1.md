---
title: 'Cómo usar tarjetas con una aplicación Conversation Learner, parte 1: Microsoft Cognitive Services | Microsoft Docs'
titleSuffix: Azure
description: Obtenga información acerca de cómo usar tarjetas con una aplicación Conversation Learner.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: e90ccd42b21eea6139c402937be7e20513d73c84
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/23/2018
ms.locfileid: "35381746"
---
# <a name="how-to-use-cards-part-1-of-2"></a>Cómo usar tarjetas (parte 1 de 2)

En este tutorial se muestra cómo agregar y usar una tarjeta simple en el bot.

Observe que Conversation Learner espera que los archivos de definición de tarjetas se encuentren en un directorio denominado "cards" que se encuentre en el directorio donde se ha iniciado el bot.

## <a name="requirements"></a>Requisitos
Para poder realizar este tutorial debe ejecutar el bot de tutorial general.

    npm run tutorial-general

## <a name="details"></a>Detalles

Las tarjetas son elementos de interfaz de usuario que permiten al usuario seleccionar una opción en la conversación. 

### <a name="open-the-demo"></a>Abrir la demostración

En la lista de aplicaciones de la interfaz de usuario web, haga clic en Tutorial-13-Cards-1. 

### <a name="the-card"></a>Tarjeta

La definición de tarjeta está en la siguiente ubicación: C:\<installedpath\>\src\cards\prompt.json.

El sistema espera encontrar las definiciones de tarjeta en este directorio cards.

![](../media/tutorial13_prompt.PNG)

- Observe la clase TextBlock y la plantilla de pregunta.
- Existen dos botones de envío y el texto que se envía para cada uno.

### <a name="actions"></a>Acciones

Hemos creado tres acciones. Como se ve a continuación, la primera acción es una tarjeta.

![](../media/tutorial13_actions.PNG)

Veamos cómo se creó el tipo de acción de la tarjeta:

![](../media/tutorial13_cardaction.PNG)

Observe la entrada de la pregunta, y los botones 1 y 2. Son referencias de plantilla de la tarjeta donde se escriben la pregunta y las respuestas correspondientes. También puede consultar y usar entidades, o bien una mezcla de texto y entidades.

El icono de ojo muestra el aspecto de la tarjeta.

### <a name="train-dialog"></a>Cuadro de diálogo de entrenamiento

Veamos un cuadro de diálogo de instrucción.

1. Haga clic en Train Dialogs (Diálogos de entrenamiento) y, a continuación, en New Train Dialog (Nuevo diálogo de entrenamiento).
1. Escriba "hi" (hola).
2. Haga clic en Score Action (Acción de puntuación).
3. Haga clic para seleccionar "Prompt go left or right" (Preguntar si ir a izquierda o derecha).
    - Hacer clic en "left" (izquierda) o "right" (derecha) equivale a la acción del usuario de escribir "left" o "right", respectivamente. 
4. Haga clic en Score Actions (Acciones de puntuación).
4. Haga clic para seleccionar "left" (izquierda). Esta es una acción de no espera.
6. Haga clic para seleccionar "Prompt go left or right" (Preguntar si ir a izquierda o derecha).
4. Haga clic en "right" (derecha).
5. Haga clic en Score Actions (Acciones de puntuación).
3. Haga clic para seleccionar "Right" (Derecha).
6. Haga clic para seleccionar "Prompt go left or right" (Preguntar si ir a izquierda o derecha).
4. Haga clic en Done Testing (Prueba completada).

Ya ha visto cómo funcionan las tarjetas. Se definen en el directorio cards como plantillas JSON. Las plantillas aparecen en la interfaz de usuario y se pueden rellenar con una cadena o una entidad, o bien con una combinación de ambas.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Tarjetas parte 2](./14-cards-2.md)
