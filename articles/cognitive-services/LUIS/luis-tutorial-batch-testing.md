---
title: Uso de las pruebas por lotes para mejorar las predicciones de LUIS | Microsoft Docs
titleSuffix: Azure
description: Cargue las pruebas por lotes, revise los resultados y mejore las predicciones de LUIS con cambios.
services: cognitive-services
author: v-geberr
manager: kamran.iqbal
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 03/19/2018
ms.author: v-geberr
ms.openlocfilehash: 5788f17f2724a0354a1db506971c2343c1800f01
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/19/2018
ms.locfileid: "36266403"
---
# <a name="use-batch-testing-to-find-prediction-accuracy-issues"></a>Usar las pruebas por lotes para encontrar problemas de precisión de predicción

En este tutorial se muestra cómo usar las pruebas por lotes para encontrar problemas de predicción de expresiones.  

En este tutorial, aprenderá a:

> [!div class="checklist"]
* Crear un archivo de prueba por lotes 
* Ejecutar una prueba por lotes
* Revisar los resultados de la prueba
* Corregir los errores de intenciones
* Volver a realizar la prueba por lotes

## <a name="prerequisites"></a>requisitos previos

> [!div class="checklist"]
> * Para este artículo, también necesita una cuenta de [LUIS][LUIS] para crear la aplicación.

