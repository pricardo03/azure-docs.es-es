---
title: Prueba de la aplicación en el portal de LUIS
titleSuffix: Azure Cognitive Services
description: Use Language Understanding (LUIS) para trabajar continuamente en la aplicación para refinarla y mejorar la comprensión del lenguaje.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 07/29/2019
ms.author: diberry
ms.openlocfilehash: 5736667acfa510d89e549afd4d0f5c61a833dd1c
ms.sourcegitcommit: 13a289ba57cfae728831e6d38b7f82dae165e59d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/09/2019
ms.locfileid: "68932745"
---
<a name="train-your-app"></a>

# <a name="test-your-luis-app-in-the-luis-portal"></a>Prueba de la aplicación de LUIS en el portal de LUIS

La [prueba](luis-concept-test.md) de una aplicación es un proceso iterativo. Tras entrenar la aplicación de LUIS, pruébela con expresiones de ejemplo para ver si las entidades e intenciones se reconocen correctamente. En caso contrario, realice actualizaciones en la aplicación de LUIS, entrénela y pruébela de nuevo. 

<!-- anchors for H2 name changes -->
<a name="test-your-app"></a>
<a name="access-the-test-page"></a>
<a name="luis-interactive-testing"></a>

## <a name="test-an-utterance"></a>Entrenar una expresión

1. Acceda a la aplicación mediante la selección de su nombre en la página **Mis aplicaciones**. 

1. Para acceder al panel deslizante **Prueba**, seleccione **Prueba** en el panel superior de la aplicación.

    ![Página de entrenamiento y prueba de la aplicación](./media/luis-how-to-interactive-test/test.png)

1. Escriba una expresión en el cuadro de texto y presione Entrar. Puede escribir tantas expresiones de prueba como quiera en **Prueba**, pero solo una expresión cada vez.

1. La expresión, su intención superior y la puntuación se agregan a la lista de expresiones bajo el cuadro de texto.

    ![Las pruebas interactivas identifican la intención incorrecta](./media/luis-how-to-interactive-test/test-weather-1.png)

## <a name="inspect-score"></a>Inspeccionar puntuación

Puede inspeccionar los detalles del resultado de las pruebas en el panel **Inspeccionar**. 
 
1. Con el panel deslizante **Prueba** abierto, haga clic en **Inspeccionar** para una expresión que quiera comparar. 

    ![Seleccione el botón Inspeccionar para ver más detalles acerca de los resultados de la prueba.](./media/luis-how-to-interactive-test/inspect.png)

1. Aparece el panel **Inspección**. El panel incluye la intención de mayor puntuación, así como cualquier entidad identificada. El panel muestra el resultado de la expresión seleccionada.

    ![El panel incluye la intención de mayor puntuación, así como cualquier entidad identificada. El panel muestra el resultado de la expresión seleccionada.](./media/luis-how-to-interactive-test/inspect-panel.png)

## <a name="correct-top-scoring-intent"></a>Intención de mayor puntuación correcta

1. Si la intención de mayor puntuación es incorrecta, haga clic en el botón **Editar**.

1.  En la lista desplegable, seleccione la intención correcta de la expresión.

    ![Seleccionar la intención correcta](./media/luis-how-to-interactive-test/intent-select.png)

## <a name="view-sentiment-results"></a>Ver los resultados de sentimiento

