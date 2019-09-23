---
title: Creación del primer experimento de aprendizaje automático automatizado
titleSuffix: Azure Machine Learning
description: Aprenda a entrenar e implementar un modelo de clasificación con aprendizaje automático automatizado en la página de aterrizaje del área de trabajo de Azure Machine Learning.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
ms.author: tzvikei
author: tsikiksr
ms.reviewer: nibaccam
ms.date: 09/09/2019
ms.openlocfilehash: 2422a4525c94f3997dd0a9a0859135e9acf59ffa
ms.sourcegitcommit: c79aa93d87d4db04ecc4e3eb68a75b349448cd17
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/18/2019
ms.locfileid: "71092003"
---
# <a name="tutorial-create-your-first-classification-model-with-automated-machine-learning"></a>Tutorial: Creación del primer modelo de clasificación con el aprendizaje automático automatizado

En este tutorial, aprenderá a crear su primer experimento de aprendizaje automático automatizado en mediante la página de aterrizaje del área de trabajo (versión preliminar) sin escribir ni una línea de código. En este ejemplo se crea un modelo de clasificación para predecir si un cliente suscribirá un depósito a plazo fijo con una institución financiera.

Con el aprendizaje automático, puede automatizar las tareas que requieren mucho tiempo. El aprendizaje automático recorre en iteración rápidamente muchas combinaciones de algoritmos e hiperparámetros para ayudarle a encontrar el mejor modelo según una métrica de éxito de su elección.

En este tutorial, aprenderá las siguientes tareas:

> [!div class="checklist"]
> * Cree un área de trabajo de Azure Machine Learning.
> * Ejecución de un experimento de aprendizaje automático automatizado.
> * Visualización de los detalles del experimento.
> * Se implementa el modelo.

## <a name="prerequisites"></a>Requisitos previos

* Una suscripción de Azure. Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://aka.ms/AMLFree).

