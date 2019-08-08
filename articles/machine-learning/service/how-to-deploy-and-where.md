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
ms.date: 07/08/2019
ms.custom: seoapril2019
ms.openlocfilehash: d26d1ca1ebceed481604d08d12cd9d5010495ab6
ms.sourcegitcommit: 08d3a5827065d04a2dc62371e605d4d89cf6564f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/29/2019
ms.locfileid: "68618415"
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
  az ml model register -n sklearn_mnist  --asset-path outputs/sklearn_mnist_model.pkl  --experiment-name myexperiment --run-id myrunid
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

> [!IMPORTANT]
> El SDK de Azure Machine Learning no proporciona una manera para que las implementaciones de servicios web o IoT Edge tengan acceso al almacén de datos o a los conjuntos de datos. Si necesita que el modelo implementado tenga acceso a los datos almacenados fuera de la implementación, como en una cuenta de Azure Storage, debe desarrollar una solución de código personalizada mediante el SDK pertinente. Por ejemplo, el [SDK de Azure Storage para Python](https://github.com/Azure/azure-storage-python).
>
> Otra alternativa que puede funcionar para su escenario son las [predicciones por lotes](how-to-run-batch-predictions.md), que proporcionan acceso a los almacenes de datos cuando se realiza la puntuación.

### <a id="script"></a> 1. Definición de los scripts y dependencias de entrada

El script de entrada recibe los datos enviados a un servicio web implementado y los pasa al modelo. A continuación, toma la respuesta devuelta por el modelo y la envía al cliente. **El script es específico para el modelo**; debe entender los datos que el modelo espera y devuelve.

El script contiene dos funciones que cargan y ejecutan el modelo:

* `init()`: normalmente, esta función carga el modelo en un objeto global. Esta función solo se ejecuta una vez cuando se inicia el contenedor de Docker para el servicio web.

* `run(input_data)`: esta función usa el modelo para predecir un valor basado en los datos de entrada. Los datos de entrada y salida de la ejecución suelen usar el formato JSON para la serialización y deserialización. También puede trabajar con datos binarios sin formato. Puede transformar los datos antes de enviarlos al modelo o antes de devolverlos al cliente.

#### <a name="what-is-get_model_path"></a>¿Qué es get_model_path?

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


input_sample = np.array([[10, 9, 8, 7, 6, 5, 4, 3, 2, 1]])
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
    # replace model_name with your actual model name, if needed
    model_path = Model.get_model_path('model_name')
    # deserialize the model file back into a sklearn model
    model = joblib.load(model_path)


input_sample = pd.DataFrame(data=[{
    # This is a decimal type sample. Use the data type that reflects this column in your data
    "input_name_1": 5.1,
    # This is a string type sample. Use the data type that reflects this column in your data
    "input_name_2": "value2",
    # This is a integer type sample. Use the data type that reflects this column in your data
    "input_name_3": 3
}])

# This is a integer type sample. Use the data type that reflects the expected result
output_sample = np.array([0])


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
inference_config = InferenceConfig(runtime="python",
                                   entry_script="x/y/score.py",
                                   conda_file="env/myenv.yml")
```

Para obtener más información, consulte la referencia de clase [InferenceConfig](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.inferenceconfig?view=azure-ml-py).

Para obtener información sobre el uso de una imagen personalizada de Docker con configuración de inferencia, consulte [Cómo implementar un modelo con una imagen personalizada de Docker](how-to-deploy-custom-docker-image.md).

### <a name="cli-example-of-inferenceconfig"></a>Ejemplo de la CLI de InferenceConfig

[!INCLUDE [inference config](../../../includes/machine-learning-service-inference-config.md)]

El comando siguiente muestra cómo implementar un modelo mediante la CLI:

```azurecli-interactive
az ml model deploy -n myservice -m mymodel:1 --ic inferenceconfig.json
```

En este ejemplo, la configuración contiene los siguientes elementos:

* Este modelo requiere Python
* El [script de entrada](#script), que se usa para controlar las solicitudes web que se envían al servicio implementado
* El archivo de Conda que describe los paquetes de Python necesarios para realizar la inferencia

Para obtener información sobre el uso de una imagen personalizada de Docker con configuración de inferencia, consulte [Cómo implementar un modelo con una imagen personalizada de Docker](how-to-deploy-custom-docker-image.md).

### <a name="3-define-your-deployment-configuration"></a>3. Definición de la configuración de la implementación

Antes de efectuar la implementación, debe definir la configuración de esta. __La configuración de implementación es específica del destino de proceso que hospedará el servicio web__. Por ejemplo, cuando se implementa de forma local debe especificar el puerto en el que el servicio aceptará las solicitudes.

Puede que también deba crear el recurso de proceso. Por ejemplo, si aún no dispone de una instancia de Azure Kubernetes Service asociada al área de trabajo.

La tabla siguiente proporciona un ejemplo de creación de una configuración de implementación para cada destino de proceso:

| Destino de proceso | Ejemplo de configuración de implementación |
| ----- | ----- |
| Local | `deployment_config = LocalWebservice.deploy_configuration(port=8890)` |
| Azure Container Instances | `deployment_config = AciWebservice.deploy_configuration(cpu_cores = 1, memory_gb = 1)` |
| Azure Kubernetes Service | `deployment_config = AksWebservice.deploy_configuration(cpu_cores = 1, memory_gb = 1)` |

> [!TIP]
> Antes de implementar el modelo como un servicio, puede que desee generar un perfil para determinar los requisitos de CPU y memoria óptimos. Puede generar perfiles del modelo mediante el SDK o la CLI. Para obtener más información, vea la referencia [profile()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py#profile-workspace--profile-name--models--inference-config--input-data-) y [az ml model profile](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/model?view=azure-cli-latest#ext-azure-cli-ml-az-ml-model-profile).
>
> Los resultados de la generación de perfiles del modelo se emiten como un objeto `Run`. Para obtener más información, consulte la referencia de la clase [ModelProfile](https://docs.microsoft.com/python/api/azureml-core/azureml.core.profile.modelprofile?view=azure-ml-py).

## <a name="deploy-to-target"></a>Implementación en el destino

La implementación usa la configuración de implementación de la configuración de inferencia para implementar los modelos. El proceso de implementación es similar, independientemente del destino de proceso. La implementación en AKS es ligeramente diferente, ya que debe proporcionar una referencia al clúster de AKS.

### <a id="local"></a> Implementación local

Para implementar de forma local, deberá tener **Docker instalado** en la máquina local.

#### <a name="using-the-sdk"></a>Uso del SDK

```python
deployment_config = LocalWebservice.deploy_configuration(port=8890)
service = Model.deploy(ws, "myservice", [model], inference_config, deployment_config)
service.wait_for_deployment(show_output = True)
print(service.state)
```

Para más información, consulte la documentación de referencia de [LocalWebservice](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.local.localwebservice?view=azure-ml-py), [Model.deploy()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py#deploy-workspace--name--models--inference-config--deployment-config-none--deployment-target-none-) y [Webservice](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.webservice?view=azure-ml-py).

#### <a name="using-the-cli"></a>Uso de la CLI

Para realizar una implementación con la CLI, use el siguiente comando. Reemplace `mymodel:1` por el nombre y la versión del modelo registrado:

```azurecli-interactive
az ml model deploy -m mymodel:1 -ic inferenceconfig.json -dc deploymentconfig.json
```

[!INCLUDE [aml-local-deploy-config](../../../includes/machine-learning-service-local-deploy-config.md)]

Para obtener más información, consulte la referencia [az ml model deploy](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/model?view=azure-cli-latest#ext-azure-cli-ml-az-ml-model-deploy).

### <a id="aci"></a> Azure Container Instances (DEVTEST)

Consulte [Implementación en Azure Container Instances](how-to-deploy-azure-container-instance.md).

### <a id="aks"></a>Azure Kubernetes Service (DEVTEST & PRODUCTION)

Consulte [Implementación en Azure Kubernetes Service](how-to-deploy-azure-kubernetes-service.md).

## <a name="consume-web-services"></a>Consumo de servicios web

Cada servicio web implementado proporciona una API REST, por lo que puede crear aplicaciones cliente en diversos lenguajes de programación. Si ha habilitado la autenticación para el servicio, deberá proporcionar una clave de servicio como token en el encabezado de solicitud.

### <a name="request-response-consumption"></a>Consumo de solicitud-respuesta

Este es un ejemplo de cómo invocar el servicio en Python:
```python
import requests
import json

headers = {'Content-Type': 'application/json'}

if service.auth_enabled:
    headers['Authorization'] = 'Bearer '+service.get_keys()[0]

print(headers)

test_sample = json.dumps({'data': [
    [1, 2, 3, 4, 5, 6, 7, 8, 9, 10],
    [10, 9, 8, 7, 6, 5, 4, 3, 2, 1]
]})

response = requests.post(
    service.scoring_uri, data=test_sample, headers=headers)
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

[!INCLUDE [aml-update-web-service](../../../includes/machine-learning-update-web-service.md)]

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

