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
ms.date: 01/01/2018
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: 8507d51f0d4d49d89fc24b38ed73df7488261daa
ms.sourcegitcommit: 803e66de6de4a094c6ae9cde7b76f5f4b622a7bb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/02/2019
ms.locfileid: "53969582"
---
# <a name="assets"></a>Recursos

En Azure Media Services, Un [recurso](https://docs.microsoft.com/rest/api/media/assets) contiene archivos digitales (como vídeos, audio, imágenes, colecciones de miniaturas, pistas de texto y subtítulos) y metadatos de estos archivos. Una vez que los archivos digitales se cargan en un recurso, se pueden usar en los flujos de trabajo de codificación, streaming y análisis de contenido de Media Services. Para obtener más información, consulte la sección [Cargar archivos digitales en los recursos](#upload-digital-files-into-assets).

Un recurso se asigna a un contenedor de blobs en la [cuenta de Azure Storage](storage-account-concept.md) y los archivos del recurso se almacenan como blobs en bloques en ese contenedor. Azure Media Services admite los niveles de blob cuando la cuenta usa el almacenamiento de uso general v2 (GPv2). Con GPv2, puede mover los archivos al [almacenamiento de acceso esporádico o al almacenamiento en frío](https://docs.microsoft.com/azure/storage/blobs/storage-blob-storage-tiers). El almacenamiento de **archivos** es adecuado para guardar archivos de origen cuando ya no son necesarios (por ejemplo, una vez codificados).

El nivel de almacenamiento de **archivos** solo se recomienda para archivos de origen muy grandes que ya se hayan codificado y cuya salida del trabajo de codificación se haya colocado en un contenedor de blobs de salida. Los blobs del contenedor de salida que quiera asociar con un recurso y usar para hacer streaming o analizar contenido, deben existir en un nivel de almacenamiento **frecuente** o **esporádico**.

## <a name="asset-definition"></a>Definición de recursos

En la tabla siguiente se muestran las propiedades de los recursos y se proporcionan sus definiciones.

|NOMBRE|Descripción|
|---|---|
|id|Identificador de recurso completo del recurso.|
|Nombre|Nombre del recurso.|
|properties.alternateId |Id. alternativo del recurso.|
|properties.assetId |Identificador del recurso.|
|properties.container |Nombre del contenedor de blobs del recurso.|
|properties.created |Fecha de creación del recurso.<br/> La fecha y la hora siempre están en formato UTC.|
|properties.description|Descripción del recurso.|
|properties.lastModified |Fecha de última modificación del recurso. <br/> La fecha y la hora siempre están en formato UTC.|
|properties.storageAccountName |El nombre de la cuenta de almacenamiento.|
|properties.storageEncryptionFormat |Formato de cifrado del recurso. Uno entre Ninguno y MediaStorageEncryption.|
|Tipo|Tipo de recurso.|

Para obtener la definición completa, consulte [Assets](https://docs.microsoft.com/rest/api/media/assets) (Recursos).

## <a name="upload-digital-files-into-assets"></a>Cargar los archivos digitales en recursos

Uno de los flujos de trabajo más comunes de Media Services es cargar, codificar y hacer streaming de un archivo. En esta sección se describen los pasos generales.

1. Use la API de Media Services v3 para crear un nuevo recurso de "entrada". Esta operación crea un contenedor en la cuenta de almacenamiento asociada a su cuenta de Media Services. La API devuelve el nombre del contenedor (por ejemplo, `"container": "asset-b8d8b68a-2d7f-4d8c-81bb-8c7bbbe67ee4"`).
   
    Si ya tiene un contenedor de blobs que quiera asociar a un recurso, puede especificar el nombre del contenedor al crear ese recurso. Media Services actualmente solo admite blobs en la raíz del contenedor y que no tengan las rutas de acceso en el nombre de archivo. Por lo tanto, un contenedor con el nombre de archivo "input.mp4" funcionará perfectamente. Sin embargo, un contenedor con el nombre de archivo "videos/inputs/input.mp4", no funcionará.

    Puede usar la CLI de Azure para cargar contenido directamente en cualquier cuenta de almacenamiento y de contenedor sobre la que tenga derechos en su suscripción. <br/>El nombre del contenedor debe ser único y ha de seguir las directrices de nomenclatura de almacenamiento. El nombre no tiene que seguir el formato de nomenclatura para los contenedores de recursos de Media Services (GUID de recurso). 
    
    ```azurecli
    az storage blob upload -f /path/to/file -c MyContainer -n MyBlob
    ```
2. Obtenga una dirección URL de SAS con permisos de lectura y escritura que podrá usar para cargar archivos digitales en el contenedor de recursos. Puede usar la API de Media Services para [enumerar las direcciones URL del contenedor de recursos](https://docs.microsoft.com/rest/api/media/assets/listcontainersas).
3. Use los SDK o API de Azure Storage (por ejemplo, la [API REST de Storage](../../storage/common/storage-rest-api-auth.md), el [SDK para JAVA](../../storage/blobs/storage-quickstart-blobs-java-v10.md) o el [SDK para .NET](../../storage/blobs/storage-quickstart-blobs-dotnet.md)) para cargar archivos en el contenedor de recursos. 
4. Use las API de Media Services v3 para crear una transformación y un trabajo para procesar el recurso de "entrada". Para obtener más información, consulte [Transformaciones y trabajos](transform-concept.md).
5. Haga streaming del contenido del recurso de "salida".

> [!TIP]
> Para obtener un ejemplo de .NET completo que muestre cómo crear el recurso, obtener una dirección URL de SAS que se pueda escribir en el contenedor del recurso de almacenamiento, cargar el archivo en el contenedor de almacenamiento mediante la dirección URL de SAS, consulte [Creación de una entrada de trabajo a partir de un archivo local](job-input-from-local-file-how-to.md).

### <a name="create-a-new-asset"></a>Crear un recurso nuevo

#### <a name="rest"></a>REST

```
PUT https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Media/mediaServices/{amsAccountName}/assets/{assetName}?api-version=2018-07-01
```

Para obtener un ejemplo de REST, consulte el ejemplo para [crear un recurso con REST](https://docs.microsoft.com/rest/api/media/assets/createorupdate#examples).

En el ejemplo se muestra cómo crear el **cuerpo de la solicitud** donde puede especificar información útil, como la descripción, el nombre del contenedor, la cuenta de almacenamiento y otros datos similares.

#### <a name="curl"></a>cURL

```cURL
curl -X PUT \
  'https://management.azure.com/subscriptions/00000000-0000-0000-000000000000/resourceGroups/resourceGroupName/providers/Microsoft.Media/mediaServices/amsAccountName/assets/myOutputAsset?api-version=2018-07-01' \
  -H 'Accept: application/json' \
  -H 'Content-Type: application/json' \
  -d '{
  "properties": {
    "description": "",
  }
}'
```

#### <a name="net"></a>.NET

```csharp
 Asset asset = await client.Assets.CreateOrUpdateAsync(resourceGroupName, accountName, assetName, new Asset());
```

Para obtener un ejemplo completo, consulte [Creación de una entrada de trabajo a partir de un archivo local](job-input-from-local-file-how-to.md). En Media Services v3, también se puede crear una entrada de trabajo a partir de las direcciones URL de HTTPS (consulte [Creación de una entrada de trabajo a partir de un una dirección URL de HTTPS](job-input-from-http-how-to.md)).

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
|Nombre|Admite: Eq, Gt, Lt|Admite: Ascendente y descendente|
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

[Diferencias entre la versión v2 y v3 de Media Services](migrate-from-v2-to-v3.md)
