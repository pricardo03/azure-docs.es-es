---
title: 'MLOps: Administrar, implementar y supervisar los modelos de aprendizaje automático'
titleSuffix: Azure Machine Learning service
description: 'Aprenda a usar el servicio Azure Machine Learning para MLOps: implementar, administrar y supervisar sus propios modelos para mejorarlos continuamente. Puede implementar los modelos que haya entrenado con Azure Machine Learning Services, en la máquina local o que procedan de otros orígenes.'
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
author: chris-lauren
ms.author: clauren
ms.date: 05/02/2019
ms.custom: seodec18
ms.openlocfilehash: 0eaf48f57c3011222b71a63d703e1ccec7aca001
ms.sourcegitcommit: 18a0d58358ec860c87961a45d10403079113164d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/05/2019
ms.locfileid: "66692825"
---
# <a name="mlops-manage-deploy-and-monitor-models-with-azure-machine-learning-service"></a>MLOps: Administración, implementación y supervisión de modelos Azure Machine Learning Services

En este artículo, obtenga información sobre cómo usar el servicio de Azure Machine Learning para administrar el ciclo de vida de los modelos. Azure Machine Learning usa un enfoque de aprendizaje de máquinas (MLOps), lo que mejora la calidad y la coherencia de sus soluciones de aprendizaje automático. Servicio de Azure Machine Learning proporciona las siguientes funcionalidades MLOps:

* Integración con las canalizaciones de Azure. Definir flujos de trabajo integración e implementación continua para los modelos.
* Un registro de modelo que mantiene varias versiones de los modelos entrenados.
* Validación del modelo. Validar los modelos entrenados y seleccione la configuración óptima para su implementación en producción automáticamente.
* Implementar los modelos como un servicio web en la nube, localmente o en dispositivos IoT Edge.
* Supervisar el rendimiento del modelo implementada, por lo que puede impulsar mejoras en la próxima versión del modelo.

Para saber más sobre los conceptos de MLOps y cómo se aplican al servicio Azure Machine Learning, vea el siguiente vídeo.

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2X1GX]

## <a name="integration-with-azure-pipelines"></a>Integración con las canalizaciones de Azure

Puede utilizar las canalizaciones de Azure para crear un proceso de integración continua que entrena un modelo. En un escenario típico, cuando un científico de datos comprueba un cambio en el repositorio de Git para un proyecto, la canalización de Azure se iniciará una ejecución de entrenamiento. A continuación, se pueden inspeccionar los resultados de la ejecución para ver las características de rendimiento del modelo entrenado. También puede crear una canalización que se implementa el modelo como un servicio web.

