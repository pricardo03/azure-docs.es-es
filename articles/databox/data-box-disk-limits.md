---
title: Límites de Azure Data Box Disk | Microsoft Docs
description: Se describen los límites del sistema y los tamaños recomendados de Microsoft Azure Data Box Disk.
services: databox
author: alkohli
ms.service: databox
ms.subservice: disk
ms.topic: article
ms.date: 01/09/2019
ms.author: alkohli
ms.openlocfilehash: 412727d79c194172f2855d014d1eaf18f44167f6
ms.sourcegitcommit: 33091f0ecf6d79d434fa90e76d11af48fd7ed16d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/09/2019
ms.locfileid: "54159356"
---
# <a name="azure-data-box-disk-limits"></a>Límites de Azure Data Box Disk


Tenga en cuenta estos límites cuando implemente y use su solución de Microsoft Azure Data Box. 

## <a name="data-box-service-limits"></a>Límites de servicio de Data Box

 - El servicio Data Box está disponible solo en Estados Unidos, Europa, Canadá y Australia en todas las regiones de Azure para la nube pública de Azure.
 - Solo se admite una cuenta de almacenamiento con Data Box Disk.

## <a name="data-box-disk-performance"></a>Rendimiento de Data Box Disk

Cuando se prueba con discos conectados mediante USB 3.0, el rendimiento del disco era de hasta 430 MB/s. Los números reales varían según el tamaño de archivo usado. Con archivos más pequeños, es posible que observe un rendimiento menor.

## <a name="azure-storage-limits"></a>Límites de almacenamiento de Azure

En esta sección se describen los límites del servicio Azure Storage y las convenciones de nomenclatura necesarias para Azure Files, blobs en bloques de Azure y blobs en páginas de Azure, según corresponda para el servicio Data Box. Revise cuidadosamente los límites de almacenamiento y siga todas las recomendaciones.

Para conocer la información más reciente sobre los límites del servicio de almacenamiento de Azure y los procedimientos recomendados para asignar nombres a recursos compartidos, contenedores y archivos, vaya a:

- [Nomenclatura y referencia de contenedores](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata)
- [Nomenclatura y referencia de recursos compartidos](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-shares--directories--files--and-metadata)
- [Convenciones de blobs en bloques y blobs en páginas](https://docs.microsoft.com/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs)

> [!IMPORTANT]
> Si hay archivos o directorios que superan los límites del servicio Azure Storage, o no cumplen las convenciones de nomenclatura de archivos y blobs de Azure, estos archivos o directorios no se ingieren en Azure Storage mediante el servicio Data Box.

## <a name="data-upload-caveats"></a>Advertencias al cargar los datos

- No copie los datos directamente en los discos. Copie los datos en las carpetas *BlockBlob* y *PageBlob* creadas previamente.
- Una carpeta en las carpetas *BlockBlob* y *PageBlob* es un contenedor. Por ejemplo, los contenedores se crean como *BlockBlob/contenedor* y *PageBlob/contenedor*.
- Si tiene un objeto existente de Azure (por ejemplo, un blob) en la nube con el mismo nombre que el objeto que se está copiando, Data Box Disk sobrescribirá el archivo en la nube.
- Todos los archivos escritos en los recursos compartidos *BlockBlob* y *PageBlob* se cargan como blob en bloques y blob en páginas, respectivamente.
- Todas las jerarquías de directorios vacías (sin archivos) que creó en las carpetas *BlockBlob* y *PageBlob* no se cargan.
- Si se han producido errores al cargar datos en Azure, se crea un registro de errores en la cuenta de almacenamiento de destino. La ruta de acceso a este registro de errores está disponible en el portal cuando se completa la carga. Puede revisar el registro para realizar acciones correctivas. No elimine los datos del origen sin comprobar los datos cargados.

## <a name="azure-storage-account-size-limits"></a>Límites de tamaño de cuenta de almacenamiento de Azure

Estos son los límites del tamaño de los datos que se copian en la cuenta de almacenamiento. Asegúrese de que los datos que carga se ajustan a estos límites. Para tener la información más actualizada sobre estos límites, vaya a [Objetivos de escalabilidad de Azure Blob Storage](https://docs.microsoft.com/azure/storage/common/storage-scalability-targets#azure-blob-storage-scale-targets) y [Objetivos de escalabilidad de Azure Files](https://docs.microsoft.com/azure/storage/common/storage-scalability-targets#azure-files-scale-targets).

| Tamaño de los datos que se copian en la cuenta de almacenamiento de Azure                      | Límite predeterminado          |
|---------------------------------------------------------------------|------------------------|
| Blob en bloques y blobs en páginas                                            | 500 TB por cuenta de almacenamiento. <br> Esto incluye datos de todos los orígenes, incluido Data Box Disk.|


## <a name="azure-object-size-limits"></a>Límites de tamaño de objeto de Azure

Estos son los tamaños de los objetos de Azure que se pueden escribir. Asegúrese de que todos los archivos que se cargan se ajustan a estos límites.

| Tipo de objeto de Azure | Límite predeterminado                                             |
|-------------------|-----------------------------------------------------------|
| Blob en bloques        | ~ 8 TB                                                 |
| Blob en páginas         | 1 TB <br> (Todos los archivos cargados en formato de blob en páginas deben tener 512 bytes alineados (un múltiplo entero), de lo contrario, se produce un error en la carga. <br> VHD y VHDX tienen 512 bytes alineados). |


## <a name="azure-block-blob-and-page-blob-naming-conventions"></a>Convenciones de nomenclatura de blobs en páginas y blobs en bloques de Azure

| Entidad                                       | Convenciones                                                                                                                                                                                                                                                                                                               |
|----------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Nombres de contenedor de blob en bloques y blob en páginas | Debe ser un nombre DNS válido que tenga entre 3 y 63 caracteres. <br>  Debe empezar por una letra o un número. <br> Solo puede contener letras minúsculas, números y el guion (-). <br> Los caracteres de guión (-) deben estar inmediatamente precedidos y seguidos por una letra o un número. <br> No se permiten guiones consecutivos en nombres. |
| Nombres de blob para blob en bloques y blob en páginas      | Los nombres de blob distinguen mayúsculas de minúsculas y pueden contener cualquier combinación de caracteres. <br> Un nombre de blob debe tener entre 1 y 1024 caracteres. <br> Los caracteres de URL reservadas deben convertirse correspondientemente. <br>El número de segmentos de ruta de acceso que componen el nombre del blob no puede superar los 254. Un segmento de ruta de acceso es la cadena entre caracteres delimitadores consecutivos (por ejemplo, la barra diagonal "/") que se corresponden con el nombre de un directorio virtual. |


## <a name="next-steps"></a>Pasos siguientes
* Revise los [Requisitos del sistema Data Box](data-box-system-requirements.md).
