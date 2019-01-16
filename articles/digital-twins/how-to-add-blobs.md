---
title: Cómo agregar blobs a objetos en Azure Digital Twins | Microsoft Docs
description: Descubra cómo agregar blobs a objetos en Azure Digital Twins.
author: kingdomofends
manager: alinast
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 01/02/2019
ms.author: adgera
ms.custom: seodec18
ms.openlocfilehash: 36f4caac38f2f4891af6f61b78b55c7eff15eae4
ms.sourcegitcommit: 818d3e89821d101406c3fe68e0e6efa8907072e7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/09/2019
ms.locfileid: "54116745"
---
# <a name="add-blobs-to-objects-in-azure-digital-twins"></a>Incorporación de blobs a objetos en Azure Digital Twins

Los blobs son representaciones no estructuradas de tipos de archivo comunes (por ejemplo, imágenes y registros). Los blobs realizan un seguimiento del tipo de datos que representan mediante un tipo MIME (por ejemplo: "image/jpeg") y los metadatos (nombre, descripción, tipo, etc.).

Azure Digital Twins admite la conexión de blobs a dispositivos, espacios y usuarios. Los blobs pueden representar una imagen de perfil de un usuario, una foto de dispositivo, un vídeo, un mapa, un archivo zip de firmware, datos de JSON, un registro, etc.

[!INCLUDE [Digital Twins Management API familiarity](../../includes/digital-twins-familiarity.md)]

## <a name="uploading-blobs-an-overview"></a>Carga de blobs: introducción

Las solicitudes de varias partes se usan para cargar blobs en puntos de conexión específicos y sus respectivas funcionalidades.

[!INCLUDE [Digital Twins multipart requests](../../includes/digital-twins-multipart.md)]

### <a name="blob-metadata"></a>Metadatos de blob

Además de **Content-Type** y **Content-Disposition**, las solicitudes de varias partes de blobs de Azure Digital Twins deben especificar el cuerpo JSON correcto. El cuerpo JSON que se enviará depende del tipo de operación de solicitud HTTP que se esté realizando.

Los cuatro esquemas JSON principales utilizados son:

![Esquemas JSON][1]

La documentación de Swagger describe estos esquemas de modelo con detalle.

[!INCLUDE [Digital Twins Swagger](../../includes/digital-twins-swagger.md)]

Para obtener información sobre el uso de la documentación de referencia, lea el artículo sobre [cómo usar Swagger](./how-to-use-swagger.md).

### <a name="examples"></a>Ejemplos

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

var fileContents = new StringContent("MY_BLOB.txt");
fileContents.Headers.ContentType = MediaTypeHeaderValue.Parse("text/plain");
multipartContent.Add(fileContents, "contents");

