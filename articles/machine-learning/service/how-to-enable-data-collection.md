---
title: 'Habilitar la recopilación de datos de modelos en producción: Azure Machine Learning'
description: Obtenga información sobre cómo recopilar datos de modelo de entrada de Azure Machine Learning en Azure Blob Storage.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: marthalc
author: marthalc
ms.date: 09/24/2018
ms.openlocfilehash: 2c5b4607fda6bc3c6b990096feb55da7a3886b8e
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46959876"
---
# <a name="collect-data-for-models-in-production"></a>Recopilar datos de modelos en producción

En este artículo, aprenderá cómo recopilar datos de modelo de entrada de las instancias de Azure Machine Learning Services que ha implementado en el clúster de Azure Kubernetes (AKS) en Azure Blob Storage. 

Una vez habilitado, estos datos que recopile le ayudarán a:
* Supervisar datos flotantes a medida que los datos de producción entran en el modelo

* Tomar mejores decisiones sobre cuándo volver a entrenar u optimizar el modelo

* Volver a entrenar el modelo con los datos recopilados

## <a name="what-is-collected-and-where-does-it-go"></a>¿Qué información se recopila y a dónde va?

Se pueden recopilar los siguientes datos:
* Datos de **entrada** de modelo de servicios web implementados en el clúster de Azure Kubernetes (AKS) (**no** se recopilan los datos de voz, imágenes y vídeo) 
  
* Predicciones de modelo con datos de entrada de producción.

> [!Note]
> La agregación previa o los cálculos previos en estos datos no forman parte del servicio en este momento.   

La salida se guarda en un blob de Azure. Puesto que los datos se agregan en un blob de Azure, puede elegir su herramienta favorita para ejecutar el análisis. 

La ruta de acceso a los datos de salida en el blob sigue esta sintaxis:

```
/modeldata/<subscriptionid>/<resourcegroup>/<workspace>/<webservice>/<model>/<version>/<identifier>/<year>/<month>/<day>/data.csv
# example: /modeldata/92c76a2f-0e1c-4216-b65e-abf7a3f34c1e/myresourcegrp/myWorkspace/aks-w-collv9/best_model/10/inputs/2018/12/31/data.csv
```

## <a name="prerequisites"></a>Requisitos previos

- Una suscripción de Azure. Si no tiene una, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

- Un área de trabajo de Azure Machine Learning, un directorio local que contiene los scripts y el SDK de Azure Machine Learning de Python instalado. Descubra cómo obtener estos requisitos previos con el documento [How to configure a development environment](how-to-configure-environment.md) (Cómo configurar un entorno de desarrollo).

- Un modelo de aprendizaje automático entrenado para implementarse en Azure Kubernetes Service (AKS). Si no tiene uno, consulte el tutorial [Train an image classification model](tutorial-train-models-with-aml.md) (Entrenar un modelo de clasificación de imágenes).

- Un [clúster de AKS](how-to-deploy-to-aks.md).

- Las siguientes dependencias y módulo instalados [en su entorno](how-to-configure-environment.md):
  + En Linux:
    ```shell
    sudo apt-get install libxml++2.6-2v5
    pip install azureml-monitoring
    ```

  + En Windows:
    ```shell
    pip install azureml-monitoring
    ```

## <a name="enable-data-collection"></a>Habilitación de recolección de datos
La recopilación de datos se puede habilitar independientemente del modelo que se implementa a través del servicio Azure Machine Learning u otras herramientas. 

Para habilitarla, debe:

1. Abrir el archivo de puntuación. 

1. Agregue el código siguiente en la parte superior del archivo:

   ```python 
   from azureml.monitoring import ModelDataCollector
   ```

2. Declare las variables de recopilación de datos en su función `init()`:

    ```python
    global inputs_dc, prediction_dc
    inputs_dc = ModelDataCollector("best_model", identifier="inputs", feature_names=["feat1", "feat2", "feat3". "feat4", "feat5", "feat6"])
    prediction_dc = ModelDataCollector("best_model", identifier="predictions", feature_names=["prediction1", "prediction2"])
    ```

    *CorrelationId* es un parámetro opcional, no es necesario configurarlo si el modelo no lo requiere. Disponer de un correlationId facilita la asignación con otros datos. (Algunos ejemplos son: LoanNumber, CustomerId, etc.)
    
    *Identifier* se usa después para crear la estructura de carpetas en el blob, se puede usar para dividir datos "sin procesar" frente a "procesados".

3.  Agregue las líneas de código siguientes en la función `run(input_df)`:

    ```python
    data = np.array(data)
    result = model.predict(data)
    inputs_dc.collect(data) #this call is saving our input data into Azure Blob
    prediction_dc.collect(result) #this call is saving our input data into Azure Blob
    ```

4. La recopilación de datos **no** se establece automáticamente en **true** al implementar un servicio en AKS, por lo que debe actualizar el archivo de configuración, como: 

    ```python
    aks_config = AksWebservice.deploy_configuration(collect_model_data=True)
    ```
    AppInsights para la supervisión del servicio también se puede activar si cambia esta configuración:
    ```python
    aks_config = AksWebservice.deploy_configuration(collect_model_data=True, enable_app_insights=True)
    ``` 

5. [Cree una imagen e implemente su servicio.](how-to-deploy-to-aks.md) 


Si ya tiene un servicio con las dependencias que se instalan en su **archivo de entorno** y **archivo de puntuación**, habilite la recopilación de datos:

1. Vaya a [Azure Portal](https://portal.azure.com).

1. Abra el área de trabajo.

1. Vaya a **Implementaciones** -> **Seleccionar servicio** -> **Editar**.

   ![Editar servicio](media/how-to-enable-data-collection/EditService.png)

1. En **Configuración avanzada**, anule la selección de **Habilitar recopilación de datos de modelos**. 

   ![Desactive la recopilación de datos](media/how-to-enable-data-collection/CheckDataCollection.png)

   En esta ventana, también puede elegir "Habilitar diagnósticos de AppInsights" para realizar un seguimiento del estado de su servicio.  

1. Seleccione **Actualizar** para aplicar el cambio.


## <a name="disable-data-collection"></a>Deshabilitar la recopilación de datos
Puede dejar de recopilar datos en cualquier momento. Use el código de Python o Azure Portal para deshabilitar la recopilación de datos.

+ Opción 1: deshabilitar en Azure Portal: 
  1. Inicie sesión en el [portal de Azure](https://portal.azure.com).

  1. Abra el área de trabajo.

  1. Vaya a **Implementaciones** -> **Seleccionar servicio** -> **Editar**.

     ![Editar servicio](media/how-to-enable-data-collection/EditService.png)

  1. En **Configuración avanzada**, anule la selección de **Habilitar recopilación de datos de modelos**. 

     ![Desactive la recopilación de datos](media/how-to-enable-data-collection/UncheckDataCollection.png) 

  1. Seleccione **Actualizar** para aplicar el cambio.

* Opción 2: uso de Python para deshabilitar la recopilación de datos:

  ```python 
  ## replace <service_name> with the name of the web service
  <service_name>.update(collect_model_data=False)
  ```

## <a name="example-notebook"></a>Cuaderno de ejemplo

El cuaderno `00.Getting Started/12.enable-data-collection-for-models-in-aks.ipynb` demuestra los conceptos de este artículo.  

Obtenga este cuaderno:
 
[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]