---
title: Límites de Azure Data Box | Microsoft Docs
description: Describe los límites del sistema y los tamaños recomendados de las conexiones y componentes de la matriz virtual de Microsoft Azure Data Box.
services: databox
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: article
ms.date: 02/22/2019
ms.author: alkohli
ms.openlocfilehash: 147cf61dcd36edc75a936cf9b467fd89c8d8a965
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/18/2019
ms.locfileid: "58082830"
---
# <a name="azure-data-box-limits"></a>Límites de Azure Data Box

Tenga en cuenta estos límites al implementar y usar Microsoft Azure Data Box. En la tabla siguiente se describen los límites de Data Box.


## <a name="data-box-service-limits"></a>Límites de servicio de Data Box

 - Si usa varias cuentas de almacenamiento con el servicio de Data Box, todas las cuentas de almacenamiento deben pertenecer a la misma región de Azure.
 - Se recomienda no usar más de tres cuentas de almacenamiento. Uso de cuentas de almacenamiento más potencialmente podría afectar al rendimiento.

## <a name="data-box-limits"></a>Límites de Data Box

- Data Box puede almacenar un máximo de 500 millones de archivos.

## <a name="azure-storage-limits"></a>Límites de almacenamiento de Azure

En esta sección se describen los límites del servicio Azure Storage y las convenciones de nomenclatura necesarias para Azure Files, blobs en bloques de Azure y blobs en páginas de Azure, según corresponda para el servicio Data Box. Revise cuidadosamente los límites de almacenamiento y siga todas las recomendaciones.

Para conocer la información más reciente sobre los límites del servicio de almacenamiento de Azure y los procedimientos recomendados para asignar nombres a recursos compartidos, contenedores y archivos, vaya a:

