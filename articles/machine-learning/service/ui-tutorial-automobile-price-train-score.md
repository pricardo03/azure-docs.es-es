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
ms.date: 08/16/2019
ms.openlocfilehash: 11c65c217ef6c150c47f387f7f80070488a8df89
ms.sourcegitcommit: 1752581945226a748b3c7141bffeb1c0616ad720
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/14/2019
ms.locfileid: "70996773"
---
# <a name="tutorial-predict-automobile-price-with-the-visual-interface"></a>Tutorial: Predicción del precio de un automóvil con la interfaz visual

En este tutorial de dos partes aprenderá a usar la interfaz visual de Azure Machine Learning para desarrollar e implementar una solución de análisis predictivo que prediga el precio de cualquier automóvil. 

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2X1GY]

En la parte uno estableceremos el entorno, arrastraremos y colocaremos conjuntos de datos y módulos de análisis en un lienzo interactivo y los conectaremos todos para crear un experimento. 

En la primera parte del tutorial, aprenderá a:

> [!div class="checklist"]
> * Creación de un nuevo experimento
> * Importar datos
> * Preparación de los datos
> * Entrenar un modelo de Machine Learning
> * Evaluar un modelo de Machine Learning

En la [segunda parte](ui-tutorial-automobile-price-deploy.md) aprenderá a implementar el modelo predictivo como servicio web de Azure para que pueda usarlo con el fin de predecir el precio de cualquier automóvil en función de las especificaciones técnicas que envíe. 

Una versión completa de este tutorial está disponible como experimento de ejemplo.

Para encontrarlo, en la **página Experimentos**, seleccione **Agregar nuevo** y, a continuación, seleccione el experimento **Ejemplo 1 - Regresión: predicción del precio de automóviles (básica)** .

## <a name="create-a-new-experiment"></a>Creación de un nuevo experimento

Para crear un experimento de interfaz visual, en primero lugar se necesita un área de trabajo de Azure Machine Learning Service. En esta sección aprenderá a crear ambos recursos.

### <a name="create-a-new-workspace"></a>Crear un área de trabajo

Si tiene un área de trabajo de Azure Machine Learning, vaya a la siguiente sección.

[!INCLUDE [aml-create-portal](../../../includes/aml-create-in-portal.md)]

### <a name="create-an-experiment"></a>Creación de un experimento

