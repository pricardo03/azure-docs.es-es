---
title: 'Inicio rápido: Preparación y visualización de datos sin escribir código'
titleSuffix: Azure Machine Learning service
description: Cree un experimento de aprendizaje automático para preparar y visualizar los datos con una interfaz de usuario del tipo arrastrar y colocar.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: quickstart
author: sdgilley
ms.author: sgilley
ms.date: 05/02/2019
ms.openlocfilehash: 0c492424e67853f7cb4a017fb4215d38a555a8a4
ms.sourcegitcommit: f6c85922b9e70bb83879e52c2aec6307c99a0cac
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/11/2019
ms.locfileid: "65545006"
---
# <a name="quickstart-prepare-and-visualize-data-without-writing-code-in-azure-machine-learning"></a>Inicio rápido: Preparación y visualización de datos sin escribir código en Azure Machine Learning

Prepare y visualice los datos en la interfaz visual de arrastrar y colocar (versión preliminar) para Azure Machine Learning. Los datos que se usarán incluyen entradas para diversos automóviles individuales, por ejemplo, información sobre la marca, el modelo, las especificaciones técnicas y el precio.  

En este inicio rápido podrá explorar y preparar los datos:

- Creará su primer experimento para agregar y obtener una vista previa de los datos
- Preparará los datos mediante la eliminación de los valores que faltan
- Ejecución del experimento
- Visualizará los datos resultantes

