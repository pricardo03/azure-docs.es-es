---
title: 'Tutorial: Predicción del precio de un automóvil con la interfaz visual'
titleSuffix: Azure Machine Learning
description: Aprenda a entrenar, puntuar e implementar un modelo de Machine Learning mediante una interfaz visual de arrastrar y colocar. Este tutorial es la primera de una serie de dos partes sobre la predicción de precios de automóviles mediante la regresión lineal.
author: peterclu
ms.author: peterlu
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
ms.date: 10/22/2019
ms.openlocfilehash: 3852531615418ffe5397295bc194de34139d6e81
ms.sourcegitcommit: b050c7e5133badd131e46cab144dd5860ae8a98e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/23/2019
ms.locfileid: "72792649"
---
# <a name="tutorial-predict-automobile-price-with-the-visual-interface"></a>Tutorial: Predicción del precio de un automóvil con la interfaz visual

En este tutorial de dos partes aprenderá a usar la interfaz visual de Azure Machine Learning para desarrollar e implementar una solución de análisis predictivo que prediga el precio de cualquier automóvil. 

En la parte uno, configurará el entorno, arrastrará y colocará los módulos en un lienzo interactivo y los conectaremos todos para crear una canalización de Machine Learning.

En la primera parte del tutorial, aprenderá a:

> [!div class="checklist"]
> * Creación de una canalización
> * Importar datos
> * Preparación de los datos
> * Entrenar un modelo de Machine Learning
> * Evaluar un modelo de Machine Learning

En la [segunda parte](ui-tutorial-automobile-price-deploy.md) del tutorial, aprenderá a implementar el modelo predictivo como un servicio web de Azure para predecir el precio de cualquier automóvil en función de las especificaciones técnicas que envíe. 

> [!Note]
>Está disponible una versión completa de este tutorial como una canalización de ejemplo.
>
>Para encontrarla, vaya a la **interfaz visual del área de trabajo**. En la sección **Nueva canalización**, seleccione **Sample 1 - Regression: Automobile Price Prediction (Basic)** (Ejemplo 1 - Regresión: predicción del precio de automóviles [básica])

## <a name="create-a-new-pipeline"></a>Creación de una canalización

Las canalizaciones de Azure Machine Learning organizan varios pasos de aprendizaje automático y procesamiento de datos dependientes en un único recurso. Las canalizaciones ayudan a organizar, administrar y reutilizar flujos de trabajo de aprendizaje automático complejos entre proyectos y usuarios. Para crear una canalización de Azure Machine Learning, necesita un área de trabajo de Azure Machine Learning Service. En esta sección aprenderá a crear estos dos recursos.

### <a name="create-a-new-workspace"></a>Crear un área de trabajo

Si tiene un área de trabajo de Azure Machine Learning Service, vaya a la siguiente sección.

[!INCLUDE [aml-create-portal](../../../includes/aml-create-in-portal.md)]

### <a name="create-the-pipeline"></a>Creación de la canalización

