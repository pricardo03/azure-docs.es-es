---
title: Directivas de streaming en Azure Media Services | Microsoft Docs
description: En este artículo se explica qué son las directivas de streaming y cómo las usa Azure Media Services.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 10/22/2018
ms.author: juliako
ms.openlocfilehash: c5f441fef95989e5c82586d96fc6c10e00a9627c
ms.sourcegitcommit: 5de9de61a6ba33236caabb7d61bee69d57799142
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/25/2018
ms.locfileid: "50085502"
---
# <a name="streaming-policies"></a>Directivas de streaming

En Azure Media Services v3, las directivas de streaming permiten definir los protocolos de streaming y las opciones de cifrado de StreamingLocators. Puede especificar el nombre de la directiva de streaming que creó o use una de las directivas de streaming predefinidas. Las directivas de streaming predefinidas disponibles actualmente son: "Predefined_DownloadOnly", "Predefined_ClearStreamingOnly", "Predefined_DownloadAndClearStreaming", "Predefined_ClearKey", "Predefined_MultiDrmCencStreaming" y "Predefined_ MultiDrmStreaming".

> [!IMPORTANT]
> Al utilizar el objeto [StreamingPolicy](https://docs.microsoft.com/rest/api/media/streamingpolicies) personalizado, debe diseñar un conjunto limitado de dichas directivas para su cuenta de Media Service y reutilizarlas para sus objetos StreamingLocator siempre que se necesiten las mismas opciones y protocolos de cifrado. La cuenta de Media Service tiene una cuota para el número de entradas de StreamingPolicy. No debe crear un nuevo objeto StreamingPolicy para cada objeto StreamingLocator.

## <a name="streamingpolicy-definition"></a>Definición de StreamingPolicy

En la tabla siguiente se muestran las propiedades de StreamingPolicy y se proporcionan sus definiciones.

|NOMBRE|Descripción|
|---|---|
|id|Identificador de recurso completo del recurso.|
|Nombre|Nombre del recurso.|
|properties.commonEncryptionCbcs|Configuración de CommonEncryptionCbcs|
|properties.commonEncryptionCenc|Configuración de CommonEncryptionCenc|
|properties.created |Hora de creación de la directiva de streaming|
|properties.defaultContentKeyPolicyName |ContentKey predeterminada que utiliza la directiva de streaming actual|
|properties.envelopeEncryption  |Configuración de EnvelopeEncryption|
|properties.noEncryption|Configuraciones de NoEncryption|
|Tipo|Tipo de recurso.|

Para conocer la definición completa, consulte [Directivas de streaming](https://docs.microsoft.com/rest/api/media/streamingpolicies).

## <a name="filtering-ordering-paging"></a>Filtrado, ordenación, paginación

Media Services admite las siguientes opciones de consulta de OData para las directivas de streaming: 

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

### <a name="filteringordering"></a>Filtrado y ordenación

En la tabla siguiente se muestra cómo pueden aplicarse estas opciones a las propiedades de StreamingPolicy: 

|NOMBRE|Filtrar|Orden|
|---|---|---|
|id|||
|Nombre|Eq, ne, ge, le, gt, lt|Ascendente y descendente|
|properties.commonEncryptionCbcs|||
|properties.commonEncryptionCenc|||
|properties.created |Eq, ne, ge, le, gt, lt|Ascendente y descendente|
|properties.defaultContentKeyPolicyName |||
|properties.envelopeEncryption|||
|properties.noEncryption|||
|Tipo|||

### <a name="pagination"></a>Paginación

La paginación se admite para cada uno de los cuatro criterios de ordenación habilitados. En este momento, el tamaño de página es 10.

> [!TIP]
> Siempre debe usar el vínculo siguiente para enumerar la colección y así no tener que depender de un tamaño de página determinado.

Si una respuesta de consulta contiene muchos elementos, el servicio devuelve una propiedad "\@odata.nextLink" para obtener la siguiente página de resultados. Esto se puede utilizar para pasar de página en el conjunto de resultados completo. No puede configurar el tamaño de página. 

Si se crean o eliminan los elementos StreamingPolicy durante la paginación a través de la colección, los cambios se ven reflejados en los resultados que se devuelven (si esos cambios se encuentran en la parte de la colección que no se ha descargado). 

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

## <a name="next-steps"></a>Pasos siguientes

[Streaming de un archivo](stream-files-dotnet-quickstart.md)
