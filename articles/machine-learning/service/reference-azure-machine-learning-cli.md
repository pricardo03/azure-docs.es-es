---
title: Uso de la extensión de la CLI de Azure Machine Learning
description: Información acerca de la extensión de la CLI de Azure Machine Learning para la CLI de Azure. La CLI de Azure es una utilidad de la línea de comandos multiplataforma que le permite trabajar con recursos de la nube de Azure. La extensión de Machine Learning permite trabajar con el servicio Azure Machine Learning.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: jordane
author: jpe316
ms.date: 09/24/2018
ms.openlocfilehash: 13d09471191deed670db97a9f18e15bc9577dd1a
ms.sourcegitcommit: a4e4e0236197544569a0a7e34c1c20d071774dd6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/15/2018
ms.locfileid: "51713425"
---
# <a name="use-the-azure-machine-learning-cli-extension"></a>Uso de la extensión de la CLI de Azure Machine Learning

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

* La[CLI de Azure](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest).

> [!NOTE]
> Para usar la CLI, debe tener una suscripción de Azure. Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://aka.ms/AMLfree) antes de empezar.

## <a name="install-the-extension"></a>Instalación de la extensión

Para instalar la extensión de la CLI de Machine Learning, use el siguiente comando:

```azurecli-interactive
az extension add -s https://azuremlsdktestpypi.blob.core.windows.net/wheels/sdk-release/Preview/E7501C02541B433786111FE8E140CAA1/azure_cli_ml-0.1.68-py2.py3-none-any.whl --pip-extra-index-urls  https://azuremlsdktestpypi.azureedge.net/sdk-release/Preview/E7501C02541B433786111FE8E140CAA1
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

+ Cree una máquina DSVM (máquina virtual de ciencia de datos). También puede crear clústeres de BatchAI para el aprendizaje distribuido o clústeres AKS para la implementación.


  ```azurecli-interactive
  az ml computetarget setup dsvm -n mydsvm
  ```

## <a name="experiments"></a>Experimentos

Los siguientes comandos muestran cómo usar la CLI para trabajar con los experimentos:

* Adjunte un proyecto (configuración de ejecución) antes de enviar un experimento:

    ```azurecli-interactive
    az ml project attach --experiment-name myhistory
    ```

* Inicie la ejecución del experimento. Al usar este comando, especifique un destino de proceso. En este ejemplo, `local` utiliza la máquina local para entrenar el modelo mediante el script `train.py`:

    ```azurecli-interactive
    az ml run submit -c local train.py
    ```

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
