---
title: Implementar un modelo para la inferencia de GPU
titleSuffix: Azure Machine Learning service
description: Obtenga información sobre cómo implementar un modelo de aprendizaje profundo como un servicio web que utiliza una GPU para la inferencia. En este artículo, se implementa un modelo de Tensorflow en un clúster de Azure Kubernetes Service. El clúster usa una máquina virtual basados en GPU para hospedar el servicio web y las solicitudes de inferencia de puntuación.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: vaidyas
author: csteegz
ms.reviewer: larryfr
ms.date: 05/02/2019
ms.openlocfilehash: 64d42b9082895e372bb780d2db023294c1a0a380
ms.sourcegitcommit: 67625c53d466c7b04993e995a0d5f87acf7da121
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/20/2019
ms.locfileid: "65884723"
---
# <a name="deploy-a-deep-learning-model-for-inference-with-gpu"></a>Implementar un modelo de aprendizaje profundo para la inferencia de GPU

Obtenga información sobre cómo usar la inferencia de GPU para un modelo implementado como un servicio web de aprendizaje automático. Inferencia o modelo de puntuación, es la fase donde se usa el modelo implementado para la predicción, con más frecuencia en los datos de producción.

En este artículo le enseña a usar el servicio Azure Machine Learning para implementar un modelo a un clúster de Azure Kubernetes Service (AKS) en una máquina de virtual (VM) basados en GPU de aprendizaje profundo de Tensorflow de ejemplo. Cuando se envían las solicitudes al servicio, el modelo usa la GPU para ejecutar las cargas de trabajo de inferencia.

Las GPU ofrecen ventajas de rendimiento con respecto a CPU en cálculo paralelizar. Casos de uso excelente para máquinas virtuales habilitadas para GPU incluyen aprendizaje profundo aprendizaje e inferencia, especialmente para grandes lotes de solicitudes de modelo.

En este ejemplo se muestra cómo implementar un TensorFlow guarda el modelo en Azure Machine Learning. Siga estos pasos:

* Crear un clúster AKS habilitadas para GPU
* Implementar un modelo de Tensorflow GPU

## <a name="prerequisites"></a>Requisitos previos

