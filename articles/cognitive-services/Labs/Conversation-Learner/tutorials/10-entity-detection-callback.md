---
title: 'Uso de la devolución de llamada de detección de entidades con una aplicación Conversation Learner: Microsoft Cognitive Services | Microsoft Docs'
titleSuffix: Azure
description: Obtenga información sobre cómo utilizar la devolución de llamada de detección de entidades con una aplicación Conversation Learner.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: e41ea5930ff0c8395d0c93aa42e224ebfc894ba8
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/23/2018
ms.locfileid: "35381771"
---
# <a name="how-to-use-entity-detection-callback"></a>Uso de la devolución de llamada de detección de entidades

En este tutorial se explica la devolución de llamada de detección de entidades y también se ilustra un patrón común para resolver entidades.

## <a name="requirements"></a>Requisitos
Este tutorial requiere la ejecución del bot "tutorialEntityDetectionCallback".

    npm run tutorial-entity-detection

## <a name="details"></a>Detalles
La devolución de llamada de detección de entidades permite utilizar código personalizado para controlar las reglas de negocio relacionadas con entidades. En esta demostración, se usarán las devoluciones de llamada y las entidades de programación para resolver el nombre de la ciudad escrito por el usuario como un nombre canónico; por ejemplo, resolver "La Gran Manzana" para "Nueva York".

### <a name="open-the-demo"></a>Abrir la demostración

En la lista de aplicaciones, haga clic en Tutorial-10-EntityDetenctionCallback. 

### <a name="entities"></a>Entidades

Se han definido tres entidades en la aplicación.

![](../media/tutorial10_entities.PNG)

1. La ciudad es una entidad personalizada que proporcionará el usuario como entrada de texto.
2. CityUnknown es una entidad de programación. El sistema rellenará esta información. Copiará la entrada de usuario si el sistema no sabe de qué ciudad se trata.
3. CityResolved es la ciudad que el sistema no conoce. Será el nombre canónico de la ciudad; por ejemplo, "La Gran Manzana" se resolverá como "Nueva York".

### <a name="actions"></a>Acciones

Se han creado tres acciones. 

![](../media/tutorial10_actions.PNG)

1. La primera acción es "Which city do you want?" (¿Qué ciudad desea?).
2. La segunda es "I don't know this city, $CityUknown. Which city do you want?" (No conozco esta ciudad, $CityUknown. ¿Qué ciudad desea?).
3. La tercera es "You said $City, and I resolved that to $CityResolved" (Ha dicho $City, y la he resuelto como $CityResolved).

### <a name="callback-code"></a>Código de devolución de llamada

Echemos un vistazo al código. Puede encontrar el método EntityDetectionCallback en el archivo C:\<installedpath>\src\demos\tutorialSessionCallbacks.ts.

![](../media/tutorial10_callbackcode.PNG)

Se llama a esta función una vez resuelta la entidad.
 
- Lo primero que se hará será borrar $CityUknown. $CityUknown solo persistirá una sola vez, ya que siempre se borra al principio.
- Después, se obtiene la lista de ciudades reconocidas. Seleccione la primera y trate de resolverla.
- La función que la resuelve (resolveCity) se ha definido anteriormente en el código. Contiene una lista de nombres canónicos de ciudades. Encuentra el nombre de la ciudad en la lista y lo devuelve. De lo contrario, busca en "cityMap" y devuelve el nombre asignado. Si no logra encontrar una ciudad, devuelve null.
- Por último, si la ciudad se resuelve con un nombre, se almacena en la entidad $CityKnown. En caso contrario, borre lo que el usuario ha dicho y rellene la entidad $CityUknown.

### <a name="train-dialogs"></a>Diálogos de entrenamiento

1. Haga clic en Train Dialogs (Diálogos de entrenamiento) y, a continuación, en New Train Dialog (Nuevo diálogo de entrenamiento).
2. Escriba "hello" (hola).
3. Haga clic en Score Actions (Acciones de puntuación) y seleccione "Which city do you want?" (¿Qué ciudad desea?).
2. Escriba "Nueva York".
    - Observe que se reconoce como una entidad de ciudad.
5. Haga clic en Score Actions (Acciones de puntuación).
    - Tenga en cuenta que se rellenan las entidades City (Ciudad) y CityResolved (Ciudad resuelta).
6. Seleccione "You said $City, and I resolved that to $CityResolved" (Ha dicho $City, y la he resuelto como $CityResolved).
7. Haga clic en Done Teaching (Aprendizaje completado).

Agregue otro diálogo de ejemplo:

1. Haga clic en New Train Dialog (Haga clic en diálogo de entrenamiento).
2. Escriba "hello" (hola).
3. Haga clic en Score Actions (Acciones de puntuación) y seleccione "Which city do you want?" (¿Qué ciudad desea?).
2. Escriba "Gran Manzana".
    - Observe que se reconoce como una entidad de ciudad.
5. Haga clic en Score Actions (Acciones de puntuación).
    - Tenga en cuenta que CityResolved muestra el efecto de la ejecución del código.
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
    - Tenga en cuenta que se ha borrado CityUknown y que se ha resuelto CityResolved.
6. Seleccione "You said $City, and I resolved that to $CityResolved" (Ha dicho $City, y la he resuelto como $CityResolved).
7. Haga clic en Done Teaching (Aprendizaje completado).

![](../media/tutorial10_bigapple.PNG)

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Devoluciones de llamada de sesión](./11-session-callbacks.md)
