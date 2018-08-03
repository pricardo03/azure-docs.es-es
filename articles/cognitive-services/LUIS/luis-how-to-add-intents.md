---
title: Adición de intenciones en aplicaciones de LUIS | Microsoft Docs
description: Use Language Understanding (LUIS) para agregar intenciones con el fin de que las aplicaciones entiendan las solicitudes del usuario y reaccionen a estas de la forma correcta.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.component: language-understanding
ms.topic: article
ms.date: 05/07/2018
ms.author: diberry
ms.service: cognitive-services
ms.openlocfilehash: 0ebf15ea49467674ab3c56aa7983131593cf5c9a
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/24/2018
ms.locfileid: "39225893"
---
# <a name="manage-intents"></a>Administrar intenciones 
Agregue [intenciones](luis-concept-intent.md) a la aplicación de LUIS para identificar los grupos de preguntas o comandos que tienen las mismas intenciones. 

Las intenciones se agregan y administran desde la página **Intents** (Intenciones), disponible en **Intents** (Intenciones) en el panel de la izquierda de LUIS. 

En el procedimiento siguiente se muestra cómo agregar la intención "Bookflight" de la aplicación TravelAgent.

## <a name="add-intent"></a>Agregar intención

1. Para abrir la aplicación (por ejemplo, TravelAgent) haga clic en su nombre en la página **My Apps** (Mis aplicaciones) y, después, haga clic en **Intents** (Intenciones) en el panel de la izquierda. 
2. En la página **Intents** (Intenciones), haga clic en **Create new intent** (Crear nueva intención).

    ![Lista de intenciones](./media/luis-how-to-add-intents/IntentsList.png)
