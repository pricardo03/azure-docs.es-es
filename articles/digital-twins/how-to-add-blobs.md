---
title: Cómo agregar blobs a objetos en Azure Digital Twins | Microsoft Docs
description: Instrucciones para agregar blobs a objetos en Azure Digital Twins
author: kingdomofends
manager: alinast
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 11/13/2018
ms.author: adgera
ms.openlocfilehash: 0929a4a63eee35d21723c980887d6b4217898682
ms.sourcegitcommit: db2cb1c4add355074c384f403c8d9fcd03d12b0c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/15/2018
ms.locfileid: "51688337"
---
# <a name="how-to-add-blobs-to-objects-in-azure-digital-twins"></a>Cómo agregar blobs a objetos en Azure Digital Twins

Los blobs son representaciones no estructuradas de tipos de archivo comunes (por ejemplo, imágenes y registros). Los blobs realizan un seguimiento del tipo de datos que representan mediante un tipo MIME (por ejemplo: "image/jpeg") y los metadatos (nombre, descripción, tipo, etc.).

Azure Digital Twins admite la conexión de blobs a dispositivos, espacios y usuarios. Los blobs pueden representar una imagen de perfil de un usuario, una foto de dispositivo, un vídeo, un mapa o un registro.

> [!NOTE]
> En este artículo se da por hecho que:
> * La instancia está configurada correctamente para recibir solicitudes de la API de administración.
> * La autenticación se ha realizado correctamente con un cliente de REST de su elección.

## <a name="uploading-blobs-an-overview"></a>Carga de blobs: introducción

Las solicitudes de varias partes se usan para cargar blobs en puntos de conexión específicos y sus respectivas funcionalidades.

> [!IMPORTANT]
> Las solicitudes de varias partes requieren tres elementos esenciales de información. Para Azure Digital Twins:
> * Un encabezado **Content-Type**:
>   * `application/json; charset=utf-8`
>   * `multipart/form-data; boundary="USER_DEFINED_BOUNDARY"`.
> * Un elemento **Content-Disposition**: `form-data; name="metadata"`.
> * El contenido del archivo que se va a cargar.
>
> Los elementos **Content-Type** y **Content-Disposition** exactos pueden variar en función del escenario de uso.

Cada solicitud de varias partes se divide en varios fragmentos. Las solicitudes de varias partes realizadas a las API de Azure Digital Twins se dividen en **dos** (**2**) partes de este tipo:

1. La primera parte es necesaria y contiene los metadatos del blob, como un tipo MIME asociado para los elementos **Content-Type** y **Content-Disposition** anteriores.

1. La segunda parte incluye el contenido real del blob (el contenido no estructurado del archivo).  

Ninguno de los dos elementos es necesario para las solicitudes **PATCH**. Ambos son necesarios para las operaciones de creación o **POST**.

### <a name="blob-metadata"></a>Metadatos de blob

Además de **Content-Type** y **Content-Disposition**, las solicitudes de varias partes también deben especificar el cuerpo JSON correcto. El cuerpo JSON que se enviará depende del tipo de operación de solicitud HTTP que se esté realizando.

Los cuatro esquemas JSON principales utilizados son:

![Blobs de espacio][1]

Estos esquemas de modelo se describen con todo detalle en la documentación de Swagger proporcionada.

[!INCLUDE [Digital Twins Swagger](../../includes/digital-twins-swagger.md)]

Para obtener información sobre el uso de la documentación de referencia proporcionada, lea el artículo sobre [cómo usar Swagger](./how-to-use-swagger.md).

### <a name="examples"></a>Ejemplos

[!INCLUDE [Digital Twins Management API](../../includes/digital-twins-management-api.md)]

Para realizar una solicitud POST que cargue un archivo de texto como un blob y lo asocie con un espacio:

```plaintext
POST YOUR_MANAGEMENT_API_URL/spaces/blobs HTTP/1.1
Content-Type: multipart/form-data; boundary="USER_DEFINED_BOUNDARY"

--USER_DEFINED_BOUNDARY
Content-Type: application/json; charset=utf-8
Content-Disposition: form-data; name="metadata"

{
  "ParentId": "54213cf5-285f-e611-80c3-000d3a320e1e",
  "Name": "My First Blob",
  "Type": "Map",
  "SubType": "GenericMap",
  "Description": "A well chosen description",
  "Sharing": "None"
}
--USER_DEFINED_BOUNDARY
Content-Disposition: form-data; name="contents"; filename="myblob.txt"
Content-Type: text/plain

This is my blob content. In this case, some text, but I could also be uploading a picture, an JSON file, a firmware zip, etc.

--USER_DEFINED_BOUNDARY--
```

| Valor del parámetro | Reemplazar por |
| --- | --- |
| *USER_DEFINED_BOUNDARY* | Nombre de un límite de contenido de varias partes |

A continuación, se proporciona una implementación de .NET de la misma carga de blobs mediante la clase [MultipartFormDataContent](https://docs.microsoft.com/dotnet/api/system.net.http.multipartformdatacontent):

```csharp
//Supply your metaData in a suitable format
var multipartContent = new MultipartFormDataContent("USER_DEFINED_BOUNDARY");

var metadataContent = new StringContent(JsonConvert.SerializeObject(metaData), Encoding.UTF8, "application/json");
metadataContent.Headers.ContentType = MediaTypeHeaderValue.Parse("application/json; charset=utf-8");
multipartContent.Add(metadataContent, "metadata");

var fileContents = new StringContent("MY_BLOB.txt");
fileContents.Headers.ContentType = MediaTypeHeaderValue.Parse("text/plain");
multipartContent.Add(fileContents, "contents");

var response = await httpClient.PostAsync("spaces/blobs", multipartContent);
```

## <a name="api-endpoints"></a>Puntos de conexión de API

A continuación, se proporciona un tutorial sobre los puntos de conexión principales y sus funciones específicas.

### <a name="devices"></a>Dispositivos

Los blobs se pueden conectar a dispositivos. La imagen siguiente (que representan la documentación de referencia de Swagger para las API de administración) especifica los puntos de conexión de API relacionados con el dispositivo para el uso de blobs y todos los parámetros de ruta de acceso necesarios que se pasarán:

![Blobs de dispositivo][2]

Por ejemplo, para actualizar o crear un blob y asociarlo a un dispositivo, se realiza una solicitud PATCH a:

```plaintext
YOUR_MANAGEMENT_API_URL/devices/blobs/YOUR_BLOB_ID
```

| Parámetro | Reemplazar por |
| --- | --- |
| *YOUR_BLOB_ID* | Identificador de blob deseado |

Las solicitudes correctas devolverán un objeto JSON DeviceBlob en la respuesta. Los objetos DeviceBlob se ajustan al esquema JSON siguiente:

| Atributo | Escriba | DESCRIPCIÓN | Ejemplos |
| --- | --- | --- | --- |
| **DeviceBlobType** | string | Categoría de blob que se puede conectar a un dispositivo. | `Model` y `Specification` |
| **DeviceBlobSubtype*** | string | Subcategoría de blob más detallada que DeviceBlobType. | `PhysicalModel`, `LogicalModel`, `KitSpecification` y `FunctionalSpecification` |

> [!TIP]
> Utilice la tabla anterior para controlar los datos de solicitud devueltos correctamente.

### <a name="spaces"></a>Espacios

Los blobs también pueden conectarse a espacios. La imagen siguiente enumera todos los puntos de conexión de la API de espacio responsables de controlar los blobs, junto con los parámetros de ruta de acceso que se pasarán:

![Blobs de espacio][3]

Por ejemplo, para devolver un blob conectado a un espacio, realice una solicitud GET a:

```plaintext
YOUR_MANAGEMENT_API_URL/spaces/blobs/YOUR_BLOB_ID
```

| Parámetro | Reemplazar por |
| --- | --- |
| *YOUR_BLOB_ID* | Identificador de blob deseado |

Realizar la solicitud PATCH al mismo punto de conexión le permitirá actualizar una descripción de metadatos y crear una nueva versión del blob. La solicitud HTTP se realiza mediante el método PATCH, junto con los metadatos y los datos de formulario de varias partes necesarios.

Las operaciones correctas devolverán un elemento SpaceBlob que se ajustará al esquema siguiente y que podrá usarse para consumir los datos devueltos:

| Atributo | Escriba | DESCRIPCIÓN | Ejemplos |
| --- | --- | --- | --- |
| **SpaceBlobType** | string | Categoría de blob que se puede conectar a un espacio. | `Map` y `Image` |
| **SpaceBlobSubtype** | string | Subcategoría de blob más detallada que SpaceBlobType. | `GenericMap`, `ElectricalMap`, `SatelliteMap` y `WayfindingMap` |

### <a name="users"></a>Usuarios

Los blobs se pueden conectar a modelos de usuario (por ejemplo, asociar una imagen de perfil). La imagen siguiente muestra los puntos de conexión de API de usuarios pertinentes y los parámetros de la ruta de acceso necesaria, como un `id`:

![Blobs de usuario][4]

Por ejemplo, para capturar un blob conectado a un usuario, realice una solicitud GET con todos los datos de formulario necesarios a:

```plaintext
YOUR_MANAGEMENT_API_URL/users/blobs/YOUR_BLOB_ID
```

| Parámetro | Reemplazar por |
| --- | --- |
| *YOUR_BLOB_ID* | Identificador de blob deseado |

El JSON devuelto (UserBlobs) se ajustará a los siguientes modelos JSON:

| Atributo | Escriba | DESCRIPCIÓN | Ejemplos |
| --- | --- | --- | --- |
| **UserBlobType** | string | Categoría de blob que se puede conectar a un usuario. | `Image` y `Video` |
| **UserBlobSubtype** |  string | Subcategoría de blob más detallada que UserBlobType. | `ProfessionalImage`, `VacationImage` y `CommercialVideo` |

## <a name="common-errors"></a>Errores comunes

No incluye la información de encabezado correcta:

```JSON
{
    "error": {
        "code": "400.600.000.000",
        "message": "Invalid media type in first section."
    }
}
```

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información acerca de la documentación de referencia de Azure Digital Twins proporcionada, lea el artículo sobre el [uso de Azure Digital Twins](how-to-use-swagger.md).

<!-- Images -->
[1]: media/how-to-add-blobs/blob-models.PNG
[2]: media/how-to-add-blobs/blobs-device-api.PNG
[3]: media/how-to-add-blobs/blobs-space-api.PNG
[4]: media/how-to-add-blobs/blobs-users-api.PNG
