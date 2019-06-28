---
title: Cómo agregar blobs a objetos en Azure Digital Twins | Microsoft Docs
description: Descubra cómo agregar blobs a objetos en Azure Digital Twins.
author: kingdomofends
manager: alinast
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 06/05/2019
ms.author: v-adgera
ms.custom: seodec18
ms.openlocfilehash: 9490772226ecdb90cdd2e0b98fe8336b91db6044
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "66754507"
---
# <a name="add-blobs-to-objects-in-azure-digital-twins"></a>Incorporación de blobs a objetos en Azure Digital Twins

Los blobs son representaciones no estructuradas de tipos de archivo comunes (por ejemplo, imágenes y registros). Los blobs realizan un seguimiento del tipo de datos que representan mediante un tipo MIME (por ejemplo: "image/jpeg") y los metadatos (nombre, descripción, tipo, etc.).

Azure Digital Twins admite la conexión de blobs a dispositivos, espacios y usuarios. Los blobs pueden representar una imagen de perfil de un usuario, una foto de dispositivo, un vídeo, un mapa, un archivo zip de firmware, datos de JSON, un registro, etc.

[!INCLUDE [Digital Twins Management API familiarity](../../includes/digital-twins-familiarity.md)]

## <a name="uploading-blobs-overview"></a>Introducción a la carga de blobs

Las solicitudes de varias partes se usan para cargar blobs en puntos de conexión específicos y sus respectivas funcionalidades.

[!INCLUDE [Digital Twins multipart requests](../../includes/digital-twins-multipart.md)]

### <a name="blob-metadata"></a>Metadatos de blob

Además de **Content-Type** y **Content-Disposition**, las solicitudes de varias partes de blobs de Azure Digital Twins deben especificar el cuerpo JSON correcto. El cuerpo JSON que se enviará depende del tipo de operación de solicitud HTTP que se esté realizando.

Los cuatro esquemas JSON principales utilizados son:

