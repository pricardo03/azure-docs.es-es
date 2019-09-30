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
ms.date: 09/26/2019
ms.openlocfilehash: 38c319fb89e8c763f8231c18cbb59bef099193e2
ms.sourcegitcommit: 55f7fc8fe5f6d874d5e886cb014e2070f49f3b94
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/25/2019
ms.locfileid: "71259328"
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

1. Seleccione **Automated ML** (ML automatizado) en la sección **Authoring** (Creación), en el panel de la izquierda.

   Verá la pantalla Getting started (Introducción), ya que este es el primer experimento con aprendizaje automático automatizado.

   ![Azure Machine Learning Studio](media/tutorial-1st-experiment-automated-ml/get-started.png)

1. Seleccione **Create Experiment** (Crear experimento). 

1. Escriba el nombre del experimento: `my-1st-automl-experiment`.

1. Seleccione **Create a new compute** (Crear un proceso) y configure el destino de proceso. Un destino de proceso es un entorno de recursos locales o en la nube que se usa para ejecutar un script de entrenamiento o para hospedar la implementación de un servicio. En este experimento se usa un proceso en la nube. 

   Campo | DESCRIPCIÓN | Valor para el tutorial
   ----|---|---
   Nombre del proceso |Un nombre único que identifique el contexto del proceso.|automl-compute
   Tamaño de la&nbsp;máquina&nbsp;virtual| Seleccione el tamaño de la máquina virtual para el proceso.|Standard_DS12_V2
   Número máximo y mínimo de nodos (en Configuración avanzada)| Para generar perfiles de datos, debe especificar uno o más nodos.|Número mínimo de nodos: 1<br>Número máximo de nodos: 6

   >[!NOTE]
   >En este tutorial, se usará la cuenta de almacenamiento y el contenedor predeterminados que se han creado con el nuevo proceso. Se rellenan de manera automática en el formulario.
    
1. Seleccione **Create** (Crear) para obtener el destino de proceso. 

   **Tarda unos minutos en completarse.** 

1. Después de la creación, seleccione el nuevo destino de proceso en la lista desplegable y seleccione **Next** (Siguiente).

