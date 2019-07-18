---
title: Cree una cuenta de Cognitive Services mediante la CLI de Azure
titlesuffix: Azure Cognitive Services
description: Cómo crear una cuenta de Azure Cognitive Services APIs mediante la CLI de Azure.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: conceptual
ms.date: 06/26/2019
ms.author: aahi
ms.openlocfilehash: 26f7f3ab60347d9ec5f2a144410ad3de436f5b5c
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/28/2019
ms.locfileid: "67454757"
---
# <a name="create-a-cognitive-services-account-using-the-azure-command-line-interfacecli"></a>Crear una cuenta de Cognitive Services mediante la interfaz de la línea de comandos de Azure (CLI)

En este artículo de inicio rápido, aprenderá a suscribirse a Azure Cognitive Services y a crear una cuenta con una suscripción de un solo servicio o de varios servicios mediante la [interfaz de la línea de comandos de Azure (CLI)](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest). Estos servicios se representan mediante los [recursos](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-portal) de Azure, que le permitirán conectarse a una o varias API de Azure Cognitive Services.

## <a name="prerequisites"></a>Requisitos previos

* Una suscripción válida a Azure. [Cree una cuenta](https://azure.microsoft.com/free/) gratis.
* [Interfaz de la línea de comandos (CLI) de Azure](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)

[!INCLUDE [cognitive-services-subscription-types](../../includes/cognitive-services-subscription-types.md)]

## <a name="install-the-azure-cli-and-sign-in"></a>Instale la CLI de Azure e inicie sesión. 

Instale la [CLI de Azure](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest). Para iniciar sesión en la instalación local de la CLI, ejecute el comando [az login](https://docs.microsoft.com/cli/azure/reference-index#az-login):

```console
az login
```

También puede usar el botón verde **Probar** para ejecutar estos comandos en su navegador.
 
## <a name="create-a-new-azure-cognitive-services-resource-group"></a>Creación de un nuevo grupo de recursos de Azure Cognitive Services

Sus suscripciones a Cognitive Services se representan por medio de recursos de Azure. Cada cuenta de Cognitive Services (y sus recursos de Azure asociados) deben pertenecer a un grupo de recursos de Azure.

### <a name="choose-your-resource-group-location"></a>Elija la ubicación de su grupo de recursos

Para crear un recurso, necesitará una de las ubicaciones de Azure disponibles para su suscripción. Puede recuperar una lista de ubicaciones disponibles con el comando [az account list-locations](/cli/azure/account#az_account_list). Se puede acceder a la mayoría de las instancias de Cognitive Services desde varios lugares. Elija el más cercano a usted o consulte las ubicaciones que están disponibles para el servicio.

> [!IMPORTANT]
> * No olvide su ubicación de Azure, ya que la necesitará cuando llame a Azure Cognitive Services.
> * La disponibilidad de algunas instancias de Cognitive Services puede variar según la región. Para obtener más información, consulte [Productos de Azure disponibles por región](https://azure.microsoft.com/global-infrastructure/services/?products=cognitive-services).  

```azurecli-interactive
az account list-locations \
    --query "[].{Region:name}" \
    --out table
```

Una vez que tenga su ubicación de Azure, cree un nuevo grupo de recursos en la CLI de Azure mediante el comando [az group create](/cli/azure/group#az_group_create).

En el ejemplo que tiene a continuación, reemplace la ubicación de Azure `westus2` con una de las ubicaciones de Azure disponibles para su suscripción.

```azurecli-interactive
az group create \
    --name cognitive-services-resource-group \
    --location westus2
```

## <a name="create-a-cognitive-services-resource"></a>Creación de un recurso de Cognitive Services

### <a name="choose-a-cognitive-service-and-pricing-tier"></a>Elegir una instancia de Cognitive Services y el plan de tarifa

Al crear un nuevo recurso, deberá conocer el "tipo" de servicio que quiere usar, junto con el [plan de tarifa](https://azure.microsoft.com/pricing/details/cognitive-services/) (o sku) que quiere. Usará esta y otra información como parámetros al crear el recurso.

> [!NOTE]
> Muchas instancias de Cognitive Services tienen un nivel gratuito que puede usar para probar el servicio. Para usar el nivel gratuito, use `F0` como sku del recurso.

### <a name="vision"></a>Visión

| Servicio                    | Clase                      |
|----------------------------|---------------------------|
| Computer Vision            | `ComputerVision`          |
| Custom Vision: Prediction | `CustomVision.Prediction` |
| Custom Vision: Training   | `CustomVision.Training`   |
| Face API                   | `Face`                    |
| Form Recognizer            | `FormRecognizer`          |
| Ink Recognizer             | `InkRecognizer`           |

### <a name="search"></a>Search

| Servicio            | Clase                  |
|--------------------|-----------------------|
| Bing Autosuggest   | `Bing.Autosuggest.v7` |
| Bing Custom Search | `Bing.CustomSearch`   |
| Bing Entity Search | `Bing.EntitySearch`   |
| Bing Search        | `Bing.Search.v7`      |
| Bing Spell Check   | `Bing.SpellCheck.v7`  |

### <a name="speech"></a>Voz

| Servicio            | Clase                 |
|--------------------|----------------------|
| Speech Services    | `SpeechServices`     |
| Reconocimiento de voz | `SpeakerRecognition` |

### <a name="language"></a>Idioma

| Servicio            | Clase                |
|--------------------|---------------------|
| Descripción del formulario | `FormUnderstanding` |
| LUIS               | `LUIS`              |
| QnA Maker          | `QnAMaker`          |
| Text Analytics     | `TextAnalytics`     |
| Traducción del texto   | `TextTranslation`   |

### <a name="decision"></a>Decisión

| Servicio           | Clase               |
|-------------------|--------------------|
| Anomaly Detector  | `AnomalyDetector`  |
| Content Moderator | `ContentModerator` |
| Personalizer      | `Personalizer`     |

Puede encontrar una lista de "tipos" de Cognitive Services disponibles con el comando [az cognitiveservices list-types](https://docs.microsoft.com/cli/azure/cognitiveservices/account?view=azure-cli-latest#az-cognitiveservices-account-list-kinds):

```azurecli-interactive
az cognitiveservices account list-kinds
```

### <a name="add-a-new-resource-to-your-resource-group"></a>Agregar un nuevo recurso al grupo de recursos

Para crear y suscribirse a un nuevo recurso de Cognitive Services, use el comando [az cognitiveservices account create](https://docs.microsoft.com/cli/azure/cognitiveservices/account?view=azure-cli-latest#az-cognitiveservices-account-create). Este comando agrega un nuevo recurso facturable al grupo de recursos creado anteriormente. Al crear este nuevo recurso, deberá conocer el "tipo" de servicio que quiere usar, junto con su plan de tarifa (o sku) y una ubicación de Azure:

Puede crear un recurso F0 (gratuito) para Anomaly Detector, denominado `anomaly-detector-resource` con el siguiente comando.

```azurecli-interactive
az cognitiveservices account create \
    --name anomaly-detector-resource \
    --group cognitive-services-resource-group \
    --kind AnomalyDetector \
    --sku F0 \
    --location westus2 \
    --yes
```

## <a name="get-the-keys-for-your-subscription"></a>Obtención de las claves para su suscripción

Para iniciar sesión en su instalación local de la interfaz de línea de comandos (CLI), use el comando [az login](https://docs.microsoft.com/cli/azure/reference-index?view=azure-cli-latest#az-login).

```console
az login
```

Use el comando [az cognitiveservices account keys list](https://docs.microsoft.com/cli/azure/cognitiveservices/account/keys?view=azure-cli-latest#az-cognitiveservices-account-keys-list) para obtener las claves de su recurso de Cognitive Services.

```azurecli-interactive
    az cognitiveservices account keys list \
    --name anomaly-detector-resource \
    --resource-group cognitive-services-resource-group
```

[!INCLUDE [cognitive-services-environment-variables](../../includes/cognitive-services-environment-variables.md)]

## <a name="clean-up-resources"></a>Limpieza de recursos

Si quiere limpiar y eliminar una suscripción a Cognitive Services, puede eliminar el recurso o grupo de recursos. Al eliminar el grupo de recursos, también se elimina cualquier otro recurso que esté asociado a dicho grupo.

Para quitar el grupo de recursos y sus recursos asociados, incluida la nueva cuenta de almacenamiento, use el comando az group delete.

```azurecli-interactive
az group delete --name storage-resource-group
```

## <a name="see-also"></a>Otras referencias

* [Autenticación de solicitudes en Azure Cognitive Services](authentication.md)
* [¿Qué es Azure Cognitive Services?](Welcome.md)
* [Compatibilidad con idiomas naturales](language-support.md)
* [Compatibilidad con contenedores de Docker](cognitive-services-container-support.md)