3. En el cuadro de diálogo **Create new intent** (Crear nueva intención), escriba el nombre de intención "BookFlight" y haga clic en **Done** (Listo).

    ![Agregar intención](./media/luis-how-to-add-intents/Addintent-dialogbox.png)

    En la página de detalles de la intención recién agregada, [agregue expresiones](#add-an-utterance-on-intent-page).

## <a name="rename-intent"></a>Cambiar el nombre de la intención

1. En la página **Intent** (Intención), haga clic en el icono de cambio de nombre ![Rename Intent](./media/luis-how-to-add-intents/Rename-Intent-btn.png) (Cambiar nombre de la intención) situado junto al nombre de intención. 

2. En la página **Intent** (Intención), el nombre de la intención actual se muestra en un cuadro de diálogo. Cambie el nombre de intención y presione Entrar. El nombre nuevo se guarda y se muestra en la página de la intención.

    ![Editar intención](./media/luis-how-to-add-intents/EditIntent-dialogbox.png)

## <a name="delete-intent"></a>Eliminar intención
Al eliminar una intención que no sea None, puede elegir agregar todas las expresiones a la intención None. Esto es útil si tiene que mover las expresiones en lugar de eliminarlas.   

1. En la página **Intent** (Intención), haga clic en el botón **Delete Intent** (Eliminar intención) situado a la derecha del nombre de la intención. 

    ![Botón Eliminar intención](./media/luis-how-to-add-intents/DeleteIntent.png)

2. Haga clic en el botón "Ok" (Aceptar) en el cuadro de diálogo de confirmación.

<!--
    TBD: waiting for confirmation about which delete dialog is going to be in //BUILD

    ![Delete Intent Dialog](./media/luis-how-to-add-intents/DeleteIntent-Confirmation.png)
-->


## <a name="add-an-utterance-on-intent-page"></a>Agregar una expresión en la página de la intención

En la página de la intención, escriba una expresión pertinente que espere de los usuarios, como `book 2 adult business tickets to Paris tomorrow on Air France` en el cuadro de texto situado debajo del nombre de la intención y, después, presione Entrar. 
 
>[!NOTE]
>LUIS convierte todas las expresiones a minúsculas.

![Captura de pantalla de la página de detalles de las intenciones, con la expresión resaltada](./media/luis-how-to-add-intents/add-new-utterance-to-intent.png) 

Las expresiones se agregan a la lista de expresiones de la intención actual. Después de agregar una expresión, [etiquete todas las entidades](luis-how-to-add-example-utterances.md) dentro de las expresiones y [entrene](luis-how-to-train.md) la aplicación. 

## <a name="create-a-pattern-from-an-utterance"></a>Crear un patrón a partir de una expresión
Vea [Add pattern from existing utterance on intent or entity page](luis-how-to-model-intent-pattern.md#add-pattern-from-existing-utterance-on-intent-or-entity-page) (Adición de un patrón a partir de una de expresión existente en la página de la intención o entidad).

## <a name="edit-an-utterance-on-intent-page"></a>Editar una expresión en la página de la intención

Para editar una expresión, seleccione el botón de puntos suspensivos (***...***) en el extremo derecho de la línea de esa expresión y seleccione **Edit** (Editar). Modifique el texto y, después, presione Entrar en el teclado.

![Captura de pantalla de la página de detalles de las intenciones con el botón de puntos suspensivos resaltado](./media/luis-how-to-add-intents/edit-utterance.png) 

## <a name="reassign-utterances-on-intent-page"></a>Reasignar expresiones en la página de la intención
Puede cambiar la intención de una o más expresiones si las reasigna a otra intención. 

Para reasignar una sola expresión a otra intención, en el extremo derecho de la fila de la expresión, seleccione el nombre correcto de la intención bajo la columna **Labeled intent** (Intención etiquetada). La expresión se quita de la lista de expresiones de la intención actual. 

![Captura de pantalla de la página de la intención BookFlight con la expresión de una intención seleccionada bajo la columna Labeled intent (Intención etiquetada).](./media/luis-how-to-add-intents/reassign-1-utterance.png)

Para cambiar la intención de varias expresiones, active las casillas situadas a la izquierda de las expresiones y, después, haga clic en **Reassign intent** (Reasignar intención). Seleccione la intención correcta de la lista.

![Captura de pantalla de la página de la intención BookFlight con una expresión activada y el botón Reassign intent (Reasignar intención) resaltado](./media/luis-how-to-add-intents/delete-several-utterances.png) 

## <a name="delete-utterances-on-intent-page"></a>Eliminar expresiones en la página de la intención

Para editar una expresión, seleccione el botón de puntos suspensivos (***...***) en el extremo derecho de la línea de esa expresión y seleccione **Delete** (Eliminar). La expresión se quita de la lista y de la aplicación de LUIS.

![Captura de pantalla de la página de detalles de la intención, con la opción Delete (Eliminar) resaltada](./media/luis-how-to-add-intents/delete-utterance-ddl.png)

Para eliminar varias expresiones:

1. Active las casillas situadas a la izquierda de la expresiones y, después, haga clic en **Delete utterances(s)** (Eliminar expresiones). 

    ![Captura de pantalla de la página de detalles de la intención, con las expresiones activadas y el botón Delete utterances(s) (Eliminar expresiones) resaltado](./media/luis-how-to-add-intents/delete-several-utterances.png)

2. Haga clic en **Done** (Listo) en el cuadro de diálogo emergente **Delete utterances?** (¿Eliminar las expresiones?).

## <a name="search-in-utterances-on-intent-page"></a>Buscar expresiones en la página de la intención
En una intención, puede buscar expresiones que contengan texto (palabras o frases). Por ejemplo, es posible que observe un error que implique una palabra determinada y que quiera buscar todos los ejemplos en los que se incluya esa palabra concreta. 

1. Haga clic en el icono de lupa en la barra de herramientas.

    ![Captura de pantalla de la página de intenciones, con el icono de búsqueda de la lupa resaltado](./media/luis-how-to-add-intents/magnifying-glass.png)

2. Aparece un cuadro de texto de búsqueda. Escriba la palabra o frase en el cuadro de búsqueda en la esquina superior derecha de la lista de expresiones. La lista de expresiones se actualiza, para mostrar solo las que incluyen el texto de búsqueda. 

    ![Captura de pantalla de la página de intenciones, con el cuadro de texto de búsqueda resaltado](./media/luis-how-to-add-intents/search-textbox.png)

    Para cancelar la búsqueda y restaurar la lista completa de expresiones, elimine el texto de búsqueda que ha escrito. Para cerrar el cuadro de texto de búsqueda, vuelva a hacer clic en el icono de lupa en la barra de herramientas.

## <a name="prediction-discrepancy-errors-on-intent-page"></a>Errores de discrepancia de predicción en la página de la intención
Es posible que en una intención, una expresión tenga una discrepancia entre la intención seleccionada y la puntuación de predicción. En LUIS, esta discrepancia se indica con un cuadro de color rojo alrededor de la puntuación. 

![Captura de pantalla de la página de la intención BookFlight, con la puntuación de discrepancia de predicción resaltada](./media/luis-how-to-add-intents/score-discrepancy.png) 

## <a name="filter-by-intent-prediction-discrepancy-errors-on-intent-page"></a>Filtrar por errores de discrepancia de predicción de intención en la página de la intención
Para filtrar la lista de expresiones solo a las que tengan una discrepancia de predicción de intención, cambie de **Show All** (Mostrar todo) a **Errors only** (Solo errores) en la barra de herramientas. 

## <a name="filter-by-entity-type-on-intent-page"></a>Filtrar por tipo de entidad en la página de la intención
Use la lista desplegable **Entity filters** (Filtros de entidad) de la barra de herramientas para filtrar las expresiones por entidad. 

![Captura de pantalla de la página de intenciones, con el filtro de tipo de entidad resaltado](./media/luis-how-to-add-intents/filter-by-entities.png) 

Para quitar el filtro, seleccione el cuadro de filtro de color azul con esa palabra o frase en la barra de herramientas.  
<!-- TBD: waiting for ux fix - bug in ux of prebuit entity number -- when filtering by it, it doesn't show the list -->

## <a name="switch-to-token-view-on-intent-page"></a>Cambiar a la vista de tokens en la página de la intención
Cambie a **Tokens View** (Vista de tokens) para ver los tokens en lugar de los nombres de tipo de entidad. En el teclado, también puede presionar **Control+E** para cambiar a la vista. 

![Captura de pantalla de la página de la intención Bookflight con la vista de tokens resaltada](./media/luis-how-to-add-intents/toggle-tokens-view.png)

## <a name="train-your-app-after-changing-model-with-intents"></a>Entrenar la aplicación después de cambiar el modelo con intenciones
Después de agregar, editar o quitar intenciones, [entrene](luis-how-to-train.md) y [publique](luis-how-to-publish-app.md) la aplicación para que los cambios se apliquen a las consultas de punto de conexión. 

## <a name="next-steps"></a>Pasos siguientes

Después de agregar intenciones a la aplicación, la tarea siguiente consiste en empezar a agregar [expresiones de ejemplo](luis-how-to-add-example-utterances.md) para las intenciones que se han agregado. 
