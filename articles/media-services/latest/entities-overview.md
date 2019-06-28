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
ms.date: 04/08/2019
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: 28c880e8709074d808a41d9920361eaa2b20ecc4
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "60732372"
---
# <a name="filtering-ordering-paging-of-media-services-entities"></a>Filtrado, ordenación y paginación de entidades de Media Services

Media Services admite las siguientes opciones de consulta de OData para las entidades de la versión v3: 

* $filter 
* $orderby 
* $top 
* $skiptoken 

Descripción del operador:

* Eq = es igual que
* Ne = no es igual que
* Ge = es mayor o igual que
* Le = es menor o igual que
* Gt = es mayor que
* Lt = es menor que

Las propiedades de entidades que son de tipo Datetime siempre están en formato UTC.

## <a name="page-results"></a>Resultados de la página

Si una respuesta de consulta contiene muchos elementos, el servicio devuelve una propiedad "\@odata.nextLink" para obtener la siguiente página de resultados. Esto se puede utilizar para pasar de página en el conjunto de resultados completo. No puede configurar el tamaño de página. El tamaño de página varía según el tipo de entidad. Lea las secciones individuales que vienen a continuación para más información.

Si se crean o eliminan entidades durante la paginación a través de la colección, los cambios se reflejan en los resultados devueltos (si esos cambios se encuentran en la parte de la colección que no se ha descargado). 

> [!TIP]
> Siempre debe usar el vínculo siguiente para enumerar la colección y no tener que depender de un tamaño de página determinado.

## <a name="assets"></a>Recursos

### <a name="filteringordering"></a>Filtrado y ordenación

