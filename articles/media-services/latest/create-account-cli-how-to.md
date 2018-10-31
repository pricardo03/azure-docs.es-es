---
title: Creación de una cuenta de Azure Media Services con la CLI de Azure | Microsoft Docs
description: Siga los pasos de este inicio rápido para crear una cuenta de Azure Media Services.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.custom: ''
ms.date: 10/15/2018
ms.author: juliako
ms.openlocfilehash: 988b56569c2537f9f0c74b1bcc2bf5e2a9400a80
ms.sourcegitcommit: 3a7c1688d1f64ff7f1e68ec4bb799ba8a29a04a8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2018
ms.locfileid: "49378812"
---
# <a name="create-an-azure-media-services-account"></a>Creación de una cuenta de Azure Media Services

Para iniciar el cifrado, la codificación, el análisis, la administración y el streaming de contenido multimedia en Azure, debe crear una cuenta de Media Services. En el momento en el que crea una cuenta de Media Services, también crea una cuenta de almacenamiento asociada (o usa una ya existente) en la misma región geográfica que la cuenta de Media Services.

En este artículo se describen los pasos para crear una cuenta de Azure Media Services con la CLI de Azure.  

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

Inicie sesión en [Azure Portal](http://portal.azure.com) e inicie **CloudShell** para ejecutar comandos de la CLI, tal como se muestra en los siguientes pasos.

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

Si decide instalar y usar la CLI localmente, para este artículo es preciso la CLI de Azure versión 2.0 o posterior. Ejecute `az --version` para encontrar la versión que tiene. Si necesita instalarla o actualizarla, consulte [Instalación de la CLI de Azure](/cli/azure/install-azure-cli). 

## <a name="set-the-azure-subscription"></a>Establecimiento de la suscripción de Azure

En el siguiente comando, proporcione el identificador de suscripción de Azure que quiere usar para la cuenta de Media Services. Para ver una lista de las suscripciones a las que tiene acceso, vaya a [Suscripciones](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade).

```azurecli-interactive
az account set --subscription mySubscriptionId
```
 
[!INCLUDE [media-services-cli-create-v3-account-include](../../../includes/media-services-cli-create-v3-account-include.md)]
 
## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Streaming de un archivo](stream-files-dotnet-quickstart.md)
