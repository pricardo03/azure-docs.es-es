---
title: Uso de la CLI para crear filtros con Azure Media Services| Microsoft Docs
description: En este tema se muestra cómo usar la CLI para crear filtros con Media Services.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/26/2018
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: a16024ad5d8b9d2355b579b9b508ef0de91f2ccd
ms.sourcegitcommit: 78ec955e8cdbfa01b0fa9bdd99659b3f64932bba
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2018
ms.locfileid: "53133866"
---
# <a name="creating-filters-with-cli"></a>Creación de filtros con la CLI 

Al entregar su contenido a los clientes (streaming de eventos en vivo o vídeo bajo demanda), es posible que el cliente necesite más flexibilidad que la descrita en el archivo de manifiesto del recurso predeterminado. Azure Media Services le permite definir filtros de cuenta y filtros de recurso para su contenido. Para obtener más información, consulte [Filtros y manifiestos dinámicos](filters-dynamic-manifest-overview.md).

En este tema se muestra cómo configurar un filtro para un recurso Vídeo bajo demanda y usar la CLI para Media Services v3 para crear [Filtros de cuenta](https://docs.microsoft.com/cli/azure/ams/account-filter?view=azure-cli-latest) y [Filtros de recurso](https://docs.microsoft.com/cli/azure/ams/asset-filter?view=azure-cli-latest). 

## <a name="prerequisites"></a>Requisitos previos 

- Instale y use la CLI localmente, para este artículo es preciso usar la CLI de Azure versión 2.0 o posterior. Ejecute `az --version` para encontrar la versión que tiene. Si necesita instalarla o actualizarla, consulte [Instalación de la CLI de Azure](/cli/azure/install-azure-cli). 

    Actualmente no todos los comandos de la [CLI de Media Services v3](https://aka.ms/ams-v3-cli-ref) funcionan en Azure Cloud Shell. Se recomienda usar la CLI localmente.
- [Cree una cuenta de Media Services](create-account-cli-how-to.md). Asegúrese de recordar el nombre del grupo de recursos y el nombre de la cuenta de Media Services. 
- Consulte [Filtros y manifiestos dinámicos](filters-dynamic-manifest-overview.md).

## <a name="define-a-filter"></a>Definición de un filtro 

En el siguiente ejemplo se definen las condiciones de selección de seguimiento que se agregan al manifiesto final. Este filtro incluye las pistas de audio en inglés con EC-3 y las pistas de vídeo con velocidad de bits en el intervalo 0 a 1000000.

Los filtros definidos en REST incluyen el objeto JSON de contenedor "Properties".  

```json
[
    {
        "trackSelections": [
            {
                "property": "Type",
                "value": "Audio",
                "operation": "Equal"
            },
            {
                "property": "Language",
                "value": "en",
                "operation": "Equal"
            },
            {
                "property": "FourCC",
                "value": "EC-3",
                "operation": "NotEqual"
            }
        ]
    },
    {
        "trackSelections": [
            {
                "property": "Type",
                "value": "Video",
                "operation": "Equal"
            },
            {
                "property": "Bitrate",
                "value": "0-1000000",
                "operation": "Equal"
            }
        ]
    }
]
```

## <a name="create-account-filters"></a>Creación de filtros de cuenta

El siguiente comando [az ams account-filter](https://docs.microsoft.com/cli/azure/ams/account-filter?view=azure-cli-latest) crea un filtro de cuenta con selecciones de seguimiento de filtro [definidas anteriormente](#define-a-filter). 

```azurecli
az ams account-filter create -a amsAccount -g resourceGroup -n filterName --tracks @C:\tracks.json
```

Vea también la sección de [ejemplos de JSON para los filtros](https://docs.microsoft.com/rest/api/media/accountfilters/createorupdate#create_an_account_filter).

## <a name="create-asset-filters"></a>Creación de filtros de recurso

El siguiente comando [az ams asset-filter](https://docs.microsoft.com/cli/azure/ams/asset-filter?view=azure-cli-latest) crea un filtro de recurso con selecciones de seguimiento de filtro [definidas anteriormente](#define-a-filter). 

```azurecli
az ams asset-filter create -a amsAccount -g resourceGroup -n filterName --asset-name assetName --tracks @C:\tracks.json
```

Vea también la sección de [ejemplos de JSON para los filtros](https://docs.microsoft.com/rest/api/media/assetfilters/createorupdate#create_an_asset_filter).

## <a name="next-step"></a>Paso siguiente

[Streaming de vídeos](stream-files-tutorial-with-api.md) 

## <a name="see-also"></a>Otras referencias

[CLI de Azure](https://docs.microsoft.com/cli/azure/ams?view=azure-cli-latest)
