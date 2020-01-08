---
title: Ejecución de predicciones por lotes mediante el diseñador de Azure Machine Learning (versión preliminar)
titleSuffix: Azure Machine Learning
description: Aprenda a entrenar un modelo y a configurar una canalización de predicción por lotes mediante el diseñador. Implemente la canalización como servicio web con parámetros que se pueda desencadenar desde cualquier biblioteca HTTP.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
ms.reviewer: trbye
ms.author: trbye
author: trevorbye
ms.date: 11/19/2019
ms.custom: Ignite2019
ms.openlocfilehash: 8d80282044adfa723940aa6f68efc1e719e713c0
ms.sourcegitcommit: ce4a99b493f8cf2d2fd4e29d9ba92f5f942a754c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/28/2019
ms.locfileid: "75532058"
---
# <a name="run-batch-predictions-using-azure-machine-learning-designer"></a>Ejecución de predicciones por lotes mediante el diseñador de Azure Machine Learning
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

En esta guía paso a paso aprenderá a usar el diseñador para entrenar un modelo y configurar una canalización de predicción por lotes y un servicio web. La predicción por lotes permite la puntuación continua y a petición de modelos entrenados en grandes conjuntos de datos, configurados opcionalmente como servicio web que se puede desencadenar desde cualquier biblioteca HTTP. 

Para configurar los servicios de puntuación por lotes con el SDK, consulte la [guía paso a paso](how-to-run-batch-predictions.md) adjunta.

En esta guía paso a paso aprenderá a realizar las tareas siguientes:

> [!div class="checklist"]
> * Creación de un experimento de Machine Learning básico en una canalización
> * Creación de una canalización de inferencias por lotes con parámetros
> * Administración y ejecución de canalizaciones manualmente o desde un punto de conexión de REST

## <a name="prerequisites"></a>Prerequisites

