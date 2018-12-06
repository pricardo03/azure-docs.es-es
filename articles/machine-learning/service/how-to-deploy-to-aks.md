---
title: Implementación de modelos en Kubernetes desde el servicio Azure Machine Learning | Microsoft Docs
description: Obtenga información sobre cómo implementar un modelo desde el servicio Azure Machine Learning en Azure Kubernetes Service. El modelo se implementa como un servicio web. Azure Kubernetes Service se recomienda para cargas de trabajo de producción de gran escala.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.author: raymondl
author: raymondlaghaeian
manager: cgronlun
ms.reviewer: larryfr
ms.date: 09/24/2018
ms.openlocfilehash: d9328b293d38114d319d79e38b91b1b67e410d94
ms.sourcegitcommit: 56d20d444e814800407a955d318a58917e87fe94
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/29/2018
ms.locfileid: "52581846"
---
# <a name="how-to-deploy-models-from-azure-machine-learning-service-to-azure-kubernetes-service"></a>Cómo implementar modelos desde el servicio Azure Machine Learning en Azure Kubernetes Service

Puede implementar el modelo en Azure Kubernetes Service (AKS) para escenarios de producción de gran escala. Azure Machine Learning puede utilizar un clúster de AKS existente o uno nuevo que se creara durante la implementación. El modelo se implementa en ASK como un servicio web.

Implementar modelos en AKS proporciona al servicio web funcionalidades de escalado automático, registro, colección de datos de modelo y tiempos de respuesta breves.

## <a name="prerequisites"></a>Requisitos previos

