---
title: Extensión de la CLI de Machine Learning
titleSuffix: Azure Machine Learning service
description: Información acerca de la extensión de la CLI de Azure Machine Learning para la CLI de Azure. La CLI de Azure es una utilidad de la línea de comandos multiplataforma que le permite trabajar con recursos de la nube de Azure. La extensión de Machine Learning permite trabajar con el servicio Azure Machine Learning.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: jordane
author: jpe316
ms.date: 08/20/2019
ms.custom: seodec18
ms.openlocfilehash: 6c5068512f8ba26f7710bca7c0fccb98e0a5be33
ms.sourcegitcommit: 4b8a69b920ade815d095236c16175124a6a34996
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/23/2019
ms.locfileid: "69996746"
---
# <a name="use-the-cli-extension-for-azure-machine-learning-service"></a>Uso de la extensión de la CLI para Azure Machine Learning Service

La CLI de Azure Machine Learning es una extensión de la [CLI de Azure](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest), una interfaz de la línea de comandos multiplataforma para la plataforma Azure. Esta extensión proporciona comandos para trabajar con el servicio Azure Machine Learning. Permite automatizar las actividades de aprendizaje automático. En esta lista se muestran algunas acciones de ejemplo que puede realizar con la extensión de la CLI:

+ Realizar experimentos para crear modelos de aprendizaje automático

+ Registrar los modelos de aprendizaje automático para el uso del cliente

+ Empaquetar, implementar y realizar un seguimiento del ciclo de vida de los modelos de aprendizaje automático

La CLI no sustituye al SDK de Azure Machine Learning. Es una herramienta complementaria que está optimizada para administrar tareas con muchos parámetros y que están bien adaptadas para la automatización.

## <a name="prerequisites"></a>Requisitos previos

