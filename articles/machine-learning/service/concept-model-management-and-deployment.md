---
title: 'MLOps: administración, implementación y supervisión de modelos de ML'
titleSuffix: Azure Machine Learning service
description: 'Aprenda a usar Azure Machine Learning Service para MLOps: implemente, administre y supervise sus propios modelos para mejorarlos de forma continua. Puede implementar los modelos que haya entrenado con Azure Machine Learning Services, en la máquina local o que procedan de otros orígenes.'
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
author: jpe316
ms.author: jordane
ms.date: 06/24/2019
ms.custom: seodec18
ms.openlocfilehash: 11a4a17d7816d2302b6549cffb9517e10ad1258d
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/28/2019
ms.locfileid: "67442345"
---
# <a name="mlops-manage-deploy-and-monitor-models-with-azure-machine-learning-service"></a>MLOps: Administración, implementación y supervisión de modelos Azure Machine Learning Services

En este artículo, obtendrá información sobre cómo usar Azure Machine Learning Service para administrar el ciclo de vida de los modelos. Azure Machine Learning usa un enfoque de operaciones de Machine Learning (MLOps), lo que mejora la calidad y la coherencia de sus soluciones de aprendizaje automático. 

Azure Machine Learning Service ofrece las siguientes funcionalidades de MLOps:

- **Implementar proyectos de aprendizaje automático desde cualquier lugar**.
- **Supervisar las aplicaciones de aprendizaje automático para los problemas relacionados con operaciones y Machine Learning**: compare las entradas del modelo durante el entrenamiento y la inferencia, explore las métricas de un modelo específico e incluya supervisión y alertas en su infraestructura de aprendizaje automático.
- **Capturar los datos necesarios para establecer un registro de auditoría completo del ciclo de vida de ML**, que incluye quién publica los modelos, el motivo por el que se realizan los cambios y el momento en que los modelos se implementan o usan en producción.
- **Automatizar el ciclo de vida de ML completo con Azure Machine Learning y Azure DevOps** para actualizar con frecuencia los modelos, probar los modelos nuevos e implementar continuamente nuevos modelos de aprendizaje automático junto con sus otras aplicaciones y servicios.

Para saber más sobre los conceptos de MLOps y cómo se aplican a Azure Machine Learning Service, vea el siguiente vídeo.

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2X1GX]

## <a name="deploy-ml-projects-from-anywhere"></a>Implementación de proyectos de aprendizaje automático desde cualquier lugar

### <a name="turn-your-training-process-into-a-reproducible-pipeline"></a>Conversión de su proceso de entrenamiento en una canalización reproducible
Use canalizaciones de aprendizaje automático de Azure Machine Learning para unir todos los pasos implicados en el proceso de entrenamiento del modelo, desde la preparación de datos hasta la extracción de características, el ajuste de hiperparámetros y la evaluación de modelos.

Para obtener más información, consulte el artículo [canalizaciones de aprendizaje automático](concept-ml-pipelines.md).

### <a name="register-and-track-ml-models"></a>Registro y supervisión de modelos de aprendizaje automático

El registro del modelo permite almacenar y versionar los modelos en la nube de Azure, en su área de trabajo. El registro de modelo facilita organizar y mantener un seguimiento de los modelos entrenados.

> [!TIP]
> Un modelo registrado es un contenedor lógico para uno o varios archivos que componen el modelo. Por ejemplo, si tiene un modelo que se almacena en varios archivos, puede registrarlos como un único modelo en el área de trabajo de Azure Machine Learning. Después del registro, puede descargar o implementar el modelo registrado y recibir todos los archivos que se registraron.
 
Los modelos registrados se identifican por el nombre y la versión. Cada vez que registra un modelo con el mismo nombre que uno existente, el registro incrementa la versión. También puede proporciona etiquetas de metadatos adicionales durante el registro para usarlas al buscar modelos. Azure Machine Learning Service admite cualquier modelo que pueda cargarse mediante Python 3.5.2 o posterior.

> [!TIP]
> También puede registrar modelos entrenados fuera de Azure Machine Learning Service.

