---
author: larryfr
ms.service: machine-learning
ms.topic: include
ms.date: 07/19/2019
ms.author: larryfr
ms.openlocfilehash: 055909d51fcd1228e8eb26189ba682e09aee6a1a
ms.sourcegitcommit: 88ae4396fec7ea56011f896a7c7c79af867c90a1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/06/2019
ms.locfileid: "70390666"
---
Las entidades del documento `inferenceconfig.json` se asignan a los parámetros de la clase [InferenceConfig](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.inferenceconfig?view=azure-ml-py). En la tabla siguiente se describe la asignación entre las entidades del documento JSON y los parámetros del método:

| Entidad JSON | Parámetro del método | DESCRIPCIÓN |
| ----- | ----- | ----- |
| `entryScript` | `entry_script` | Ruta de acceso al archivo local que contiene el código que se ejecuta para la imagen. |
| `runtime` | `runtime` | el runtime que se usará para la imagen. Los runtimes admitidos actualmente son `spark-py` y `python`. |
| `condaFile` | `conda_file` | Opcional. Ruta de acceso a un archivo local que contiene una definición de entorno de Conda que se usará para la imagen. |
| `extraDockerFileSteps` | `extra_docker_file_steps` | Opcional. Ruta de acceso a un archivo local que contiene los pasos de Docker adicionales que se deben ejecutar al configurar la imagen. |
| `sourceDirectory` | `source_directory` | Opcional. Ruta de acceso a las carpetas que contienen todos los archivos para crear la imagen. |
| `enableGpu` | `enable_gpu` | Opcional. Determina si se habilita la compatibilidad con GPU en la imagen. La imagen GPU se debe usar en un servicio de Azure como Azure Container Instances, Proceso de Azure Machine Learning, Azure Virtual Machines y Azure Kubernetes Service. El valor predeterminado es False. |
| `baseImage` | `base_image` | Opcional. Una imagen personalizada que se usará como imagen base. Si no se proporciona ninguna imagen base, la imagen se basará en el parámetro runtime proporcionado. |
| `baseImageRegistry` | `base_image_registry` | Opcional. registro de imágenes que contiene la imagen base. |
| `cudaVersion` | `cuda_version` | Opcional. versión de CUDA que se debe instalar para las imágenes que requieren compatibilidad con GPU. La imagen GPU se debe usar en un servicio de Azure como Azure Container Instances, Proceso de Azure Machine Learning, Azure Virtual Machines y Azure Kubernetes Service. Las versiones compatibles son 9.0, 9.1 y 10.0. Si se establece `enable_gpu`, el valor predeterminado es 9.1. |
| `description` | `description` | Una descripción de la imagen. |

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