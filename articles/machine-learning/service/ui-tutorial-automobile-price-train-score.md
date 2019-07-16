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
ms.openlocfilehash: 21f5a2d93b708e93f124bd44177bb7852dfbd86a
ms.sourcegitcommit: dad277fbcfe0ed532b555298c9d6bc01fcaa94e2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/10/2019
ms.locfileid: "67720536"
---
# <a name="tutorial-predict-automobile-price-with-the-visual-interface"></a>Tutorial: Predicción del precio de un automóvil con la interfaz visual

En este tutorial, se amplía el examen de cómo desarrollar una solución predictiva en la interfaz visual de Azure Machine Learning Service. Cuando acabe este tutorial, tendrá una solución que puede predecir el precio de cualquier coche a partir de las especificaciones técnicas que le envíe.

En la primera parte del tutorial, aprenderá a:

> [!div class="checklist"]
> * Importar y limpiar datos
> * Entrenar un modelo de Machine Learning
> * Puntuar y evaluar un modelo

En la [parte dos](ui-tutorial-automobile-price-deploy.md) del tutorial, aprenderá a implementar el modelo predictivo como un servicio web de Azure.

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2X1GY]

Una versión completa de este tutorial está disponible como experimento de ejemplo.

Para encontrarlo, en la **página Experimentos**, seleccione **Agregar nuevo** y, a continuación, seleccione el experimento **Ejemplo 1 - Regresión: predicción del precio de automóviles (básica)** .

## <a name="create-a-workspace"></a>Crear un área de trabajo

