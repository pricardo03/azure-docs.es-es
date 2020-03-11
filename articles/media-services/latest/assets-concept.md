---
title: Recursos
titleSuffix: Azure Media Services
description: Conozca qué son los recursos y cómo se usan en Azure Media Services.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 08/29/2019
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: ce32343faefbcf2484ec0b1b39f752654a2d8514
ms.sourcegitcommit: f915d8b43a3cefe532062ca7d7dbbf569d2583d8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/05/2020
ms.locfileid: "78303620"
---
# <a name="assets-in-azure-media-services"></a>Recursos de Azure Media Services

En Azure Media Services, un [recurso](https://docs.microsoft.com/rest/api/media/assets) es un concepto básico. Es donde se introducen elementos multimedia (por ejemplo, a través de la carga o la ingesta en vivo), se generan elementos multimedia (a partir de la salida de un trabajo) y desde el que se publican elementos multimedia (para streaming). 

Un recurso se asigna a un contenedor de blobs en la [cuenta de Azure Storage](storage-account-concept.md) y los archivos del recurso se almacenan como blobs en bloques en ese contenedor. Los recursos contienen información sobre archivos digitales almacenados en Azure Storage (como vídeos, audio, imágenes, colecciones de miniaturas, pistas de texto y subtítulos).

Azure Media Services admite los niveles de blob cuando la cuenta usa el almacenamiento de uso general v2 (GPv2). Con GPv2, puede mover los archivos al [almacenamiento de acceso esporádico o al almacenamiento en frío](https://docs.microsoft.com/azure/storage/blobs/storage-blob-storage-tiers). El almacenamiento de **archivos** es adecuado para guardar archivos de origen cuando ya no son necesarios (por ejemplo, una vez codificados).

El nivel de almacenamiento de **archivos** solo se recomienda para archivos de origen muy grandes que ya se hayan codificado y cuya salida del trabajo de codificación se haya colocado en un contenedor de blobs de salida. Los blobs del contenedor de salida que quiera asociar con un recurso y usar para transmitir o analizar contenido deben existir en un nivel de almacenamiento **frecuente** o **esporádico**.

## <a name="naming"></a>Nomenclatura 

### <a name="assets"></a>Recursos

Los nombres de los recursos tienen que ser únicos. Los nombres de recursos de Media Services v3 (por ejemplo, recursos, trabajos y transformaciones) están sujetos a las restricciones de nomenclatura de Azure Resource Manager. Para más información, consulte [Convenciones de nomenclatura](media-services-apis-overview.md#naming-conventions).

### <a name="blobs"></a>Datos BLOB

Los nombres de los archivos o blobs dentro de un recurso deben seguir los [requisitos para los nombres de blobs](https://docs.microsoft.com/rest/api/storageservices/Naming-and-Referencing-Containers--Blobs--and-Metadata) y los [requisitos para los nombres NTFS](https://docs.microsoft.com/windows/win32/fileio/naming-a-file). La razón de estos requisitos es que los archivos se puedan copiar desde Blob Storage a un disco NTFS local para su procesamiento.

## <a name="map-v3-asset-properties-to-v2"></a>Asignación de las propiedades de los recursos de v3 a v2

En la tabla siguiente se muestra cómo las propiedades de los [recursos](https://docs.microsoft.com/rest/api/media/assets/createorupdate#asset) de v3 se asignan a las propiedades de los recursos de v2.

|Propiedades de v3|Propiedades de v2|
|---|---|
|`id`: (único), la ruta de acceso completa de Azure Resource Manager; consulte ejemplos en el artículo sobre [recursos](https://docs.microsoft.com/rest/api/media/assets/createorupdate).||
|`name`: (único), consulte [Convenciones de nomenclatura](media-services-apis-overview.md#naming-conventions). ||
|`alternateId`|`AlternateId`|
|`assetId`|`nb:cid:UUID:`: (único), valor que empieza por el prefijo `Id`.|
|`created`|`Created`|
|`description`|`Name`|
|`lastModified`|`LastModified`|
|`storageAccountName`|`StorageAccountName`|
|`storageEncryptionFormat`| `Options` (opciones de creación)|
|`type`||

## <a name="storage-side-encryption"></a>Cifrado del lado de almacenamiento

Para proteger los recursos en reposo, estos se deben cifrar mediante el cifrado del lado de almacenamiento. En la tabla siguiente se muestra cómo funciona el cifrado del lado de almacenamiento en Media Services:

|Opción de cifrado|Descripción|Media Services v2|Media Services v3|
|---|---|---|---|
|Cifrado de almacenamiento en Media Services|Cifrado AES-256, clave administrada por Media Services|Admitido<sup>(1)</sup>|No admitido<sup>(2)</sup>|
|[Storage Service Encryption para datos en reposo](https://docs.microsoft.com/azure/storage/common/storage-service-encryption)|Cifrado en el lado servidor que ofrece Azure Storage, clave administrada por Azure o por el cliente.|Compatible|Compatible|
|[Cifrado en el lado de cliente de almacenamiento](https://docs.microsoft.com/azure/storage/common/storage-client-side-encryption)|Cifrado en el lado cliente que ofrece Azure Storage, clave administrada por el cliente en Key Vault.|No compatible|No compatible|

<sup>1</sup> Aunque Media Services admite el control del contenido sin cifrado, no se recomienda.

<sup>2</sup> En Media Services v3, el cifrado de almacenamiento (cifrado con AES-256) solo es compatible con versiones anteriores si los recursos se crearon con Media Services v2. Esto significa que la versión v3 funciona con los recursos cifrados de almacenamiento ya existentes pero no permitirá la creación de otros.

## <a name="next-steps"></a>Pasos siguientes

[Administración de recursos en Azure Media Services](manage-asset-concept.md)

## <a name="see-also"></a>Consulte también

[Diferencias entre la versión v2 y v3 de Media Services](migrate-from-v2-to-v3.md)