* Un área de trabajo de Azure Machine Learning services
* Una distribución de Python
* Tensorflow registrado que se guarda el modelo. Para obtener información sobre cómo registrar modelos, vea [implementar modelos](../service/how-to-deploy-and-where.md#registermodel).

En este artículo se basa en el cuaderno de Jupyter, [implementar modelos de Tensorflow en AKS](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/deployment/production-deploy-to-aks-gpu/production-deploy-to-aks-gpu.ipynb). El cuaderno de Jupyter usa TensorFlow guarda modelos y las implementa en un clúster de AKS. También puede aplicar el Bloc de notas para cualquier plataforma que admite GPU realizando pequeños cambios en el archivo de puntuación y el archivo de entorno de aprendizaje de automático.  

## <a name="provision-an-aks-cluster-with-gpus"></a>Aprovisionar un clúster de AKS con GPU

Azure tiene muchas opciones diferentes de GPU. Puede usar cualquiera de ellos para inferencia. Consulte [la lista de máquinas virtuales de la serie N](https://azure.microsoft.com/pricing/details/virtual-machines/linux/#n-series) para obtener un desglose completo de funcionalidades y los costos.

Para obtener más información sobre el uso de AKS con el servicio de Azure Machine Learning, consulte [cómo implementar y dónde](../service/how-to-deploy-and-where.md#deploy-aks).

```python
# Provision AKS cluster with GPU machine
prov_config = AksCompute.provisioning_configuration(vm_size="Standard_NC6")

# Create the cluster
aks_target = ComputeTarget.create(
    workspace=ws, name=aks_name, provisioning_configuration=prov_config
)

aks_target.wait_for_deployment()
```

> [!IMPORTANT]
> Azure le facturará siempre y cuando se aprovisiona el clúster de AKS. Asegúrese de eliminar el clúster de AKS cuando haya terminado con él.

## <a name="write-the-entry-script"></a>Escribir el script de entrada

Guarde el código siguiente en el directorio de trabajo como `score.py`. Este archivo de puntuaciones imágenes como se envían a su servicio. Carga el modelo de TensorFlow guardado, pasa la imagen de entrada a la sesión de TensorFlow en cada solicitud POST y, a continuación, devuelve las puntuaciones resultantes. Otros marcos de inferencia requieren diferentes archivos de puntuación.

```python
import tensorflow as tf
import numpy as np
import ujson
from azureml.core.model import Model
from azureml.contrib.services.aml_request import AMLRequest, rawhttp
from azureml.contrib.services.aml_response import AMLResponse

def init():
    global session
    global input_name
    global output_name
    
    session = tf.Session()

    model_path = Model.get_model_path('resnet50')
    model = tf.saved_model.loader.load(session, ['serve'], model_path)
    if len(model.signature_def['serving_default'].inputs) > 1:
        raise ValueError("This score.py only supports one input")
    input_name = [tensor.name for tensor in model.signature_def['serving_default'].inputs.values()][0]
    output_name = [tensor.name for tensor in model.signature_def['serving_default'].outputs.values()]
    

@rawhttp
def run(request):
    if request.method == 'POST':
        reqBody = request.get_data(False)
        resp = score(reqBody)
        return AMLResponse(resp, 200)
    if request.method == 'GET':
        respBody = str.encode("GET is not supported")
        return AMLResponse(respBody, 405)
    return AMLResponse("bad request", 500)

def score(data):
    result = session.run(output_name, {input_name: [data]})
    return ujson.dumps(result[1])

if __name__ == "__main__":
    init()
    with open("lynx.jpg", 'rb') as f: #load file for testing locally
        content = f.read()
        print(score(content))

```

## <a name="define-the-conda-environment"></a>Definir el entorno de conda

Cree un archivo de entorno de conda denominado `myenv.yml` para especificar las dependencias para el servicio. Es importante especificar que está usando `tensorflow-gpu` para lograr un rendimiento acelerado.

```yaml
name: aml-accel-perf
channels:
  - defaults
dependencies:
  - tensorflow-gpu = 1.12
  - numpy
  - ujson
  - pip:
    - azureml-core
    - azureml-contrib-services
```

## <a name="define-the-gpu-inferenceconfig-class"></a>Definir la clase InferenceConfig de GPU

Crear un `InferenceConfig` objeto que permite que las GPU y se asegura de que CUDA se instala con la imagen de Docker.

```python
from azureml.core.model import Model
from azureml.core.model import InferenceConfig

aks_service_name ='gpu-rn'
gpu_aks_config = AksWebservice.deploy_configuration(autoscale_enabled = False, 
                                                    num_replicas = 3, 
                                                    cpu_cores=2, 
                                                    memory_gb=4)
model = Model(ws,"resnet50")

inference_config = InferenceConfig(runtime= "python", 
                                   entry_script="score.py",
                                   conda_file="myenv.yml", 
                                   gpu_enabled=True)
```

Para más información, consulte:

- [Clase InferenceConfig](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.inferenceconfig?view=azure-ml-py)
- [Clase AksServiceDeploymentConfiguration](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.aks.aksservicedeploymentconfiguration?view=azure-ml-py)

## <a name="deploy-the-model"></a>Implementación del modelo

Implementar el modelo en el clúster de AKS y espere a que se crea el servicio.

```python
aks_service = Model.deploy(ws,
                           models=[model],
                           inference_config=inference_config, 
                           deployment_config=aks_config,
                           deployment_target=aks_target,
                           name=aks_service_name)

aks_service.wait_for_deployment(show_output = True)
print(aks_service.state)
```

> [!NOTE]
> Servicio Azure Machine Learning no implementa un modelo con un `InferenceConfig` objeto que espera GPU esté habilitado para un clúster que no tiene una GPU.

Para obtener más información, consulte [clase modelo](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py).

## <a name="issue-a-sample-query-to-your-deployed-model"></a>Emitir una consulta de ejemplo para el modelo implementado

Enviar una consulta de prueba con el modelo implementado. Cuando se envía una imagen jpeg al modelo, puntúa la imagen.

```python
scoring_url = aks_service.scoring_uri
api_key = aks_service.get_key()(0)
IMAGEURL = "https://upload.wikimedia.org/wikipedia/commons/thumb/6/68/Lynx_lynx_poing.jpg/220px-Lynx_lynx_poing.jpg"

headers = {'Authorization':('Bearer '+ api_key)}
img_data = read_image_from(IMAGEURL).read()
r = requests.post(scoring_url, data = img_data, headers=headers)
```

> [!IMPORTANT]
> Para minimizar la latencia y optimizar el rendimiento, asegúrese de que el cliente está en la misma región de Azure como punto de conexión. En este ejemplo, las API se crean en la región East US Azure.

## <a name="clean-up-the-resources"></a>Limpiar los recursos

Elimine los recursos cuando haya terminado con este ejemplo.

> [!IMPORTANT]
> Azure factura en función de cuánto se implementa el clúster de AKS. Asegúrese de que limpiar una vez haya terminado con él.

```python
aks_service.delete()
aks_target.delete()
```

## <a name="next-steps"></a>Pasos siguientes

* [Implementar el modelo en FPGA](../service/how-to-deploy-fpga-web-service.md)
* [Implementar el modelo con ONNX](../service/concept-onnx.md#deploy-onnx-models-in-azure)
* [Entrenar modelos de DNN de Tensorflow](../service/how-to-train-tensorflow.md)
