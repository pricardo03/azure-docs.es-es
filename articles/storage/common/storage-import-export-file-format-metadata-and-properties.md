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
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
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
|`metadata-name`|String|Opcional. El elemento XML especifica el nombre de los metadatos para el blob y su valor especifica el valor de la configuración de metadatos.|  
  
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
|`Last-Modified`|String|Opcional. Hora de la última modificación del blob. Solo para trabajos de exportación.|  
|`Etag`|String|Opcional. Valor ETag del blob. Solo para trabajos de exportación.|  
|`Content-Length`|String|Opcional. Tamaño del blob en bytes. Solo para trabajos de exportación.|  
|`Content-Type`|String|Opcional. Tipo de contenido del blob.|  
|`Content-MD5`|String|Opcional. Hash MD5 del blob.|  
|`Content-Encoding`|String|Opcional. Codificación del contenido del blob.|  
|`Content-Language`|String|Opcional. Idioma del contenido del blob.|  
|`Cache-Control`|String|Opcional. Cadena de control de caché para el blob.|  

## <a name="next-steps"></a>Pasos siguientes

Vea [Set Blob Properties](/rest/api/storageservices/set-blob-properties) (Establecer propiedades del blob), [Set Blob Metadata](/rest/api/storageservices/set-blob-metadata) (Establecer metadatos del blob) y [Setting and Retrieving Properties and Metadata for Blob Resources](/rest/api/storageservices/setting-and-retrieving-properties-and-metadata-for-blob-resources) (Configuración y recuperación de propiedades y metadatos para los recursos del blob) para consultar reglas detalladas sobre cómo establecer propiedades y metadatos del blob.
