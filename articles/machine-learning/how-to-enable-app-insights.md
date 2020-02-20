---
title: Supervisión y recopilación de datos de los puntos de conexión del servicio web Machine Learning
titleSuffix: Azure Machine Learning
description: Supervisión de los servicios web implementados con Azure Machine Learning mediante Azure App Insights
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: peterlu
author: peterclu
ms.date: 11/12/2019
ms.openlocfilehash: 6498717f5d542be228483b9c323dbd8f3ca2ff00
ms.sourcegitcommit: 79cbd20a86cd6f516acc3912d973aef7bf8c66e4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/14/2020
ms.locfileid: "77251930"
---
# <a name="monitor-and-collect-data-from-ml-web-service-endpoints"></a>Supervisión y recopilación de datos de los puntos de conexión del servicio web ML
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

En este artículo, aprenderá a recopilar datos y a supervisar los modelos implementados en los puntos de conexión de servicio en Azure Kubernetes Service (AKS) o Azure Container Instances (ACI) habilitando Azure Application Insights. Además de recopilar los datos de entrada y la respuesta de un punto de conexión, puede supervisar:

* Tasas de solicitudes, tiempos de respuesta y tasas de error
* Tasas de dependencias, tiempos de respuesta y tasas de error
* Excepciones

[Más información sobre Azure Application Insights](../azure-monitor/app/app-insights-overview.md). 


## <a name="prerequisites"></a>Prerrequisitos

