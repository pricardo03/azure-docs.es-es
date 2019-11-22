---
title: Etiquetado de una entidad en una expresión de ejemplo
titleSuffix: Azure Cognitive Services
description: Obtenga información sobre cómo etiquetar una entidad de aprendizaje automático con subcomponentes en una expresión de ejemplo en una página de detalles de la intención del portal de LUIS.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: quickstart
ms.date: 11/15/2019
ms.author: diberry
ms.openlocfilehash: 87b7375b2da0effbc18fff6a7e9d67383c93a403
ms.sourcegitcommit: 2d3740e2670ff193f3e031c1e22dcd9e072d3ad9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/16/2019
ms.locfileid: "74134124"
---
# <a name="label-machine-learned-entity-in-an-example-utterance"></a>Etiquetado de una entidad de aprendizaje automático en una expresión de ejemplo

Etiquetar una entidad en una expresión de ejemplo muestra que LUIS tiene un ejemplo de la entidad y dónde la entidad puede aparecer en la expresión. 

## <a name="labeling-machine-learned-entity"></a>Etiquetado de una entidad de aprendizaje automático

Analice en la frase `hi, please I want a cheese pizza in 20 minutes`. 

1. Seleccione el texto situado más a la izquierda y, a continuación, seleccione el texto situado más a la derecha de la entidad. El _pedido completo_ aparece etiquetado en la siguiente imagen.

    > [!div class="mx-imgBorder"]
    > ![Etiqueta completa de la entidad de aprendizaje automático](media/label-utterances/example-1-label-machine-learned-entity-complete-order.png)

1. Seleccione la entidad en la ventana emergente. La entidad completa etiquetada de pedido de pizza incluye todas las palabras (de izquierda a derecha en inglés) que están etiquetadas. 

> [!TIP]
> Las entidades disponibles en la ventana emergente son relativas al contexto en el que aparece el texto. Por ejemplo, si tiene una entidad de aprendizaje automático de 5 niveles y selecciona texto en el tercer nivel (indicado por un nombre de entidad etiquetada en la expresión de ejemplo), las entidades disponibles en la ventana emergente se limitan al contexto de los subcomponentes del tercer nivel (subcomponentes de cuarto nivel). 

## <a name="review-labeled-text"></a>Revisión del texto etiquetado

Después de etiquetar, revise la expresión de ejemplo. LUIS aplica el modelo actual a la expresión de ejemplo después del etiquetado. La línea sólida indica que el texto se ha etiquetado. 

> [!div class="mx-imgBorder"]
> ![Entidad de aprendizaje automático etiquetada por completo](media/label-utterances/example-1-label-machine-learned-entity-complete-order-labeled.png)

## <a name="when-to-train"></a>Cuándo realizar el entrenamiento

Si el modelo actual debe ser compatible con la entidad etiquetada, pero la expresión de ejemplo continúa mostrando el texto como predicho, pero no etiquetado, entrene la aplicación.  

## <a name="confirm-predicted-entity"></a>Confirmación de entidad predicha

Si el indicador visual está por encima de la expresión, indica que el texto se ha predicho, pero _no se etiquetado todavía_. Para convertir la predicción en una etiqueta, seleccione la expresión y, a continuación, seleccione **Confirm entity predictions** (Confirmar predicciones de entidad).

> [!div class="mx-imgBorder"]
> ![Predicción completa de la entidad de aprendizaje automático](media/label-utterances/example-1-label-machine-learned-entity-complete-order-predicted.png)

## <a name="label-subcomponent-entity-by-painting-with-entity-palette-cursor"></a>Etiquetado de la entidad de subcomponente pintando con el cursor de la paleta de entidades

1. Para corregir las predicciones (entidades, que aparecen sobre la expresión de ejemplo), abra la paleta de entidades. 

    > [!div class="mx-imgBorder"]
    > ![Paleta de entidades para la entidad de aprendizaje automático](media/label-utterances/pizza-entity-palette-with-pizza-type-selected.png)

1. Seleccione el subcomponente de la entidad. Esta acción se indica visualmente con un nuevo cursor. El cursor sigue al mouse mientras se mueve por el portal. 

    > [!div class="mx-imgBorder"]
    > ![Paleta de entidades para la entidad de aprendizaje automático](media/label-utterances/pizza-type-entity-palette-cursor.png)

1. En la expresión de ejemplo, _pinte_ la entidad con el cursor. 

    > [!div class="mx-imgBorder"]
    > ![Paleta de entidades para la entidad de aprendizaje automático](media/label-utterances/paint-subcomponent-with-entity-palette-cursor.png)

## <a name="labeling-matching-text-entities-to-a-machine-learned-entity"></a>Etiquetado de entidades con texto coincidente en una entidad de aprendizaje automático

Entre las entidades con texto coincidente se incluyen las entidades predefinidas, las entidades de expresión regular y las entidades de lista. Se agregan a una entidad de aprendizaje automático, como restricciones a un subcomponente, al crear o entidad de aprendizaje automático. 

**Una vez que se agregan estas restricciones, no es necesario etiquetar el texto coincidente en la expresión de ejemplo.**

## <a name="entity-prediction-errors"></a>Errores de predicción de entidades

Los errores de predicción de entidades muestran un indicador de precaución. Esto indica que la entidad predicha no coincide con la entidad etiquetada. 

> [!div class="mx-imgBorder"]
> ![Paleta de entidades para la entidad de aprendizaje automático](media/label-utterances/example-utterance-indicates-prediction-error.png)

## <a name="next-steps"></a>Pasos siguientes

Use el [panel](luis-how-to-use-dashboard.md) y [revise las expresiones de punto de conexión](luis-how-to-review-endpoint-utterances.md) para mejorar la calidad de la predicción de la aplicación.
