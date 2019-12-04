---
title: archivo de inclusión
description: archivo de inclusión
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: luis
ms.topic: include
ms.custom: include file
ms.date: 11/20/2019
ms.author: diberry
ms.openlocfilehash: e4147fbb27c8538f801f6c49f8b535a283faf50f
ms.sourcegitcommit: b77e97709663c0c9f84d95c1f0578fcfcb3b2a6c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/22/2019
ms.locfileid: "74325901"
---
La aplicación cliente debe saber si una expresión no es significativa o adecuada para la aplicación. La intención **None** se agrega a cada aplicación como parte del proceso de creación para determinar si la aplicación cliente no debe responder a una expresión.

Si LUIS devuelve la intención **None** para una expresión, la aplicación cliente puede preguntar al usuario si quiere finalizar la conversación o dar más instrucciones para continuarla. 

Si deja la intención **None** vacía, una expresión que debería predecirse fuera del dominio del asunto se predecirá en una de las intenciones del dominio de sujeto existente. El resultado es que la aplicación cliente, como un bot de chat, realizará operaciones incorrectas en función de una predicción incorrecta. 

1. Seleccione **Intents** (Intenciones) en el panel izquierdo.

1. Seleccione la intención **None**. Agregue tres expresiones que el usuario podría solicitar pero que no son pertinentes para la aplicación de pedidos de pizza.

    |Expresiones de ejemplo de `None`|
    |--|
    |`Barking dogs are annoying`|
    |`Order a pizza for me`|
    |`Penguins in the ocean`|

    Estos ejemplos no deben usar palabras que se esperan en el dominio de sujeto, como `pizza`, `cheese`, `crust`, `pickup` `deliver`.