* Si no tiene una suscripción a Azure, cree una cuenta gratuita antes de empezar. Pruebe hoy mismo la [versión gratuita o de pago de Azure Machine Learning](https://aka.ms/AMLFree).

* Un área de trabajo de Azure Machine Learning, un directorio local que contenga los scripts y el SDK de Azure Machine Learning para Python instalado. Para obtener información sobre cómo obtener estos requisitos previos, consulte [Procedimiento de configuración de un entorno de desarrollo](how-to-configure-environment.md).
* Un modelo de Machine Learning entrenado para implementarse en Azure Kubernetes Service (AKS) o Azure Container Instance (ACI). Si no tiene uno, consulte el tutorial [Entrenamiento de un modelo de clasificación de imágenes](tutorial-train-models-with-aml.md).

## <a name="web-service-metadata-and-response-data"></a>Metadatos de servicio web y datos de respuesta

>[!Important]
> Azure Application Insights solo registra cargas de hasta 64 kb. Si se alcanza este límite, solo se registran las salidas más recientes del modelo. 

Los metadatos y la respuesta al servicio (que corresponden a los metadatos del servicio web y las predicciones del modelo) se registran en los seguimientos de Azure Application Insights, en el mensaje `"model_data_collection"`. Puede consultar Azure Application Insights directamente para acceder a estos datos o configurar una [exportación continua](https://docs.microsoft.com/azure/azure-monitor/app/export-telemetry) a una cuenta de almacenamiento para una retención más larga o un procesamiento adicional. Los datos del modelo se pueden utilizar entonces en Azure Machine Learning para configurar el etiquetado, un nuevo entrenamiento, la capacidad de explicación, el análisis de datos u otro uso. 

## <a name="use-the-azure-portal-to-configure"></a>Uso de Azure Portal para configurar

Puede habilitar y deshabilitar Azure Application Insights en Azure Portal. 

1. Abra el área de trabajo en [Azure Portal](https://portal.azure.com).

1. En la pestaña **Implementaciones**, seleccione el servicio en el que desea habilitar Azure Application Insights.

   [![Lista de servicios de la pestaña Implementaciones](./media/how-to-enable-app-insights/Deployments.PNG)](././media/how-to-enable-app-insights/Deployments.PNG#lightbox)

3. Seleccione **Editar**.

   [![Botón Editar](././media/how-to-enable-app-insights/Edit.PNG)](./././media/how-to-enable-app-insights/Edit.PNG#lightbox)

4. En **Configuración avanzada**, seleccione la casilla **Habilitar diagnósticos de Application Insights**.

   [![Casilla seleccionada para habilitar diagnósticos](./media/how-to-enable-app-insights/AdvancedSettings.png)](././media/how-to-enable-app-insights/AdvancedSettings.png#lightbox)

1. Seleccione **Actualizar** de la parte inferior de la pantalla para aplicar los cambios.

### <a name="disable"></a>Disable

1. Abra el área de trabajo en [Azure Portal](https://portal.azure.com).
1. Seleccione **Implementaciones**, elija el servicio y luego seleccione **Editar**.

   [![Uso del botón Editar](././media/how-to-enable-app-insights/Edit.PNG)](./././media/how-to-enable-app-insights/Edit.PNG#lightbox)

1. En **Configuración avanzada**, desactive la casilla **Habilitar diagnósticos de Application Insights**.

   [![Casilla no seleccionada para habilitar diagnósticos](./media/how-to-enable-app-insights/uncheck.png)](././media/how-to-enable-app-insights/uncheck.png#lightbox)

1. Seleccione **Actualizar** de la parte inferior de la pantalla para aplicar los cambios.
 
## <a name="use-python-sdk-to-configure"></a>Uso del SDK de Python para configurar 

### <a name="update-a-deployed-service"></a>Actualización de un servicio implementado

1. Identifique el servicio en el área de trabajo. El valor de `ws` es el nombre del área de trabajo.

    ```python
    from azureml.core.webservice import Webservice
    aks_service= Webservice(ws, "my-service-name")
    ```
2. Actualice el servicio y habilite Azure Application Insights.

    ```python
    aks_service.update(enable_app_insights=True)
    ```

### <a name="log-custom-traces-in-your-service"></a>Registro de seguimientos personalizados del servicio

Si desea registrar seguimientos personalizados, siga el proceso de implementación estándar de AKS o ACI en el documento sobre [cómo realizar la implementación y donde](how-to-deploy-and-where.md). Después, siga estos pasos:

1. Actualice el archivo de puntuación mediante la adición de instrucciones de impresión.
    
    ```python
    print ("model initialized" + time.strftime("%H:%M:%S"))
    ```

2. Actualice la configuración del servicio.
    
    ```python
    config = Webservice.deploy_configuration(enable_app_insights=True)
    ```

3. Cree una imagen e impleméntela en [AKS o ACI](how-to-deploy-and-where.md).

### <a name="disable-tracking-in-python"></a>Deshabilitación del seguimiento en Python

Para deshabilitar Azure Application Insights, use el siguiente código:

```python 
## replace <service_name> with the name of the web service
<service_name>.update(enable_app_insights=False)
```

## <a name="evaluate-data"></a>Evaluación de los datos
Los datos del servicio se almacenan en la cuenta de Azure Application Insights en el mismo grupo de recursos de Azure Machine Learning.
Para verlo:

1. Vaya al área de trabajo de Azure Machine Learning en [Azure Machine Learning Studio](https://ml.azure.com) y haga clic en el vínculo de Application Insights.

    [![AppInsightsLoc](./media/how-to-enable-app-insights/AppInsightsLoc.png)](././media/how-to-enable-app-insights/AppInsightsLoc.png#lightbox)

1. Seleccione la pestaña **Información general** para ver un conjunto básico de métricas del servicio.

   [![Información general](./media/how-to-enable-app-insights/overview.png)](././media/how-to-enable-app-insights/overview.png#lightbox)

1. Para examinar los metadatos y la respuesta de la solicitud de servicio web, seleccione la tabla **Solicitudes** de la sección**Registros (Analytics)** y seleccione **Ejecutar** para ver las solicitudes.

   [![Datos del modelo](./media/how-to-enable-app-insights/model-data-trace.png)](././media/how-to-enable-app-insights/model-data-trace.png#lightbox)


3. Para buscar en los seguimientos personalizados, seleccione **Analytics**.
4. En la sección de esquema, seleccione **Seguimientos**. A continuación, seleccione **Ejecutar** para ejecutar la consulta. Los datos deben aparecer en un formato de tabla y mostrar la asignación a las llamadas personalizadas en el archivo de puntuación.

   [![Seguimientos personalizados](./media/how-to-enable-app-insights/logs.png)](././media/how-to-enable-app-insights/logs.png#lightbox)

Para más información sobre el uso de Azure Application Insights, consulte [¿Qué es Application Insights?](../azure-monitor/app/app-insights-overview.md).

## <a name="export-data-for-further-processing-and-longer-retention"></a>Exportación de datos para su posterior procesamiento y una retención más duradera

>[!Important]
> Azure Application Insights solo admite exportaciones a Blob Storage. Otros límites de esta funcionalidad de exportación se enumeran en [Exportación de telemetría desde Application Insights](https://docs.microsoft.com/azure/azure-monitor/app/export-telemetry#continuous-export-advanced-storage-configuration).

Puede usar la [exportación continua](https://docs.microsoft.com/azure/azure-monitor/app/export-telemetry) de Azure Application Insights para enviar mensajes a una cuenta de almacenamiento admitida, donde se pueda establecer una retención más larga. Los mensajes de `"model_data_collection"` se almacenan en formato JSON y se pueden analizar fácilmente para extraer los datos del modelo. 

Se puede usar Azure Data Factory, canalizaciones de Azure Machine Learning u otras herramientas de procesamiento de datos para transformar los datos según sea necesario. Una vez transformados los datos, puede registrarlos en el área de trabajo de Azure Machine Learning como un conjunto de datos. Para ello, vea [Creación de conjuntos de datos y acceso a ellos (versión preliminar) en Azure Machine Learning](how-to-create-register-datasets.md).

   [![Exportación continua](./media/how-to-enable-app-insights/continuous-export-setup.png)](././media/how-to-enable-app-insights/continuous-export-setup.png)


## <a name="example-notebook"></a>Cuaderno de ejemplo

El cuaderno [enable-app-insights-in-production-service.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/deployment/enable-app-insights-in-production-service/enable-app-insights-in-production-service.ipynb) muestra los conceptos de este artículo. 
 
[!INCLUDE [aml-clone-in-azure-notebook](../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Pasos siguientes

* Consulte [Implementación de un modelo en un clúster de Azure Kubernetes Service](https://docs.microsoft.com/azure/machine-learning/how-to-deploy-azure-kubernetes-service) o [Implementación de un modelo en Azure Container Instances](https://docs.microsoft.com/azure/machine-learning/how-to-deploy-azure-container-instance) para implementar los modelos en los puntos de conexión del servicio web y permitir que Azure Application Insights aproveche la recopilación de datos y la supervisión de puntos de conexión.
* Consulte [MLOps: Administración, implementación y supervisión de modelos con Azure Machine Learning](https://docs.microsoft.com/azure/machine-learning/concept-model-management-and-deployment) para más información sobre cómo aprovechar los datos recopilados de los modelos en producción. Estos datos pueden ayudar a mejorar continuamente el proceso de aprendizaje automático.
