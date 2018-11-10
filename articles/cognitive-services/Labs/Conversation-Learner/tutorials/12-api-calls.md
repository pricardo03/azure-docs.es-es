---
title: 'Uso de llamadas API con un modelo de Conversation Learner: Microsoft Cognitive Services | Microsoft Docs'
titleSuffix: Azure
description: Obtenga información sobre cómo usar las llamadas API con un modelo de Conversation Learner.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 815d1e9f6d1e4b9937647d55b67665e1b27f501e
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/07/2018
ms.locfileid: "51240773"
---
# <a name="how-to-add-api-calls-to-a-conversation-learner-model"></a>Incorporación de llamadas API a un modelo de Conversation Learner

En este tutorial se explica cómo agregar llamadas API al modelo. Las llamadas API son funciones que se definen y escriben en el bot y a las que Conversation Learner puede llamar.

## <a name="video"></a>Vídeo

[![Vista previa del tutorial 12](https://aka.ms/cl-tutorial-12-preview)](https://aka.ms/blis-tutorial-12)

## <a name="requirements"></a>Requisitos
Este tutorial requiere la ejecución del bot "tutorialAPICalls.ts".

    npm run tutorial-api-calls

## <a name="details"></a>Detalles

- Las llamadas API pueden leer y manipular entidades.
- Las llamadas API tienen acceso al objeto del administrador de memoria.
- Las llamadas API también pueden adoptar argumentos; esto permite reutilizar la misma llamada API para distintos propósitos.

### <a name="open-the-demo"></a>Abrir la demostración

En la lista de modelos de la interfaz de usuario web, haga clic en Tutorial-12-APICalls. 

### <a name="entities"></a>Entidades

Hemos definido una entidad en el modelo, llamada number.

![](../media/tutorial12_entities.PNG)

### <a name="api-calls"></a>Llamadas a API
El código de las llamadas API se define en este archivo: C:\<installedpath\>\src\demos\tutorialAPICalls.ts.

![](../media/tutorial12_apicalls.PNG)

- La primera devolución de llamada de API es RandomGreeting. Devuelve un saludo aleatorio definido en la variable de saludo.
- Devolución de llamada de Multiply API: multiplicará dos números proporcionados por el usuario. Devuelve el resultado de la multiplicación de los dos números. Esto muestra que las devoluciones de llamada de API puede adoptar entradas. Tenga en cuenta que el administrador de memoria es el primer argumento. 
- Devolución de llamada de ClearEntities API: borra la entidad de número para permitir que el usuario escriba el siguiente número. Esto ilustra cómo las llamadas API pueden manipular entidades.

### <a name="actions"></a>Acciones

Hemos creado cuatro acciones. 

![](../media/tutorial12_actions.PNG)

- Además de "What number do you want to multiply by 12?" (¿Qué número desea multiplicar por 12?), que es una acción de comunicación, hay tres llamadas API distintas que ilustran los patrones típicos de llamadas API.

- RandomGreeting: es una acción de no espera. Para configurar esto, en el cuadro de diálogo Crear acción, se ha seleccionado el tipo de acción API_LOCAL y luego se ha seleccionado RandomGreeting. 

![](../media/tutorial12_setupapicall.PNG)

El botón para actualizar junto a la API se usa en caso de que se desee detener el bot y realizar cualquier cambio a las API. Al hacer clic en el botón de actualización, se seleccionarían los últimos cambios.

La acción de multiplicación se ha creado así: después de seleccionar API_Local y API, escribimos una entidad ($number) para el primer valor de entrada (num1string) y un valor (12) para el segundo valor de entrada (num2string). Esto proporciona un nivel de direccionamiento indirecto entre el bot y las llamadas API, por lo que la misma devolución de llamada se puede asignar a algunas acciones del sistema y difieren en la forma en que se asignan las acciones.

![](../media/tutorial12_actionmultiply.PNG)

### <a name="train-dialog"></a>Diálogo de entrenamiento

Veamos un cuadro de diálogo de instrucción.

1. Haga clic en Train Dialogs (Diálogos de entrenamiento) y, a continuación, en New Train Dialog (Nuevo diálogo de entrenamiento).
1. Escriba "hi" (hola).
2. Haga clic en Score Action (Acción de puntuación).
3. Haga clic para seleccionar RandomGreeting. Esto ejecutará la llamada a Random Greeting API.
3. Haga clic para seleccionar "What number to do you want to multiply by 12?" (¿Qué número desea multiplicar por 12?).
4. Escriba "8". Después, haga clic en Score Actions (Acciones de puntuación).
4. Seleccione "Multiply $number 12" (Multiplicar $number por 12). Tenga en cuenta el resultado de la multiplicación.
5. Seleccione "Clear Entities" (Borrar entidades).
    - El valor de la entidad `number` se borró.
3. Haga clic para seleccionar "What number to do you want to multiply by 12?" (¿Qué número desea multiplicar por 12?).
4. Haga clic en Done Testing (Prueba completada).

![](../media/tutorial12_dialog.PNG)

Ha visto cómo registrar devoluciones de llamada de API, sus patrones comunes y cómo definir argumentos y asociar valores y entidades en ellos.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Tarjetas parte 1](./13-cards-1.md)
