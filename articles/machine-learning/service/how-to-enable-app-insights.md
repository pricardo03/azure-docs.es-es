---
title: Habilitación de Application Insights para el servicio Azure Machine Learning en producción
description: Aprenda a configurar Application Insights para el servicio Azure Machine Learning implementado en Azure Kubernetes Service
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: marthalc
author: marthalc
ms.date: 10/01/2018
ms.openlocfilehash: 45871ab515c7ffd9520b1d77d3fd1e77abcc29ef
ms.sourcegitcommit: 4eddd89f8f2406f9605d1a46796caf188c458f64
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/11/2018
ms.locfileid: "49114574"
---
# <a name="monitor-your-azure-machine-learning-models-in-production-with-application-insights"></a>Supervisión de los modelos de Azure Machine Learning en producción con Application Insights

En este artículo, puede obtener información sobre cómo configurar **Application Insights** para el servicio **Azure Machine Learning**. Una vez habilitado, Application Insights le ofrece la oportunidad de supervisar:
* Tasas de solicitudes, tiempos de respuesta y tasas de error
* Tasas de dependencias, tiempos de respuesta y tasas de error
* Excepciones

Más información sobre Application Insights [aquí](../../application-insights/app-insights-overview.md). 

## <a name="prerequisites"></a>Requisitos previos
* Una suscripción de Azure. Si no tiene una, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.
* Un área de trabajo de Azure Machine Learning, un directorio local que contenga los scripts y el SDK de Azure Machine Learning para Python instalado. Descubra cómo obtener estos requisitos previos con el documento [How to configure a development environment](how-to-configure-environment.md) (Cómo configurar un entorno de desarrollo).
* Un modelo de aprendizaje automático entrenado para implementarse en Azure Kubernetes Service (AKS). Si no tiene uno, consulte el tutorial [Train an image classification model](tutorial-train-models-with-aml.md) (Entrenar un modelo de clasificación de imágenes).
* Un [clúster de AKS](how-to-deploy-to-aks.md).

## <a name="enable--disable-in-the-portal"></a>Habilitación y deshabilitación en el portal

Puede habilitar y deshabilitar Application Insights en Azure Portal.

### <a name="enable"></a>Habilitar

1. Abra el área de trabajo en [Azure Portal](https://portal.azure.com).

1. Vaya a las implementaciones y seleccione el servicio en el que desea habilitar Application Insights.

   [![Implementaciones](media/how-to-enable-app-insights/Deployments.PNG)](./media/how-to-enable-app-insights/Deployments.PNG#lightbox)

3. Haga clic en **Editar** y vaya a **Configuración avanzada**.

   [![Editar](media/how-to-enable-app-insights/Edit.PNG)](./media/how-to-enable-app-insights/Edit.PNG#lightbox)

4. En **Configuración avanzada**, seleccione **Habilitar diagnósticos de Application Insights**.

   [![Editar](media/how-to-enable-app-insights/AdvancedSettings.png)](./media/how-to-enable-app-insights/AdvancedSettings.png#lightbox)

1. Seleccione **Actualizar** en la parte inferior de la pantalla para aplicar los cambios. 

### <a name="disable"></a>Disable
Para deshabilitar Application Insights en Azure Portal, realice los pasos siguientes:

1. Inicie sesión Azure Portal en https://portal.azure.com.
1. Vaya a su área de trabajo.
1. Elija **Implementaciones**, a continuación elija **Seleccionar servicio** y, a continuación, **Editar**.

   [![Editar](media/how-to-enable-app-insights/Edit.PNG)](./media/how-to-enable-app-insights/Edit.PNG#lightbox)

1. En **Configuración avanzada**, anule la selección de la opción **Habilitar diagnósticos de Application Insights**. 

   [![Desactivar](media/how-to-enable-app-insights/uncheck.png)](./media/how-to-enable-app-insights/uncheck.png#lightbox)

1. Seleccione **Actualizar** en la parte inferior de la pantalla para aplicar los cambios. 

## <a name="enable--disable-from-the-sdk"></a>Habilitación y deshabilitación desde el SDK

### <a name="update-a-deployed-service"></a>Actualización de un servicio implementado
1. Identifique el servicio en el área de trabajo (ws = nombre del área de trabajo)

    ```python
    aks_service= Webservice(ws, "my-service-name")
    ```
2. Actualice el servicio y habilite Application Insights. 

    ```python
    aks_service.update(enable_app_insights=True)
    ```

### <a name="log-custom-traces-in-your-service"></a>Registro de seguimientos personalizados del servicio
Si desea registrar seguimientos personalizados, deberá seguir el [proceso de implementación estándar de AKS](how-to-deploy-to-aks.md) y conseguirá:

1. Actualizar el archivo de puntuación mediante la adición de instrucciones de impresión.
    
    ```python
    print ("model initialized" + time.strftime("%H:%M:%S"))
    ```

2. Actualizar la configuración de AKS.
    
    ```python
    aks_config = AksWebservice.deploy_configuration(enable_app_insights=True)
    ```

3. [Crear una imagen e implementarla](how-to-deploy-to-aks.md).  

### <a name="disable-tracking-in-python"></a>Deshabilitación del seguimiento en Python

Para deshabilitar Application Insights, use el siguiente código:

```python 
## replace <service_name> with the name of the web service
<service_name>.update(enable_app_insights=False)
```
    

## <a name="evaluate-data"></a>Evaluación de los datos
Los datos del servicio se almacenan en la cuenta de Application Insights en el mismo grupo de recursos del área de trabajo del servicio Azure Machine Learning.
Para verlo:
1. Vaya al grupo de recursos en [Azure Portal](https://portal.azure.com) y haga clic en el recurso de Application Insights. 
2. La pestaña **Información general** muestra el conjunto básico de métricas del servicio.

   [![Información general](media/how-to-enable-app-insights/overview.png)](./media/how-to-enable-app-insights/overview.png#lightbox)

3. Para buscar en los seguimientos personalizados, haga clic en **Analytics**.
4. En la sección de esquema, haga clic en **Seguimientos** y, a continuación, en **Ejecutar** la consulta. Los datos deben aparecer en un formato de tabla a continuación y se puede ver la asignación a las llamadas personalizadas en el archivo de puntuación. 

   [![Seguimientos personalizados](media/how-to-enable-app-insights/logs.png)](./media/how-to-enable-app-insights/logs.png#lightbox)

Haga clic [aquí](../../application-insights/app-insights-overview.md) para más información sobre el uso de Application Insights.
    

## <a name="example-notebook"></a>Cuaderno de ejemplo

El cuaderno [00.Getting Started/13.enable-app-insights-in-production-service.ipynb](https://github.com/Azure/MachineLearningNotebooks/tree/master/01.getting-started/13.enable-app-insights) muestra los conceptos de este artículo.  Obtenga este cuaderno:
 
[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Pasos siguientes
También puede recopilar datos de los modelos en producción. Consulte el artículo [Recopilación de datos de modelos en producción](how-to-enable-data-collection.md) 
