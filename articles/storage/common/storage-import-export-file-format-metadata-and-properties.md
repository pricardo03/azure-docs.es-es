---
title: Formato del archivo de propiedades y metadatos de Azure Import/Export | Microsoft Docs
description: Obtenga información sobre cómo especificar metadatos y propiedades para uno o más blobs que forman parte de un trabajo de importación o exportación.
author: muralikk
services: storage
ms.service: storage
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.subservice: common
ms.openlocfilehash: 2066d4a2ed6db97285d92d15e14dbd21629dbdfa
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "61478562"
---
# <a name="azure-importexport-service-metadata-and-properties-file-format"></a>Formato del archivo de propiedades y metadatos de Azure Import/Export
Puede especificar metadatos y propiedades para uno o más blobs como parte de un trabajo de importación o exportación. Para establecer los metadatos o las propiedades para blobs que se crean como parte de un trabajo de importación, proporcione un archivo de metadatos o propiedades en la unidad de disco duro que contiene los datos que se van a importar. Para un trabajo de exportación, los metadatos y las propiedades se escriben en un archivo de metadatos o propiedades que se incluye en la unidad de disco duro que se le devuelve.  
  
## <a name="metadata-file-format"></a>Formato del archivo de metadatos  
El formato de un archivo de metadatos es el siguiente:  
  
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<Metadata>  
[<metadata-name-1>metadata-value-1</metadata-name-1>]  
[<metadata-name-2>metadata-value-2</metadata-name-2>]  
. . .  
</Metadata>  
```
  
|Elemento XML|Type|DESCRIPCIÓN|  
|-----------------|----------|-----------------|  
|`Metadata`|Elemento raíz|Elemento raíz del archivo de metadatos.|  
|`metadata-name`|Cadena|Opcional. El elemento XML especifica el nombre de los metadatos para el blob y su valor especifica el valor de la configuración de metadatos.|  
  
## <a name="properties-file-format"></a>Formato de archivo de propiedades  
El formato de un archivo de propiedades es el siguiente:  
  
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<Properties>  
[<Last-Modified>date-time-value</Last-Modified>]  
[<Etag>etag</Etag>]  
[<Content-Length>size-in-bytes<Content-Length>]  
[<Content-Type>content-type</Content-Type>]  
[<Content-MD5>content-md5</Content-MD5>]  
[<Content-Encoding>content-encoding</Content-Encoding>]  
[<Content-Language>content-language</Content-Language>]  
[<Cache-Control>cache-control</Cache-Control>]  
</Properties>  
```
  
|Elemento XML|Type|DESCRIPCIÓN|  
|-----------------|----------|-----------------|  
|`Properties`|Elemento raíz|Elemento raíz del archivo de propiedades.|  
|`Last-Modified`|Cadena|Opcional. Hora de la última modificación del blob. Solo para trabajos de exportación.|  
|`Etag`|Cadena|Opcional. Valor ETag del blob. Solo para trabajos de exportación.|  
|`Content-Length`|Cadena|Opcional. Tamaño del blob en bytes. Solo para trabajos de exportación.|  
|`Content-Type`|Cadena|Opcional. Tipo de contenido del blob.|  
|`Content-MD5`|Cadena|Opcional. Hash MD5 del blob.|  
|`Content-Encoding`|Cadena|Opcional. Codificación del contenido del blob.|  
|`Content-Language`|Cadena|Opcional. Idioma del contenido del blob.|  
|`Cache-Control`|Cadena|Opcional. Cadena de control de caché para el blob.|  

## <a name="next-steps"></a>Pasos siguientes

Vea [Set Blob Properties](/rest/api/storageservices/set-blob-properties) (Establecer propiedades del blob), [Set Blob Metadata](/rest/api/storageservices/set-blob-metadata) (Establecer metadatos del blob) y [Setting and Retrieving Properties and Metadata for Blob Resources](/rest/api/storageservices/setting-and-retrieving-properties-and-metadata-for-blob-resources) (Configuración y recuperación de propiedades y metadatos para los recursos del blob) para consultar reglas detalladas sobre cómo establecer propiedades y metadatos del blob.
