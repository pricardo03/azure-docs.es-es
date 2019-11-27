---
title: Recopilar datos en los modelos de producción
titleSuffix: Azure Machine Learning
description: Obtenga información sobre cómo recopilar datos de modelo de entrada de Azure Machine Learning en Azure Blob Storage.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: laobri
ms.author: copeters
author: lostmygithubaccount
ms.date: 10/15/2019
ms.custom: seodec18
ms.openlocfilehash: 20bc148e392900aecb63ad393ec6e90cda65585a
ms.sourcegitcommit: 35715a7df8e476286e3fee954818ae1278cef1fc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/08/2019
ms.locfileid: "73839096"
---
# <a name="collect-data-for-models-in-production"></a>Recopilar datos de modelos en producción
[!INCLUDE [applies-to-skus](../../../includes/aml-applies-to-basic-enterprise-sku.md)]

>[!IMPORTANT]
> Aunque este SDK se retirará pronto, sigue siendo adecuado para los desarrolladores que supervisan el desplazamiento de los datos en los modelos; sin embargo, la mayoría de los desarrolladores deben usar la [supervisión de datos simplificada con Application Insights ](https://docs.microsoft.com/azure/machine-learning/service/how-to-enable-app-insights). 

En este artículo, aprenderá cómo recopilar datos de modelo de entrada de las instancias de Azure Machine Learning que ha implementado en el clúster de Azure Kubernetes (AKS) en Azure Blob Storage. 

Una vez habilitado, estos datos que recopile le ayudarán a:
* [Supervisar datos flotantes](how-to-monitor-data-drift.md) a medida que los datos de producción entran en el modelo

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
/modeldata/<subscriptionid>/<resourcegroup>/<workspace>/<webservice>/<model>/<version>/<designation>/<year>/<month>/<day>/data.csv
# example: /modeldata/1a2b3c4d-5e6f-7g8h-9i10-j11k12l13m14/myresourcegrp/myWorkspace/aks-w-collv9/best_model/10/inputs/2018/12/31/data.csv
```

>[!Note]
> En las versiones del SDK anteriores a la `0.1.0a16`, el argumento `designation` se llamaba `identifier`. Si el código se desarrolló con una versión anterior, tendrá que actualizarlo en consecuencia.

## <a name="prerequisites"></a>Requisitos previos

- Si no tiene una suscripción a Azure, cree una cuenta gratuita antes de empezar. Pruebe hoy mismo la [versión gratuita o de pago de Azure Machine Learning](https://aka.ms/AMLFree).

- Un área de trabajo de Azure Machine Learning, un directorio local que contenga los scripts y el SDK de Azure Machine Learning para Python instalado. Descubra cómo obtener estos requisitos previos con el documento sobre [cómo configurar un entorno de desarrollo](how-to-configure-environment.md).

- Un modelo de aprendizaje automático entrenado para implementarse en Azure Kubernetes Service (AKS). Si no tiene uno, consulte el tutorial sobre [entrenamiento de un modelo de clasificación de imágenes](tutorial-train-models-with-aml.md).

- Un clúster de Azure Kubernetes Service. Para obtener información sobre cómo crear e implementar elementos en uno, consulte el documento [Cómo implementar y dónde](how-to-deploy-and-where.md).

- [Configuración del entorno](how-to-configure-environment.md) e instalación del [SDK de supervisión](https://aka.ms/aml-monitoring-sdk).

## <a name="enable-data-collection"></a>Habilitación de recolección de datos
La recopilación de datos se puede habilitar independientemente del modelo que se implementa a través de Azure Machine Learning u otras herramientas. 

Para habilitarla, debe:

1. Abrir el archivo de puntuación.

1. Agregue el [código siguiente](https://aka.ms/aml-monitoring-sdk) en la parte superior del archivo:

   ```python 
   from azureml.monitoring import ModelDataCollector
   ```

2. Declare las variables de recopilación de datos en su función `init()`:

    ```python
    global inputs_dc, prediction_dc
    inputs_dc = ModelDataCollector("best_model", designation="inputs", feature_names=["feat1", "feat2", "feat3". "feat4", "feat5", "feat6"])
    prediction_dc = ModelDataCollector("best_model", designation="predictions", feature_names=["prediction1", "prediction2"])
    ```

    *CorrelationId* es un parámetro opcional, no es necesario configurarlo si el modelo no lo requiere. Disponer de un correlationId facilita la asignación con otros datos. (Algunos ejemplos incluyen: LoanNumber, CustomerId, etc.).
    
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

5. Para crear una imagen e implementar el servicio, consulte el documento [Cómo implementar y dónde](how-to-deploy-and-where.md).


Si ya tiene un servicio con las dependencias que se instalan en su **archivo de entorno** y **archivo de puntuación**, habilite la recopilación de datos:

1. Vaya a [Azure Machine Learning Studio](https://ml.azure.com).

1. Abra el área de trabajo.

1. Vaya a **Implementaciones** -> **Seleccionar servicio** -> **Editar**.

   ![Editar servicio](media/how-to-enable-data-collection/EditService.PNG)

1. En **Configuración avanzada**, seleccione **Habilitar recopilación de datos de modelos**.

    [![Activar la colección de datos](media/how-to-enable-data-collection/CheckDataCollection.png)](./media/how-to-enable-data-collection/CheckDataCollection.png#lightbox)

   En esta ventana, también puede elegir "Habilitar diagnósticos de AppInsights" para realizar un seguimiento del estado de su servicio.

1. Seleccione **Actualizar** para aplicar el cambio.


## <a name="disable-data-collection"></a>Deshabilitar la recopilación de datos
Puede dejar de recopilar datos en cualquier momento. Use el código de Python o Azure Machine Learning Studio para deshabilitar la recopilación de datos.

+ Opción 1: Deshabilitar en Azure Machine Learning Studio: 
  1. Inicie sesión en [Azure Machine Learning Studio](https://ml.azure.com).

  1. Abra el área de trabajo.

  1. Vaya a **Implementaciones** -> **Seleccionar servicio** -> **Editar**.

     [![Editar la opción](media/how-to-enable-data-collection/EditService.PNG)](./media/how-to-enable-data-collection/EditService.PNG#lightbox)

  1. En **Configuración avanzada**, anule la selección de **Habilitar recopilación de datos de modelos**.

     [![Desactivar la colección de datos](media/how-to-enable-data-collection/UncheckDataCollection.png)](./media/how-to-enable-data-collection/UncheckDataCollection.png#lightbox)

  1. Seleccione **Actualizar** para aplicar el cambio.

  También puede acceder a esta configuración en el área de trabajo de [Azure Machine Learning Studio](https://ml.azure.com).

+ Opción 2: uso de Python para deshabilitar la recopilación de datos:

  ```python 
  ## replace <service_name> with the name of the web service
  <service_name>.update(collect_model_data=False)
  ```

## <a name="validate-your-data-and-analyze-it"></a>Validación y análisis de los datos
Puede elegir la herramienta que prefiera para analizar los datos recopilados en el blob de Azure.

Para acceder rápidamente a los datos del blob:

1. Inicie sesión en [Azure Machine Learning Studio](https://ml.azure.com).

1. Abra el área de trabajo.
1. Haga clic en **Almacenamiento**.

    [![Almacenamiento](media/how-to-enable-data-collection/StorageLocation.png)](./media/how-to-enable-data-collection/StorageLocation.png#lightbox)

1. Siga la ruta de acceso a los datos de salida del blob utilizando esta sintaxis:

```
/modeldata/<subscriptionid>/<resourcegroup>/<workspace>/<webservice>/<model>/<version>/<designation>/<year>/<month>/<day>/data.csv
# example: /modeldata/1a2b3c4d-5e6f-7g8h-9i10-j11k12l13m14/myresourcegrp/myWorkspace/aks-w-collv9/best_model/10/inputs/2018/12/31/data.csv
```


### <a name="analyzing-model-data-through-power-bi"></a>Análisis de los datos del modelo con Power BI

1. Descargue y abra [Power BI Desktop](https://www.powerbi.com).

1. Seleccione **Obtener datos** y haga clic en [**Azure Blob Storage**](https://docs.microsoft.com/power-bi/desktop-data-sources).

    [![Configuración del blob de PBI](media/how-to-enable-data-collection/PBIBlob.png)](./media/how-to-enable-data-collection/PBIBlob.png#lightbox)


1. Agregue el nombre de la cuenta de almacenamiento y escriba la clave de almacenamiento. Encontrará esta información en la sección **Configuración** >> Claves de acceso del blob.

1. Seleccione el contenedor **modeldata** y haga clic en **Editar**.

    [![Navegador de PBI](media/how-to-enable-data-collection/pbiNavigator.png)](./media/how-to-enable-data-collection/pbiNavigator.png#lightbox)

1. En el editor de consultas, haga clic en la columna "Nombre" y agregue la cuenta de almacenamiento 1. Especifique la ruta del modelo en el filtro. Nota: si desea buscar únicamente en los archivos de un determinado año o mes, expanda la ruta de acceso del filtro. Por ejemplo, si desea buscar en los datos de marzo: /modeldata/idsuscripción>/nombregruporecursos>/nombreáreatrabajo>/nombreservicioweb>/nombremodelo>/versiónmodelo>/designación+>/año>/3

1. Filtre los datos pertinentes en función del **nombre**. Si almacenó **predicciones** y **entradas**, habrá que crear una consulta para cada una.

1. Haga clic en la flecha doble situada al lado de la columna **Contenido** para combinar los archivos.

    [![Contenido de PBI](media/how-to-enable-data-collection/pbiContent.png)](./media/how-to-enable-data-collection/pbiContent.png#lightbox)

1. Haga clic en Aceptar y se realizará la carga previa de los datos.

    [![pbiCombine](media/how-to-enable-data-collection/pbiCombine.png)](./media/how-to-enable-data-collection/pbiCombine.png#lightbox)

1. Ahora puede hacer clic en **Close and Apply** (Cerrar y aplicar).

1.  Si agregó entradas y predicciones, las tablas se correlacionarán automáticamente en función del valor de **RequestId**.

1. Comience a crear los informes personalizados con los datos del modelo.


### <a name="analyzing-model-data-using-databricks"></a>Análisis de los datos del modelo con Databricks

1. Cree un [área de trabajo de Databricks](https://docs.microsoft.com/azure/azure-databricks/quickstart-create-databricks-workspace-portal).

1. Acceda al área de trabajo de Databricks.

1. En esa área, seleccione **Cargar datos**.

    [![Carga de DB](media/how-to-enable-data-collection/dbupload.png)](./media/how-to-enable-data-collection/dbupload.png#lightbox)

1. Cree una tabla y seleccione **Otros orígenes de datos** -> Azure Blob Storage -> Create Table in Notebook (Crear tabla en Notebook).

    [![Tabla de DB](media/how-to-enable-data-collection/dbtable.PNG)](./media/how-to-enable-data-collection/dbtable.PNG#lightbox)

1. Actualice la ubicación de los datos. Este es un ejemplo:

    ```
    file_location = "wasbs://mycontainer@storageaccountname.blob.core.windows.net/modeldata/1a2b3c4d-5e6f-7g8h-9i10-j11k12l13m14/myresourcegrp/myWorkspace/aks-w-collv9/best_model/10/inputs/2018/*/*/data.csv" 
    file_type = "csv"
    ```
 
    [![DBsetup](media/how-to-enable-data-collection/dbsetup.png)](./media/how-to-enable-data-collection/dbsetup.png#lightbox)

1. Siga los pasos de la plantilla para ver y analizar los datos.

## <a name="example-notebook"></a>Cuaderno de ejemplo

El cuaderno [how-to-use-azureml/deployment/enable-data-collection-for-models-in-aks/enable-data-collection-for-models-in-aks.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/deployment/enable-data-collection-for-models-in-aks/enable-data-collection-for-models-in-aks.ipynb) muestra los conceptos de este artículo.  

[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]