Si no está familiarizado con el aprendizaje automático, la serie de vídeos [Data Science for Beginners](https://docs.microsoft.com/azure/machine-learning/studio/data-science-for-beginners-the-5-questions-data-science-answers) (Ciencia de datos para principiantes) le puede brindar una excelente introducción al aprendizaje automático.

## <a name="prerequisites"></a>Requisitos previos

Si no tiene una suscripción a Azure, cree una cuenta gratuita antes de empezar. Pruebe hoy mismo la [versión gratuita o de pago de Azure Machine Learning Service](https://aka.ms/AMLFree).

### <a name="create-a-workspace"></a>Crear un área de trabajo

Si tiene un área de trabajo de Azure Machine Learning Service, vaya a la [siguiente sección](#start). En caso contrario, créela ahora.

[!INCLUDE [aml-create-portal](../../../includes/aml-create-in-portal.md)]

## <a name="start"></a> Apertura de la página web de la interfaz visual

1. Abra el área de trabajo en [Azure Portal](https://portal.azure.com/).  

1. En el área de trabajo, seleccione **Interfaz visual**.  Luego, seleccione **Iniciar interfaz visual**.  
 
    ![Iniciar interfaz visual](./media/ui-quickstart-run-experiment/launch-ui.png)

    La página web de la interfaz se abre en una nueva página del explorador.  

## <a name="create-your-first-experiment"></a>Creación de su primer experimento

La herramienta de interfaz visual le proporciona un área de trabajo visual e interactiva para generar, probar e iterar con toda facilidad sobre un modelo de análisis predictivo. Se arrastran y colocan conjuntos de datos y módulos de análisis en un lienzo interactivo, conectándolos todos para formar un _experimento_.  Cree su primer experimento hoy mismo.

1. En la esquina inferior izquierda, seleccione **Agregar nuevo**.
![Agregar nuevo experimento](./media/ui-quickstart-run-experiment/add-new.png)

1. Seleccione **Experimento en blanco**.

1. El experimento recibe un nombre predeterminado. Seleccione este texto y cambie su nombre por el de "Inicio rápido: exploración de datos". No es necesario que este nombre sea único.

1. El **minimapa** de la parte inferior de la pantalla es útil para ver experimentos de gran tamaño.  No lo necesitará en este inicio rápido, así que haga clic en la flecha de la parte superior para minimizarlo.  

    ![Renombrar el experimento](./media/ui-quickstart-run-experiment/rename.png)

## <a name="add-data"></a>Agregar datos

Lo primero que necesita en el aprendizaje automático son datos. Hay varios conjuntos de datos de ejemplo en esta interfaz que puede usar; otra opción es importarlos de diversos orígenes. En este ejemplo, usaremos el conjunto de datos de ejemplo, **Automobile price data (Raw)** (Datos de precios de automóviles, sin procesar). 

1. A la izquierda del lienzo de experimentos, hay una paleta de conjuntos de datos y módulos. Seleccione **Conjuntos de datos guardados** y, a continuación, seleccione **Ejemplos** para ver los conjuntos de datos de ejemplo disponibles.

1. Seleccione el conjunto de datos **Automobile price data (raw)** y arrástrelo al lienzo.

   ![Arrastrar datos al lienzo](./media/ui-quickstart-run-experiment/drag-data.png)


## <a name="select-columns"></a>Select columns

Seleccione las columnas de datos con las que va a trabajar.  Para empezar, configure el módulo para que muestre todas las columnas disponibles.

> [!TIP]
> Si conoce el nombre de los datos o el módulo que desea, utilice la barra de búsqueda de la parte superior de la paleta para encontrarlo rápidamente.  Durante el resto del inicio rápido usará este acceso directo.

1. Escriba **Select** (Seleccionar) en el cuadro de búsqueda para buscar el módulo **Select Columns in Dataset** (Seleccionar columnas en el conjunto de datos).

1. Haga clic y arrastre el módulo **Select Columns in Dataset** al lienzo. Coloque el módulo bajo el conjunto de datos que agregó anteriormente.

1. Conecte el conjunto de datos al módulo **Select Columns in Dataset**: haga clic en el puerto de salida del conjunto de datos, arrastre al puerto de entrada de **Select Columns in Dataset** y, finalmente, suelte el botón del mouse. El conjunto de datos y el módulo permanecen conectados aunque se desplace por el lienzo.

    > [!TIP]
    > Los conjuntos de datos y los módulos tienen puertos de entrada y de salida representados por pequeños círculos: los puertos de entrada arriba y los puertos de salida abajo. Puede crear un flujo de datos a través del experimento si conecta un puerto de salida de un módulo a un puerto de entrada de otro.
    >
    > Si tiene problemas para conectar los módulos, intente arrastrarlo hacia el nodo al que se va a conectar.

    ![Conectar módulos](./media/ui-quickstart-run-experiment/connect-modules.gif) 

    El signo de exclamación en rojo indica que no se han configurado aún las propiedades de este módulo. Se hará a continuación.
   
1. Seleccione el módulo **Select Columns in Dataset**.

1. En el panel **Propiedades** a la derecha del lienzo, seleccione **Editar columnas**.

    En el cuadro de diálogo **Seleccionar columnas**, seleccione **TODAS LAS COLUMNAS** e incluya **todas las características**. El cuadro de diálogo debe ser similar al siguiente:

     ![selector de columnas](./media/ui-quickstart-run-experiment/select-all.png)

1. En la esquina inferior derecha, seleccione **Aceptar** para cerrar el selector de columnas.

## <a name="run-the-experiment"></a>Ejecución del experimento

En cualquier momento, puede hacer clic en el puerto de salida de un conjunto de datos o de un módulo para ver el aspecto de los datos en ese punto del flujo de datos.  Si la opción **Visualizar** está deshabilitada, primero debe ejecutar el experimento.  Se hará a continuación.

[!INCLUDE [aml-ui-create-training-compute](../../../includes/aml-ui-create-training-compute.md)]

Cuando el destino de proceso está disponible, se ejecuta el experimento. Una vez completada la ejecución, aparece una marca de verificación verde en cada módulo.

![Ver estado](./media/ui-quickstart-run-experiment/status1.png)

## <a name="preview-the-data"></a>Vista previa de los datos

Ahora que ha ejecutado el experimento inicial, puede visualizar los datos para conocer mejor la información con la que debe trabajar.

1. Seleccione el puerto de salida en la parte inferior del módulo **Select Columns in Dataset** y, a continuación, seleccione **Visualizar**.

1. Haga clic en diferentes columnas de la ventana de datos para ver información acerca de esa columna.  

    En este conjunto de datos, cada fila representa un automóvil y las variables asociadas a cada automóvil aparecen como columnas.    Hay 205 filas y 26 columnas en este conjunto de datos.

     Cada vez que haga clic en una columna de datos, la información de **estadística** y la imagen de **visualización** de esa columna aparecerá a la izquierda.  Por ejemplo, si hace clic en **num-of-doors** (número de puertas) verá que tiene dos valores únicos y dos valores que faltan.  Desplácese hacia abajo para ver los valores: dos y cuatro puertas.

     ![Vista previa de los datos](./media/ui-quickstart-run-experiment/preview-data.gif)

1. Haga clic en cada columna para más información sobre el conjunto de datos.

## <a name="prepare-data"></a>Preparación de los datos

Normalmente, un conjunto de datos requiere algún procesamiento previo antes de que se pueda analizar. Es posible que haya observado los valores que faltan en las columnas de varias filas. Estos valores que faltan se deben limpiar para que el modelo pueda analizar los datos de manera adecuada. Va a quitar todas las filas que tengan valores que faltan. Además, la columna **normalized-losses** tiene una gran proporción de valores que faltan, por lo que excluiremos esa columna del modelo por completo.

> [!TIP]
> Limpiar los valores que faltan de los datos de entrada es un requisito previo para usar la mayoría de los módulos.  

### <a name="remove-column"></a>Quitar columna

En primer lugar, elimine por completo la columna **normalized-losses**.

1. Seleccione el módulo **Select Columns in Dataset**.

1. En el panel **Propiedades** a la derecha del lienzo, seleccione **Editar columnas**.

    * Deje las opciones **With rules** (Con reglas) y **TODAS LAS COLUMNAS** seleccionadas.

    * En los menús desplegables, seleccione **Excluir** y **nombres de columna** y luego haga clic en el cuadro de texto. Escriba **normalized-losses**.

    * En la esquina inferior derecha, seleccione **Aceptar** para cerrar el selector de columnas.

    ![Excluir una columna](./media/ui-quickstart-run-experiment/exclude-column.png)
        
    Ahora el panel de propiedades de Select Columns in Dataset (Seleccionar columnas en el conjunto de datos) indica que se pasará por todas las columnas del conjunto de datos excepto **normalized-losses**.
        
    El panel de propiedades muestra que la columna **normalized-losses** se ha excluido.
        
    ![Panel de propiedades](./media/ui-quickstart-run-experiment/property-pane.png)
        
    Puede agregar un comentario a un módulo; para ello, haga doble clic en el módulo y escriba texto. Esto puede ayudarle a ver de un vistazo lo que el módulo hace en el experimento. 

1. Haga doble clic en el módulo **Select Columns in Dataset** y escriba el comentario "Excluir normalized-losses". 
    
    Después de escribir el comentario, haga clic fuera del módulo.  Aparece una flecha hacia abajo que indica que el módulo contiene un comentario.

1. Haga clic en esta flecha para mostrar el comentario.

    El módulo ahora muestra una flecha hacia arriba para ocultar el comentario.
        
    ![Comentarios](./media/ui-quickstart-run-experiment/comments.png)

### <a name="clean-missing-data"></a>Limpiar datos que faltan

A continuación, agregue otro módulo que quite las filas restantes que tengan datos que faltan.

1. Escriba **Clean** (Limpiar) en el cuadro de búsqueda para encontrar el módulo **Clean Missing Data** (Limpiar datos que faltan).

1. Arrastre el módulo **Clean Missing Data** al lienzo del experimento y conéctelo con el módulo **Select Columns in Dataset**. 

1. En el panel Propiedades, seleccione **Remove entire row** (Quitar la fila entera) en **Cleaning mode** (Modo de limpieza).

    Estas opciones indican a **Clean Missing Data** (Limpiar los datos que faltan) que limpie los datos quitando las filas con valores que faltan.

1. Haga doble clic en el módulo y escriba el comentario "Quitar las filas sin valor".
 
    ![Quitar filas](./media/ui-quickstart-run-experiment/remove-rows.png)

    El experimento debería tener ahora un aspecto similar al siguiente:
    
    ![select-column](./media/ui-quickstart-run-experiment/experiment-clean.png)

## <a name="visualize-the-results"></a>Visualización de los resultados

Como ha realizado cambios en los módulos del experimento, el estado ha cambiado a "Borrador".  Para visualizar los nuevos datos limpios, primero debe ejecutar de nuevo el experimento.

1. Seleccione **Ejecutar** en la parte inferior para ejecutar el experimento.

    Esta vez se puede reutilizar el destino de proceso que creó anteriormente.  

1. Seleccione **Ejecutar** en el cuadro de diálogo.

   ![Ejecutar experimento](./media/ui-quickstart-run-experiment/select-compute.png)

1. Cuando se complete la ejecución, haga clic con el botón derecho en el módulo **Clean Missing Data** para visualizar los nuevos datos limpios.  

    ![Visualizar datos limpios](./media/ui-quickstart-run-experiment/visualize-cleaned.png)

1. Haga clic en diferentes columnas de la ventana de datos limpios para ver cómo han cambiado los datos.  

    ![Visualizar datos limpios](media/ui-quickstart-run-experiment/visualize-result.png)

    Ahora hay 193 filas y 25 columnas.

    Si hace clic en **num-of-doors** (número de puertas) verá que aún tiene dos valores únicos pero ahora ya no tiene ningún valor que falte.  

## <a name="clean-up-resources"></a>Limpieza de recursos

[!INCLUDE [aml-ui-cleanup](../../../includes/aml-ui-cleanup.md)]

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha aprendido a hacer lo siguiente:

- Creará su primer experimento para agregar y obtener una vista previa de los datos
- Preparará los datos mediante la eliminación de los valores que faltan
- Visualizará los datos resultantes

Seguir el tutorial para usar estos datos para predecir el precio de un automóvil.

> [!div class="nextstepaction"]
> [Tutorial: Predicción del precio de un automóvil con la interfaz visual](ui-tutorial-automobile-price-train-score.md)
