---
title: Implementación de un modelo para la inferencia con GPU
titleSuffix: Azure Machine Learning service
description: En este artículo se enseña a usar Azure Machine Learning Service para implementar un modelo de aprendizaje profundo de TensorFlow habilitado para GPU como un servicio web y las solicitudes de inferencia de puntuación.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: vaidyas
author: csteegz
ms.reviewer: larryfr
ms.date: 06/01/2019
ms.openlocfilehash: 8086d059913cc61bff0bca31681368bea6d76777
ms.sourcegitcommit: 5bdd50e769a4d50ccb89e135cfd38b788ade594d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2019
ms.locfileid: "67543794"
---
# <a name="deploy-a-deep-learning-model-for-inference-with-gpu"></a>Implementación de un modelo de aprendizaje profundo para la inferencia con GPU

En este artículo se enseña a usar Azure Machine Learning Service para implementar un modelo de aprendizaje profundo de TensorFlow habilitado para GPU como un servicio web.

Implemente el modelo a un clúster de Azure Kubernetes Service (AKS) para realizar inferencias habilitadas para GPU. Inferencia o modelo de puntuación es la fase donde se usa el modelo implementado para la predicción. El uso de GPU en lugar de las CPU ofrece ventajas de rendimiento en el cálculo que se puede paralelizar considerablemente.

Aunque en este ejemplo se usa un modelo de TensorFlow, puede aplicar los pasos siguientes en cualquier marco de aprendizaje automático que admita GPU realizando pequeños cambios en el archivo de puntuación y el archivo de entorno. 

En este artículo, podrá llevar a cabo estos pasos:

* Crear un clúster de AKS habilitado para GPU.
* Implementar un modelo de GPU TensorFlow.
* Emitir una consulta de ejemplo para el modelo implementado.

## <a name="prerequisites"></a>Requisitos previos

