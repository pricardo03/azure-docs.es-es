---
title: Creación de una cuenta de Media Services con la CLI de Azure | Microsoft Docs
description: Siga los pasos de este inicio rápido para crear una cuenta de Azure Media Services.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.custom: seodec18
ms.date: 12/08/2018
ms.author: juliako
ms.openlocfilehash: fc80e2c540279c0a0c8acb575c9e5b1478a46cf8
ms.sourcegitcommit: 78ec955e8cdbfa01b0fa9bdd99659b3f64932bba
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2018
ms.locfileid: "53133373"
---
# <a name="create-an-azure-media-services-account"></a>Creación de una cuenta de Azure Media Services

Para iniciar el cifrado, la codificación, el análisis, la administración y el streaming de contenido multimedia en Azure, debe crear una cuenta de Media Services. En el momento en el que crea una cuenta de Media Services, también crea una cuenta de almacenamiento asociada (o usa una ya existente) en la misma región geográfica que la cuenta de Media Services.
En este artículo se describen los pasos para crear una cuenta de Azure Media Services con la CLI de Azure.  

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Requisitos previos

- Una suscripción de Azure activa. Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) antes de empezar.
- Instale y use la CLI localmente, para este artículo es preciso usar la CLI de Azure versión 2.0 o posterior. Ejecute `az --version` para encontrar la versión que tiene. Si necesita instalarla o actualizarla, consulte [Instalación de la CLI de Azure](/cli/azure/install-azure-cli). 

    Actualmente no todos los comandos de la [CLI de Media Services v3](https://aka.ms/ams-v3-cli-ref) funcionan en Azure Cloud Shell. Se recomienda usar la CLI localmente.

## <a name="set-the-azure-subscription"></a>Establecimiento de la suscripción de Azure

En el siguiente comando, proporcione el identificador de suscripción de Azure que quiere usar para la cuenta de Media Services. Para ver una lista de las suscripciones a las que tiene acceso, vaya a [Suscripciones](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade).

```azurecli
az account set --subscription mySubscriptionId
```
 
[!INCLUDE [media-services-cli-create-v3-account-include](../../../includes/media-services-cli-create-v3-account-include.md)]
 
## <a name="next-steps"></a>Pasos siguientes

[Streaming de un archivo](stream-files-dotnet-quickstart.md)

## <a name="see-also"></a>Otras referencias

[CLI de Azure](https://docs.microsoft.com/cli/azure/ams?view=azure-cli-latest)

