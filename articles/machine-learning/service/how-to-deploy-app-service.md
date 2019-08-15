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
ms.openlocfilehash: 84de9d53b19f5aa9b73570aa0d115d204e8b6596
ms.sourcegitcommit: 670c38d85ef97bf236b45850fd4750e3b98c8899
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/08/2019
ms.locfileid: "68848216"
---
# <a name="deploy-a-machine-learning-model-to-azure-app-service-preview"></a>Implementación de un modelo de aprendizaje automático en Azure App Service (versión preliminar)

Aprenda a implementar un modelo de Azure Machine Learning Service como aplicación web en Azure App Service.

> [!IMPORTANT]
> Aunque tanto Azure Machine Learning Service como Azure App Service están disponibles con carácter general, la posibilidad de implementar un modelo desde Machine Learning Service en App Service está en versión preliminar.

Con Azure Machine Learning Service, puede crear una imagen de Docker a partir de modelos de aprendizaje automático entrenados. Esta imagen contiene un servicio web que recibe datos, los envía al modelo y, luego, devuelve la respuesta. Azure App Service se puede usar para implementar la imagen y proporciona las siguientes características:

* [Compatibilidad con SSL](/azure/app-service/app-service-web-ssl-cert-load) para comunicaciones seguras entre los clientes y el servicio.
* [Escalado horizontal](/azure/azure-monitor/platform/autoscale-get-started?toc=%2fazure%2fapp-service%2ftoc.json) a varias instancias sin tener que volver a implementar.
* [Autenticación avanzada](/azure/app-service/configure-authentication-provider-aad) para mejorar la seguridad.

Para más información sobre las características proporcionadas por Azure App Service, consulte la [introducción a App Service](/azure/app-service/overview).

## <a name="prerequisites"></a>Requisitos previos

* Un área de trabajo de Azure Machine Learning. Para más información, consulte el artículo [Crear un área de trabajo](how-to-manage-workspace.md).
* Un modelo de aprendizaje automático entrenado registrado en el área de trabajo. Si no tiene un modelo, use el [Tutorial: Entrenamiento de modelos de clasificación de imágenes](tutorial-train-models-with-aml.md) para entrenar y registrar uno.
* Una imagen de Docker creada a partir del modelo. Si no tiene una imagen, use el [Tutorial: Implementación de un modelo de clasificación de imágenes](tutorial-deploy-models-with-aml.md) para crear una.

## <a name="deploy-image-as-a-web-app"></a>Implementación de la imagen como una aplicación web

1. En [Azure Portal](https://portal.azure.com), seleccione el área de trabajo para Azure Machine Learning Service. En la sección __Información general__, use el vínculo __Registro__ para acceder a la instancia de Azure Container Registry del área de trabajo.

    ![Captura de pantalla de la información general del área de trabajo](media/how-to-deploy-app-service/workspace-overview.png)

2. En Azure Container Registry, seleccione __Repositorios__ y, luego, seleccione el __nombre de la imagen__ que quiere implementar. En la versión que quiera implementar, seleccione la entrada __...__ y, luego, __Implementar en la aplicación web__.

    ![Captura de pantalla de la implementación desde ACR en una aplicación web](media/how-to-deploy-app-service/deploy-to-web-app.png)

3. Para crear la aplicación web, proporcione un nombre de sitio, una suscripción, un grupo de recursos y seleccione la ubicación o el plan de App Service. Por último, seleccione __Crear__.

    ![Captura de pantalla del cuadro de diálogo Nueva aplicación web](media/how-to-deploy-app-service/web-app-for-containers.png)

## <a name="use-the-web-app"></a>Uso de la aplicación web

En [Azure Portal](https://portal.azure.com), seleccione la aplicación web creada en el paso anterior. En la sección __Información general__, copie la __dirección URL__. Este valor es la __dirección URL base__ del servicio.

![Captura de pantalla de la información general de la aplicación web](media/how-to-deploy-app-service/web-app-overview.png)

El servicio web que pasa las solicitudes al modelo se encuentra en `{baseurl}/score`. Por ejemplo, `https://mywebapp.azurewebsites.net/score`. El siguiente código de Python muestra cómo enviar datos a la dirección URL y mostrar la respuesta:

```python
import requests
import json

scoring_uri = "https://mywebapp.azurewebsites.net/score"

headers = {'Content-Type':'application/json'}

if service.auth_enabled:
    headers['Authorization'] = 'Bearer '+service.get_keys()[0]

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