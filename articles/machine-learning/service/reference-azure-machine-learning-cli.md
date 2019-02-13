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
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: 65936348dcb40c6ceb71ebf735da8bb2120af654
ms.sourcegitcommit: a65b424bdfa019a42f36f1ce7eee9844e493f293
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/04/2019
ms.locfileid: "55694523"
---
# <a name="use-the-cli-extension-for-azure-machine-learning-service"></a>Uso de la extensión de la CLI para Azure Machine Learning Service

La CLI de Azure Machine Learning es una extensión de la [CLI de Azure](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest), una interfaz de la línea de comandos multiplataforma para la plataforma Azure. Esta extensión proporciona comandos para trabajar con el servicio Azure Machine Learning desde la línea de comandos. Permite crear los scripts que automatizan los flujos de trabajo de aprendizaje automático. Por ejemplo, puede crear scripts que realicen las siguientes acciones:

+ Realizar experimentos para crear modelos de aprendizaje automático

+ Registrar los modelos de aprendizaje automático para el uso del cliente

+ Empaquetar, implementar y realizar un seguimiento del ciclo de vida de los modelos de aprendizaje automático

La CLI no sustituye al SDK de Azure Machine Learning. Es una herramienta complementaria optimizada para controlar tareas con muchos parámetros, como las siguientes:

* Creación de recursos de proceso

* Envío de un experimento parametrizado

* Registro de modelos

* Creación de imágenes

* Implementación del servicio

## <a name="prerequisites"></a>Requisitos previos


* Para usar la CLI, debe tener una suscripción de Azure. Si no tiene una suscripción a Azure, cree una cuenta gratuita antes de empezar. Pruebe hoy mismo la [versión gratuita o de pago de Azure Machine Learning Service](http://aka.ms/AMLFree).

* La[CLI de Azure](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest).

## <a name="install-the-extension"></a>Instalación de la extensión

Para instalar la extensión de la CLI de Machine Learning, use el siguiente comando:

```azurecli-interactive
az extension add -s https://azuremlsdktestpypi.blob.core.windows.net/wheels/sdk-release/Preview/E7501C02541B433786111FE8E140CAA1/azure_cli_ml-1.0.10-py2.py3-none-any.whl --pip-extra-index-urls  https://azuremlsdktestpypi.azureedge.net/sdk-release/Preview/E7501C02541B433786111FE8E140CAA1
```

Cuando se le solicite, seleccione `y` para instalar la extensión.

Para comprobar que se ha instalado la extensión, utilice el siguiente comando para mostrar una lista de los subcomandos específicos de ML:

```azurecli-interactive
az ml -h
```

> [!TIP]
> Para actualizar la extensión debe __eliminarla__ y volver a __instalarla__. De este modo se instala la versión más reciente.

## <a name="remove-the-extension"></a>Eliminación de la extensión

Para eliminar la extensión de la CLI, use el siguiente comando:

```azurecli-interactive
az extension remove -n azure-cli-ml
```

## <a name="resource-management"></a>Administración de recursos

Los siguientes comandos muestran cómo utilizar la CLI para administrar los recursos que usa Azure Machine Learning.


+ Cree un área de trabajo de Azure Machine Learning:

    ```azurecli-interactive
    az ml workspace create -n myworkspace -g myresourcegroup
    ```

+ Defina un área de trabajo predeterminada:

    ```azurecli-interactive
    az configure --defaults aml_workspace=myworkspace group=myresourcegroup
    ```
    
* Asociación de un clúster de AKS

    ```azurecli-interactive
    az ml computetarget attach aks -n myaks -i myaksresourceid -g myrg -w myworkspace
    ```

## <a name="experiments"></a>Experimentos

Los siguientes comandos muestran cómo usar la CLI para trabajar con los experimentos:

* Adjunte un proyecto (configuración de ejecución) antes de enviar un experimento:

    ```azurecli-interactive
    az ml project attach --experiment-name myhistory
    ```

* Inicie la ejecución del experimento. Al usar este comando, especifique el nombre del archivo `.runconfig` que contiene la configuración de ejecución. El destino de proceso utiliza la configuración de ejecución para crear el entorno de entrenamiento para el modelo. En este ejemplo, la configuración de ejecución se carga desde el archivo `./aml_config/myrunconfig.runconfig`.

    ```azurecli-interactive
    az ml run submit -c myrunconfig train.py
    ```

    Al adjuntar un proyecto mediante el comando `az ml project attach`, se crean los archivos `.runconfig` predeterminados denominados `docker.runconfig` y `local.runconfig`. Es posible que deba modificarlos antes de usarlos para entrenar un modelo. 

    También puede crear una configuración de ejecución mediante programación con la clase [RunConfiguration](https://docs.microsoft.com/python/api/azureml-core/azureml.core.runconfig.runconfiguration?view=azure-ml-py). Una vez creada, puede usar el método `save()` para crear el archivo `.runconfig`.

* Consulte una lista de los experimentos enviados:

    ```azurecli-interactive
    az ml history list
    ```

## <a name="model-registration-image-creation--deployment"></a>Registro de modelos, creación e implementación de imágenes

Los siguientes comandos muestran cómo registrar un modelo entrenado e implementarlo como servicio de producción:

+ Registre un modelo con Azure Machine Learning:

  ```azurecli-interactive
  az ml model register -n mymodel -m sklearn_regression_model.pkl
  ```

+ Cree una imagen que contenga el modelo y las dependencias de aprendizaje automático: 

  ```azurecli-interactive
  az ml image create container -n myimage -r python -m mymodel:1 -f score.py -c myenv.yml
  ```

+ Implemente una imagen en un destino de proceso:

  ```azurecli-interactive
  az ml service create aci -n myaciservice --image-id myimage:1
  ```
