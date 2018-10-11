---
title: Paquete FPGA para aceleración de hardware de Azure Machine Learning
description: Obtenga información sobre los paquetes Python disponibles para los usuarios de Azure Machine Learning.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: reference
ms.reviewer: jmartens
ms.author: tedway
author: tedway
ms.date: 05/07/2018
ROBOTS: NOINDEX
ms.openlocfilehash: 344423ea9943e04dc5f02ebee0903d179be90bbd
ms.sourcegitcommit: 55952b90dc3935a8ea8baeaae9692dbb9bedb47f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2018
ms.locfileid: "48887588"
---
# <a name="azure-machine-learning-hardware-acceleration-package"></a>Paquete Azure Machine Learning Hardware Acceleration

>[!Note]
>**Este artículo está en desuso.** Este paquete FPGA ha quedado en desuso. La compatibilidad con esta funcionalidad se agregó al SDK de Azure Machine Learning. El soporte técnico para este paquete finalizará gradualmente. [Ver la escala de tiempo del soporte técnico](overview-what-happened-to-workbench.md#timeline). Obtenga información sobre el [soporte técnico para FPGA](concept-accelerate-with-fpgas.md) actualizado.

El paquete Azure Machine Learning Hardware Acceleration es una extensión que se puede instalar con PIP de Python para Azure Machine Learning que permite que los científicos de datos y los desarrolladores puedan hacer rápidamente lo siguiente:

+ Caracterizar las imágenes con una versión cuantizada de ResNet 50.

+ Entrenar los clasificadores según esas características.

+ Implementar los modelos en [matrices de puertas programables por campos (FPGA)](concept-accelerate-with-fpgas.md) en Azure para obtener una inferencia de latencia muy baja.

## <a name="prerequisites"></a>Requisitos previos

1. Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

1. Una cuenta de Administración de modelos de Azure Machine Learning. Para más información sobre cómo crear la cuenta, consulte el documento de [instalación de Workbench y el inicio rápido de Azure Machine Learning](../desktop-workbench/quickstart-installation.md). 

1. El paquete debe estar instalado. 

 
## <a name="how-to-install-the-package"></a>Instalación del paquete

1. Descargue e instale la versión más reciente de [Git](https://git-scm.com/downloads).

2. Instale [Anaconda (Python 3.6)](https://conda.io/miniconda.html).

   Para descargar un entorno Anaconda preconfigurado, use el comando siguiente desde el símbolo del sistema de Git:

    ```
    git clone https://aka.ms/aml-real-time-ai
    ```
1. Para crear el entorno, abra un **símbolo del sistema de Anaconda** y use el comando siguiente:

    ```
    conda env create -f aml-real-time-ai/environment.yml
    ```

1. Para activar el entorno, use el comando siguiente:

    ```
    conda activate amlrealtimeai
    ```

## <a name="sample-code"></a>Código de ejemplo

Este código de ejemplo le explica cómo usar el SDK para implementar un modelo en una FPGA.

1. Importe el paquete:
   ```python
   import amlrealtimeai
   from amlrealtimeai import resnet50
   ```

1. Preprocese la imagen:
   ```python 
   from amlrealtimeai.resnet50.model import LocalQuantizedResNet50
   model_path = os.path.expanduser('~/models')
   model = LocalQuantizedResNet50(model_path)
   print(model.version)
   ```

1. Caracterice las imágenes:
   ```python 
   from amlrealtimeai.resnet50.model import LocalQuantizedResNet50
   model_path = os.path.expanduser('~/models')
   model = LocalQuantizedResNet50(model_path)
   print(model.version)
   ```

1. Cree un clasificador:
   ```python
   model.import_graph_def(include_featurizer=False)
   print(model.classifier_input)
   print(model.classifier_output)
   ```

1. Cree la definición de servicio:
   ```python
   from amlrealtimeai.pipeline import ServiceDefinition, TensorflowStage, BrainWaveStage
   save_path = os.path.expanduser('~/models/save')
   service_def_path = os.path.join(save_path, 'service_def.zip')

   service_def = ServiceDefinition()
   service_def.pipeline.append(TensorflowStage(tf.Session(), in_images, image_tensors))
   service_def.pipeline.append(BrainWaveStage(model))
   service_def.pipeline.append(TensorflowStage(tf.Session(), model.classifier_input, model.classifier_output))
   service_def.save(service_def_path)
   print(service_def_path)
   ```
 
1. Prepare el modelo para que se ejecute en una FPGA:
   ```python
   from amlrealtimeai import DeploymentClient

   subscription_id = "<Your Azure Subscription ID>"
   resource_group = "<Your Azure Resource Group Name>"
   model_management_account = "<Your AzureML Model Management Account Name>"

   model_name = "resnet50-model"
   service_name = "quickstart-service"

   deployment_client = DeploymentClient(subscription_id, resource_group, model_management_account)
   ```

1. Implemente el modelo para que se ejecute en una FPGA:
   ```python
   service = deployment_client.get_service_by_name(service_name)
   model_id = deployment_client.register_model(model_name, service_def_path)

   if(service is None):
      service = deployment_client.create_service(service_name, model_id)    
   else:
      service = deployment_client.update_service(service.id, model_id)
   ```

1. Cree el cliente:
    ```python
   from amlrealtimeai import PredictionClient
   client = PredictionClient(service.ipAddress, service.port)  
   ```

1. Llame a la API:
   ```python
   image_file = R'C:\path_to_file\image.jpg'
   results = client.score_image(image_file)
   ```

## <a name="reporting-issues"></a>Información sobre los problemas

Use el [foro](https://aka.ms/aml-forum-service) para informar cualquier problema que encuentre con este paquete.

## <a name="next-steps"></a>Pasos siguientes

[Implementación de un modelo como un servicio web en una FPGA](how-to-deploy-fpga-web-service.md)