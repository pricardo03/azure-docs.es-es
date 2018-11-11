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
ms.openlocfilehash: 45ed1867d6d151250340bb21450b4b0d9b00e993
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/07/2018
ms.locfileid: "51243154"
---
# <a name="what-is-the-azure-machine-learning-cli"></a>¿Qué es la CLI de Azure Machine Learning?

La extensión de la interfaz de la línea de comandos (CLI) de Azure Machine Learning es para científicos y desarrolladores de datos que trabajan con el servicio de Azure Machine Learning. Permite automatizar rápidamente los flujos de trabajo de aprendizaje automático y ponerlos en producción, como:
+ Realizar experimentos para crear modelos de aprendizaje automático

+ Registrar los modelos de aprendizaje automático para el uso del cliente

+ Empaquetar, implementar y realizar un seguimiento del ciclo de vida de los modelos de aprendizaje automático

Esta CLI de aprendizaje automático es una extensión de [CLI de Azure](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest) que se ha creado a partir del <a href="https://aka.ms/aml-sdk" target="_blank">SDK</a> basado en Python para el servicio de Azure Machine Learning.

> [!NOTE]
> La CLI está actualmente en versión preliminar y se actualizará.

## <a name="installing-and-uninstalling"></a>Instalación y desinstalación

Puede instalar la CLI mediante este comando desde nuestro índice Pypi en versión preliminar:
```AzureCLI
az extension add -s https://azuremlsdktestpypi.blob.core.windows.net/wheels/sdk-release/Preview/E7501C02541B433786111FE8E140CAA1/azure_cli_ml-0.1.68-py2.py3-none-any.whl --pip-extra-index-urls  https://azuremlsdktestpypi.azureedge.net/sdk-release/Preview/E7501C02541B433786111FE8E140CAA1
```

Puede quitar la CLI con este comando:
```AzureCLI
az extension remove -n azure-cli-ml
```

Para actualizar la CLI, puede seguir los pasos anteriores para **quitar** y **agregar**.

## <a name="using-the-cli-vs-the-sdk"></a>Uso de la CLI frente al SDK
La CLI es más adecuada para la automatización por parte de un rol de operaciones de desarrollo, o como parte de una canalización de integración y entrega continua. Se ha optimizado para controlar tareas poco frecuentes y muy parametrizadas. 

Algunos ejemplos son:
- proceso de aprovisionamiento
- realización de un experimento parametrizado
- registro de modelos, creación de imágenes
- implementación de servicios

Se recomienda a los científicos de datos que utilicen el SDK de Azure ML.

## <a name="common-machine-learning-cli-commands"></a>Comandos de la CLI de aprendizaje automático comunes
> [!NOTE]
> Los archivos de muestra que puede usar para ejecutar correctamente los siguientes comandos pueden encontrarse [aquí.](https://github.com/Azure/MachineLearningNotebooks/tree/cli/cli)

Utilice el amplio conjunto de comandos `az ml` para interactuar con el servicio en cualquier entorno de línea de comandos, incluido el shell de la nube de Azure Portal.

Este es un ejemplo de comandos comunes:

### <a name="workspace-creation--compute-setup"></a>Creación del área de trabajo y configuración del proceso

+ Cree un área de trabajo de Azure Machine Learning Services, el recurso de nivel superior para el aprendizaje automático.
   ```AzureCLI
   az ml workspace create -n myworkspace -g myresourcegroup
   ```

+ Configure la CLI para usar esta área de trabajo de forma predeterminada.
   ```AzureCLI
   az configure --defaults aml_workspace=myworkspace group=myresourcegroup
   ```

+ Cree una máquina DSVM (máquina virtual de ciencia de datos). También puede crear clústeres de BatchAI para el aprendizaje distribuido o clústeres AKS para la implementación.
  ```AzureCLI
  az ml computetarget setup dsvm -n mydsvm
  ```

### <a name="experiment-submission"></a>Realización de experimentos
+ Adjunte un proyecto (configuración de ejecución) para realizar un experimento. Esto se usa para realizar un seguimiento de las ejecuciones del experimento.
  ```AzureCLI
  az ml project attach --experiment-name myhistory
  ```

+ Realice un experimento con el servicio Azure Machine Learning en el destino de proceso que prefiera. Este ejemplo se ejecutará en su entorno de proceso local. Asegúrese de que el archivo de entorno de conda captura las dependencias de python.

  ```AzureCLI
  az ml run submit -c local train.py
  ```

+ Ver una lista de los experimentos realizados.
```AzureCLI
az ml history list
```

### <a name="model-registration-image-ceation--deployment"></a>Registro de modelos, creación e implementación de imágenes

+ Registre un modelo con Azure Machine Learning.
  ```AzureCLI
  az ml model register -n mymodel -m sklearn_regression_model.pkl
  ```

+ Cree una imagen que contenga el modelo y las dependencias de aprendizaje automático. 
  ```AzureCLI
  az ml image create container -n myimage -r python -m mymodel:1 -f score.py -c myenv.yml
  ```

+ Implemente el modelo empaquetado en destinos como ACI y AKS.
  ```AzureCLI
  az ml service create aci -n myaciservice --image-id myimage:1
  ```
    
## <a name="full-command-list"></a>Lista de comandos completa
Puede encontrar la lista completa de comandos para la extensión de la CLI (y sus parámetros admitidos) mediante la ejecución de ```az ml COMMANDNAME -h```. 
