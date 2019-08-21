---
title: Creación del primer experimento de aprendizaje automático automatizado
titleSuffix: Azure Machine Learning service
description: Aprenda a entrenar e implementar un modelo de clasificación con aprendizaje automático automatizado en Azure Portal.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: tzvikei
author: tsikiksr
ms.reviewer: nibaccam
ms.date: 07/23/2019
ms.openlocfilehash: 7ef19db472b30d82f14a5dd650cb8f4cb1f3ed3a
ms.sourcegitcommit: 5b76581fa8b5eaebcb06d7604a40672e7b557348
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/13/2019
ms.locfileid: "68990073"
---
# <a name="tutorial-use-automated-machine-learning-to-train-and-deploy-your-first-classification-model-preview"></a>Tutorial: Uso del aprendizaje automático automatizado para entrenar e implementar su primer modelo de clasificación (versión preliminar)

En este tutorial, aprenderá a crear su primer experimento de aprendizaje automático automatizado en Azure Portal. En este ejemplo se crea un modelo de clasificación para predecir si un cliente suscribirá un depósito a plazo con el banco.

A través de las funcionalidades de aprendizaje automático automatizado del servicio y Azure Portal, puede empezar el proceso de aprendizaje automático automatizado. La selección de algoritmos y el ajuste de hiperparámetros se hacen automáticamente. La técnica del aprendizaje automático automatizado recorre en iteración muchas combinaciones de algoritmos e hiperparámetros hasta que encuentra el mejor modelo según su criterio, y todo ello sin que tenga que escribir ninguna línea de código.

En este tutorial, ha aprendido las tareas siguientes:

> [!div class="checklist"]
> * Configurar un área de trabajo de Azure Machine Learning Service.
> * Cree un experimento.
> * Entrenar automáticamente un modelo de clasificación.
> * Ver detalles de ejecución del entrenamiento.
> * Se implementa el modelo.

## <a name="prerequisites"></a>Requisitos previos

* Una suscripción de Azure. Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://aka.ms/AMLFree).