* Un área de trabajo de servicios de Azure Machine Learning.
* Una distribución de Python.
* Un modelo guardado de Tensorflow registrado.
    * Para obtener información sobre cómo registrar modelos, vea [Implementación de modelos](../service/how-to-deploy-and-where.md#registermodel).

Puede completar la primera parte de esta serie de procedimientos, [Cómo entrenar un modelo de TensorFlow](how-to-train-tensorflow.md), para cumplir los requisitos previos necesarios.

## <a name="provision-an-aks-cluster-with-gpus"></a>Aprovisionar un clúster de AKS con GPU

Azure tiene muchas opciones diferentes de GPU. Puede usar cualquiera de ellas para inferencia. Consulte [la lista de máquinas virtuales de la serie N](https://azure.microsoft.com/pricing/details/virtual-machines/linux/#n-series) para obtener un desglose completo de funcionalidades y costos.

Para obtener más información sobre el uso de AKS con Azure Machine Learning Service, consulte [Cómo implementar y dónde](../service/how-to-deploy-and-where.md#deploy-aks).

```Python
# Choose a name for your cluster
aks_name = "aks-gpu"

# Check to see if the cluster already exists
try:
    compute_target = ComputeTarget(workspace=ws, name=aks_name)
    print('Found existing compute target')
except ComputeTargetException:
    print('Creating a new compute target...')
    # Provision AKS cluster with GPU machine
    prov_config = AksCompute.provisioning_configuration(vm_size="Standard_NC6")

    # Create the cluster
    aks_target = ComputeTarget.create(
        workspace=ws, name=aks_name, provisioning_configuration=prov_config
    )

    aks_target.wait_for_completion(show_output=True)
```

> [!IMPORTANT]
> Azure le facturará siempre y cuando se aprovisione el clúster de AKS. Asegúrese de eliminar el clúster de AKS cuando haya terminado con él.

## <a name="write-the-entry-script"></a>Escribir el script de entrada

Guarde el código siguiente en el directorio de trabajo como `score.py`. Este archivo puntúa las imágenes a medida que se envían a su servicio. Carga el modelo guardado de TensorFlow, pasa la imagen de entrada a la sesión de TensorFlow en cada solicitud POST y, después, devuelve las puntuaciones resultantes. Otros marcos de inferencia requieren diferentes archivos de puntuación.

```python
import json
import numpy as np
import os
import tensorflow as tf

from azureml.core.model import Model

def init():
    global X, output, sess
    tf.reset_default_graph()
    model_root = Model.get_model_path('tf-dnn-mnist')
    saver = tf.train.import_meta_graph(os.path.join(model_root, 'mnist-tf.model.meta'))
    X = tf.get_default_graph().get_tensor_by_name("network/X:0")
    output = tf.get_default_graph().get_tensor_by_name("network/output/MatMul:0")
    
    sess = tf.Session()
    saver.restore(sess, os.path.join(model_root, 'mnist-tf.model'))

def run(raw_data):
    data = np.array(json.loads(raw_data)['data'])
    # make prediction
    out = output.eval(session=sess, feed_dict={X: data})
    y_hat = np.argmax(out, axis=1)
    return y_hat.tolist()

```
## <a name="define-the-conda-environment"></a>Definir el entorno de conda

Cree un archivo de entorno de conda denominado `myenv.yml` para especificar las dependencias para el servicio. Es importante especificar que está usando `tensorflow-gpu` para lograr un rendimiento acelerado.

```yaml
name: project_environment
dependencies:
  # The python interpreter version.
  # Currently Azure ML only supports 3.5.2 and later.
- python=3.6.2

- pip:
  - azureml-defaults==1.0.43.*
- numpy
- tensorflow-gpu=1.12
channels:
- conda-forge
```

## <a name="define-the-gpu-inferenceconfig-class"></a>Definir la clase de GPU InferenceConfig

Cree un objeto `InferenceConfig` que habilite las GPU y se asegure de que CUDA se instala con la imagen de Docker.

```python
from azureml.core.model import Model
from azureml.core.model import InferenceConfig

aks_service_name ='aks-dnn-mnist'
gpu_aks_config = AksWebservice.deploy_configuration(autoscale_enabled = False, 
                                                    num_replicas = 3, 
                                                    cpu_cores=2, 
                                                    memory_gb=4)
model = Model(ws,"tf-dnn-mnist")

inference_config = InferenceConfig(runtime= "python", 
                                   entry_script="score.py",
                                   conda_file="myenv.yml", 
                                   enable_gpu=True)
```

Para más información, consulte:

- [Clase InferenceConfig](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.inferenceconfig?view=azure-ml-py)
- [Clase AksServiceDeploymentConfiguration](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.aks.aksservicedeploymentconfiguration?view=azure-ml-py)

## <a name="deploy-the-model"></a>Implementación del modelo

Implemente el modelo en el clúster de AKS y espere a que se cree el servicio.

```python
aks_service = Model.deploy(ws,
                           models=[model],
                           inference_config=inference_config, 
                           deployment_config=gpu_aks_config,
                           deployment_target=aks_target,
                           name=aks_service_name)

aks_service.wait_for_deployment(show_output = True)
print(aks_service.state)
```

> [!NOTE]
> Azure Machine Learning Service no implementará un modelo con un objeto `InferenceConfig` que espere que la GPU esté habilitada para un clúster que no tiene una GPU.

Para obtener más información, consulte [Clase Model](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py).

## <a name="issue-a-sample-query-to-your-model"></a>Emitir una consulta de ejemplo para el modelo.

Enviar una consulta de prueba para el modelo implementado. Cuando se envía una imagen jpeg al modelo, este puntúa la imagen. El código de ejemplo siguiente usa una función de utilidad externa para cargar imágenes. Puede encontrar el código pertinente en el PIR [Ejemplo de TensorFlow en GitHub](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-tensorflow/utils.py). 

```python
# Used to test your webservice
from utils import load_data 

# Load test data from model training
X_test = load_data('./data/mnist/test-images.gz', False) / 255.0
y_test = load_data('./data/mnist/test-labels.gz', True).reshape(-1)

# send a random row from the test set to score
random_index = np.random.randint(0, len(X_test)-1)
input_data = "{\"data\": [" + str(list(X_test[random_index])) + "]}"

api_key = aks_service.get_keys()[0]
headers = {'Content-Type':'application/json', 'Authorization':('Bearer '+ api_key)}
resp = requests.post(aks_service.scoring_uri, input_data, headers=headers)

print("POST to url", aks_service.scoring_uri)
#print("input data:", input_data)
print("label:", y_test[random_index])
print("prediction:", resp.text)
```

> [!IMPORTANT]
> Para minimizar la latencia y optimizar el rendimiento, asegúrese de que el cliente está en la misma región de Azure que el punto de conexión. En este ejemplo, las API se crean en la región de Azure Este de EE. UU.

## <a name="clean-up-the-resources"></a>Limpieza de recursos

Elimine los recursos cuando haya terminado con este ejemplo.

> [!IMPORTANT]
> Azure factura en función de cuánto tiempo se implementa el clúster de AKS. Asegúrese de limpiarlo cuando haya terminado con él.

```python
aks_service.delete()
aks_target.delete()
```

## <a name="next-steps"></a>Pasos siguientes

* [Implementar el modelo en FPGA](../service/how-to-deploy-fpga-web-service.md)
* [Implementar el modelo con ONNX](../service/concept-onnx.md#deploy-onnx-models-in-azure)
* [Entrenar modelos DNN de TensorFlow](../service/how-to-train-tensorflow.md)
