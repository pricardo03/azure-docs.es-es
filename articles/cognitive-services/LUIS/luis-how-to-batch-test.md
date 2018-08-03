---
title: 'Prueba por lotes de la aplicación de LUIS: Azure | Microsoft Docs'
description: Use las pruebas por lotes de Language Understanding (LUIS) para buscar expresiones con intenciones y entidades incorrectas.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 03/14/2018
ms.author: diberry
ms.openlocfilehash: 2c648cdd82f89a9646fa0b311a7f1f68dd4bc4a9
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/24/2018
ms.locfileid: "39223588"
---
# <a name="batch-testing"></a>Pruebas por lotes
 Las pruebas por lotes son una prueba completa en el modelo entrenado actual para medir su rendimiento en LUIS. 

<a name="batch-testing"></a>
## <a name="import-a-dataset-file-for-batch-testing"></a>Importar un archivo de conjunto de datos para las pruebas por lotes

1. Seleccione **Prueba** en la barra superior y, después, seleccione el **panel Pruebas por lotes**.

    ![Vínculo de Pruebas por lotes](./media/luis-how-to-batch-test/batch-testing-link.png)

2. Seleccione **Importar conjunto de datos**. Aparece el cuadro de diálogo **Import new dataset** (Importar nuevo conjunto de datos). Seleccione **Choose File** (Elegir archivo) y ubique un archivo JSON con el [formato JSON](luis-concept-batch-test.md#batch-file-format) correcto que contiene *no más de 1000* expresiones para probar.

    ![Importar archivo de conjunto de datos](./media/luis-how-to-batch-test/batchtest-importset.png)

    Los errores de importación se notifican en una barra de notificación de color rojo en la parte superior del explorador. Cuando una importación tiene errores, no se crea ningún conjunto de datos. Para obtener más información, vea [Errores comunes](luis-concept-batch-test.md#common-errors-importing-a-batch).

3. En el campo **Nombre del conjunto de datos**, escriba un nombre para el archivo del conjunto de datos. El archivo del conjunto de datos incluye una **matriz de expresiones** que incluye la *intención etiquetada* y las *entidades*. Revise el [archivo por lotes de ejemplo](luis-concept-batch-test.md#batch-file-format) para consultar la sintaxis. 

4. Seleccione **Listo**. Se agrega el archivo del conjunto de datos.

## <a name="run-rename-export-or-delete-dataset"></a>Ejecutar, cambiar el nombre, exportar o eliminar el conjunto de datos
Para ejecutar, cambiar el nombre, exportar o eliminar el conjunto de datos, use el botón de puntos suspensivos (***...***) al final de la fila del conjunto de datos.

![Acciones del conjunto de datos](./media/luis-how-to-batch-test/batch-testing-options.png)

## <a name="run-a-batch-test-on-your-trained-app"></a>Ejecutar una prueba por lotes en la aplicación entrenada

Para ejecutar la prueba, seleccione el nombre del conjunto de datos. Cuando se complete la prueba, esta fila muestra el resultado de la prueba del conjunto de datos.

![Resultado de la prueba por lotes](./media/luis-how-to-batch-test/run-test.png)

El conjunto de datos que se puede descargar es el mismo archivo que se cargó para realizar la prueba por lotes.

|Estado|Significado|
|--|--|
|![Icono de círculo verde de prueba correcta](./media/luis-how-to-batch-test/batch-test-result-green.png)|Todas las expresiones son correctas.|
|![Icono de X de color rojo de prueba incorrecta](./media/luis-how-to-batch-test/batch-test-result-red.png)|Al menos una intención de expresión no coincidió con la predicción.|
|![Icono de preparado para la prueba](./media/luis-how-to-batch-test/batch-test-result-blue.png)|La prueba está lista para ejecutarse.|

<a name="access-batch-test-result-details-in-a-visualized-view"></a>
## <a name="view-batch-test-results"></a>Ver los resultados de la prueba por lotes 
Para revisar los resultados de la prueba por lotes, seleccione **Ver resultados**.

![Resultados de la prueba por lotes](./media/luis-how-to-batch-test/run-test-results.png)

<!--
 Select the **See results** link that appears after you run the test. A scatter graph known as an error matrix displays. The data points represent the utterances in the dataset. 

Green points indicate correct prediction, and red ones indicate incorrect prediction.

The filtering panel on the right side of the screen displays a list of all intents and entities in the app, with a green point for intents/entities that were predicted correctly in all dataset utterances, and a red point for those items with errors. Also, for each intent/entity, you can see the number of correct predictions out of the total utterances.

-->


<a name="filter-chart-results-by-intent-or-entity"></a> ## Filtrar los resultados del gráfico

Para filtrar el gráfico por una intención o entidad específica, seleccione la intención o entidad en el panel de filtrado del lado derecho. Los puntos de datos y su distribución se actualizan en el gráfico según su selección. 
 
![Resultado de la prueba por lotes visualizado](./media/luis-how-to-batch-test/filter-by-entity.png) 

## <a name="view-single-point-utterance-data"></a>Ver los datos de expresión de un único punto
En el gráfico, mantenga el puntero sobre un punto de datos para ver la puntuación de exactitud de la predicción. Seleccione un punto de datos para recuperar su expresión correspondiente en la lista de expresiones en la parte inferior de la página. 

![Expresión seleccionada](./media/luis-how-to-batch-test/selected-utterance.png)


<a name="relabel-utterances-and-retrain"></a>
<a name="false-test-results"></a>
## <a name="view-section-data"></a>Ver los datos de la sección
En el gráfico de cuatro secciones, seleccione el nombre de sección, como **Falso positivo** en la parte superior derecha del gráfico. Debajo del gráfico se muestran todas las expresiones de esa sección en una lista. 

![Expresiones seleccionadas por la sección](./media/luis-how-to-batch-test/selected-utterances-by-section.png)

En la imagen anterior, la expresión `switch on` está etiquetada con la intención TurnAllOn, pero recibió la predicción de intención None. Se trata de un valor que indica que la intención TurnAllOn necesita más expresiones de ejemplo para realizar la predicción esperada. 

Las dos secciones del gráfico en rojo indican las expresiones que no coincidieron con la predicción esperada. Estas indican expresiones para las que LUIS necesita más entrenamiento. 

Las dos secciones del gráfico en color verde coincidieron con la predicción esperada.

## <a name="next-steps"></a>Pasos siguientes

Si las pruebas indican que la aplicación de LUIS no reconoce las entidades y las intenciones correctas, puede trabajar para mejorar el rendimiento de la aplicación de LUIS etiquetando más expresiones o agregando características. 

* [Label suggested utterances with LUIS](luis-how-to-review-endoint-utt.md) (Etiquetado de expresiones sugeridas con LUIS) 
* [Use features to improve your LUIS app's performance](luis-how-to-add-features.md) (Uso de características para mejorar el rendimiento de la aplicación de LUIS) 
* [Comprenda las pruebas por lotes con este tutorial](luis-tutorial-batch-testing.md)
* [Obtenga información sobre los conceptos de las pruebas por lotes](luis-concept-batch-test.md).