- Una suscripción de Azure. Si no tiene una, cree una [cuenta gratuita](https://aka.ms/AMLfree) antes de empezar.

- Un área de trabajo del servicio de Azure Machine Learning, un directorio local que contenga los scripts y el SDK de Azure Machine Learning para Python instalado. Descubra cómo obtener estos requisitos previos con el documento [How to configure a development environment](how-to-configure-environment.md) (Cómo configurar un entorno de desarrollo).

- Un modelo de aprendizaje automático entrenado. Si no tiene uno, consulte el tutorial [Train an image classification model](tutorial-train-models-with-aml.md) (Entrenamiento un modelo de clasificación de imágenes).

## <a name="initialize-the-workspace"></a>Inicialización del área de trabajo

Para inicializar el área de trabajo, cargue el archivo `config.json` que contiene la información del área de trabajo.

```python
from azureml.coreazureml import Workspace

# Load existing workspace from the config file info.
ws = Workspace.from_config()
```

## <a name="register-the-model"></a>Registro del modelo

Para registrar un modelo existente, especifique la ruta de acceso, la descripción y las etiquetas del modelo.

```python
from azureml.core.model import Model

model = Model.register(model_path = "model.pkl", # this points to a local file
                        model_name = "best_model", # this is the name the model is registered as
                        tags = {"data": "diabetes", "type": "regression"},
                        description = "Ridge regression model to predict diabetes",
                        workspace = ws)
```

## <a name="create-a-docker-image"></a>Cree una imagen de Docker.

Azure Kubernetes Service usa imágenes de Docker. Para crear la imagen, siga estos pasos:

1. Para configurar la imagen, debe crear un script de puntuación y el archivo de entorno. Para obtener un ejemplo de creación del archivo de script y entorno, consulte las secciones siguientes del ejemplo de clasificación de imagen:

    * [Create a scoring script (score.py)](tutorial-deploy-models-with-aml.md#create-scoring-script) (Creación de un script de puntuación [score.py])

        > [!IMPORTANT]
        > El script de puntuación recibe los datos enviados desde los clientes y los pasa al modelo de puntuación. Documente la estructura de datos que esperan el modelo y el script. Esta documentación simplifica el proceso de creación de un cliente para consumir el servicio web.

    * [Create an environment file (myenv.yml)](tutorial-deploy-models-with-aml.md#create-environment-file) (Creación de un archivo de entorno [myenv.yml]) 

   El ejemplo siguiente usa estos archivos para configurar la imagen:

    ```python
    from azureml.core.image import ContainerImage

    # Image configuration
    image_config = ContainerImage.image_configuration(execution_script = "score.py",
                                                        runtime = "python",
                                                        conda_file = "myenv.yml",
                                                        description = "Image with ridge regression model",
                                                        tags = {"data": "diabetes", "type": "regression"}
                                                        )
    ```

1. Para crear la imagen, use la configuración de la imagen y el modelo. Esta operación puede tardar 5 minutos en completarse:

    ```python
    image = ContainerImage.create(name = "myimage1",
                                    # this is the model object
                                    models = [model],
                                    image_config = image_config,
                                    workspace = ws)

    # Wait for the create process to complete
    image.wait_for_creation(show_output = True)
    ```

## <a name="create-the-aks-cluster"></a>Creación del clúster de AKS

El fragmento de código siguiente muestra cómo crear el clúster de AKS. Este proceso tarda unos 20 minutos en completarse:

> [!IMPORTANT]
> Crear el clúster de AKS es un proceso único para el área de trabajo. Una vez creado, puede volver a usar este clúster para varias implementaciones. Si elimina el clúster o el grupo de recursos que lo contiene, tendrá que crear un clúster la próxima vez que necesite realizar una implementación.


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

### <a name="attach-existing-aks-cluster-optional"></a>Asociación de un clúster de AKS existente (opcional)

Si tiene un clúster de AKS existente en su suscripción de Azure, puede usarlo para implementar la imagen. El fragmento de código siguiente muestra cómo conectar un clúster al área de trabajo. 

> [!IMPORTANT]
> Solo se admite la versión de AKS 1.11.3.

```python
# Get the resource id from https://porta..azure.com -> Find your resource group -> click on the Kubernetes service -> Properties
resource_id = '/subscriptions/<your subscription id>/resourcegroups/<your resource group>/providers/Microsoft.ContainerService/managedClusters/<your aks service name>'

# Set to the name of the cluster
cluster_name='my-existing-aks' 

# Attatch the cluster to your workgroup
aks_target = AksCompute.attach(workspace=ws, name=cluster_name, resource_id=resource_id)

# Wait for the operation to complete
aks_target.wait_for_completion(True)
```

## <a name="deploy-your-web-service"></a>Implementación del servicio web

El fragmento de código siguiente muestra cómo implementar la imagen en el clúster de AKS:

```python
from azureml.core.webservice import Webservice, AksWebservice

# Set configuration and service name
aks_config = AksWebservice.deploy_configuration()
aks_service_name ='aks-service-1'
# Deploy from image
aks_service = Webservice.deploy_from_image(workspace = ws, 
                                            name = aks_service_name,
                                            image = image,
                                            deployment_config = aks_config,
                                            deployment_target = aks_target)
# Wait for the deployment to complete
aks_service.wait_for_deployment(show_output = True)
print(aks_service.state)
```

> [!TIP]
> Si hay errores durante la implementación, use `aks_service.get_logs()` para ver los registros de servicio de AKS. La información registrada puede indicar la causa del error.

## <a name="test-the-web-service"></a>Prueba del servicio web

Use `aks_service.run()` para probar el servicio web. El fragmento de código siguiente muestra cómo pasar datos al servicio y mostrar la predicción:

```python
import json

test_sample = json.dumps({'data': [
    [1,2,3,4,5,6,7,8,9,10], 
    [10,9,8,7,6,5,4,3,2,1]
]})
test_sample = bytes(test_sample,encoding = 'utf8')

prediction = aks_service.run(input_data = test_sample)
print(prediction)
```

## <a name="cleanup"></a>Limpieza

Para eliminar el servicio, la imagen y el modelo, use el siguiente fragmento de código:

```python
aks_service.delete()
image.delete()
model.delete()
```

## <a name="next-steps"></a>Pasos siguientes

Obtenga información en el artículo [Consume a ML Model deployed as a web service ](how-to-consume-web-service.md) (Consumir un modelo de ML implementado como servicio web).