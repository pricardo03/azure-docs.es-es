---
title: Cómo implementar modelos en instancias de proceso
titleSuffix: Azure Machine Learning
description: Aprenda a implementar modelos de Azure Machine Learning como un servicio web con instancias de proceso.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: mnark
author: MrudulaN
ms.reviewer: larryfr
ms.date: 10/25/2019
ms.openlocfilehash: 0c80604bba78289c7612e3ca54a359de1a5e992d
ms.sourcegitcommit: ce4a99b493f8cf2d2fd4e29d9ba92f5f942a754c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/28/2019
ms.locfileid: "75535220"
---
# <a name="deploy-a-model-to-azure-machine-learning-compute-instances"></a>Implementación de un modelo en instancias de proceso de Azure Machine Learning

[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

> [!NOTE]
> Las instancias de proceso (versión preliminar) solo están disponibles para las áreas de trabajo con una región de **Centro y norte de EE. UU.** o **Sur de Reino Unido**.
>Si el área de trabajo se encuentra en otra región, puede seguir creando y usando una [máquina virtual de cuadernos](concept-compute-instance.md#notebookvm) en su lugar.  Puede implementar un modelo en una instancia de proceso o en una máquina virtual de cuadernos siguiendo los pasos descritos en este artículo.

Obtenga información sobre cómo usar Azure Machine Learning para implementar un modelo como un servicio web en la instancia de proceso de Azure Machine Learning. Use las instancias de proceso si se cumple una de las condiciones siguientes:

- Debe implementar y validar rápidamente el modelo.
- Está probando un modelo que está en desarrollo.

> [!TIP]
> La implementación de un modelo de Jupyter Notebook de una instancia de proceso, en un servicio web de la misma máquina virtual es una _implementación local_. En este caso, la máquina "local" es la instancia de proceso. Para más información sobre las implementaciones, consulte [Implementación de modelos con Azure Machine Learning](service/how-to-deploy-and-where.md).

## <a name="prerequisites"></a>Prerequisites

- Un área de trabajo de Azure Machine Learning con una instancia de proceso en ejecución. Para más información, consulte [Configuración del entorno y del área de trabajo](tutorial-1st-experiment-sdk-setup.md).

## <a name="deploy-to-the-compute-instances"></a>Implementación en instancias de proceso

En la instancia de proceso se incluye un cuaderno de ejemplo en el que se muestran las implementaciones locales. Siga estos pasos para cargar el cuaderno e implementar el modelo como un servicio web en la máquina virtual:

1. En [Azure Machine Learning Studio](https://ml.azure.com), seleccione las instancias de proceso de Azure Machine Learning.

1. Abra el subdirectorio `samples-*` y, después, abra `how-to-use-azureml/deploy-to-local/register-model-deploy-local.ipynb`. Una vez abiertos, ejecute el cuaderno.

    ![Captura de pantalla del servicio local en ejecución en el cuaderno](./media/how-to-deploy-local-container-notebook-vm/deploy-local-service.png)

1. El cuaderno muestra la dirección URL y el puerto en el que se está ejecutando el servicio. Por ejemplo, `https://localhost:6789`. También puede ejecutar la celda que contiene `print('Local service port: {}'.format(local_service.port))` para mostrar el puerto.

    ![Captura de pantalla del puerto del servicio local en ejecución](./media/how-to-deploy-local-container-notebook-vm/deploy-local-service-port.png)

1. Para probar el servicio desde una instancia de proceso, use la dirección URL de `https://localhost:<local_service.port>`. Para realizar una prueba desde un cliente remoto, obtenga la dirección URL pública del servicio que se ejecuta en la instancia de proceso. La dirección URL pública se puede determinar mediante la siguiente fórmula: 
    * Máquina virtual de cuadernos: `https://<vm_name>-<local_service_port>.<azure_region_of_workspace>.notebooks.azureml.net/score`. 
    * Instancia de proceso: `https://<vm_name>-<local_service_port>.<azure_region_of_workspace>.instances.azureml.net/score`. 

    Por ejemplo, 
    * Máquina virtual de cuadernos: `https://vm-name-6789.northcentralus.notebooks.azureml.net/score` 
    * Instancia de proceso: `https://vm-name-6789.northcentralus.instances.azureml.net/score`

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
service_url = "https://vm-name-6789.northcentralus.notebooks.azureml.net/score"
# for a compute instance, the url would be https://vm-name-6789.northcentralus.instances.azureml.net/score
resp = requests.post(service_url, test_sample, headers=headers)
print("prediction:", resp.text)
```

## <a name="next-steps"></a>Pasos siguientes

* [Cómo implementar un modelo con una imagen personalizada de Docker](service/how-to-deploy-custom-docker-image.md)
* [Solución de problemas de implementación](how-to-troubleshoot-deployment.md)
* [Protección de los servicios web de Azure Machine Learning con SSL](how-to-secure-web-service.md)
* [Consumir un modelo de ML que está implementado como un servicio web](how-to-consume-web-service.md)
* [Supervisión de los modelos de Azure Machine Learning con Application Insights](how-to-enable-app-insights.md)
* [Recopilar datos de modelos en producción](how-to-enable-data-collection.md)