1. Inicie sesión en [ml.azure.com](https://ml.azure.com) y seleccione el área de trabajo con la que quiere trabajar.

1. Seleccione **Visual Interface** (Interfaz visual).

    ![Captura de pantalla del área de trabajo visual que muestra cómo acceder a la interfaz visual](./media/ui-tutorial-automobile-price-train-score/launch-visual-interface.png)

1. Seleccione **Easy-to-use prebuilt modules** (Módulos precompilados fáciles de usar).

1. Seleccione el nombre predeterminado de la canalización **"Pipeline-Created-on ..."** (Canalización: fecha de creación) arriba del lienzo y cámbielo por algo significativo. Por ejemplo, **"Automobile price prediction"** (Predicción del precio de automóviles). No es necesario que el nombre sea único.

## <a name="import-data"></a>Importar datos

Se incluyen varios conjuntos de datos de ejemplo en esta interfaz para que experimente con ellos. En este tutorial, usaremos el conjunto de datos de ejemplo, **Automobile price data (Raw)** (Datos de precios de automóviles, sin procesar). 

1. A la izquierda del lienzo de la canalización, hay una paleta de conjuntos de datos y módulos. Seleccione **Datasets** (Conjuntos de datos) y examine la sección **Samples** (Ejemplos) para ver los conjuntos de datos de ejemplo disponibles.

1. Seleccione el conjunto de datos **Automobile price data (Raw)** y arrástrelo al lienzo.

   ![Arrastrar datos al lienzo](./media/ui-tutorial-automobile-price-train-score/drag-data.gif)

### <a name="visualize-the-data"></a>Visualización de los datos

Puede visualizar los datos para comprender el conjunto de datos que va a usar.

1. Seleccione el módulo **Automobile price data (RAW)** .

1. En el panel **Properties** (Propiedades) a la derecha del lienzo, seleccione **Outputs** (Salidas).

1. Seleccione el icono de gráfico para visualizar los datos.

    ![Visualización de los datos](./media/ui-tutorial-automobile-price-train-score/visualize-data.png)

1. Seleccione las diferentes columnas de la ventana de datos para ver información sobre cada una.

    Cada fila representa un automóvil y las variables asociadas a cada uno aparecen como columnas. Hay 205 filas y 26 columnas en este conjunto de datos.

## <a name="prepare-data"></a>Preparación de los datos

Los conjuntos de datos suelen necesitar algún procesamiento previo antes del análisis. Puede que haya observado que faltan algunos valores al visualizar el conjunto de datos. Estos valores que faltan se deben limpiar para que el modelo pueda analizar los datos de manera adecuada. Quitará columnas con muchos valores que faltan y quitará todas las filas individuales que tengan valores que faltan.

### <a name="remove-a-column"></a>Quitar una columna

Al entrenar un modelo, hay que hacer algo con los datos que faltan. En este conjunto de datos, faltan muchos valores en la columna **normalized-losses**, por lo que excluiremos toda esa columna del modelo.

1. Seleccione las columnas de datos con las que va a trabajar. Escriba **Select** (Seleccionar) en el cuadro de búsqueda de la parte superior de la paleta y busque el módulo **Select Columns in Dataset** (Seleccionar columnas del conjunto de datos).

1. Haga clic y arrastre el módulo **Select Columns in Dataset** (Seleccionar columnas del conjunto de datos) al lienzo. Coloque el módulo bajo el módulo del conjunto de datos.

1. Conecte el conjunto de datos que agregó anteriormente al módulo **Select Columns in Dataset** (Seleccionar columnas del conjunto de datos) haciendo clic sobre él y arrastrándolo. Arrastre desde el puerto de salida del conjunto de datos, que es el círculo pequeño situado en la parte inferior del conjunto de datos en el lienzo, hasta el puerto de entrada de **Select Columns in Dataset** (Seleccionar columnas del conjunto de datos), que es el círculo pequeño de la parte superior del módulo.

    > [!TIP]
    > Puede crear un flujo de datos mediante la canalización si conecta el puerto de salida de un módulo al puerto de entrada de otro.
    >

    ![Conectar módulos](./media/ui-tutorial-automobile-price-train-score/connect-modules.gif)

1. Seleccione el módulo **Select Columns in Dataset**.

1. En el panel **Properties** (Propiedades) a la derecha del lienzo, seleccione **Parameters** > **Edit column** (Parámetros > Editar columna).

1. Seleccione el signo **+** para agregar una nueva regla.

1. En el menú desplegable, seleccione **Exclude** (Excluir) y **Column names** (Nombres de columna).
    
1. Escriba **normalized-losses** en el cuadro de texto.

1. En la esquina inferior derecha, seleccione **Save** (Guardar) para cerrar el selector de columnas.

    ![Excluir una columna](./media/ui-tutorial-automobile-price-train-score/exclude-column.png)
        
    El panel de propiedades muestra que la columna **normalized-losses** se ha excluido.

1. Seleccione el módulo **Select Columns in Dataset**. 

1. En **Properties** (Propiedades), seleccione **Parameters** > **Comment** (Parámetros > Comentario) y escriba "Exclude normalized losses" (Excluir pérdidas normalizadas).

### <a name="clean-missing-data"></a>Limpiar datos que faltan

Después de quitar la columna **normalized-losses**, aún faltan valores en el conjunto de datos. Puede quitar el resto de los datos que faltan mediante el módulo **Clean Missing Data** (Limpiar valores que faltan).

> [!TIP]
> Un requisito previo para usar la mayoría de los módulos de la interfaz visual es limpiar los valores que faltan.

1. Escriba **Clean** (Limpiar) en el cuadro de búsqueda para encontrar el módulo **Clean Missing Data** (Limpiar datos que faltan).

1. Arrastre el módulo **Clean Missing Data** (Limpiar valores que faltan) al lienzo de la canalización y conéctelo con el módulo **Select Columns in Dataset** (Seleccionar columnas del conjunto de datos). 

1. En el panel Propiedades, seleccione **Remove entire row** (Quitar la fila entera) en **Cleaning mode** (Modo de limpieza).

1. En el panel Properties (Propiedades), escriba "Remove missing value rows" (Quitar filas de valores que faltan) en el cuadro **Comment** (Comentario).  

    La canalización debe parecerse a esta:
    
    ![select-column](./media/ui-tutorial-automobile-price-train-score/pipeline-clean.png)

## <a name="train-a-machine-learning-model"></a>Entrenar un modelo de Machine Learning

Ahora que los datos están preprocesados, puede construir un modelo predictivo. Usará los datos para entrenar el modelo. Luego, probará el modelo para ver con cuánta exactitud es capaz de predecir los precios.

### <a name="select-an-algorithm"></a>Selección de un algoritmo

**Clasificación** y **regresión** son dos tipos de algoritmos de aprendizaje automático supervisado. **Clasificación** predice una respuesta a partir de un conjunto definido de categorías, como el color (rojo, azul o verde). **Regresión** se usa para predecir un número.

Como lo que se desea es predecir un precio, que es un número, se puede usar un algoritmo de regresión. En este ejemplo, va a usar un modelo de regresión lineal.

### <a name="split-the-data"></a>División de los datos

Use sus datos tanto para entrenar el modelo como para probarlo; para ello, divídalos en dos conjuntos de datos distintos.

1. Escriba **split data** (dividir datos) en el cuadro de búsqueda para buscar el módulo **Split Data** (Dividir datos) y conéctelo al puerto izquierdo del módulo **Clean Missing Data** (Limpiar datos que faltan).

1. Seleccione el módulo **Split Data** (Dividir datos).

1. En el panel Properties (Propiedades), establezca el valor de **Fraction of rows in the first output dataset** (Fracción de filas del primer conjunto de datos de salida) en 0,7.

    Este valor divide los datos en el 70 % para entrenar el modelo y el 30 % para las pruebas.

1. En el panel Properties (Propiedades), escriba "Split the dataset into training set(0.7) and test set(0.3)" (Dividir el conjunto de datos en conjunto de entrenamiento[0,7] y conjunto de pruebas[0,3]) en el cuadro **Comment** (Comentario).

### <a name="train-the-model"></a>Entrenamiento del modelo

Para entrenar el modelo, proporciónele un conjunto de datos que incluya el precio. El modelo examina los datos y busca correlaciones entre las características de un automóvil y su precio.

1. Para seleccionar el algoritmo de aprendizaje, borre e el cuadro de búsqueda de la paleta de módulos.

1. Expanda **Machine Learning Algorithms** (Algoritmos de aprendizaje automático).
    
    Se muestran varias categorías de módulos que se pueden usar para inicializar algoritmos de Aprendizaje automático.

1. Para esta canalización, seleccione **Regresión** > **Regresión lineal** y arrástrela al lienzo de la canalización.

1. Busque y arrastre el módulo **Entrenar modelo** al lienzo de la canalización. 

1. Conecte la salida del módulo Regresión lineal a la entrada izquierda del módulo Entrenar modelo.

1. Conecte la salida de datos de entrenamiento (puerto izquierdo) del módulo **Dividir datos** a la entrada derecha del módulo **Entrenar modelo**.

    ![Captura de pantalla que muestra la configuración correcta del módulo Train Model (Entrenar modelo). El módulo Linear Regression (Regresión lineal) se conecta al puerto izquierdo del módulo Train Model (Entrenar modelo) y el módulo Split Data (Dividir datos) se conecta al puerto derecho de Train Model (Entrenar modelo)](./media/ui-tutorial-automobile-price-train-score/pipeline-train-model.png)

1. Seleccione el módulo **Train Model** (Entrenar modelo).

1. En el panel Properties (Propiedades), seleccione **Edit column** (Editar columna).

1. En el cuadro de diálogo **Label column** (Columna de etiqueta), expanda el menú desplegable y seleccione **Column names** (Nombres de columna). En el cuadro de texto, escriba **price**. El precio es el valor que el modelo va a predecir.

    La canalización debe ser parecida a esta:

    ![Captura de pantalla que muestra la configuración correcta de la canalización después de agregar el módulo Entrenar modelo.](./media/ui-tutorial-automobile-price-train-score/pipeline-train-graph.png)

## <a name="evaluate-a-machine-learning-model"></a>Evaluar un modelo de Machine Learning

Ahora que ha entrenado el modelo con el 70 % de los datos, puede usarlo para puntuar el otro 30 % y ver si funciona bien.

1. Escriba **score model** en el cuadro de búsqueda para encontrar el módulo **Score Model** (Puntuar modelo) y arrástrelo al lienzo de la canalización. 

1. Conecte la salida del módulo **Train Model** (Entrenar modelo) al puerto de entrada izquierdo de **Score Model** (Entrenar modelo). Conecte la salida de los datos de prueba (puerto derecho) del módulo **Split Data** al puerto de entrada derecho de **Score Model**.

1. Escriba **evaluate** en el cuadro de búsqueda para encontrar el módulo **Evaluate Model** (Evaluar modelo) y arrástrelo al lienzo de la canalización. 

1. Conecte la salida del módulo **Score Model** (Puntuar modelo) a la entrada izquierda de **Evaluate Model** (Evaluar modelo). 

    La canalización final debe parecerse a esta:

    ![Captura de pantalla que muestra la configuración correcta de la canalización.](./media/ui-tutorial-automobile-price-train-score/pipeline-final-graph.png)

### <a name="run-the-pipeline"></a>Ejecución de la canalización

[!INCLUDE [aml-ui-create-training-compute](../../../includes/aml-ui-create-training-compute.md)]

### <a name="view-results"></a>Visualización de los resultados

Una vez finalizada la ejecución, puede ver los resultados de la ejecución de la canalización. 

1. Para ver la salida del módulo **Score Model** (Puntuar modelo), seleccione el módulo **Score Model** (Puntuar modelo).

1. En el panel **Properties** (Propiedades), seleccione **Outputs** > **Visualize** (Salidas > Visualizar). La salida muestra los valores previstos para el precio y los valores conocidos de los datos de prueba.

    ![Captura de pantalla de la visualización de salida en que aparece la columna "Scored Label" (Etiqueta puntuada)](./media/ui-tutorial-automobile-price-train-score/score-result.png)

1. Para ver la salida del módulo **Evaluate Model** (Evaluar modelo), seleccione el módulo **Evaluate Model** (Evaluar modelo).

1. En el panel **Properties** (Propiedades), seleccione **Output** > **Visualize** (Salida > Visualizar) y, luego, seleccione **Visualize** (Visualizar).

Se muestran las siguientes estadísticas de su modelo:

* **Desviación media (MAE)** : la media de errores absolutos (un error es la diferencia entre el valor previsto y el valor real).
* **Error cuadrático medio (RMSE)** : la raíz cuadrada de la media de errores al cuadrado de las predicciones realizadas sobre el conjunto de datos de prueba.
* **Error absoluto relativo**: la media de errores absolutos en relación con la diferencia absoluta entre los valores reales y la media de todos los valores reales.
* **Error al cuadrado relativo**: la media de errores al cuadrado en relación con la diferencia al cuadrado entre los valores reales y la media de todos los valores reales.
* **Coeficiente de determinación**: también se conoce como valor R cuadrado y es una métrica estadística que indica cómo se ajusta un modelo a los datos.

Para cada una de las estadísticas de errores, cuanto menor sea el valor, mejor. Un valor inferior indica que las predicciones se ajustan más a los valores reales. En Coefficient of Determination (Coeficiente de determinación), cuanto más cerca esté el valor de uno (1,0), mejores son las predicciones.

## <a name="clean-up-resources"></a>Limpieza de recursos

[!INCLUDE [aml-ui-cleanup](../../../includes/aml-ui-cleanup.md)]

## <a name="next-steps"></a>Pasos siguientes

En la primera parte de este tutorial, ha completado estos pasos:

* Ha creado una canalización
* Ha preparado los datos
* Ha entrenado el modelo
* Ha puntuado y evaluado el modelo

En la segunda parte, aprenderá a implementar el modelo como un punto de conexión en tiempo real.

> [!div class="nextstepaction"]
> [Continuar con la implementación de modelos](ui-tutorial-automobile-price-deploy.md)