Si tiene un área de trabajo de Azure Machine Learning Service, vaya a la [siguiente sección](#open-the-visual-interface-webpage).

[!INCLUDE [aml-create-portal](../../../includes/aml-create-in-portal.md)]

## <a name="open-the-visual-interface-webpage"></a>Apertura de la página web de la interfaz visual

1. Abra el área de trabajo en [Azure Portal](https://portal.azure.com/).

1. En el área de trabajo, seleccione **Interfaz visual**. Luego, seleccione **Iniciar interfaz visual**. 

    ![Captura de pantalla de Azure Portal que muestra cómo acceder a la interfaz visual de un área de trabajo de Machine Learning Service](./media/ui-tutorial-automobile-price-train-score/launch-ui.png)

## <a name="create-your-first-experiment"></a>Creación de su primer experimento

La herramienta de interfaz visual le proporciona un área de trabajo visual e interactiva para generar modelos de análisis predictivo. Arrastre y coloque conjuntos de datos y módulos de análisis en un lienzo interactivo y conéctelos todos para crear un *experimento*.

1. Para crear un experimento, seleccione **+New** (+Nuevo) en la parte inferior de la ventana de la interfaz visual.

    ![Agregar nuevo experimento](./media/ui-tutorial-automobile-price-train-score/add-new.png)

1. Seleccione **Experimento en blanco**.

1. Seleccione el nombre de experimento predeterminado **"Experimented Created on ...** "(Experimento creado el...) en la parte superior del lienzo y cámbielo por otro que sea significativo. Por ejemplo, **Automobile price prediction** (Predicción del precio de automóviles). No es necesario que el nombre sea único.

## <a name="add-data"></a>Agregar datos

Lo primero que necesita en el aprendizaje automático son datos. Hay varios conjuntos de datos de ejemplo en esta interfaz que puede usar. También puede importar los datos de los orígenes ya existentes. En este tutorial, usaremos el conjunto de datos de ejemplo, **Automobile price data (Raw)** (Datos de precios de automóviles, sin procesar). 

1. A la izquierda del lienzo de experimentos, hay una paleta de conjuntos de datos y módulos. Seleccione **Conjuntos de datos guardados** y, a continuación, seleccione **Ejemplos** para ver los conjuntos de datos de ejemplo disponibles.

1. Seleccione el conjunto de datos **Automobile price data (raw)** y arrástrelo al lienzo.

   ![Arrastrar datos al lienzo](./media/ui-tutorial-automobile-price-train-score/drag-data.png)

## <a name="select-columns"></a>Select columns

Seleccione las columnas de datos con las que va a trabajar. Para empezar, configure el módulo para que muestre todas las columnas disponibles.

> [!TIP]
> Si conoce el nombre de los datos o el módulo que desea, utilice la barra de búsqueda de la parte superior de la paleta para encontrarlo rápidamente. Durante el resto del tutorial usará este acceso directo.


1. Escriba **Select** (Seleccionar) en el cuadro de búsqueda para buscar el módulo **Select Columns in Dataset** (Seleccionar columnas en el conjunto de datos).

1. Haga clic y arrastre el módulo **Select Columns in Dataset** al lienzo. Coloque el módulo bajo el conjunto de datos que agregó anteriormente.

1. Conecte el conjunto de datos al módulo **Select Columns in Dataset**: haga clic en el puerto de salida del conjunto de datos, arrastre al puerto de entrada de **Select Columns in Dataset** y, finalmente, suelte el botón del mouse. El conjunto de datos y el módulo permanecen conectados aunque se desplace por el lienzo.

    > [!TIP]
    > Los conjuntos de datos y los módulos tienen puertos de entrada y de salida representados por pequeños círculos: los puertos de entrada arriba y los puertos de salida abajo. Puede crear un flujo de datos a través del experimento si conecta un puerto de salida de un módulo a un puerto de entrada de otro.
    >

    ![Conectar módulos](./media/ui-tutorial-automobile-price-train-score/connect-modules.gif)

    El signo de exclamación en rojo indica que no se han configurado aún las propiedades de este módulo.

1. Seleccione el módulo **Select Columns in Dataset**.

1. En el panel **Propiedades** a la derecha del lienzo, seleccione **Editar columnas**.

    En el cuadro de diálogo **Seleccionar columnas**, seleccione **TODAS LAS COLUMNAS** e incluya **todas las características**. El cuadro de diálogo debe ser similar al siguiente:

     ![selector de columnas](./media/ui-tutorial-automobile-price-train-score/select-all.png)

1. En la esquina inferior derecha, seleccione **Aceptar** para cerrar el selector de columnas.

## <a name="run-the-experiment"></a>Ejecución del experimento

En cualquier momento, puede hacer clic en el puerto de salida de un conjunto de datos o de un módulo para ver el aspecto de los datos en ese punto del flujo de datos. Si la opción **Visualizar** está deshabilitada, primero debe ejecutar el experimento.

Un experimento se ejecuta en un destino de proceso que es un recurso de proceso que está asociado al área de trabajo. Una vez creado un destino de proceso, puede volver a utilizarlo para futuras ejecuciones.

[!INCLUDE [aml-ui-create-training-compute](../../../includes/aml-ui-create-training-compute.md)]

Cuando el destino de proceso está disponible, se ejecuta el experimento. Una vez completada la ejecución, aparece una marca de verificación verde en cada módulo.


## <a name="preview-the-data"></a>Vista previa de los datos

Ahora que ha ejecutado el experimento inicial, puede visualizar los datos para conocer mejor el conjunto de datos con el que debe trabajar.

1. Seleccione el puerto de salida en la parte inferior del módulo **Select Columns in Dataset** y, a continuación, seleccione **Visualizar**.

1. Haga clic en diferentes columnas de la ventana de datos para ver información acerca de esa columna.

    En este conjunto de datos, cada fila representa un automóvil y las variables asociadas a cada automóvil aparecen como columnas. Hay 205 filas y 26 columnas en este conjunto de datos.

     Cada vez que haga clic en una columna de datos, la información de **estadística** y la imagen de **visualización** de esa columna aparecerá a la izquierda. Por ejemplo, si hace clic en **num-of-doors** (número de puertas) verá que tiene dos valores únicos y dos valores que faltan. Desplácese hacia abajo para ver los valores: dos y cuatro puertas.

     ![Vista previa de los datos](./media/ui-tutorial-automobile-price-train-score/preview-data.gif)

1. Haga clic en cada columna para saber más sobre el conjunto de datos y pensar si estas columnas serán útiles para predecir el precio de un automóvil.

## <a name="prepare-data"></a>Preparación de los datos

Normalmente, un conjunto de datos requiere algún procesamiento previo antes de que se pueda analizar. Puede que haya observado que faltan algunos valores al visualizar el conjunto de datos. Estos valores que faltan se deben limpiar para que el modelo pueda analizar los datos de manera adecuada. Va a quitar todas las filas que tengan valores que faltan. Además, la columna **normalized-losses** tiene una gran proporción de valores que faltan, por lo que excluiremos esa columna del modelo por completo.

> [!TIP]
> Limpiar los valores que faltan de los datos de entrada es un requisito previo para usar la mayoría de los módulos.

### <a name="remove-column"></a>Quitar columna

En primer lugar, elimine por completo la columna **normalized-losses**.

1. Seleccione el módulo **Select Columns in Dataset**.

1. En el panel **Propiedades** a la derecha del lienzo, seleccione **Editar columnas**.

    * Deje las opciones **With rules** (Con reglas) y **ALL COLUMNS** (TODAS LAS COLUMNAS) seleccionadas.

    * En los menús desplegables, seleccione **Excluir** y **nombres de columna** y luego haga clic en el cuadro de texto. Escriba **normalized-losses**.

    * En la esquina inferior derecha, seleccione **Aceptar** para cerrar el selector de columnas.

    ![Excluir una columna](./media/ui-tutorial-automobile-price-train-score/exclude-column.png)
        
    Ahora el panel de propiedades de Select Columns in Dataset (Seleccionar columnas en el conjunto de datos) indica que se pasará por todas las columnas del conjunto de datos excepto **normalized-losses**.
        
    El panel de propiedades muestra que la columna **normalized-losses** se ha excluido.
        
    ![Panel de propiedades](./media/ui-tutorial-automobile-price-train-score/property-pane.png)
        
    Puede agregar un comentario a un módulo; para ello, haga doble clic en el módulo y escriba texto. Esto puede ayudarle a ver de un vistazo lo que el módulo hace en el experimento. 

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

    Estas opciones indican a **Clean Missing Data** (Limpiar los datos que faltan) que limpie los datos quitando las filas con valores que faltan.

1. Haga doble clic en el módulo y escriba el comentario "Quitar las filas sin valor".
 
    ![Quitar filas](./media/ui-tutorial-automobile-price-train-score/remove-rows.png)

    El experimento debería tener ahora un aspecto similar al siguiente:
    
    ![select-column](./media/ui-tutorial-automobile-price-train-score/experiment-clean.png)

## <a name="visualize-the-results"></a>Visualización de los resultados

Como ha realizado cambios en los módulos del experimento, el estado ha cambiado a "Borrador".  Para visualizar los nuevos datos limpios, primero debe ejecutar de nuevo el experimento.

1. Seleccione **Ejecutar** en la parte inferior para ejecutar el experimento.

    Esta vez se puede reutilizar el destino de proceso que creó anteriormente.

1. Seleccione **Ejecutar** en el cuadro de diálogo.

   ![Ejecutar experimento](./media/ui-tutorial-automobile-price-train-score/select-compute.png)

1. Cuando se complete la ejecución, haga clic con el botón derecho en el módulo **Clean Missing Data** para visualizar los nuevos datos limpios.

    ![Visualizar datos limpios](./media/ui-tutorial-automobile-price-train-score/visualize-cleaned.png)

1. Haga clic en diferentes columnas de la ventana de datos limpios para ver cómo han cambiado los datos.

    ![Visualizar datos limpios](media/ui-tutorial-automobile-price-train-score/visualize-result.png)

    Ahora hay 193 filas y 25 columnas.

    Si hace clic en **num-of-doors** (número de puertas) verá que aún tiene dos valores únicos pero ahora ya no tiene ningún valor que falte. Haga clic en el resto de las columnas para ver que no hayan quedado valores que faltan en el conjunto de datos. 

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

    El experimento debería ser similar al siguiente:

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

* **Desviación media (MAE)** : la media de errores absolutos (un error es la diferencia entre el valor previsto y el valor real).
* **Error cuadrático medio (RMSE)** : la raíz cuadrada de la media de errores al cuadrado de las predicciones realizadas sobre el conjunto de datos de prueba.
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

* Ha creado un experimento
* Preparación de los datos
* Entrenamiento del modelo
* Puntuar y evaluar el modelo

En la segunda parte, aprenderá a implementar el modelo como un servicio web de Azure.

> [!div class="nextstepaction"]
> [Continuar con la implementación de modelos](ui-tutorial-automobile-price-deploy.md)
