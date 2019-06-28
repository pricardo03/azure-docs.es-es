---
title: Revisión de expresiones del usuario
titleSuffix: Language Understanding - Azure Cognitive Services
description: El aprendizaje activo captura las consultas de punto de conexión y selecciona las expresiones de punto de conexión del usuario de las que no está seguro. Revise estas expresiones para seleccionar la intención y marque las entidades para estas expresiones de lectura. Acepte estos cambios en sus expresiones de ejemplo, entrene y publique. LUIS identificará después las expresiones con mayor precisión.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 03/25/2019
ms.author: diberry
ms.openlocfilehash: 8fac360682ef11c438cdec333fac21d6f8cfc117
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "60195171"
---
# <a name="how-to-review-endpoint-utterances-in-luis-portal-for-active-learning"></a>Revisión de las expresiones de punto de conexión en el portal de LUIS para aprendizaje activo

El [aprendizaje activo](luis-concept-review-endpoint-utterances.md) captura las consultas de punto de conexión y selecciona las expresiones de punto de conexión del usuario de las que no está seguro. Revise estas expresiones para seleccionar la intención y marque las entidades para estas expresiones de lectura. Acepte estos cambios en sus expresiones de ejemplo, entrene y publique. LUIS identificará después las expresiones con mayor precisión.


## <a name="enable-active-learning"></a>Habilitación del aprendizaje activo

Para habilitar el aprendizaje activo, registre las consultas de usuario. Esto se consigue estableciendo la [consulta de punto de conexión](luis-get-started-create-app.md#query-the-endpoint-with-a-different-utterance) con el parámetro y el valor de la cadena de consulta `log=true`.

## <a name="disable-active-learning"></a>Deshabilitación del aprendizaje activo

Para deshabilitar el aprendizaje activo, no registre las consultas de usuario. Esto se consigue estableciendo la [consulta de punto de conexión](luis-get-started-create-app.md#query-the-endpoint-with-a-different-utterance) con el parámetro y el valor de la cadena de consulta `log=false`.

## <a name="filter-utterances"></a>Filtrar expresiones

1. Para abrir la aplicación (por ejemplo, TravelAgent), seleccione su nombre en la página **My Apps** (Mis aplicaciones) y después haga clic en **Build** (Generar) en la barra superior.

1. Debajo de **Improve app performance** (Mejorar el rendimiento de la aplicación), seleccione **Review endpoint utterances** (Revisar las expresiones de punto de conexión).

1. En la página **Review endpoint utterances** (Revisar las expresiones de punto de conexión), realice la selección en el cuadro de texto **Filter list by intent or entity** (Filtrar la lista por intención o entidad). En esta lista desplegable se incluyen todas las intenciones bajo en **INTENTS** (INTENCIONES) y todas las entidades bajo **ENTITIES** (ENTIDADES).

    ![Filtro de expresiones](./media/label-suggested-utterances/filter.png)

1. Seleccione una categoría (intenciones o entidades) en la lista desplegable y revise las expresiones.

    ![Expresiones de intención](./media/label-suggested-utterances/intent-utterances.png)

## <a name="label-entities"></a>Etiquetar entidades
LUIS reemplaza los tokens de entidad (las palabras) con nombres de entidad que se resaltan en color azul. Si una expresión tiene entidades sin etiquetar, etiquételas en la expresión. 

1. Seleccione la palabra en la expresión. 

1. Seleccione una entidad de la lista.

    ![Etiqueta de entidad](./media/label-suggested-utterances/label-entity.png)

## <a name="align-single-utterance"></a>Alinear una expresión

Cada expresión tiene una intención sugerida que se muestra en la columna **Aligned intent** (Intención alineada). 

1. Si está de acuerdo con esa sugerencia, haga clic en la marca de verificación.

    ![Mantener la intención alineada](./media/label-suggested-utterances/align-intent-check.png)

1. Si no está de acuerdo con la sugerencia, seleccione la intención correcta en la lista desplegable de intenciones alineadas y después haga clic en la marca de verificación situada la derecha de la intención alineada. 

    ![Alinear intención](./media/label-suggested-utterances/align-intent.png)

1. Después de hacer clic en la marca de verificación, la expresión se quita de la lista. 

## <a name="align-several-utterances"></a>Alinear varias expresiones

Para alinear varias expresiones, active la casilla situada a la izquierda de las expresiones y, después, haga clic en el botón**Add selected** (Agregar selección). 

![Alinear varias](./media/label-suggested-utterances/add-selected.png)

## <a name="verify-aligned-intent"></a>Comprobar la intención alineada

Puede comprobar si la expresión se ha alineado con la intención correcta si va a la página **Intents** (Intenciones), selecciona el nombre de la intención y revisa las expresiones. La expresión de **Review endpoint utterances** (Revisar las expresiones de punto de conexión) está en la lista.

## <a name="delete-utterance"></a>Eliminar la expresión

Cada expresión se puede eliminar de la lista de revisión. Una vez eliminada, no volverá a aparecer en la lista. Esto sucede incluso si el usuario escribe la misma expresión desde el punto de conexión. 

Si no está seguro de si la expresión se debe eliminar, muévala a la intención None, o bien cree una intención como "varios" y mueva la expresión a esa intención. 

## <a name="delete-several-utterances"></a>Eliminar varias expresiones

Para eliminar varias expresiones, seleccione cada elemento y haga clic en la Papelera situada a la derecha del botón **Add selected** (Agregar selección).

![Eliminar varias](./media/label-suggested-utterances/delete-several.png)


## <a name="next-steps"></a>Pasos siguientes

Para probar cómo mejora el rendimiento después de etiquetar las expresiones sugeridas, puede acceder a la consola de prueba si hace clic en **Test** (Prueba) en el panel superior. Para obtener instrucciones sobre cómo probar la aplicación mediante la consola de prueba, vea [Train and test your app](luis-interactive-test.md) (Entrenar y probar la aplicación).