* El archivo de datos **bankmarketing_train.csv**. [Descárguelo](https://automlsamplenotebookdata.blob.core.windows.net/automl-sample-notebook-data/bankmarketing_train.csv).

## <a name="create-a-workspace"></a>Crear un área de trabajo

[!INCLUDE [aml-create-portal](../../../includes/aml-create-in-portal.md)]

## <a name="create-an-experiment"></a>Creación de un experimento

1. Vaya al panel izquierdo del área de trabajo. Seleccione **Aprendizaje automático automatizado**  en la sección **Creación (versión preliminar)** .

    ![Panel de navegación de Azure Portal](media/tutorial-1st-experiment-automated-ml/nav-pane.png)

    Como este es el primer experimento con Machine Learning automatizado, verá la pantalla **Welcome to Automated Machine Learning** (Bienvenido a Machine Learning automatizado). 

1. Seleccione **Create Experiment** (Crear experimento). Luego escriba **my-1st-automl-experiment** como nombre del experimento.

1. Seleccione **Create a new compute** (Crear un proceso) y configure el contexto del proceso de este experimento.

    Campo| Valor
    ---|---
    Nombre del proceso| Escriba un nombre único que identifique el contexto del proceso. En este ejemplo, se usa **automl-compute**.
    Tamaño de la máquina virtual| Seleccione el tamaño de la máquina virtual para el proceso. Se usa **Standard_DS12_V2**.
    Configuración adicional| *Min node* (Nodos mín.): 1. Para habilitar la generación de perfiles de datos, debe tener uno o más nodos. <br> *Max node* (Nodos máx.): 6. 

    Para crear el proceso nuevo, seleccione **Crear**. Esto tarda unos minutos. 

    Una vez finalizada la creación, seleccione el proceso nuevo en la lista desplegable y, después, seleccione **Siguiente**.

1. En este tutorial, se usa la cuenta de almacenamiento y el contenedor predeterminados que se han creado con el nuevo proceso. Se rellenan de manera automática en el formulario.

1. Seleccione **Cargar** y elija el archivo **bankmarketing_train.csv** en el equipo local para cargarlo en el contenedor predeterminado. La versión preliminar pública solo admite cargas de archivos locales y cuentas de Azure Blob Storage. Cuando se haya completado la carga, seleccione el archivo en la lista. 

    [![Seleccionar el archivo de datos](media/tutorial-1st-experiment-automated-ml/select-data-file.png)](media/tutorial-1st-experiment-automated-ml/select-data-file-expanded.png#lightbox)

1. La pestaña **Preview** (Versión preliminar) permite configurar más datos de este experimento.

    En la pestaña **Preview** (Versión preliminar), indique que los datos incluyen encabezados. De manera predeterminada, el servicio incluye todas las características (columnas) para el entrenamiento. En este ejemplo, desplácese hacia la derecha y elija **Ignore** (Omitir) para ignorar la característica **day_of_week**.

    ![Configuración de la pestaña Preview (Versión preliminar)](media/tutorial-1st-experiment-automated-ml/preview-tab-config.gif)


    >[!NOTE]
    > La generación de perfiles de datos no está disponible en aquellos procesos que tienen cero nodos mínimos.

1. Seleccione **Classification** (Clasificación) como tarea de predicción.

1. Seleccione **y** como la columna de destino en la que desea realizar las predicciones. Esta columna indica si el cliente se suscribió a un depósito a plazo o no.

1. Expanda **Configuración avanzada** y rellene los campos como se indica a continuación.

    Configuración avanzada|Valor
    ------|------
    Métrica principal| AUC_weighted 
    Exit criteria (Criterios de salida)| Cuando se cumple cualquiera de estos criterios, el trabajo de entrenamiento termina antes de la finalización completa: <br> *Training job time (minutes)* (Tiempo de trabajo de entrenamiento [minutos]): 5  <br> *Max number of iterations* (Número máximo de iteraciones): 10 
    Preprocessing (Preprocesamiento)| Habilita el preprocesamiento que realiza el aprendizaje automático automatizado. Aquí se incluyen la limpieza, preparación y transformación automáticas de los datos para generar características sintéticas.
    Validación| Seleccione la validación cruzada de K iteraciones y **2** como número de validaciones cruzadas. 
    Simultaneidad| Seleccione **5** como número máximo de iteraciones simultáneas.

   >[!NOTE]
   > Para este experimento, no se establece una métrica ni núcleos máximos por umbral de iteraciones. Tampoco se impide que se pruebe ningún algoritmo.

1. Para ejecutar el experimento, seleccione **Iniciar**.

   Cuando se inicie el experimento, verá la pantalla **Run Detail** (Detalle de ejecución) en blanco con el siguiente estado en la parte superior. 

      ![Preparación de ejecución](media/tutorial-1st-experiment-automated-ml/run-preparing.png)
      
El proceso de preparación del experimento tarda unos minutos. Cuando finaliza el proceso, el mensaje de estado cambia **Run is running** (Ejecución en marcha).

##  <a name="view-experiment-details"></a>Visualización de los detalles del experimento

A medida que el experimento progresa, la pantalla **Run Detail** (Detalle de ejecución) actualiza el gráfico de iteración y la lista con las distintas iteraciones (modelos) que se ejecutan. La lista de iteraciones está ordenada por puntuación de métrica. De manera predeterminada, el modelo con la puntuación más alta según la métrica **AUC_weighted** aparece en la parte superior de la lista.

>[!TIP]
> Los trabajos de entrenamiento tardan varios minutos para que cada canalización termine de ejecutarse.

[![Panel de detalles de ejecución](media/tutorial-1st-experiment-automated-ml/run-details.png)](media/tutorial-1st-experiment-automated-ml/run-details-expanded.png#lightbox)

## <a name="deploy-the-model"></a>Implementación del modelo

Para este experimento, **VotingEnsemble** se considera el mejor modelo según la métrica **AUC_weighted**. Con el aprendizaje automático automatizado en Azure Portal, se puede implementar este modelo como servicio web para realizar predicciones con datos nuevos. 

1. En la página **Run Detail** (Detalle de ejecución), seleccione el botón **Deploy Best Model** (Implementar el mejor modelo).

1. Rellene el panel **Deploy Best Model** (Implementar el mejor modelo) como se indica a continuación:

    Campo| Valor
    ----|----
    Deployment name (Nombre de implementación)| my-automl-deploy
    Deployment description (Descripción de implementación)| Implementación de mi primer experimento de aprendizaje automático automatizado
    Scoring script (Script de puntuación)| Autogenerate (Generar automáticamente)
    Environment script (Script del entorno)| Autogenerate (Generar automáticamente)
    
1. Seleccione **Implementar**. La implementación tarda unos 20 minutos en completarse.

    El siguiente mensaje aparece cuando la implementación finaliza correctamente:

    ![Implementación completada](media/tutorial-1st-experiment-automated-ml/deploy-complete-status.png)
    
    Eso es todo. Tiene un servicio web operativo para generar predicciones.

## <a name="clean-up-resources"></a>Limpieza de recursos

Los archivos de implementación son mayores que los archivos de datos y del experimento, por lo que cuesta más almacenarlos. Elimine solo los archivos de implementación para minimizar costos en su cuenta y si desea conservar el área de trabajo y los archivos del experimento. Elimine todo el grupo de recursos completo si no planea usar ninguno de los archivos.  

### <a name="delete-the-deployment-instance"></a>Eliminación de la instancia de implementación

Elimine solo la instancia de implementación de Azure Portal, si desea mantener el área de trabajo y el grupo de recursos para otros tutoriales y para explorarlos. 

1. Vaya al panel **Recursos** de la izquierda y seleccione **Implementaciones**. 

1. Seleccione la implementación que desea eliminar y seleccione **Eliminar.** 

1. Seleccione **Continuar**.

### <a name="delete-the-resource-group"></a>Eliminar el grupo de recursos

[!INCLUDE [aml-delete-resource-group](../../../includes/aml-delete-resource-group.md)]

## <a name="next-steps"></a>Pasos siguientes

En este tutorial de aprendizaje automático automatizado, ha usado Azure Portal para crear e implementar un modelo de clasificación. Para más información y ver los pasos siguientes, consulte estos artículos:

+ Obtenga información sobre cómo [consumir un servicio web](how-to-consume-web-service.md).
+ Más información acerca del [preprocesamiento](how-to-create-portal-experiments.md#preprocess).
+ Más información acerca de la [generación de perfiles de datos](how-to-create-portal-experiments.md#profile).
+ Más información acerca del [aprendizaje automático automatizado](concept-automated-ml.md).

>[!NOTE]
> Este conjunto de conjuntos de marketing bancario está disponible bajo [licencia de Creative Commons (CCO: dominio público)](https://creativecommons.org/publicdomain/zero/1.0/). Todos los derechos de los contenidos individuales de la base de datos tienen la licencia [Database Contents License](https://creativecommons.org/publicdomain/zero/1.0/) y están disponibles en [Kaggle](https://www.kaggle.com/janiobachmann/bank-marketing-dataset). Este conjunto de datos estaba disponible originalmente en la [base de datos de aprendizaje automático de UCI](https://archive.ics.uci.edu/ml/datasets/bank+marketing).<br><br>
> Cite el siguiente trabajo: <br> [Moro et al., 2014] S. Moro, P. Cortez y P. Rita. A Data-Driven Approach to Predict the Success of Bank Telemarketing. Decision Support Systems, Elsevier, 62:22-31, June 2014.
