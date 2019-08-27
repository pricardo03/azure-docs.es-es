---
title: Implementación de modelos de aprendizaje automático en Azure App Service (versión preliminar)
titleSuffix: Azure Machine Learning service
description: Aprenda a usar Azure Machine Learning Service para implementar un modelo en una aplicación web en Azure App Service.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: aashishb
author: aashishb
ms.reviewer: larryfr
ms.date: 07/01/2019
ms.openlocfilehash: ada2a19de12c2f3f6b23fcc3d759afb0c747d37d
ms.sourcegitcommit: d3dced0ff3ba8e78d003060d9dafb56763184d69
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/22/2019
ms.locfileid: "69897461"
---
# <a name="deploy-a-machine-learning-model-to-azure-app-service-preview"></a>Implementación de un modelo de aprendizaje automático en Azure App Service (versión preliminar)

Aprenda a implementar un modelo de Azure Machine Learning Service como aplicación web en Azure App Service.

> [!IMPORTANT]
> Aunque tanto Azure Machine Learning Service como Azure App Service están disponibles con carácter general, la posibilidad de implementar un modelo desde Machine Learning Service en App Service está en versión preliminar.

Con Azure Machine Learning Service, puede crear una imagen de Docker a partir de modelos de aprendizaje automático entrenados. Esta imagen contiene un servicio web que recibe datos, los envía al modelo y, luego, devuelve la respuesta. Azure App Service se puede usar para implementar la imagen y proporciona las siguientes características:

* [Autenticación](/azure/app-service/configure-authentication-provider-aad) avanzada para mejorar la seguridad. Los métodos de autenticación incluyen Azure Active Directory y la autenticación multifactor.
* [Escalado automático](/azure/azure-monitor/platform/autoscale-get-started?toc=%2fazure%2fapp-service%2ftoc.json) sin tener que volver a realizar la implementación.
* [Compatibilidad con SSL](/azure/app-service/app-service-web-ssl-cert-load) para comunicaciones seguras entre los clientes y el servicio.

Para más información sobre las características proporcionadas por Azure App Service, consulte la [introducción a App Service](/azure/app-service/overview).

> [!IMPORTANT]
> Si necesita poder registrar los datos de puntuación que se usan con el modelo implementado, o los resultados de la puntuación, debe implementar Azure Kubernetes Service en su lugar. Para obtener más información, consulte [Recopilar datos en los modelos de producción](how-to-enable-data-collection.md).

## <a name="prerequisites"></a>Requisitos previos

* Un área de trabajo de Azure Machine Learning. Para más información, consulte el artículo [Crear un área de trabajo](how-to-manage-workspace.md).
* Un modelo de aprendizaje automático entrenado registrado en el área de trabajo. Si no tiene un modelo, use el [Tutorial: Entrenamiento de modelos de clasificación de imágenes](tutorial-train-models-with-aml.md) para entrenar y registrar uno.

    > [!IMPORTANT]
    > En los fragmentos de código de este artículo se supone que se han establecido las siguientes variables:
    >
    > * `ws`: su área de trabajo de Azure Machine Learning.
    > * `model`: el modelo registrado que se implementará.
    > * `inference_config`: la configuración de inferencia del modelo.
    >
    > Para más información sobre la definición de estas variables, consulte [Implementación de modelos con el servicio Azure Machine Learning](how-to-deploy-and-where.md).

## <a name="prepare-for-deployment"></a>Preparación de la implementación

Antes de realizar la implementación, debe definir qué necesita para ejecutar el modelo como un servicio web. En la lista siguiente se describen los elementos básicos necesarios para una implementación:

* Un __script de entrada__. Este script acepta solicitudes, puntúa la solicitud mediante el modelo y devuelve los resultados.

    > [!IMPORTANT]
    > El script de entrada es específico del modelo; debe comprender el formato de los datos de la solicitud entrante, el formato de los datos que espera el modelo y el formato de los datos que se devuelven a los clientes.
    >
    > Si los datos de la solicitud están en un formato que el modelo no puede usar, el script puede transformarlos a un formato aceptable. También puede transformar la respuesta antes de devolverla al cliente.

    > [!IMPORTANT]
    > El SDK de Azure Machine Learning no proporciona una manera para que el servicio web acceda al almacén de datos o a los conjuntos de datos. Si necesita que el modelo implementado tenga acceso a los datos almacenados fuera de la implementación, como en una cuenta de Azure Storage, debe desarrollar una solución de código personalizada mediante el SDK pertinente. Por ejemplo, el [SDK de Azure Storage para Python](https://github.com/Azure/azure-storage-python).
    >
    > Otra alternativa que puede funcionar para su escenario son las [predicciones por lotes](how-to-run-batch-predictions.md), que proporcionan acceso a los almacenes de datos cuando se realiza la puntuación.

    Para obtener más información sobre los scripts de entrada, consulte [Implementación de modelos con el servicio Azure Machine Learning](how-to-deploy-and-where.md).

* **Dependencias**, como scripts de asistente o paquetes de Python/Conda, necesarias para ejecutar el modelo o el script de entrada.

Estas entidades se encapsulan en una __configuración de inferencia__. La configuración de inferencia hace referencia al script de entrada y a otras dependencias.

> [!IMPORTANT]
> Al crear una configuración de inferencia para su uso con Azure App Service, debe usar un objeto [Environment](https://docs.microsoft.com//python/api/azureml-core/azureml.core.environment%28class%29?view=azure-ml-py). En el ejemplo siguiente se muestra cómo crear un objeto de entorno y cómo usarlo con una configuración de inferencia:
>
> ```python
> from azureml.core import Environment
> from azureml.core.environment import CondaDependencies
>
> # Create an environment and add conda dependencies to it
> myenv = Environment(name="myenv")
> # Enable Docker based environment
> myenv.docker.enabled = True
> # Build conda dependencies
> myenv.python.conda_dependencies = CondaDependencies.create(conda_packages=['scikit-learn'])
> ```

Para obtener más información sobre los entornos, consulte el tema sobre la [creación y administración de entornos de entrenamiento e implementación](how-to-use-environments.md).

Para obtener más información sobre la configuración de inferencia, consulte [Implementación de modelos con el servicio Azure Machine Learning](how-to-deploy-and-where.md).

> [!IMPORTANT]
> Al realizar la implementación en Azure App Service, no es necesario crear una __configuración de implementación__.

## <a name="create-the-image"></a>Crear la imagen

Para crear la imagen de Docker que se implementa en Azure App Service, use [Model.package](https://docs.microsoft.com//python/api/azureml-core/azureml.core.model.model?view=azure-ml-py#package-workspace--models--inference-config--generate-dockerfile-false-). El siguiente fragmento de código muestra cómo compilar una nueva imagen a partir del modelo y la configuración de inferencia:

```python
package = Model.package(ws, [model], inference_config)
package.wait_for_creation(show_output=True)
```

Si `show_output=True`, se muestra la salida del proceso de compilación de Docker. Una vez finalizado el proceso, la imagen se ha creado en Azure Container Registry para su área de trabajo.

## <a name="deploy-image-as-a-web-app"></a>Implementación de la imagen como una aplicación web

1. En [Azure Portal](https://portal.azure.com), seleccione el área de trabajo para Azure Machine Learning Service. En la sección __Información general__, use el vínculo __Registro__ para acceder a la instancia de Azure Container Registry del área de trabajo.

    [![Captura de pantalla de la información general del área de trabajo](media/how-to-deploy-app-service/workspace-overview.png)](media/how-to-deploy-app-service/workspace-overview-expanded.png)

2. En Azure Container Registry, seleccione __Repositorios__ y, luego, seleccione el __nombre de la imagen__ que quiere implementar. En la versión que quiera implementar, seleccione la entrada __...__ y, luego, __Implementar en la aplicación web__.

    [![Captura de pantalla de la implementación desde ACR en una aplicación web](media/how-to-deploy-app-service/deploy-to-web-app.png)](media/how-to-deploy-app-service/deploy-to-web-app-expanded.png)

3. Para crear la aplicación web, proporcione un nombre de sitio, una suscripción, un grupo de recursos y seleccione la ubicación o el plan de App Service. Por último, seleccione __Crear__.

    ![Captura de pantalla del cuadro de diálogo Nueva aplicación web](media/how-to-deploy-app-service/web-app-for-containers.png)

## <a name="use-the-web-app"></a>Uso de la aplicación web

En [Azure Portal](https://portal.azure.com), seleccione la aplicación web creada en el paso anterior. En la sección __Información general__, copie la __dirección URL__. Este valor es la __dirección URL base__ del servicio.

[![Captura de pantalla de la información general de la aplicación web](media/how-to-deploy-app-service/web-app-overview.png)](media/how-to-deploy-app-service/web-app-overview-expanded.png)

El servicio web que pasa las solicitudes al modelo se encuentra en `{baseurl}/score`. Por ejemplo, `https://mywebapp.azurewebsites.net/score`. El siguiente código de Python muestra cómo enviar datos a la dirección URL y mostrar la respuesta:

```python
import requests
import json

scoring_uri = "https://mywebapp.azurewebsites.net/score"

headers = {'Content-Type':'application/json'}

print(headers)
    
test_sample = json.dumps({'data': [
    [1,2,3,4,5,6,7,8,9,10],
    [10,9,8,7,6,5,4,3,2,1]
]})

response = requests.post(scoring_uri, data=test_sample, headers=headers)
print(response.status_code)
print(response.elapsed)
print(response.json())
```

## <a name="next-steps"></a>Pasos siguientes

* Para más información sobre la configuración de la aplicación web, consulte la documentación de [App Service en Linux](/azure/app-service/containers/).
* Para más información sobre el escalado, consulte [Introducción al escalado automático en Azure](/azure/azure-monitor/platform/autoscale-get-started?toc=%2fazure%2fapp-service%2ftoc.json).
* Para más información sobre la compatibilidad con SSL, consulte [Uso de un certificado SSL en Azure App Service](/azure/app-service/app-service-web-ssl-cert-load).
* Para más información sobre la autenticación, consulte [Configuración de una aplicación de App Service para usar la información de inicio de sesión de Azure Active Directory](/azure/app-service/configure-authentication-provider-aad).
* [Consumir un modelo de ML que está implementado como un servicio web](how-to-consume-web-service.md)