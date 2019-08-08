---
title: 'Uso de las operaciones de bifurcación y de deshacer con un modelo de Conversation Learner: Microsoft Cognitive Services | Microsoft Docs'
titleSuffix: Azure
description: Obtenga información sobre cómo usar las operaciones de bifurcación y de deshacer con un modelo de Conversation Learner.
services: cognitive-services
author: nitinme
manager: nolachar
ms.service: cognitive-services
ms.subservice: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: nitinme
ROBOTS: NOINDEX
ms.openlocfilehash: f4f3024451696dbd0244d9da39cba67b49447af1
ms.sourcegitcommit: ad9120a73d5072aac478f33b4dad47bf63aa1aaa
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/01/2019
ms.locfileid: "68703639"
---
# <a name="how-to-use-branching-and-undo-operations"></a>Uso de las operaciones de ramificación y de deshacer
En este tutorial, se van a analizar las operaciones de deshacer y bifurcación.


## <a name="details"></a>Detalles
### <a name="undo"></a>Deshacer
Permite al desarrollador "deshacer" la última entrada de usuario o una selección de acciones. En segundo plano, "deshacer" realmente crea un diálogo y lo vuelve a reproducir hasta el paso anterior.  Esto significa que se volverá a llamar a la devolución de llamada de detección de entidades y a las llamadas API en el diálogo.

### <a name="branch"></a>Rama
Crea un diálogo de entrenamiento que empieza de la misma forma que un diálogo de entrenamiento existente; esto ahorra el esfuerzo de volver a introducir manualmente los turnos del diálogo. En segundo plano, la "rama" crea un diálogo y vuelve a reproducir el diálogo de entrenamiento existente hasta el paso seleccionado.  Esto significa que se volverá a llamar a la devolución de llamada de detección de entidades y a las llamadas API en el diálogo.


## <a name="requirements"></a>Requisitos
Para poder realizar este tutorial debe ejecutar el bot de pedido de pizza:

    npm run demo-pizza

### <a name="open-or-import-the-demo"></a>Apertura o importación de la demostración

Si ya ha trabajado en el tutorial de pedido de pizza, simplemente abra ese modelo en la lista en la interfaz de usuario web. En caso contrario, debe hacer clic en "Import Tutorials" (Importar tutoriales) y seleccionar el modelo denominado "Demo-PizzaOrder" (Demostración-PedidoPizza).

## <a name="undo"></a>Deshacer

Este es un ejemplo de cómo ver la característica `Undo` en acción:

### <a name="training-dialogs"></a>Diálogos de entrenamiento
1. En el panel izquierdo, haga clic en "Train Dialogs" (Diálogos de entrenamiento) y, a continuación, en el botón `New Train Dialog`.
2. Escriba "Order a pizza" (Pedir una pizza).
3. Haga clic en el botón `Score Actions`.
4. Haga clic para seleccionar "What would you like on your pizza" (¿Qué quiere que lleve la pizza?).
5. Escriba "anything" (nada).
6. Haga clic en el botón `Undo`.
    - Se elimina la última entrada, dejando la última respuesta del bot: "What would you like on your pizza" (¿Qué quiere que lleve la pizza?).

## <a name="branch"></a>Rama

Para esta demostración, abriremos un diálogo de entrenamiento existente y crearemos un nuevo diálogo de entrenamiento a partir del primero, mediante ramificación.

1. En el panel izquierdo, haga clic en "Train Dialogs" (Diálogos de entrenamiento).
2. Observe la cuadrícula, verá solo un entrenamiento que empieza por "new order" (nuevo pedido).
3. En la cuadrícula, haga clic en "new order" (nuevo pedido) para abrir el diálogo de entrenamiento existente.
4. Haga clic en el último "no" en el diálogo.
5. Haga clic en el icono de "rama", que se muestra en un círculo en rojo en esta imagen:
    - ![](../media/tutorial15_branch.PNG)
    - Se copia todo el diálogo de entrenamiento anterior al "no" en un nuevo diálogo de entrenamiento.
    - Esto evita tener que reintroducir los turnos anteriores para explorar la "rama" de una nueva conversación a partir de este punto.
6. Escriba "sí" y pulse Entrar.
7. Haga clic en el botón `Score Actions`.
    - En este momento, el bot elige automáticamente una respuesta, pero esta respuesta no nos gusta, por lo que vamos a cambiarla.
8. Haga clic en la última respuesta del bot.
    - Esto nos permitirá seleccionar una respuesta distinta.
9. Seleccione "UseLastToppings" (UsarÚltimosIngredientes).
10. Haga clic en el botón `Score Actions`.
    - De nuevo, el bot elige automáticamente una respuesta. Debería decir "You have sausage, cheese and mushrooms on your pizza" (Su pizza tiene jamón, queso y champiñones). 
    - Esta vez nos gusta la respuesta, por lo que la mantendremos.
11. Haga clic en el botón `Score Actions`.
    - De nuevo, el bot elige automáticamente una respuesta; debería decir "Would you like anything else?" (¿Desea algo más?).
12. Escriba "no".
13. Haga clic en el botón `Save Branch`.
14. Observe que ahora la cuadrícula tiene dos entrenamientos que empiezan por "new order" (nuevo pedido).
    - Uno de ellos es el que ha usado para realizar la ramificación correspondiente.
    - Y el otro es la versión ramificada que acaba de guardar.
    - Haga clic en cada uno de ellos para comprobar estas expectativas.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Control de versiones y etiquetado](./18-version-tag.md)