> [!Tip]
> Si aún no tiene una suscripción, puede registrarse para obtener una [cuenta gratuita](https://azure.microsoft.com/free/).

## <a name="create-new-app"></a>Crear nueva aplicación
En este artículo se usa el dominio creado previamente HomeAutomation. El dominio creado previamente tiene intenciones, entidades y expresiones para controlar dispositivos de HomeAutomation, como las luces. Cree la aplicación, agregue el dominio, entrene y publique.

1. En el sitio web de [LUIS], cree una nueva aplicación seleccionando **Crear nueva aplicación** en la página **MyApps**. 

    ![Crear nueva aplicación](./media/luis-tutorial-batch-testing/create-app-1.png)

2. Escriba el nombre `Batchtest-HomeAutomation` en el cuadro de diálogo.

    ![Escribir el nombre de la aplicación](./media/luis-tutorial-batch-testing/create-app-2.png)

3. Seleccione **Dominios creados previamente** en la esquina inferior izquierda. 

    ![Seleccionar Dominio creado previamente](./media/luis-tutorial-batch-testing/prebuilt-domain-1.png)

4. Haga clic en **Agregar dominio** para HomeAutomation.

    ![Agregar dominio HomeAutomation](./media/luis-tutorial-batch-testing/prebuilt-domain-2.png)

5. Haga clic en **Entrenar** en la barra de navegación superior derecha.

    ![Hacer clic en el botón Entrenar](./media/luis-tutorial-batch-testing/train-button.png)

## <a name="batch-test-criteria"></a>Criterios de prueba por lotes
Las pruebas por lotes pueden probar hasta 1000 expresiones a la vez. El lote no debería tener duplicados. [Exporte](create-new-app.md#export-app) la aplicación para ver la lista de expresiones actuales.  

La estrategia de prueba para LUIS usa tres conjuntos de datos independientes: expresiones de modelo, expresiones de prueba por lotes, y expresiones de punto de conexión. Para este tutorial, asegúrese de que no usa las expresiones de modelo (que se agregan a una intención) ni las de punto de conexión. 

No use ninguna de las expresiones que ya se encuentran en la aplicación para la prueba por lotes:

```
'breezeway on please',
'change temperature to seventy two degrees',
'coffee bar on please',
'decrease temperature for me please',
'dim kitchen lights to 25 .',
'fish pond off please',
'fish pond on please',
'illuminate please',
'living room lamp on please',
'living room lamps off please',
'lock the doors for me please',
'lower your volume',
'make camera 1 off please',
'make some coffee',
'play dvd',
'set lights bright',
'set lights concentrate',
'set lights out bedroom',
'shut down my work computer',
'silence the phone',
'snap switch fan fifty percent',
'start master bedroom light .',
'theater on please',
'turn dimmer off',
'turn off ac please',
'turn off foyer lights',
'turn off living room light',
'turn off staircase',
'turn off venice lamp',
'turn on bathroom heater',
'turn on external speaker',
'turn on my bedroom lights .',
'turn on the furnace room lights',
'turn on the internet in my bedroom please',
'turn on thermostat please',
'turn the fan to high',
'turn thermostat on 70 .' 
```

## <a name="create-a-batch-to-test-intent-prediction-accuracy"></a>Crear un lote para probar la precisión de la predicción de intenciones
1. Cree `homeauto-batch-1.json` en un editor de texto como [VSCode](https://code.visualstudio.com/). 

2. Agregue expresiones con la **intención** que quiera predecir en la prueba. Para este tutorial, para que resulte sencillo, escoja expresiones de `HomeAutomation.TurnOn` y `HomeAutomation.TurnOff`, y cambie el texto `on` y `off` de las expresiones. Para la intención `None`, agregue un par de expresiones que no formen parte del área [dominio](luis-glossary.md#domain) (asunto). 

    Para entender cómo se correlacionan los resultados de la prueba por lotes con el lote JSON, agregue solo seis intenciones.

    ```JSON
    [
        {
          "text": "lobby on please",
          "intent": "HomeAutomation.TurnOn",
          "entities": []
        },
        {
          "text": "change temperature to seventy one degrees",
          "intent": "HomeAutomation.TurnOn",
          "entities": []
        },
        {
          "text": "where is my pizza",
          "intent": "None",
          "entities": []
        },
        {
          "text": "help",
          "intent": "None",
          "entities": []
        },
        {
          "text": "breezeway off please",
          "intent": "HomeAutomation.TurnOff",
          "entities": []
        },
        {
          "text": "coffee bar off please",
          "intent": "HomeAutomation.TurnOff",
          "entities": []
        }
    ]
    ```

## <a name="run-the-batch"></a>Ejecute el lote
1. Haga clic en **Probar** en la barra de navegación superior. 

    ![Hacer clic en Probar en la barra de navegación](./media/luis-tutorial-batch-testing/test-1.png)

2. Seleccione el **panel Prueba por lotes** en el panel de la derecha. 

    ![Seleccionar el panel Prueba por lotes](./media/luis-tutorial-batch-testing/test-2.png)

3. Seleccione **Importar conjunto de datos**.

    ![Seleccionar Importar conjunto de datos](./media/luis-tutorial-batch-testing/test-3.png)

4. Elija la ubicación del sistema de archivos del archivo `homeauto-batch-1.json`.

5. Asigne el nombre `set 1` al conjunto de datos.

    ![Selección del archivo](./media/luis-tutorial-batch-testing/test-4.png)

6. Haga clic en el botón **Ejecutar**. Espere hasta que se realice la prueba.

    ![Hacer clic en Ejecutar](./media/luis-tutorial-batch-testing/test-5.png)

7. Seleccione **Ver resultados**.

    ![Ver los resultados](./media/luis-tutorial-batch-testing/test-6.png)

8. Revise los resultados en el gráfico y la leyenda.

    ![Resultados de lote](./media/luis-tutorial-batch-testing/batch-result-1.png)

## <a name="review-batch-results"></a>Revisar los resultados de lote
Los resultados de lote se encuentran en dos secciones. La sección superior contiene el gráfico y la leyenda. La sección inferior muestra expresiones cuando selecciona el nombre de un área del gráfico.

Los errores se indican en color rojo. El gráfico se divide en cuatro secciones, y dos de las secciones se muestran en color rojo. **Estas son las secciones a las que debe prestar atención**. 

La sección de la parte superior derecha indica que la prueba predijo incorrectamente la existencia de una intención o una entidad. La sección de la parte inferior izquierda indica que la prueba predijo incorrectamente la ausencia de una intención o una entidad.

### <a name="homeautomationturnoff-test-results"></a>Resultado de la prueba de HomeAutomation.TurnOff
En la leyenda, seleccione la intención `HomeAutomation.TurnOff`. Tiene un icono verde de prueba correcta a la izquierda del nombre en la leyenda. No hay errores para esta intención. 

![Resultados de lote](./media/luis-tutorial-batch-testing/batch-result-1.png)

### <a name="homeautomationturnon-and-none-intents-have-errors"></a>Las intenciones HomeAutomation.TurnOn y None tienen errores
Las otras dos intenciones tienen errores, lo que significa que las predicciones de prueba no coincidieron con las expectativas del archivo por lotes. Seleccione la intención `None` en la leyenda para revisar el primer error. 

![Intención None](./media/luis-tutorial-batch-testing/none-intent-failures.png)

Los errores aparecen en el gráfico en las secciones de color rojo: **Falso positivo** y **Falso negativo**. Seleccione el nombre de sección **Falso negativo** en el gráfico para ver las expresiones que obtuvieron un error debajo del gráfico. 

![Errores de Falso negativo](./media/luis-tutorial-batch-testing/none-intent-false-negative.png)

La expresión con error, `help` se esperaba como una intención `None`, pero la prueba predijo la intención `HomeAutomation.TurnOn`.  

Hay dos errores, uno en HomeAutomation.TurnOn y otro en None. Ambos están causados por la expresión `help`, porque no pudo cumplir la expectativa de None y era una coincidencia inesperada para la intención HomeAutomation.TurnOn. 

Para determinar por qué se produce un error en las expresiones `None`, revise las expresiones que están actualmente en `None`. 

## <a name="review-none-intents-utterances"></a>Revisar las expresiones de la intención None

1. Cierre el panel **Prueba**; para ello, haga clic en el botón **Probar** situado en el panel de navegación superior. 

2. Seleccione **Compilar** en el panel de navegación superior. 

3. Seleccione la intención **None** de la lista de intenciones.

4. Presione Control+E para ver la vista de token de las expresiones 
    
    |expresiones de la intención None|Puntuación de predicción|
    |--|--|
    |"reducir la temperatura por mí"|0,44|
    |"atenuar las luces de la cocina a 25".|0,43|
    |"bajar el volumen"|0,46|
    |"activar Internet en mi habitación"|0,28|

## <a name="fix-none-intents-utterances"></a>Corregir las expresiones de la intención None
    
Se supone que cualquier expresión en `None` se encuentra fuera del dominio de la aplicación. Estas expresiones están relacionadas con HomeAutomation, por lo que se encuentran en la intención incorrecta. 

LUIS también otorga a las expresiones una puntuación de predicción inferior al 50 % (<.50). Si observa las expresiones en las otras dos intenciones, veré puntuaciones de predicción muy superiores. Cuando LUIS tiene bajas puntuaciones para expresiones de ejemplo es una buena indicación de que las expresiones provocan que LUIS confunda entre la intención actual y otras intenciones. 

Para corregir la aplicación, es necesario mover las expresiones que se encuentran actualmente en la intención `None` a la intención correcta, y la intención `None` necesita intenciones nuevas y más apropiadas. 

Tres de las expresiones en la intención `None` están diseñadas para reducir la configuración de dispositivos de automatización. Usan palabras como `dim`, `lower` o `decrease`. La cuarta expresión le pide que active Internet. Puesto que las cuatro expresiones son sobre cómo activar o cambiar el grado de energía en un dispositivo, deberían moverse a la intención `HomeAutomation.TurnOn`. 

Esto simplemente es una solución. También puede crear una nueva intención de `ChangeSetting` y mover las expresiones que usan atenuar, reducir y disminuir a dicha intención nueva. 

## <a name="fix-the-app-based-on-batch-results"></a>Corregir la aplicación en función de los resultados de lote
Mueva las cuatro expresiones a la intención `HomeAutomation.TurnOn`. 

1. Active la casilla encima de la lista de expresiones, para que se seleccionen todas las expresiones. 

2. En la lista desplegable **Reassign intent** (Reasignar intenciones), seleccione `HomeAutomation.TurnOn`. 

    ![Mover expresiones](./media/luis-tutorial-batch-testing/move-utterances.png)

    Después de que se reasignen las cuatro expresiones, la lista de expresiones para la intención `None` estará vacía.

3. Agregue cuatro intenciones nuevas para la intención None:

    ```
    "fish"
    "dogs"
    "beer"
    "pizza"
    ```

    Estas expresiones están definitivamente fuera del dominio de HomeAutomation. Cuando escriba cada expresión, observe la puntuación de la misma. La puntuación puede ser baja, o incluso muy baja (con un cuadro rojo alrededor). Después de entrenar la aplicación, en el paso 8, la puntuación será mucho mayor. 

7. Quite todas las etiquetas seleccionando la etiqueta azul en la expresión y luego seleccione **Quitar etiqueta**.

8. Haga clic en **Entrenar** en la barra de navegación superior derecha. La puntuación de cada expresión es mucho mayor. Todas las puntuaciones de la intención `None` ahora deberían ser mayores de 80. 

## <a name="verify-the-fix-worked"></a>Comprobar que la corrección ha funcionado
Para comprobar que las expresiones en la prueba por lotes se predicen correctamente para la intención **None**, vuelva a ejecutar la prueba por lotes.

1. Haga clic en **Probar** en la barra de navegación superior. 

2. Seleccione el **panel Prueba por lotes** en el panel de la derecha. 

3. Haga clic en los tres puntos (...) a la derecha del nombre del lote y seleccione **Ejecutar conjunto de datos**. Espere hasta que se realice la prueba por lotes.

    ![Ejecutar conjunto de datos](./media/luis-tutorial-batch-testing/run-dataset.png)

4. Seleccione **Ver resultados**. Todas las intenciones deben tener un icono verde a la izquierda de los nombres de la intención. Con el filtro correcto establecido en la intención `HomeAutomation.Turnoff`, seleccione el punto verde del panel superior derecho más cercano al centro del gráfico. El nombre de la expresión aparece en la tabla debajo del gráfico. La puntuación de `breezeway off please` es muy baja. Otra opción es agregar más expresiones a la intención para aumentar esta puntuación. 

    ![Ejecutar conjunto de datos](./media/luis-tutorial-batch-testing/turnoff-low-score.png)

<!--
    The Entities section of the legend may have errors. That is the next thing to fix.

## Create a batch to test entity detection
1. Create `homeauto-batch-2.json` in a text editor such as [VSCode](https://code.visualstudio.com/). 

2. Utterances have entities identified with `startPos` and `endPost`. These two elements identify the entity before [tokenization](luis-glossary.md#token), which happens in some [cultures](luis-supported-languages.md#tokenization) in LUIS. If you plan to batch test in a tokenized culture, learn how to [extract](luis-concept-data-extraction.md#tokenized-entity-returned) the non-tokenized entities.

    Copy the following JSON into the file:

    ```JSON
    [
        {
          "text": "lobby on please",
          "intent": "HomeAutomation.TurnOn",
          "entities": [
            {
              "entity": "HomeAutomation.Room",
              "startPos": 0,
              "endPos": 4
            }
          ]
        },
        {
          "text": "change temperature to seventy one degrees",
          "intent": "HomeAutomation.TurnOn",
          "entities": [
            {
              "entity": "HomeAutomation.Operation",
              "startPos": 7,
              "endPos": 17
            }
          ]
        },
        {
          "text": "where is my pizza",
          "intent": "None",
          "entities": []
        },
        {
          "text": "help",
          "intent": "None",
          "entities": []
        },
        {
          "text": "breezeway off please",
          "intent": "HomeAutomation.TurnOff",
          "entities": [
            {
              "entity": "HomeAutomation.Room",
              "startPos": 0,
              "endPos": 9
            }
          ]
        },
        {
          "text": "coffee bar off please",
          "intent": "HomeAutomation.TurnOff",
          "entities": [
            {
              "entity": "HomeAutomation.Device",
              "startPos": 0,
              "endPos": 10
            }
          ]
        }
      ]
    ```

3. Import the batch file, following the [same instructions](#run-the-batch) as the first import, and name the dataset `set 2`. Run the test.

## Possible entity errors
Since the intents in the right-side filter of the test panel still pass the test, this section focuses on correct entity identification. 

Entity testing is diferrent than intents. An utterance will have only one top scoring intent, but it may have several entities. An utterance's entity may be correctly identified, may be incorrectly identified as an entity other than the one in the batch test, may overlap with other entities, or not identified at all. 

## Review entity errors
1. Select `HomeAutomation.Device` in the filter panel. The chart changes to show a single false positive and several true negatives. 

2. Select the False positive section name. The utterance for this chart point is displayed below the chart. The labeled intent and the predicted intent are the same, which is consistent with the test -- the intent prediction is correct. 

    The issue is that the HomeAutomation.Device was detected but the batch expected HomeAutomation.Room for the utterance "coffee bar off please". `Coffee bar` could be a room or a device, depending on the environment and context. As the model designer, you can either enforce the selection as `HomeAutomation.Room` or change the batch file to use `HomeAutomation.Device`. 

    If you want to reinforce that coffee bar is a room, you nee to add an utterances to LUIS that help LUIS decide a coffee bar is a room. 

    The most direct route is to add the utterance to the intent but that to add the utterance for every entity detection error is not the machine-learned solution. Another fix would be to add an utterance with `coffee bar`.

## Add utterance to help extract entity
1. Select the **Test** button on the top navigation to close the batch test panel.

2. On the `HomeAutomation.TurnOn` intent, add the utterance, `turn coffee bar on please`. The uttterance should have all three entities detected after you select enter. 

3. Select **Train** on the top navigation panel. Wait until training completes successfully.

3. Select **Test** on the top navigation panel to open the Batch testing pane again. 

4. If the list of datasets is not visible, select **Back to list**. Select the three dots (...) at the end of `Set 2` and select `Run Dataset`. Wait for the test to complete.

5. Select **See results** to review the test results.

6. 
-->
## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Más información sobre las expresiones de ejemplo](luis-how-to-add-example-utterances.md)

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions