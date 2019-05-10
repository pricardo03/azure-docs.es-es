---
title: 'Tutorial: Predicción del precio de un automóvil con la interfaz visual'
titleSuffix: Azure Machine Learning service
description: Aprenda a entrenar, puntuar e implementar un modelo de Machine Learning mediante una interfaz visual de arrastrar y colocar. Este tutorial es la primera de una serie de dos partes sobre la predicción de precios de automóviles mediante la regresión lineal.
author: peterclu
ms.author: peterclu
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
ms.date: 04/06/2019
ms.openlocfilehash: 352f695f186b656ad0f6471d8ede905d53ad5274
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/07/2019
ms.locfileid: "65234107"
---
# <a name="tutorial-predict-automobile-price-with-the-visual-interface"></a>Tutorial: Predicción del precio de un automóvil con la interfaz visual

En este tutorial, se amplía el examen de cómo desarrollar una solución predictiva en la interfaz visual de Azure Machine Learning Service. Cuando acabe este tutorial, tendrá una solución que puede predecir el precio de cualquier coche a partir de las especificaciones técnicas que le envíe.

Este tutorial es la [continuación del tutorial de inicio rápido](ui-quickstart-run-experiment.md) y es la **primera parte de una serie de tutoriales de dos partes**. Sin embargo, no es preciso completar el inicio rápido antes de empezar.

En la primera parte de la serie de tutoriales, aprenderá a:

> [!div class="checklist"]
> * Importar y limpiar datos (los mismos pasos que el inicio rápido)
> * Entrenar un modelo de Machine Learning
> * Puntuar y evaluar un modelo

En la [parte dos](ui-tutorial-automobile-price-deploy.md) de la serie de tutoriales, aprenderá a implementar el modelo predictivo como servicio web de Azure.

> [!NOTE]
> Una versión completa de este tutorial está disponible como experimento de ejemplo.
> En la página Experiments (Experimentos), vaya a **Add New** > **Sample 1 - Regression: Automobile Price Prediction (Basic)** [Agregar nuevo > Ejemplo 1 - Regresión: predicción del precio de automóviles (básica)]

> [!VIDEO https://channel9.msdn.com/Shows/AI-Show/Build-zero-code-machine-learning-models-with-Azure-Machine-Learning-service/player]


## <a name="create-a-workspace"></a>Crear un área de trabajo

Si tiene un área de trabajo de Azure Machine Learning Service, vaya a la [siguiente sección](#open-the-visual-interface-webpage). En caso contrario, créela ahora.

[!INCLUDE [aml-create-portal](../../../includes/aml-create-in-portal.md)]

## <a name="open-the-visual-interface-webpage"></a>Apertura de la página web de la interfaz visual

1. Abra el área de trabajo en [Azure Portal](https://portal.azure.com/).  

1. En el área de trabajo, seleccione **Interfaz visual**.  Luego, seleccione **Iniciar interfaz visual**.  

    ![Captura de pantalla de Azure Portal que muestra cómo acceder a la interfaz visual de un área de trabajo de Machine Learning Service](./media/ui-tutorial-automobile-price-train-score/launch-ui.png)

    La página web de la interfaz se abre en una nueva página del explorador.  

## <a name="import-and-clean-your-data"></a>Importación y limpieza de datos

Lo primero que se necesita es tener datos limpios. Si completó el inicio rápido, aquí puede reutilizar el experimento de preparación de datos. Si no ha completado el inicio rápido, omita la siguiente sección y [empiece un nuevo experimento](#start-from-a-new-experiment).

### <a name="reuse-the-quickstart-experiment"></a>Reutilización del experimento del inicio rápido

1. Abra el experimento del inicio rápido.

1. Seleccione **Save As** (Guardar como) en la parte inferior de la ventana.

1. Asigne un nombre nuevo en el cuadro de diálogo emergente que aparece.

    ![Captura de pantalla que muestra cómo cambiar el nombre de un experimento a "Tutorial - Predict Automobile Price" (Tutorial: predicción del precio de un automóvil)](./media/ui-tutorial-automobile-price-train-score/save-a-copy.png)

1. El experimento debería tener ahora un aspecto similar al siguiente:

    ![Captura de pantalla que muestra el estado esperado del experimento. El conjunto de datos de los automóviles se conecta al módulo Select Columns (Seleccionar columnas) que se conecta a Clean Missing Data (Limpiar datos que faltan)](./media/ui-tutorial-automobile-price-train-score/save-copy-result.png)

Si ha reutilizado correctamente el experimento del inicio rápido, omita la sección siguiente y empiece a [entrenar el modelo](#train-the-model).

### <a name="start-from-a-new-experiment"></a>Inicio con un experimento nuevo

Si no ha completado el inicio rápido, siga estos pasos para crear rápidamente un experimento que importe y limpie el conjunto de datos de los automóviles.

1. Para crear un experimento, seleccione **+NEW** (+Nuevo) en la parte inferior de la ventana de la interfaz visual.

1. Seleccione **EXPERIMENT** >  **Blank Experiment** (Experimento > Experimento en blanco).

1. Seleccione el nombre de experimento predeterminado **"Experimented Created on ...**"(Experimento creado el...) en la parte superior del lienzo y cámbielo por otro que sea significativo. Por ejemplo, **Automobile price prediction** (Predicción del precio de automóviles). No es necesario que el nombre sea único.

1. A la izquierda del lienzo de experimentos, hay una paleta de conjuntos de datos y módulos. Para encontrar módulos, utilice el cuadro de búsqueda de la parte superior de la paleta de módulos. Escriba **automobile** (automóvil) en el cuadro de búsqueda para encontrar el conjunto de datos llamado **Automobile price data (Raw)** (Datos de precios de automóviles [sin formato]). Arrastre este conjunto de datos al lienzo del experimento.

    ![Captura de pantalla de cómo encontrar el conjunto de datos de precios de automóviles](./media/ui-tutorial-automobile-price-train-score/automobile-dataset.png)

    Ahora que ya tiene los datos, puede agregar un módulo que quite completamente la columna **normalized-losses** (pérdidas normalizadas). Luego, agregue otro módulo que quite las filas que tengan datos que falten.

1. Escriba **select columns** (seleccionar columnas) en el cuadro de búsqueda para buscar el módulo **Select Columns in Dataset** (Seleccionar columnas en el conjunto de datos). Después, arrastre este módulo al lienzo del experimento. Este módulo le permite seleccionar las columnas de datos que desea incluir en el modelo, o excluir de él.

1. Conecte el puerto de salida del conjunto de datos **Automobile price data (Raw)** al puerto de entrada del módulo Select Columns in Dataset.

    ![Gif animado que muestra cómo conectar el módulo Automobile Price Data (Datos de precios de automóviles) al módulo Select Columns (Seleccionar columnas)](./media/ui-tutorial-automobile-price-train-score/connect-modules.gif)

1. Seleccione el módulo Select Columns in Dataset y, después, seleccione **Launch column selector** (Iniciar selector de columnas) en el panel **Properties** (Propiedades).

   1. A la izquierda, seleccione **With rules** (Con reglas).

   1. Junto a **Begin With** (Comenzar con), seleccione **All columns** (Todas las columnas). Estas reglas indican a **Select Columns in Dataset** (Seleccionar columnas en conjunto de datos) que pase por todas las columnas (excepto por las que estamos a punto de excluir).

   1. En los menús desplegables, seleccione **Exclude** (Excluir) y, después, los **nombres de columna** y luego escriba **normalized-losses** (pérdidas normalizadas) en el cuadro de texto.

   1. Seleccione el botón OK (Aceptar) para cerrar el selector de columnas (en la parte inferior derecha).

     Ahora el panel de propiedades de **Select Columns in Dataset** (Seleccionar columnas en el conjunto de datos) indica que se pasará por todas las columnas del conjunto de datos excepto **normalized-losses**.

1. Agregue un comentario al módulo **Select Columns in Dataset** (Seleccionar columnas en conjunto de datos), para lo que debe hacer doble clic en el módulo y escribir "Exclude normalized losses" (Excluir pérdidas normalizadas). Esto puede ayudarle a ver de un vistazo lo que el módulo hace en el experimento.

    ![Captura de pantalla que muestra la configuración correcta del módulo Select Columns (Seleccionar columnas)](./media/ui-tutorial-automobile-price-train-score/select-columns.png)

1. Escriba **Clean** (Limpiar) en el cuadro de búsqueda para encontrar el módulo **Clean Missing Data** (Limpiar datos que faltan). Arrastre el módulo **Clean Missing Data** (Limpiar datos que faltan) al lienzo del experimento y conéctelo al módulo **Select Columns in Dataset** (Seleccionar columnas en conjunto de datos).

1. En el panel **Propiedades**, seleccione **Remove entire row** (Quitar la fila entera) en **Cleaning mode** (Modo de limpieza). Estas opciones indican a **Clean Missing Data** (Limpiar datos que faltan) que limpie los datos quitando las filas con valores que faltan. Haga doble clic en el módulo y escriba el comentario "Quitar las filas sin valor".

![Captura de pantalla que muestra la configuración correcta del módulo Clean Missing Data (Limpiar datos que faltan)](./media/ui-tutorial-automobile-price-train-score/clean-missing-data.png)

## <a name="train-the-model"></a>Entrenamiento del modelo

Ahora que los datos están listos puede construir un modelo predictivo. Usará los datos para entrenar el modelo. Luego, probará el modelo para ver con cuánta exactitud es capaz de predecir los precios.

**Clasificación** y **regresión** son dos tipos de algoritmos de aprendizaje automático supervisado. **Clasificación** predice una respuesta a partir de un conjunto definido de categorías, como el color (rojo, azul o verde). **Regresión** se usa para predecir un número.

Como lo que se desea es predecir un precio, que es un número, se puede usar un algoritmo de regresión. En este ejemplo, va a usar un modelo de regresión lineal.

Para entrenar el modelo, proporciónele un conjunto de datos que incluya el precio. El modelo examina los datos y busca correlaciones entre las características de un automóvil y su precio. Después, pruebe el modelo, para lo que debe proporcionarle un conjunto de características de los automóviles con el que esté familiarizado y vea si la predicción del modelo se acerca al precio conocido.

Use sus datos tanto para entrenar el modelo como para probarlo, para lo que los dividirá en conjuntos de datos distintos, de entrenamiento y de prueba.

1. Escriba **split data** (dividir datos) en el cuadro de búsqueda para buscar el módulo **Split Data** (Dividir datos) y conéctelo al puerto izquierdo del módulo **Clean Missing Data** (Limpiar datos que faltan).

1. Seleccione el módulo **Split Data** (Dividir datos) que acaba de conectar para seleccionarlo. En el panel Properties (Propiedades), establezca el valor de Fraction of rows (Fracción de filas) del primer conjunto de datos de salida en 0,7. De esta forma, usaremos el 70 % de los datos para entrenar el modelo y retendremos el 30 % para la realización de pruebas.

    ![Captura de pantalla que muestra la configuración correcta del panel Properties (Propiedades). Los valores de "Split Data" (Dividir datos) debe ser "Split Rows" (Dividir filas); 0,7; división aleatoria; 0; False.](./media/ui-tutorial-automobile-price-train-score/split-data.png)

1. Haga doble clic en **Split Data** (Dividir datos) y escriba el comentario "Split the dataset into training set(0.7) and test set(0.3)" [Dividir el conjunto de datos en conjunto de entrenamiento (0,7) y conjunto de prueba (0,3)]

1. Para seleccionar el algoritmo de aprendizaje, borre e el cuadro de búsqueda de la paleta de módulos.

1. Expanda **Machine Learning** y, luego, **Initialize Model** (Inicializar modelo). Se muestran varias categorías de módulos que se pueden usar para inicializar algoritmos de Aprendizaje automático.

1. Para este experimento, seleccione **Regression** > **Linear Regression** (Regresión > Regresión lineal) y arrástrelo al lienzo del experimento.

    ![Captura de pantalla que muestra la configuración correcta del panel Properties (Propiedades). Los valores de "Split Data" (Dividir datos) debe ser "Split Rows" (Dividir filas); 0,7; división aleatoria; 0; False.](./media/ui-tutorial-automobile-price-train-score/linear-regression-module.png)

1. Busque y arrastre el módulo **Train Model** (Entrenar modelo) al lienzo del experimento. Conecte la salida del módulo Linear Regression (Regresión lineal) a la entrada izquierda del módulo Train Model (Entrenar modelo) y conecte la salida de datos de entrenamiento (puerto izquierdo) del módulo **Split Data** (Dividir datos) a la entrada derecha del módulo **Train Model** (Entrenar modelo).

    ![Captura de pantalla que muestra la configuración correcta del módulo Train Model (Entrenar modelo). El módulo Linear Regression (Regresión lineal) se conecta al puerto izquierdo del módulo Train Model (Entrenar modelo) y el módulo Split Data (Dividir datos) se conecta al puerto derecho de Train Model (Entrenar modelo)](./media/ui-tutorial-automobile-price-train-score/train-model.png)

1. Seleccione el módulo **Train Model** (Entrenar modelo). En el panel Properties (Propiedades), seleccione Launch column selector (Iniciar selector de columnas) y escriba **price** (precio) junto a **Include column names** (Incluir nombres de columna). El precio es el valor que el modelo va a predecir.

    ![Captura de pantalla que muestra la configuración correcta del módulo selector de columnas. With rules > Include column names > "price" (Con reglas > Incluir nombres de columna > "precio")](./media/ui-tutorial-automobile-price-train-score/select-price.png)

    Ahora el experimento debería ser similar a este.
    ![Captura de pantalla que muestra la configuración correcta del experimento después de agregar el módulo Train Model (Entrenar modelo).](./media/ui-tutorial-automobile-price-train-score/train-graph.png)

### <a name="run-the-training-experiment"></a>Ejecución del experimento de entrenamiento

[!INCLUDE [aml-ui-create-training-compute](../../../includes/aml-ui-create-training-compute.md)]

## <a name="score-and-evaluate-the-model"></a>Puntuación y evaluación del modelo

Ahora que ha entrenado el modelo con el 70 % de los datos, puede usarlo para puntuar el otro 30 % y ver si funciona bien.

1. Escriba **score model** (puntuar modelo) en el cuadro de búsqueda para buscar el módulo **Score Model** (Puntuar modelo) y arrastre el módulo al lienzo del experimento. Conecte la salida del módulo **Train Model** (Entrenar modelo) al puerto de entrada izquierdo de **Score Model** (Entrenar modelo). Conecte la salida de los datos de prueba (puerto derecho) del módulo **Split Data** al puerto de entrada derecho de **Score Model**.

1. Escriba **evaluate** (evaluar) en el cuadro de búsqueda para buscar **Evaluate Model** (Evaluar modelo) y arrastre el módulo al lienzo del experimento. Conecte la salida del módulo **Score Model** (Puntuar modelo) a la entrada izquierda de **Evaluate Model** (Evaluar modelo). El experimento final debe tener un aspecto similar a este:

    ![Captura de pantalla que muestra la configuración correcta final del experimento.](./media/ui-tutorial-automobile-price-train-score/final-graph.png)

1. Ejecute el experimento con el mismo destino de proceso que ha usado antes.

1. Vea la salida del módulo **Score Model** (Puntuar modelo) y seleccione el puerto de salida de **Score Model** (Puntuar modelo) y seleccione **Visualize** (Visualizar). La salida muestra los valores previstos para el precio y los valores conocidos de los datos de prueba.

    ![Captura de pantalla de la visualización de salida en que aparece la columna "Scored Label" (Etiqueta puntuada)](./media/ui-tutorial-automobile-price-train-score/score-result.png)

1. Para ver la salida del módulo Evaluate Model (Evaluar modelo), seleccione el puerto de salida y después seleccione Visualize (Visualizar).

    ![Captura de pantalla que muestra los resultados de la evaluación del experimento final.](./media/ui-tutorial-automobile-price-train-score/evaluate-result.png)

Se muestran las siguientes estadísticas de su modelo:

* **Desviación media (MAE)**: la media de errores absolutos (un error es la diferencia entre el valor previsto y el valor real).
* **Error cuadrático medio (RMSE)**: la raíz cuadrada de la media de errores al cuadrado de las predicciones realizadas sobre el conjunto de datos de prueba.
* **Error absoluto relativo**: la media de errores absolutos en relación con la diferencia absoluta entre los valores reales y la media de todos los valores reales.
* **Error al cuadrado relativo**: la media de errores al cuadrado en relación con la diferencia al cuadrado entre los valores reales y la media de todos los valores reales.
* **Coeficiente de determinación**: también se conoce como valor R cuadrado y es una métrica estadística que indica cómo se ajusta un modelo a los datos.

Para cada una de las estadísticas de errores, cuanto menor sea el valor, mejor. Un valor inferior indica que las predicciones se adaptan más estrechamente a los valores reales. En Coefficient of Determination (Coeficiente de determinación), cuanto más cerca esté el valor de uno (1,0), mejores son las predicciones.

## <a name="manage-experiments-in-azure-machine-learning-service-workspace"></a>Administración de experimentos en el área de trabajo de Azure Machine Learning Service

Los experimentos que cree en la interfaz visual se pueden administrar desde el área de trabajo de Azure Machine Learning Service. Use el área de trabajo para ver información más detallada, como las ejecuciones de experimentos individuales, los registros de diagnóstico, los gráficos de ejecución, etc.

1. Abra el área de trabajo en [Azure Portal](https://portal.azure.com/).  

1. En el área de trabajo, seleccione **Experiments** (Experimentos). Luego, seleccione el experimento que ha creado.

    ![Captura de pantalla que muestra cómo ir a los experimentos en Azure Portal](./media/ui-tutorial-automobile-price-train-score/portal-experiments.png)

    En esta página, verá información general acerca del experimento y las últimas veces que se ha ejecutado.

    ![Captura de pantalla que muestra información general acerca de las estadísticas del experimento en Azure portal](./media/ui-tutorial-automobile-price-train-score/experiment-overview.png)

1. Seleccione un número de ejecución para ver más información acerca de ella.

    ![Captura de pantalla de informe de ejecución detallado](./media/ui-tutorial-automobile-price-train-score/run-details.png)

    El informe de ejecución se actualiza en tiempo real. Si ha usado un módulo **Execute Python Script** (Ejecutar script Python) en el experimento, puede especificar que la salida de los registros de los scripts se genere en la pestaña **Logs** (Registros).

## <a name="clean-up-resources"></a>Limpieza de recursos

[!INCLUDE [aml-ui-cleanup](../../../includes/aml-ui-cleanup.md)]

## <a name="next-steps"></a>Pasos siguientes

En la primera parte de este tutorial, ha completado estos pasos:

* Reutilizar el experimento creado en el inicio rápido
* Preparación de los datos
* Entrenamiento del modelo
* Puntuar y evaluar el modelo

En la segunda parte, aprenderá a implementar el modelo como un servicio web de Azure.

> [!div class="nextstepaction"]
> [Continuar con la implementación de modelos](ui-tutorial-automobile-price-deploy.md)