1. Si no tiene una suscripción a Azure, cree una cuenta gratuita antes de empezar. Pruebe la [versión gratuita o de pago de Azure Machine Learning](https://aka.ms/AMLFree).

1. Cree un [área de trabajo](tutorial-1st-experiment-sdk-setup.md).

1. Inicie sesión en [Azure Machine Learning Studio](https://ml.azure.com/).

En esta guía paso a paso se asume el conocimiento básico sobre la compilación de una canalización sencilla en el diseñador. Para una introducción guiada al diseñador, complete el [tutorial](tutorial-designer-automobile-price-train-score.md). 

## <a name="create-a-pipeline"></a>Crear una canalización

Para crear una canalización de inferencias por lotes, primero necesita un experimento de aprendizaje automático. Para crear uno, vaya a la pestaña **Designer** (Diseñador) en el área de trabajo y cree una canalización; para ello, seleccione la opción **Easy-to-use prebuilt modules** (Módulos precompilados fáciles de usar).

![Página principal del diseñador](./media/how-to-run-batch-predictions-designer/designer-batch-scoring-1.png)

A continuación se facilita un modelo de aprendizaje automático sencillo para fines de demostración. Los datos son un conjunto de datos registrado creado a partir de los datos sobre diabetes de Azure Open Datasets. Consulte la [sección de guía paso a paso](how-to-create-register-datasets.md#create-datasets-with-azure-open-datasets) para registrar conjuntos de datos de Azure Open Datasets. Los datos se dividen en conjuntos de entrenamiento y validación, y se entrena y se puntúa un árbol de decisión impulsado. La canalización debe ejecutarse al menos una vez para poder crear una canalización de inferencia. Haga clic en el botón **Run** (Ejecutar) para ejecutar la canalización.

![Creación de un experimente sencillo](./media/how-to-run-batch-predictions-designer/designer-batch-scoring-2.png)

## <a name="create-a-batch-inference-pipeline"></a>Creación de una canalización de inferencias por lotes

Ahora que se ha ejecutado la canalización, hay una nueva opción disponible junto a **Run** (Ejecutar) y **Publish** (Publicar) denominada **Create inference pipeline** (Crear canalización de inferencia). Haga clic en la lista desplegable y seleccione **Batch inference pipeline** (Canalización de inferencias por lotes).

![Creación de una canalización de inferencias por lotes](./media/how-to-run-batch-predictions-designer/designer-batch-scoring-5.png)

El resultado es una canalización de inferencias por lotes predeterminada. Esto incluye un nodo para la configuración del experimento de canalización original, un nodo para los datos sin procesar para la puntuación y un nodo para puntuar los datos sin procesar en la canalización original.

![Canalización de inferencias por lotes predeterminada](./media/how-to-run-batch-predictions-designer/designer-batch-scoring-6.png)

Puede agregar otros nodos para cambiar el comportamiento del proceso de inferencia por lotes. En este ejemplo se agrega un nodo para el muestreo aleatorio de los datos de entrada antes de la puntuación. Cree un nodo **Partition and Sample** (Partición y muestra) y colóquelo entre los nodos datos sin procesar y puntuación. A continuación, haga clic en el nodo **Partition and Sample** (Partición y muestra) para obtener acceso a los parámetros y la configuración.

![Nuevo nodo](./media/how-to-run-batch-predictions-designer/designer-batch-scoring-7.png)

El parámetro *Rate of sampling* (Frecuencia de muestreo) controla el porcentaje del conjunto de datos original del que se tomará una muestra aleatoria. Se trata de un parámetro que será útil para ajustar con frecuencia, por lo que se habilita como parámetro de canalización. Los parámetros de canalización se pueden cambiar en tiempo de ejecución y se pueden especificar en un objeto de carga al volver a ejecutar la canalización desde un punto de conexión REST. 

Para habilitar este campo como parámetro de canalización, haga clic en los puntos suspensivos situados sobre el campo y luego en **Add to pipeline parameter** (Agregar al parámetro de canalización). 

![Configuración de la muestra](./media/how-to-run-batch-predictions-designer/designer-batch-scoring-8.png)

A continuación, asígnele un nombre y un valor predeterminado al parámetro. El nombre se utilizará para identificar el parámetro y lo especificará en una llamada a REST.

![Parámetro Pipeline](./media/how-to-run-batch-predictions-designer/designer-batch-scoring-9.png)

## <a name="deploy-batch-inferencing-pipeline"></a>Implementación de la canalización de inferencias por lotes

Ahora está listo para implementar la canalización. Haga clic en el botón **Deploy** (Implementar) que abre la interfaz para configurar un punto de conexión. Haga clic en la lista desplegable y seleccione **New PipelineEndpoint** (Nuevo punto de conexión de canalización).

![Implementación de la canalización](./media/how-to-run-batch-predictions-designer/designer-batch-scoring-10.png)

Dele un nombre y una descripción opcional al punto de conexión. Cerca de la parte inferior, verá el parámetro `sample-rate` configurado con un valor predeterminado de 0,8. Cuando esté listo, haga clic en **Deploy** (Implementar).

![Configuración del punto de conexión](./media/how-to-run-batch-predictions-designer/designer-batch-scoring-11.png)

## <a name="manage-endpoints"></a>Administrar puntos de conexión 

Una vez finalizada la implementación, vaya a la pestaña **Endpoints** (Puntos de conexión) y haga clic en el nombre del punto de conexión que acaba de crear.

![Vínculo al punto de conexión](./media/how-to-run-batch-predictions-designer/designer-batch-scoring-12.png)

En esta pantalla se muestran todas las canalizaciones publicadas en el punto de conexión específico. Haga clic en la canalización de inferencia.

![Canalización de inferencia](./media/how-to-run-batch-predictions-designer/designer-batch-scoring-13.png)

En la página de detalles de la canalización se muestran el historial de ejecuciones detallado y la información de la cadena de conexión de la canalización. Haga clic en el botón **Run** (Ejecutar) para crear una ejecución manual de la canalización.

![Detalles de la canalización](./media/how-to-run-batch-predictions-designer/designer-batch-scoring-14.png)

En la configuración de ejecución puede proporcionar una descripción para la ejecución y cambiar el valor de los parámetros de canalización. Esta vez, vuelva a ejecutar la canalización de inferencia con una frecuencia de muestreo de 0,9. Haga clic en **Run** (Ejecutar) para ejecutar la canalización.

![Ejecución de la canalización](./media/how-to-run-batch-predictions-designer/designer-batch-scoring-15.png)

La pestaña **Consume** (Consumir) contiene el punto de conexión REST para volver a ejecutar la canalización. Para hacer una llamada a REST, necesitará un encabezado de autenticación de tipo portador de OAuth 2.0. Consulte la siguiente [sección de tutorial](tutorial-pipeline-batch-scoring-classification.md#publish-and-run-from-a-rest-endpoint) para más información sobre la configuración de la autenticación en el área de trabajo y la realización de una llamada a REST con parámetros.

## <a name="next-steps"></a>Pasos siguientes

Siga el [tutorial](tutorial-designer-automobile-price-train-score.md) del diseñador para entrenar e implementar un modelo de regresión.