En la tabla siguiente se muestra cómo pueden aplicarse las opciones de filtrado y ordenación a las propiedades del [recurso](https://docs.microsoft.com/rest/api/media/assets): 

|NOMBRE|Filtrar|Orden|
|---|---|---|
|id|||
|Nombre|eq, gt, lt| ascendente y descendente|
|properties.alternateId |eq||
|properties.assetId |eq||
|properties.container |||
|properties.created| eq, gt, lt| ascendente y descendente|
|properties.description |||
|properties.lastModified |||
|properties.storageAccountName |||
|properties.storageEncryptionFormat | ||
|Tipo|||

El ejemplo de C# siguiente se filtra según la fecha de creación:

```csharp
var odataQuery = new ODataQuery<Asset>("properties/created lt 2018-05-11T17:39:08.387Z");
var firstPage = await MediaServicesArmClient.Assets.ListAsync(CustomerResourceGroup, CustomerAccountName, odataQuery);
```

### <a name="pagination"></a>Paginación 

La paginación se admite para cada uno de los cuatro criterios de ordenación habilitados. En este momento, el tamaño de página es 1000.

#### <a name="c-example"></a>Ejemplo de C#

En el ejemplo de C# siguiente se muestra cómo enumerar todos los recursos de la cuenta.

```csharp
var firstPage = await MediaServicesArmClient.Assets.ListAsync(CustomerResourceGroup, CustomerAccountName);

var currentPage = firstPage;
while (currentPage.NextPageLink != null)
{
    currentPage = await MediaServicesArmClient.Assets.ListNextAsync(currentPage.NextPageLink);
}
```

#### <a name="rest-example"></a>Ejemplo de REST

Analice el siguiente ejemplo de donde se usa $skiptoken. Asegúrese de reemplazar *amstestaccount* con el nombre de cuenta y establezca el valor de *api-version* a la versión más reciente.

Si solicita una lista de recursos similar a la siguiente:

```
GET  https://management.azure.com/subscriptions/00000000-3761-485c-81bb-c50b291ce214/resourceGroups/mediaresources/providers/Microsoft.Media/mediaServices/amstestaccount/assets?api-version=2018-07-01 HTTP/1.1
x-ms-client-request-id: dd57fe5d-f3be-4724-8553-4ceb1dbe5aab
Content-Type: application/json; charset=utf-8
```

Obtendrá una respuesta similar a esta:

```
HTTP/1.1 200 OK
 
{
"value":[
{
"name":"Asset 0","id":"/subscriptions/00000000-3761-485c-81bb-c50b291ce214/resourceGroups/mediaresources/providers/Microsoft.Media/mediaservices/amstestaccount/assets/Asset 0","type":"Microsoft.Media/mediaservices/assets","properties":{
"assetId":"00000000-5a4f-470a-9d81-6037d7c23eff","created":"2018-12-11T22:12:44.98Z","lastModified":"2018-12-11T22:15:48.003Z","container":"asset-98d07299-5a4f-470a-9d81-6037d7c23eff","storageAccountName":"amsdevc1stoaccount11","storageEncryptionFormat":"None"
}
},
// lots more assets
{
"name":"Asset 517","id":"/subscriptions/00000000-3761-485c-81bb-c50b291ce214/resourceGroups/mediaresources/providers/Microsoft.Media/mediaservices/amstestaccount/assets/Asset 517","type":"Microsoft.Media/mediaservices/assets","properties":{
"assetId":"00000000-912e-447b-a1ed-0f723913b20d","created":"2018-12-11T22:14:08.473Z","lastModified":"2018-12-11T22:19:29.657Z","container":"asset-fd05a503-912e-447b-a1ed-0f723913b20d","storageAccountName":"amsdevc1stoaccount11","storageEncryptionFormat":"None"
}
}
],"@odata.nextLink":"https:// management.azure.com/subscriptions/00000000-3761-485c-81bb-c50b291ce214/resourceGroups/mediaresources/providers/Microsoft.Media/mediaServices/amstestaccount/assets?api-version=2018-07-01&$skiptoken=Asset+517"
}
```

A continuación, puede solicitar la siguiente página si envía una solicitud "get":

```
https://management.azure.com/subscriptions/00000000-3761-485c-81bb-c50b291ce214/resourceGroups/mediaresources/providers/Microsoft.Media/mediaServices/amstestaccount/assets?api-version=2018-07-01&$skiptoken=Asset+517
```

Para obtener más ejemplos de REST, consulte [Assets - List](https://docs.microsoft.com/rest/api/media/assets/list) (Recursos: lista).

## <a name="content-key-policies"></a>Directivas de clave de contenido

### <a name="filteringordering"></a>Filtrado y ordenación

En la tabla siguiente se muestra cómo pueden aplicarse estas opciones a las propiedades de [directivas de clave de contenido](https://docs.microsoft.com/rest/api/media/contentkeypolicies): 

|NOMBRE|Filtrar|Orden|
|---|---|---|
|id|||
|Nombre|eq, ne, ge, le, gt, lt|ascendente y descendente|
|properties.created |eq, ne, ge, le,  gt, lt|ascendente y descendente|
|properties.description |eq, ne, ge, le, gt, lt||
|properties.lastModified|eq, ne, ge, le, gt, lt|ascendente y descendente|
|properties.options |||
|properties.policyId|eq, ne||
|Tipo|||

### <a name="pagination"></a>Paginación

La paginación se admite para cada uno de los cuatro criterios de ordenación habilitados. En este momento, el tamaño de página es 10.

En el siguiente ejemplo de C# se muestra cómo enumerar todos los elementos de **directivas de clave de contenido** de la cuenta.

```csharp
var firstPage = await MediaServicesArmClient.ContentKeyPolicies.ListAsync(CustomerResourceGroup, CustomerAccountName);

var currentPage = firstPage;
while (currentPage.NextPageLink != null)
{
    currentPage = await MediaServicesArmClient.ContentKeyPolicies.ListNextAsync(currentPage.NextPageLink);
}
```

Para obtener ejemplos de REST, vea [Content Key Policies - List](https://docs.microsoft.com/rest/api/media/contentkeypolicies/list) (Lista de directivas de clave de contenido)

## <a name="jobs"></a>Trabajos

### <a name="filteringordering"></a>Filtrado y ordenación

En la tabla siguiente se muestra cómo pueden aplicarse estas opciones a las propiedades de [trabajos](https://docs.microsoft.com/rest/api/media/jobs): 

| NOMBRE    | Filtrar                        | Orden |
|---------|-------------------------------|-------|
| Nombre                    | eq            | ascendente y descendente|
| properties.state        | eq, ne        |                         |
| properties.created      | gt, ge, lt, le| ascendente y descendente|
| properties.lastModified | gt, ge, lt, le | ascendente y descendente| 

### <a name="pagination"></a>Paginación

En Media Services v3 se admite la paginación de los trabajos.

En el ejemplo de C# siguiente se muestra cómo enumerar los trabajos de la cuenta.

```csharp            
List<string> jobsToDelete = new List<string>();
var pageOfJobs = client.Jobs.List(config.ResourceGroup, config.AccountName, "Encode");

bool exit;
do
{
    foreach (Job j in pageOfJobs)
    {
        jobsToDelete.Add(j.Name);
    }

    if (pageOfJobs.NextPageLink != null)
    {
        pageOfJobs = client.Jobs.ListNext(pageOfJobs.NextPageLink);
        exit = false;
    }
    else
    {
        exit = true;
    }
}
while (!exit);

```

Para obtener ejemplos de REST, vea [Jobs - List](https://docs.microsoft.com/rest/api/media/jobs/list) (Trabajos: lista)

## <a name="streaming-locators"></a>Localizadores de streaming

### <a name="filteringordering"></a>Filtrado y ordenación

En la tabla siguiente se muestra cómo pueden aplicarse estas opciones a las propiedades de StreamingLocator: 

|NOMBRE|Filtrar|Orden|
|---|---|---|
|id |||
|Nombre|eq, ne, ge, le, gt, lt|ascendente y descendente|
|properties.alternativeMediaId  |||
|properties.assetName   |||
|properties.contentKeys |||
|properties.created |eq, ne, ge, le,  gt, lt|ascendente y descendente|
|properties.defaultContentKeyPolicyName |||
|properties.endTime |eq, ne, ge, le, gt, lt|ascendente y descendente|
|properties.startTime   |||
|properties.streamingLocatorId  |||
|properties.streamingPolicyName |||
|Tipo   |||

### <a name="pagination"></a>Paginación

La paginación se admite para cada uno de los cuatro criterios de ordenación habilitados. En este momento, el tamaño de página es 10.

En el ejemplo de C# siguiente se muestra cómo enumerar todos los elementos StreamingLocators de la cuenta.

```csharp
var firstPage = await MediaServicesArmClient.StreamingLocators.ListAsync(CustomerResourceGroup, CustomerAccountName);

var currentPage = firstPage;
while (currentPage.NextPageLink != null)
{
    currentPage = await MediaServicesArmClient.StreamingLocators.ListNextAsync(currentPage.NextPageLink);
}
```

Para obtener ejemplos de REST, consulte [Streaming Locators - List](https://docs.microsoft.com/rest/api/media/streaminglocators/list) (Lista de localizadores de streaming).

## <a name="streaming-policies"></a>Directivas de streaming

### <a name="filteringordering"></a>Filtrado y ordenación

En la tabla siguiente se muestra cómo pueden aplicarse estas opciones a las propiedades de StreamingPolicy: 

|NOMBRE|Filtrar|Orden|
|---|---|---|
|id|||
|Nombre|eq, ne, ge, le, gt, lt|ascendente y descendente|
|properties.commonEncryptionCbcs|||
|properties.commonEncryptionCenc|||
|properties.created |eq, ne, ge, le,  gt, lt|ascendente y descendente|
|properties.defaultContentKeyPolicyName |||
|properties.envelopeEncryption|||
|properties.noEncryption|||
|Tipo|||

### <a name="pagination"></a>Paginación

La paginación se admite para cada uno de los cuatro criterios de ordenación habilitados. En este momento, el tamaño de página es 10.

En el ejemplo de C# siguiente se muestra cómo enumerar todos los elementos StreamingPolicies de la cuenta.

```csharp
var firstPage = await MediaServicesArmClient.StreamingPolicies.ListAsync(CustomerResourceGroup, CustomerAccountName);

var currentPage = firstPage;
while (currentPage.NextPageLink != null)
{
    currentPage = await MediaServicesArmClient.StreamingPolicies.ListNextAsync(currentPage.NextPageLink);
}
```

Para obtener ejemplos de REST, consulte [Streaming Policies - List](https://docs.microsoft.com/rest/api/media/streamingpolicies/list) (Lista de directivas de streaming).

## <a name="transform"></a>Transformación

### <a name="filteringordering"></a>Filtrado y ordenación

En la tabla siguiente se muestra cómo pueden aplicarse estas opciones a las propiedades de [transformaciones](https://docs.microsoft.com/rest/api/media/transforms): 

| NOMBRE    | Filtrar                        | Orden |
|---------|-------------------------------|-------|
| Nombre                    | eq            | ascendente y descendente|
| properties.created      | gt, ge, lt, le| ascendente y descendente|
| properties.lastModified | gt, ge, lt, le | ascendente y descendente|

## <a name="next-steps"></a>Pasos siguientes

[Streaming de un archivo](stream-files-dotnet-quickstart.md)
