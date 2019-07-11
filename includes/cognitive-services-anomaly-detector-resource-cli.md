---
author: aahill
ms.author: aahi
ms.service: cognitive-services
ms.topic: include
ms.date: 06/20/2019
ms.openlocfilehash: 1ea0b01997d3d5cecff73f951c51de5898c2f07a
ms.sourcegitcommit: 837dfd2c84a810c75b009d5813ecb67237aaf6b8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2019
ms.locfileid: "67503637"
---
Los siguientes comandos de la CLI de Azure aprovisionan un recurso de Anomaly Detector en el plan de tarifa gratuito. Haga clic en el botón **Pruébelo**, pegue el código y presione Entrar para ejecutarlo en Azure Cloud Shell. Se imprimirán las claves para autenticar la aplicación. Cuando finalice, [cree una variable de entorno](../articles/cognitive-services/cognitive-services-apis-create-account.md#configure-an-environment-variable-for-authentication) para cualquiera de las claves, llamada `ANOMALY_DETECTOR_KEY`.

> [!NOTE]
> * Si lo desea, puede:
>    * crear un recurso mediante [Azure Portal](../articles/cognitive-services/cognitive-services-apis-create-account.md) o la [CLI de Azure](../articles/cognitive-services/cognitive-services-apis-create-account.md) en la máquina local.
>    * obtener una [clave de prueba](https://azure.microsoft.com/try/cognitive-services/#decision) válida durante siete días de forma gratuita. Después de registrarse, estará disponible en el [sitio web de Azure](https://azure.microsoft.com/try/cognitive-services/my-apis/).
>    * ver este recurso en [Azure Portal](https://portal.azure.com/). 

Los servicios de Cognitive Services se representan por medio de recursos de Azure que se aprovisionan. Cada cuenta de Cognitive Services (y sus recursos de Azure asociados) deben pertenecer a un grupo de recursos de Azure.

1. Creación de un grupo de recursos de Azure

    ```azurecli-interactive
    az group create \
        --name example-anomaly-detector-resource-group \
        --location westus2
    ```

2. Creación de un recurso de Anomaly Detector en el nivel gratuito

    ```azurecli-interactive
    az cognitiveservices account create \
        --name example-anomaly-detector-resource \
        --resource-group example-anomaly-detector-resource-group \
        --kind AnomalyDetector \
        --sku F0 \
        --location westus2 \
        --yes
    ```

3. Enumeración de las claves del recurso

    ```azurecli-interactive
    az cognitiveservices account keys list \
        --name example-anomaly-detector-resource \
        --resource-group example-anomaly-detector-resource-group
    ```