var response = await httpClient.PostAsync("spaces/blobs", multipartContent);
```

En ambos ejemplos:

1. Compruebe que los encabezados incluyan: `Content-Type: multipart/form-data; boundary="USER_DEFINED_BOUNDARY"`.
1. Compruebe que el cuerpo esté formado por varias partes:

   - La primera parte contiene los metadatos necesarios de blob.
   - La segunda parte contiene el archivo de texto.

1. Compruebe que el archivo de texto se proporcione como `Content-Type: text/plain`.

## <a name="api-endpoints"></a>Puntos de conexión de API

Las siguientes secciones describen los principales puntos de conexión de API relacionados con el blob y sus funcionalidades.

### <a name="devices"></a>Dispositivos

Puede conectar blobs a dispositivos. La siguiente imagen muestra la documentación de referencia de Swagger para las API de administración. Especifica los puntos de conexión de API relacionados con los dispositivos para el consumo de blobs y los parámetros de ruta de acceso necesarios que se pasarán a dichos puntos de conexión.

![Blobs de dispositivo][2]

Por ejemplo, para actualizar o crear un blob y conectarlo a un dispositivo, realice una solicitud HTTP PATCH autenticada a:

```plaintext
YOUR_MANAGEMENT_API_URL/devices/blobs/YOUR_BLOB_ID
```

| Parámetro | Reemplazar por |
| --- | --- |
| *YOUR_BLOB_ID* | Identificador de blob deseado |

Las solicitudes correctas devolverán un objeto JSON **DeviceBlob** en la respuesta. Los objetos **DeviceBlob** se ajustan al esquema JSON siguiente:

| Atributo | Escriba | DESCRIPCIÓN | Ejemplos |
| --- | --- | --- | --- |
| **DeviceBlobType** | string | Categoría de blob que se puede conectar a un dispositivo | `Model` y `Specification` |
| **DeviceBlobSubtype** | string | Subcategoría de blob más detallada que **DeviceBlobType** | `PhysicalModel`, `LogicalModel`, `KitSpecification` y `FunctionalSpecification` |

> [!TIP]
> Utilice la tabla anterior para controlar los datos de solicitud devueltos correctamente.

### <a name="spaces"></a>Espacios

También puede conectar blobs a espacios. En la imagen siguiente se enumeran todos los puntos de conexión de API de espacio responsables de controlar los blobs. También muestra los parámetros de ruta de acceso que se van a pasar a esos puntos de conexión.

![Blobs de espacio][3]

Por ejemplo, para devolver un blob conectado a un espacio, realice una solicitud HTTP GET autenticada a:

```plaintext
YOUR_MANAGEMENT_API_URL/spaces/blobs/YOUR_BLOB_ID
```

| Parámetro | Reemplazar por |
| --- | --- |
| *YOUR_BLOB_ID* | Identificador de blob deseado |

Una solicitud PATCH al mismo punto de conexión actualiza las descripciones de metadatos y crea nuevas versiones del blob. La solicitud HTTP se realiza mediante el método PATCH, con los metadatos y los datos de formulario de varias partes necesarios.

Las operaciones realizadas correctamente devuelven un objeto **SpaceBlob** que se ajusta al esquema siguiente. Puede usarlo para consumir los datos devueltos.

| Atributo | Escriba | DESCRIPCIÓN | Ejemplos |
| --- | --- | --- | --- |
| **SpaceBlobType** | string | Categoría de blob que se puede conectar a un espacio | `Map` y `Image` |
| **SpaceBlobSubtype** | string | Subcategoría de blob más detallada que **SpaceBlobType** | `GenericMap`, `ElectricalMap`, `SatelliteMap` y `WayfindingMap` |

### <a name="users"></a>Usuarios

Puede conectar blobs a modelos de usuario (por ejemplo, para asociar una imagen de perfil). En la imagen siguiente se muestran los puntos de conexión de API de usuarios pertinentes y los parámetros de ruta de acceso necesarios, como `id`:

![Blobs de usuario][4]

Por ejemplo, para capturar un blob conectado a un usuario, realice una solicitud HTTP GET autenticada con todos los datos de formulario necesarios a:

```plaintext
YOUR_MANAGEMENT_API_URL/users/blobs/YOUR_BLOB_ID
```

| Parámetro | Reemplazar por |
| --- | --- |
| *YOUR_BLOB_ID* | Identificador de blob deseado |

Los objetos JSON **UserBlob** devueltos se ajustarán a los siguientes modelos JSON:

| Atributo | Escriba | DESCRIPCIÓN | Ejemplos |
| --- | --- | --- | --- |
| **UserBlobType** | string | Categoría de blob que se puede conectar a un usuario | `Image` y `Video` |
| **UserBlobSubtype** |  string | Subcategoría de blob más detallada que **UserBlobType** | `ProfessionalImage`, `VacationImage` y `CommercialVideo` |

## <a name="common-errors"></a>Errores comunes

Un error común no incluye la información de encabezado correcta:

```JSON
{
    "error": {
        "code": "400.600.000.000",
        "message": "Invalid media type in first section."
    }
}
```

## <a name="next-steps"></a>Pasos siguientes

- Para obtener más información sobre la documentación de referencia de Swagger para Azure Digital Twins, lea [Uso de Azure Digital Twins Swagger](how-to-use-swagger.md).

<!-- Images -->
[1]: media/how-to-add-blobs/blob-models.PNG
[2]: media/how-to-add-blobs/blobs-device-api.PNG
[3]: media/how-to-add-blobs/blobs-space-api.PNG
[4]: media/how-to-add-blobs/blobs-users-api.PNG
