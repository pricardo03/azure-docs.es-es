---
title: Supervisión de modelos con Application Insights
titleSuffix: Azure Machine Learning service
description: Use Application Insights para supervisar los servicios web implementados mediante Azure Machine Learning Service
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: marthalc
author: marthalc
ms.date: 10/01/2018
ms.custom: seodec18
ms.openlocfilehash: 176d0f00774106bbf9ae8dade0fe3fd4062530bb
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2018
ms.locfileid: "53277189"
---
# <a name="monitor-your-azure-machine-learning-models-with-application-insights"></a>Supervisión de los modelos de Azure Machine Learning con Application Insights

En este artículo, puede obtener información sobre cómo configurar Azure Application Insights para el servicio Azure Machine Learning. Application Insights le ofrece la oportunidad de supervisar:
* Tasas de solicitudes, tiempos de respuesta y tasas de error.
* Tasas de dependencias, tiempos de respuesta y tasas de error.
* Excepciones.

[Más información sobre Application Insights](../../application-insights/app-insights-overview.md). 


## <a name="prerequisites"></a>Requisitos previos

* Si no tiene una suscripción a Azure, cree una cuenta gratuita antes de empezar. Pruebe hoy mismo la [versión gratuita o de pago de Azure Machine Learning Service](http://aka.ms/AMLFree).

* Un área de trabajo de Azure Machine Learning, un directorio local que contenga los scripts y el SDK de Azure Machine Learning para Python instalado. Para obtener información acerca de cómo obtener estos requisitos previos, consulte [Procedimiento de configuración de un entorno de desarrollo](how-to-configure-environment.md).
* Un modelo de Machine Learning entrenado para implementarse en Azure Kubernetes Service (AKS) o Azure Container Instance (ACI). Si no tiene uno, consulte el tutorial [Entrenamiento de un modelo de clasificación de imágenes](tutorial-train-models-with-aml.md).


## <a name="enable-and-disable-from-the-sdk"></a>Habilitación y deshabilitación desde el SDK

### <a name="update-a-deployed-service"></a>Actualización de un servicio implementado
1. Identifique el servicio en el área de trabajo. El valor de `ws` es el nombre del área de trabajo.

    ```python
    from azureml.core.webservice import Webservice
    aks_service= Webservice(ws, "my-service-name")
    ```
2. Actualice el servicio y habilite Application Insights. 

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

3. Cree una imagen e impleméntela en [AKS](how-to-deploy-to-aks.md) o [ACI](how-to-deploy-to-aci.md).  

### <a name="disable-tracking-in-python"></a>Deshabilitación del seguimiento en Python

Para deshabilitar Application Insights, use el siguiente código:

```python 
## replace <service_name> with the name of the web service
<service_name>.update(enable_app_insights=False)
```
    
## <a name="enable-and-disable-in-the-portal"></a>Habilitación y deshabilitación en el portal

Puede habilitar y deshabilitar Application Insights en Azure Portal.

1. Abra el área de trabajo en [Azure Portal](https://portal.azure.com).

1. En la pestaña **Implementaciones**, seleccione el servicio en el que desea habilitar Application Insights.

   [![Lista de servicios de la pestaña Implementaciones](media/how-to-enable-app-insights/Deployments.PNG)](./media/how-to-enable-app-insights/Deployments.PNG#lightbox)

3. Seleccione **Editar**.

   [![Botón Editar](media/how-to-enable-app-insights/Edit.PNG)](./media/how-to-enable-app-insights/Edit.PNG#lightbox)

4. En **Configuración avanzada**, seleccione la casilla de verificación **Habilitar diagnósticos de Application Insights**.

   [![Casilla seleccionada para habilitar diagnósticos](media/how-to-enable-app-insights/AdvancedSettings.png)](./media/how-to-enable-app-insights/AdvancedSettings.png#lightbox)

1. Seleccione **Actualizar** en la parte inferior de la pantalla para aplicar los cambios. 

### <a name="disable"></a>Disable
1. Abra el área de trabajo en [Azure Portal](https://portal.azure.com).
1. Seleccione **Implementaciones**, seleccione el servicio y seleccione **Editar**.

   [![Uso del botón Editar](media/how-to-enable-app-insights/Edit.PNG)](./media/how-to-enable-app-insights/Edit.PNG#lightbox)

1. En **Configuración avanzada**, anule la selección de la opción **Habilitar diagnósticos de Application Insights**. 

   [![Casilla no seleccionada para habilitar diagnósticos](media/how-to-enable-app-insights/uncheck.png)](./media/how-to-enable-app-insights/uncheck.png#lightbox)

1. Seleccione **Actualizar** en la parte inferior de la pantalla para aplicar los cambios. 
 

## <a name="evaluate-data"></a>Evaluación de los datos
Los datos del servicio se almacenan en la cuenta de Application Insights en el mismo grupo de recursos del servicio Azure Machine Learning.
Para verlo:
1. Vaya al área de trabajo de Machine Learning Service en [Azure Portal](https://portal.azure.com) y haga clic en el vínculo de Application Insights.

    [![AppInsightsLoc](media/how-to-enable-app-insights/AppInsightsLoc.png)](./media/how-to-enable-app-insights/AppInsightsLoc.png#lightbox)

1. Seleccione la pestaña **Información general** para ver un conjunto básico de métricas del servicio.

   [![Información general](media/how-to-enable-app-insights/overview.png)](./media/how-to-enable-app-insights/overview.png#lightbox)

3. Para buscar en los seguimientos personalizados, seleccione **Analytics**.
4. En la sección de esquema, seleccione **Seguimientos**. A continuación, seleccione **Ejecutar** para ejecutar la consulta. Los datos deben aparecer en un formato de tabla y mostrar la asignación a las llamadas personalizadas en el archivo de puntuación. 

   [![Seguimientos personalizados](media/how-to-enable-app-insights/logs.png)](./media/how-to-enable-app-insights/logs.png#lightbox)

Para más información sobre el uso de Application Insights, consulte [¿Qué es Application Insights?](../../application-insights/app-insights-overview.md).
    

## <a name="example-notebook"></a>Cuaderno de ejemplo

El cuaderno [how-to-use-azureml/deployment/enable-app-insights-in-production-service/enable-app-insights-in-production-service.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/deployment/enable-app-insights-in-production-service/enable-app-insights-in-production-service.ipynb) muestra los conceptos de este artículo. 
 
[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Pasos siguientes
También puede recopilar datos de los modelos en producción. Consulte el artículo [Recopilación de datos de modelos en producción](how-to-enable-data-collection.md). 


## <a name="other-references"></a>Otras referencias
* [Azure Monitor para contenedores](https://docs.microsoft.com/azure/monitoring/monitoring-container-insights-overview?toc=%2fazure%2fmonitoring%2ftoc.json)
