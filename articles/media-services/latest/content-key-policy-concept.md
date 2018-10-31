---
title: Directivas de clave de contenido de Azure Media Services | Microsoft Docs
description: En este artículo se explica qué son las directivas de clave de contenido y cómo las usa Azure Media Services.
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
ms.openlocfilehash: 9a5ef8df9b1ca87430fb5e8d1da94f1899c4a856
ms.sourcegitcommit: c2c279cb2cbc0bc268b38fbd900f1bac2fd0e88f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/24/2018
ms.locfileid: "49985869"
---
# <a name="content-key-policies"></a>Directivas de clave de contenido

Puede usar Azure Media Services para proteger su contenido multimedia desde el momento en que este deja el equipo y pasa a través del almacenamiento, el procesamiento y la entrega. Con Media Services puede entregar el contenido cifrado de forma dinámica en vivo y a petición con Estándar de cifrado avanzado (AES-128) o cualquiera de los tres sistemas de administración de derechos digitales (DRM) principales: Microsoft PlayReady, Google Widevine y Apple FairPlay. Media Services también proporciona un servicio para entregar claves AES y licencias de DMR (PlayReady, Widevine y FairPlay) a los clientes autorizados.

En Azure Media Services v3, las directivas de clave de contenido permiten especificar cómo se entrega la clave de contenido a los clientes finales mediante el componente Media Services Key Delivery. Para obtener más información, vea [Introducción a la protección de contenido](content-protection-overview.md).

## <a name="contentkeypolicies-definition"></a>Definición de directivas de clave de contenido

En la tabla siguiente se muestran las propiedades de las directivas de clave de contenido y se proporcionan sus definiciones.

|NOMBRE|Escriba|DESCRIPCIÓN|
|---|---|---|
|id|string|Identificador de recurso completo del recurso.|
|Nombre|string|Nombre del recurso.|
|properties.created |string|Fecha de creación de la directiva|
|properties.description |string|Descripción de la directiva.|
|properties.lastModified    |string|Fecha de la última modificación de la directiva|
|properties.options |ContentKeyPolicyOption[]|Opciones de la directiva de clave.|
|properties.policyId    |string|Identificador de directiva heredado.|
|Tipo   |string|Tipo de recurso.|

Para obtener la definición completa, vea [Content Key Policies](https://docs.microsoft.com/rest/api/media/contentkeypolicies) (Directivas de clave de contenido).

## <a name="filtering-ordering-paging"></a>Filtrado, ordenación, paginación

Media Services admite las siguientes opciones de consulta de OData para las directivas de clave de contenido: 

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
|properties.created |Eq, ne, ge, le, gt, lt|Ascendente y descendente|
|properties.description |Eq, ne, ge, le, gt, lt||
|properties.lastModified    |Eq, ne, ge, le, gt, lt|Ascendente y descendente|
|properties.options |||
|properties.policyId    |Eq, ne||
|Tipo   |||

### <a name="pagination"></a>Paginación

La paginación se admite para cada uno de los cuatro criterios de ordenación habilitados. En este momento, el tamaño de página es 10.

> [!TIP]
> Siempre debe usar el vínculo siguiente para enumerar la colección y así no tener que depender de un tamaño de página determinado.

Si una respuesta de consulta contiene muchos elementos, el servicio devuelve una propiedad "\@odata.nextLink" para obtener la siguiente página de resultados. Esto se puede utilizar para pasar de página en el conjunto de resultados completo. No puede configurar el tamaño de página. 

Si se crean o eliminan elementos StreamingPolicy durante la paginación de la colección, los cambios se ven reflejados en los resultados que se devuelven (si esos cambios se encuentran en la parte de la colección que no se ha descargado). 

En el ejemplo de C# siguiente se muestra cómo enumerar todos los elementos ContentKeyPolicies de la cuenta.

```csharp
var firstPage = await MediaServicesArmClient.ContentKeyPolicies.ListAsync(CustomerResourceGroup, CustomerAccountName);

var currentPage = firstPage;
while (currentPage.NextPageLink != null)
{
    currentPage = await MediaServicesArmClient.ContentKeyPolicies.ListNextAsync(currentPage.NextPageLink);
}
```

Para obtener ejemplos de REST, vea [Content Key Policies - List](https://docs.microsoft.com/rest/api/media/contentkeypolicies/list) (Lista de directivas de clave de contenido)

## <a name="next-steps"></a>Pasos siguientes

[Uso del cifrado dinámico AES-128 y el servicio de entrega de claves](protect-with-aes128.md)

[Uso del cifrado dinámico de DRM y el servicio de entrega de licencias](protect-with-drm.md)
