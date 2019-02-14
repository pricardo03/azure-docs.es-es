---
title: Precisión de la adición de patrones
titleSuffix: Language Understanding - Azure Cognitive Services
description: Obtenga información sobre cómo agregar plantillas de patrón en aplicaciones de Language Understanding (LUIS) para mejorar la precisión de las predicciones.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 01/23/2019
ms.author: diberry
ms.openlocfilehash: 9d3352017723f5beac318d461a537820b6593bef
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/07/2019
ms.locfileid: "55881679"
---
# <a name="how-to-add-patterns-to-improve-prediction-accuracy"></a>Cómo agregar patrones para mejorar la precisión de las predicciones
Después de que una aplicación de LUIS reciba expresiones de punto de conexión, use el [concepto](luis-concept-patterns.md) de patrones para mejorar la precisión de la predicción para las expresiones que revelan un patrón en el orden y la elección de las palabras. En los patrones se usan [entidades](luis-concept-entity-types.md) y sus roles para extraer datos mediante una sintaxis de patrón concreta. 

## <a name="add-template-utterance-to-create-pattern"></a>Agregar expresiones de plantilla para crear el patrón
1. Para abrir la aplicación, seleccione su nombre en la página **My Apps** (Mis aplicaciones) y, después, haga clic en **Patterns** (Patrones) en el panel de la izquierda, en **Improve app performance** (Mejorar el rendimiento de la aplicación).

    ![Captura de pantalla de la lista de patrones](./media/luis-how-to-model-intent-pattern/patterns-1.png)

2. Seleccione la intención correcta para el patrón. 

    ![Selección de la intención](./media/luis-how-to-model-intent-pattern/patterns-2.png)

3. En el cuadro de texto de la plantilla, escriba la expresión de plantilla y presione Entrar. Cuando quiera escribir el nombre de la entidad, debe usar la sintaxis de entidad de patrón correcta. Comience la sintaxis de la entidad con `{`. Se mostrará la lista de entidades. Seleccione la entidad correcta y, después, presione Entrar. 

    ![Captura de pantalla de la entidad del patrón](./media/luis-how-to-model-intent-pattern/patterns-3.png)

    Si la entidad incluye un rol, indíquelo con un signo de dos puntos, `:`, después del nombre de la entidad, como en `{Location:Origin}`. La lista de roles para las entidades se muestra en una lista. Seleccione el rol y, después, presione Entrar. 

    ![Captura de pantalla de la entidad con el rol](./media/luis-how-to-model-intent-pattern/patterns-4.png)

    Después de seleccionar la entidad correcta, termine de escribir el patrón y, luego, presione Entrar. Cuando haya terminado de escribir los patrones, [entrene](luis-how-to-train.md) la aplicación.

    ![Captura de pantalla del patrón indicado con ambos tipos de entidades](./media/luis-how-to-model-intent-pattern/patterns-5.png)

## <a name="search-patterns"></a>Patrones de búsqueda
La búsqueda permite encontrar patrones que contienen un texto especificado.  

1. Haga clic en el icono de la lupa.

    ![Captura de pantalla de la página de patrones con el icono de la herramienta de búsqueda resaltado](./media/luis-how-to-model-intent-pattern/search-icon.png)

    Escriba el texto de búsqueda en el cuadro de búsqueda en la esquina superior derecha de la lista de patrones y presione Entrar. La lista de patrones se actualiza para mostrar solo los que incluyen el texto de búsqueda.

    ![Captura de pantalla de la página de patrones con el texto de búsqueda resaltado en el cuadro de búsqueda](./media/luis-how-to-model-intent-pattern/search-text.png)

    Para cancelar la búsqueda y restaurar la lista completa de patrones, elimine el texto de búsqueda que ha escrito.

<!-- TBD: should I be able to click on the magnifying glass again to close the search box? It doesn't reset the list. -->

## <a name="edit-a-pattern"></a>Editar un patrón
1. Para editar un patrón, seleccione el botón de puntos suspensivos (***...***) en el extremo derecho de la línea de ese patrón y seleccione **Edit** (Editar). 

    ![Captura de pantalla del elemento de menú Editar en la fila del patrón](./media/luis-how-to-model-intent-pattern/patterns-three-dots.png) 

2. Escriba los cambios en el cuadro de texto. Cuando termine, presione Entrar. Cuando haya terminado de modificar los patrones, [entrene](luis-how-to-train.md) la aplicación.

    ![Captura de pantalla de la edición del patrón](./media/luis-how-to-model-intent-pattern/edit-pattern.png)

## <a name="reassign-individual-pattern-to-different-intent"></a>Reasignar un patrón individual a otra intención

Para reasignar un solo patrón a otra intención, haga clic en el cuadro de lista de la intención situado a la derecha del texto del patrón y seleccione otra intención.

![Captura de pantalla de la reasignación de un patrón individual a otra intención](./media/luis-how-to-model-intent-pattern/reassign-individual-pattern.png)

## <a name="reassign-several-patterns-to-different-intent"></a>Reasignar varios patrones a otra intención

Para reasignar varios patrones a otra intención, active la casilla situada a la izquierda de cada patrón o active la casilla superior. La opción **Reassign intent** (Reasignar intención) se muestra en la barra de herramientas. Seleccione la intención correcta para los patrones. 

