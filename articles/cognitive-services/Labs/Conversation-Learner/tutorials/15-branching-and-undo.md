---
title: 'Uso de las operaciones de bifurcación y de deshacer con una aplicación Conversation Learner: Microsoft Cognitive Services | Microsoft Docs'
titleSuffix: Azure
description: Obtenga información sobre cómo usar las operaciones de bifurcación y de deshacer con una aplicación Conversation Learner.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 724a9e47267e0bd7417130efe54c609ac7a465fb
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/23/2018
ms.locfileid: "35381779"
---
# <a name="how-to-use-branching-and-undo-operations"></a>Uso de las operaciones de bifurcación y de deshacer
En este tutorial, se van a analizar las operaciones de deshacer y bifurcación.

## <a name="details"></a>Detalles
- Deshacer: permite al desarrollador "deshacer" una entrada de usuario o una selección de acciones. En segundo plano, "deshacer" realmente crea un diálogo y lo vuelve a reproducir hasta el paso anterior.  Esto significa que se volverá a llamar a la devolución de llamada de detección de entidades y a las llamadas API en el diálogo.

- Bifurcación: crea un diálogo de entrenamiento que empieza de la misma forma que un diálogo de entrenamiento existente; esto ahorra el esfuerzo de volver a introducir manualmente los turnos del diálogo. En segundo plano, la "bifurcación" crea un diálogo y vuelve a reproducir el diálogo de entrenamiento existente hasta el paso seleccionado.  Esto significa que se volverá a llamar a la devolución de llamada de detección de entidades y a las llamadas API en el diálogo.


## <a name="requirements"></a>Requisitos
Para poder realizar este tutorial debe ejecutar el bot de pedido de pizza:

    npm run demo-pizza

### <a name="open-the-demo"></a>Abrir la demostración

En la lista de aplicaciones de la interfaz de usuario web, haga clic en TutorialDemo Pizza Order. 

Para obtener información sobre la demostración Pizza Order, vea el tutorial sobre pedidos de pizza.

## <a name="undo"></a>Deshacer

Se va a deshacer parte del diálogo y se volverá a crear a partir de dicho paso.

### <a name="training-dialogs"></a>Diálogos de entrenamiento
Vamos a iniciar una sesión de entrenamiento. 

1. Haga clic en Train Dialogs (Diálogos de entrenamiento) y, a continuación, en New Train Dialog (Nuevo diálogo de entrenamiento).
1. Escriba "order a pizza" (pedir una pizza).
2. Haga clic en Score Action (Acción de puntuación).
3. Haga clic para seleccionar "What would you like on your pizza" (¿Qué quieres que lleve la pizza?).
4. Escriba "mushrooms and cheese" (setas y queso).
5. Haga clic en Score Actions (Acciones de puntuación).
3. Haga clic para seleccionar "You have $Toppings on your pizza" (Tiene $Toppings en su pizza).
6. Seleccione "Would you like anything else?" (¿Quiere algo más?).
7. Escriba "Remove mushrooms and add peppers" (Quitar setas y agregar pimientos).
    - Seleccione setas y desactive la entidad de ingredientes. Vamos a crear una acción que se va a deshacer.
2. Haga clic en Undo Step (Deshacer paso).
    - Tenga en cuenta que se elimina la última entrada y que se vuelve a "Would you like anything else?" (¿Quiere algo más?).  (captura de pantalla siguiente)
2. Escriba "Remove mushrooms and add peppers" (Quitar setas y agregar pimientos).
8. Haga clic para seleccionar "You have $Toppings on your pizza" (Tiene $Toppings en su pizza).
    - Asegúrese de que ambas entidades se seleccionan correctamente.
2. Haga clic en Score Action (Acción de puntuación). Puede continuar con el diálogo corregido ahora.
4. Haga clic en Done Teaching (Aprendizaje completado).

Ahora ha visto cómo usar la opción de deshacer para quitar una acción y una entrada de usuario.

![](../media/tutorial15_undo.PNG)

## <a name="branch"></a>Rama

Por ejemplo, se va a abrir un diálogo de entrenamiento existente y crear otro diálogo de entrenamiento mediante bifurcación.

1. Haga clic en Train Dialogs (Diálogos de entrenamiento) y luego en "new order" (nuevo pedido) para abrir el diálogo existente. 
2. Haga clic en el último "no" en el diálogo (véase la captura de pantalla siguiente).
3. Haga clic en Bifurcación.
    - Tenga en cuenta que se elimina "no", y el diálogo entero hasta dicho punto se copia en otro nuevo. 
    - Esto ahorra tener que reintroducir los turnos anteriores para explorar una nueva "bifurcación" a partir de este punto.
1. Escriba "sí".
2. Haga clic en Score Action (Acción de puntuación).
3. Seleccione "You have $Toppings on your pizza" (Tiene $Toppings en su pizza).
6. Seleccione "Would you like anything else?" (¿Quiere algo más?).
7. Escriba "no".
4. Haga clic en Done Teaching (Aprendizaje completado).

![](../media/tutorial15_branch.PNG)

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Control de versiones y etiquetado](./16-versioning-and-tagging.md)
