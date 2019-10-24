---
title: 'Filtrado, ordenación y paginación de entidades de Media Services: Azure | Microsoft Docs'
description: En este artículo se describe el filtrado, la ordenación y la paginación de entidades de Azure Media Services.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 10/11/2019
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: ed509ac8fea43a9c011bbbf76c1dc433cd78d43c
ms.sourcegitcommit: 8b44498b922f7d7d34e4de7189b3ad5a9ba1488b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/13/2019
ms.locfileid: "72298953"
---
# <a name="filtering-ordering-paging-of-media-services-entities"></a>Filtrado, ordenación y paginación de entidades de Media Services

En este tema se describen las opciones de consulta de OData y la compatibilidad con la paginación disponible al enumerar entidades de Azure Media Services v3.

## <a name="considerations"></a>Consideraciones

* Las propiedades de entidades que son de tipo Datetime siempre están en formato UTC.
* El espacio en blanco de la cadena de consulta debe codificarse para URL antes de enviar una solicitud.

## <a name="comparison-operators"></a>Operadores de comparación

Puede usar los operadores siguientes para comparar un campo con un valor constante:

Operadores de igualdad:

- `eq`: se comprueba si un campo es **igual a** un valor constante
- `ne`: se comprueba si campo **no es igual a** un valor constante

Operadores de rango:

- `gt`: se comprueba si un campo es **mayor que** un valor constante
- `lt`: se comprueba si un campo es **menor que** un valor constante
- `ge`: se comprueba si un campo es **mayor o igual que** un valor constante
- `le`: se comprueba si un campo es **menor o igual que** un valor constante

## <a name="filter"></a>Filtrar

**$filter**: use filter para suministrar un parámetro de filtro de OData que busque únicamente los objetos que le interesen.

En el siguiente ejemplo de REST se filtra por alternateId en un recurso:

```
GET https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/mediaresources/providers/Microsoft.Media/mediaServices/amstestaccount/assets?api-version=2018-07-01&$filter=properties/alternateId%20eq%20'unique identifier'
```

En el siguiente ejemplo en C# se filtra por la fecha de creación del recurso:

```csharp
var odataQuery = new ODataQuery<Asset>("properties/created lt 2018-05-11T17:39:08.387Z");
var firstPage = await MediaServicesArmClient.Assets.ListAsync(CustomerResourceGroup, CustomerAccountName, odataQuery);
```    

## <a name="order-by"></a>Ordenar por

**$orderby** : se usa para ordenar los objetos devueltos por el parámetro especificado. Por ejemplo:    

```
GET https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/mediaresources/providers/Microsoft.Media/mediaServices/amstestaccount/assets?api-version=2018-07-01$orderby=properties/created%20gt%202018-05-11T17:39:08.387Z
```

Para ordenar los resultados de forma ascendente o descendente, anexe `asc` o `desc` al nombre del campo, separados por un espacio. Por ejemplo, `$orderby properties/created desc`.

## <a name="skip-token"></a>Token de omisión

**$skiptoken**: si la respuesta a una consulta contiene muchos elementos, el servicio devuelve un valor de token de omisión (`@odata.nextLink`) que se usa para obtener la siguiente página de resultados. Esto se puede utilizar para pasar de página en el conjunto de resultados completo.

En Media Services v3, no se puede configurar el tamaño de página. El tamaño de página varía según el tipo de entidad. Lea las secciones individuales que vienen a continuación para más información.

Si se crean o eliminan entidades durante la paginación a través de la colección, los cambios se reflejan en los resultados devueltos (si esos cambios se encuentran en la parte de la colección que no se ha descargado). 

> [!TIP]
> Se debe usar siempre `nextLink` para enumerar la colección y no tener que depender de un tamaño de página determinado.
>
> `nextLink` solo estará presente si hay más de una página de entidades.

Analice el siguiente ejemplo de donde se usa $skiptoken. Asegúrese de reemplazar *amstestaccount* con el nombre de cuenta y establezca el valor de *api-version* a la versión más reciente.

Si solicita una lista de recursos similar a la siguiente:

```
GET  https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/mediaresources/providers/Microsoft.Media/mediaServices/amstestaccount/assets?api-version=2018-07-01 HTTP/1.1
x-ms-client-request-id: dd57fe5d-f3be-4724-8553-4ceb1dbe5aab
Content-Type: application/json; charset=utf-8
```

Obtendrá una respuesta similar a esta:

```
HTTP/1.1 200 OK

{
"value":[
{
"name":"Asset 0","id":"/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/mediaresources/providers/Microsoft.Media/mediaservices/amstestaccount/assets/Asset 0","type":"Microsoft.Media/mediaservices/assets","properties":{
"assetId":"00000000-0000-0000-0000-000000000000","created":"2018-12-11T22:12:44.98Z","lastModified":"2018-12-11T22:15:48.003Z","container":"asset-00000000-0000-0000-0000-0000000000000","storageAccountName":"amsacctname","storageEncryptionFormat":"None"
}
},
// lots more assets
{
"name":"Asset 517","id":"/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/mediaresources/providers/Microsoft.Media/mediaservices/amstestaccount/assets/Asset 517","type":"Microsoft.Media/mediaservices/assets","properties":{
"assetId":"00000000-0000-0000-0000-000000000000","created":"2018-12-11T22:14:08.473Z","lastModified":"2018-12-11T22:19:29.657Z","container":"asset-00000000-0000-0000-0000-000000000000","storageAccountName":"amsacctname","storageEncryptionFormat":"None"
}
}
],"@odata.nextLink":"https:// management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/mediaresources/providers/Microsoft.Media/mediaServices/amstestaccount/assets?api-version=2018-07-01&$skiptoken=Asset+517"
}
```