- [Nomenclatura y referencia de contenedores](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata)
- [Nomenclatura y referencia de recursos compartidos](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-shares--directories--files--and-metadata)
- [Convenciones de blobs en bloques y blobs en páginas](https://docs.microsoft.com/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs)

> [!IMPORTANT]
> Si hay archivos o directorios que superan los límites de servicio de almacenamiento de Azure, o no se ajustan a las convenciones de nomenclatura de archivos/Blob de Azure, estos archivos o directorios no se ingieren en el almacenamiento de Azure a través del servicio de Data Box.

## <a name="data-upload-caveats"></a>Advertencias al cargar los datos

- No copie los archivos directamente en cualquiera de los recursos compartidos creados previamente. Deberá crear una carpeta en el recurso compartido y, a continuación, copiar archivos en esa carpeta.
- Una carpeta en *StorageAccount_BlockBlob* y *StorageAccount_PageBlob* es un contenedor. Por ejemplo, los contenedores se crean como *StorageAccount_BlockBlob/container* y *StorageAccount_PageBlob/container*.
- Todas las carpetas que se han creado creó directamente en *StorageAccount_AzureFiles* se traducen en un recurso compartido de archivos de Azure.
- Si tiene un objeto existente de Azure (por ejemplo, un blob o un archivo) en la nube con el mismo nombre que el objeto que se está copiando, Data Box sobrescribirá el archivo en la nube.
- Todos los archivos escritos en los recursos compartidos *StorageAccount_BlockBlob* y *StorageAccount_BlockBlob* se cargan como blob en bloques y blob en páginas, respectivamente.
- Almacenamiento de blobs de Azure no es compatible con los directorios. Si crea una carpeta en la carpeta *StorageAccount_BlockBlob* y, después, se crean carpetas virtuales en el nombre del blob. Para Azure Files, se mantiene la estructura de directorios real.
- Cualquier vaciar una jerarquía de directorios (sin los archivos) que creó en *StorageAccount_BlockBlob* y *StorageAccount_PageBlob* carpetas no está cargado.
- Si se han producido errores al cargar datos en Azure, se crea un registro de errores en la cuenta de almacenamiento de destino. La ruta de acceso a este registro de errores está disponible cuando se completa la carga. Puede examinar el registro para realizar acciones correctivas. No elimine los datos desde el origen sin comprobar los datos cargados.

## <a name="azure-storage-account-size-limits"></a>Límites de tamaño de cuenta de almacenamiento de Azure

Estos son los límites del tamaño de los datos que se copian en la cuenta de almacenamiento. Asegúrese de que los datos que carga se ajustan a estos límites. Para tener la información más actualizada sobre estos límites, vaya a [Objetivos de escalabilidad de Azure Blob Storage](https://docs.microsoft.com/azure/storage/common/storage-scalability-targets#azure-blob-storage-scale-targets) y [Objetivos de escalabilidad de Azure Files](https://docs.microsoft.com/azure/storage/common/storage-scalability-targets#azure-files-scale-targets).

| Tamaño de los datos que se copian en la cuenta de almacenamiento de Azure                      | Límite predeterminado          |
|---------------------------------------------------------------------|------------------------|
| Blob en bloques y blob en páginas                                            | 500 TB por cuenta de almacenamiento. <br> Aquí se incluyen datos de todos los orígenes, incluido Data Box Disk.|
| Archivo de Azure                                                          | 5 TB por recurso compartido.<br> Todas las carpetas de *StorageAccount_AzureFiles* deben seguir este límite.       |

## <a name="azure-object-size-limits"></a>Límites de tamaño de objeto de Azure

Estos son los tamaños de los objetos de Azure que se pueden escribir. Asegúrese de que todos los archivos que se cargan se ajustan a estos límites.

| Tipo de objeto de Azure | Límite predeterminado                                             |
|-------------------|-----------------------------------------------------------|
| Blob en bloques        | ~4,75 TiB                                                 |
| Blob en páginas         | 8 TiB <br> Todos los archivos cargados en formato de blob en páginas deben tener 512 bytes alineados (un múltiplo entero), de lo contrario, se produce un error en la carga. <br> VHD y VHDX tienen 512 bytes alineados. |
| Archivos de Azure        | 1 TiB                                                      |
| Discos administrados     | 4 TiB <br> Para obtener más información sobre el tamaño y los límites, consulte: <li>[Objetivos de escalabilidad de SSDs estándar](../virtual-machines/windows/disks-types.md#standard-ssd)</li><li>[Objetivos de escalabilidad de Premium SSD](../virtual-machines/windows/disks-types.md#standard-hdd)</li><li>[Objetivos de escalabilidad de unidades de disco duro estándar](../virtual-machines/windows/disks-types.md#premium-ssd)</li><li>[Precios y facturación de discos administrados](../virtual-machines/windows/disks-types.md#billing)</li>                                                     |

## <a name="azure-block-blob-page-blob-and-file-naming-conventions"></a>Convenciones de nomenclatura de archivos, blobs en páginas y blobs en bloques de Azure

| Entidad                                       | Convenciones                                                                                                                                                                                                                                                                                                               |
|----------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Nombres de contenedor de blob en bloques y blob en páginas | Debe ser un nombre DNS válido que tenga entre 3 y 63 caracteres. <br>  Debe empezar por una letra o un número. <br> Solo puede contener letras minúsculas, números y el guion (-). <br> Los caracteres de guión (-) deben estar inmediatamente precedidos y seguidos por una letra o un número. <br> No se permiten guiones consecutivos en nombres. |
| Nombres de recursos compartidos de archivos de Azure                  | Lo mismo que antes.                                                                                                                                                                                                                                                                                                             |
| Nombres de archivos y directorios para archivos de Azure     |<li> No distingue mayúsculas y minúsculas, mantiene las mayúsculas y minúsculas, y no debe superar los 255 caracteres. </li><li> No puede terminar en una barra diagonal (/). </li><li>Si se proporciona, se quitará automáticamente. </li><li> No se permiten los caracteres siguientes: <code>" \\ / : \| < > * ?</code></li><li> Los caracteres de URL reservadas deben convertirse correspondientemente. </li><li> No se permiten caracteres de ruta de acceso de dirección URL no válidos. Puntos de código como \\uE000 no son caracteres Unicode válidos. Algunos caracteres ASCII o Unicode, como los caracteres de control (0 x 00 a 0x1F, \\u0081, etc.), tampoco están permitidos. Para conocer las reglas que rigen las cadenas Unicode en HTTP/1.1, consulte RFC 2616, sección 2.2: Basic Rules y RFC 3987. </li><li> No se permiten nombres de archivo siguientes: LPT1, LPT2, LPT3, LPT4, LPT5, LPT6, LPT7, LPT8, LPT9, COM1, COM2, COM3, COM4, COM5, COM6, COM7, COM8, COM9, PRN, AUX, NUL, CON, CLOCK$, carácter de punto (.) y caracteres de dos puntos (..).</li>|
| Nombres de blob para blob en bloques y blob en páginas      | </li><li>Los nombres de blob distinguen mayúsculas de minúsculas y pueden contener cualquier combinación de caracteres. </li><li>Un nombre de blob debe tener entre 1 y 1024 caracteres. </li><li>Los caracteres de URL reservadas deben convertirse correspondientemente. </li><li>El número de segmentos de ruta de acceso que componen el nombre del blob no puede superar los 254. Un segmento de ruta de acceso es la cadena entre caracteres delimitadores consecutivos (por ejemplo, la barra diagonal "/") que se corresponden con el nombre de un directorio virtual.</li> |