1. Abra el área de trabajo en [Azure Portal](https://portal.azure.com/).

1. En el área de trabajo, seleccione **Interfaz visual**. Luego, seleccione **Iniciar interfaz visual**. 

    ![Captura de pantalla de Azure Portal que muestra cómo acceder a la interfaz visual de un área de trabajo de Machine Learning Service](./media/ui-tutorial-automobile-price-train-score/launch-ui.png)

1. Para crear un experimento, seleccione **+New** (+Nuevo) en la parte inferior de la ventana de la interfaz visual.

1. Seleccione **Experimento en blanco**.

1. Seleccione el nombre de experimento predeterminado **"Experiment created on ...** "(Experimento creado el...) en la parte superior del lienzo y cámbielo por otro que sea significativo. Por ejemplo, **"Automobile price prediction"** (Predicción del precio de automóviles). No es necesario que el nombre sea único.

## <a name="import-data"></a>Importar datos

El aprendizaje automático depende de los datos. Afortunadamente, hay disponibles varios conjuntos de datos de ejemplo en esta interfaz para que experimente con ellos. En este tutorial, usaremos el conjunto de datos de ejemplo, **Automobile price data (Raw)** (Datos de precios de automóviles, sin procesar). 

1. A la izquierda del lienzo de experimentos, hay una paleta de conjuntos de datos y módulos. Seleccione **Conjuntos de datos guardados** y, a continuación, seleccione **Ejemplos** para ver los conjuntos de datos de ejemplo disponibles.

1. Seleccione el conjunto de datos **Automobile price data (raw)** y arrástrelo al lienzo.

   ![Arrastrar datos al lienzo](./media/ui-tutorial-automobile-price-train-score/drag-data.gif)

1. Seleccione las columnas de datos con las que va a trabajar. Escriba **Select** (Seleccionar) en el cuadro de búsqueda de la parte superior de la paleta y busque el módulo **Select Columns in Dataset** (Seleccionar columnas del conjunto de datos).

1. Haga clic y arrastre el módulo **Select Columns in Dataset** (Seleccionar columnas del conjunto de datos) al lienzo. Coloque el módulo bajo el módulo del conjunto de datos.

1. Conecte el conjunto de datos que agregó anteriormente al módulo **Select Columns in Dataset** (Seleccionar columnas del conjunto de datos) haciendo clic sobre él y arrastrándolo. Arrastre desde el puerto de salida del conjunto de datos, que es el círculo pequeño situado en la parte inferior del conjunto de datos en el lienzo hasta el puerto de entrada de **Select Columns in Dataset** (Seleccionar columnas del conjunto de datos), que es el círculo pequeño de la parte superior del módulo.

    > [!TIP]
    > Puede crear un flujo de datos a través del experimento si conecta un puerto de salida de un módulo a un puerto de entrada de otro.
    >

    ![Conectar módulos](./media/ui-tutorial-automobile-price-train-score/connect-modules.gif)

    El signo de exclamación en rojo indica que no se han configurado aún las propiedades de este módulo.

1. Seleccione el módulo **Select Columns in Dataset**.

1. En el panel **Propiedades** a la derecha del lienzo, seleccione **Editar columnas**.

    En el cuadro de diálogo **Seleccionar columnas**, seleccione **TODAS LAS COLUMNAS** e incluya **todas las características**. El cuadro de diálogo debe ser similar al siguiente:

     ![selector de columnas](./media/ui-tutorial-automobile-price-train-score/select-all.gif)

1. En la esquina inferior derecha, seleccione **Aceptar** para cerrar el selector de columnas.

### <a name="run-the-experiment"></a>Ejecución del experimento

En cualquier momento, puede hacer clic en el puerto de salida de un conjunto de datos o de un módulo para ver el aspecto de los datos en ese punto del flujo de datos. Si la opción **Visualizar** está deshabilitada, primero debe ejecutar el experimento.

[!INCLUDE [aml-ui-create-training-compute](../../../includes/aml-ui-create-training-compute.md)]

Cuando el destino de proceso está disponible, se ejecuta el experimento. Una vez completada la ejecución, aparece una marca de verificación verde en cada módulo.


### <a name="visualize-the-data"></a>Visualización de los datos

Ahora que ha ejecutado el experimento inicial, puede visualizar los datos para conocer mejor el conjunto de datos del que dispone.

1. Seleccione el puerto de salida en la parte inferior del módulo **Select Columns in Dataset** y, a continuación, seleccione **Visualizar**.

1. Haga clic en diferentes columnas de la ventana de datos para ver información acerca de esa columna.

    En este conjunto de datos, cada fila representa un automóvil y las variables asociadas a cada automóvil aparecen como columnas. Hay 205 filas y 26 columnas en este conjunto de datos.

    Cada vez que haga clic en una columna de datos, la información de **estadística** y la imagen de **visualización** de esa columna aparecerá a la izquierda.

    [![Vista previa de los datos](./media/ui-tutorial-automobile-price-train-score/preview-data.gif)](./media/ui-tutorial-automobile-price-train-score/preview-data.gif#lightbox)

1. Haga clic en cada columna para saber más sobre el conjunto de datos y pensar si estas columnas serán útiles para predecir el precio de un automóvil.

## <a name="prepare-data"></a>Preparación de los datos

Normalmente, los conjuntos de datos requieren procesamiento previo para el análisis. Puede que haya observado que faltan algunos valores al visualizar el conjunto de datos. Estos valores que faltan se deben limpiar para que el modelo pueda analizar los datos de manera adecuada. Va a quitar todas las filas que tengan valores que faltan. Además, la columna **normalized-losses** tiene una gran proporción de valores que faltan, por lo que excluiremos esa columna del modelo por completo.

> [!TIP]
> Limpiar los valores que faltan de los datos de entrada es un requisito previo para usar la mayoría de los módulos.

### <a name="remove-column"></a>Quitar columna

En primer lugar, elimine por completo la columna **normalized-losses**.

1. Seleccione el módulo **Select Columns in Dataset**.

1. En el panel **Propiedades** a la derecha del lienzo, seleccione **Editar columnas**.

    * Deje las opciones **With rules** (Con reglas) y **ALL COLUMNS** (TODAS LAS COLUMNAS) seleccionadas.

    * En los menús desplegables, seleccione **Excluir** y **nombres de columna** y luego haga clic en el cuadro de texto. Escriba **normalized-losses**.

    * En la esquina inferior derecha, seleccione **Aceptar** para cerrar el selector de columnas.

    ![Excluir una columna](./media/ui-tutorial-automobile-price-train-score/exclude-column.gif)
        
    Ahora el panel de propiedades de Select Columns in Dataset (Seleccionar columnas en el conjunto de datos) indica que se pasará por todas las columnas del conjunto de datos excepto **normalized-losses**.
        
    El panel de propiedades muestra que la columna **normalized-losses** se ha excluido.

1. Haga doble clic en el módulo **Select Columns in Dataset** y escriba el comentario "Excluir normalized-losses". 
    
    Después de escribir el comentario, haga clic fuera del módulo. Aparece una flecha hacia abajo que indica que el módulo contiene un comentario.

1. Haga clic en esta flecha para mostrar el comentario.

    El módulo ahora muestra una flecha hacia arriba para ocultar el comentario.
        
    ![Comentarios](./media/ui-tutorial-automobile-price-train-score/comments.png)

### <a name="clean-missing-data"></a>Limpiar datos que faltan

Cuando entrena un modelo, tiene que hacer algo sobre los datos que faltan. En este caso, agregará un módulo para quitar las filas restantes que tienen datos que faltan.

1. Escriba **Clean** (Limpiar) en el cuadro de búsqueda para encontrar el módulo **Clean Missing Data** (Limpiar datos que faltan).

1. Arrastre el módulo **Clean Missing Data** al lienzo del experimento y conéctelo con el módulo **Select Columns in Dataset**. 

1. En el panel Propiedades, seleccione **Remove entire row** (Quitar la fila entera) en **Cleaning mode** (Modo de limpieza).

1. Haga doble clic en el módulo y escriba el comentario "Quitar las filas sin valor".

    El experimento debería tener ahora un aspecto similar al siguiente:
    
    ![select-column](./media/ui-tutorial-automobile-price-train-score/experiment-clean.png)

## <a name="train-a-machine-learning-model"></a>Entrenar un modelo de Machine Learning

Ahora que los datos están listos puede construir un modelo predictivo. Usará los datos para entrenar el modelo. Luego, probará el modelo para ver con cuánta exactitud es capaz de predecir los precios.

### <a name="select-an-algorithm"></a>Selección de un algoritmo

**Clasificación** y **regresión** son dos tipos de algoritmos de aprendizaje automático supervisado. **Clasificación** predice una respuesta a partir de un conjunto definido de categorías, como el color (rojo, azul o verde). **Regresión** se usa para predecir un número.

Como lo que se desea es predecir un precio, que es un número, se puede usar un algoritmo de regresión. En este ejemplo, va a usar un modelo de regresión lineal.

### <a name="split-the-data"></a>División de los datos

Use sus datos tanto para entrenar el modelo como para probarlo, para lo que los dividirá en conjuntos de datos distintos, de entrenamiento y de prueba.

1. Escriba **split data** (dividir datos) en el cuadro de búsqueda para buscar el módulo **Split Data** (Dividir datos) y conéctelo al puerto izquierdo del módulo **Clean Missing Data** (Limpiar datos que faltan).

1. Seleccione el módulo **Split Data** (Dividir datos). En el panel Properties (Propiedades), establezca el valor de Fraction of rows (Fracción de filas) del primer conjunto de datos de salida en 0,7. De esta forma, usaremos el 70 % de los datos para entrenar el modelo y retendremos el 30 % para la realización de pruebas.

1. Haga doble clic en **Split Data** (Dividir datos) y escriba el comentario "Split the dataset into training set(0.7) and test set(0.3)" [Dividir el conjunto de datos en conjunto de entrenamiento (0,7) y conjunto de prueba (0,3)]

### <a name="train-the-model"></a>Entrenamiento del modelo

Para entrenar el modelo, proporciónele un conjunto de datos que incluya el precio. El modelo examina los datos y busca correlaciones entre las características de un automóvil y su precio.

1. Para seleccionar el algoritmo de aprendizaje, borre e el cuadro de búsqueda de la paleta de módulos.

1. Expanda **Machine Learning** y, luego, **Initialize Model** (Inicializar modelo). Se muestran varias categorías de módulos que se pueden usar para inicializar algoritmos de Aprendizaje automático.

1. Para este experimento, seleccione **Regression** > **Linear Regression** (Regresión > Regresión lineal) y arrástrelo al lienzo del experimento.

1. Busque y arrastre el módulo **Train Model** (Entrenar modelo) al lienzo del experimento. Conecte la salida del módulo Linear Regression (Regresión lineal) a la entrada izquierda del módulo Train Model (Entrenar modelo) y conecte la salida de datos de entrenamiento (puerto izquierdo) del módulo **Split Data** (Dividir datos) a la entrada derecha del módulo **Train Model** (Entrenar modelo).

    ![Captura de pantalla que muestra la configuración correcta del módulo Train Model (Entrenar modelo). El módulo Linear Regression (Regresión lineal) se conecta al puerto izquierdo del módulo Train Model (Entrenar modelo) y el módulo Split Data (Dividir datos) se conecta al puerto derecho de Train Model (Entrenar modelo)](./media/ui-tutorial-automobile-price-train-score/train-model.png)

1. Seleccione el módulo **Train Model** (Entrenar modelo). En el panel Properties (Propiedades), seleccione Launch column selector (Iniciar selector de columnas) y escriba **price** (precio) junto a **Include column names** (Incluir nombres de columna). El precio es el valor que el modelo va a predecir.

    ![Captura de pantalla que muestra la configuración correcta del módulo selector de columnas. With rules > Include column names > "price" (Con reglas > Incluir nombres de columna > "precio")](./media/ui-tutorial-automobile-price-train-score/select-price.png)

    El experimento debería ser similar al siguiente:

    ![Captura de pantalla que muestra la configuración correcta del experimento después de agregar el módulo Train Model (Entrenar modelo).](./media/ui-tutorial-automobile-price-train-score/train-graph.png)

## <a name="evaluate-a-machine-learning-model"></a>Evaluar un modelo de Machine Learning

Ahora que ha entrenado el modelo con el 70 % de los datos, puede usarlo para puntuar el otro 30 % y ver si funciona bien.

1. Escriba **score model** (puntuar modelo) en el cuadro de búsqueda para buscar el módulo **Score Model** (Puntuar modelo) y arrastre el módulo al lienzo del experimento. Conecte la salida del módulo **Train Model** (Entrenar modelo) al puerto de entrada izquierdo de **Score Model** (Entrenar modelo). Conecte la salida de los datos de prueba (puerto derecho) del módulo **Split Data** al puerto de entrada derecho de **Score Model**.

1. Escriba **evaluate** (evaluar) en el cuadro de búsqueda para buscar **Evaluate Model** (Evaluar modelo) y arrastre el módulo al lienzo del experimento. Conecte la salida del módulo **Score Model** (Puntuar modelo) a la entrada izquierda de **Evaluate Model** (Evaluar modelo). El experimento final debe tener un aspecto similar a este:

    ![Captura de pantalla que muestra la configuración correcta final del experimento.](./media/ui-tutorial-automobile-price-train-score/final-graph.png)

1. Ejecute el experimento con el recurso de proceso que creó anteriormente.

1. Vea la salida del módulo **Score Model** (Puntuar modelo) y seleccione el puerto de salida de **Score Model** (Puntuar modelo) y seleccione **Visualize** (Visualizar). La salida muestra los valores previstos para el precio y los valores conocidos de los datos de prueba.

    ![Captura de pantalla de la visualización de salida en que aparece la columna "Scored Label" (Etiqueta puntuada)](./media/ui-tutorial-automobile-price-train-score/score-result.png)

1. Para ver la salida del módulo **Evaluate Model** (Evaluar modelo), seleccione el puerto de salida y **Visualize** (Visualizar).

    ![Captura de pantalla que muestra los resultados de la evaluación del experimento final.](./media/ui-tutorial-automobile-price-train-score/evaluate-result.png)

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

* Ha creado un experimento
* Preparación de los datos
* Entrenamiento del modelo
* Puntuar y evaluar el modelo

En la segunda parte, aprenderá a implementar el modelo como un servicio web de Azure.

> [!div class="nextstepaction"]
> [Continuar con la implementación de modelos](ui-tutorial-automobile-price-deploy.md)
