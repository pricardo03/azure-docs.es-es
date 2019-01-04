---
title: Recursos en Azure Media Services | Microsoft Docs
description: En este artículo se explica qué son los artículos y cómo los usa Azure Media Services.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 12/08/2018
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: f9a6f0963ce8f45da567bb4f6326e9fcc8f435ef
ms.sourcegitcommit: 78ec955e8cdbfa01b0fa9bdd99659b3f64932bba
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2018
ms.locfileid: "53140139"
---
# <a name="assets"></a>Recursos

Un **recurso** contiene archivos digitales (como vídeos, audio, imágenes, colecciones de miniaturas, pistas de texto y subtítulos) y metadatos de estos archivos. Una vez que los archivos digitales se cargan en un recurso, se pueden utilizar en los flujos de trabajo de codificación y streaming en Media Services.

Un recurso se asigna a un contenedor de blobs en la [cuenta de Azure Storage](storage-account-concept.md) y los archivos del recurso se almacenan como blobs en bloques en ese contenedor. Puede interactuar con los archivos del recurso en los contenedores mediante los clientes del SDK de Storage.

Azure Media Services admite los niveles de blob cuando la cuenta usa el almacenamiento de uso general v2 (GPv2). Con GPv2, puede mover los archivos al almacenamiento de acceso esporádico o al almacenamiento en frío. El almacenamiento en frío es adecuado para guardar archivos de origen cuando ya no son necesarios (por ejemplo, una vez codificados).

En Media Services v3, la entrada de trabajo se puede crear a partir de recursos o desde direcciones URL de HTTP(s). Para crear un recurso que se puede usar como una entrada para el trabajo, consulte [Creación de una entrada de trabajo a partir de un archivo local](job-input-from-local-file-how-to.md).

Además, lea sobre las [cuentas de almacenamiento en Media Services](storage-account-concept.md) y sobre las [transformaciones y trabajos](transform-concept.md).

## <a name="asset-definition"></a>Definición de recursos

En la tabla siguiente se muestran las propiedades de los recursos y se proporcionan sus definiciones.

|NOMBRE|Descripción|
|---|---|
|id|Identificador de recurso completo del recurso.|
|Nombre|Nombre del recurso.|
|properties.alternateId |Id. alternativo del recurso.|
|properties.assetId |Identificador del recurso.|
|properties.container |Nombre del contenedor de blobs del recurso.|
|properties.created |Fecha de creación del recurso.|
|properties.description|Descripción del recurso.|
|properties.lastModified |Fecha de última modificación del recurso.|
|properties.storageAccountName |El nombre de la cuenta de almacenamiento.|
|properties.storageEncryptionFormat |Formato de cifrado del recurso. Uno entre Ninguno y MediaStorageEncryption.|
|Tipo|Tipo de recurso.|

Para conocer la definición completa, consulte [Assets](https://docs.microsoft.com/rest/api/media/assets) (Recursos).

## <a name="filtering-ordering-paging"></a>Filtrado, ordenación, paginación

Media Services admite las siguientes opciones de consulta de OData para los recursos: 

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

En la tabla siguiente se muestra cómo pueden aplicarse estas opciones a las propiedades del recurso: 

|NOMBRE|Filtrar|Orden|
|---|---|---|
|id|||
|Nombre|Admite: Eq, Gt, Lt|Es compatible con: Ascendente y descendente|
|properties.alternateId |Admite: Eq||
|properties.assetId |Admite: Eq||
|properties.container |||
|properties.created|Admite: Eq, Gt, Lt| Admite: Ascendente y descendente|
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

> [!TIP]
> Siempre debe usar el vínculo siguiente para enumerar la colección y así no tener que depender de un tamaño de página determinado.

Si una respuesta de consulta contiene muchos elementos, el servicio devuelve una propiedad "\@odata.nextLink" para obtener la siguiente página de resultados. Esto se puede utilizar para pasar de página en el conjunto de resultados completo. No puede configurar el tamaño de página. 

Si se crean o eliminan recursos durante la paginación a través de la colección, los cambios se ven reflejados en los resultados que se devuelven (si esos cambios se encuentran en la parte de la colección que no se ha descargado). 

En el ejemplo de C# siguiente se muestra cómo enumerar todos los recursos de la cuenta.

```csharp
var firstPage = await MediaServicesArmClient.Assets.ListAsync(CustomerResourceGroup, CustomerAccountName);

var currentPage = firstPage;
while (currentPage.NextPageLink != null)
{
    currentPage = await MediaServicesArmClient.Assets.ListNextAsync(currentPage.NextPageLink);
}
```

Para obtener ejemplos de REST, consulte [Assets - List](https://docs.microsoft.com/rest/api/media/assets/list) (Recursos: lista)

## <a name="storage-side-encryption"></a>Cifrado del lado de almacenamiento

Para proteger los recursos en reposo, estos se deben cifrar mediante el cifrado del lado de almacenamiento. En la tabla siguiente se muestra cómo funciona el cifrado del lado de almacenamiento en Media Services:

|Opción de cifrado|DESCRIPCIÓN|Media Services v2|Media Services v3|
|---|---|---|---|
|Cifrado de almacenamiento en Media Services|Cifrado de AES-256, clave administrada por Media Services|Admitido<sup>(1)</sup>|No admitido<sup>(2)</sup>|
|[Storage Service Encryption para datos en reposo](https://docs.microsoft.com/azure/storage/common/storage-service-encryption)|Cifrado en el lado del servidor que ofrece Azure Storage, clave administrada por Azure o por el cliente|Compatible|Compatible|
|[Cifrado en el lado de cliente de almacenamiento](https://docs.microsoft.com/azure/storage/common/storage-client-side-encryption)|Cifrado en el lado de cliente que ofrece Azure Storage, clave administrada por el cliente en Key Vault|No compatible|No compatible|

<sup>1</sup> Aunque Media Services admite el control del contenido de una forma clara o sin ninguna forma de cifrado, hacerlo así no es recomendable.

<sup>2</sup> En Media Services v3, el cifrado de almacenamiento (cifrado con AES-256) solo es compatible con versiones anteriores si los recursos se crearon con Media Services v2. Esto significa que la versión v3 funciona con los recursos cifrados de almacenamiento ya existentes pero no permitirá la creación de otros nuevos.

## <a name="next-steps"></a>Pasos siguientes

[Streaming de un archivo](stream-files-dotnet-quickstart.md)
