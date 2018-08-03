---
title: 'Uso de la devolución de llamada de detección de entidades con un modelo de Conversation Learner: Microsoft Cognitive Services | Microsoft Docs'
titleSuffix: Azure
description: Obtenga información sobre cómo utilizar la devolución de llamada de detección de entidades con un modelo de Conversation Learner.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: f168018a23d03ffb957da2dd1f67881420a21208
ms.sourcegitcommit: 4e5ac8a7fc5c17af68372f4597573210867d05df
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/20/2018
ms.locfileid: "39171110"
---
# <a name="how-to-use-entity-detection-callback"></a>Uso de la devolución de llamada de detección de entidades

En este tutorial se explica la devolución de llamada de detección de entidades y también se ilustra un patrón común para resolver entidades.

## <a name="video"></a>Vídeo

[![Vista previa del tutorial 10](http://aka.ms/cl-tutorial-10-preview)](http://aka.ms/blis-tutorial-10)

## <a name="requirements"></a>Requisitos
Para poder realizar este tutorial debe ejecutar el bot `tutorialEntityDetectionCallback`.

    npm run tutorial-entity-detection

## <a name="details"></a>Detalles
La devolución de llamada de detección de entidades permite utilizar código personalizado para controlar las reglas de negocio relacionadas con entidades. En esta demostración se usan las devoluciones de llamada y las entidades de programación para resolver el nombre de la ciudad escrito por el usuario como un nombre canónico; por ejemplo, resolver "La Gran Manzana" para "Nueva York".

### <a name="open-the-demo"></a>Abrir la demostración

En la lista de modelos, haga clic en Tutorial-10-EntityDetectionCallback. 

### <a name="entities"></a>Entidades

Hay tres entidades definidas en el modelo.

![](../media/tutorial10_entities.PNG)

1. La ciudad es una entidad personalizada que proporcionará el usuario como entrada de texto.
2. CityUnknown es una entidad de programación. El sistema rellenará esta entidad. Copiará la entrada de usuario si el sistema no sabe de qué ciudad se trata.
3. CityResolved es la ciudad que el sistema no conoce. Esta entidad será el nombre canónico de la ciudad; por ejemplo, "La Gran Manzana" se resolverá como "Nueva York".

### <a name="actions"></a>Acciones

Hay tres acciones definidas en el modelo.

![](../media/tutorial10_actions.PNG)

1. La primera acción es "Which city do you want?" (¿Qué ciudad desea?).
2. La segunda es "I don't know this city, $CityUknown. Which city do you want?" (No conozco esta ciudad, $CityUknown. ¿Qué ciudad desea?).
3. La tercera es "You said $City, and I resolved that to $CityResolved" (Ha dicho $City, y la he resuelto como $CityResolved).

### <a name="callback-code"></a>Código de devolución de llamada

Echemos un vistazo al código. Puede encontrar el método EntityDetectionCallback en el archivo C:\<installedpath>\src\demos\tutorialSessionCallbacks.ts.

![](../media/tutorial10_callbackcode.PNG)

Se llama a esta función una vez resuelta la entidad.
 
- Lo primero que se hará será borrar $CityUknown. $CityUknown solo persistirá una sola vez, ya que siempre se borra al principio.
- Después, obtenga la lista de ciudades reconocidas. Seleccione la primera y trate de resolverla.
- La función que la resuelve (resolveCity) se ha definido anteriormente en el código. Contiene una lista de nombres canónicos de ciudades. Encuentra el nombre de la ciudad en la lista y lo devuelve. De lo contrario, busca en "cityMap" y devuelve el nombre asignado. Si no logra encontrar una ciudad, devuelve null.
- Por último, si la ciudad se resuelve con un nombre, se almacena en la entidad $CityKnown. En caso contrario, borre lo que el usuario ha dicho y rellene la entidad $CityUknown.

### <a name="train-dialogs"></a>Diálogos de entrenamiento

1. Haga clic en Train Dialogs (Diálogos de entrenamiento) y, a continuación, en New Train Dialog (Nuevo diálogo de entrenamiento).
2. Escriba "hello" (hola).
3. Haga clic en Score Actions (Acciones de puntuación) y seleccione "Which city do you want?" (¿Qué ciudad desea?).
2. Escriba "Nueva York".
    - El texto se reconoce como una entidad de ciudad.
5. Haga clic en Score Actions (Acciones de puntuación)
    - Se rellenaron `City` y `CityResolved`.
6. Seleccione "You said $City, and I resolved that to $CityResolved" (Ha dicho $City, y la he resuelto como $CityResolved).
7. Haga clic en Done Teaching (Aprendizaje completado).

Agregue otro diálogo de ejemplo:

1. Haga clic en New Train Dialog (Haga clic en diálogo de entrenamiento).
2. Escriba "hello" (hola).
3. Haga clic en Score Actions (Acciones de puntuación) y seleccione "Which city do you want?" (¿Qué ciudad desea?).
2. Escriba "Gran Manzana".
    - El texto se reconoce como una entidad de ciudad.
5. Haga clic en Score Actions (Acciones de puntuación)
    - `CityResolved` muestra el efecto del código en ejecución.
6. Seleccione "You said $City, and I resolved that to $CityResolved" (Ha dicho $City, y la he resuelto como $CityResolved).
7. Haga clic en Done Teaching (Aprendizaje completado).

Agregue otro diálogo de ejemplo:

1. Haga clic en New Train Dialog (Haga clic en diálogo de entrenamiento).
2. Escriba "hello" (hola).
3. Haga clic en Score Actions (Acciones de puntuación) y seleccione "Which city do you want?" (¿Qué ciudad desea?).
2. Escriba "foo".
    - Este es un ejemplo de una ciudad que el sistema no conoce. 
5. Haga clic en Score Actions (Acciones de puntuación).
6. Seleccione "I don't know this city, $CityUknown. Which city do you want?" (No conozco esta ciudad, $CityUknown. ¿Qué ciudad desea?).
7. Escriba "Nueva York".
8. Haga clic en Score Actions (Acciones de puntuación).
    - `CityUknown` se borró y `CityResolved` se rellenó.
6. Seleccione "You said $City, and I resolved that to $CityResolved" (Ha dicho $City, y la he resuelto como $CityResolved).
7. Haga clic en Done Teaching (Aprendizaje completado).

![](../media/tutorial10_bigapple.PNG)

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Devoluciones de llamada de sesión](./11-session-callbacks.md)