El [extensión de Azure Machine Learning](https://marketplace.visualstudio.com/items?itemName=ms-air-aiagility.vss-services-azureml) hace que sea más fácil trabajar con las canalizaciones de Azure. Proporciona las siguientes mejoras a las canalizaciones de Azure:

* Habilita la selección de área de trabajo al definir una conexión de servicio.
* Permite liberar las canalizaciones se desencadenen en los modelos entrenados creados en una canalización de aprendizaje.

Para obtener más información sobre el uso de canalizaciones de Azure con Azure Machine Learning, consulte el [integración continua e implementación de modelos de aprendizaje automático con canalizaciones de Azure](/azure/devops/pipelines/targets/azure-machine-learning) artículo y el [MLOps de servicio de Azure Machine Learning](https://aka.ms/mlops) repositorio.

## <a name="convert-and-optimize-models"></a>Convertir y optimizar los modelos

Convertir el modelo de [intercambio de red neuronal abierta](https://onnx.ai) (ONNX) puede mejorar el rendimiento. En promedio, convertir a ONNX puede producir un aumento del rendimiento en 2.

Para obtener más información sobre ONNX con el servicio de Azure Machine Learning, consulte el [Create y acelerar los modelos de aprendizaje automático](concept-onnx.md) artículo.

## <a name="register-models"></a>Registrar modelos

El registro del modelo permite almacenar y versionar los modelos en la nube de Azure, en su área de trabajo. El registro de modelo facilita organizar y mantener un seguimiento de los modelos entrenados.

> [!TIP]
> También puede registrar modelos entrenados fuera del servicio de Azure Machine Learning.
 
Los modelos registrados se identifican por el nombre y la versión. Cada vez que registra un modelo con el mismo nombre que uno existente, el registro incrementa la versión. También puede proporciona etiquetas de metadatos adicionales durante el registro para usarlas al buscar modelos. El servicio de Azure Machine Learning es compatible con cualquier modelo que se puede cargar mediante Python 3.5.2 o superior.

No se puede eliminar los modelos que se usan en una implementación activa.

Para más información, consulte la sección de registro de modelos de [Implementación de modelos](how-to-deploy-and-where.md#registermodel).

Para un ejemplo de registro de un modelo almacenado en formato pickle, consulte [Tutorial: Entrenamiento de un modelo de clasificación de imágenes](tutorial-deploy-models-with-aml.md).

## <a name="package-and-debug-models"></a>Modelos de paquetes y depuración

Antes de implementar un modelo en producción, lo se empaqueta en una imagen de Docker. En la mayoría de los casos, la creación de imágenes produce automáticamente en segundo plano durante la implementación. Para escenarios avanzados, puede especificar manualmente la imagen.

Si experimenta problemas con la implementación, puede implementar en su entorno de desarrollo local para solucionar problemas y depuración.

Para obtener más información, consulte [implementar modelos](how-to-deploy-and-where.md#registermodel) y [solucionar problemas en implementaciones](how-to-troubleshoot-deployment.md).

## <a name="validate-and-profile-models"></a>Validar y generar perfiles de modelos

Servicio Azure Machine Learning puede utilizar la generación de perfiles para determinar la configuración de CPU y memoria ideal para usarlo al implementar el modelo. Se produce la validación del modelo como parte de este proceso, con los datos que proporcione para el proceso de generación de perfiles.

## <a name="use-models"></a>Usar modelos

Modelos de aprendizaje automático entrenado se puede implementar como servicios web en la nube o localmente en su entorno de desarrollo. También puede implementar modelos a los dispositivos de Azure IoT Edge. Las implementaciones pueden utilizar la CPU, GPU o matrices de puertas programable de campo (FPGA) para inferencia. También puede usar modelos de Power BI.

Cuando se usa un modelo como un servicio web o un dispositivo de IoT Edge, proporcione los siguientes elementos:

* Los modelos que se utilizan para puntuar los datos enviados al servicio o dispositivo.
* Un script de entrada. Este script acepta las solicitudes, usa los modelos para puntuar los datos y devolver una respuesta.
* Un archivo de entorno de conda que describe las dependencias requeridas por el script de entrada y de modelos.
* Los recursos adicionales, como texto, datos, etc. requeridos por el script de entrada y de modelos.

Estos activos se empaqueta en una imagen de Docker e implementan como un servicio web o un módulo IoT Edge.

Si lo desea, puede usar los siguientes parámetros para optimizar aún más la implementación:

* Permitir GPU: Se usa para habilitar la compatibilidad con GPU en la imagen de Docker. La imagen se debe usar en los servicios de Microsoft Azure como Azure Container Instances, Azure Kubernetes Service, Azure Machine Learning Compute o máquinas virtuales de Azure.
* Pasos del archivo de docker adicional: Un archivo que contiene los pasos adicionales de Docker para ejecutarlo al crear la imagen de Docker.
* Imagen base: Una imagen personalizada para usarla como imagen base. Si no usa una imagen personalizada, el servicio de Azure Machine Learning proporciona la imagen base.

También proporcionan la configuración de la plataforma de implementación de destino. Por ejemplo, la máquina virtual tipo de familia, la memoria disponible y número de núcleos al implementar en Azure Kubernetes Service.

Cuando se crea la imagen, también se agregan los componentes requeridos por el servicio Azure Machine Learning. Por ejemplo, los recursos necesarios para ejecutar el servicio web e interactuar con IoT Edge.

> [!NOTE]
> No se puede modificar o cambiar el servidor web o los componentes de IoT Edge utilizados en la imagen de Docker. Servicio Azure Machine Learning usa una configuración de servidor web y componentes de IoT Edge que se han probado y compatible con Microsoft.

### <a name="web-service"></a>Servicio web

Puede usar los modelos en **servicios web** con los siguientes destinos de proceso:

* Azure Container Instances
* Azure Kubernetes Service
* Entorno de desarrollo local

Para implementar el modelo como un servicio web, debe proporcionar los siguientes elementos:

* El modelo o un conjunto de modelos.
* Dependencias necesarias para utilizar el modelo. Por ejemplo, una secuencia de comandos que acepta solicitudes e invoca el modelo, las dependencias de conda, etcetera.
* Configuración de implementación que se describe cómo y dónde para implementar el modelo.

Para obtener más información, consulte [implementar modelos](how-to-deploy-and-where.md).

### <a name="iot-edge-devices"></a>Dispositivos IoT Edge


Puede usar modelos con dispositivos IoT a través de **módulos de Azure IoT Edge**. Módulos de IoT Edge se implementan en un dispositivo de hardware, lo que permite la inferencia o modelo de puntuación en el dispositivo.

Para obtener más información, consulte [implementar modelos](how-to-deploy-and-where.md).

### <a name="analytics"></a>Análisis

Microsoft Power BI admite el uso de modelos de aprendizaje automático para el análisis de datos. Para obtener más información, consulte [integración de Azure Machine Learning en Power BI (versión preliminar)](https://docs.microsoft.com/power-bi/service-machine-learning-integration).

## <a name="monitor-and-collect-data"></a>Supervisar y recopilar datos

Supervisión permite comprender qué datos se envían a su modelo y las predicciones que devuelve.

Esta información le ayudará a comprender cómo se usa el modelo. Los datos recopilados de entrada también pueden ser útiles en versiones futuras de entrenamiento del modelo.

Para más información, consulte [cómo habilitar la recopilación de datos de un modelo](how-to-enable-data-collection.md).

## <a name="next-steps"></a>Pasos siguientes

Más información sobre [cómo y dónde puede implementar modelos](how-to-deploy-and-where.md) con el servicio Azure Machine Learning. Para obtener un ejemplo de implementación, consulte [Tutorial: Implementar un modelo de clasificación de imágenes en Azure Container Instances](tutorial-deploy-models-with-aml.md).

Aprenda a crear [integración continua e implementación de modelos de aprendizaje automático con canalizaciones de Azure](/azure/devops/pipelines/targets/azure-machine-learning). 

Obtenga información sobre cómo crear aplicaciones de cliente y servicios que [consumen un modelo implementado como servicio web](how-to-consume-web-service.md).
