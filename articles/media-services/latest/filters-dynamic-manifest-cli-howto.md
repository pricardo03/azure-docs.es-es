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
ms.openlocfilehash: 2ba3de32f4ec3b9f6faf1d5a51da9c1c91e4a2e4
ms.sourcegitcommit: eecd816953c55df1671ffcf716cf975ba1b12e6b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/28/2019
ms.locfileid: "55099313"
---
# <a name="creating-filters-with-cli"></a>Creación de filtros con la CLI 

Al entregar su contenido a los clientes (streaming de eventos en vivo o vídeo bajo demanda), es posible que el cliente necesite más flexibilidad que la descrita en el archivo de manifiesto del recurso predeterminado. Azure Media Services le permite definir filtros de cuenta y filtros de recurso para su contenido. Para obtener más información, consulte [Filtros y manifiestos dinámicos](filters-dynamic-manifest-overview.md).

En este tema se muestra cómo configurar un filtro para un recurso Vídeo bajo demanda y usar la CLI para Media Services v3 para crear [Filtros de cuenta](https://docs.microsoft.com/cli/azure/ams/account-filter?view=azure-cli-latest) y [Filtros de recurso](https://docs.microsoft.com/cli/azure/ams/asset-filter?view=azure-cli-latest). 

## <a name="prerequisites"></a>Requisitos previos 

- [Cree una cuenta de Media Services](create-account-cli-how-to.md). Asegúrese de recordar el nombre del grupo de recursos y el nombre de la cuenta de Media Services. 
- Consulte [Filtros y manifiestos dinámicos](filters-dynamic-manifest-overview.md).

[!INCLUDE [media-services-cli-instructions](../../../includes/media-services-cli-instructions.md)]

## <a name="define-a-filter"></a>Definición de un filtro 

En el siguiente ejemplo se definen las condiciones de selección de seguimiento que se agregan al manifiesto final. Este filtro incluye las pistas de audio con EC-3 y las pistas de vídeo con velocidad de bits en el intervalo 0 a 1000000.

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

El comando permite pasar un parámetro `--tracks` opcional que contiene un valor JSON que representa las selecciones de seguimiento.  Use @{file} para cargar el valor JSON desde un archivo. Si usa la CLI de Azure localmente, especifique la ruta de acceso completa al de archivo:

```azurecli
az ams account-filter create -a amsAccount -g resourceGroup -n filterName --tracks @tracks.json
```

Vea también la sección de [ejemplos de JSON para los filtros](https://docs.microsoft.com/rest/api/media/accountfilters/createorupdate#create_an_account_filter).

## <a name="create-asset-filters"></a>Creación de filtros de recurso

El siguiente comando [az ams asset-filter](https://docs.microsoft.com/cli/azure/ams/asset-filter?view=azure-cli-latest) crea un filtro de recurso con selecciones de seguimiento de filtro [definidas anteriormente](#define-a-filter). 

```azurecli
az ams asset-filter create -a amsAccount -g resourceGroup -n filterName --asset-name assetName --tracks @tracks.json
```

Vea también la sección de [ejemplos de JSON para los filtros](https://docs.microsoft.com/rest/api/media/assetfilters/createorupdate#create_an_asset_filter).

## <a name="next-step"></a>Paso siguiente

[Streaming de vídeos](stream-files-tutorial-with-api.md) 

## <a name="see-also"></a>Otras referencias

[CLI de Azure](https://docs.microsoft.com/cli/azure/ams?view=azure-cli-latest)
