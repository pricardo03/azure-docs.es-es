---
title: Rehidratación de los datos de blob desde el nivel de archivo
description: Rehidrate los blobs de Archive Storage para poder acceder a los datos.
services: storage
author: mhopkins-msft
ms.author: mhopkins
ms.date: 11/14/2019
ms.service: storage
ms.subservice: blobs
ms.topic: conceptual
ms.reviewer: hux
ms.openlocfilehash: d6370509b49ae464b53525e7320676b04912bd12
ms.sourcegitcommit: 598c5a280a002036b1a76aa6712f79d30110b98d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/15/2019
ms.locfileid: "74113715"
---
# <a name="rehydrate-blob-data-from-the-archive-tier"></a>Rehidratación de los datos de blob desde el nivel de archivo

Mientras un blob se encuentra en el nivel de acceso de archivo, se considera sin conexión y no se puede leer ni modificar. Los metadatos de blob quedan en línea y se mantienen disponibles, lo que permite enumerar el blob y sus propiedades. La lectura y modificación de los datos de blob solo están disponibles con niveles en línea, como acceso frecuente o esporádico. Hay dos opciones para recuperar los datos almacenados en el nivel de acceso de archivo y obtener acceso a ellos.

1. [Rehidratación de un blob archivado a un nivel en línea](#rehydrate-an-archived-blob-to-an-online-tier): rehidrate un blob de archivo a acceso frecuente o esporádico cambiando su nivel mediante la operación [Establecer el nivel del blob](https://docs.microsoft.com/rest/api/storageservices/set-blob-tier).
2. [Copia de un blob archivado en un nivel en línea](#copy-an-archived-blob-to-an-online-tier): cree una nueva copia de blob de archivo mediante la operación [Copiar blob](https://docs.microsoft.com/rest/api/storageservices/copy-blob). Especifique otro nombre de blob y un nivel de destino de acceso frecuente o esporádico.

 Para obtener más información sobre los niveles de acceso, consulte [Azure Blob Storage: niveles de acceso frecuente, esporádico y de archivo](storage-blob-storage-tiers.md).

## <a name="rehydrate-an-archived-blob-to-an-online-tier"></a>Rehidratación de un blob archivado en un nivel en línea

[!INCLUDE [storage-blob-rehydration](../../../includes/storage-blob-rehydrate-include.md)]

## <a name="copy-an-archived-blob-to-an-online-tier"></a>Copia de un blob archivado en un nivel en línea

Si no quiere rehidratar un archivo blob, puede elegir realizar una operación [Copiar blob](https://docs.microsoft.com/rest/api/storageservices/copy-blob). Su blob original permanecerá sin modificaciones en el archivo, mientras se crea un nuevo blob en el nivel frecuente o esporádico en línea para que pueda trabajar. En la operación Copiar blob, también puede establecer la propiedad opcional *​​x-ms-rehydrate-priority* en Estándar o Alta (versión preliminar) para especificar la prioridad con la que desea que se cree su copia de blob.

Los blobs de archivo solo se pueden copiar en los niveles de destino en línea dentro de la misma cuenta de almacenamiento. No se admite la copia de un blob de archivo en otro blob de archivo.

La copia de un BLOB desde el archivo puede tardar horas en completarse, en función de la prioridad de rehidratación seleccionada. En segundo plano, la operación **Copiar blob** lee el blob de origen de archivo para crear un nuevo blob en línea en el nivel de destino seleccionado. El nuevo blob puede ser visible cuando enumera los blobs, pero los datos no estarán disponibles hasta que la lectura del blob de archivo de origen esté completa y los datos se escriban en el nuevo blob de destino en línea. El nuevo BLOB es una copia independiente y cualquier modificación o eliminación en él no afecta al BLOB de archivo de origen.

## <a name="pricing-and-billing"></a>Precios y facturación

La rehidratación de blobs del archivo en los niveles de acceso frecuente o esporádico se cobran como operaciones de lectura y recuperación de datos. El uso de la prioridad alta (versión preliminar) tiene costos de operación y recuperación de datos más altos en comparación con la prioridad estándar. La rehidratación de alta prioridad aparece como una partida independiente en la factura. Si una solicitud de prioridad alta para devolver un blob de archivo de varios gigabytes tarda más de 5 horas, no se le cobrará la tarifa de recuperación de prioridad alta. Sin embargo, se siguen aplicando las tarifas de recuperación estándar, ya que la rehidratación se prioriza en otras solicitudes.

La copia de blobs del archivo en los niveles de acceso frecuente o esporádico se cobran como operaciones de lectura y recuperación de datos. Una operación de escritura se cobra por la creación de la nueva copia de BLOB. Las tarifas de eliminación anticipada no se aplican al realizar la copia en un blob en línea porque el blob de origen permanece sin modificar en el nivel de archivo. Si se selecciona, se aplican cargos por recuperación de alta prioridad.

Los blobs de nivel de archivo deben estar almacenados durante un mínimo de 180 días. La eliminación o rehidratación de blobs archivados antes de 180 días incurrirá en tarifas de eliminación anticipada.

> [!NOTE]
> Para obtener más información sobre los precios de los blobs en bloques, consulte la página [Precios de Azure Storage](https://azure.microsoft.com/pricing/details/storage/blobs/). Para obtener más información sobre los cargos por la transferencia de datos salientes, consulte la página [Detalles de precios de ancho de banda](https://azure.microsoft.com/pricing/details/data-transfers/).

## <a name="next-steps"></a>Pasos siguientes

* [Más información sobre los niveles de Blob Storage](storage-blob-storage-tiers.md)
* [Comprobación de los precios de los niveles de acceso frecuente, esporádico y de archivo en cuentas de Blob Storage y de GPv2 por región](https://azure.microsoft.com/pricing/details/storage/)
* [Administración del ciclo de vida de Azure Blob Storage](storage-lifecycle-management-concepts.md)
* [Detalles de precios de Transferencias de datos](https://azure.microsoft.com/pricing/details/data-transfers/)
