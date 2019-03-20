---
title: Implementación de modelos como servicios web
titleSuffix: Azure Machine Learning service
description: 'Obtenga información sobre cómo y dónde implementar los modelos de Azure Machine Learning Service, incluidos: Azure Container Instances, Azure Kubernetes Service, Azure IoT Edge y matrices de puertas programables.'
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: aashishb
author: aashishb
ms.reviewer: larryfr
ms.date: 12/07/2018
ms.custom: seodec18
ms.openlocfilehash: f2d2ded849af5054935b6bec8f74e021078b7641
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/18/2019
ms.locfileid: "57860435"
---
# <a name="deploy-models-with-the-azure-machine-learning-service"></a>Implementación de modelos con el servicio Azure Machine Learning

El SDK de Azure Machine Learning proporciona varias maneras que puede implementar el modelo entrenado. En este documento, aprenderá a implementar el modelo como un servicio web en la nube de Azure o en dispositivos IoT Edge.

Puede implementar modelos en los destinos de proceso siguientes:

| Destino de proceso | Tipo de implementación | DESCRIPCIÓN |
| ----- | ----- | ----- |
| [Azure Kubernetes Service (AKS)](#aks) | Inferencia en tiempo real | Es útil para las implementaciones de producción a gran escala. Proporciona escalado automático y tiempos de respuesta rápidos. |
| [Proceso de aprendizaje automático de Azure](#azuremlcompute) | Inferencia de lote | Predicción por lotes se ejecutan en el proceso sin servidor. Es compatible con máquinas virtuales normales y de baja prioridad. |
| [Azure Container Instances (ACI)](#aci) | Prueba | Se recomienda para desarrollo o pruebas. **No es adecuado para cargas de trabajo de producción.** |
| [Azure IoT Edge](#iotedge) | (Versión preliminar) Módulo de IoT | Implementa modelos en dispositivos IoT. Se produce inferencia en el dispositivo. |
| [Matriz de puertas programables (FPGA)](#fpga) | (Versión preliminar) Servicio Web | Latencia ultrabaja para inferencia en tiempo real. |

El proceso de implementación de un modelo es similar para todos los destinos de proceso:

1. Entrene y registre un modelo.
1. Configure y registre una imagen que utilice al modelo.
1. Implemente la imagen en un destino de proceso.
1. Prueba de la implementación

El siguiente vídeo muestra cómo implementar en Azure Container Instances:

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2Kwk3]


Para obtener más información sobre los conceptos implicados en el flujo de trabajo de implementación, consulte [Administración, implementación y supervisión de modelos con Azure Machine Learning Service](concept-model-management-and-deployment.md).

## <a name="prerequisites"></a>Requisitos previos

- Una suscripción de Azure. Si no tiene una suscripción a Azure, cree una cuenta gratuita antes de empezar. Pruebe hoy mismo la [versión gratuita o de pago de Azure Machine Learning Service](https://aka.ms/AMLFree).

- Un área de trabajo del servicio Azure Machine Learning y el SDK de Azure Machine Learning para Python instalado. Obtenga información sobre cómo cumplir estos requisitos previos mediante la [guía de inicio rápido de introducción de Azure Machine Learning](quickstart-get-started.md).

- Un modelo entrenado. Si no tiene un modelo entrenado, siga los pasos del tutorial [Entrenamiento de un modelo](tutorial-train-models-with-aml.md) para entrenar y registrar uno con Azure Machine Learning Service.

    > [!NOTE]
    > Aunque el servicio de Azure Machine Learning puede funcionar con cualquier modelo genérico que se puede cargar en Python 3, los ejemplos de este documento demuestran mediante un modelo que se almacenan en formato de pickle de Python.
    > 
    > Para obtener más información sobre el uso de modelos ONNX, consulte el documento [ONNX y Azure Machine Learning](how-to-build-deploy-onnx.md).

## <a id="registermodel"></a> Registro de un modelo entrenado

El registro de modelos es una manera de almacenar y organizar los modelos entrenados en la nube de Azure. Los modelos se registran en un área de trabajo de Azure Machine Learning Service. El modelo se puede entrenar con Azure Machine Learning u otro servicio. El código siguiente muestra cómo registrar un modelo de archivo, establezca un nombre, etiquetas y una descripción:

```python
from azureml.core.model import Model

model = Model.register(model_path = "outputs/sklearn_mnist_model.pkl",
                       model_name = "sklearn_mnist",
                       tags = {"key": "0.1"},
                       description = "test",
                       workspace = ws)
```

**Tiempo estimado**: Aproximadamente 10 segundos.

Para obtener un ejemplo de registro de un modelo, vea [entrenar un clasificador de imágenes](tutorial-train-models-with-aml.md).

Para más información, consulte la documentación de referencia de la [clase Model](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py).

## <a id="configureimage"></a> Creación y registro de una imagen

Los modelos implementados se empaquetan como una imagen. La imagen contiene las dependencias necesarias para ejecutar el modelo.

Para las implementaciones de **Azure Container Instance**, **Azure Kubernetes Service** y **Azure IoT Edge**, se usa la clase [azureml.core.image.ContainerImage](https://docs.microsoft.com/python/api/azureml-core/azureml.core.image.containerimage?view=azure-ml-py) para crear una configuración de imagen. La configuración de imagen se usa posteriormente para crear una nueva imagen de Docker. 

El código siguiente muestra cómo crear una nueva configuración de imagen:

```python
from azureml.core.image import ContainerImage

# Image configuration
image_config = ContainerImage.image_configuration(execution_script = "score.py",
                                                 runtime = "python",
                                                 conda_file = "myenv.yml"}
                                                 )
```

**Tiempo estimado**: Aproximadamente 10 segundos.

Los parámetros importantes de este ejemplo se describen en la tabla siguiente:

| Parámetro | DESCRIPCIÓN |
| ----- | ----- |
| `execution_script` | Especifica un script de Python que se utiliza para recibir las solicitudes enviadas al servicio. En este ejemplo, el script se encuentra en el archivo `score.py`. Para más información, consulte la sección [Script de ejecución](#script). |
| `runtime` | Indica que la imagen usa Python. La otra opción es `spark-py`, que usa Python con Apache Spark. |
| `conda_file` | Se usa para proporcionar un archivo de entorno conda. Este archivo define el entorno conda para el modelo implementado. Para obtener más información sobre cómo crear este archivo, consulte [Creación del archivo de entorno (myenv.yml)](tutorial-deploy-models-with-aml.md#create-environment-file). |

Para obtener un ejemplo de creación de una configuración de la imagen, consulte [implementar un clasificador de imágenes](tutorial-deploy-models-with-aml.md).

Para más información, consulte la documentación de referencia de la [clase ContainerImage](https://docs.microsoft.com/python/api/azureml-core/azureml.core.image.containerimage?view=azure-ml-py).

### <a id="script"></a> Script de ejecución

El script de ejecución recibe los datos enviados a una imagen implementada y los pasa al modelo. A continuación, toma la respuesta devuelta por el modelo y la envía al cliente. **La secuencia de comandos es específico de su modelo**; deben entender los datos que el modelo de espera y se devuelve. Para ver un script de ejemplo que funciona con un modelo de clasificación de imágenes, consulte [implementar un clasificador de imágenes](tutorial-deploy-models-with-aml.md).

El script contiene dos funciones que cargar y ejecutan el modelo:

* `init()`: normalmente, esta función carga el modelo en un objeto global. Esta función solo se ejecuta una vez cuando se inicia el contenedor de Docker.

* `run(input_data)`: esta función usa el modelo para predecir un valor basado en los datos de entrada. Los datos de entrada y salida de la ejecución suelen usar el formato JSON para la serialización y deserialización. También puede trabajar con datos binarios sin formato. Puede transformar los datos antes de enviarlos al modelo o antes de devolverlos al cliente.

#### <a name="working-with-json-data"></a>Trabajo con datos JSON

El siguiente script de ejemplo acepta y devuelve datos JSON. La función `run` transforma los datos de JSON a un formato que el modelo espere y, a continuación, transforma la respuesta a JSON antes de devolverla:

```python
# import things required by this script
import json
import numpy as np
import os
import pickle
from sklearn.externals import joblib
from sklearn.linear_model import LogisticRegression

from azureml.core.model import Model

# load the model
def init():
    global model
    # retrieve the path to the model file using the model name
    model_path = Model.get_model_path('sklearn_mnist')
    model = joblib.load(model_path)

# Passes data to the model and returns the prediction
def run(raw_data):
    data = np.array(json.loads(raw_data)['data'])
    # make prediction
    y_hat = model.predict(data)
    return json.dumps(y_hat.tolist())
```

#### <a name="working-with-binary-data"></a>Trabajo con datos binarios

Si el modelo acepta __datos binarios__, utilice `AMLRequest`, `AMLResponse` y `rawhttp`. El siguiente script de ejemplo acepta datos binarios y devuelve los bytes invertidos para las solicitudes POST. Para las solicitudes GET, devuelve la dirección URL completa en el cuerpo de la respuesta:

```python
from azureml.contrib.services.aml_request  import AMLRequest, rawhttp
from azureml.contrib.services.aml_response import AMLResponse

def init():
    print("This is init()")

# Accept and return binary data
@rawhttp
def run(request):
    print("This is run()")
    print("Request: [{0}]".format(request))
    # handle GET requests
    if request.method == 'GET':
        respBody = str.encode(request.full_path)
        return AMLResponse(respBody, 200)
    # handle POST requests
    elif request.method == 'POST':
        reqBody = request.get_data(False)
        respBody = bytearray(reqBody)
        respBody.reverse()
        respBody = bytes(respBody)
        return AMLResponse(respBody, 200)
    else:
        return AMLResponse("bad request", 500)
```

> [!IMPORTANT]
> El espacio de nombres `azureml.contrib` cambia con frecuencia mientras trabajamos para mejorar el servicio. Por lo tanto, todo el contenido de este espacio de nombres debe considerarse como una versión preliminar, no completamente compatible con Microsoft.
>
> Si necesita probar esto en su entorno de desarrollo local, puede instalar los componentes en el espacio de nombres `contrib`mediante el comando siguiente: 
> ```shell
> pip install azureml-contrib-services
> ```

### <a id="createimage"></a> Registro de una imagen

Una vez haya creado la configuración de imagen, puede usarla para registrar una imagen. Esta imagen se almacena en el registro de contenedor de su área de trabajo. Una vez creada, puede implementar la misma imagen en varios servicios.

```python
# Register the image from the image configuration
image = ContainerImage.create(name = "myimage", 
                              models = [model], #this is the model object
                              image_config = image_config,
                              workspace = ws
                              )
```

**Tiempo estimado**: aproximadamente 3 minutos.

Las imágenes se versionan automáticamente al registrar varias imágenes con el mismo nombre. Por ejemplo, a la primera imagen registrada como `myimage` se le asigna un identificador de `myimage:1`. La próxima vez registre una imagen como `myimage`, el identificador de la nueva imagen será `myimage:2`.

Para más información, consulte la documentación de referencia de la [clase ContainerImage](https://docs.microsoft.com/python/api/azureml-core/azureml.core.image.containerimage?view=azure-ml-py).

## <a id="deploy"></a> Implementación de la imagen

Al llegar a la implementación, el proceso es ligeramente diferente dependiendo del destino de proceso donde se realiza implementación. Use la información de las secciones siguientes para aprender a realizar la implementación en los destinos siguientes:

| Destino de proceso | Tipo de implementación | DESCRIPCIÓN |
| ----- | ----- | ----- |
| [Azure Kubernetes Service (AKS)](#aks) | Servicio Web (inferencia en tiempo real)| Es útil para las implementaciones de producción a gran escala. Proporciona escalado automático y tiempos de respuesta rápidos. |
| [Proceso de aprendizaje automático de Azure](#azuremlcompute) | Servicio Web (inferencia de lote)| Predicción por lotes se ejecutan en el proceso sin servidor. Es compatible con máquinas virtuales normales y de baja prioridad. |
| [Azure Container Instances (ACI)](#aci) | Servicio Web (desarrollo y pruebas)| Se recomienda para desarrollo o pruebas. **No es adecuado para cargas de trabajo de producción.** |
| [Azure IoT Edge](#iotedge) | (Versión preliminar) Módulo de IoT | Implementa modelos en dispositivos IoT. Se produce inferencia en el dispositivo. |
| [Matriz de puertas programables (FPGA)](#fpga) | (Versión preliminar) Servicio Web | Latencia ultrabaja para inferencia en tiempo real. |

> [!IMPORTANT]
> El uso compartido de recursos entre orígenes (CORS) no se admite actualmente al implementar un modelo como servicio web.

Los ejemplos de esta sección se usa [deploy_from_image](https://docs.microsoft.com/en-us/python/api/azureml-core/azureml.core.webservice(class)?view=azure-ml-py#deploy-from-model-workspace--name--models--image-config--deployment-config-none--deployment-target-none-), lo que requiere que se va a registrar la imagen y el modelo antes de realizar una implementación. Para obtener más información sobre otros métodos de implementación, consulte [implementar](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice(class)?view=azure-ml-py#deploy-workspace--name--model-paths--image-config--deployment-config-none--deployment-target-none-) y [deploy_from_model](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice(class)?view=azure-ml-py#deploy-from-model-workspace--name--models--image-config--deployment-config-none--deployment-target-none-).

### <a id="aci"></a> Implementar en Azure Container Instances (DEVTEST)

Utilice Azure Container Instances para implementar los modelos como un servicio web si se dan una o varias de las siguientes condiciones:

- Debe implementar y validar rápidamente el modelo. La implementación de ACI finaliza en menos de 5 minutos.
- Está probando un modelo que está en desarrollo. Para ver la disponibilidad de cuotas y regiones de ACI, consulte el documento [Disponibilidad de cuotas y regiones en Azure Container Instances](https://docs.microsoft.com/azure/container-instances/container-instances-quotas).

Para realizar la implementación en Azure Container Instances, siga estos pasos:

1. Defina la configuración de la implementación. Esta configuración depende de los requisitos del modelo. En el ejemplo siguiente se define una configuración que usa un núcleo de CPU y 1 GB de memoria:

    [!code-python[](~/aml-sdk-samples/ignore/doc-qa/how-to-deploy-to-aci/how-to-deploy-to-aci.py?name=configAci)]

2. Para implementar la imagen creada en la sección [Crear la imagen](#createimage) de este documento, use el código siguiente:

    [!code-python[](~/aml-sdk-samples/ignore/doc-qa/how-to-deploy-to-aci/how-to-deploy-to-aci.py?name=option3Deploy)]

    **Tiempo estimado**: Aproximadamente 5 minutos.

Para más información, consulte la documentación de referencia de las clases [AciWebservice](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.aciwebservice?view=azure-ml-py) y [Webservice](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.webservice?view=azure-ml-py).

### <a id="aks"></a> Implementación en Azure Kubernetes Service (producción)

Para implementar el modelo como un servicio web de producción a gran escala, use Azure Kubernetes Service (AKS). Puede usar un clúster AKS existente o crear uno nuevo con el SDK de Azure Machine Learning, la CLI o Azure Portal.

Crear un clúster de AKS es un proceso único en el área de trabajo. Puede volver a usar este clúster con diferentes implementaciones. 

> [!IMPORTANT]
> Si elimina el clúster, deberá crear uno nuevo la próxima vez que necesite implementar.

Azure Kubernetes Service proporciona las siguientes funcionalidades:

* Escalado automático
* Registro
* Recopilación de datos de modelos
* Tiempos de respuesta rápidos para los servicios web
* Terminación de TLS
* Authentication

#### <a name="autoscaling"></a>Escalado automático

El escalado automático puede controlarse estableciendo `autoscale_target_utilization`, `autoscale_min_replicas`, y `autoscale_max_replicas` para el AKS servicio web. El ejemplo siguiente muestra cómo habilitar el escalado automático:

```python
aks_config = AksWebservice.deploy_configuration(autoscale_enabled=True, 
                                                autoscale_target_utilization=30,
                                                autoscale_min_replicas=1,
                                                autoscale_max_replicas=4)
```

Las decisiones de escalar o reducir verticalmente se basa en el uso de las réplicas de contenedor actual. El número de réplicas que están ocupadas (procesando una solicitud) dividido por el total de número de réplicas actuales es el uso de la actual. Si este número supera el uso de destino, se crean más réplicas. Si es inferior, se reducen las réplicas. De forma predeterminada, la utilización de destino es 70%.

Las decisiones al agregar las réplicas se crean y se implementa rápidamente (aproximadamente 1 segundo). Decisiones para quitar las réplicas de tardar más tiempo (aproximadamente 1 minuto). Este comportamiento mantiene réplicas inactivas en torno a durante un minuto en caso de que lleguen nuevas solicitudes que pueden controlar.

Puede calcular las réplicas necesarias mediante el código siguiente:

```python
from math import ceil
# target requests per second
targetRps = 20
# time to process the request (in seconds)
reqTime = 10
# Maximum requests per container
maxReqPerContainer = 1
# target_utilization. 70% in this example
targetUtilization = .7

concurrentRequests = targetRps * reqTime / targetUtilization

# Number of container replicas
replicas = ceil(concurrentRequests / maxReqPerContainer)
```

Para obtener más información sobre cómo `autoscale_target_utilization`, `autoscale_max_replicas`, y `autoscale_min_replicas`, consulte el [AksWebservice.deploy_configuration](https://docs.microsoft.com/en-us/python/api/azureml-core/azureml.core.webservice.akswebservice?view=azure-ml-py#deploy-configuration-autoscale-enabled-none--autoscale-min-replicas-none--autoscale-max-replicas-none--autoscale-refresh-seconds-none--autoscale-target-utilization-none--collect-model-data-none--auth-enabled-none--cpu-cores-none--memory-gb-none--enable-app-insights-none--scoring-timeout-ms-none--replica-max-concurrent-requests-none--max-request-wait-time-none--num-replicas-none--primary-key-none--secondary-key-none--tags-none--properties-none--description-none-) referencia.

#### <a name="create-a-new-cluster"></a>Creación de un cliente nuevo

Para crear un nuevo clúster de Azure Kubernetes Service, use el código siguiente:

> [!IMPORTANT]
> Crear el clúster de AKS es un proceso único para el área de trabajo. Una vez creado, puede volver a usar este clúster para varias implementaciones. Si elimina el clúster o el grupo de recursos que lo contiene, tendrá que crear un nuevo clúster la próxima vez que tenga que realizar una implementación.
> Para [`provisioning_configuration()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.akscompute?view=azure-ml-py), si elige valores personalizados para agent_count y vm_size, deberá asegurarse de que agent_count multiplicado por vm_size sea mayor o igual que 12 CPU virtuales. Por ejemplo, si usa un valor de vm_size de "Standard_D3_v2", que tiene 4 CPU virtuales, debe elegir un valor de agent_count de 3 o mayor.

```python
from azureml.core.compute import AksCompute, ComputeTarget

# Use the default configuration (you can also provide parameters to customize this)
prov_config = AksCompute.provisioning_configuration()

aks_name = 'aml-aks-1' 
# Create the cluster
aks_target = ComputeTarget.create(workspace = ws, 
                                    name = aks_name, 
                                    provisioning_configuration = prov_config)

# Wait for the create process to complete
aks_target.wait_for_completion(show_output = True)
print(aks_target.provisioning_state)
print(aks_target.provisioning_errors)
```

**Tiempo estimado**: aproximadamente 20 minutos.

#### <a name="use-an-existing-cluster"></a>Uso de un clúster existente

Si ya dispone de clúster de AKS en su suscripción de Azure, y es la versión 1.11. ## y tiene al menos 12 CPU virtuales, puede usarlo para implementar la imagen. El código siguiente muestra cómo asociar un 1.11 AKS existente. ## clúster al área de trabajo:

```python
from azureml.core.compute import AksCompute, ComputeTarget
# Set the resource group that contains the AKS cluster and the cluster name
resource_group = 'myresourcegroup'
cluster_name = 'mycluster'

# Attach the cluster to your workgroup
attach_config = AksCompute.attach_configuration(resource_group = resource_group,
                                         cluster_name = cluster_name)
aks_target = ComputeTarget.attach(ws, 'mycompute', attach_config)

# Wait for the operation to complete
aks_target.wait_for_completion(True)
```

**Tiempo estimado**: aproximadamente 3 minutos.

Para obtener más información sobre cómo crear un clúster de AKS fuera el SDK de Azure Machine Learning, consulte los artículos siguientes:

* [Crear un clúster de AKS](https://docs.microsoft.com/cli/azure/aks?toc=%2Fen-us%2Fazure%2Faks%2FTOC.json&bc=%2Fen-us%2Fazure%2Fbread%2Ftoc.json&view=azure-cli-latest#az-aks-create)
* [Crear un clúster de AKS (portal)](https://docs.microsoft.com/azure/aks/kubernetes-walkthrough-portal?view=azure-cli-latest)

#### <a name="deploy-the-image"></a>Implementación de la imagen

Para implementar la imagen creada en la sección [Creación de la imagen](#createimage) de este documento al clúster de servidores de Azure Kubernetes, use el código siguiente:

```python
from azureml.core.webservice import Webservice, AksWebservice

# Set configuration and service name
aks_config = AksWebservice.deploy_configuration()
aks_service_name ='aks-service-1'
# Deploy from image
service = Webservice.deploy_from_image(workspace = ws, 
                                            name = aks_service_name,
                                            image = image,
                                            deployment_config = aks_config,
                                            deployment_target = aks_target)
# Wait for the deployment to complete
service.wait_for_deployment(show_output = True)
print(service.state)
```

**Tiempo estimado**: aproximadamente 3 minutos.

Para más información, consulte la documentación de referencia de las clases [AksWebservice](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.akswebservice?view=azure-ml-py) y [Webservice](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.webservice.webservice?view=azure-ml-py).

### <a id="azuremlcompute"></a> Inferencia de proceso de aprendizaje automático de Azure

Destinos de proceso de Azure ML se crean y administran mediante el servicio de Azure Machine Learning. Se puede usar para la predicción por lotes de las canalizaciones de aprendizaje automático de Azure.

Para ver un tutorial de inferencia de batch con Azure Machine Learning Compute, lea el [cómo ejecutar predicciones por lotes](how-to-run-batch-predictions.md) documento.


### <a id="fpga"></a> Implementación en matrices de puertas programables (FPGA)

Project Brainwave permite conseguir una latencia muy baja en solicitudes de inferencia en tiempo real. Project Brainwave acelera las redes neurales profundas (DNN) implementadas en matrices de puertas programables por campo en la nube de Azure. Las DNN que se utilizan habitualmente están disponibles en forma de características para transferir conocimientos o pueden personalizarse con ponderaciones entrenadas con sus propios datos.

Para ver un tutorial sobre la implementación de un modelo con Project Brainwave, consulte el documento sobre la [implementación en una matriz FPGA](how-to-deploy-fpga-web-service.md).

### <a id="iotedge"></a> Implementación en Azure IoT Edge

Un dispositivo Azure IoT Edge es un dispositivo basado en Linux o Windows que ejecuta el entorno de ejecución de Azure IoT Edge. Con Azure IoT Hub, puede implementar modelos de aprendizaje automático en estos dispositivos como módulos de IoT Edge. Implementar un modelo en un dispositivo IoT Edge permite que el dispositivo use el modelo directamente, en lugar de tener que enviar datos a la nube para su procesamiento. Obtendrá tiempos de respuesta más rápidos y una menor transferencia de datos.

Los módulos de Azure IoT Edge se implementan en el dispositivo desde un registro de contenedor. Cuando se crea una imagen desde el modelo, se almacena en el registro de contenedor del área de trabajo.

> [!IMPORTANT]
> La información de esta sección se da por supuesto que ya está familiarizado con los módulos de Azure IoT Hub y Azure IoT Edge. Mientras que parte de la información de esta sección es específica al servicio Azure Machine Learning, se produce la mayor parte del proceso para implementar en un dispositivo de edge en el servicio IoT de Azure.
>
> Si no está familiarizado con Azure IoT, consulte [aspectos básicos de Azure IoT](https://docs.microsoft.com/azure/iot-fundamentals/) y [Azure IoT Edge](https://docs.microsoft.com/azure/iot-edge/) para obtener información básica. A continuación, usar los demás vínculos en esta sección para obtener más información sobre operaciones específicas.

#### <a name="set-up-your-environment"></a>Configuración del entorno

* Un entorno de desarrollo. Para obtener más información, consulte el documento sobre [cómo configurar un entorno de desarrollo](how-to-configure-environment.md).

* Una instancia de [Azure IoT Hub](../../iot-hub/iot-hub-create-through-portal.md) en la suscripción de Azure. 

* Un modelo entrenado. Para obtener un ejemplo de entrenamiento de un modelo, consulte el documento [Train an image classification model with Azure Machine Learning](tutorial-train-models-with-aml.md) (Entrenamiento de un modelo de clasificación de imágenes con Azure Machine Learning). Hay disponible un modelo previamente entrenado en el [Kit de herramientas de AI para el repositorio de Azure IoT Edge GitHub](https://github.com/Azure/ai-toolkit-iot-edge/tree/master/IoT%20Edge%20anomaly%20detection%20tutorial).

#### <a id="getcontainer"></a> Obtener las credenciales del registro de contenedor

Para implementar un módulo de IoT Edge en el dispositivo, Azure IoT necesita las credenciales para el registro de contenedor que almacena las imágenes de Docker en Azure Machine Learning Service.

Puede obtener las credenciales de dos maneras:

+ **En Azure Portal**:

  1. Inicie sesión en el [Azure Portal](https://portal.azure.com/signin/index).

  1. Vaya al área de trabajo del servicio Azure Machine Learning y seleccione __Introducción__. Para ir a la configuración del registro de contenedor, seleccione el vínculo __Registro__.

     ![Imagen de la entrada en el registro de contenedor](./media/how-to-deploy-and-where/findregisteredcontainer.png)

  1. Una vez en el registro de contenedor, seleccione **Claves de acceso** y habilite el usuario administrador.
 
     ![Imagen de la pantalla de claves de acceso](./media/how-to-deploy-and-where/findaccesskey.png)

  1. Guarde los valores de **servidor de inicio de sesión**, **nombre de usuario** y **contraseña**. 

+ **Con un script de Python**:

  1. Use el siguiente script de Python después del código ejecutado anteriormente para crear un contenedor:

     ```python
     # Getting your container details
     container_reg = ws.get_details()["containerRegistry"]
     reg_name=container_reg.split("/")[-1]
     container_url = "\"" + image.image_location + "\","
     subscription_id = ws.subscription_id
     from azure.mgmt.containerregistry import ContainerRegistryManagementClient
     from azure.mgmt import containerregistry
     client = ContainerRegistryManagementClient(ws._auth,subscription_id)
     result= client.registries.list_credentials(resource_group_name, reg_name, custom_headers=None, raw=False)
     username = result.username
     password = result.passwords[0].value
     print('ContainerURL{}'.format(image.image_location))
     print('Servername: {}'.format(reg_name))
     print('Username: {}'.format(username))
     print('Password: {}'.format(password))
     ```
  1. Guarde los valores de ContainerURL, Servername, Username y Password. 

     Estas credenciales son necesarias para proporcionar el acceso de dispositivo IoT Edge a las imágenes en su registro de contenedor privado.

#### <a name="prepare-the-iot-device"></a>Preparación del dispositivo IoT

Registrar el dispositivo con Azure IoT Hub y, a continuación, instalar el runtime de IoT Edge en el dispositivo. Si no está familiarizado con este proceso, consulte [inicio rápido: Implementación del primer módulo de IoT Edge en un dispositivo Linux x64](../../iot-edge/quickstart-linux.md).

Otros métodos para registrar un dispositivo son:

* [Azure Portal](https://docs.microsoft.com/azure/iot-edge/how-to-register-device-portal)
* [CLI de Azure](https://docs.microsoft.com/azure/iot-edge/how-to-register-device-cli)
* [Visual Studio Code](https://docs.microsoft.com/azure/iot-edge/how-to-register-device-vscode)

#### <a name="deploy-the-model-to-the-device"></a>Implementación del modelo en el dispositivo

Para implementar el modelo en el dispositivo, use la información de registro recopilada en el [obtener las credenciales del registro de contenedor](#getcontainer) sección con la implementación del módulo de los pasos para los módulos de IoT Edge. Por ejemplo, cuando [módulos de implementación de Azure IoT Edge desde Azure portal](../../iot-edge/how-to-deploy-modules-portal.md), debe configurar el __configuración del registro__ para el dispositivo. Use la __servidor de inicio de sesión__, __username__, y __contraseña__ para el registro de contenedor del área de trabajo.

También puede implementar mediante [CLI de Azure](https://docs.microsoft.com/azure/iot-edge/how-to-deploy-modules-cli) y [Visual Studio Code](https://docs.microsoft.com/azure/iot-edge/how-to-deploy-modules-vscode).

## <a name="testing-web-service-deployments"></a>Prueba de implementaciones de servicio web

Para probar una implementación de servicio web, puede usar el método `run` del objeto Webservice. En el ejemplo siguiente, un documento JSON está establecido en un servicio web y se muestra el resultado. Los datos enviados deben coincidir con lo que espera el modelo. En este ejemplo, el formato de datos coincide con la entrada esperada por el modelo de diabetes.

```python
import json

test_sample = json.dumps({'data': [
    [1,2,3,4,5,6,7,8,9,10], 
    [10,9,8,7,6,5,4,3,2,1]
]})
test_sample = bytes(test_sample,encoding = 'utf8')

prediction = service.run(input_data = test_sample)
print(prediction)
```

El servicio web es una API REST, por lo que puede crear aplicaciones cliente en una variedad de lenguajes de programación. Para obtener más información, consulte cómo [crear aplicaciones cliente para consumir servicios web](how-to-consume-web-service.md).

## <a id="update"></a> Actualización del servicio web

Cuando cree una nueva imagen, debe actualizar manualmente cada servicio que quiera que use la nueva imagen. Para actualizar el servicio web, utilice el método `update`. El código siguiente muestra cómo actualizar el servicio web para usar una nueva imagen:

```python
from azureml.core.webservice import Webservice
from azureml.core.image import Image

service_name = 'aci-mnist-3'
# Retrieve existing service
service = Webservice(name = service_name, workspace = ws)

# point to a different image
new_image = Image(workspace = ws, id="myimage2:1")

# Update the image used by the service
service.update(image = new_image)
print(service.state)
```

Para obtener más información, consulte la documentación de referencia de la clase [Webservice](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice(class)?view=azure-ml-py).

## <a name="clean-up"></a>Limpieza

Para eliminar un servicio web implementado, use `service.delete()`.

Para eliminar una imagen, utilice `image.delete()`.

Para eliminar un modelo registrado, use `model.delete()`.

Para obtener más información, consulte la documentación de referencia de los métodos [WebService.delete()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice(class)?view=azure-ml-py#delete--), [Image.delete()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.image.image(class)?view=azure-ml-py#delete--) y [Model.delete()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py#delete--).

## <a name="troubleshooting"></a>solución de problemas

* __Si hay errores durante la implementación__, use `service.get_logs()` para ver los registros de servicio. La información registrada puede indicar la causa del error.

* Los registros pueden contener un error que le insta a __establecer el nivel de registro en DEBUG__. Para establecer el nivel de registro, agregue las líneas siguientes al script de puntuación, cree la imagen y, a continuación, cree un servicio mediante la imagen:

    ```python
    import logging
    logging.basicConfig(level=logging.DEBUG)
    ```

    Este cambio habilita el registro adicional y puede devolver más información sobre por qué se está produciendo el error.

## <a name="next-steps"></a>Pasos siguientes

* [Protección de los servicios web de Azure Machine Learning con SSL](how-to-secure-web-service.md)
* [Consumir un modelo de ML que está implementado como un servicio web](how-to-consume-web-service.md)
* [Cómo ejecutar predicciones por lotes](how-to-run-batch-predictions.md)
* [Supervisión de los modelos de Azure Machine Learning con Application Insights](how-to-enable-app-insights.md)
* [Recopilar datos de modelos en producción](how-to-enable-data-collection.md)
* [SDK de Azure Machine Learning Service](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py)
* [Uso de Azure Machine Learning Service con Azure Virtual Network](how-to-enable-virtual-network.md)
* [Procedimientos recomendados para compilar sistemas de recomendaciones](https://github.com/Microsoft/Recommenders)
* [Compilación de una API de recomendaciones en tiempo real en Azure](https://docs.microsoft.com/azure/architecture/reference-architectures/ai/real-time-recommendation)
