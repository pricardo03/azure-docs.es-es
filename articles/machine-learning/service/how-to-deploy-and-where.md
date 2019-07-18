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
ms.openlocfilehash: dcb90eb8ee25b8b0c780006f3555a5a9b815ffdd
ms.sourcegitcommit: 6cb4dd784dd5a6c72edaff56cf6bcdcd8c579ee7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2019
ms.locfileid: "67514304"
---
# <a name="deploy-models-with-the-azure-machine-learning-service"></a>Implementación de modelos con el servicio Azure Machine Learning

Aprenda a implementar el modelo de Machine Learning como un servicio web en la nube de Azure o en dispositivos IoT Edge. 

El flujo de trabajo es siempre parecido independientemente de [donde implemente](#target) el modelo:

1. Registre el modelo.
1. Preparación para la implementación (especificación de recursos, uso y destino de proceso)
1. Implemente el modelo en el destino de proceso.
1. Pruebe el modelo implementado, también denominado servicio web.

Para obtener más información sobre los conceptos implicados en el flujo de trabajo de implementación, consulte [Administración, implementación y supervisión de modelos con Azure Machine Learning Service](concept-model-management-and-deployment.md).

## <a name="prerequisites"></a>Requisitos previos

- Un modelo. Si no tiene un modelo entrenado, puede usar el modelo y los archivos de dependencia que se proporcionan en [este tutorial](https://aka.ms/azml-deploy-cloud).

- La [extensión de la CLI de Azure para Machine Learning Service](reference-azure-machine-learning-cli.md), el [SDK de Python para Azure Machine Learning](https://aka.ms/aml-sdk) o la [extensión de Visual Studio Code para Azure Machine Learning](how-to-vscode-tools.md).

## <a id="registermodel"></a> Registro del modelo

Un contenedor lógico de modelos registrado para uno o varios archivos que componen el modelo. Por ejemplo, si tiene un modelo que se almacena en varios archivos, puede registrarlos como un único modelo en el área de trabajo. Después del registro, puede descargar o implementar el modelo registrado y recibir todos los archivos que se registraron.

Los modelos de Machine Learning se registran en el área de trabajo de Azure Machine Learning. El modelo puede proceder de Azure Machine Learning o de otro lugar. En los ejemplos siguientes se muestra cómo registrar un modelo a partir de un archivo:

### <a name="register-a-model-from-an-experiment-run"></a>Registro de un modelo a partir de una ejecución de experimento

+ **Ejemplo de Scikit-Learn mediante el SDK**
  ```python
  model = run.register_model(model_name='sklearn_mnist', model_path='outputs/sklearn_mnist_model.pkl')
  print(model.name, model.id, model.version, sep='\t')
  ```

  > [!TIP]
  > Para incluir varios archivos en el registro del modelo, establezca `model_path` en el directorio que contiene los archivos.

+ **Uso de la CLI**

  ```azurecli-interactive
  az ml model register -n sklearn_mnist  --asset-path outputs/sklearn_mnist_model.pkl  --experiment-name myexperiment
  ```

  > [!TIP]
  > Para incluir varios archivos en el registro del modelo, establezca `--asset-path` en el directorio que contiene los archivos.

+ **Uso de Visual Studio Code**

  Registre los modelos mediante todas las carpetas o archivos de modelo con la extensión de [Visual Studio Code](how-to-vscode-tools.md#deploy-and-manage-models).

### <a name="register-an-externally-created-model"></a>Registro de un modelo creado externamente

[!INCLUDE [trusted models](../../../includes/machine-learning-service-trusted-model.md)]

Puede registrar un modelo creado externamente proporcionando una **ruta de acceso local** a este. Puede proporcionar una carpeta o un único archivo.

+ **Ejemplo de Open Neural Network Exchange con el SDK de Python:**
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

  > [!TIP]
  > Para incluir varios archivos en el registro del modelo, establezca `model_path` en el directorio que contiene los archivos.

+ **Uso de la CLI**
  ```azurecli-interactive
  az ml model register -n onnx_mnist -p mnist/model.onnx
  ```

  > [!TIP]
  > Para incluir varios archivos en el registro del modelo, establezca `-p` en el directorio que contiene los archivos.

**Tiempo estimado**: Aproximadamente 10 segundos.

Para más información, consulte la documentación de referencia de la [clase Model](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py).

Para obtener más información sobre cómo trabajar con modelos entrenados al margen de Azure Machine Learning Service, consulte [Cómo implementar un modelo existente](how-to-deploy-existing-model.md).

<a name="target"></a>

## <a name="choose-a-compute-target"></a>Elección de un destino de proceso

Se pueden utilizar los siguientes destinos o recursos de proceso para hospedar la implementación del servicio web. 

[!INCLUDE [aml-compute-target-deploy](../../../includes/aml-compute-target-deploy.md)]

## <a name="prepare-to-deploy"></a>Preparación de la actualización

Para implementar como un servicio web, debe crear una configuración de inferencia (`InferenceConfig`) y una configuración de implementación. Inferencia o modelo de puntuación, es la fase donde se usa el modelo implementado para la predicción, frecuentemente en datos de producción. En la configuración de la inferencia, especifique los scripts y dependencias necesarios para servir el modelo. En la configuración de implementación especifique los detalles sobre cómo servir el modelo en el destino de proceso.


### <a id="script"></a> 1. Definición de los scripts y dependencias de entrada

El script de entrada recibe los datos enviados a un servicio web implementado y los pasa al modelo. A continuación, toma la respuesta devuelta por el modelo y la envía al cliente. **El script es específico para el modelo**; debe entender los datos que el modelo espera y devuelve.

El script contiene dos funciones que cargan y ejecutan el modelo:

* `init()`: normalmente, esta función carga el modelo en un objeto global. Esta función solo se ejecuta una vez cuando se inicia el contenedor de Docker para el servicio web.

* `run(input_data)`: esta función usa el modelo para predecir un valor basado en los datos de entrada. Los datos de entrada y salida de la ejecución suelen usar el formato JSON para la serialización y deserialización. También puede trabajar con datos binarios sin formato. Puede transformar los datos antes de enviarlos al modelo o antes de devolverlos al cliente.

#### <a name="what-is-getmodelpath"></a>¿Qué es get_model_path?

Cuando registra un modelo, puede proporcionar un nombre de modelo que se usa para administrar este en el registro. Usará este nombre con [Model.get_model_path()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py#get-model-path-model-name--version-none---workspace-none-) para recuperar la ruta de acceso de los archivos del modelo en el sistema de archivos local. Si registra una carpeta o una colección de archivos, esta API devolverá la ruta de acceso al directorio que contiene esos archivos.

Cuando registra un modelo, puede darle un nombre que se corresponda con la ubicación donde se coloque el modelo, ya sea localmente o durante la implementación del servicio.

El ejemplo siguiente devuelve una ruta de acceso a un único archivo denominado `sklearn_mnist_model.pkl` (que se registró con el nombre `sklearn_mnist`):

```python
model_path = Model.get_model_path('sklearn_mnist')
``` 

#### <a name="optional-automatic-swagger-schema-generation"></a>Generación automática de esquemas con Swagger (opcional)

Para generar automáticamente un esquema para el servicio web, proporcione un ejemplo de la entrada y salida del constructor de uno de los objetos de tipo definidos, y el tipo y ejemplo empleados para crear automáticamente el esquema. Azure Machine Learning Service creará, a continuación, una especificación de [OpenAPI](https://swagger.io/docs/specification/about/) (Swagger) para el servicio web durante la implementación.

Actualmente se admiten los siguientes tipos:

* `pandas`
* `numpy`
* `pyspark`
* Objeto estándar de Python

Para usar la generación de esquemas, incluya el paquete `inference-schema` en el archivo de entorno de Conda. En el ejemplo siguiente se usa `[numpy-support]` ya que el script de entrada utiliza un tipo de parámetro numpy: 

#### <a name="example-dependencies-file"></a>Ejemplo de archivo de dependencias
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

Si desea usar la generación automática de esquemas, el script de entrada **debe** importar los paquetes `inference-schema`. 

Defina los formatos de ejemplo de entrada y salida de las variables `input_sample` y `output_sample`, que representan los formatos de solicitud y respuesta del servicio web. Use estos ejemplos en los decoradores de entrada y salida de la función `run()`. El siguiente ejemplo de Scikit-learn usa generación de esquemas.

> [!TIP]
> Después de implementar el servicio, utilice la propiedad `swagger_uri` para recuperar el documento JSON del esquema.

#### <a name="example-entry-script"></a>Script de entrada de ejemplo

En el siguiente ejemplo se muestra cómo aceptar y devolver datos JSON:

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

#### <a name="example-script-with-dictionary-input-support-consumption-from-power-bi"></a>Script de ejemplo con entrada de diccionario (admite uso de Power BI)

En el ejemplo siguiente se muestra cómo definir datos de entrada como diccionario <clave: valor> mediante Dataframe. Este método se admite para usar el servicio web implementado desde Power BI ([más información sobre cómo utilizar el servicio web desde Power BI](https://docs.microsoft.com/power-bi/service-machine-learning-integration)):

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
Para ver más scripts de ejemplo, consulte estos ejemplos:

* Pytorch: [https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-pytorch](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-pytorch)
* TensorFlow: [https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-tensorflow](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-tensorflow)
* Keras: [https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-keras](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-keras)
* ONNX: [https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/deployment/onnx/](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/deployment/onnx/)
* Puntuación frente a datos binarios: [Consumo de un servicio web](how-to-consume-web-service.md)

### <a name="2-define-your-inferenceconfig"></a>2. Definición de InferenceConfig

La configuración de inferencia describe cómo configurar el modelo para realizar predicciones. En el siguiente ejemplo le indicaremos cómo crear una configuración de inferencia. Esta configuración especifica el runtime, el script de entrada y (opcionalmente) el archivo de entorno de conda:

```python
inference_config = InferenceConfig(runtime= "python",
                                   entry_script="x/y/score.py",
                                   conda_file="env/myenv.yml")
```

Para obtener más información, consulte la referencia de clase [InferenceConfig](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.inferenceconfig?view=azure-ml-py).

Para obtener información sobre el uso de una imagen personalizada de Docker con configuración de inferencia, consulte [Cómo implementar un modelo con una imagen personalizada de Docker](how-to-deploy-custom-docker-image.md).

### <a name="cli-example-of-inferenceconfig"></a>Ejemplo de la CLI de InferenceConfig

El siguiente documento JSON es un ejemplo de configuración de inferencia para su uso con la CLI de aprendizaje automático:

```JSON
{
   "entryScript": "x/y/score.py",
   "runtime": "python",
   "condaFile": "env/myenv.yml",
   "sourceDirectory":"C:/abc",
}
```

Las entidades siguientes son válidas en este archivo:

* __entryScript__: ruta de acceso al archivo local que contiene el código que se ejecuta para la imagen.
* __runtime__: el runtime que se usará para la imagen. Los runtimes admitidos actualmente son "spark-py" y "python".
* __condaFile__ (opcional): ruta de acceso al archivo local que contiene una definición de entorno de conda que se usará para la imagen.
* __extraDockerFileSteps__ (opcional): ruta de acceso al archivo local que contiene los pasos de Docker adicionales que se deben ejecutar al configurar la imagen.
* __sourceDirectory__ (opcional): ruta de acceso a las carpetas que contienen todos los archivos para crear la imagen.
* __enableGpu__ (opcional): determina si se habilita o no la compatibilidad con GPU en la imagen. La imagen GPU se debe usar en servicios de Microsoft Azure como Azure Container Instances, Azure Machine Learning Compute, Azure Virtual Machines y Azure Kubernetes Service. El valor predeterminado es False.
* __baseImage__ (opcional): una imagen personalizada que se usará como imagen base. Si no se especifica ninguna imagen base, la imagen base se usará en función del parámetro de runtime proporcionado.
* __baseImageRegistry__ (opcional): registro de imágenes que contiene la imagen base.
* __cudaVersion__ (opcional): versión de CUDA que se debe instalar para las imágenes que requieren compatibilidad con GPU. La imagen GPU se debe usar en servicios de Microsoft Azure como Azure Container Instances, Azure Machine Learning Compute, Azure Virtual Machines y Azure Kubernetes Service. Las versiones compatibles son 9.0, 9.1 y 10.0. Si se establece "enable_gpu", el valor predeterminado es "9.1".

Estas entidades se asignan a los parámetros de la clase [InferenceConfig](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.inferenceconfig?view=azure-ml-py).

El comando siguiente muestra cómo implementar un modelo mediante la CLI:

```azurecli-interactive
az ml model deploy -n myservice -m mymodel:1 --ic inferenceconfig.json
```

En este ejemplo, la configuración contiene los siguientes elementos:

* Un directorio que contiene los recursos necesarios para realizar la inferencia
* Este modelo requiere Python
* El [script de entrada](#script), que se usa para controlar las solicitudes web que se envían al servicio implementado
* El archivo de Conda que describe los paquetes de Python necesarios para realizar la inferencia

Para obtener información sobre el uso de una imagen personalizada de Docker con configuración de inferencia, consulte [Cómo implementar un modelo con una imagen personalizada de Docker](how-to-deploy-custom-docker-image.md).

### <a name="3-define-your-deployment-configuration"></a>3. Definición de la configuración de la implementación

Antes de efectuar la implementación, debe definir la configuración de esta. La configuración de implementación es específica del destino de proceso que hospedará el servicio web. Por ejemplo, cuando se implementa de forma local debe especificar el puerto en el que el servicio aceptará las solicitudes.

Puede que también deba crear el recurso de proceso. Por ejemplo, si aún no dispone de una instancia de Azure Kubernetes Service asociada al área de trabajo.

La tabla siguiente proporciona un ejemplo de creación de una configuración de implementación para cada destino de proceso:

| Destino de proceso | Ejemplo de configuración de implementación |
| ----- | ----- |
| Local | `deployment_config = LocalWebservice.deploy_configuration(port=8890)` |
| Azure Container Instances | `deployment_config = AciWebservice.deploy_configuration(cpu_cores = 1, memory_gb = 1)` |
| Azure Kubernetes Service | `deployment_config = AksWebservice.deploy_configuration(cpu_cores = 1, memory_gb = 1)` |

Las secciones siguientes muestran cómo crear la configuración de implementación y cómo usarla posteriormente para implementar el servicio web.

### <a name="optional-profile-your-model"></a>Opcional: Generación del perfil del modelo
Antes de implementar el modelo como un servicio, puede que desee generar un perfil para determinar los requisitos de CPU y memoria óptimos. Puede generar perfiles del modelo mediante el SDK o la CLI.

Para más información, revise la documentación del SDK aquí: https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py#profile-workspace--profile-name--models--inference-config--input-data-

Los resultados de la generación de perfiles del modelo se emiten como un objeto de ejecución.
Puede encontrar los detalles específicos sobre el esquema de generación de perfiles del modelo aquí: https://docs.microsoft.com/python/api/azureml-core/azureml.core.profile.modelprofile?view=azure-ml-py

## <a name="deploy-to-target"></a>Implementación en el destino

### <a id="local"></a> Implementación local

Para implementar de forma local, deberá tener **Docker instalado** en la máquina local.

+ **Uso del SDK**

  ```python
  deployment_config = LocalWebservice.deploy_configuration(port=8890)
  service = Model.deploy(ws, "myservice", [model], inference_config, deployment_config)
  service.wait_for_deployment(show_output = True)
  print(service.state)
  ```

+ **Uso de la CLI**

  ```azurecli-interactive
  az ml model deploy -m sklearn_mnist:1 -ic inferenceconfig.json -dc deploymentconfig.json
  ```

### <a id="aci"></a> Azure Container Instances (DEVTEST)

Utilice Azure Container Instances para implementar los modelos como un servicio web si se dan una o varias de las siguientes condiciones:
- Debe implementar y validar rápidamente el modelo.
- Está probando un modelo que está en desarrollo. 

Para ver la disponibilidad de cuotas y regiones de ACI, consulte el artículo [Disponibilidad de cuotas y regiones en Azure Container Instances](https://docs.microsoft.com/azure/container-instances/container-instances-quotas).

+ **Uso del SDK**

  ```python
  deployment_config = AciWebservice.deploy_configuration(cpu_cores = 1, memory_gb = 1)
  service = Model.deploy(ws, "aciservice", [model], inference_config, deployment_config)
  service.wait_for_deployment(show_output = True)
  print(service.state)
  ```

+ **Uso de la CLI**

  ```azurecli-interactive
  az ml model deploy -m sklearn_mnist:1 -n aciservice -ic inferenceconfig.json -dc deploymentconfig.json
  ```


+ **Uso de Visual Studio Code**

  Para [implementar los modelos con VS Code](how-to-vscode-tools.md#deploy-and-manage-models) no es necesario crear un contenedor ACI para realizar pruebas por adelantado, ya que estos contenedores se crean sobre la marcha.

Para más información, consulte la documentación de referencia de las clases [AciWebservice](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.aciwebservice?view=azure-ml-py) y [Webservice](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.webservice?view=azure-ml-py).

### <a id="aks"></a>Azure Kubernetes Service (DEVTEST & PRODUCTION)

Puede usar un clúster AKS existente o crear uno nuevo con el SDK de Azure Machine Learning, la CLI o Azure Portal.

<a id="deploy-aks"></a>

Si ya tiene un clúster de AKS asociado, puede realizar la implementación en él. Si aún no ha creado un clúster de AKS o no lo ha asociado, siga el proceso para <a href="#create-attach-aks">crear un nuevo clúster de AKS</a>.

+ **Uso del SDK**

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

+ **Uso de la CLI**

  ```azurecli-interactive
  az ml model deploy -ct myaks -m mymodel:1 -n aksservice -ic inferenceconfig.json -dc deploymentconfig.json
  ```

+ **Uso de Visual Studio Code**

  También puede [realizar la implementación en AKS mediante la extensión de VS Code](how-to-vscode-tools.md#deploy-and-manage-models), pero necesitará configurar clústeres de AKS de antemano.

Más información sobre la implementación de AKS y el escalado automático en la referencia [AksWebservice.deploy_configuration](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.akswebservice).

#### Creación de un clúster nuevo<a id="create-attach-aks"></a>
**Tiempo estimado**: aproximadamente 20 minutos.

Crear o asociar un clúster de AKS es un proceso único en el área de trabajo. Puede volver a usar este clúster con diferentes implementaciones. Si elimina el clúster o el grupo de recursos que lo contiene, tendrá que crear un nuevo clúster la próxima vez que tenga que realizar una implementación. Puede tener varios clústeres de AKS asociados al área de trabajo.

Si desea crear un clúster de AKS para desarrollo, validación y pruebas, puede establecer `cluster_purpose = AksCompute.ClusterPurpose.DEV_TEST` al usar [`provisioning_configuration()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.akscompute?view=azure-ml-py). Un clúster creado con esta configuración solo tendrá un nodo.

> [!IMPORTANT]
> Si establece `cluster_purpose = AksCompute.ClusterPurpose.DEV_TEST` creará un clúster de AKS que no es adecuado para administrar el tráfico de producción. Los tiempos de inferencia pueden ser mayores que en un clúster creado para producción. La tolerancia a errores no está tampoco garantizada en los clústeres de desarrollo y pruebas.
>
> Se recomienda que los clústeres creados para desarrollo y pruebas utilicen al menos dos CPU virtuales.

En el ejemplo siguiente se muestra cómo crear un nuevo clúster de Azure Kubernetes Service:

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

Para más información sobre cómo crear un clúster de AKS fuera del SDK de Azure Machine Learning, consulte los artículos siguientes:
* [Creación de un clúster de AKS](https://docs.microsoft.com/cli/azure/aks?toc=%2Fazure%2Faks%2FTOC.json&bc=%2Fazure%2Fbread%2Ftoc.json&view=azure-cli-latest#az-aks-create)
* [Creación de un clúster de AKS: Portal](https://docs.microsoft.com/azure/aks/kubernetes-walkthrough-portal?view=azure-cli-latest)

Para más información sobre el parámetro `cluster_purpose`, consulte la referencia [AksCompute.ClusterPurpose](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.aks.akscompute.clusterpurpose?view=azure-ml-py).

> [!IMPORTANT]
> Para [`provisioning_configuration()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.akscompute?view=azure-ml-py), si elige valores personalizados para agent_count y vm_size, deberá asegurarse de que agent_count multiplicado por vm_size sea mayor o igual que 12 CPU virtuales. Por ejemplo, si usa un valor de vm_size de "Standard_D3_v2", que tiene 4 CPU virtuales, debe elegir un valor de agent_count de 3 o mayor.
>
> El SDK de Azure Machine Learning no admite escalar un clúster de AKS. Para escalar los nodos en el clúster, use la interfaz de usuario para el clúster de AKS en Azure Portal. Solo puede cambiar el número de nodos, no el tamaño de máquina virtual del clúster.

#### <a name="attach-an-existing-aks-cluster"></a>Asociación de un clúster de AKS ya existente
**Tiempo estimado:** Aproximadamente 5 minutos.

Si ya tiene un clúster de AKS en su suscripción a Azure y es de la versión 1.12.## puede usarlo para implementar la imagen.

> [!WARNING]
> Cuando se asocia un clúster de AKS a un área de trabajo, puede definir cómo utilizará el clúster estableciendo el parámetro `cluster_purpose`.
>
> Si no establece el parámetro `cluster_purpose`, o establece `cluster_purpose = AksCompute.ClusterPurpose.FAST_PROD`, el clúster deberá tener al menos 12 CPU virtuales disponibles.
>
> Si establece `cluster_purpose = AksCompute.ClusterPurpose.DEV_TEST`, el clúster no necesitará tener 12 CPU virtuales. No obstante, un clúster que está configurado para desarrollo y pruebas no es adecuado para un tráfico de nivel de producción y puede aumentar los tiempos de inferencia.

El código siguiente muestra cómo asociar un clúster AKS 1.12.## existente al área de trabajo:

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

Para más información sobre `attack_configuration()`, consulte la referencia [AksCompute.attach_configuration()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.akscompute?view=azure-ml-py#attach-configuration-resource-group-none--cluster-name-none--resource-id-none--cluster-purpose-none-).

Para más información sobre el parámetro `cluster_purpose`, consulte la referencia [AksCompute.ClusterPurpose](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.aks.akscompute.clusterpurpose?view=azure-ml-py).

## <a name="consume-web-services"></a>Consumo de servicios web

Cada servicio web implementado proporciona una API REST, por lo que puede crear aplicaciones cliente en diversos lenguajes de programación. Si ha habilitado la autenticación para el servicio, deberá proporcionar una clave de servicio como token en el encabezado de solicitud.

### <a name="request-response-consumption"></a>Consumo de solicitud-respuesta

Este es un ejemplo de cómo invocar el servicio en Python:
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


### <a id="azuremlcompute"></a> Inferencia por lotes
Los destinos de procesos de Azure Machine Learning se crean y administran mediante Azure Machine Learning Service. Estos se pueden usar para la predicción por lotes en canalizaciones de Azure Machine Learning.

Para ver un tutorial sobre la inferencia por lotes con Azure Machine Learning Compute, lea el artículo [Cómo ejecutar predicciones por lotes](how-to-run-batch-predictions.md).

### <a id="iotedge"></a> Inferencia de IoT Edge
La compatibilidad con implementaciones en el perímetro está en versión preliminar. Para más información, consulte el artículo [Implementación de Azure Machine Learning como un módulo de IoT Edge](https://docs.microsoft.com/azure/iot-edge/tutorial-deploy-machine-learning).


## <a id="update"></a> Actualización de servicios web

Cuando cree un nuevo modelo, debe actualizar manualmente cada servicio que quiera que use el nuevo modelo. Para actualizar el servicio web, utilice el método `update`. El código siguiente muestra cómo actualizar el servicio web para usar un nuevo modelo:

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

## <a name="continuous-model-deployment"></a>Implementación de modelos continua 

Puede implementar continuamente modelos mediante la extensión de Machine Learning para [Azure DevOps](https://azure.microsoft.com/services/devops/). Mediante el uso de la extensión de Machine Learning para Azure DevOps, puede desencadenar una canalización de implementación cuando se registra un nuevo modelo de aprendizaje automático en el área de trabajo de Azure Machine Learning Service. 

1. Regístrese en [Azure Pipelines](https://docs.microsoft.com/azure/devops/pipelines/get-started/pipelines-sign-up?view=azure-devops), lo que hace posible la integración continua y la entrega de la aplicación en cualquier plataforma y en cualquier nube. Azure Pipelines [difiere de Canalizaciones de Machine Learning](concept-ml-pipelines.md#compare). 

1. [Cree un proyecto de Azure DevOps.](https://docs.microsoft.com/azure/devops/organizations/projects/create-project?view=azure-devops)

1. Instalación de la [extensión de aprendizaje automático para Azure Pipelines](https://marketplace.visualstudio.com/items?itemName=ms-air-aiagility.vss-services-azureml&targetId=6756afbe-7032-4a36-9cb6-2771710cadc2&utm_source=vstsproduct&utm_medium=ExtHubManageList) 

1. Use __Conexiones de servicio__ para configurar una conexión de entidad de servicio para el área de trabajo de Azure Machine Learning Service para tener acceso a todos los artefactos. Vaya a Configuración del proyecto, haga clic en Conexiones de servicio y seleccione Azure Resource Manager.

    ![view-service-connection](media/how-to-deploy-and-where/view-service-connection.png) 

1. Defina AzureMLWorkspace como el __nivel de ámbito__ y rellene los parámetros subsiguientes.

    ![view-azure-resource-manager](media/how-to-deploy-and-where/resource-manager-connection.png)

1. A continuación, para implementar continuamente el modelo de aprendizaje automático mediante Azure Pipelines, seleccione __Versión__ en Canalizaciones. Agregue un artefacto nuevo y seleccione el artefacto del modelo de AzureML y la conexión de servicio que se ha creado en el paso anterior. Seleccione el modelo y la versión para desencadenar una implementación. 

    ![select-AzureMLmodel-artifact](media/how-to-deploy-and-where/enable-modeltrigger-artifact.png)

1. Habilite el desencadenador del modelo en el artefacto del modelo. Al activar el desencadenador, cada vez que se registra la versión especificada (es decir, la versión más reciente) de ese modelo en el área de trabajo, se desencadena una canalización de versiones de Azure DevOps. 

    ![enable-model-trigger](media/how-to-deploy-and-where/set-modeltrigger.png)

En [el repositorio MLOps](https://github.com/Microsoft/MLOps), encontrará proyectos de muestra y ejemplos.

## <a name="clean-up-resources"></a>Limpieza de recursos
Para eliminar un servicio web implementado, use `service.delete()`.
Para eliminar un modelo registrado, use `model.delete()`.

Para más información, consulte la documentación de referencia para [WebService.delete()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice(class)?view=azure-ml-py#delete--) y [Model.delete()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py#delete--).

## <a name="next-steps"></a>Pasos siguientes
* [Cómo implementar un modelo con una imagen personalizada de Docker](how-to-deploy-custom-docker-image.md)
* [Solución de problemas de implementación](how-to-troubleshoot-deployment.md)
* [Protección de los servicios web de Azure Machine Learning con SSL](how-to-secure-web-service.md)
* [Consumir un modelo de ML que está implementado como un servicio web](how-to-consume-web-service.md)
* [Supervisión de los modelos de Azure Machine Learning con Application Insights](how-to-enable-app-insights.md)
* [Recopilar datos de modelos en producción](how-to-enable-data-collection.md)

