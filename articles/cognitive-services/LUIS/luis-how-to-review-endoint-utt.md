---
title: Revisión de las expresiones de punto de conexión para Language Understanding (LUIS)
titleSuffix: Azure Cognitive Services
description: La característica innovadora de LUIS es el concepto de aprendizaje activo. Una vez que LUIS tiene consultas de punto de conexión, el aprendizaje activo mejora la calidad de los resultados mediante la selección de expresiones de las que no está seguro. Si etiqueta estas expresiones, las entrena y las publica, LUIS las identifica con más precisión.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 09/06/2018
ms.author: diberry
ms.openlocfilehash: a5e0dabe251d14389923df3efe41f6ba80f41bdd
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "47030802"
---
# <a name="review-endpoint-utterances"></a>Revisar las expresiones de punto de conexión

La característica innovadora de LUIS es el [concepto](luis-concept-review-endpoint-utterances.md) de aprendizaje activo. Una vez que LUIS tiene consultas de punto de conexión, usa el aprendizaje activo para mejorar la calidad de los resultados. En el proceso de aprendizaje activo, LUIS examina la expresiones de punto de conexión y selecciona aquellas de las que no está seguro. Si etiqueta estas expresiones, las entrena y las publica, LUIS las identifica con más precisión. 

## <a name="filter-utterances"></a>Filtrar expresiones
1. Para abrir la aplicación (por ejemplo, TravelAgent), seleccione su nombre en la página **My Apps** (Mis aplicaciones) y después haga clic en **Build** (Generar) en la barra superior.

2. Debajo de **Improve app performance** (Mejorar el rendimiento de la aplicación), seleccione **Review endpoint utterances** (Revisar las expresiones de punto de conexión).

3. En la página **Review endpoint utterances** (Revisar las expresiones de punto de conexión), realice la selección en el cuadro de texto **Filter list by intent or entity** (Filtrar la lista por intención o entidad). En esta lista desplegable se incluyen todas las intenciones bajo en **INTENTS** (INTENCIONES) y todas las entidades bajo **ENTITIES** (ENTIDADES).

    ![Filtro de expresiones](./media/label-suggested-utterances/filter.png)

4. Seleccione una categoría (intenciones o entidades) en la lista desplegable y revise las expresiones.

    ![Expresiones de intención](./media/label-suggested-utterances/intent-utterances.png)

## <a name="label-entities"></a>Etiquetar entidades
LUIS reemplaza los tokens de entidad (las palabras) con nombres de entidad que se resaltan en color azul. Si una expresión tiene entidades sin etiquetar, etiquételas en la expresión. 

1. Seleccione la palabra en la expresión. 

2. Seleccione una entidad de la lista.

    ![Etiqueta de entidad](./media/label-suggested-utterances/label-entity.png)

## <a name="align-single-utterance"></a>Alinear una expresión

Cada expresión tiene una intención sugerida que se muestra en la columna **Aligned intent** (Intención alineada). 

1. Si está de acuerdo con esa sugerencia, haga clic en la marca de verificación.

    ![Mantener la intención alineada](./media/label-suggested-utterances/align-intent-check.png)

2. Si no está de acuerdo con la sugerencia, seleccione la intención correcta en la lista desplegable de intenciones alineadas y después haga clic en la marca de verificación situada la derecha de la intención alineada. 

    ![Alinear intención](./media/label-suggested-utterances/align-intent.png)

3. Después de hacer clic en la marca de verificación, la expresión se quita de la lista. 

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
