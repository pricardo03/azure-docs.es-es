---
author: Blackmist
ms.service: machine-learning
ms.topic: include
ms.date: 01/28/2020
ms.author: larryfr
ms.openlocfilehash: 6970732f53ffa99849e20f279c8bdf4160c30a0a
ms.sourcegitcommit: 5d6ce6dceaf883dbafeb44517ff3df5cd153f929
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2020
ms.locfileid: "76845152"
---
Las entidades del documento `inferenceconfig.json` se asignan a los parámetros de la clase [InferenceConfig](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.inferenceconfig?view=azure-ml-py). En la tabla siguiente se describe la asignación entre las entidades del documento JSON y los parámetros del método:

| Entidad JSON | Parámetro del método | Descripción |
| ----- | ----- | ----- |
| `entryScript` | `entry_script` | Ruta de acceso al archivo local que contiene el código que se ejecuta para la imagen. |
| `runtime` | `runtime` | Opcional. el runtime que se usará para la imagen. Los entornos de ejecución admitidos son `spark-py` y `python`. Si se establece `environment`, se omite esta entrada. |
| `condaFile` | `conda_file` | Opcional. Ruta de acceso a un archivo local que contiene una definición de entorno de Conda que se usará para la imagen.  Si se establece `environment`, se omite esta entrada. |
| `extraDockerFileSteps` | `extra_docker_file_steps` | Opcional. Ruta de acceso a un archivo local que contiene los pasos de Docker adicionales que se deben ejecutar al configurar la imagen.  Si se establece `environment`, se omite esta entrada.|
| `sourceDirectory` | `source_directory` | Opcional. Ruta de acceso a carpetas que contienen todos los archivos para crear la imagen, lo que facilita el acceso a los archivos de esta carpeta o subcarpeta. Puede cargar una carpeta completa desde la máquina local como dependencias para el servicio web. Nota: Las rutas de acceso entry_script, conda_file y extra_docker_file_steps son rutas de acceso relativas a la ruta de acceso source_directory. |
| `enableGpu` | `enable_gpu` | Opcional. Determina si se habilita la compatibilidad con GPU en la imagen. La imagen de GPU debe usarse en un servicio de Azure, como Azure Container Instances. Por ejemplo, Azure Machine Learning Compute, Azure Virtual Machines y Azure Kubernetes Service. El valor predeterminado es False. Si se establece `environment`, se omite esta entrada.|
| `baseImage` | `base_image` | Opcional. Una imagen personalizada que se usará como imagen base. Si no se proporciona ninguna imagen base, la imagen se basará en el parámetro runtime proporcionado. Si se establece `environment`, se omite esta entrada. |
| `baseImageRegistry` | `base_image_registry` | Opcional. registro de imágenes que contiene la imagen base. Si se establece `environment`, se omite esta entrada.|
| `cudaVersion` | `cuda_version` | Opcional. versión de CUDA que se debe instalar para las imágenes que requieren compatibilidad con GPU. La imagen de GPU debe usarse en un servicio de Azure. como Azure Container Instances, Azure Machine Learning Compute, Azure Virtual Machines y Azure Kubernetes Service. Las versiones compatibles son 9.0, 9.1 y 10.0. Si se establece `enable_gpu`, el valor predeterminado es 9.1. Si se establece `environment`, se omite esta entrada. |
| `description` | `description` | Una descripción de la imagen. Si se establece `environment`, se omite esta entrada.  |
| `environment` | `environment` | Opcional.  [Entorno](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.environment?view=azure-ml-py) de Azure Machine Learning.|

El siguiente JSON es un ejemplo de la configuración de inferencia que se puede usar con la CLI:

```json
{
    "entryScript": "score.py",
    "runtime": "python",
    "condaFile": "myenv.yml",
    "extraDockerfileSteps": null,
    "sourceDirectory": null,
    "enableGpu": false,
    "baseImage": null,
    "baseImageRegistry": null
}
```

Puede incluir especificaciones completas de un [entorno](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.environment?view=azure-ml-py) de Azure Machine Learning en el archivo de configuración de inferencia. Si este entorno no existe en el área de trabajo, Azure Machine Learning lo creará. De lo contrario, Azure Machine Learning actualizará el entorno si es necesario. El siguiente código JSON es un ejemplo:

```json
{
    "entryScript": "score.py",
    "environment": {
        "docker": {
            "arguments": [],
            "baseDockerfile": null,
            "baseImage": "mcr.microsoft.com/azureml/base:intelmpi2018.3-ubuntu16.04",
            "enabled": false,
            "sharedVolumes": true,
            "shmSize": null
        },
        "environmentVariables": {
            "EXAMPLE_ENV_VAR": "EXAMPLE_VALUE"
        },
        "name": "my-deploy-env",
        "python": {
            "baseCondaEnvironment": null,
            "condaDependencies": {
                "channels": [
                    "conda-forge"
                ],
                "dependencies": [
                    "python=3.6.2",
                    {
                        "pip": [
                            "azureml-defaults",
                            "azureml-telemetry",
                            "scikit-learn",
                            "inference-schema[numpy-support]"
                        ]
                    }
                ],
                "name": "project_environment"
            },
            "condaDependenciesFile": null,
            "interpreterPath": "python",
            "userManagedDependencies": false
        },
        "version": "1"
    }
}
```

También puede usar un [entorno](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.environment?view=azure-ml-py) de Azure Machine Learning existente en parámetros de la CLI distintos y quitar la clave de "entorno" del archivo de configuración de inferencia. Use -e como nombre del entorno y --ev como versión del entorno. Si no especifica --ev, se usará la versión más reciente. Este es un ejemplo de un archivo de configuración de inferencia:

```json
{
    "entryScript": "score.py",
    "sourceDirectory": null
}
```

El siguiente comando muestra cómo implementar un modelo mediante el archivo de configuración de inferencia anterior (denominado myInferenceConfig.json). 

También usa la versión más reciente de un [entorno](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.environment?view=azure-ml-py) existente de Azure Machine Learning (llamado AzureML-Minimal).

```azurecli-interactive
az ml model deploy -m mymodel:1 --ic myInferenceConfig.json -e AzureML-Minimal --dc deploymentconfig.json
```
