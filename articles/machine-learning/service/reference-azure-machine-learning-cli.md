---
title: Acerca de la extensión de la CLI de Azure Machine Learning
description: Obtenga información sobre la extensión de la CLI de aprendizaje automático para Azure Machine Learning.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: reference
ms.reviewer: jmartens
ms.author: jordane
author: jpe316
ms.date: 09/24/2018
ms.openlocfilehash: 5d14373b265ea30d235cc5bc7b87ee13c4fd8105
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46991800"
---
# <a name="what-is-the-azure-machine-learning-cli"></a>¿Qué es la CLI de Azure Machine Learning?

La extensión de la interfaz de la línea de comandos (CLI) de Azure Machine Learning es para científicos y desarrolladores de datos que trabajan con el servicio de Azure Machine Learning. Permite automatizar rápidamente los flujos de trabajo de aprendizaje automático y ponerlos en producción, como:
+ Realizar experimentos para crear modelos de aprendizaje automático

+ Registrar los modelos de aprendizaje automático para el uso del cliente

+ Empaquetar, implementar y realizar un seguimiento del ciclo de vida de los modelos de aprendizaje automático

Esta CLI de aprendizaje automático es una extensión de [CLI de Azure](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest) que se ha creado a partir del <a href="http://aka.ms/aml-sdk" target="_blank">SDK</a> basado en Python para el servicio de Azure Machine Learning.

> [!NOTE]
> La CLI está actualmente en versión preliminar y se actualizará.

## <a name="installing-and-uninstalling"></a>Instalación y desinstalación

Puede instalar la CLI mediante este comando desde nuestro índice Pypi en versión preliminar:
```AzureCLI
az extension add -s https://azuremlsdktestpypi.blob.core.windows.net/wheels/sdk-release/Preview/E7501C02541B433786111FE8E140CAA1/azure_cli_ml-0.1.50-py2.py3-none-any.whl --pip-extra-index-urls  https://azuremlsdktestpypi.azureedge.net/sdk-release/Preview/E7501C02541B433786111FE8E140CAA1
```

Puede quitar la CLI con este comando:
```AzureCLI
az extension remove -n azure-cli-ml
```

## <a name="using-the-cli-vs-the-sdk"></a>Uso de la CLI frente al SDK
La CLI es más adecuada para la automatización por parte de un rol de operaciones de desarrollo, o como parte de una canalización de integración y entrega continua. Se ha optimizado para controlar tareas poco frecuentes y muy parametrizadas. 

Algunos ejemplos son:
- proceso de aprovisionamiento
- realización de un experimento parametrizado
- registro de modelos, creación de imágenes
- implementación de servicios

Se recomienda a los científicos de datos que utilicen el SDK de Azure ML.

## <a name="common-machine-learning-cli-commands"></a>Comandos de la CLI de aprendizaje automático comunes

Utilice el amplio conjunto de comandos `az ml` para interactuar con el servicio en cualquier entorno de línea de comandos, incluido el shell de la nube de Azure Portal.

Este es un ejemplo de comandos comunes:

### <a name="workspace-creation--compute-setup"></a>Creación del área de trabajo y configuración del proceso

+ Cree un área de trabajo de Azure Machine Learning, el recurso de nivel superior para el aprendizaje automático.
  ```AzureCLI
  az ml workspace create -n myworkspace -g myresourcegroup
  ```

+ Configure la CLI para usar esta área de trabajo de forma predeterminada.
```AzureCLI
az configure --defaults aml_workspace=myworkspace group=myresourcegroup
```

+ Cree una instancia de DSVM (Data Science Virtual Machine) para los modelos de aprendizaje. También puede crear clústeres de BatchAI para el aprendizaje distribuido.
  ```AzureCLI
  az ml computetarget setup dsvm -n mydsvm
  ```

### <a name="experiment-submission"></a>Realización de experimentos
+ Adjunte un proyecto (configuración de ejecución) para realizar un experimento. Esto se usa para realizar un seguimiento de las ejecuciones del experimento.
  ```AzureCLI
  az ml project attach --experiment-name myhistory
  ```

+ Realizar un experimento con el servicio de Azure Machine Learning en el destino de proceso de su elección (en este ejemplo se usa una instancia de Data Science Virtual Machine)
  ```AzureCLI
  az ml run submit -c mydsvm train.py
  ```

+ Ver una lista de los experimentos realizados.
```AzureCLI
az ml history list
```

### <a name="model-registration-image-ceation--deployment"></a>Registro de modelos, creación e implementación de imágenes

+ Registre un modelo con Azure Machine Learning.
  ```AzureCLI
  az ml model register -n mymodel -m mymodel.pkl  -w myworkspace -g myresourcegroup
  ```

+ Cree una imagen que contenga el modelo y las dependencias de aprendizaje automático. 
  ```AzureCLI
  az ml image create -n myimage -r python -m rfmodel.pkl -f score.py -c myenv.yml
  ```

+ Implemente el modelo empaquetado en destinos como ACI y AKS.
  ```AzureCLI
  az ml service create aci -n myaciservice -i myimage:1
  ```
    
## <a name="full-command-list"></a>Lista de comandos completa
Puede encontrar la lista completa de comandos para la extensión de la CLI (y sus parámetros admitidos) mediante la ejecución de ```az ml COMMANDNAME -h```. 
