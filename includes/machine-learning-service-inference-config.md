---
author: larryfr
ms.service: machine-learning
ms.topic: include
ms.date: 07/19/2019
ms.author: larryfr
ms.openlocfilehash: 03b40b1f334816f08d7e169f8ff78bdb7c06c4de
ms.sourcegitcommit: e72073911f7635cdae6b75066b0a88ce00b9053b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/19/2019
ms.locfileid: "68348495"
---
Las entidades del documento `inferenceconfig.json` se asignan a los parámetros de la clase [InferenceConfig](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.inferenceconfig?view=azure-ml-py). En la tabla siguiente se describe la asignación entre las entidades del documento JSON y los parámetros del método:

| Entidad JSON | Parámetro de método | DESCRIPCIÓN |
| ----- | ----- | ----- |
| `entryScript` | `entry_script` | ruta de acceso al archivo local que contiene el código que se ejecuta para la imagen. |
| `runtime` | `runtime` | el runtime que se usará para la imagen. Los runtimes admitidos actualmente son "spark-py" y "python". |
| `condaFile` | `conda_file` | Opcional. ruta de acceso al archivo local que contiene una definición de entorno de conda que se usará para la imagen. |
| `extraDockerFileSteps` | `extra_docker_file_steps` | Opcional. ruta de acceso al archivo local que contiene los pasos de Docker adicionales que se deben ejecutar al configurar la imagen. |
| `sourceDirectory` | `source_directory` | Opcional. ruta de acceso a las carpetas que contienen todos los archivos para crear la imagen. |
| `enableGpu` | `enable_gpu` | Opcional. Determina si se habilita la compatibilidad con GPU en la imagen. La imagen GPU debe usarse en servicios de Microsoft Azure, como Azure Container Instances, Azure Machine Learning Compute, Azure Virtual Machines y Azure Kubernetes Service. El valor predeterminado es False. |
| `baseImage` | `base_image` | Opcional. una imagen personalizada que se usará como imagen base. Si no se especifica ninguna imagen base, la imagen base se usará en función del parámetro de runtime proporcionado. |
| `baseImageRegistry` | `base_image_registry` | Opcional. registro de imágenes que contiene la imagen base. |
| `cudaVersion` | `cuda_version` | Opcional. versión de CUDA que se debe instalar para las imágenes que requieren compatibilidad con GPU. La imagen GPU debe usarse en servicios de Microsoft Azure, como Azure Container Instances, Azure Machine Learning Compute, Azure Virtual Machines y Azure Kubernetes Service. Las versiones compatibles son 9.0, 9.1 y 10.0. Si se establece "enable_gpu", el valor predeterminado es "9.1". |
| `description` | `description` |  Descripción de esta imagen. |

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