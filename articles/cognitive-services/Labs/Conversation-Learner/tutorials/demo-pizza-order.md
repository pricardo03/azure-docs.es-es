---
title: 'Aplicación Conversation Learner de demostración, pedido de pizza: Microsoft Cognitive Services | Microsoft Docs'
titleSuffix: Azure
description: Obtenga información acerca de cómo crear una aplicación Conversation Learner de demostración.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 99cd89c4f4430f2d65ed0963e3092d51a83842d7
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/23/2018
ms.locfileid: "35381839"
---
# <a name="demo-pizza-order"></a>Demostración: pedido de pizza
Esta demostración ilustra un bot de realización de pedidos de pizza. Admite el pedido de una sola pizza con esta funcionalidad:

- Reconocer los ingredientes de pizza en las expresiones del usuario.
- Comprobar si quedan ingredientes de pizza en stock o si están agotados y responder en consecuencia.
- Recordar los ingredientes de pizza de un pedido anterior y ofrecerlos para iniciar un nuevo pedido con los mismos ingredientes.

## <a name="requirements"></a>Requisitos
Para poder realizar este tutorial debe ejecutar el bot de pedido de pizza.

    npm run demo-pizza

### <a name="open-the-demo"></a>Abrir la demostración

En la lista de aplicaciones de la interfaz de usuario web, haga clic en TutorialDemo Pizza Order. 

## <a name="entities"></a>Entidades

Hemos creado tres entidades.

- Ingredientes: acumulará los ingredientes que el usuario ha pedido. Incluye los ingredientes válidos que se encuentran en stock. Comprueba si un queda ingrediente en stock o si está agotado.
- OutofStock: se usa para comunicar al usuario que el ingrediente seleccionado está agotado.
- LastToppings: una vez que se realiza un pedido, esta entidad se usa para ofrecer al usuario la lista de ingredientes en su pedido.

![](../media/tutorial_pizza_entities.PNG)

### <a name="actions"></a>Acciones

Hemos creado un conjunto de acciones, que incluyen preguntar al usuario qué quiere en su pizza, indicarle qué ha agregado hasta ahora, etc.

También existen dos llamadas API:

- FinalizeOrder: para realizar el pedido de pizza.
- UseLastToppings: para migrar los ingredientes del pedido anterior. 

![](../media/tutorial_pizza_actions.PNG)

### <a name="training-dialogs"></a>Cuadros de diálogo de entrenamiento
Hemos definido una serie de cuadros de diálogo de entrenamiento. 

![](../media/tutorial_pizza_dialogs.PNG)

Por ejemplo, vamos a probar una sesión de instrucción.

1. Haga clic en Train Dialogs (Diálogos de entrenamiento) y, a continuación, en New Train Dialog (Nuevo diálogo de entrenamiento).
1. Escriba "order a pizza" (pedir una pizza).
2. Haga clic en Score Action (Acción de puntuación).
3. Haga clic para seleccionar "What would you like on your pizza" (¿Qué quieres que lleve la pizza?)
4. Escriba "mushrooms and cheese" (setas y queso).
    - Observe que LUIS los ha etiquetado ambos como Toppings (Ingredientes). Si no es correcto, podría haga clic para resaltarlo y corregirlo.
    - El signo "+" situado junto a la entidad significa que se está agregando al conjunto de ingredientes.
5. Haga clic en Score Actions (Acciones de puntuación).
    - Observe que las setas y el queso no están en la memoria de ingredientes.
3. Haga clic para seleccionar "You have $Toppings on your pizza" (Tiene $Toppings en su pizza).
    - Observe que esta es una acción de no espera, por lo que el bot le preguntará por la siguiente acción.
6. Seleccione "Would you like anything else?" (¿Quiere algo más?).
7. Escriba "Remove mushrooms and add peppers" (Quitar setas y agregar pimientos).
    - Observe que el ingrediente **mushrooms** (setas) tiene un signo "-" al lado para quitarlo. Observe también que el ingrediente "peppers" (pimientos) tiene un signo "+" para agregarlo.
2. Haga clic en Score Action (Acción de puntuación).
    - Observe que **peppers** (pimientos) está en negrita porque es nuevo. Observe también que **mushrooms** (setas) está tachado.
8. Haga clic para seleccionar "You have $Toppings on your pizza" (Tiene $Toppings en su pizza).
6. Seleccione "Would you like anything else?" (¿Quiere algo más?).
7. Escriba "add peas" (agregar guisantes).
    - Los guisantes son un ejemplo de ingrediente que está agotado. Observe que sigue etiquetado como ingrediente.
2. Haga clic en Score Action (Acción de puntuación).
    - Los guisantes aparecen como OutOfStock.
    - Para ver cómo ha ocurrido esto, vamos a abrir el código en C:\<\installedpath>\src\demos\demoPizzaOrder.ts. Observe el método EntityDetectionCallback. Este método se llama después de cada ingrediente para ver si está en stock. Si no es así, se borra del conjunto de ingredientes y se agrega a la entidad OutOfStock. La variable inStock se define sobre el método que tiene la lista de ingredientes en stock.
6. Seleccione "We don't have $OutOfStock" (No tenemos $OutOfStock).
7. Seleccione "Would you like anything else?" (¿Quiere algo más?).
8. Escriba "no".
9. Haga clic en Score Action (Acción de puntuación).
10. Seleccione la llamada API "FinalizeOrder". 
    - Esta llamará a la función "FinalizeOrder" definida en el código. Esta acción borra los ingredientes y devuelve "Your order is on its way" (Su pedido está en camino). 
2. Escriba "order another" (realizar otro pedido). Se está iniciando un nuevo pedido.
9. Haga clic en Score Action (Acción de puntuación).
    - Observe que el queso y los pimientos están en la memoria como ingredientes del último pedido.
1. Seleccione "Would you like $LastToppings" (¿Quiere $LastToppings?).
2. Escriba "yes" (sí).
3. Haga clic en Score Action (Acción de puntuación).
    - El bot quiere realizar la acción UseLastToppings. Este es el segundo de los dos métodos de devolución de llamada. Copiará los ingredientes del último pedido en la lista de ingredientes y borrará los últimos ingredientes. Esta es una manera de recordar el último pedido. Si el usuario indica que quiere otra pizza, proporciona esos ingredientes como opciones.
2. Haga clic para seleccionar "You have $Toppings on your pizza" (Tiene $Toppings en su pizza).
3. Seleccione "Would you like anything else?" (¿Quiere algo más?).
8. Escriba "no".
4. Haga clic en Done Teaching (Aprendizaje completado).

![](../media/tutorial_pizza_callbackcode.PNG)

![](../media/tutorial_pizza_apicalls.PNG)

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Demostración: iniciador de aplicaciones de realidad virtual](./demo-vr-app-launcher.md)
