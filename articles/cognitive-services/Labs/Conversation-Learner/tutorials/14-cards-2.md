---
title: 'Cómo usar tarjetas con un modelo de Conversation Learner, parte 2: Microsoft Cognitive Services | Microsoft Docs'
titleSuffix: Azure
description: Obtenga información sobre cómo usar tarjetas con una modelo de Conversation Learner.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 1c7c88742c69041594006add76f7e3c642c64dec
ms.sourcegitcommit: 4e5ac8a7fc5c17af68372f4597573210867d05df
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/20/2018
ms.locfileid: "39170579"
---
# <a name="how-to-use-cards-part-1-of-2"></a>Cómo usar tarjetas (parte 1 de 2)
En este tutorial se explica cómo agregar una tarjeta de formulario rellenable al bot. Se le mostrará cómo los campos de formulario se convierten en entidades.

Conversation Learner espera que los archivos de definición de tarjetas se encuentren en un directorio denominado "cards" que se encuentre en el directorio donde se ha iniciado el bot.

## <a name="video"></a>Vídeo

[![Vista previa del tutorial 14](http://aka.ms/cl-tutorial-14-preview)](http://aka.ms/blis-tutorial-14)

## <a name="requirements"></a>Requisitos
Para poder realizar este tutorial debe ejecutar el bot del tutorial general

    npm run tutorial-general

## <a name="details"></a>Detalles

Las tarjetas son elementos de interfaz de usuario que permiten al usuario seleccionar una opción en la conversación. 

### <a name="open-the-demo"></a>Abrir la demostración

En la lista de modelos de la interfaz de usuario web, haga clic en Tutorial-14-Cards-2. 

### <a name="the-card"></a>Tarjeta

La definición de tarjeta está en la siguiente ubicación: C:\<installedpath\>\src\cards\shippingAddress.json.

Esta tarjeta recopila tres campos de la dirección de envío: ciudad, calle y estado.

![](../media/tutorial14_card.PNG)

### <a name="actions"></a>Acciones

Se han creado tres acciones. Como se ve a continuación, la primera acción es una tarjeta.

![](../media/tutorial14_actions.PNG)

Veamos cómo se creó el tipo de acción de la tarjeta:

- Observe Address-Street (Dirección: calle), donde el tipo es Input.text y su identificador.
- De forma similar, hay un campo Address-City (Dirección: ciudad) y un menú desplegable con el identificador de Address-State (Dirección: estado).

Los identificadores son importantes, ya que cuando los campos se rellenan y envían, serán los nombres de entidad que recibirán dichos valores en el bot.

## <a name="entities"></a>Entidades
Se han definido tres entidades coincidentes con la tarjeta, como se ha visto anteriormente.

![](../media/tutorial14_entities.PNG)

## <a name="actions"></a>Acciones

Hemos definido dos acciones.

![](../media/tutorial14_actions.PNG)

- La primera es la tarjeta de dirección de envío, donde el tipo de acción es TARJETA y la Plantilla se selecciona del menú desplegable como shippingAddress.
- La segunda es una acción sencilla para volver a leer la dirección de envío.

![](../media/tutorial14_sa_card.PNG)

### <a name="train-dialog"></a>Diálogo de entrenamiento

Veamos un cuadro de diálogo de instrucción.

1. Haga clic en Train Dialogs (Diálogos de entrenamiento) y, a continuación, en New Train Dialog (Nuevo diálogo de entrenamiento).
1. Escriba "hi" (hola).
2. Haga clic en Score Action (Acción de puntuación).
3. Haga clic para seleccionar "Shipping Address" (Dirección de envío).
4. Rellene la tarjeta y envíela.
    - Observe que dichos valores se han movido ahora a la memoria de la entidad. No se necesita ningún análisis, ya que el formulario ya ha particionado las entradas.
5. Haga clic en Score Actions (Acciones de puntuación).
3. Haga clic para seleccionar "Shipping to $Address..." (Enviar a $Address...).
4. Haga clic en Done Testing (Prueba completada).

![](../media/tutorial14_train_dialog.PNG)

Ahora ha visto cómo obtener los valores de la tarjeta que tiene campos rellenables y menús desplegables, así como la forma de capturarlos y recopilarlos en entidades de bot.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Crear ramas y deshacer](./15-branching-and-undo.md)
