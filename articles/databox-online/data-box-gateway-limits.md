---
title: Límites de Azure Data Box Gateway | Microsoft Docs
description: Se describen los límites del sistema y los tamaños recomendados de Microsoft Azure Data Box Gateway.
services: databox
author: alkohli
ms.service: databox
ms.subservice: gateway
ms.topic: article
ms.date: 01/15/2019
ms.author: alkohli
ms.openlocfilehash: f01fb88bf0ea726b421111a262bdfdd68cd3d38c
ms.sourcegitcommit: 9f07ad84b0ff397746c63a085b757394928f6fc0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/17/2019
ms.locfileid: "54388420"
---
# <a name="azure-data-box-gateway-limits-preview"></a>Límites de Azure Data Box Gateway (versión preliminar)


Tenga en cuenta estos límites cuando implemente y use su solución de Microsoft Azure Data Box Gateway. 

> [!IMPORTANT] 
> Data Box Gateway está en versión preliminar. Revise las [condiciones de uso de la versión preliminar](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) antes de implementar esta solución. 


## <a name="data-box-gateway-service-limits"></a>Límites de servicio de Data Box Gateway

- En esta versión, el servicio está disponible solo en determinadas regiones de EE. UU., Europa y Asia Pacífico. Para más información, vaya a [Disponibilidad por región](#data-box-gateway-overview#region-availability). La cuenta de almacenamiento debe estar físicamente más cercana a la región donde se implementa el dispositivo (puede ser diferente del servicio geográfico).
- No se puede mover un recurso de Data Box Gateway a un grupo de recursos o suscripción diferentes. Para más información, consulte [Traslado de los recursos a un nuevo grupo de recursos o a una nueva suscripción](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-move-resources).

## <a name="data-box-gateway-device-limits"></a>Límites de dispositivos de Data Box Gateway

En la tabla siguiente se describen los límites del dispositivo de Data Box Gateway.

| DESCRIPCIÓN | Valor |
|---|---|
| No. de archivos por dispositivo |100 millones <br> El límite es ~ 25 millones de archivos para cada 2 TB de espacio en disco con un límite máximo de 100 millones |
| No. de recursos compartidos por dispositivo |24 |
|Tamaño máximo de archivo escrito en un recurso compartido|Para un dispositivo virtual de 2 TB, el tamaño máximo de archivo es de 500 GB. <br> El tamaño máximo de archivo aumenta con el tamaño del disco de datos en la relación anterior hasta que alcanza un máximo de 5 TB. |

## <a name="azure-storage-limits"></a>Límites de almacenamiento de Azure

En esta sección se describen los límites del servicio de Azure Storage y las convenciones de nomenclatura necesarias para Azure Files, blobs en bloques de Azure y blobs en páginas de Azure, según corresponda para el servicio Data Box Gateway o Data Box Edge. Revise cuidadosamente los límites de almacenamiento y siga todas las recomendaciones.

Para conocer la información más reciente sobre los límites del servicio de almacenamiento de Azure y los procedimientos recomendados para asignar nombres a recursos compartidos, contenedores y archivos, vaya a:

- [Nomenclatura y referencia de contenedores](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata)
- [Nomenclatura y referencia de recursos compartidos](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-shares--directories--files--and-metadata)
- [Convenciones de blobs en bloques y blobs en páginas](https://docs.microsoft.com/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs)

> [!IMPORTANT]
> Si hay archivos o directorios que superen los límites del servicio Azure Storage, o no cumplen las convenciones de nomenclatura de archivos y blobs de Azure, estos archivos o directorios no se ingieren en Azure Storage mediante el servicio Data Box Gateway o Data Box Edge.

## <a name="data-upload-caveats"></a>Advertencias al cargar los datos

Las advertencias siguientes se aplican a los datos cuando se trasladan en Azure.

- Se recomienda que no haya más de un dispositivo que escriba en el mismo contenedor.
- Si tiene un objeto existente de Azure (por ejemplo, un blob o un archivo) en la nube con el mismo nombre que el objeto que se está copiando, el dispositivo sobrescribirá el archivo en la nube.
- Una jerarquía de directorios vacía (sin archivos) que se creó en las carpetas de recurso compartido no se carga en los contenedores de blobs.
- Si va a copiar archivos de mayor tamaño que el del dispositivo, se recomienda usar *Robocopy* o *rsync* para asegurarse de que no haya errores.

## <a name="azure-storage-account-size-and-object-size-limits"></a>Límites de tamaño objeto y de tamaño de cuenta de almacenamiento en Azure

Estos son los límites del tamaño de los datos que se copian en la cuenta de almacenamiento. Asegúrese de que los datos que carga se ajustan a estos límites. Para tener la información más actualizada sobre estos límites, vaya a [Objetivos de escalabilidad de Azure Blob Storage](https://docs.microsoft.com/azure/storage/common/storage-scalability-targets#azure-blob-storage-scale-targets) y [Objetivos de escalabilidad de Azure Files](https://docs.microsoft.com/azure/storage/common/storage-scalability-targets#azure-files-scale-targets).

| Tamaño de los datos que se copian en la cuenta de almacenamiento de Azure                      | Límite predeterminado          |
|---------------------------------------------------------------------|------------------------|
| Blob en bloques y blobs en páginas                                            | 500 TB por cuenta de almacenamiento|


## <a name="azure-object-size-limits"></a>Límites de tamaño de objeto de Azure

Estos son los tamaños de los objetos de Azure que se pueden escribir. Asegúrese de que todos los archivos que se cargan se ajustan a estos límites.

| Tipo de objeto de Azure | Límite predeterminado                                             |
|-------------------|-----------------------------------------------------------|
| Blob en bloques        | ~ 8 TB                                                 |
| Blob en páginas         | 1 TB <br> Todos los archivos cargados en formato de blob en páginas deben tener 512 bytes alineados (un múltiplo entero); de lo contrario, se produce un error en la carga. <br> VHD y VHDX tienen 512 bytes alineados. |


## <a name="next-steps"></a>Pasos siguientes

- [Preparación para implementar Azure Data Box Gateway](data-box-gateway-deploy-prep.md)