[![Esquemas JSON](media/how-to-add-blobs/blob-models.PNG)](media/how-to-add-blobs/blob-models.PNG#lightbox)

Los metadatos del blob JSON se ajustan al modelo siguiente:

```JSON
{
    "parentId": "00000000-0000-0000-0000-000000000000",
    "name": "My First Blob",
    "type": "Map",
    "subtype": "GenericMap",
    "description": "A well chosen description",
    "sharing": "None"
  }
```

| Atributo | Type | DESCRIPCIÓN |
| --- | --- | --- |
| **parentId** | Cadena | Entidad primaria con la que se asocia el blob (espacios, dispositivos o usuarios) |
| **name** |Cadena | Nombre fácil de usar para el blob |
| **type** | Cadena | Tipo de blob, no se puede usar *type* ni *typeId*  |
| **typeId** | Entero | Id. de tipo de blob, no se puede usar *type* ni *typeId* |
| **subtype** | Cadena | Subtipo de blob, no se puede usar *subtype* ni *subtypeId* |
| **subtypeId** | Entero | Id. de subtipo del blob, no se puede usar *subtype* ni *subtypeId* |
| **descripción** | Cadena | Descripción personalizada del blob |
| **sharing** | Cadena | Si el blob se puede compartir, enum [`None`, `Tree`, `Global`] |

Los metadatos del blob siempre se proporcionan como el primer fragmento con **Content-Type** `application/json` o como archivo `.json`. Los datos de archivos se proporcionan en el segundo fragmento y pueden ser de cualquier tipo MIME admitido.

La documentación de Swagger describe estos esquemas de modelo con detalle.

[!INCLUDE [Digital Twins Swagger](../../includes/digital-twins-swagger.md)]

Para obtener información sobre el uso de la documentación de referencia, lea el artículo sobre [cómo usar Swagger](./how-to-use-swagger.md).

<div id="blobModel"></div>

### <a name="blobs-response-data"></a>Datos de respuesta de blobs

Los blobs devueltos individualmente cumplen con el esquema JSON siguiente:

```JSON
{
  "id": "00000000-0000-0000-0000-000000000000",
  "name": "string",
  "parentId": "00000000-0000-0000-0000-000000000000",
  "type": "string",
  "subtype": "string",
  "typeId": 0,
  "subtypeId": 0,
  "sharing": "None",
  "description": "string",
  "contentInfos": [
    {
      "type": "string",
      "sizeBytes": 0,
      "mD5": "string",
      "version": "string",
      "lastModifiedUtc": "2019-01-12T00:58:08.689Z",
      "metadata": {
        "additionalProp1": "string",
        "additionalProp2": "string",
        "additionalProp3": "string"
      }
    }
  ],
  "fullName": "string",
  "spacePaths": [
    "string"
  ]
}
```

| Atributo | Type | DESCRIPCIÓN |
| --- | --- | --- |
| **id** | Cadena | Identificador único para el blob |
| **name** |Cadena | Nombre fácil de usar para el blob |
| **parentId** | Cadena | Entidad primaria con la que se asocia el blob (espacios, dispositivos o usuarios) |
| **type** | Cadena | Tipo de blob, no se puede usar *type* ni *typeId*  |
| **typeId** | Entero | Id. de tipo de blob, no se puede usar *type* ni *typeId* |
| **subtype** | Cadena | Subtipo de blob, no se puede usar *subtype* ni *subtypeId* |
| **subtypeId** | Entero | Id. de subtipo del blob, no se puede usar *subtype* ni *subtypeId* |
| **sharing** | Cadena | Si el blob se puede compartir, enum [`None`, `Tree`, `Global`] |
| **descripción** | Cadena | Descripción personalizada del blob |
| **contentInfos** | Matriz | Especifica la información de metadatos sin estructurar, incluida la versión |
| **fullName** | Cadena | Nombre completo del blob |
| **spacePaths** | Cadena | Ruta de acceso al espacio |

Los metadatos del blob siempre se proporcionan como el primer fragmento con **Content-Type** `application/json` o como archivo `.json`. Los datos de archivos se proporcionan en el segundo fragmento y pueden ser de cualquier tipo MIME admitido.

### <a name="blob-multipart-request-examples"></a>Ejemplos de solicitud de varias partes del blob

[!INCLUDE [Digital Twins Management API](../../includes/digital-twins-management-api.md)]

Para cargar un archivo de texto como blob y asociarlo a un espacio, realice una solicitud HTTP POST autenticada a:

```plaintext
YOUR_MANAGEMENT_API_URL/spaces/blobs
```

Con el siguiente cuerpo:

```plaintext
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

| Valor | Reemplazar por |
| --- | --- |
| USER_DEFINED_BOUNDARY | Nombre de un límite de contenido de varias partes |

El siguiente código es una implementación de .NET de la misma carga de blobs mediante la clase [MultipartFormDataContent](https://docs.microsoft.com/dotnet/api/system.net.http.multipartformdatacontent):

```csharp
//Supply your metadata in a suitable format
var multipartContent = new MultipartFormDataContent("USER_DEFINED_BOUNDARY");

var metadataContent = new StringContent(JsonConvert.SerializeObject(metaData), Encoding.UTF8, "application/json");
metadataContent.Headers.ContentType = MediaTypeHeaderValue.Parse("application/json; charset=utf-8");
multipartContent.Add(metadataContent, "metadata");

//MY_BLOB.txt is the String representation of your text file
var fileContents = new StringContent("MY_BLOB.txt");
fileContents.Headers.ContentType = MediaTypeHeaderValue.Parse("text/plain");
multipartContent.Add(fileContents, "contents");

var response = await httpClient.PostAsync("spaces/blobs", multipartContent);
```

Por último, los usuarios de [cURL](https://curl.haxx.se/) pueden hacer solicitudes de formulario de varias partes de la misma manera:

[![Blobs de dispositivo](media/how-to-add-blobs/curl.PNG)](media/how-to-add-blobs/curl.PNG#lightbox)

```bash
curl
 -X POST "YOUR_MANAGEMENT_API_URL/spaces/blobs"
 -H "Authorization: Bearer YOUR_TOKEN"
 -H "Accept: application/json"
 -H "Content-Type: multipart/form-data"
 -F "meta={\"ParentId\": \"YOUR_SPACE_ID\",\"Name\":\"My CURL Blob",\"Type\":\"Map\",\"SubType\":\"GenericMap\",\"Description\": \"A well chosen description\", \"Sharing\": \"None\"};type=application/json"
 -F "text=PATH_TO_FILE;type=text/plain"
```

| Valor | Reemplazar por |
| --- | --- |
| YOUR_TOKEN | El token de OAuth 2.0 válido |
| YOUR_SPACE_ID | El identificador del espacio con el cual asociar el blob |
| PATH_TO_FILE | La ruta de acceso al archivo de texto |

Una solicitud POST correcta devuelve el identificador del nuevo blob (resaltado en rojo anteriormente).

## <a name="api-endpoints"></a>Puntos de conexión de API

Las siguientes secciones describen los principales puntos de conexión de API relacionados con el blob y sus funcionalidades.

### <a name="devices"></a>Dispositivos

Puede conectar blobs a dispositivos. La siguiente imagen muestra la documentación de referencia de Swagger para las API de administración. Especifica los puntos de conexión de API relacionados con los dispositivos para el consumo de blobs y los parámetros de ruta de acceso necesarios que se pasarán a dichos puntos de conexión.

[![Blobs de dispositivo](media/how-to-add-blobs/blobs-device-api.PNG)](media/how-to-add-blobs/blobs-device-api.PNG#lightbox)

Por ejemplo, para actualizar o crear un blob y conectarlo a un dispositivo, realice una solicitud HTTP PATCH autenticada a:

```plaintext
YOUR_MANAGEMENT_API_URL/devices/blobs/YOUR_BLOB_ID
```

| Parámetro | Reemplazar por |
| --- | --- |
| *YOUR_BLOB_ID* | Identificador de blob deseado |

Las solicitudes correctas devuelven un objeto JSON como [se ha descrito anteriormente](#blobModel).

### <a name="spaces"></a>Espacios

También puede conectar blobs a espacios. En la imagen siguiente se enumeran todos los puntos de conexión de API de espacio responsables de controlar los blobs. También muestra los parámetros de ruta de acceso que se van a pasar a esos puntos de conexión.

[![Blobs de espacio](media/how-to-add-blobs/blobs-space-api.PNG)](media/how-to-add-blobs/blobs-space-api.PNG#lightbox)

Por ejemplo, para devolver un blob conectado a un espacio, realice una solicitud HTTP GET autenticada a:

```plaintext
YOUR_MANAGEMENT_API_URL/spaces/blobs/YOUR_BLOB_ID
```

| Parámetro | Reemplazar por |
| --- | --- |
| *YOUR_BLOB_ID* | Identificador de blob deseado |

Las solicitudes correctas devuelven un objeto JSON como [se ha descrito anteriormente](#blobModel).

Una solicitud PATCH al mismo punto de conexión actualiza las descripciones de metadatos y crea versiones del blob. La solicitud HTTP se realiza mediante el método PATCH, con los metadatos y los datos de formulario de varias partes necesarios.

### <a name="users"></a>Usuarios

Puede conectar blobs a modelos de usuario (por ejemplo, para asociar una imagen de perfil). En la imagen siguiente se muestran los puntos de conexión de API de usuarios pertinentes y los parámetros de ruta de acceso necesarios, como `id`:

[![Blobs de usuario](media/how-to-add-blobs/blobs-users-api.PNG)](media/how-to-add-blobs/blobs-users-api.PNG#lightbox)

Por ejemplo, para capturar un blob conectado a un usuario, realice una solicitud HTTP GET autenticada con todos los datos de formulario necesarios a:

```plaintext
YOUR_MANAGEMENT_API_URL/users/blobs/YOUR_BLOB_ID
```

| Parámetro | Reemplazar por |
| --- | --- |
| *YOUR_BLOB_ID* | Identificador de blob deseado |

Las solicitudes correctas devuelven un objeto JSON como [se ha descrito anteriormente](#blobModel).

## <a name="common-errors"></a>Errores comunes

Un error común incluye no suministrar la información de encabezado correcta:

```JSON
{
    "error": {
        "code": "400.600.000.000",
        "message": "Invalid media type in first section."
    }
}
```

Para resolver este error, verifique que la solicitud general tenga un encabezado **Content-Type** adecuado:

* `multipart/mixed`
* `multipart/form-data`

Además, verifique que cada fragmento de varias partes tenga un **Content-Type** correspondiente, según sea necesario.

## <a name="next-steps"></a>Pasos siguientes

- Para obtener más información sobre la documentación de referencia de Swagger para Azure Digital Twins, lea [Uso de Azure Digital Twins Swagger](how-to-use-swagger.md).

- Para cargar blobs a través de Postman, lea [Configuración de Postman](./how-to-configure-postman.md).