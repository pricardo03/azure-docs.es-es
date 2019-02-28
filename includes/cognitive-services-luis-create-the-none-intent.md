---
title: archivo de inclusión
description: archivo de inclusión
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.subservice: luis
ms.topic: include
ms.custom: include file
ms.date: 12/21/2018
ms.author: diberry
ms.openlocfilehash: 355fe134939b26c51d6e03368f782845628a6b96
ms.sourcegitcommit: 90c6b63552f6b7f8efac7f5c375e77526841a678
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/23/2019
ms.locfileid: "56738927"
---
La aplicación cliente debe saber si una expresión no es significativa o adecuada para la aplicación. La intención **None** se agrega a cada aplicación como parte del proceso de creación para determinar si la aplicación cliente no puede responder a una expresión.

Si LUIS devuelve la intención **None** para una expresión, la aplicación cliente puede preguntar al usuario si quiere finalizar la conversación o dar más instrucciones para continuarla. 

> [!CAUTION] 
> No deje la intención **None** vacía. 

1. Seleccione **Intents** (Intenciones) en el panel izquierdo.

2. Seleccione la intención **None**. Agregue tres expresiones que el usuario podría solicitar pero que no son pertinentes para la aplicación de recursos humanos:

    | Expresiones de ejemplo|
    |--|
    |Barking dogs are annoying (Los perros que ladran son molestos)|
    |Order a pizza for me (Pídanme una pizza)|
    |Penguins in the ocean (Pingüinos en el océano)|
