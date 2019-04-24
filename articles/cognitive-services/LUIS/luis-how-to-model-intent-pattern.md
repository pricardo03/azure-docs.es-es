---
title: Precisión de la adición de patrones
titleSuffix: Language Understanding - Azure Cognitive Services
description: Agregar plantillas de patrón para mejorar la precisión de predicción en las aplicaciones de Language Understanding (LUIS).
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 04/01/2019
ms.author: diberry
ms.openlocfilehash: 202b9632b7a7faaf955874a0300edbe5134b7fa1
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60196702"
---
# <a name="how-to-add-patterns-to-improve-prediction-accuracy"></a>Adición de patrones para mejorar la precisión de predicción
Una vez que una aplicación de LUIS recibe grabaciones de voz de punto de conexión, utilice un [patrón](luis-concept-patterns.md) para mejorar la precisión de predicción para declaraciones que revelan ningún patrón en el orden de las palabras y opciones de word. Patrones de utilizar específico [sintaxis](luis-concept-patterns.md#pattern-syntax) para indicar la ubicación de: [entidades](luis-concept-entity-types.md), entidad [roles](luis-concept-roles.md)y texto opcional.

## <a name="add-template-utterance-to-create-pattern"></a>Agregar expresiones de plantilla para crear el patrón
1. Para abrir la aplicación, seleccione su nombre en la página **My Apps** (Mis aplicaciones) y, después, haga clic en **Patterns** (Patrones) en el panel de la izquierda, en **Improve app performance** (Mejorar el rendimiento de la aplicación).

    ![Captura de pantalla de la lista de patrones](./media/luis-how-to-model-intent-pattern/patterns-1.png)

2. Seleccione la intención correcta para el patrón. 

    ![Selección de la intención](./media/luis-how-to-model-intent-pattern/patterns-2.png)

3. En el cuadro de texto de la plantilla, escriba la expresión de plantilla y presione Entrar. Cuando quiera escribir el nombre de la entidad, debe usar la sintaxis de entidad de patrón correcta. Comience la sintaxis de la entidad con `{`. Se mostrará la lista de entidades. Seleccione la entidad correcta y, después, presione Entrar. 

    ![Captura de pantalla de la entidad del patrón](./media/luis-how-to-model-intent-pattern/patterns-3.png)

    Si la entidad incluye un [rol](luis-concept-roles.md), indicar el rol con un signo de dos puntos, `:`, cuando el nombre de la entidad, como `{Location:Origin}`. La lista de roles para las entidades se muestra en una lista. Seleccione el rol y, después, presione Entrar. 

    ![Captura de pantalla de la entidad con el rol](./media/luis-how-to-model-intent-pattern/patterns-4.png)

    Después de seleccionar la entidad correcta, termine de escribir el patrón y, luego, presione Entrar. Cuando haya terminado de escribir los patrones, [entrene](luis-how-to-train.md) la aplicación.

    ![Captura de pantalla del patrón indicado con ambos tipos de entidades](./media/luis-how-to-model-intent-pattern/patterns-5.png)

## <a name="train-your-app-after-changing-model-with-patterns"></a>Entrenar la aplicación después de cambiar el modelo con patrones
Después de agregar, editar o quitar un patrón, [entrene](luis-how-to-train.md) y [publique](luis-how-to-publish-app.md) la aplicación para que los cambios se apliquen a las consultas de punto de conexión. 

<a name="search-patterns"></a>
<a name="edit-a-pattern"></a>
<a name="reassign-individual-pattern-to-different-intent"></a>
<a name="reassign-several-patterns-to-different-intent"></a>
<a name="delete-a-single-pattern"></a>
<a name="delete-several-patterns"></a>
<a name="filter-pattern-list-by-entity"></a>
<a name="filter-pattern-list-by-intent"></a>
<a name="remove-entity-or-intent-filter"></a>
<a name="add-pattern-from-existing-utterance-on-intent-or-entity-page"></a>

## <a name="use-contextual-toolbar"></a>Utilice la barra de herramientas contextual

La barra de herramientas contextual encima de la lista de patrones permite:

* Búsqueda de patrones
* Editar un patrón
* Reasignar un patrón individual a otra intención
* Reasignar varios patrones a otra intención
* Eliminar un patrón único
* Eliminar varios patrones
* Filtrar la lista de patrones por entidad
* Filtro de lista patrón por intención
* Quitar el filtro de intención o entidad
* Adición de un patrón a partir de una de expresión existente en la página de la intención o entidad

## <a name="next-steps"></a>Pasos siguientes

* Obtenga información sobre cómo [crear un patrón](luis-tutorial-pattern.md) con un pattern.any y funciones con el tutorial.
* Obtenga información sobre cómo [entrenar](luis-how-to-train.md) la aplicación.
