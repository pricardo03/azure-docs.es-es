---
title: Uso de MLflow con Azure Machine Learning Service
titleSuffix: Azure Machine Learning service
description: Registre métricas y artefactos e implemente modelos en producción mediante MLflow con el servicio Azure Machine Learning.
services: machine-learning
author: rastala
ms.author: roastala
ms.service: machine-learning
ms.subservice: core
ms.reviewer: nibaccam
ms.topic: conceptual
ms.date: 07/15/2019
ms.custom: seodec18
ms.openlocfilehash: 2030365cd78480c25e224edfea9e395aafa6661c
ms.sourcegitcommit: 920ad23613a9504212aac2bfbd24a7c3de15d549
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/15/2019
ms.locfileid: "68227916"
---
# <a name="track-metrics-and-deploy-models-with-mlflow-and-azure-machine-learning-service-preview"></a>Seguimiento de métricas e implementación de modelos con MLflow y el servicio Azure Machine Learning (versión preliminar)

En este artículo se muestra cómo habilitar el URI de seguimiento de MLflow y la API de registro, que en conjunto se conocen como [Seguimiento de MLflow](https://mlflow.org/docs/latest/quickstart.html#using-the-tracking-api), con el servicio Azure Machine Learning Service. Esto le permite:

+ Realizar un seguimiento de las métricas y los artefactos del sus experimentos, así como registrarlos, en el [área de trabajo de Azure Machine Learning Service](https://docs.microsoft.com/azure/machine-learning/service/concept-azure-machine-learning-architecture#workspaces). Si ya usa el Seguimiento de MLflow para los experimentos, el área de trabajo proporciona una ubicación centralizada, segura y escalable para almacenar los modelos y las métricas de entrenamiento.

+ Implementar sus experimentos de MLflow como un servicio web Azure Machine Learning. Mediante la implementación como un servicio web, puede aplicar las funcionalidades de detección del desfase de datos y de supervisión de Azure Machine Learning en los modelos de producción. 

[MLflow](https://www.mlflow.org) es una biblioteca de código abierto para administrar el ciclo de vida de los experimentos de aprendizaje automático. El Seguimiento de MLFlow es un componente de MLflow que lleva a cabo un registro y un seguimiento de las métricas de ejecución de entrenamiento y los artefactos del modelo, independientemente del entorno de su experimento, ya sea local, en una máquina virtual, en un clúster de proceso remoto o incluso en Azure Databricks.

![Diagrama de MLflow con Azure Machine Learning](media/how-to-use-mlflow/mlflow-diagram-track.png)

## <a name="compare-mlflow-and-azure-machine-learning-clients"></a>Comparación entre los clientes de MLflow y Azure Machine Learning

 En la tabla siguiente se resumen los distintos clientes que pueden usar Azure Machine Learning Service y sus respectivas funcionalidades.

 El Seguimiento de MLflow ofrece funciones de registro de métricas y almacenamiento de artefactos que de otra manera solo están disponibles a través del [SDK de Python de Azure Machine Learning](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py).

| | Implementación y seguimiento de MLflow | Azure Machine Learning <br> SDK de Python |  Azure Machine Learning <br> CLI | Portal de Azure|
|-|-|-|-|-|-|
| Administración del área de trabajo |   | ✓ | ✓ | ✓ |
| Uso de almacenes de datos  |   | ✓ | ✓ | |
| Métricas de registro      | ✓ | ✓ |   | |
| Carga de artefactos | ✓ | ✓ |   | |
| Visualización de métricas     | ✓ | ✓ | ✓ | ✓ |
| Administración de procesos   |   | ✓ | ✓ | ✓ |
| Implementación de modelos    | ✓ | ✓ | ✓ | ✓ |
|Supervisión del rendimiento del modelo||✓|  |   |
| Detección del desfase de datos |   | ✓ |   | ✓ |

## <a name="prerequisites"></a>Requisitos previos

* [Instale MLflow](https://mlflow.org/docs/latest/quickstart.html).
* [Instale el SDK de Python de Azure Machine Learning en el equipo local y cree un área de trabajo de Azure Machine Learning](setup-create-workspace.md#sdk). El SDK proporciona conectividad para que MLflow acceda al área de trabajo.

## <a name="track-experiment-runs"></a>Seguimiento de las ejecuciones del experimento

El Seguimiento de MLflow con Azure Machine Learning Service le permite almacenar las métricas y los artefactos registrados desde las ejecuciones locales y remotas en el área de trabajo de Azure Machine Learning.

### <a name="local-runs"></a>Ejecuciones locales

Instale el paquete `azureml-contrib-run` que va a usar el Seguimiento de MLflow con Azure Machine Learning en los experimentos ejecutados localmente en un editor de código o Jupyter Notebook.

```shell
pip install azureml-contrib-run
```

>[!NOTE]
>El espacio de nombres azureml.contrib cambia con frecuencia, ya que estamos trabajando para mejorar el servicio. Por lo tanto, todo el contenido de este espacio de nombres debe considerarse como una versión preliminar, no completamente compatible con Microsoft.

Importe las clases `mlflow` y [`Workspace`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace(class)?view=azure-ml-py) para acceder al URI de seguimiento de MLflow y configurar el área de trabajo.

En el código siguiente, el método `get_mlflow_tracking_uri()` asigna un dirección URI de seguimiento única al área de trabajo (`ws`) y `set_tracking_uri()` apunta el URI de seguimiento de MLflow a esa dirección.

```Python
import mlflow
from azureml.core import Workspace

ws = Workspace.from_config()

mlflow.set_tracking_uri(ws.get_mlflow_tracking_uri())
```

>[!NOTE]
>El URI de seguimiento es válido hasta una hora como máximo. Si reinicia el script después de un tiempo de inactividad, use la API get_mlflow_tracking_uri para obtener un nuevo URI.

Establezca el nombre del experimento de MLflow con `set_experiment()` y comience la ejecución de entrenamiento con `start_run()`. Después, use `log_metric()` para activar la API de registro de MLflow y empezar a registrar las métricas de la ejecución de entrenamiento.

```Python
experiment_name = 'experiment_with_mlflow'
mlflow.set_experiment(experiment_name)

with mlflow.start_run():
    mlflow.log_metric('alpha', 0.03)
```

### <a name="remote-runs"></a>Ejecuciones remotas

Las ejecuciones remotas permiten entrenar modelos en procesos más eficaces, como máquinas virtuales habilitadas para GPU o clústeres de Proceso de Machine Learning. Consulte [Configuración de destinos de proceso del entrenamiento del modelo](how-to-set-up-training-targets.md) para obtener información sobre las distintas opciones de proceso.

Configure el proceso y el entorno de ejecución de entrenamiento con la clase [`Environment`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.environment?view=azure-ml-py). Incluya paquetes de PIP `mlflow` y `azure-contrib-run` en la sección [`CondaDependencies`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.conda_dependencies.condadependencies?view=azure-ml-py) del entorno. Después, construya [`ScriptRunConfig`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.script_run_config.scriptrunconfig?view=azure-ml-py) con el proceso remoto como destino de proceso.

```Python
from azureml.core import Environment
from azureml.core.conda_dependencies import CondaDependencies
from azureml.core import ScriptRunConfig

exp = Experiment(workspace = 'my_workspace',
                 name='my_experiment')

mlflow_env = Environment(name='mlflow-env')

cd = CondaDependencies.create(pip_packages=['mlflow', 'azureml-contrib-run'])

mlflow_env.python.conda_dependencies = cd

src = ScriptRunConfig(source_directory='./my_script_location', script='my_training_script.py')

src.run_config.target = 'my-remote-compute-compute'
src.run_config.environment = mlflow_env
```

En el script de entrenamiento, importe `mlflow` para usar las API de registro de MLflow y empiece a registrar las métricas de ejecución.

```Python
import mlflow

with mlflow.start_run():
    mlflow.log_metric('example', 1.23)
```

Con esta configuración del proceso y de la ejecución de entrenamiento, use el método `Experiment.submit('train.py')` para enviar una ejecución. Esto establece automáticamente el URI de seguimiento de MLflow y dirige el registro de MLflow al área de trabajo.

```Python
run = exp.submit(src)
```

### <a name="mlflow-with-azure-databricks-runs"></a>Mlflow con ejecuciones de Azure Databricks

Para ejecutar los experimentos de Mlflow con Azure Databricks, primero debe crear un [área de trabajo y un clúster de Azure Databricks](https://docs.microsoft.com/azure/azure-databricks/quickstart-create-databricks-workspace-portal). En su clúster, compruebe que instala la biblioteca *azureml-mlflow* desde PyPi para asegurarse de que el clúster tiene acceso a las funciones y clases necesarias.

#### <a name="install-libraries"></a>Instalar bibliotecas

Para instalar bibliotecas en el clúster, vaya a la pestaña **Bibliotecas** y haga clic en **Instalar nuevo**.

 ![Diagrama de MLflow con Azure Machine Learning](media/how-to-use-mlflow/azure-databricks-cluster-libraries.png)

En el campo **Paquete**, escriba azureml-mlflow y, a continuación, haga clic en instalar. Repita este paso según sea necesario para instalar otros paquetes adicionales en el clúster para el experimento.

 ![Diagrama de MLflow con Azure Machine Learning](media/how-to-use-mlflow/install-libraries.png)

#### <a name="notebook-and-workspace-set-up"></a>Configuración del cuaderno y del área de trabajo

Una vez haya configurado el clúster, importe el cuaderno del experimento, ábralo y asóciele el clúster.

El código siguiente debe estar en el cuaderno del experimento. De este modo, obtendrá los detalles de la suscripción a Azure para crear una instancia del área de trabajo. Se supone que tiene un grupo de recursos y un área de trabajo de Azure machine Learning. Si no es así, puede [crearlos](setup-create-workspace.md#portal). 

```python
import mlflow
import mlflow.azureml
import azureml.mlflow
import azureml.core

from azureml.core import Workspace
from azureml.mlflow import get_portal_url

subscription_id = 'subscription_id'

# Azure Machine Learning resource group NOT the managed resource group
resource_group = 'resource_group_name' 

#Azure Machine Learning workspace name, NOT Azure Databricks workspace
workspace_name = 'workspace_name'  

# Instantiate Azure Machine Learning workspace
ws = Workspace.get(name=workspace_name,
                   subscription_id=subscription_id,
                   resource_group=resource_group)

```
#### <a name="set-mlflow-tracking-uri"></a>Establecimiento del URI de seguimiento de MLflow
Una vez cree la instancia de su área de trabajo, establezca el URI de seguimiento de MLflow. Al hacerlo, vincula el seguimiento de MLflow al área de trabajo de Azure Machine Learning. Después de esto, todos los experimentos se redirigirán al servicio de seguimiento administrado de Azure Machine Learning.

```python
uri = ws.get_mlflow_tracking_uri()
mlflow.set_tracking_uri(uri)
```

En su script de entrenamiento, importe mlflow para usar las API de registro de MLflow y empiece a registrar las métricas de ejecución. En el ejemplo siguiente, se registra la métrica de pérdida de tiempo. 

```python
import mlflow 
mlflow.log_metric('epoch_loss', loss.item()) 
```

## <a name="view-metrics-and-artifacts-in-your-workspace"></a>Visualización de las métricas y los artefactos en el área de trabajo

Las métricas y los artefactos procedentes del registro de MLflow se conservan en el área de trabajo. Para verlos en cualquier momento, vaya al área de trabajo y busque el experimento por su nombre en [Azure Portal](https://portal.azure.com) o mediante la ejecución del código siguiente. 

```python
run.get_metrics()
ws.get_details()
```

## <a name="deploy-mlflow-models-as-a-web-service"></a>Implementación de modelos de MLflow como servicio web

La implementación de los experimentos de MLflow como servicio web de Azure Machine Learning le permite aprovechar las funcionalidades de detección del desfase de datos y de administración de modelos de Azure Machine Learning y aplicarlas a los modelos de producción.

![Diagrama de MLflow con Azure Machine Learning](media/how-to-use-mlflow/mlflow-diagram-deploy.png)

### <a name="log-your-model"></a>Registro del modelo
Antes de implementarlo, asegúrese de que el modelo está guardado a fin de poder hacer referencia a él y a la ubicación de su ruta de acceso en la implementación. En el script de entrenamiento, debe haber un código similar al siguiente método [mlflow.sklearn.log_model()](https://www.mlflow.org/docs/latest/python_api/mlflow.sklearn.html) , que guarda su modelo en el directorio de salidas especificado. 

```python
# change sklearn to pytorch, tensorflow, etc. based on your experiment's framework 
import mlflow.sklearn

# Save the model to the outputs directory for capture
mlflow.sklearn.log_model(regression_model, model_save_path)
```
>[!NOTE]
> Incluya el parámetro `conda_env` para pasar una representación del diccionario de las dependencias y el entorno en los que se debe ejecutar este modelo.

### <a name="retrieve-model-from-previous-run"></a>Recuperación del modelo de la ejecución anterior

Para recuperar la ejecución deseada, necesitamos el identificador de ejecución y la ruta de acceso del historial de ejecución de donde se guardó el modelo. 

```python
# gets the list of runs for your experiment as an array
experiment_name = 'experiment-with-mlflow'
exp = ws.experiments[experiment_name]
runs = list(exp.get_runs())

# get the run ID and the path in run history
runid = runs[0].id
model_save_path = 'model'
```

### <a name="create-docker-image"></a>Creación de una imagen de Docker

La función `mlflow.azureml.build_image()` genera una imagen de Docker a partir del modelo guardado de una manera compatible con el marco. Crea automáticamente el código de contenedor de inferencia específico del marco y especifica las dependencias del paquete. Especifique la ruta de acceso del modelo, el área de trabajo, el identificador de ejecución y otros parámetros.

En el código siguiente, compilamos una imagen de Docker mediante *runs:/<run.id>/model* como la ruta de acceso model_uri para un experimento de Scikit-learn.

```python
import mlflow.azureml

azure_image, azure_model = mlflow.azureml.build_image(model_uri='runs:/{}/{}'.format(runid, model_save_path),
                                                      workspace=ws,
                                                      model_name='sklearn-model',
                                                      image_name='sklearn-image',
                                                      synchronous=True)
```
La imagen de Docker puede tardar varios minutos en crearse. 

### <a name="deploy-the-docker-image"></a>Implementación de la imagen de Docker 

Una vez creada la imagen, use el SDK de Azure Machine Learning para implementar la imagen como un servicio web.

Primero, especifique la configuración de la implementación. Azure Container Instance (ACI) es una opción adecuada para una implementación rápida de desarrollo y pruebas, mientras que Azure Kubernetes Service (AKS) resulta apropiado para implementaciones de producción escalables.

#### <a name="deploy-to-aci"></a>Implementación en ACI

Establezca la configuración de implementación con el método [deploy_configuration()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.aciwebservice?view=azure-ml-py#deploy-configuration-cpu-cores-none--memory-gb-none--tags-none--properties-none--description-none--location-none--auth-enabled-none--ssl-enabled-none--enable-app-insights-none--ssl-cert-pem-file-none--ssl-key-pem-file-none--ssl-cname-none--dns-name-label-none-). También puede agregar etiquetas y descripciones para ayudar a realizar un seguimiento del servicio web.

```python
from azureml.core.webservice import AciWebservice, Webservice

# Configure 
aci_config = AciWebservice.deploy_configuration(cpu_cores=1, 
                                                memory_gb=1, 
                                                tags={'method' : 'sklearn'}, 
                                                description='Diabetes model',
                                                location='eastus2')
```

A continuación, implemente la imagen mediante el método [deploy_from_image()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.webservice(class)?view=azure-ml-py#deploy-from-image-workspace--name--image--deployment-config-none--deployment-target-none-) del SDK de Azure Machine Learning. 

```python
webservice = Webservice.deploy_from_image( image=azure_image, 
                                           workspace=ws, 
                                           name='diabetes-model-1', 
                                           deployment_config=aci_config)

webservice.wait_for_deployment(show_output=True)
```
#### <a name="deploy-to-aks"></a>Implementación en AKS

Para realizar la implementación en AKS, debe crear un clúster de AKS y usar la imagen de Docker que quiera implementar. En este ejemplo, se usa la imagen creada anteriormente en nuestra implementación de ACI.

Para obtener la imagen de la implementación de ACI anterior, usamos la clase [Image](https://docs.microsoft.com/python/api/azureml-core/azureml.core.image.image.image?view=azure-ml-py). 

```python
from azureml.core.image import Image

# Get the image by name, you can change this based on the image you want to deploy
myimage = Image(workspace=ws, name='sklearn-image') 
```

El proceso de AKS puede tardar entre 20 y25 minutos en crear un nuevo clúster.

```python
from azureml.core.compute import AksCompute, ComputeTarget

# Use the default configuration (can also provide parameters to customize)
prov_config = AksCompute.provisioning_configuration()

aks_name = 'aks-mlflow' 

# Create the cluster
aks_target = ComputeTarget.create(workspace=ws, 
                                  name=aks_name, 
                                  provisioning_configuration=prov_config)

aks_target.wait_for_completion(show_output = True)

print(aks_target.provisioning_state)
print(aks_target.provisioning_errors)
```
Establezca la configuración de implementación con el método [deploy_configuration()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.aciwebservice?view=azure-ml-py#deploy-configuration-cpu-cores-none--memory-gb-none--tags-none--properties-none--description-none--location-none--auth-enabled-none--ssl-enabled-none--enable-app-insights-none--ssl-cert-pem-file-none--ssl-key-pem-file-none--ssl-cname-none--dns-name-label-none-). También puede agregar etiquetas y descripciones para ayudar a realizar un seguimiento del servicio web.

```python
from azureml.core.webservice import Webservice, AksWebservice
from azureml.core.image import ContainerImage

# Set the web service configuration (using default here with app insights)
aks_config = AksWebservice.deploy_configuration(enable_app_insights=True)

# Unique service name
service_name ='aks-service'
```

A continuación, implemente la imagen mediante el método [deploy_from_image()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.webservice(class)?view=azure-ml-py#deploy-from-image-workspace--name--image--deployment-config-none--deployment-target-none-) del SDK de Azure Machine Learning. 

```python
# Webservice creation using single command
aks_service = Webservice.deploy_from_image( workspace=ws, 
                                            name=service_name,
                                            deployment_config = aks_config
                                            image = myimage,
                                            deployment_target = aks_target)

aks_service.wait_for_deployment(show_output=True)
```

La implementación del servicio puede tardar varios minutos.

## <a name="clean-up-resources"></a>Limpieza de recursos

Si no tiene pensado usar los artefactos o las métricas registradas en el área de trabajo, la funcionalidad para eliminarlos de forma individual no está disponible actualmente. Por ello, deberá eliminar el grupo de recursos que contiene la cuenta de almacenamiento y el área de trabajo para no incurrir en cargos:

1. En Azure Portal, seleccione **Grupos de recursos** a la izquierda del todo.

   ![Eliminación en Azure Portal](media/how-to-use-mlflow/delete-resources.png)

1. En la lista, seleccione el grupo de recursos que creó.

1. Seleccione **Eliminar grupo de recursos**.

1. Escriba el nombre del grupo de recursos. A continuación, seleccione **Eliminar**.


## <a name="example-notebooks"></a>Cuadernos de ejemplo

En [MLflow con cuadernos de Azure ML](https://aka.ms/azureml-mlflow-examples) se demuestran y se analizan con mayor profundidad los conceptos presentados en este artículo.

## <a name="next-steps"></a>Pasos siguientes
* [Administra sus modelos](concept-model-management-and-deployment.md).
* Supervise los modelos de producción para el [desfase de datos](how-to-monitor-data-drift.md).
