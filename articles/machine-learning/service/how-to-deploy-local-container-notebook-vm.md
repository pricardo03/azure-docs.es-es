---
title: Implementación de modelos en máquinas virtuales de Notebook
titleSuffix: Azure Machine Learning
description: Aprenda a implementar modelos de Azure Machine Learning como un servicio web con máquinas virtuales de Notebook.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: mnark
author: MrudulaN
ms.reviewer: larryfr
ms.date: 08/08/2019
ms.openlocfilehash: 046f998038c47a48a8528bf36d87ac836395eec2
ms.sourcegitcommit: e97a0b4ffcb529691942fc75e7de919bc02b06ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/15/2019
ms.locfileid: "71002822"
---
# <a name="deploy-a-model-to-notebook-vms"></a>Implementación de un modelo en máquinas virtuales de Notebook

Aprenda a usar Azure Machine Learning para implementar un modelo como un servicio web en su máquina virtual de Notebook. Use las máquinas virtuales de Notebook si se cumple una de las condiciones siguientes:

- Debe implementar y validar rápidamente el modelo.
- Está probando un modelo que está en desarrollo.

> [!TIP]
> La implementación de un modelo de Jupyter Notebook en una máquina virtual de Notebook, en un servicio web de la misma máquina virtual es una _implementación local_. En este caso, la máquina "local" es la máquina virtual de Notebook. Para más información sobre las implementaciones, consulte [Implementación de modelos con Azure Machine Learning](how-to-deploy-and-where.md).

## <a name="prerequisites"></a>Requisitos previos

- Un área de trabajo de Azure Machine Learning con una máquina virtual de Notebook en ejecución. Para más información, consulte [Configuración del entorno y del área de trabajo](tutorial-1st-experiment-sdk-setup.md).

## <a name="deploy-to-the-notebook-vms"></a>Implementación en máquinas virtuales de Notebook

En la máquina virtual de Notebook se incluye un cuaderno de ejemplo en el que se muestran las implementaciones locales. Siga estos pasos para cargar el cuaderno e implementar el modelo como un servicio web en la máquina virtual:

1. En [Azure Portal](https://portal.azure.com), seleccione las máquinas virtuales de Notebook para Azure Machine Learning.

1. Abra el subdirectorio `samples-*` y, después, abra `how-to-use-azureml/deploy-to-local/register-model-deploy-local.ipynb`. Una vez abiertos, ejecute el cuaderno.

    ![Captura de pantalla del servicio local en ejecución en el cuaderno](media/how-to-deploy-local-container-notebookvm/deploy-local-service.png)

1. El cuaderno muestra la dirección URL y el puerto en el que se está ejecutando el servicio. Por ejemplo, `https://localhost:6789`. También puede ejecutar la celda que contiene `print('Local service port: {}'.format(local_service.port))` para mostrar el puerto.

    ![Captura de pantalla del puerto del servicio local en ejecución](media/how-to-deploy-local-container-notebookvm/deploy-local-service-port.png)

1. Para probar el servicio desde la máquina virtual de Notebook use la dirección URL `https://localhost:<local_service.port>`. Para realizar una prueba desde un cliente remoto, obtenga la dirección URL pública del servicio en ejecución en la máquina virtual de Notebook. La dirección URL pública se puede determinar mediante la siguiente fórmula: `https://<notebookvm_name>-<local_service_port>.<azure_region_of_notebook>.notebooks.azureml.net/score`. Por ejemplo, `https://mynotebookvm-6789.eastus2.notebooks.azureml.net/score`.

## <a name="test-the-service"></a>Probar el servicio

Para enviar datos de ejemplo al servicio en ejecución, use el código siguiente. Reemplace el valor `service_url` por la dirección URL del paso anterior:

```python
import requests
import json
test_sample = json.dumps({'data': [
    [1,2,3,4,5,6,7,8,9,10],
    [10,9,8,7,6,5,4,3,2,1]
]})
test_sample = bytes(test_sample,encoding = 'utf8')
access_token = "your bearer token"
headers = {'Content-Type':'application/json', 'Authorization': 'Bearer ' + access_token}
service_url = "https://mynotebookvm-6789.eastus2.notebooks.azureml.net/score"
resp = requests.post(service_url, test_sample, headers=headers)
print("prediction:", resp.text)
```

## <a name="next-steps"></a>Pasos siguientes

* [Cómo implementar un modelo con una imagen personalizada de Docker](how-to-deploy-custom-docker-image.md)
* [Solución de problemas de implementación](how-to-troubleshoot-deployment.md)
* [Protección de los servicios web de Azure Machine Learning con SSL](how-to-secure-web-service.md)
* [Consumir un modelo de ML que está implementado como un servicio web](how-to-consume-web-service.md)
* [Supervisión de los modelos de Azure Machine Learning con Application Insights](how-to-enable-app-insights.md)
* [Recopilar datos de modelos en producción](how-to-enable-data-collection.md)