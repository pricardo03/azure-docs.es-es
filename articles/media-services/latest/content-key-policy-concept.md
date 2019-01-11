---
title: 'Directivas de clave de contenido de Media Services: Azure | Microsoft Docs'
description: En este artículo se explica qué son las directivas de clave de contenido y cómo las usa Azure Media Services.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 12/20/2018
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: f12632b20d516c81e21a50cfdda7e40d4163afc1
ms.sourcegitcommit: 21466e845ceab74aff3ebfd541e020e0313e43d9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/21/2018
ms.locfileid: "53742225"
---
# <a name="content-key-policies"></a>Directivas de clave de contenido

Puede usar Azure Media Services para proteger su contenido multimedia desde el momento en que este deja el equipo y pasa a través del almacenamiento, el procesamiento y la entrega. Con Media Services puede entregar el contenido cifrado de forma dinámica en vivo y a petición con el Estándar de cifrado avanzado (AES-128) o cualquiera de los tres sistemas de administración de derechos digitales (DRM) principales: Microsoft PlayReady, Google Widevine y Apple FairPlay. Media Services también proporciona un servicio para entregar claves AES y licencias de DMR (PlayReady, Widevine y FairPlay) a los clientes autorizados.

En Azure Media Services v3, una [directiva de clave de contenido](https://docs.microsoft.com/rest/api/media/contentkeypolicies) permite especificar cómo se entrega la clave de contenido a los clientes finales mediante el componente Media Services Key Delivery. Para obtener más información, vea [Introducción a la protección de contenido](content-protection-overview.md).

Se recomienda volver a usar el mismo elemento ContentKeyPolicy para todos los activos. ContentKeyPolicy se puede actualizar, por lo que si desea realizar una rotación de claves, después puede agregar un nuevo elemento ContentKeyPolicyOption al ContentKeyPolicy existente con una restricción de token con las nuevas claves. O bien, puede actualizar la clave de verificación principal y la lista de claves de verificación alternativas de la directiva y la opción existente. Puede tardar hasta 15 minutos para que las memorias caché de entrega de claves se actualicen y recojan la directiva actualizada.

## <a name="contentkeypolicy-definition"></a>Definición de ContentKeyPolicy

En la tabla siguiente se muestran las propiedades de las directivas de clave de contenido y se proporcionan sus definiciones.

|NOMBRE|Descripción|
|---|---|
|id|Identificador de recurso completo del recurso.|
|Nombre|Nombre del recurso.|
|properties.created |Fecha de creación de la directiva|
|properties.description |Descripción de la directiva.|
|properties.lastModified|Fecha de la última modificación de la directiva|
|properties.options |Opciones de la directiva de clave.|
|properties.policyId|Identificador de directiva heredado.|
|Tipo|Tipo de recurso.|

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

En la tabla siguiente se muestra cómo pueden aplicarse estas opciones a las propiedades de ContentKeyPolicy: 

|NOMBRE|Filtrar|Orden|
|---|---|---|
|id|||
|Nombre|Eq, ne, ge, le, gt, lt|Ascendente y descendente|
|properties.created |Eq, ne, ge, le, gt, lt|Ascendente y descendente|
|properties.description |Eq, ne, ge, le, gt, lt||
|properties.lastModified|Eq, ne, ge, le, gt, lt|Ascendente y descendente|
|properties.options |||
|properties.policyId|Eq, ne||
|Tipo|||

### <a name="pagination"></a>Paginación

La paginación se admite para cada uno de los cuatro criterios de ordenación habilitados. En este momento, el tamaño de página es 10.

> [!TIP]
> Siempre debe usar el vínculo siguiente para enumerar la colección y así no tener que depender de un tamaño de página determinado.

Si una respuesta de consulta contiene muchos elementos, el servicio devuelve una propiedad "\@odata.nextLink" para obtener la siguiente página de resultados. Esto se puede utilizar para pasar de página en el conjunto de resultados completo. No puede configurar el tamaño de página. 

Si se crean o eliminan ContentKeyPolicy durante la paginación a través de la colección, los cambios se ven reflejados en los resultados que se devuelven (si esos cambios se encuentran en la parte de la colección que no se ha descargado). 

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
