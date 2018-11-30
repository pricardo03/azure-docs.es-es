---
title: 'Implementación de servicios web en Azure Container Instances (ACI): Azure Machine Learning'
description: Obtenga información sobre cómo implementar un modelo entrenado como un servicio web en Azure Container Instances (ACI) con el servicio Azure Machine Learning. En este artículo se muestran tres maneras de implementar un modelo en ACI. Se diferencian en el número de líneas de código y el control sobre la nomenclatura de los componentes de la implementación.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.author: raymondl
author: raymondlaghaeian
ms.reviewer: sgilley
ms.date: 09/24/2018
ms.openlocfilehash: 3c07f39a6c6c4ce244ba49a26617b3e645c57acb
ms.sourcegitcommit: a4e4e0236197544569a0a7e34c1c20d071774dd6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/15/2018
ms.locfileid: "51710382"
---
# <a name="deploy-web-services-to-azure-container-instances"></a>Implementación de servicios web en Azure Container Instances 

Puede implementar el modelo entrenado como un servicio web en [Azure Container Instances](https://azure.microsoft.com/services/container-instances/) (ACI), [Azure Kubernetes Service](https://azure.microsoft.com/services/kubernetes-service/) (AKS), dispositivos IoT Edge o [matrices de puertas programables por campos (FPGA)](concept-accelerate-with-fpgas.md). 

ACI es normalmente más económico que AKS y se puede configurar en 4-6 líneas de código. Se trata de la opción perfecta para probar implementaciones. Más adelante, cuando esté listo para usar los modelos y servicios web en entornos de producción a gran escala, puede [implementarlos en AKS](how-to-deploy-to-aks.md).

En este artículo se muestran tres maneras de implementar un modelo en ACI. Se diferencian en el número de líneas de código y el control sobre la nomenclatura de los componentes de la implementación. Desde el método que menos cantidad de código y control requiere hasta el método que más precisa, estas son las opciones que ofrece ACI:

* Implementación desde el archivo de modelo mediante `Webservice.deploy()` 
* Implementación desde el modelo registrado mediante `Webservice.deploy_from_model()`
* Implementación del modelo registrado desde una imagen mediante `Webservice.deploy_from_image()`

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://aka.ms/AMLfree) antes de empezar.


## <a name="prerequisites"></a>Requisitos previos

- Un área de trabajo del servicio Azure Machine Learning y el SDK de Azure Machine Learning para Python instalado. Obtenga información sobre cómo cumplir estos requisitos previos mediante la [guía de inicio rápido de introducción de Azure Machine Learning](quickstart-get-started.md).

- El objeto del área de trabajo del servicio Azure Machine Learning.

    ```python
    from azureml.core import Workspace
    ws = Workspace.from_config()
    ```

- Un modelo para implementarlo. Los ejemplos de este documento usan el modelo creado en el tutorial sobre cómo [entrenar un modelo](tutorial-train-models-with-aml.md). Si no usa este modelo, modifique los pasos para hacer referencia al nombre del modelo.  También deberá escribir su propio script de puntuación para ejecutar el modelo.


## <a name="configure-an-image"></a>Configuración de una API

Configure la imagen de Docker que se usa para almacenar todos los archivos del modelo.
1. Cree un script de puntuación (score.py) [siguiendo estas instrucciones](tutorial-deploy-models-with-aml.md#create-scoring-script).

    > [!IMPORTANT]
    > El script de puntuación recibe los datos enviados desde los clientes y los pasa al modelo de puntuación. Documente la estructura de datos que esperan el modelo y el script. Esta documentación simplifica el proceso de creación de un cliente para consumir el servicio web.

1. Cree un archivo de entorno (myenv.yml) [con estas instrucciones](tutorial-deploy-models-with-aml.md#create-environment-file).

1. Use estos dos archivos para configurar la imagen de Docker en Python con el SDK de la siguiente forma:

    ```python
    from azureml.core.image import ContainerImage

    image_config = ContainerImage.image_configuration(execution_script = "score.py",
                                                      runtime = "python",
                                                      conda_file = "myenv.yml",
                                                      description = "Image with mnist model",
                                                      tags = {"data": "mnist", "type": "classification"}
                                                     )
    ```

## <a name="configure-the-aci-container"></a>Configuración del contenedor de ACI

Configure el contenedor de ACI especificando el número de CPU y gigabytes de RAM necesarios para dicho contenedor de ACI. Un núcleo y 1 gigabyte de RAM son suficientes para muchos modelos. Si cree que necesitará más en otro momento, vuelva a crear la imagen e implemente el servicio de nuevo.  

```python
from azureml.core.webservice import AciWebservice

aciconfig = AciWebservice.deploy_configuration(cpu_cores = 1, 
                                               memory_gb = 1, 
                                               tags = {"data": "mnist", "type": "classification"},
                                               description = 'Handwriting recognition')
```

## <a name="register-a-model"></a>Registrar un modelo

> Omita este requisito previo si va a realizar la [implementación desde un archivo de modelo](#deploy-from-model-file) (`Webservice.deploy()`).

Registre un modelo que use [Webservice.deploy_from_model](#deploy-from-registered-model) o [Webservice.deploy_from_image](#deploy-from-image). O bien, si ya tiene un modelo registrado, recupérelo en este momento.

### <a name="retrieve-a-registered-model"></a>Recuperación de un modelo registrado
Si utiliza Azure Machine Learning para entrenar un modelo, es posible que este ya se haya registrado en el área de trabajo.  Por ejemplo, en el último paso del tutorial sobre cómo [entrenar un modelo](tutorial-train-models-with-aml.md) se registró el modelo.  A continuación, recupere el modelo registrado para implementarlo.

```python
from azureml.core.model import Model

model_name = "sklearn_mnist"
model=Model(ws, model_name)
```
  
### <a name="register-a-model-file"></a>Registro de un archivo de modelo

Si el modelo se creó en otra parte, puede registrarlo en el área de trabajo.  Para registrar un modelo, el archivo de modelo (`sklearn_mnist_model.pkl` en este ejemplo) debe estar en el directorio de trabajo actual. A continuación, registre ese archivo como un modelo denominado `sklearn_mnist` en el área de trabajo con `Model.register()`.
    
```python
from azureml.core.model import Model

model_name = "sklearn_mnist"
model = Model.register(model_path = "sklearn_mnist_model.pkl",
                        model_name = model_name,
                        tags = {"data": "mnist", "type": "classification"},
                        description = "Mnist handwriting recognition",
                        workspace = ws)
```

<a name='deploy-from-model-file'/>
## <a name="option-1-deploy-from-model-file"></a>Opción 1: Implementación desde el archivo de modelo

La opción de implementación desde un archivo de modelo es con la que menos código hay que escribir, pero también ofrece la menor cantidad de control sobre la nomenclatura de componentes. Esta opción se inicia con un archivo de modelo y se registra en el área de trabajo automáticamente.  Sin embargo, no se puede asignar un nombre al modelo ni asociar etiquetas o una descripción a él.  

Esta opción utiliza el método de SDK Webservice.deploy().  

**Tiempo estimado**: La implementación tarda aproximadamente 6-7 minutos.

1. Asegúrese de que el archivo de modelo está en el directorio de trabajo local.

1. Abra el archivo de modelo de requisitos previos, score.py, y cambie la sección `init()` a:

    ```python
    def init():
        global model
        # retreive the local path to the model using the model name
        model_path = Model.get_model_path('sklearn_mnist_model.pkl')
        model = joblib.load(model_path)
    ```

1. Implemente el archivo de modelo.

    ```python
    from azureml.core.webservice import Webservice
    
    service_name = 'aci-mnist-1'
    service = Webservice.deploy(deployment_config = aciconfig,
                                    image_config = image_config,
                                    model_paths = ['sklearn_mnist_model.pkl'],
                                    name = service_name,
                                    workspace = ws)
    
    service.wait_for_deployment(show_output = True)
    print(service.state)
    ```

1. Ya puede [probar el servicio web](#test-web-service).

<a name='deploy-from-registered-model'/>
## <a name="option-2-deploy-from-registered-model"></a>Opción 2: Implementación desde el modelo registrado

Con la opción de implementar un archivo de modelo hay que escribir unas pocas líneas de código más y permite cierto control sobre la nomenclatura de las salidas. Se trata de una manera cómoda de implementar un modelo registrado existente.  Sin embargo, no puede asignar un nombre a la imagen de Docker.  

Esta opción utiliza el método de SDK Webservice.deploy_from_model().

**Tiempo estimado**: La implementación tarda aproximadamente 8 minutos.

1. Ejecute el código para configurar el contenedor de Docker y el de ACI y especifique el modelo registrado.

    ```python
    from azureml.core.webservice import Webservice

    service_name = 'aci-mnist-2'
    service = Webservice.deploy_from_model(deployment_config = aciconfig,
                                           image_config = image_config,
                                           models = [model], # this is the registered model object
                                           name = service_name,
                                           workspace = ws)
    service.wait_for_deployment(show_output = True)
    print(service.state)
    ```

1. Ya puede [probar el servicio web](#test-web-service).

<a name='deploy-from-image'/>
## <a name="option-3-deploy-from-image"></a>Opción 3: Implementación desde una imagen

Implemente un modelo registrado (`model`) mediante `Webservice.deploy_from_image()`. Este método permite crear la imagen de Docker por separado y, a continuación, implementarla a partir de ella.

1. Compile y registre la imagen de Docker en el área de trabajo utilizando `ContainerImage.create()`.

    Este método ofrece un mayor control sobre la imagen si se crea en un paso independiente.  El modelo registrado (`model`) se incluye en la imagen.
    
    ```python
    from azureml.core.image import ContainerImage
    
    image = ContainerImage.create(name = "myimage1",
                                  models = [model], # this is the registered model object
                                  image_config = image_config,
                                  workspace = ws)
    
    image.wait_for_creation(show_output = True)
    ```
**Tiempo estimado**: Aproximadamente 3 minutos.

1. Implemente la imagen de Docker como un servicio mediante `Webservice.deploy_from_image()`.

    Ya puede implementar la imagen en ACI.  
    
    ```python
    from azureml.core.webservice import Webservice
    
    service_name = 'aci-mnist-3'
    service = Webservice.deploy_from_image(deployment_config = aciconfig,
                                                image = image,
                                                name = service_name,
                                                workspace = ws)
    service.wait_for_deployment(show_output = True)
    print(service.state)
    ```   
 
**Tiempo estimado**: Aproximadamente 3 minutos.

Este método proporciona el máximo control sobre la creación y nomenclatura de los componentes de la implementación.

Ya puede probar el servicio web.

## <a name="a-nametest-web-servicetest-the-web-service"></a><a name='test-web-service'/>Prueba del servicio web

El servicio web es el mismo, independientemente de qué método se haya utilizado.  Para obtener predicciones, use el método `run` del servicio.  

```python
# Load Data
import os
import urllib

os.makedirs('./data', exist_ok = True)

urllib.request.urlretrieve('http://yann.lecun.com/exdb/mnist/t10k-images-idx3-ubyte.gz', filename = './data/test-images.gz')

from utils import load_data
X_test = load_data('./data/test-images.gz', False) / 255.0

from sklearn import datasets
import numpy as np
import json

# find 5 random samples from test set
n = 5
sample_indices = np.random.permutation(X_test.shape[0])[0:n]

test_samples = json.dumps({"data": X_test[sample_indices].tolist()})
test_samples = bytes(test_samples, encoding = 'utf8')

# predict using the deployed model
prediction = service.run(input_data = test_samples)
print(prediction)
```


## <a name="clean-up-resources"></a>Limpieza de recursos

Si no va a usar este servicio web, elimínelo para que no se generen gastos.

```python
service.delete()
```

## <a name="next-steps"></a>Pasos siguientes

* Obtenga información en el artículo [Consume a ML Model deployed as a web service ](how-to-consume-web-service.md) (Consumir un modelo de ML implementado como servicio web).
* Obtenga información sobre la [implementación en Azure Kubernetes Service](how-to-deploy-to-aks.md) para realizar implementaciones de mayor escala. 
