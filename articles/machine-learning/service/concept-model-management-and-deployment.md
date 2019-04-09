---
title: Administración, registro, implementación y supervisión de modelos de ML
titleSuffix: Azure Machine Learning service
description: Aprenda a usar Azure Machine Learning Services para implementar, administrar y supervisar sus propios modelos para mejorarlos de forma continua. Puede implementar los modelos que haya entrenado con Azure Machine Learning Services, en la máquina local o que procedan de otros orígenes.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
author: chris-lauren
ms.author: clauren
ms.date: 1/23/2019
ms.custom: seodec18
ms.openlocfilehash: 2cd2d328d33744854bc525e5ecf1dfa3b6e4bcc8
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2019
ms.locfileid: "59275449"
---
# <a name="manage-deploy-and-monitor-models-with-azure-machine-learning-service"></a>Administración, implementación y supervisión de modelos Azure Machine Learning Services

En este artículo aprenderá a usar Azure Machine Learning Services para implementar, administrar y supervisar sus propios modelos para mejorarlos continuamente. Puede implementar los modelos que haya entrenado con Azure Machine Learning, en la máquina local o que procedan de otros orígenes. 

En el siguiente diagrama se ilustra el flujo de trabajo de implementación completo: [![Flujo de trabajo de implementación para Azure Machine Learning](media/concept-model-management-and-deployment/deployment-pipeline.png)](media/concept-model-management-and-deployment/deployment-pipeline.png#lightbox)

El flujo de trabajo de implementación incluye los siguientes pasos:
1. **Registrar el modelo** en un registro hospedado en el área de trabajo de Azure Machine Learning Services
1. **Registrar una imagen** que empareje un modelo con un script de puntuación y las dependencias en un contenedor portátil 
1. **Implementar** la imagen como servicio web en la nube o en dispositivos perimetrales
1. **Supervisar y recopilar datos**
1. **Actualizar** una implementación para usar una imagen nueva.

Cada paso puede realizarse de forma independiente o como parte de un comando de implementación único. Además, puede integrar la implementación en un **flujo de trabajo de CI/CD** tal como se muestra en este gráfico.

[!['Ciclo de Azure Machine Learning continua e integración continua (CI/CD) de la implementación '](media/concept-model-management-and-deployment/model-ci-cd.png)](media/concept-model-management-and-deployment/model-ci-cd.png#lightbox)

## <a name="step-1-register-model"></a>Paso 1: Registro del modelo

El registro del modelo permite almacenar y versionar los modelos en la nube de Azure, en su área de trabajo. El registro de modelo facilita organizar y mantener un seguimiento de los modelos entrenados.
 
Los modelos registrados se identifican por el nombre y la versión. Cada vez que registra un modelo con el mismo nombre que uno existente, el registro incrementa la versión. También puede proporciona etiquetas de metadatos adicionales durante el registro para usarlas al buscar modelos. Azure Machine Learning Service admite cualquier modelo que pueda cargarse mediante Python 3. 

No se pueden eliminar los modelos que una imagen esté utilizando.

Para más información, consulte la sección de registro de modelos de [Implementación de modelos](how-to-deploy-and-where.md#registermodel).

Para un ejemplo de registro de un modelo almacenado en formato pickle, consulte [Tutorial: Entrenamiento de un modelo de clasificación de imágenes](tutorial-deploy-models-with-aml.md).

Para información sobre el uso de modelos ONNX, consulte el documento [ONNX y Azure Machine Learning](how-to-build-deploy-onnx.md).

## <a name="step-2-register-image"></a>Paso 2: Registro de la imagen

Las imágenes permiten implementar el modelo de confianza, así como los componentes necesarios para usarlo. Una imagen contiene los siguientes elementos:

* El modelo
* El motor de puntuación
* El archivo o la aplicación de puntuación
* Las dependencias necesarias para puntuar el modelo

La imagen también puede incluir componentes del SDK para el registro y la supervisión. Los datos de los registros del SDK pueden usarse para ajustar o volver a entrenar el modelo, su entrada y salida incluidas.

Azure Machine Learning admite las plataformas más populares, pero en general vale cualquiera donde se pueda instalar pip.

Al crearse el área de trabajo, también se crearon otros recursos de Azure que esta usa.
Todos los objetos utilizados para crear la imagen predeterminada se almacenan en la cuenta de almacenamiento de Azure en el área de trabajo. Puede proporcionar etiquetas de metadatos adicionales al crear la imagen. El registro de imágenes también almacena las etiquetas de metadatos, que se pueden consultar para encontrar la imagen.

También puede utilizar imágenes personalizadas, que se pueden cargar en Azure Container Registry y utilizadas por el servicio de Azure Machine Learning.

Para más información, consulte la sección de configuración y registro de imágenes de [Implementación de modelos](how-to-deploy-and-where.md#configureimage).

## <a name="step-3-deploy-image"></a>Paso 3: Implementación de la imagen

Puede implementar imágenes registradas en la nube o en dispositivos perimetrales. En el proceso de implementación se crean todos los recursos necesarios para supervisar al modelo, equilibrar sus cargas y escalarlo automáticamente. El acceso a los servicios implementados se puede proteger con autenticación basada en certificados si se proporcionan los recursos de seguridad durante la implementación. También puede actualizar una implementación existente para usar una imagen más reciente.

Las implementaciones de servicio web también permiten búsquedas. Por ejemplo, puede buscar todas las implementaciones de una imagen o un modelo específicos.

[![Idestinos nferencing](media/concept-model-management-and-deployment/inferencing-targets.png)](media/concept-model-management-and-deployment/inferencing-targets.png#lightbox)

Puede implementar las imágenes en los siguientes destinos de implementación en la nube:

* Azure Container Instances
* Azure Kubernetes Service
* Máquinas con FPGA de Azure
* Dispositivos de Azure IoT Edge

Al implementarse el servicio, la solicitud de inferencia equilibra la carga automáticamente y el clúster se escala para dar respuesta a cualquier aumento de demanda. Los [datos de telemetría sobre el servicio](how-to-enable-app-insights.md) se pueden capturar en el servicio Azure Application Insights asociado al área de trabajo.

Para más información, consulte la sección de implementación de [Implementación de modelos](how-to-deploy-and-where.md#deploy).

## <a name="step-4-monitor-models-and-collect-data"></a>Paso 4: Supervisión de los modelos y recopilación de datos

Hay un SDK para la captura de datos y el registro del modelo disponible para supervisar la entrada, la salida y otros datos pertinentes del modelo. Los datos se almacenan como un blob en la cuenta de almacenamiento de Azure para el área de trabajo.

Para usar el SDK con el modelo, se importa el SDK en la aplicación o el script de puntuación. A continuación se puede usar el SDK para registrar datos como parámetros, resultados o detalles de la entrada.

Si decide habilitar la recopilación de datos del modelo cada vez que implementa la imagen, los detalles necesarios para capturar los datos, como las credenciales del almacén personal de blobs, se aprovisionan automáticamente.

> [!Important]
> Microsoft no ve los datos recopilados del modelo. Los datos se envían directamente a la cuenta de almacenamiento de Azure para el área de trabajo.

Para más información, consulte [cómo habilitar la recopilación de datos de un modelo](how-to-enable-data-collection.md).

## <a name="step-5-update-the-deployment"></a>Paso 5: Actualización de la implementación

Las actualizaciones del modelo no se registran automáticamente. De forma similar, registrar una nueva imagen no actualiza automáticamente las implementaciones que se crearon a partir de una versión anterior de la imagen. En su lugar, debe registrar manualmente el modelo, registrar la imagen y, luego, actualizar el modelo. Para más información, consulte la sección de actualización de [Implementación de modelos](how-to-deploy-and-where.md#update).

## <a name="next-steps"></a>Pasos siguientes

Más información sobre [cómo y dónde puede implementar modelos](how-to-deploy-and-where.md) con el servicio Azure Machine Learning. Para obtener un ejemplo de implementación, consulte [Tutorial: Implementar un modelo de clasificación de imágenes en Azure Container Instances](tutorial-deploy-models-with-aml.md).

Obtenga información sobre cómo crear aplicaciones de cliente y servicios que [consumen un modelo implementado como servicio web](how-to-consume-web-service.md).