No se puede eliminar un modelo registrado que se esté usando en una implementación activa.
Para más información, consulte la sección de registro de modelos de [Implementación de modelos](how-to-deploy-and-where.md#registermodel).

### <a name="package-and-debug-models"></a>Empaquetado y depuración de modelos

Antes de implementar un modelo en producción, se empaqueta en una imagen de Docker. En la mayoría de los casos, la creación de imágenes se produce automáticamente en segundo plano durante la implementación. Para los escenarios avanzados, puede especificar manualmente la imagen.

Si experimenta problemas con la implementación, puede implementarla en su entorno de desarrollo local para solucionar problemas y depurarla.

Para obtener más información, consulte [implementación de modelos](how-to-deploy-and-where.md#registermodel) y [solución problemas en implementaciones](how-to-troubleshoot-deployment.md).

### <a name="validate-and-profile-models"></a>Validación y generación de perfiles de modelos

Azure Machine Learning Service puede utilizar la generación de perfiles para determinar la configuración de CPU y memoria ideal que se usará al implementar el modelo. La validación del modelo se produce como parte de este proceso con ayuda de los datos que proporcione para el proceso de generación de perfiles.

### <a name="convert-and-optimize-models"></a>Conversión y optimización de modelos

Si convierte el modelo a [Open Neural Network Exchange](https://onnx.ai) (ONNX), puede mejorar el rendimiento. En promedio, la conversión a ONNX puede duplicar el rendimiento.

Para obtener más información sobre ONNX con Azure Machine Learning Service, consulte el artículo sobre [creación y aceleración de los modelos de aprendizaje automático](concept-onnx.md).

### <a name="use-models"></a>Uso de modelos

Los modelos de Machine Learning entrenados se pueden implementar como servicios web en la nube o localmente en su entorno de desarrollo. También puede implementar en dispositivos Azure IoT Edge. Las implementaciones pueden utilizar matrices de puertas programable (FPGA) por CPU, GPU o campos para las inferencias. También puede usar modelos de Power BI.

Cuando use un modelo como servicio web o dispositivo de IoT Edge, proporcione los siguientes elementos:

* Los modelos que se usan para puntuar los datos enviados al servicio o dispositivo.
* Un script de entrada. Este script acepta las solicitudes, usa los modelos para puntuar los datos y devuelve una respuesta.
* Un archivo de entorno de conda que describa las dependencias requeridas por el script de entrada y los modelos.
* Los recursos adicionales, como texto, datos, etc. requeridos por el script de entrada y los modelos.

Estos activos se empaquetan en una imagen de Docker y se implementan como un servicio web o un módulo IoT Edge.

Si quiere, puede usar los siguientes parámetros para optimizar aún más la implementación:

* Habilitar GPU: se usa para habilitar la compatibilidad con GPU en la imagen de Docker. La imagen se debe usar en servicios de Microsoft Azure como Azure Container Instances, Azure Kubernetes Service, Proceso de Machine Learning de Azure o Azure Virtual Machines.
* Pasos adicionales para el archivo de Docker: Un archivo que contiene los pasos adicionales de Docker que debe realizar al crear la imagen de Docker.
* Imagen base: una imagen personalizada para usarse como imagen base. Si no usa una imagen personalizada, Azure Machine Learning Service le proporciona la imagen base.

También debe proporcionar la configuración de la plataforma de implementación de destino. Por ejemplo, el tipo de familia de la máquina virtual, la memoria disponible y número de núcleos cuando se implemente en Azure Kubernetes Service.

Cuando se crea la imagen, también se agregan los componentes requeridos por Azure Machine Learning Service. Por ejemplo, los recursos necesarios para ejecutar el servicio web e interactuar con IoT Edge.

> [!NOTE]
> No se puede modificar o cambiar el servidor web ni los componentes de IoT Edge utilizados en la imagen de Docker. Azure Machine Learning Service usa una configuración de servidor web y componentes de IoT Edge compatibles y autorizados por Microsoft.

#### <a name="web-service"></a>Servicio web

Puede usar los modelos en **servicios web** con los siguientes destinos de proceso:

* Azure Container Instances
* Azure Kubernetes Service
* Entorno de desarrollo locales

Para implementar el modelo como un servicio web, debe proporcionar los siguientes elementos:

* El modelo o un conjunto de modelos.
* Las dependencias necesarias para usar el modelo. Por ejemplo, un script que acepta solicitudes e invoca el modelo, las dependencias de conda, etcétera.
* La configuración de implementación que describe cómo y dónde implementar el modelo.

Para obtener más información, consulte [Implementación de modelos](how-to-deploy-and-where.md).

#### <a name="iot-edge-devices"></a>Dispositivos de IoT Edge

Puede usar modelos con dispositivos IoT a través de los **módulos de Azure IoT Edge**. Los módulos de IoT Edge se implementan en un dispositivo de hardware, lo que permite la inferencia o puntuación de modelos en el dispositivo.

Para obtener más información, consulte [Implementación de modelos](how-to-deploy-and-where.md).

### <a name="analytics"></a>Análisis

Microsoft Power BI admite el uso de modelos de Machine Learning para el análisis de datos. Para obtener más información, consulte la [integración de Azure Machine Learning en Power BI (versión preliminar)](https://docs.microsoft.com/power-bi/service-machine-learning-integration).


## <a name="monitor-ml-applications-for-operational-and-ml-related-issues"></a>Supervise las aplicaciones de ML en busca de problemas relacionados con aprendizaje automático y operaciones

La supervisión permite comprender qué datos se envían a su modelo y las predicciones que devuelve.

Esta información le ayudará a comprender cómo se usa el modelo. Los datos de entrada recopilados también pueden ser útiles para entrenar versiones futuras del modelo.

Para más información, consulte [cómo habilitar la recopilación de datos de un modelo](how-to-enable-data-collection.md).


## <a name="capture-an-end-to-end-audit-trail-of-the-ml-lifecycle"></a>Captura de un registro de auditoría completo del ciclo de vida de ML

Azure Machine Learning le ofrece la capacidad de realizar un seguimiento del registro de auditoría completo de todos los recursos de aprendizaje automático. Concretamente:

- Azure Machine Learning se integra con Git para realizar un seguimiento de la información sobre el repositorio, rama o commit del que procede el código.
- Los conjuntos de datos de Azure Machine Learning le ayudan a supervisar y realizar versiones de sus datos.
- El historial de ejecución de Azure Machine Learning administra el código, los datos y los procesos utilizados para entrenar un modelo.
- El registro de modelos de Azure Machine Learning captura todos los metadatos asociados al modelo (el experimento que lo entrenó, dónde se está implementando, si las implementaciones son correctas).

## <a name="automate-the-end-to-end-ml-lifecycle"></a>Automatización del ciclo de vida completo de Machine Learning 

Puede usar GitHub y Azure Pipelines para crear un proceso de integración continua que entrene un modelo. En un escenario común, cuando un científico de datos comprueba un cambio en el repositorio de Git para un proyecto, Azure Pipelines inicia una ejecución de entrenamiento. A continuación, se pueden inspeccionar los resultados de la ejecución para ver las características de rendimiento del modelo entrenado. También puede crear una canalización que implemente el modelo como un servicio web.

La [extensión de Azure Machine Learning](https://marketplace.visualstudio.com/items?itemName=ms-air-aiagility.vss-services-azureml) facilita el trabajo con Azure Pipelines. Ofrece las siguientes mejoras a Azure Pipelines:

* Habilita la selección de áreas de trabajo al definir una conexión de servicio.
* Permite que los modelos entrenados creados con una canalización de entrenamiento desencadenen las canalizaciones de versión.

Para obtener más información sobre el uso de Azure Pipelines con Azure Machine Learning, consulte el artículo sobre [integración continua e implementación de modelos de Machine Learning con Azure Pipelines](/azure/devops/pipelines/targets/azure-machine-learning) y el repositorio de [MLOps de Azure Machine Learning Service](https://aka.ms/mlops).

## <a name="next-steps"></a>Pasos siguientes

Más información sobre [cómo y dónde puede implementar modelos](how-to-deploy-and-where.md) con el servicio Azure Machine Learning. Para obtener un ejemplo de implementación, consulte el [Tutorial: Implementación de un modelo de clasificación de imágenes en Azure Container Instances](tutorial-deploy-models-with-aml.md).

Aprenda a generar una [integración continua e implementación de modelos de Machine Learning con Azure Pipelines](/azure/devops/pipelines/targets/azure-machine-learning). 

Obtenga información sobre cómo crear aplicaciones de cliente y servicios que [consumen un modelo implementado como servicio web](how-to-consume-web-service.md).
