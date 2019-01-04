---
title: Administración, registro, implementación y supervisión de modelos de ML
titleSuffix: Azure Machine Learning service
description: Aprenda a usar Azure Machine Learning Services para implementar, administrar y supervisar sus propios modelos para mejorarlos de forma continua. Puede implementar los modelos que haya entrenado con Azure Machine Learning Services, en la máquina local o que procedan de otros orígenes.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: jmartens
author: chris-lauren
ms.author: clauren
ms.date: 09/24/2018
ms.custom: seodec18
ms.openlocfilehash: 25f149ad4df43a7e5b443d6abd72be91072cb47f
ms.sourcegitcommit: 1c1f258c6f32d6280677f899c4bb90b73eac3f2e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/11/2018
ms.locfileid: "53250217"
---
# <a name="manage-deploy-and-monitor-models-with-azure-machine-learning-service"></a>Administración, implementación y supervisión de modelos Azure Machine Learning Services

En este artículo aprenderá a usar Azure Machine Learning Services para implementar, administrar y supervisar sus propios modelos para mejorarlos continuamente. Puede implementar los modelos que haya entrenado con Azure Machine Learning, en la máquina local o que procedan de otros orígenes. 

En el siguiente diagrama se ilustra el flujo de trabajo de implementación completo: [ ![Flujo de trabajo de implementación de Azure Machine Learning](media/concept-model-management-and-deployment/deployment-pipeline.png) ](media/concept-model-management-and-deployment/deployment-pipeline.png#lightbox)

El flujo de trabajo de implementación incluye los siguientes pasos:
1. **Registrar el modelo** en un registro hospedado en el área de trabajo de Azure Machine Learning Services
1. **Registrar una imagen** que empareje un modelo con un script de puntuación y las dependencias en un contenedor portátil 
1. **Implementar** la imagen como servicio web en la nube o en dispositivos perimetrales
1. **Supervisar y recopilar datos**

Cada paso puede realizarse de forma independiente o como parte de un comando de implementación único. Además, puede integrar la implementación en un **flujo de trabajo de CI/CD** tal como se muestra en este gráfico.

[ !["Integración continua/implementación continua (CI/CD) de Azure Machine Learning"](media/concept-model-management-and-deployment/model-ci-cd.png) ](media/concept-model-management-and-deployment/model-ci-cd.png#lightbox)


## <a name="step-1-register-model"></a>Paso 1: Registro del modelo

El registro de modelo realiza un seguimiento de todos los modelos del área de trabajo de Azure Machine Learning Services.
Los modelos se identifican por el nombre y la versión. Cada vez que registra un modelo con el mismo nombre que uno existente, el registro incrementa la versión. También puede proporciona etiquetas de metadatos adicionales durante el registro para usarlas al buscar modelos.

No se pueden eliminar los modelos que una imagen esté utilizando.

## <a name="step-2-register-image"></a>Paso 2: Registro de la imagen

Las imágenes permiten implementar el modelo de confianza, así como los componentes necesarios para usarlo. Una imagen contiene los siguientes elementos:

* El modelo
* El motor de puntuación
* El archivo o la aplicación de puntuación
* Las dependencias necesarias para puntuar el modelo

La imagen también puede incluir componentes del SDK para el registro y la supervisión. Los datos de los registros del SDK pueden usarse para ajustar o volver a entrenar el modelo, su entrada y salida incluidas.

Azure Machine Learning admite las plataformas más populares, pero en general vale cualquiera donde se pueda instalar pip.

Al crearse el área de trabajo, también se crearon otros recursos de Azure que esta usa.
Todos los objetos utilizados para crear la imagen se almacenan en la cuenta de almacenamiento de Azure del área de trabajo. La imagen se crea y se almacena en Azure Container Registry. Puede proporcionar etiquetas de metadatos adicionales al crear la imagen, que también almacena el registro de imagen y se pueden consultar para buscar la imagen.

## <a name="step-3-deploy-image"></a>Paso 3: Implementación de la imagen

Puede implementar imágenes registradas en la nube o en dispositivos perimetrales. En el proceso de implementación se crean todos los recursos necesarios para supervisar, equilibrar la carga y realizar el automático del modelo. El acceso a los servicios implementados se puede proteger con autenticación basada en certificados si se proporcionan los recursos de seguridad durante la implementación. También puede actualizar una implementación existente para usar una imagen más reciente.

Las implementaciones de servicio web también permiten búsquedas. Por ejemplo, puede buscar todas las implementaciones de una imagen o un modelo específicos.

[ ![Destinos de inferencia](media/concept-model-management-and-deployment/inferencing-targets.png) ](media/concept-model-management-and-deployment/inferencing-targets.png#lightbox)

Puede implementar sus imágenes en los siguientes [destinos de implementación](how-to-deploy-and-where.md) en la nube:

* Azure Container Instances
* Azure Kubernetes Service
* Máquinas con FPGA de Azure
* Dispositivos de Azure IoT Edge

Al implementarse el servicio, la solicitud de inferencia equilibra la carga automáticamente y el clúster se escala para dar respuesta a cualquier aumento de demanda. Los [datos de telemetría sobre el servicio](how-to-enable-app-insights.md) se pueden capturar en el servicio Azure Application Insights asociado al área de trabajo.

## <a name="step-4-monitor-models-and-collect-data"></a>Paso 4: Supervisión de los modelos y recopilación de datos

Hay un SDK para la captura de datos y el registro del modelo disponible para supervisar la entrada, la salida y otros datos pertinentes del modelo. Los datos se almacenan como un blob en la cuenta de almacenamiento de Azure para el área de trabajo.

Para usar el SDK con el modelo, se importa el SDK en la aplicación o el script de puntuación. A continuación se puede usar el SDK para registrar datos como parámetros, resultados o detalles de la entrada.

Si decide [habilitar la recopilación de datos del modelo](how-to-enable-data-collection.md) cada vez que implementa la imagen, los detalles necesarios para capturar los datos, como las credenciales del almacén personal de blobs, se aprovisionan automáticamente.

> [!Important]
> Microsoft no ve los datos recopilados del modelo. Los datos se envían directamente a la cuenta de almacenamiento de Azure para el área de trabajo.

## <a name="next-steps"></a>Pasos siguientes

Más información sobre [cómo y dónde puede implementar modelos](how-to-deploy-and-where.md) con el servicio Azure Machine Learning.
