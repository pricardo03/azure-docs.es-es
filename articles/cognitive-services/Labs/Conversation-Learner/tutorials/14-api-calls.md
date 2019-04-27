---
title: 'Uso de llamadas API con un modelo de Conversation Learner: Microsoft Cognitive Services | Microsoft Docs'
titleSuffix: Azure
description: Obtenga información sobre cómo usar las llamadas API con un modelo de Conversation Learner.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.subservice: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: aba3c2eb925370704ea52364891502a7a09cc9ec
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60635760"
---
# <a name="how-to-add-api-calls-to-a-conversation-learner-model"></a>Incorporación de llamadas API a un modelo de Conversation Learner

En este tutorial se explica cómo agregar llamadas API al modelo. Las llamadas API son funciones que se definen y escriben en el bot y a las que Conversation Learner puede llamar.

## <a name="video"></a>Vídeo

[![Vista previa del tutorial de llamadas API](https://aka.ms/cl_Tutorial_v3_APICalls_Preview)](https://aka.ms/cl_Tutorial_v3_APICalls)

## <a name="requirements"></a>Requisitos
Este tutorial requiere la ejecución del bot "tutorialAPICalls.ts".

    npm run tutorial-api-calls

## <a name="details"></a>Detalles

- Las llamadas API pueden leer y manipular entidades.
- Las llamadas API tienen acceso al objeto del administrador de memoria.
- Las llamadas API pueden tomar argumentos.

### <a name="open-the-demo"></a>Abrir la demostración

En la interfaz de usuario web, haga clic en "Import Tutorials" (Importar tutoriales) y seleccione el modelo denominado "Tutorial-14-APICalls".

### <a name="entities"></a>Entidades

Hemos definido una entidad en el modelo, denominada `number`.

![](../media/tutorial12_entities.PNG)

### <a name="api-calls"></a>Llamadas a API
El código de las llamadas API se define en este archivo: `C:\<installedpath>\src\demos\tutorialAPICalls.ts`.

![](../media/tutorial12_apicalls.PNG)

- La devolución de llamada `RandomGreeting` devuelve un saludo aleatorio definido en la matriz `greeting`.
- La devolución de llamada `Multiply` multiplicará dos números que ha pasado la acción que la llama y devuelve un resultado que se puede representar en la interfaz de usuario.
    - Tenga en cuenta que el administrador de memoria es el primer argumento. 
    - Tenga en cuenta que las devoluciones de llamada API pueden tener varias entradas, en este caso `num1string` y `num2string`.
- La devolución de llamada `ClearEntities` borra la entidad numérica para que el usuario pueda escribir otro número. 
    - Se ilustra la forma en que las llamadas API pueden manipular entidades.

### <a name="actions"></a>Acciones
Hemos creado cuatro acciones. Tres de ellas son las acciones de API "No-Wait" y la cuarta es una acción "Text" que pregunta al usuario algo similar a lo que hemos visto en otros tutoriales. Para ver cómo se ha creado cada una, haga lo siguiente:
1. En el panel izquierdo, haga clic en "Actions" (Acciones) y luego en una de las cuatro acciones que aparecen en la cuadrícula.
2. Fíjese en los valores de cada campo en el formulario que aparece.
3. Fíjese en el botón `Refresh` situado junto al campo API.
    - Si tiene que detener el bot y realizar cambios en las API mientras la página de la interfaz de usuario está abierta, puede hacer clic en el botón `Refresh` para que se actualicen los cambios más recientes.

![](../media/tutorial12_actions.PNG)

#### <a name="clearentities-multiply-and-randomgreeting"></a>ClearEntities, Multiply y RandomGreeting
Estas tres acciones son de tipo API. Todas ellas se basan en las funciones de devolución de llamada API para realizar su trabajo y, posiblemente, devuelven un valor que se presentará al usuario.

#### <a name="what-number-do-you-want-to-multiply-by-12"></a>"What number do you want to multiply by 12" (¿Qué número quiere multiplicar por 12?)
Esta es la acción "Text" y simplemente hace una pregunta al usuario. Aunque esta acción no interactúa realmente con una de las devoluciones de llamada API, pide al usuario que responda con un número que pasará a la memoria de una entidad que puede usar después la acción "Multiply" que utiliza una de las devoluciones de llamada API.


### <a name="train-dialog"></a>Diálogo de entrenamiento

Veamos un "diálogo de entrenamiento".

1. En el panel izquierdo, haga clic en `Train Dialogs` y después en el botón `New Train Dialog`.
2. Escriba "hola".
3. Haga clic en el botón `Score Actions`.
4. Seleccione `RandomGreeting`. 
    - Esto ejecutará la llamada a Random Greeting API.
    - Esto NO esperará una respuesta del usuario.
5. Seleccionar `What number to do you want to multiply by 12?`
6. Escriba un número, cualquier número y solo un número.
    - Fíjese en que el número se ha etiquetado automáticamente como la entidad `number`.
7. Haga clic en el botón `Score Actions`.
8. Seleccione la acción `Multiply`.
    - Fíjese en el resultado de la multiplicación por 12.
    - Fíjese en que la memoria sigue conteniendo el valor que ha especificado en `number`.
9. Seleccione la acción `ClearEntities`.
    - Fíjese en que el valor de la entidad `number` se ha borrado de la memoria.
10. Haga clic en el botón `Save`.

Ha visto cómo registrar devoluciones de llamada de API, sus patrones comunes y cómo definir argumentos y asociar valores y entidades en ellos.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Tarjetas parte 1](./15-cards.md)