Si el **análisis de sentimiento** está configurado en la página **[Publicar](luis-how-to-publish-app.md#enable-sentiment-analysis)** , los resultados de la prueba incluyen el sentimiento que se encontró en la expresión. 

![Imagen del panel Prueba con el análisis de sentimiento](./media/luis-how-to-interactive-test/sentiment.png)

## <a name="correct-matched-patterns-intent"></a>Corregir la intención del patrón coincidente

Si usa [Patrones](luis-concept-patterns.md) y la expresión coincide con un patrón, pero se predijo una intención incorrecta, seleccione el vínculo **Editar** junto al patrón y haga clic en la intención correcta.

## <a name="compare-with-published-version"></a>Comparar con la versión publicada

Puede probar la versión activa de la aplicación con la versión del [punto de conexión](luis-glossary.md#endpoint) publicado. En el panel **Inspeccionar**, seleccione **Comparar con el publicado**. Todas las pruebas en el modelo publicado se deducirán del saldo de la cuota de suscripción de Azure. 

![Comparar con el publicado](./media/luis-how-to-interactive-test/inspect-panel-compare.png)

## <a name="view-endpoint-json-in-test-panel"></a>Ver el punto de conexión de JSON en el panel Prueba
Puede ver el punto de conexión de JSON devuelto para la comparación si selecciona la **vista Mostrar JSON**.

![Respuesta JSON publicada](./media/luis-how-to-interactive-test/inspect-panel-compare-json.png)

<!--Service name is 'Bing Spell Check v7 API' in the portal-->
## <a name="additional-settings-in-test-panel"></a>Configuración adicional en el panel de pruebas

### <a name="luis-endpoint"></a>Punto de conexión de LUIS

Si tiene varios puntos de conexión de LUIS, use el vínculo **Configuración adicional** del panel Publicado de Prueba para cambiar el punto de conexión usado para realizar pruebas. Si no está seguro de qué punto de conexión usar, seleccione el valor predeterminado **Starter_Key**. 

![Panel Prueba con el vínculo Configuración adicional resaltado](./media/luis-how-to-interactive-test/interactive-with-spell-check-service-key.png)


### <a name="view-bing-spell-check-corrections-in-test-panel"></a>Ver las correcciones de Bing Spell Check en el panel Prueba

Requisitos para ver las correcciones de ortografía: 

* Aplicación publicada
* [Clave de servicio](https://azure.microsoft.com/try/cognitive-services/?api=spellcheck-api) de Bing Spell Check. La clave de servicio no se almacena y debe restablecerse en cada sesión del explorador. 

Use el procedimiento siguiente para incluir el servicio [Bing Spell Check v7](https://azure.microsoft.com/services/cognitive-services/spell-check/) en los resultados del panel Prueba. 

1. En el panel **Prueba**, escriba una expresión. Cuando se haya predicho la expresión, haga clic en **[Inspeccionar](#inspect-score)** debajo de la expresión especificada. 

1. Cuando se abra el panel **Inspeccionar**, seleccione **[Comparar con el publicado](#compare-with-published-version)** . 

1. Cuando se abra el panel **Publicado**, seleccione **[Configuración adicional](#additional-settings-in-test-panel)** .

1. En el cuadro de diálogo emergente, seleccione **Habilitar Bing Spell Check**, escriba la clave y, a continuación, seleccione **Listo**. 
    ![Escribir la clave de servicio de Bing Spell Check](./media/luis-how-to-interactive-test/interactive-with-spell-check-service-key-text.png)

1. Escriba una consulta con una ortografía incorrecta, como `book flite to seattle` y presione Entrar. La ortografía incorrecta de la palabra `flite` se sustituye en la consulta enviada a LUIS y el JSON resultante muestra la consulta original, como `query`, y la ortografía corregida en la consulta, como `alteredQuery`.

<a name="json-file-with-no-duplicates"></a>
<a name="import-a-dataset-file-for-batch-testing"></a>
<a name="export-rename-delete-or-download-dataset"></a>
<a name="run-a-batch-test-on-your-trained-app"></a>
<a name="access-batch-test-result-details-in-a-visualized-view"></a>
<a name="filter-chart-results-by-intent-or-entity"></a>
<a name="investigate-false-sections"></a>
<a name="view single-point utterance data"></a>
<a name="relabel-utterances-and-retrain"></a>
<a name="false-test-results"></a>

## <a name="batch-testing"></a>Pruebas por lotes
Vea los [conceptos](luis-concept-batch-test.md) de las pruebas por lotes y obtenga información sobre [cómo](luis-how-to-batch-test.md) probar un lote de expresiones.

## <a name="next-steps"></a>Pasos siguientes

Si las pruebas indican que la aplicación de LUIS no reconoce las entidades y las intenciones correctas, puede trabajar para mejorar la precisión de la aplicación de LUIS etiquetando más expresiones o agregando características. 

* [Label suggested utterances with LUIS](luis-how-to-review-endpoint-utterances.md) (Etiquetado de expresiones sugeridas con LUIS) 
* [Use features to improve your LUIS app's performance](luis-how-to-add-features.md) (Uso de características para mejorar el rendimiento de la aplicación de LUIS) 