![Captura de pantalla de la reasignación de varios patrones a otra intención](./media/luis-how-to-model-intent-pattern/reassign-many-patterns.png)

## <a name="delete-a-single-pattern"></a>Eliminar un único patrón

1. Para eliminar un patrón, seleccione el botón de puntos suspensivos (***...***) en el extremo derecho de la línea de ese patrón y seleccione **Delete** (Eliminar). 

    ![Captura de pantalla de eliminación de la expresión](./media/luis-how-to-model-intent-pattern/patterns-three-dots-ddl.png)

2. Haga clic en **Ok** (Aceptar) para confirmar la eliminación.

    ![Captura de pantalla de confirmación de eliminación](./media/luis-how-to-model-intent-pattern/confirm-delete.png)

## <a name="delete-several-patterns"></a>Eliminar varios patrones

1. Para eliminar varios patrones, active la casilla situada a la izquierda de cada patrón o active la casilla superior. La opción **Delete patterns(s)** (Eliminar patrones) se muestra en la barra de herramientas. Haga clic en **Delete patterns(s)** (Eliminar patrones).  

    ![Captura de pantalla de la eliminación de varios patrones](./media/luis-how-to-model-intent-pattern/delete-many-patterns.png)

2. Aparece el cuadro de diálogo de confirmación **Delete patterns** (Eliminar patrones). Haga clic en **Ok** (Aceptar) para finalizar la eliminación.

    ![Captura de pantalla del cuadro de confirmación de la eliminación de varios patrones](./media/luis-how-to-model-intent-pattern/delete-many-patterns-confirmation.png)

## <a name="filter-pattern-list-by-entity"></a>Filtrar la lista de patrones por entidad

Para filtrar la lista de patrones por una entidad específica, haga clic en **Entity filters** (Filtros de entidad) en la barra de herramientas situada sobre los patrones. 

![Captura de pantalla del filtrado de modelos por entidad](./media/luis-how-to-model-intent-pattern/filter-entities-1.png)

Después de aplicar el filtro, el nombre de la entidad aparece debajo de la barra de herramientas. 

## <a name="filter-pattern-list-by-intent"></a>Filtrar la lista de patrones por intención

Para filtrar la lista de patrones por una intención específica, haga clic en **Intent filters** (Filtros de intención) en la barra de herramientas situada encima de los patrones. 

![Captura de pantalla del filtrado de patrones por intención](./media/luis-how-to-model-intent-pattern/filter-intents-1.png)

Después de aplicar el filtro, el nombre de la intención aparece debajo de la barra de herramientas. 

## <a name="remove-entity-or-intent-filter"></a>Quitar el filtro de intención o entidad
Cuando se filtra la lista de patrones, el nombre de la intención o entidad aparece debajo de la barra de herramientas. Para quitar el filtro, haga clic en el nombre.

![Captura de pantalla de la eliminación de la entidad del filtro](./media/luis-how-to-model-intent-pattern/filter-entities-2.png)

Se quita el filtro y se muestran todos los patrones. 

## <a name="add-pattern-from-existing-utterance-on-intent-or-entity-page"></a>Adición de un patrón a partir de una de expresión existente en la página de la intención o entidad
Puede crear un patrón a partir de una expresión existente en la página **Intent** (Intención) o **Entity** (Entidad). Todas las expresiones de cualquier página de intención o entidad se muestran en una lista en la que la columna de la derecha proporciona acceso a las opciones de nivel de expresión como **Edit** (Editar), **Delete** (Eliminar) y **Add as pattern** (Agregar como patrón).

1. En la fila seleccionada de la expresión, seleccione el botón de puntos suspensivos (***...***) a la derecha de la expresión y seleccione **Add as pattern** (Agregar como patrón).

    [![Captura de pantalla de la tabla de expresiones con la opción Agregar patrón resaltada en el menú de opciones](./media/luis-how-to-model-intent-pattern/add-pattern-from-utterance.png "Screenshot of utterances table with Add pattern highlighted in options menu")](./media/luis-how-to-model-intent-pattern/add-pattern-from-utterance.png)

2. Modifique el patrón de acuerdo con las [reglas de sintaxis](luis-concept-patterns.md#pattern-syntax). Si la expresión seleccionada está etiquetada con entidades, esas entidades ya están en el patrón con la sintaxis correcta.

    ![Captura de pantalla de los patrones filtrados por entidad](./media/luis-how-to-model-intent-pattern/confirm-patterns-modal.png)

## <a name="train-your-app-after-changing-model-with-patterns"></a>Entrenar la aplicación después de cambiar el modelo con patrones
Después de agregar, editar o quitar un patrón, [entrene](luis-how-to-train.md) y [publique](luis-how-to-publish-app.md) la aplicación para que los cambios se apliquen a las consultas de punto de conexión. 

## <a name="next-steps"></a>Pasos siguientes

* Obtenga información sobre cómo [crear un patrón](luis-tutorial-pattern.md) con pattern.any y roles.
* Obtenga información sobre cómo [entrenar](luis-how-to-train.md) la aplicación.