1. Seleccione **Upload from local file** (Cargar desde archivo local) para empezar a crear un nuevo conjunto de datos. 

    1. Seleccione **Examinar**.
    
    1. Seleccione el archivo **bankmarketing_train.csv** en el equipo local. Este es el archivo que descargó como [requisito previo](https://automlsamplenotebookdata.blob.core.windows.net/automl-sample-notebook-data/bankmarketing_train.csv).

    1. Asigne un nombre único al conjunto de datos y proporcione una descripción opcional. 

    1. Seleccione **Next** (Siguiente) en la parte inferior izquierda para cargarlo en el contenedor predeterminado que se configuró automáticamente durante la creación del área de trabajo. La versión preliminar pública solo admite cargas de archivos locales. 
    
       Una vez completada la carga, el formulario de configuración y vista previa se rellena de forma inteligente en función del tipo de archivo. 
       
    1. Compruebe que el formulario **Settings y Preview** (Configuración y vista previa) se rellenan como se indica a continuación y seleccione **Next** (Siguiente).
        
        Campo|Valor para el tutorial
        ---|---
        Formato de archivo| Delimitado
        Delimitador| Coma
        Encoding| UTF-8
        Encabezados de columna| Todos los archivos tienen los mismos encabezados
        Omitir filas | None
    
    1. El formulario **Scheme** (Esquema) permite una configuración adicional de los datos para este experimento. En este ejemplo, seleccione el modificador de alternancia de la característica **day_of_week**, de modo que no se incluya para este experimento. Seleccione **Done** (Listo) para completar la carga de archivos y la creación del conjunto de datos para el experimento.

        ![Configuración de la pestaña Preview (Versión preliminar)](media/tutorial-1st-experiment-automated-ml/schema-tab-config.gif)

1. Seleccione **Classification** (Clasificación) como tarea de predicción.

1. Seleccione **y** como la columna de destino en la que desea realizar las predicciones. Esta columna indica si el cliente se suscribió a un depósito a plazo o no.

1. Expanda **Configuración avanzada** y rellene los campos como se indica a continuación.

   >[!NOTE]
   > En este tutorial, no se va a establecer una puntuación de métricas ni núcleos máximos por umbral de iteraciones. Tampoco se impedirá que se pruebe ningún algoritmo.
   
   Configuración&nbsp;avanzada|DESCRIPCIÓN|Valor&nbsp;para&nbsp;tutorial
   ------|---------|---
   Métrica principal| Métrica de evaluación por la que se medirá el algoritmo de aprendizaje automático.|AUC_weighted
   Exit criteria (Criterios de salida)| Si se cumplen los criterios, se detiene el trabajo de entrenamiento. |Tiempo de&nbsp;entrenamiento&nbsp;del trabajo: 5 <br> <br> Número máximo&nbsp;#&nbsp;de&nbsp;iteraciones&#58;10
   Preprocessing (Preprocesamiento)| Habilita el preprocesamiento que realiza el aprendizaje automático automatizado. Aquí se incluyen la limpieza, preparación y transformación automáticas de los datos para generar características sintéticas.| Habilitar
   Tipo de validación | Elija un tipo de validación cruzada.|Validación cruzada con k iteraciones
   Número de validaciones | Número de pruebas. | Dos validaciones cruzadas 
   Simultaneidad| El número máximo de iteraciones simultáneas.|5
   
1. Para ejecutar el experimento, seleccione **Iniciar**. Aparece una pantalla con un mensaje de estado cuando comienza la preparación del experimento.

>[!IMPORTANT]
> La preparación necesita de **10 a 15 minutos** en preparar la ejecución del experimento. Una vez que se ejecuta, se tarda de **2 a 3 minutos más para cada iteración**.  
>
> En producción, probablemente puede descansar un poco. Pero para este tutorial, se recomienda empezar a explorar los resultados de la iteración a medida que se completan mientras los demás siguen en ejecución. 

##  <a name="explore-iteration-results"></a>Exploración de los resultados de la iteración

A medida que el experimento progresa, la pantalla actualiza el **gráfico de iteración** y la **lista de iteraciones** con las distintas iteraciones (modelos) creadas a medida que se completan y las ordena por puntuación métrica. De manera predeterminada, el modelo con la puntuación más alta según la métrica **AUC_weighted** elegida aparece en la parte superior de la lista.

Mientras espera a que finalicen todas las iteraciones del experimento, seleccione el **nombre** de una iteración completada para explorar los detalles de rendimiento. 
   
A continuación se muestran los gráficos y las métricas de ejecución generadas para cada iteración, como una curva de precisión-recuperación, una matriz de confusión, puntuaciones de precisión ponderadas, etc. 

![Detalles de la ejecución de iteración](media/tutorial-1st-experiment-automated-ml/run-detail.gif)

## <a name="deploy-the-model"></a>Implementación del modelo

El aprendizaje automático automatizado de la página de aterrizaje del área de trabajo permite implementar el mejor modelo como un servicio web en pocos pasos. La implementación es la integración del modelo para que pueda predecir datos nuevos e identificar posibles áreas de oportunidad. En este experimento, la implementación em un servicio web significa que la institución financiera tiene ahora una solución web iterativa y escalable para identificar posibles clientes de depósitos a plazo fijo. 

Una vez completada la ejecución, vuelva a la página de detalles **Iteration chart** (Gráfico de iteración) y **Iterations list** (Lista de iteraciones). 

En el contexto de este experimento, **VotingEnsemble** se considera el mejor modelo, según la métrica **AUC_weighted**.  Se implementa este modelo, pero se recomienda que la implementación tarda unos 20 minutos en completarse. El proceso de implementación conlleva varios pasos, como el registro del modelo, la generación de recursos y su configuración para el servicio web.

1. Seleccione el botón **Deploy Best Model** (Implementar el mejor modelo) en la esquina superior derecha.

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