A continuación, puede solicitar la siguiente página si envía una solicitud "get":

```
https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/mediaresources/providers/Microsoft.Media/mediaServices/amstestaccount/assets?api-version=2018-07-01&$skiptoken=Asset+517
```

En el ejemplo de C# siguiente se muestra cómo enumerar todos los localizadores de streaming de la cuenta.

```csharp
var firstPage = await MediaServicesArmClient.StreamingLocators.ListAsync(CustomerResourceGroup, CustomerAccountName);

var currentPage = firstPage;
while (currentPage.NextPageLink != null)
{
    currentPage = await MediaServicesArmClient.StreamingLocators.ListNextAsync(currentPage.NextPageLink);
}
```

## <a name="using-logical-operators-to-combine-query-options"></a>Uso de operadores lógicos para combinar las opciones de consulta

Media Services V3 admite los operadores lógicos "or" y "and". 

En el siguiente ejemplo de REST se comprueba el estado del trabajo:

```
https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/qbtest/providers/Microsoft.Media/mediaServices/qbtest/transforms/VideoAnalyzerTransform/jobs?$filter=properties/state%20eq%20Microsoft.Media.JobState'Scheduled'%20or%20properties/state%20eq%20Microsoft.Media.JobState'Processing'&api-version=2018-07-01
```

La misma consulta de C# se construye así: 

```csharp
var odataQuery = new ODataQuery<Job>("properties/state eq Microsoft.Media.JobState'Scheduled' or properties/state eq Microsoft.Media.JobState'Processing'");
client.Jobs.List(config.ResourceGroup, config.AccountName, VideoAnalyzerTransformName, odataQuery);
```

## <a name="filtering-and-ordering-options-of-entities"></a>Opciones de filtrado y ordenación de entidades

En la tabla siguiente se muestra cómo pueden aplicarse las opciones de filtrado y ordenación a diferentes entidades:

|Nombre de entidad|Nombre de propiedad|Filtrar|Orden|
|---|---|---|---|
|[Recursos](https://docs.microsoft.com/rest/api/media/assets/)|Nombre|`eq`, `gt`, `lt`, `ge`, `le`|`asc` y `desc`|
||properties.alternateId |`eq`||
||properties.assetId |`eq`||
||properties.created| `eq`, `gt`, `lt`| `asc` y `desc`|
|[Directivas de claves de contenido](https://docs.microsoft.com/rest/api/media/contentkeypolicies)|Nombre|`eq`, `ne`, `ge`, `le`, `gt`, `lt`|`asc` y `desc`|
||properties.created    |`eq`, `ne`, `ge`, `le`, `gt`, `lt`|`asc` y `desc`|
||properties.description    |`eq`, `ne`, `ge`, `le`, `gt`, `lt`||
||properties.lastModified|`eq`, `ne`, `ge`, `le`, `gt`, `lt`|`asc` y `desc`|
||properties.policyId|`eq`, `ne`||
|[Trabajos](https://docs.microsoft.com/rest/api/media/jobs)| Nombre  | `eq`            | `asc` y `desc`|
||properties.state        | `eq`, `ne`        |                         |
||properties.created      | `gt`, `ge`, `lt`, `le`| `asc` y `desc`|
||properties.lastModified | `gt`, `ge`, `lt`, `le` | `asc` y `desc`| 
|[Localizadores de streaming](https://docs.microsoft.com/rest/api/media/streaminglocators)|Nombre|`eq`, `ne`, `ge`, `le`, `gt`, `lt`|`asc` y `desc`|
||properties.created    |`eq`, `ne`, `ge`, `le`, `gt`, `lt`|`asc` y `desc`|
||properties.endTime    |`eq`, `ne`, `ge`, `le`, `gt`, `lt`|`asc` y `desc`|
|[Directivas de streaming](https://docs.microsoft.com/rest/api/media/streamingpolicies)|Nombre|`eq`, `ne`, `ge`, `le`, `gt`, `lt`|`asc` y `desc`|
||properties.created    |`eq`, `ne`, `ge`, `le`, `gt`, `lt`|`asc` y `desc`|
|[Transformaciones](https://docs.microsoft.com/rest/api/media/transforms)| Nombre | `eq`            | `asc` y `desc`|
|| properties.created      | `gt`, `ge`, `lt`, `le`| `asc` y `desc`|
|| properties.lastModified | `gt`, `ge`, `lt`, `le`| `asc` y `desc`|

## <a name="next-steps"></a>Pasos siguientes

* [Enumeración de recursos](https://docs.microsoft.com/rest/api/media/assets/list)
* [Enumeración de directivas de claves de contenido](https://docs.microsoft.com/rest/api/media/contentkeypolicies/list)
* [Enumeración de trabajos](https://docs.microsoft.com/rest/api/media/jobs/list)
* [Enumeración de directivas de streaming](https://docs.microsoft.com/rest/api/media/streamingpolicies/list)
* [Enumeración de localizadores de streaming](https://docs.microsoft.com/rest/api/media/streaminglocators/list)
* [Streaming de un archivo](stream-files-dotnet-quickstart.md)
* [Cuotas y limitaciones](limits-quotas-constraints.md)