* Para usar la CLI, debe tener una suscripción de Azure. Si no tiene una suscripción a Azure, cree una cuenta gratuita antes de empezar. Pruebe hoy mismo la [versión gratuita o de pago de Azure Machine Learning Service](https://aka.ms/AMLFree).

* La[CLI de Azure](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest).

## <a name="full-reference-docs"></a>Documentos de referencia completos

Busque los [documentos de referencia completos sobre la extensión azure-cli-ml de la CLI de Azure](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/?view=azure-cli-latest).

## <a name="install-the-extension"></a>Instalación de la extensión

Para instalar la extensión de la CLI de Machine Learning, use el siguiente comando:

```azurecli-interactive
az extension add -n azure-cli-ml
```

> [!TIP]
> [Aquí](https://aka.ms/azml-deploy-cloud) puede encontrar algunos archivos de ejemplo que puede usar con los comandos siguientes.

Cuando se le solicite, seleccione `y` para instalar la extensión.

Para comprobar que se ha instalado la extensión, utilice el siguiente comando para mostrar una lista de los subcomandos específicos de ML:

```azurecli-interactive
az ml -h
```

## <a name="update-the-extension"></a>Actualización de la extensión

Para actualizar la extensión de la CLI de Machine Learning, use el siguiente comando:

```azurecli-interactive
az extension update -n azure-cli-ml
```


## <a name="remove-the-extension"></a>Eliminación de la extensión

Para eliminar la extensión de la CLI, use el siguiente comando:

```azurecli-interactive
az extension remove -n azure-cli-ml
```

## <a name="resource-management"></a>Administración de recursos

Los siguientes comandos muestran cómo utilizar la CLI para administrar los recursos que usa Azure Machine Learning.

+ Cree un grupo de recursos si todavía no tiene uno:

    ```azurecli-interactive
    az group create -n myresourcegroup -l westus2
    ```

+ Cree un área de trabajo de Azure Machine Learning:

    ```azurecli-interactive
    az ml workspace create -w myworkspace -g myresourcegroup
    ```

    Para más información, consulte [az ml workspace create](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/workspace?view=azure-cli-latest#ext-azure-cli-ml-az-ml-workspace-create).

+ Adjunte una configuración de área de trabajo a una carpeta para permitir el reconocimiento contextual de la CLI.

    ```azurecli-interactive
    az ml folder attach -w myworkspace -g myresourcegroup
    ```

    Este comando crea un subdirectorio `.azureml` que contiene archivos de entorno de conda y runconfig de ejemplo. También contiene un archivo `config.json` que se usa para comunicarse con el área de trabajo de Azure Machine Learning.

    Para más información, consulte [az ml folder attach](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/folder?view=azure-cli-latest#ext-azure-cli-ml-az-ml-folder-attach).

+ Adjunte un contenedor de blobs de Azure como almacén de datos.

    ```azurecli-interactive
    az ml datastore attach-blob  -n datastorename -a accountname -c containername
    ```

    Para más información, consulte [az ml datastore attach-blob](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/datastore?view=azure-cli-latest#ext-azure-cli-ml-az-ml-datastore-attach-blob).

+ Cargue archivos a un almacén de archivos.

    ```azurecli-interactive
    az ml datastore upload  -n datastorename -p sourcepath
    ```

    Para más información, consulte [az ml datastore upload](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/datastore?view=azure-cli-latest#ext-azure-cli-ml-az-ml-datastore-upload).

+ Adjunte un clúster de AKS como destino de proceso.

    ```azurecli-interactive
    az ml computetarget attach aks -n myaks -i myaksresourceid -g myresourcegroup -w myworkspace
    ```

    Para más información, consulte [az ml computetarget attach aks](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/computetarget/attach?view=azure-cli-latest#ext-azure-cli-ml-az-ml-computetarget-attach-aks)

+ Cree un nuevo destino AMLcompute.

    ```azurecli-interactive
    az ml computetarget create amlcompute -n cpu --min-nodes 1 --max-nodes 1 -s STANDARD_D3_V2
    ```

    Para más información, consulte [az ml computetarget create amlcompute](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/computetarget/create?view=azure-cli-latest#ext-azure-cli-ml-az-ml-computetarget-create-amlcompute).

## <a id="experiments"></a>Ejecución de los experimentos

* Inicie la ejecución del experimento. Cuando use este comando, especifique el nombre del archivo runconfig (el texto que va antes de \*.runconfig, si mira el sistema de archivo) con respecto al parámetro -c.

    ```azurecli-interactive
    az ml run submit-script -c sklearn -e testexperiment train.py
    ```

    > [!TIP]
    > El comando `az ml folder attach` crea un subdirectorio `.azureml`, que contiene dos archivos runconfig de ejemplo. 
    >
    > Si tiene un script de Python que crea un objeto de configuración de ejecución mediante programación, puede usar [RunConfig.save()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.runconfiguration?view=azure-ml-py#save-path-none--name-none--separate-environment-yaml-false-) para guardarlo como archivo runconfig.
    >
    > Para ver más archivos runconfig de ejemplo, consulte [https://github.com/MicrosoftDocs/pipelines-azureml/tree/master/.azureml](https://github.com/MicrosoftDocs/pipelines-azureml/tree/master/.azureml).

    Para más información, consulte [az ml run submit-script](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/run?view=azure-cli-latest#ext-azure-cli-ml-az-ml-run-submit-script).

* Vea una lista de los experimentos:

    ```azurecli-interactive
    az ml experiment list
    ```

    Para obtener más información, consulte [az ml experiment list](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/experiment?view=azure-cli-latest#ext-azure-cli-ml-az-ml-experiment-list).

## <a name="environment-management"></a>Administrador de entornos

Los comandos siguientes muestran cómo crear, registrar y enumerar los [entornos](how-to-configure-environment.md) de Azure Machine Learning Service para el área de trabajo:

+ Creación de archivos de scaffolding para un entorno:

    ```azurecli-interactive
    az ml environment scaffold -n myenv -d myenvdirectory
    ```

    Para más información, consulte [az ml environment scaffold](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/environment?view=azure-cli-latest#ext-azure-cli-ml-az-ml-environment-scaffold).

+ Registro de un entorno:

    ```azurecli-interactive
    az ml environment register -d myenvdirectory
    ```

    Para más información, consulte [az ml environment register](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/environment?view=azure-cli-latest#ext-azure-cli-ml-az-ml-environment-register).

+ Lista de entornos registrados:

    ```azurecli-interactive
    az ml environment list
    ```

    Para más información, consulte [az ml environment list](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/environment?view=azure-cli-latest#ext-azure-cli-ml-az-ml-environment-list).

+ Descargue un entorno registrado:

    ```azurecli-interactive
    az ml environment download -n myenv -d downloaddirectory
    ```

    Para más información, consulte [az ml environment download](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/environment?view=azure-cli-latest#ext-azure-cli-ml-az-ml-environment-download).

## <a name="model-registration-profiling-deployment"></a>Registro del modelo, generación de perfiles, implementación

Los siguientes comandos muestran cómo registrar un modelo entrenado e implementarlo como servicio de producción:

+ Registre un modelo con Azure Machine Learning:

    ```azurecli-interactive
    az ml model register -n mymodel -p sklearn_regression_model.pkl
    ```

    Para más información, consulte [az ml model register](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/model?view=azure-cli-latest#ext-azure-cli-ml-az-ml-model-register).

+ **OPCIONAL** Genere un perfil del modelo para obtener los valores óptimos de CPU y memoria para la implementación.
    ```azurecli-interactive
    az ml model profile -n myprofile -m mymodel:1 --ic inferenceconfig.json -d "{\"data\": [[1,2,3,4,5,6,7,8,9,10],[10,9,8,7,6,5,4,3,2,1]]}" -t myprofileresult.json
    ```

    Para más información, consulte [az ml model profile](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/model?view=azure-cli-latest#ext-azure-cli-ml-az-ml-model-profile).

+ Implementación del modelo en AKS
    ```azurecli-interactive
    az ml model deploy -n myservice -m mymodel:1 --ic inferenceconfig.json --dc deploymentconfig.json --ct akscomputetarget
    ```
    
    Para más información sobre el esquema del archivo de configuración de inferencia, consulte [Esquema de configuración de inferencia](#inferenceconfig).
    
    Para más información sobre el esquema del archivo de configuración de implementación, consulte [Esquema de configuración de implementación](#deploymentconfig).

    Para más información, consulte [az ml model deploy](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/model?view=azure-cli-latest#ext-azure-cli-ml-az-ml-model-deploy).

<a id="inferenceconfig"></a>

## <a name="inference-configuration-schema"></a>Esquema de configuración de inferencia

[!INCLUDE [inferenceconfig](../../../includes/machine-learning-service-inference-config.md)]

<a id="deploymentconfig"></a>

## <a name="deployment-configuration-schema"></a>Esquema de configuración de implementación

### <a name="local-deployment-configuration-schema"></a>Esquema de configuración de implementación local

[!INCLUDE [deploymentconfig](../../../includes/machine-learning-service-local-deploy-config.md)]

### <a name="azure-container-instance-deployment-configuration-schema"></a>Esquema de configuración de implementación de Azure Container Instances 

[!INCLUDE [deploymentconfig](../../../includes/machine-learning-service-aci-deploy-config.md)]

### <a name="azure-kubernetes-service-deployment-configuration-schema"></a>Esquema de configuración de implementación de Azure Kubernetes Service

[!INCLUDE [deploymentconfig](../../../includes/machine-learning-service-aks-deploy-config.md)]

## <a name="next-steps"></a>Pasos siguientes

* [Referencia de comandos para la extensión de la CLI de Machine Learning](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml?view=azure-cli-latest).

* [Entrenamiento e implementación de modelos de aprendizaje automático mediante Azure Pipelines](/azure/devops/pipelines/targets/azure-machine-learning)
