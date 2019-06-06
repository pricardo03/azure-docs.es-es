---
title: Creación de filtros con la API de REST de Azure Media Services | Microsoft Docs
description: En este tema se describe cómo crear filtros para que su cliente pueda usarlos para el streaming de secciones específicas de una secuencia. Media Services crea manifiestos dinámicos para lograr este streaming selectivo.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 06/03/2019
ms.author: juliako
ms.openlocfilehash: 01c1711fb70d31fe84c7e20272de0eb7ce82c879
ms.sourcegitcommit: 600d5b140dae979f029c43c033757652cddc2029
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/04/2019
ms.locfileid: "66494232"
---
# <a name="creating-filters-with-media-services-rest-api"></a>Creación de filtros con la API REST de Media Services

Al entregar su contenido a los clientes (streaming de eventos en vivo o vídeo bajo demanda), es posible que el cliente necesite más flexibilidad que la descrita en el archivo de manifiesto del recurso predeterminado. Azure Media Services le permite definir filtros de cuenta y filtros de recurso para su contenido. 

Para una descripción detallada de esta característica y los escenarios donde se utiliza, consulte [los manifiestos dinámicos](filters-dynamic-manifest-overview.md) y [filtros](filters-concept.md).

En este tema se muestra cómo definir un filtro para un recurso Vídeo bajo demanda y usar las API REST para crear [Filtros de cuenta](https://docs.microsoft.com/rest/api/media/accountfilters) y [Filtros de recurso](https://docs.microsoft.com/rest/api/media/assetfilters). 

> [!NOTE]
> No olvide revisar [presentationTimeRange](filters-concept.md#presentationtimerange).

## <a name="prerequisites"></a>Requisitos previos 

Para completar los pasos descritos en este tema, ha de:

- Consulte [Filtros y manifiestos dinámicos](filters-dynamic-manifest-overview.md).
- [Configuración de Postman para llamadas API REST de Azure Media Services](media-rest-apis-with-postman.md).

    Asegúrese de seguir el último paso en el tema [Obtención del token de Azure AD](media-rest-apis-with-postman.md#get-azure-ad-token). 

## <a name="define-a-filter"></a>Definición de un filtro  

Este es el ejemplo de **Cuerpo de la solicitud** que define las condiciones de selección de seguimiento que se agregan al manifiesto. Este filtro incluye las pistas de audio con EC-3 y las pistas de vídeo con velocidad de bits en el intervalo 0 a 1000000.

```json
{
    "properties": {
        "tracks": [
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
                        "operation": "Equal"
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
     }
}
```

## <a name="create-account-filters"></a>Creación de filtros de cuenta

En la colección de Postman que ha descargado, seleccione **Filtros de cuenta**->**Create or update an Account Filter** (Crear o actualizar un filtro de cuenta).

El método **PUT** de solicitud HTTP es similar a:

```
PUT https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Media/mediaServices/{accountName}/accountFilters/{filterName}?api-version=2018-07-01
```

Seleccione la pestaña **Cuerpo** y pegue el código json [que definió anteriormente](#define-a-filter).

Seleccione **Enviar**. 

Se ha creado el filtro.

Para más información, vea el tema sobre [creación o actualización](https://docs.microsoft.com/rest/api/media/accountfilters/createorupdate). Vea también la sección de [ejemplos de JSON para los filtros](https://docs.microsoft.com/rest/api/media/accountfilters/createorupdate#create_an_account_filter).

## <a name="create-asset-filters"></a>Creación de filtros de recurso  

En la colección de Postman "Media Services v3" que ha descargado, seleccione **Recursos**->**Create or update Asset Filter (Crear o actualizar filtro de recurso)** .

El método **PUT** de solicitud HTTP es similar a:

```
PUT https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Media/mediaServices/{accountName}/assets/{assetName}/assetFilters/{filterName}?api-version=2018-07-01
```

Seleccione la pestaña **Cuerpo** y pegue el código json [que definió anteriormente](#define-a-filter).

Seleccione **Enviar**. 

Se ha creado el filtro de recurso.

Para más información sobre cómo crear o actualizar los filtros de recurso, vea el tema sobre [creación o actualización](https://docs.microsoft.com/rest/api/media/assetfilters/createorupdate). Vea también la sección de [ejemplos de JSON para los filtros](https://docs.microsoft.com/rest/api/media/assetfilters/createorupdate#create_an_asset_filter). 

## <a name="next-steps"></a>Pasos siguientes

[Streaming de vídeos](stream-files-tutorial-with-rest.md) 
