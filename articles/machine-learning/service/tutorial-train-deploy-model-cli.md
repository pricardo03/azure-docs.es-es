---
title: Entrenamiento e implementación de modelos desde la CLI
titleSuffix: Azure Machine Learning
description: Aprenda a usar la extensión de Machine Learning para la CLI de Azure para entrenar, registrar e implementar un modelo desde la línea de comandos.
ms.author: larryfr
author: Blackmist
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.date: 09/12/2019
ms.openlocfilehash: 1854599956755716955a6e691c3266ac54ddafd9
ms.sourcegitcommit: f4d8f4e48c49bd3bc15ee7e5a77bee3164a5ae1b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2019
ms.locfileid: "73581547"
---
# <a name="tutorial-train-and-deploy-a-model-from-the-cli"></a>Tutorial: Entrenar e implementar un modelo desde la CLI
[!INCLUDE [applies-to-skus](../../../includes/aml-applies-to-basic-enterprise-sku.md)]

En este tutorial, usará la extensión de Machine Learning para la CLI de Azure para entrenar, registrar e implementar un modelo.

En los scripts de entrenar de Python de este tutorial, se usa [scikit-learn](https://scikit-learn.org/) para entrenar un modelo básico. Este tutorial no se centra en los scripts ni en el modelo, sino en el proceso de uso de la CLI para trabajar con Azure Machine Learning.

Aprenda a realizar las siguientes acciones:

> [!div class="checklist"]
> * Instalación de la extensión de Machine Learning
> * Creación de un área de trabajo de Azure Machine Learning
> * Creación del recurso de proceso usado para entrenar el modelo
> * Inicio de una ejecución de entrenamiento
> * Registro y descarga de un modelo
> * Implementación del modelo como servicio web
> * Puntuación de datos mediante el servicio web

## <a name="prerequisites"></a>Requisitos previos

* Una suscripción de Azure. Si no tiene una suscripción a Azure, cree una cuenta gratuita antes de empezar. Pruebe hoy mismo la [versión gratuita o de pago de Azure Machine Learning](https://aka.ms/AMLFree).

* Para usar los comandos de la CLI de este documento desde su **entorno local**, necesita la [CLI de Azure](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).

    Si usa el [Azure Cloud Shell](https://azure.microsoft.com//features/cloud-shell/), la CLI es accesible a través del explorador y reside en la nube.

## <a name="download-the-example-project"></a>Descarga del proyecto de ejemplo

Para este tutorial, descargue el proyecto [https://github.com/microsoft/MLOps](https://github.com/microsoft/MLOps). Los archivos de los directorios `model-training` y `model-deployment` se usan en los pasos de este tutorial.

Para una copia local de los archivos, [descargue un archivo ZIP](https://github.com/microsoft/MLOps/archive/master.zip) o clone el repositorio con el comando Git siguiente:

```azurecli-interactive
git clone https://github.com/microsoft/MLOps.git
```

### <a name="training-files"></a>Archivos de aprendizaje

El directorio `model-training` contiene los archivos siguientes, que se usan para entrenar un modelo:

* `.azureml\sklearn.runconfig`: un archivo de __configuración de ejecución__. Este archivo define el entorno en tiempo de ejecución necesario para entrenar el modelo.
* `train-sklearn.py`: el script de entrenamiento. Este archivo entrena el modelo.
* `mylib.py`: un módulo auxiliar, usado por `train-sklearn.py`.
* `training-env.yml`: define las dependencias de software que se necesitan para ejecutar el script de entrenamiento.

El script de entrenamiento usa el conjunto de datos Diabetes que se proporciona con scikit-learn para entrenar un modelo.

### <a name="deployment-files"></a>Archivos de implementación

El directorio `model-deployment` contiene los archivos siguientes, que se usan para implementar el modelo entrenado como servicio web:

* `aciDeploymentConfig.yml`: un archivo de __configuración de implementación__. Este archivo define el entorno de hospedaje necesario para el modelo.
* `inferenceConfig.yml`: un archivo de configuración de inferencia. Este archivo define el entorno de software que el servicio usa para puntuar los datos con el modelo.
* `score.py`: un script de Python que acepta datos entrantes, los puntúa con el modelo y, luego, devuelve una respuesta.
* `scoring-env.yml`: las dependencias de conda que se necesitan para ejecutar el modelo y el script `score.py`.

## <a name="connect-to-your-azure-subscription"></a>Conexión a su suscripción de Azure

Hay varias maneras de autenticarse en la suscripción a Azure desde la CLI. La manera más básica consiste en autenticarse interactivamente a través de un explorador. Para autenticarse de forma interactiva, abra una línea de comandos o un terminal y use el siguiente comando:

```azurecli-interactive
az login
```

Si la CLI puede abrir el explorador predeterminado, lo hará y cargará una página de inicio de sesión. De lo contrario, tendrá que abrir un explorador y seguir las instrucciones de la línea de comandos. Las instrucciones implican navegar a [https://aka.ms/devicelogin](https://aka.ms/devicelogin) y escribir un código de autorización.

## <a name="install-the-machine-learning-extension"></a>Instalación de la extensión de Machine Learning

Para instalar la extensión de Machine Learning, use el siguiente comando:

```azurecli-interactive
az extension add -n azure-cli-ml
```

Si recibe un mensaje que indica que la extensión ya está instalada, use el comando siguiente para actualizar a la versión más reciente:

```azurecli-interactive
az extension update -n azure-cli-ml
```

## <a name="create-a-resource-group"></a>Crear un grupo de recursos

Un grupo de recursos es un contenedor básico de recursos en la plataforma Azure. Al trabajar con Azure Machine Learning, el grupo de recursos contendrá el área de trabajo de Azure Machine Learning. También contendrá otros servicios de Azure que usa el área de trabajo. Por ejemplo, si entrena el modelo mediante un recurso de proceso basado en la nube, ese recurso se crea en el grupo de recursos.

Utilice el comando siguiente para __crear un nuevo grupo de recursos__. Reemplace `<resource-group-name>` por el nombre que va a usar para este grupo de recursos. Reemplace `<location>` por la región de Azure que va a usar para este grupo de recursos:

> [!TIP]
> Debe seleccionar una región en la que Azure Machine Learning esté disponible. Para obtener más información, consulte los [Productos disponibles por región](https://azure.microsoft.com/global-infrastructure/services/?products=machine-learning-service).

```azurecli-interactive
az group create --name <resource-group-name> --location <location>
```

La respuesta de este comando será similar al siguiente JSON:

```json
{
  "id": "/subscriptions/<subscription-GUID>/resourceGroups/<resourcegroupname>",
  "location": "<location>",
  "managedBy": null,
  "name": "<resource-group-name>",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "tags": null,
  "type": null
}
```

Para obtener más información sobre cómo trabajar con grupos de recursos, consulte [az group](https://docs.microsoft.com//cli/azure/group?view=azure-cli-latest).

## <a name="create-a-workspace"></a>Crear un área de trabajo

Para crear una nueva área de trabajo, use el comando siguiente. Reemplace `<workspace-name>` por el nombre que desea usar para esta área de trabajo. Reemplace `<resource-group-name>` por el nombre del grupo de recursos:

```azurecli-interactive
az ml workspace create -w <workspace-name> -g <resource-group-name>
```

La salida de este comando es similar al JSON siguiente:

```json
{
  "applicationInsights": "/subscriptions/<service-GUID>/resourcegroups/<resource-group-name>/providers/microsoft.insights/components/<application-insight-name>",
  "containerRegistry": "/subscriptions/<service-GUID>/resourcegroups/<resource-group-name>/providers/microsoft.containerregistry/registries/<acr-name>",
  "creationTime": "2019-08-30T20:24:19.6984254+00:00",
  "description": "",
  "friendlyName": "<workspace-name>",
  "id": "/subscriptions/<service-GUID>/resourceGroups/<resource-group-name>/providers/Microsoft.MachineLearningServices/workspaces/<workspace-name>",
  "identityPrincipalId": "<GUID>",
  "identityTenantId": "<GUID>",
  "identityType": "SystemAssigned",
  "keyVault": "/subscriptions/<service-GUID>/resourcegroups/<resource-group-name>/providers/microsoft.keyvault/vaults/<key-vault-name>",
  "location": "<location>",
  "name": "<workspace-name>",
  "resourceGroup": "<resource-group-name>",
  "storageAccount": "/subscriptions/<service-GUID>/resourcegroups/<resource-group-name>/providers/microsoft.storage/storageaccounts/<storage-account-name>",
  "type": "Microsoft.MachineLearningServices/workspaces",
  "workspaceid": "<GUID>"
}
```

## <a name="connect-local-project-to-workspace"></a>Conexión de un proyecto local a un área de trabajo

Desde un terminal o símbolo del sistema, use los comandos siguientes, cambie los directorios al directorio `mlops` y, a continuación, conéctese al área de trabajo:

```azurecli-interactive
cd ~/mlops
az ml folder attach -w <workspace-name> -g <resource-group-name>
```

La salida de este comando es similar al JSON siguiente:

```json
{
  "Experiment name": "model-training",
  "Project path": "/Code/MLOps/model-training",
  "Resource group": "<resource-group-name>",
  "Subscription id": "<subscription-id>",
  "Workspace name": "<workspace-name>"
}
```

Este comando crea un archivo `.azureml/config.json` que contiene la información necesaria para conectarse al área de trabajo. El resto de los comandos `az ml` que se usan en este tutorial usarán este archivo, por lo que no es necesario que agregue el área de trabajo y el grupo de recursos a todos los comandos.

## <a name="create-the-compute-target-for-training"></a>Creación del destino de proceso para entrenamiento

En este ejemplo se usa una máquina virtual de Azure Machine Learning Notebook para entrenar el modelo. Para crear una máquina virtual de Notebook, use el comando siguiente:

```azurecli-interactive
az ml computetarget create amlcompute -n cpu --max-nodes 4 --vm-size Standard_D2_V2
```

La salida de este comando es similar al JSON siguiente:

```json
{
  "location": "<location>",
  "name": "cpu",
  "provisioningErrors": null,
  "provisioningState": "Succeeded"
}
```

Este comando crea un destino de proceso denominado `cpu`, con un máximo de cuatro nodos. El tamaño de la máquina virtual seleccionado proporciona una máquina virtual con un recurso de GPU. Para información sobre el tamaño de la máquina virtual, consulte [Tamaños y tipos de máquinas virtuales].

> [!IMPORTANT]
> El nombre del destino de proceso (en este caso, `cpu`) es importante, porque el archivo `.azureml/sklearn.runconfig` que se usa en la sección siguiente hace referencia a él.

## <a name="submit-the-training-run"></a>Envío de la ejecución de entrenamiento

Para iniciar una ejecución de entrenamiento en el destino de proceso `cpu`, cambie los directorios al directorio `model-training` y, a continuación, use el comando siguiente:

```azurecli-interactive
cd ~/mlops/model-training
az ml run submit-script -e myexperiment -c sklearn -d training-env.yml -t runoutput.json train-sklearn.py
```

Este comando especifica un nombre para el experimento (`myexperiment`). El experimento almacena información sobre esta ejecución en el área de trabajo.

El parámetro `-c sklearn` especifica el archivo `.azureml/sklearn.runconfig`. Como se mencionó anteriormente, este archivo contiene información que se usa para configurar el entorno que la ejecución de entrenamiento usa. Si inspecciona este archivo, verá que hace referencia al destino de proceso `cpu` que creó anteriormente. También muestra el número de nodos que se usará cuando se realice el entrenamiento (`"nodeCount": "4"`) y contiene una sección `"condaDependenciees"` que muestra los paquetes de Python necesarios para ejecutar el script de entrenamiento.

Para más información sobre cómo ejecutar archivos de configuración, consulte [Configuración y uso de destinos de proceso para el entrenamiento del modelo](how-to-set-up-training-targets.md#create-run-configuration-and-submit-run-using-azure-machine-learning-cli) o este [archivo JSON](https://github.com/microsoft/MLOps/blob/b4bdcf8c369d188e83f40be8b748b49821f71cf2/infra-as-code/runconfigschema.json) para ver todo el esquema de un runconfig.

El parámetro `-t` almacena una referencia a esta ejecución en un archivo JSON y se usará en los pasos siguientes para registrar y descargar el modelo.

A medida que se procesa la ejecución del entrenamiento, transmite información desde la sesión de entrenamiento en el recurso de proceso remoto. Parte de la información es similar al texto siguiente:

```text
alpha is 0.80, and mse is 3340.02
alpha is 0.85, and mse is 3349.36
alpha is 0.90, and mse is 3359.09
alpha is 0.95, and mse is 3369.13


The experiment completed successfully. Finalizing run...
Cleaning up all outstanding Run operations, waiting 300.0 seconds
```

Este texto se registra desde el script de entrenamiento (`train-sklearn.py`) y muestra dos de las métricas de rendimiento de este modelo. En este caso, queremos el modelo con el valor alfa más alto. Las métricas de rendimiento son específicas para el modelo que se va a entrenar. Otros modelos tendrán distintas métricas de rendimiento.

Si inspecciona el `train-sklearn.py`, observará que también usa el valor alfa al almacenar los modelos entrenados en el archivo. En este caso, entrena varios modelos. El que tenga el valor alfa más alto debe ser el mejor. Al examinar la salida anterior y el código, el modelo con un alfa de 0,95 se guardó como `./outputs/ridge_0.95.pkl`.

El modelo se guardó en el directorio `./outputs` en el destino de proceso en el que se entrenó. En este caso, la máquina virtual de Azure Machine Learning Notebook en la nube de Azure. El proceso de entrenamiento carga automáticamente el contenido del directorio `./outputs` desde el destino de proceso en el que se realiza el entrenamiento al área de trabajo de Azure Machine Learning. Se almacena como parte del experimento (en este ejemplo, `myexperiment`).

## <a name="register-the-model"></a>Registro del modelo

Para registrar el modelo directamente desde la versión almacenada en el experimento, use el comando siguiente:

```azurecli-interactive
az ml model register -n mymodel -f runoutput.json --asset-path "outputs/ridge_0.95.pkl" -t registeredmodel.json
```

Este comando registra el archivo `outputs/ridge_0.95.pkl` creado por la ejecución de entrenamiento como un registro de modelo nuevo denominado `mymodel`. El `--assets-path` hace referencia a una ruta de acceso en un experimento. En este caso, el experimento y la información de ejecución se cargan desde el archivo `runoutput.json` creado por el comando de entrenamiento. El `-t registeredmodel.json` crea un archivo JSON que hace referencia al modelo registrado nuevo que creó este comando y lo usan otros comandos de la CLI que funcionan con modelos registrados.

La salida de este comando es similar al JSON siguiente:

```json
{
  "createdTime": "2019-09-19T15:25:32.411572+00:00",
  "description": "",
  "experimentName": "myexperiment",
  "framework": "Custom",
  "frameworkVersion": null,
  "id": "mymodel:1",
  "name": "mymodel",
  "properties": "",
  "runId": "myexperiment_1568906070_5874522d",
  "tags": "",
  "version": 1
}
```

### <a name="model-versioning"></a>Control de versiones de los modelos

Anote el número de versión que se devuelve para el modelo. La versión se incrementa cada vez que se registra un modelo nuevo con este nombre. Por ejemplo, puede descargar el modelo y registrarlo desde un archivo local con los comandos siguientes:

```azurecli-interactive
az ml model download -i "mymodel:1" -t .
az ml model register -n mymodel -p "ridge_0.95.pkl"
```

El primer comando descarga el modelo registrado en el directorio actual. El nombre de archivo es `ridge_0.95.pkl`, que es el archivo al que se hace referencia al registrar el modelo. El segundo comando registra el modelo local (`-p "ridge_0.95.pkl"`) con el mismo nombre que el registro anterior (`mymodel`). Esta vez, los datos JSON devueltos muestran la versión como 2.

## <a name="deploy-the-model"></a>Implementación del modelo

Para implementar un modelo, cambie los directorios al directorio `model-deployment` y use el comando siguiente:

```azurecli-interactive
cd ~/mlops/model-deployment
az ml model deploy -n myservice -m "mymodel:1" --ic inferenceConfig.yml --dc aciDeploymentConfig.yml
```

Es posible que reciba el mensaje "Error al crear el cliente Docker". Puede pasar por alto este mensaje. La CLI puede implementar un servicio web en un contenedor de Docker local y comprueba Docker. En este caso, no se usa una implementación local.

Este comando implementa un servicio nuevo denominado `myservice`, con la versión 1 del modelo que registró anteriormente.

El archivo `inferenceConfig.yml` proporciona información sobre cómo realizar la inferencia, como el script de entrada (`score.py`) y las dependencias de software. Para más información sobre la estructura de este archivo, consulte el [esquema de configuración de inferencia](reference-azure-machine-learning-cli.md#inference-configuration-schema). Para más información sobre los scripts de entrada, consulte [Implementación de modelos con Azure Machine Learning](how-to-deploy-and-where.md#prepare-to-deploy).

El `aciDeploymentConfig.yml` describe el entorno de implementación que se usa para hospedar el servicio. La configuración de implementación es específica del tipo de proceso que se usa para la implementación. En este caso, se usa una instancia de Azure Container Instances. Para más información, consulte el [esquema de configuración de implementación](reference-azure-machine-learning-cli.md#deployment-configuration-schema).

El proceso de implementación tardará varios minutos en completarse.

> [!TIP]
> En este ejemplo, se utiliza Azure Container Instances. Las implementaciones en ACI crean automáticamente el recurso ACI necesario. Si en su lugar va a implementar en Azure Kubernetes Service, debe crear un clúster de AKS de antemano y especificarlo como parte del comando `az ml model deploy`. Para ver un ejemplo de implementación en AKS, consulte [Implementación de un modelo en un clúster de Azure Kubernetes Service](how-to-deploy-azure-kubernetes-service.md).

Después de varios minutos, se devuelve información similar al JSON siguiente:

```json
ACI service creation operation finished, operation "Succeeded"
{
  "computeType": "ACI",
  {...ommitted for space...}
  "runtimeType": null,
  "scoringUri": "http://6c061467-4e44-4f05-9db5-9f9a22ef7a5d.eastus2.azurecontainer.io/score",
  "state": "Healthy",
  "tags": "",
  "updatedAt": "2019-09-19T18:22:32.227401+00:00"
}
```

### <a name="the-scoring-uri"></a>El URI de puntuación

El `scoringUri` devuelto de la implementación es el punto de conexión REST para un modelo implementado como un servicio web. También puede obtener este URI con el comando siguiente:

```azurecli-interactive
az ml service show -n myservice
```

Este comando devuelve el mismo documento JSON, incluido el `scoringUri`.

El punto de conexión REST se puede usar para enviar datos al servicio. Para información sobre cómo crear una aplicación cliente que envía datos al servicio, consulte [Consumir un modelo de Azure Machine Learning que está implementado como un servicio web](how-to-consume-web-service.md).

### <a name="send-data-to-the-service"></a>Envío de datos al servicio

Aunque puede crear una aplicación cliente para llamar al punto de conexión, la CLI de Machine Learning proporciona una utilidad que puede actuar como un cliente de prueba. Use el comando siguiente para enviar datos de prueba al servicio:

```azurecli-interactive
az ml service run -n myservice -d '{"data":[[1,2,3,4,5,6,7,8,9,10]]}'
```

La respuesta del comando es similar a `[4684.920839774082]`.

## <a name="clean-up-resources"></a>Limpieza de recursos

> [!IMPORTANT]
> Los recursos que creó pueden usarse como requisitos previos para otros tutoriales y artículos de procedimientos de Azure Machine Learning.

### <a name="delete-deployed-service"></a>Eliminación del servicio implementado

Si piensa seguir usando el área de trabajo de Azure Machine Learning, pero quiere deshacerse del servicio implementado para reducir los costos, use el comando siguiente:

```azurecli-interactive
az ml service delete -n myservice
```

Este comando devuelve un documento JSON que contiene el nombre del servicio eliminado. La eliminación del servicio puede tardar varios minutos.

### <a name="delete-the-training-compute"></a>Eliminación del proceso de entrenamiento

Si piensa seguir usando el área de trabajo de Azure Machine Learning, pero quiere deshacerse del destino de proceso `cpu` creado para el entrenamiento, use el comando siguiente:

```azurecli-interactive
az ml computetarget delete -n cpu
```

Este comando devuelve un documento JSON que contiene el identificador del destino de proceso eliminado. La eliminación del destino de proceso puede tardar varios minutos.

### <a name="delete-everything"></a>Eliminar todo el contenido

Si no va a usar los recursos creados, elimínelos para no incurrir en cargos adicionales.

Para eliminar el grupo de recursos y todos los recursos de Azure creados en este documento, use el comando siguiente. Sustituya `<resource-group-name>` por el nombre del grupo de recursos que creó:

```azurecli-interactive
az group delete -g <resource-group-name> -y
```

## <a name="next-steps"></a>Pasos siguientes

En este tutorial de Azure Machine Learning, usó la CLI de Machine Learning para las tareas siguientes:

> [!div class="checklist"]
> * Instalación de la extensión de Machine Learning
> * Creación de un área de trabajo de Azure Machine Learning
> * Creación del recurso de proceso usado para entrenar el modelo
> * Inicio de una ejecución de entrenamiento
> * Registro y descarga de un modelo
> * Implementación del modelo como servicio web
> * Puntuación de datos mediante el servicio web

Para más información sobre cómo usar la CLI, consulte [Uso de la extensión de la CLI para Azure Machine Learning](reference-azure-machine-learning-cli.md).
