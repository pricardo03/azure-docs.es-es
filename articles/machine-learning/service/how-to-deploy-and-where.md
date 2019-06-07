---
title: Cómo y dónde implementar los modelos
titleSuffix: Azure Machine Learning service
description: 'Obtenga información sobre cómo y dónde implementar los modelos de Azure Machine Learning Service, incluidos: Azure Container Instances, Azure Kubernetes Service, Azure IoT Edge y matrices de puertas programables.'
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: jordane
author: jpe316
ms.reviewer: larryfr
ms.date: 05/31/2019
ms.custom: seoapril2019
ms.openlocfilehash: 89539509e759da7f041ce0216397b1a9c8ff1f16
ms.sourcegitcommit: 45e4466eac6cfd6a30da9facd8fe6afba64f6f50
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/07/2019
ms.locfileid: "66753102"
---
# <a name="deploy-models-with-the-azure-machine-learning-service"></a>Implementación de modelos con el servicio Azure Machine Learning

Obtenga información sobre cómo implementar el modelo de machine learning como un servicio web en la nube de Azure o en dispositivos IoT Edge. 

El flujo de trabajo es similar con independencia de [donde implementar](#target) el modelo:

1. Registre el modelo.
1. Preparar la implementación (especificar recursos de uso, el destino de proceso)
1. Implementar el modelo en el destino de proceso.
1. Probar el modelo implementado, también denominado servicio web.

Para obtener más información sobre los conceptos implicados en el flujo de trabajo de implementación, consulte [Administración, implementación y supervisión de modelos con Azure Machine Learning Service](concept-model-management-and-deployment.md).

## <a name="prerequisites"></a>Requisitos previos

- Un modelo. Si no tiene un modelo entrenado, puede usar el modelo de & archivos de dependencia proporcionan en [este tutorial](https://aka.ms/azml-deploy-cloud).

- El [extensión de la CLI de Azure para el servicio Machine Learning](reference-azure-machine-learning-cli.md), [SDK de Python de Azure Machine Learning](https://aka.ms/aml-sdk), o el [extensión de código de Visual Studio de Azure Machine Learning](how-to-vscode-tools.md).

## <a id="registermodel"></a> Registrar el modelo

Registre los modelos en el área de trabajo de Azure Machine Learning de aprendizaje automático. El modelo puede proceder de Azure Machine Learning o puede proceder de otro lugar. Los ejemplos siguientes muestran cómo registrar un modelo de archivo:

### <a name="register-a-model-from-an-experiment-run"></a>Registrar un modelo a partir de una ejecución de experimento

+ **Ejemplo de Scikit-Learn con el SDK**
  ```python
  model = run.register_model(model_name='sklearn_mnist', model_path='outputs/sklearn_mnist_model.pkl')
  print(model.name, model.id, model.version, sep='\t')
  ```
+ **Mediante la CLI**
  ```azurecli-interactive
  az ml model register -n sklearn_mnist  --asset-path outputs/sklearn_mnist_model.pkl  --experiment-name myexperiment
  ```


+ **Usar VS Code**

  Registrar modelos con los archivos de modelo o las carpetas con el [VS Code](how-to-vscode-tools.md#deploy-and-manage-models) extensión.

### <a name="register-an-externally-created-model"></a>Registrar un modelo creado externamente

[!INCLUDE [trusted models](../../../includes/machine-learning-service-trusted-model.md)]

Puede registrar un modelo creado externamente proporcionando un **ruta de acceso local** al modelo. Puede proporcionar una carpeta o un único archivo.

+ **Ejemplo ONNX con el SDK de Python:**
  ```python
  onnx_model_url = "https://www.cntk.ai/OnnxModels/mnist/opset_7/mnist.tar.gz"
  urllib.request.urlretrieve(onnx_model_url, filename="mnist.tar.gz")
  !tar xvzf mnist.tar.gz
  
  model = Model.register(workspace = ws,
                         model_path ="mnist/model.onnx",
                         model_name = "onnx_mnist",
                         tags = {"onnx": "demo"},
                         description = "MNIST image classification CNN from ONNX Model Zoo",)
  ```

+ **Mediante la CLI**
  ```azurecli-interactive
  az ml model register -n onnx_mnist -p mnist/model.onnx
  ```

**Tiempo estimado**: Aproximadamente 10 segundos.

Para más información, consulte la documentación de referencia de la [clase Model](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py).

<a name="target"></a>

## <a name="choose-a-compute-target"></a>Elija un destino de proceso

El siguiente proceso destinos, o los recursos de proceso, se puede usar para hospedar la implementación del servicio web. 

[!INCLUDE [aml-compute-target-deploy](../../../includes/aml-compute-target-deploy.md)]

## <a name="prepare-to-deploy"></a>Preparación de la actualización

Para implementar como un servicio web, debe crear una configuración de inferencia (`InferenceConfig`) y una configuración de implementación. Inferencia o modelo de puntuación, es la fase donde se usa el modelo implementado para la predicción, con más frecuencia en los datos de producción. En la configuración de la inferencia, especifique los scripts y las dependencias necesarias para atender el modelo. En la configuración de implementación especifique detalles de cómo servir el modelo en el destino de proceso.


### <a id="script"></a> 1. Definir la secuencia de comandos de entrada & dependencias

El script de entrada recibe los datos enviados a un servicio web implementado y lo pasa al modelo. A continuación, toma la respuesta devuelta por el modelo y la envía al cliente. **La secuencia de comandos es específico de su modelo**; deben entender los datos que el modelo de espera y se devuelve.

El script contiene dos funciones que cargar y ejecutan el modelo:

* `init()`: normalmente, esta función carga el modelo en un objeto global. Esta función se ejecuta solo una vez cuando se inicia el contenedor de Docker para el servicio web.

* `run(input_data)`: esta función usa el modelo para predecir un valor basado en los datos de entrada. Los datos de entrada y salida de la ejecución suelen usar el formato JSON para la serialización y deserialización. También puede trabajar con datos binarios sin formato. Puede transformar los datos antes de enviarlos al modelo o antes de devolverlos al cliente.

#### <a name="optional-automatic-swagger-schema-generation"></a>(Opcional) Generación automática de esquema de Swagger

Para generar un esquema para el servicio web automáticamente, proporcionar una muestra de la entrada o salida en el constructor para uno de los objetos de tipo definido y el tipo y el ejemplo se usan para crear automáticamente el esquema. Servicio Azure Machine Learning, a continuación, crea un [OpenAPI](https://swagger.io/docs/specification/about/) especificación (Swagger) para el servicio web durante la implementación.

Actualmente se admiten los siguientes tipos:

* `pandas`
* `numpy`
* `pyspark`
* objeto de Python estándar

Para usar la generación de esquemas, incluya el `inference-schema` paquete en el archivo de entorno de conda. En el ejemplo siguiente se usa `[numpy-support]` puesto que la secuencia de comandos de entrada utiliza un tipo de parámetro numpy: 

#### <a name="example-dependencies-file"></a>Archivo de dependencias de ejemplo
El siguiente código YAML es un ejemplo de un archivo de dependencias de Conda para inferencia.

```YAML
name: project_environment
dependencies:
  - python=3.6.2
  - pip:
    - azureml-defaults
    - scikit-learn==0.20.0
    - inference-schema[numpy-support]
```

Si desea usar la generación automática de esquemas, el script de entrada **debe** importar el `inference-schema` paquetes. 

Definir la entrada y salida de los formatos de ejemplo en el `input_sample` y `output_sample` variables que representan los formatos de solicitud y respuesta para el servicio web. Usar estos ejemplos en la entrada y salida de los decoradores de función en el `run()` función. El scikit-aprender el ejemplo siguiente usa la generación de esquema.

> [!TIP]
> Después de implementar el servicio, utilice el `swagger_uri` propiedad para recuperar el documento de esquema JSON.

#### <a name="example-entry-script"></a>Script de ejemplo de entrada

El ejemplo siguiente muestra cómo se aceptan y devuelven datos JSON:

```python
#example: scikit-learn and Swagger
import json
import numpy as np
from sklearn.externals import joblib
from sklearn.linear_model import Ridge
from azureml.core.model import Model

from inference_schema.schema_decorators import input_schema, output_schema
from inference_schema.parameter_types.numpy_parameter_type import NumpyParameterType

def init():
    global model
    # note here "sklearn_regression_model.pkl" is the name of the model registered under
    # this is a different behavior than before when the code is run locally, even though the code is the same.
    model_path = Model.get_model_path('sklearn_regression_model.pkl')
    # deserialize the model file back into a sklearn model
    model = joblib.load(model_path)

input_sample = np.array([[10,9,8,7,6,5,4,3,2,1]])
output_sample = np.array([3726.995])

@input_schema('data', NumpyParameterType(input_sample))
@output_schema(NumpyParameterType(output_sample))
def run(data):
    try:
        result = model.predict(data)
        # you can return any datatype as long as it is JSON-serializable
        return result.tolist()
    except Exception as e:
        error = str(e)
        return error
```

#### <a name="example-script-with-dictionary-input-support-consumption-from-power-bi"></a>Script de ejemplo con la entrada del diccionario (consumo de soporte técnico de Power BI)

En el ejemplo siguiente se muestra cómo definir datos de entrada como < clave: valor > diccionario mediante la trama de datos. Este método se admite para consumir el servicio web implementado desde Power BI ([más información sobre cómo utilizar el servicio web de Power BI](https://docs.microsoft.com/power-bi/service-machine-learning-integration)):

```python
import json
import pickle
import numpy as np
import pandas as pd
import azureml.train.automl
from sklearn.externals import joblib
from azureml.core.model import Model

from inference_schema.schema_decorators import input_schema, output_schema
from inference_schema.parameter_types.numpy_parameter_type import NumpyParameterType
from inference_schema.parameter_types.pandas_parameter_type import PandasParameterType

def init():
    global model
    model_path = Model.get_model_path('model_name')   # replace model_name with your actual model name, if needed
    # deserialize the model file back into a sklearn model
    model = joblib.load(model_path)

input_sample = pd.DataFrame(data=[{
              "input_name_1": 5.1,         # This is a decimal type sample. Use the data type that reflects this column in your data
              "input_name_2": "value2",    # This is a string type sample. Use the data type that reflects this column in your data
              "input_name_3": 3            # This is a integer type sample. Use the data type that reflects this column in your data
            }])

output_sample = np.array([0])              # This is a integer type sample. Use the data type that reflects the expected result

@input_schema('data', PandasParameterType(input_sample))
@output_schema(NumpyParameterType(output_sample))
def run(data):
    try:
        result = model.predict(data)
        # you can return any datatype as long as it is JSON-serializable
        return result.tolist()
    except Exception as e:
        error = str(e)
        return error
```
Para obtener más secuencias de comandos de ejemplo, vea los ejemplos siguientes:

* Pytorch: [https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-pytorch](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-pytorch)
* TensorFlow: [https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-tensorflow](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-tensorflow)
* Keras: [https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-keras](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-keras)
* ONNX: [https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/deployment/onnx/](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/deployment/onnx/)
* Puntuación con datos binarios: [Cómo consumir un servicio web](how-to-consume-web-service.md)

### <a name="2-define-your-inferenceconfig"></a>2. Definir su InferenceConfig

La configuración de inferencia describe cómo configurar el modelo para realizar predicciones. El ejemplo siguiente muestra cómo crear una configuración de inferencia:

```python
inference_config = InferenceConfig(source_directory="C:/abc",
                                   runtime= "python",
                                   entry_script="x/y/score.py",
                                   conda_file="env/myenv.yml")
```

En este ejemplo, la configuración contiene los siguientes elementos:

* Un directorio que contiene recursos necesarios para inferencia
* Que este modelo requiere Python
* El [script de entrada](#script), que se usa para controlar las solicitudes web en el servicio implementado
* El archivo conda que describe los paquetes de Python necesarios para inferencia

Para obtener información sobre la funcionalidad de InferenceConfig, consulte el [configuración avanzada](#advanced-config) sección.

### <a name="3-define-your-deployment-configuration"></a>3. Definir la configuración de implementación

Antes de implementar, debe definir la configuración de implementación. La configuración de implementación es el destino de proceso que se va a hospedar el servicio web específica. Por ejemplo, cuando se implementa de forma local debe especificar el puerto donde el servicio acepta las solicitudes.

También es posible que deba crear el recurso de proceso. Por ejemplo, si aún no tiene dispone de un servicio de Kubernetes de Azure asociado con el área de trabajo.

En la tabla siguiente proporciona un ejemplo de creación de una configuración de implementación para cada destino de proceso:

| Destino de proceso | Ejemplo de configuración de implementación |
| ----- | ----- |
| Local | `deployment_config = LocalWebservice.deploy_configuration(port=8890)` |
| Azure Container Instances | `deployment_config = AciWebservice.deploy_configuration(cpu_cores = 1, memory_gb = 1)` |
| Azure Kubernetes Service | `deployment_config = AksWebservice.deploy_configuration(cpu_cores = 1, memory_gb = 1)` |

Las siguientes secciones muestran cómo crear la configuración de implementación y, a continuación, usarlo para implementar el servicio web.

## <a name="deploy-to-target"></a>Implementar en destino

### <a id="local"></a> Implementación local

Para implementar localmente, deberá tener **Docker instalado** en el equipo local.

+ **Mediante el SDK**

  ```python
  deployment_config = LocalWebservice.deploy_configuration(port=8890)
  service = Model.deploy(ws, "myservice", [model], inference_config, deployment_config)
  service.wait_for_deployment(show_output = True)
  print(service.state)
  ```

+ **Mediante la CLI**

  ```azurecli-interactive
  az ml model deploy -m sklearn_mnist:1 -ic inferenceconfig.json -dc deploymentconfig.json
  ```

### <a id="aci"></a> Azure Container Instances (DEVTEST)

Utilice Azure Container Instances para implementar los modelos como un servicio web si se dan una o varias de las siguientes condiciones:
- Debe implementar y validar rápidamente el modelo.
- Está probando un modelo que está en desarrollo. 

Para ver la disponibilidad de cuotas y regiones de ACI, consulte el [disponibilidad cuotas y regiones de Azure Container Instances](https://docs.microsoft.com/azure/container-instances/container-instances-quotas) artículo.

+ **Mediante el SDK**

  ```python
  deployment_config = AciWebservice.deploy_configuration(cpu_cores = 1, memory_gb = 1)
  service = Model.deploy(ws, "aciservice", [model], inference_config, deployment_config)
  service.wait_for_deployment(show_output = True)
  print(service.state)
  ```

+ **Mediante la CLI**

  ```azurecli-interactive
  az ml model deploy -m sklearn_mnist:1 -n aciservice -ic inferenceconfig.json -dc deploymentconfig.json
  ```


+ **Usar VS Code**

  Para [implementar los modelos con VS Code](how-to-vscode-tools.md#deploy-and-manage-models) no es necesario crear un contenedor de ACI para probar con antelación, ya que se crean contenedores ACI sobre la marcha.

Para más información, consulte la documentación de referencia de las clases [AciWebservice](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.aciwebservice?view=azure-ml-py) y [Webservice](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.webservice?view=azure-ml-py).

### <a id="aks"></a>Azure Kubernetes Service (DEVTEST y producción)

Puede usar un clúster AKS existente o crear uno nuevo con el SDK de Azure Machine Learning, la CLI o Azure Portal.

<a id="deploy-aks"></a>

Si ya tiene un clúster de AKS conectado, puede implementar en él. Si aún no ha creado o asociado a un clúster de AKS, siga el proceso de <a href="#create-attach-aks">crear un nuevo clúster AKS</a>.

+ **Mediante el SDK**

  ```python
  aks_target = AksCompute(ws,"myaks")
  # If deploying to a cluster configured for dev/test, ensure that it was created with enough
  # cores and memory to handle this deployment configuration. Note that memory is also used by
  # things such as dependencies and AML components.
  deployment_config = AksWebservice.deploy_configuration(cpu_cores = 1, memory_gb = 1)
  service = Model.deploy(ws, "aksservice", [model], inference_config, deployment_config, aks_target)
  service.wait_for_deployment(show_output = True)
  print(service.state)
  print(service.get_logs())
  ```

+ **Mediante la CLI**

  ```azurecli-interactive
  az ml model deploy -ct myaks -m mymodel:1 -n aksservice -ic inferenceconfig.json -dc deploymentconfig.json
  ```

+ **Usar VS Code**

  También puede [implementación en AKS mediante la extensión de VS Code](how-to-vscode-tools.md#deploy-and-manage-models), pero necesitará configurar clústeres AKS de antemano.

Obtenga información sobre implementación de AKS y escalado automático en el [AksWebservice.deploy_configuration](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.akswebservice) referencia.

#### Crear un nuevo clúster AKS<a id="create-attach-aks"></a>
**Tiempo estimado:** Aproximadamente 5 minutos.

Crear o adjuntar un clúster de AKS es una vez procesar para el área de trabajo. Puede volver a usar este clúster con diferentes implementaciones. Si elimina el clúster o el grupo de recursos que lo contiene, debe crear un nuevo clúster la próxima vez que necesite para implementar. Puede tener varios clústeres AKS conectados al área de trabajo.

Si desea crear un clúster de AKS para desarrollo, validación y pruebas, establece `cluster_purpose = AksCompute.ClusterPurpose.DEV_TEST` al usar [ `provisioning_configuration()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.akscompute?view=azure-ml-py). Un clúster creado con esta configuración solo tendrá un nodo.

> [!IMPORTANT]
> Establecer `cluster_purpose = AksCompute.ClusterPurpose.DEV_TEST` crea un clúster de AKS que no es adecuado para controlar el tráfico de producción. Tiempos de inferencia sea más largos que en un clúster creado para la producción. Tolerancia a errores no se garantiza que para los clústeres de desarrollo y pruebas.
>
> Se recomienda que los clústeres creados para desarrollo y pruebas utilizan al menos dos CPU virtuales.

El ejemplo siguiente muestra cómo crear un nuevo clúster de Azure Kubernetes Service:

```python
from azureml.core.compute import AksCompute, ComputeTarget

# Use the default configuration (you can also provide parameters to customize this).
# For example, to create a dev/test cluster, use:
# prov_config = AksCompute.provisioning_configuration(cluster_purpose = AksComputee.ClusterPurpose.DEV_TEST)
prov_config = AksCompute.provisioning_configuration()

aks_name = 'myaks'
# Create the cluster
aks_target = ComputeTarget.create(workspace = ws,
                                    name = aks_name,
                                    provisioning_configuration = prov_config)

# Wait for the create process to complete
aks_target.wait_for_completion(show_output = True)
```

Para obtener más información sobre cómo crear un clúster de AKS fuera el SDK de Azure Machine Learning, consulte los artículos siguientes:
* [Crear un clúster de AKS](https://docs.microsoft.com/cli/azure/aks?toc=%2Fazure%2Faks%2FTOC.json&bc=%2Fazure%2Fbread%2Ftoc.json&view=azure-cli-latest#az-aks-create)
* [Crear un clúster de AKS (portal)](https://docs.microsoft.com/azure/aks/kubernetes-walkthrough-portal?view=azure-cli-latest)

Para obtener más información sobre la `cluster_purpose` parámetro, vea el [AksCompute.ClusterPurpose](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.aks.akscompute.clusterpurpose?view=azure-ml-py) referencia.

> [!IMPORTANT]
> Para [`provisioning_configuration()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.akscompute?view=azure-ml-py), si elige valores personalizados para agent_count y vm_size, deberá asegurarse de que agent_count multiplicado por vm_size sea mayor o igual que 12 CPU virtuales. Por ejemplo, si usa un valor de vm_size de "Standard_D3_v2", que tiene 4 CPU virtuales, debe elegir un valor de agent_count de 3 o mayor.

**Tiempo estimado**: aproximadamente 20 minutos.

#### <a name="attach-an-existing-aks-cluster"></a>Adjuntar un clúster AKS existente

Si ya dispone de clúster de AKS en su suscripción de Azure, y es la versión 1.12. ##, puede usar para implementar la imagen.

> [!WARNING]
> Cuando se asocia un clúster de AKS a un área de trabajo, puede definir cómo utilizará el clúster estableciendo el `cluster_purpose` parámetro.
>
> Si no establece la `cluster_purpose` parámetro, o un conjunto `cluster_purpose = AksCompute.ClusterPurpose.FAST_PROD`, a continuación, el clúster debe tener al menos 12 CPU virtuales disponibles.
>
> Si establece `cluster_purpose = AksCompute.ClusterPurpose.DEV_TEST`, a continuación, el clúster no necesita tener 12 CPU virtuales. Sin embargo, un clúster que está configurado para desarrollo y pruebas no es adecuado para el tráfico de nivel de producción y puede aumentar los tiempos de inferencia.

El código siguiente muestra cómo asociar un 1.12 AKS existente. ## clúster al área de trabajo:

```python
from azureml.core.compute import AksCompute, ComputeTarget
# Set the resource group that contains the AKS cluster and the cluster name
resource_group = 'myresourcegroup'
cluster_name = 'mycluster'

# Attach the cluster to your workgroup. If the cluster has less than 12 virtual CPUs, use the following instead:
# attach_config = AksCompute.attach_configuration(resource_group = resource_group,
#                                         cluster_name = cluster_name,
#                                         cluster_purpose = AksCompute.ClusterPurpose.DEV_TEST)
attach_config = AksCompute.attach_configuration(resource_group = resource_group,
                                         cluster_name = cluster_name)
aks_target = ComputeTarget.attach(ws, 'mycompute', attach_config)
```

Para obtener más información sobre `attack_configuration()`, consulte el [AksCompute.attach_configuration()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.akscompute?view=azure-ml-py#attach-configuration-resource-group-none--cluster-name-none--resource-id-none--cluster-purpose-none-) referencia.

Para obtener más información sobre la `cluster_purpose` parámetro, vea el [AksCompute.ClusterPurpose](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.aks.akscompute.clusterpurpose?view=azure-ml-py) referencia.

## <a name="consume-web-services"></a>Consumo de servicios web

Todos los servicios web implementados proporciona una API de REST, por lo que puede crear aplicaciones cliente en una variedad de lenguajes de programación. Si ha habilitado la autenticación para el servicio, deberá proporcionar una clave de servicio como un token en el encabezado de solicitud.

### <a name="request-response-consumption"></a>Consumo de solicitud-respuesta

Este es un ejemplo de cómo invocar el servicio de Python:
```python
import requests
import json

headers = {'Content-Type':'application/json'}

if service.auth_enabled:
    headers['Authorization'] = 'Bearer '+service.get_keys()[0]

print(headers)
    
test_sample = json.dumps({'data': [
    [1,2,3,4,5,6,7,8,9,10], 
    [10,9,8,7,6,5,4,3,2,1]
]})

response = requests.post(service.scoring_uri, data=test_sample, headers=headers)
print(response.status_code)
print(response.elapsed)
print(response.json())
```

Para obtener más información, consulte cómo [crear aplicaciones cliente para consumir servicios web](how-to-consume-web-service.md).


### <a id="azuremlcompute"></a> Inferencia de lote
Objetivos de Machine Learning Compute de Azure se crean y administran mediante el servicio de Azure Machine Learning. Se puede usar para la predicción por lotes de canalizaciones de Azure Machine Learning.

Para ver un tutorial de inferencia de batch con Azure Machine Learning Compute, lea el [cómo ejecutar predicciones por lotes](how-to-run-batch-predictions.md) artículo.

### <a id="iotedge"></a> Inferencia de IoT Edge
Compatibilidad para implementar en el extremo está en versión preliminar. Para obtener más información, consulte el [implementar Azure Machine Learning como un módulo IoT Edge](https://docs.microsoft.com/azure/iot-edge/tutorial-deploy-machine-learning) artículo.


## <a id="update"></a> Actualizar los servicios web

Cuando se crea un nuevo modelo, debe actualizar manualmente cada servicio que desea usar el nuevo modelo. Para actualizar el servicio web, utilice el método `update`. El código siguiente muestra cómo actualizar el servicio web para usar un nuevo modelo:

```python
from azureml.core.webservice import Webservice
from azureml.core.model import Model

# register new model
new_model = Model.register(model_path = "outputs/sklearn_mnist_model.pkl",
                       model_name = "sklearn_mnist",
                       tags = {"key": "0.1"},
                       description = "test",
                       workspace = ws)

service_name = 'myservice'
# Retrieve existing service
service = Webservice(name = service_name, workspace = ws)

# Update to new model(s)
service.update(models = [new_model])
print(service.state)
print(service.get_logs())
```

<a id="advanced-config"></a>

## <a name="advanced-settings"></a>Configuración avanzada 

**<a id="customimage"></a> Usar una imagen base personalizada**

Internamente, InferenceConfig crea una imagen de Docker que contiene el modelo y otros recursos necesarios para el servicio. Si no se especifica, se usa una imagen base predeterminado.

Al crear una imagen para usar con la configuración de la inferencia, la imagen debe cumplir los siguientes requisitos:

* Ubuntu 16.04 o posterior.
* 4.5 Conda. # o superior.
* Python 3.5. # o 3.6. #.

Para usar una imagen personalizada, establezca la `base_image` propiedad de la configuración de inferencia a la dirección de la imagen. El ejemplo siguiente muestra cómo usar una imagen desde ambos un público y privado de Azure Container Registry:

```python
# use an image available in public Container Registry without authentication
inference_config.base_image = "mcr.microsoft.com/azureml/o16n-sample-user-base/ubuntu-miniconda"

# or, use an image available in a private Container Registry
inference_config.base_image = "myregistry.azurecr.io/mycustomimage:1.0"
inference_config.base_image_registry.address = "myregistry.azurecr.io"
inference_config.base_image_registry.username = "username"
inference_config.base_image_registry.password = "password"
```

La siguiente imagen URI son para las imágenes proporcionadas por Microsoft y se puede usar sin proporcionar un valor de nombre o la contraseña de usuario:

* `mcr.microsoft.com/azureml/o16n-sample-user-base/ubuntu-miniconda`
* `mcr.microsoft.com/azureml/onnxruntime:v0.4.0`
* `mcr.microsoft.com/azureml/onnxruntime:v0.4.0-cuda10.0-cudnn7`
* `mcr.microsoft.com/azureml/onnxruntime:v0.4.0-tensorrt19.03`

Para usar estas imágenes, establezca el `base_image` al URI de la lista anterior. Establezca `base_image_registry.address` en `mcr.microsoft.com`.

> [!IMPORTANT]
> Las imágenes de Microsoft que utilizan CUDA o TensorRT deben usarse solo en los servicios de Microsoft Azure.

Para obtener más información acerca de cómo cargar sus propias imágenes en Azure Container Registry, consulte [inserte la primera imagen en un registro de contenedor privado de Docker](https://docs.microsoft.com/azure/container-registry/container-registry-get-started-docker-cli).

Si el modelo está entrenado en Azure Machine Learning Compute, usando __versión 1.0.22 o mayor__ del SDK Azure Machine Learning, se crea una imagen durante el entrenamiento. El ejemplo siguiente muestra cómo usar esta imagen:

```python
# Use an image built during training with SDK 1.0.22 or greater
image_config.base_image = run.properties["AzureML.DerivedImageName"]
```

## <a name="clean-up-resources"></a>Limpieza de recursos
Para eliminar un servicio web implementado, use `service.delete()`.
Para eliminar un modelo registrado, use `model.delete()`.

Para obtener más información, consulte la documentación de referencia [WebService.delete()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice(class)?view=azure-ml-py#delete--), y [Model.delete()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py#delete--).

## <a name="next-steps"></a>Pasos siguientes
* [Solución de problemas de implementación](how-to-troubleshoot-deployment.md)
* [Protección de los servicios web de Azure Machine Learning con SSL](how-to-secure-web-service.md)
* [Consumir un modelo de ML que está implementado como un servicio web](how-to-consume-web-service.md)
* [Supervisión de los modelos de Azure Machine Learning con Application Insights](how-to-enable-app-insights.md)
* [Recopilar datos de modelos en producción](how-to-enable-data-collection.md)

