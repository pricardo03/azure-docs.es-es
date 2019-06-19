---
title: 'Uso de la devolución de llamada de detección de entidades con un modelo de Conversation Learner: Microsoft Cognitive Services | Microsoft Docs'
titleSuffix: Azure
description: Obtenga información sobre cómo utilizar la devolución de llamada de detección de entidades con un modelo de Conversation Learner.
services: cognitive-services
author: nitinme
manager: nolachar
ms.service: cognitive-services
ms.subservice: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: nitinme
ms.openlocfilehash: ed254ea3db1c8f0e680cba71185e9b4505d8f200
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "66387649"
---
# <a name="how-to-use-entity-detection-callback"></a>Uso de la devolución de llamada de detección de entidades

En este tutorial se explica la devolución de llamada de detección de entidades y también se ilustra un patrón común para resolver entidades.

## <a name="video"></a>Vídeo

[![Vista preliminar del tutorial de devolución de llamada de detección de entidad](https://aka.ms/cl_Tutorial_v3_EntityDetection_Preview)](https://aka.ms/cl_Tutorial_v3_EntityDetection)

## <a name="requirements"></a>Requisitos
Para poder realizar este tutorial debe ejecutar el bot `tutorialEntityDetectionCallback`.

    npm run tutorial-entity-detection

## <a name="details"></a>Detalles
Las devoluciones de llamada de detección de entidad habilitan la modificación del comportamiento de reconocimiento de entidades y la manipulación de entidades a través del código. Esta funcionalidad se mostrará mediante un tutorial del patrón típico de diseño de devolución de llamada de detección de entidad. Por ejemplo, para que "la gran manzana" se resuelva como "nueva york".

## <a name="steps"></a>Pasos

### <a name="create-the-model"></a>Crear el modelo

1. En la interfaz de usuario web, haga clic en "New Model" (Modelo nuevo).
2. En el campo "Name" (Nombre), escriba "DevoluciónLlamadaDetecciónEntidad" y pulse Entrar.
3. Haga clic en el botón "Create" (Crear).

En este ejemplo se necesitan tres entidades, así que vamos a crear las tres.

### <a name="create-the-custom-trained-entity"></a>Creación de la entidad entrenada personalizada

1. En el panel izquierdo, haga clic en "Entities" (Entidades) y, a continuación, en el botón "New Entity" (Nueva entidad).
2. Seleccione "Custom Trained" (Entrenada de forma personalizada) para "Entity Type" (Tipo de entidad).
3. Escriba "Ciudad" para "Entity Name" (Nombre de entidad).
4. Haga clic en el botón "Create" (Crear).

### <a name="create-the-first-programmatic-entity"></a>Creación de la primera entidad mediante programación

1. Haga clic en el botón "New Entity" (Nueva entidad).
2. Select "Programmatic" (Mediante programación) para "Entity Type" (Tipo de entidad).
3. Escriba "CiudadDesconocida" para "Entity Name" (Nombre de entidad).
4. Haga clic en el botón "Create" (Crear).

### <a name="create-the-first-programmatic-entity"></a>Creación de la primera entidad mediante programación

1. Haga clic en el botón "New Entity" (Nueva entidad).
2. Select "Programmatic" (Mediante programación) para "Entity Type" (Tipo de entidad).
3. Escriba "CiudadResuelta" para "Entity Name" (Nombre de entidad).
4. Haga clic en el botón "Create" (Crear).

Ahora vamos a crear tres acciones.

### <a name="action-creation"></a>Creación de acciones

1. En el panel izquierdo, haga clic en "Actions" (Acciones) y, a continuación, en el botón "New Action" (Nueva acción).
2. En el campo "Bot's response..." (Respuesta del bot), escriba "¿Qué ciudad está buscando?"
3. En el campo "Expected Entity in User reply..." (Entidad esperada en la respuesta del usuario...), escriba "Ciudad".
4. En el campo "Disqualifying Entities" (Entidades descalificadoras), escriba "Ciudad".
5. Haga clic en el botón "Create" (Crear).
6. Haga clic en el botón "New Action" (Nueva acción).
7. En el campo "Bot's response..." (Respuesta del bot), escriba "¿Qué ciudad está buscando?"
8. En el campo "Expected Entity in User reply..." (Entidad esperada en la respuesta del usuario...), escriba "No conozco esta ciudad".
9. Haga clic en el botón "Create" (Crear).
10. Haga clic en el botón "New Action" (Nueva acción).
11. En el campo "Bot's response..." (Respuesta del bot), escriba "$Ciudad-Resuelta está muy bien".
12. Haga clic en el botón "Create" (Crear).

Este es el código de devolución de llamada:

![](../media/tutorial10_callbackcode.PNG)

### <a name="train-the-model"></a>Entrenamiento del modelo

1. En el panel izquierdo, haga clic en "Train Dialogs" (Diálogos de entrenamiento) y, a continuación, en el botón "New Train Dialog" (Nuevo diálogo de entrenamiento).
2. En el panel del chat, donde se indica "Type your message..." (Escriba su mensaje…), escriba "hola".
3. Haga clic en el botón "Score Actions" (Puntuar acciones).
4. Seleccione la respuesta, "¿Qué ciudad está buscando?"
5. En el panel del chat, donde se indica "Type your message..." (Escriba su mensaje…), escriba "la gran manzana".
6. Haga clic en el botón "Score Actions" (Puntuar acciones).
    - Al hacer clic en el botón se desencadena la devolución de llamada de detección de entidad.
    - El código de devolución de llamada establece el valor de la entidad "CiudadResuelta" correctamente como "nueva york"
7. Seleccione la respuesta, "nueva york está muy bien".

Este patrón es típico de muchos escenarios de bot. Las entidades extraídas y las expresiones del usuario se suministran a la lógica de negocios, y esta lógica transforma la expresión a la forma canónica, que posteriormente se guarda en las entidades mediante programación, para los posteriores turnos del diálogo.

![](../media/tutorial10_bigapple.PNG)

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Devoluciones de llamada de sesión](./13-session-callbacks.md)