* Descargue el archivo de datos [**bankmarketing_train.csv**](https://automlsamplenotebookdata.blob.core.windows.net/automl-sample-notebook-data/bankmarketing_train.csv). La columna **y** indica si un cliente se ha suscrito a un depósito a plazo fijo, que se identifica más adelante como la columna de destino para las predicciones de este tutorial. 

## <a name="create-a-workspace"></a>Crear un área de trabajo

Un área de trabajo de Azure Machine Learning es un recurso básico de la nube que se usa para experimentar, entrenar e implementar modelos de aprendizaje automático. Vincula la suscripción y el grupo de recursos de Azure con un objeto fácilmente consumido del servicio. 

Puede crear un área de trabajo mediante Azure Portal, una consola basada en web para administrar los recursos de Azure. 

[!INCLUDE [aml-create-portal](../../../includes/aml-create-in-portal.md)]

>[!IMPORTANT] 
> Tome nota del **área de trabajo** y de la **suscripción**. Los necesitará para asegurarse de que crea el experimento en el lugar correcto. 

## <a name="create-and-run-the-experiment"></a>Creación y ejecución de un experimento

Complete los siguientes pasos de configuración y ejecución del experimento en la página de aterrizaje del área de trabajo, una interfaz consolidada que incluye herramientas de aprendizaje automático para realizar escenarios de ciencia de datos para los profesionales de ciencia de datos de todos los niveles de conocimiento.

1. Inicie sesión en la [página de aterrizaje del área de trabajo](https://ml.azure.com/workspaceportal/).

1. Seleccione la suscripción y el área de trabajo que ha creado.

1. Seleccione **Comenzar**.

1.  Seleccione **Automated ML** (ML automatizada) en la sección **Authoring** (Creación), en el panel de la izquierda.
Verá la pantalla **Getting started** (Introducción), ya que este es el primer experimento con Machine Learning automatizado.

    ![Azure Machine Learning Studio](media/tutorial-1st-experiment-automated-ml/get-started.png)

1. Seleccione **Create Experiment** (Crear experimento). 

1. Escriba **my-1st-automl-experiment** como nombre del experimento.

1. Seleccione **Create a new compute** (Crear un proceso). Un proceso es un entorno de recursos locales o en la nube que se usa para ejecutar un script de entrenamiento o para hospedar la implementación de un servicio. En este experimento se usa un proceso en la nube. 

    1. Configure el contexto del proceso de este experimento.
        
        Campo | Valor
        ----|---
        Nombre del proceso |  Escriba un nombre único que identifique el contexto del proceso. En este ejemplo, se usa **automl-compute**.
        Tamaño de la máquina virtual| Seleccione el tamaño de la máquina virtual para el proceso. Use el valor predeterminado **Standard_DS12_V2**.
        Configuración adicional| *Min node* (Nodos mín.): 1. Para habilitar la generación de perfiles de datos, debe tener uno o más nodos. <br> *Max node* (Nodos máx.): 6.
 
    1. Para crear el proceso nuevo, seleccione **Crear**. Tarda unos minutos en completarse. 

    1. Una vez finalizada la creación, seleccione el proceso nuevo en la lista desplegable y, después, seleccione **Siguiente**.

    >[!NOTE]
    >En este tutorial, se usará la cuenta de almacenamiento y el contenedor predeterminados que se han creado con el nuevo proceso. Se rellenan de manera automática en el formulario.

1. Seleccione **Upload from local file** (Cargar desde archivo local). Desde aquí, cree un nuevo conjunto de datos con el archivo **bankmarketing_train.csv** que descargó anteriormente para este tutorial. 

    1. Seleccione **Browse** (Examinar) y, a continuación, seleccione el archivo **bankmarketing_train.csv** en el equipo local. 

    1. Asigne un nombre único al conjunto de datos y proporcione una descripción opcional. 

    1. Seleccione **Next** (Siguiente) para cargarlo en el contenedor predeterminado que se configuró automáticamente durante la creación del área de trabajo. La versión preliminar pública solo admite cargas de archivos locales. 

    1. Una vez completada la carga, el formulario de **configuración y versión preliminar** se rellena de forma inteligente en función del tipo de archivo. Asegúrese de que el formulario se rellene como se indica a continuación.
        
        Campo|Valor
        ---|---
        Formato de archivo| Delimitado
        Delimitador| Coma
        Encoding| UTF-8
        Encabezados de columna| Todos los archivos tienen los mismos encabezados
        Omitir filas | None

        >[!NOTE]
        > Si algunos de los valores de este formulario se actualizan, la versión preliminar se actualizará en consecuencia.

        Seleccione **Next** (Siguiente).
    

    1. El formulario **Scheme** (Esquema) permite una configuración adicional de los datos para este experimento. En este ejemplo, seleccione el modificador de alternancia de la característica **day_of_week**, de modo que no se incluya para este experimento. Seleccione **Done** (Listo) para completar la carga de archivos y la creación del conjunto de datos para el experimento.

        ![Configuración de la pestaña Preview (Versión preliminar)](media/tutorial-1st-experiment-automated-ml/schema-tab-config.gif)

        
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

   Cuando se inicie el experimento, verá una pantalla en blanco con un mensaje de estado en la parte superior.

El proceso de preparación del experimento tarda unos minutos. Cuando finaliza el proceso, el mensaje de estado cambia a **Run is running** (Ejecución en marcha).

##  <a name="view-experiment-details"></a>Visualización de los detalles del experimento

A medida que el experimento progresa, la pantalla actualiza el **gráfico de iteración** y la **lista de iteraciones** con las distintas iteraciones (modelos) que se ejecutan. La lista de iteraciones está ordenada por puntuación de métrica. De manera predeterminada, el modelo con la puntuación más alta según la métrica **AUC_weighted** aparece en la parte superior de la lista.

>[!WARNING]
> Los trabajos de entrenamiento tardan varios minutos para que cada canalización termine de ejecutarse.

[![Panel de detalles de ejecución](media/tutorial-1st-experiment-automated-ml/run-details.png)](media/tutorial-1st-experiment-automated-ml/run-details-expanded.png#lightbox)

## <a name="deploy-the-model"></a>Implementación del modelo

Mediante el uso del aprendizaje automático automatizado en la página de aterrizaje del área de trabajo se puede implementar el mejor modelo como un servicio web en pocos pasos. La implementación es la integración del modelo para que pueda predecir datos nuevos e identificar posibles áreas de oportunidad. En este experimento, la implementación em un servicio web significa que la institución financiera tiene ahora una solución web iterativa y escalable para identificar posibles clientes de depósitos a plazo fijo. 

En el contexto de este experimento, **VotingEnsemble** se considera el mejor modelo, según la métrica **AUC_weighted**.  Se implementa este modelo, pero se recomienda que la implementación tarda unos 20 minutos en completarse. El proceso de implementación conlleva varios pasos, como el registro del modelo, la generación de recursos y su configuración para el servicio web.

1. En la página **Run Detail** (Detalle de ejecución), seleccione el botón **Deploy Best Model** (Implementar el mejor modelo) en la esquina superior derecha.

1. Rellene el panel **Deploy Best Model** (Implementar el mejor modelo) como se indica a continuación:

    Campo| Valor
    ----|----
    Deployment name (Nombre de implementación)| my-automl-deploy
    Deployment description (Descripción de implementación)| Implementación de mi primer experimento de aprendizaje automático automatizado
    Scoring script (Script de puntuación)| Autogenerate (Generar automáticamente)
    Environment script (Script del entorno)| Autogenerate (Generar automáticamente)
    
1. Seleccione **Implementar**.  

    Aparece un mensaje de implementación completa cuando la implementación finaliza correctamente.
    
Ya tiene un servicio web operativo para generar predicciones.

## <a name="clean-up-resources"></a>Limpieza de recursos

Los archivos de implementación son mayores que los archivos de datos y del experimento, por lo que cuesta más almacenarlos. Elimine solo los archivos de implementación para minimizar costos en su cuenta y si desea conservar el área de trabajo y los archivos del experimento. Elimine todo el grupo de recursos completo si no planea usar ninguno de los archivos.  

### <a name="delete-the-deployment-instance"></a>Eliminación de la instancia de implementación

Elimine solo la instancia de implementación de Azure Portal, si desea mantener el área de trabajo y el grupo de recursos para otros tutoriales y para explorarlos. 

1. Vaya a [Azure Portal](https://portal.azure.com//). Vaya hasta el área de trabajo y, a la izquierda, en el panel **Assets** (Recursos), seleccione **Deployments** (Implementaciones). 

1. Seleccione la implementación que desea eliminar y seleccione **Eliminar.** 

1. Seleccione **Continuar**.

### <a name="delete-the-resource-group"></a>Eliminar el grupo de recursos

[!INCLUDE [aml-delete-resource-group](../../../includes/aml-delete-resource-group.md)]

## <a name="next-steps"></a>Pasos siguientes

En este tutorial de aprendizaje automático automatizado, ha usado la página de aterrizaje del área de trabajo para crear e implementar un modelo de clasificación. Para más información y ver los pasos siguientes, consulte estos artículos:

> [!div class="nextstepaction"]
> [Consumo de un servicio web](how-to-consume-web-service.md)

+ Más información acerca del [preprocesamiento](how-to-create-portal-experiments.md#preprocess).
+ Más información acerca de la [generación de perfiles de datos](how-to-create-portal-experiments.md#profile).
+ Más información acerca del [aprendizaje automático automatizado](concept-automated-ml.md).

>[!NOTE]
> Este conjunto de conjuntos de marketing bancario está disponible bajo [licencia de Creative Commons (CCO: dominio público)](https://creativecommons.org/publicdomain/zero/1.0/). Todos los derechos de los contenidos individuales de la base de datos tienen la licencia [Database Contents License](https://creativecommons.org/publicdomain/zero/1.0/) y están disponibles en [Kaggle](https://www.kaggle.com/janiobachmann/bank-marketing-dataset). Este conjunto de datos estaba disponible originalmente en la [base de datos de aprendizaje automático de UCI](https://archive.ics.uci.edu/ml/datasets/bank+marketing).<br><br>
> [Moro et al., 2014] S. Moro, P. Cortez y P. Rita. A Data-Driven Approach to Predict the Success of Bank Telemarketing. Decision Support Systems, Elsevier, 62:22-31, June 2014.
