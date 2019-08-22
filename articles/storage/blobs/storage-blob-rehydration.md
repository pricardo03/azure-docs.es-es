---
title: Rehidratación de los datos de blob desde el nivel de archivo
description: Rehidrate los blobs de Archive Storage para poder acceder a los datos.
services: storage
author: mhopkins-msft
ms.author: mhopkins
ms.date: 08/07/2019
ms.service: storage
ms.subservice: blobs
ms.topic: conceptual
ms.reviewer: hux
ms.openlocfilehash: 2e7d56a1461dfd89a7309288aadb0ba245d0f885
ms.sourcegitcommit: 0f54f1b067f588d50f787fbfac50854a3a64fff7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/12/2019
ms.locfileid: "68957509"
---
# <a name="rehydrate-blob-data-from-the-archive-tier"></a>Rehidratación de los datos de blob desde el nivel de archivo

Mientras un blob se encuentra en el nivel de acceso de archivo, se considera sin conexión y no se puede leer ni modificar. Los metadatos de blob quedan en línea y se mantienen disponibles, lo que permite enumerar el blob y sus propiedades. La lectura y modificación de los datos de blob solo están disponibles con niveles en línea, como acceso frecuente o esporádico. Hay dos opciones para recuperar los datos almacenados en el nivel de acceso de archivo y obtener acceso a ellos.

1. [Rehidratación de un blob archivado a un nivel en línea](#rehydrate-an-archived-blob-to-an-online-tier): rehidrate un blob archivado a acceso frecuente o esporádico cambiando su nivel mediante la operación [Establecer el nivel del blob](https://docs.microsoft.com/rest/api/storageservices/set-blob-tier).
2. [Copia de un blob archivado en un nivel en línea](#copy-an-archived-blob-to-an-online-tier): cree una nueva copia de un blob archivado mediante la operación [Copiar blob](https://docs.microsoft.com/rest/api/storageservices/copy-blob). Especifique otro nombre de blob y un nivel de destino de acceso frecuente o esporádico.

 Para obtener más información sobre los niveles de acceso, consulte [Azure Blob Storage: niveles de acceso frecuente, esporádico y de archivo](storage-blob-storage-tiers.md).

## <a name="rehydrate-an-archived-blob-to-an-online-tier"></a>Rehidratación de un blob archivado en un nivel en línea

[!INCLUDE [storage-blob-rehydration](../../../includes/storage-blob-rehydrate-include.md)]

## <a name="copy-an-archived-blob-to-an-online-tier"></a>Copia de un blob archivado en un nivel en línea

Si no quiere rehidratar un blob, puede elegir una operación [Copiar blob](https://docs.microsoft.com/rest/api/storageservices/copy-blob). El blob original permanecerá sin modificar en el archivo mientras se trabaja en el nuevo blob en el nivel de acceso frecuente o esporádico. Puede establecer la propiedad opcional *x-ms-rehydrate-priority* en Estándar o Alta (versión preliminar) al utilizar el proceso de copia.

Los blobs de archivo solo se pueden copiar en los niveles de destino en línea. No se admite la copia de un blob de archivo en otro blob de archivo.

La copia de un blob desde el archivo lleva tiempo. En segundo plano, la operación **Copiar blob** rehidrata temporalmente el blob de origen de archivo para crear un blob en línea en el nivel de destino. Este nuevo blob no estará disponible hasta que se complete la rehidratación temporal en el archivo y se escriban los datos en el nuevo blob.

## <a name="pricing-and-billing"></a>Precios y facturación

La rehidratación de blobs del archivo en los niveles de acceso frecuente o esporádico se cobran como operaciones de lectura y recuperación de datos. El uso de la prioridad alta (versión preliminar) tiene costos de operación y recuperación de datos más altos en comparación con la prioridad estándar. La rehidratación de alta prioridad aparece como una partida independiente en la factura. Si una solicitud de prioridad alta para devolver un blob de archivo de varios gigabytes tarda más de cinco horas, no se le cobrará la tarifa de recuperación de prioridad alta. Sin embargo, se seguirán aplicando las tarifas de recuperación estándar.

La copia de blobs del archivo en los niveles de acceso frecuente o esporádico se cobran como operaciones de lectura y recuperación de datos. Se cobra una operación de escritura por la creación de la nueva copia. Las tarifas de eliminación anticipada no se aplican al realizar la copia en un blob en línea porque el blob de origen permanece sin modificar en el nivel de archivo. Se aplican cargos de prioridad alta.

Los blobs de nivel de archivo deben estar almacenados durante un mínimo de 180 días. La eliminación o rehidratación de blobs archivados antes de 180 días incurrirá en tarifas de eliminación anticipada.

> [!NOTE]
> Para obtener más información sobre los precios de los blobs en bloques, consulte la página [Precios de Azure Storage](https://azure.microsoft.com/pricing/details/storage/blobs/). Para obtener más información sobre los cargos por la transferencia de datos salientes, consulte la página [Detalles de precios de ancho de banda](https://azure.microsoft.com/pricing/details/data-transfers/).

## <a name="next-steps"></a>Pasos siguientes

* [Más información sobre los niveles de Blob Storage](storage-blob-storage-tiers.md)
* [Comprobación de los precios de los niveles de acceso frecuente, esporádico y de archivo en cuentas de Blob Storage y de GPv2 por región](https://azure.microsoft.com/pricing/details/storage/)
* [Administración del ciclo de vida de Azure Blob Storage](storage-lifecycle-management-concepts.md)
* [Detalles de precios de Transferencias de datos](https://azure.microsoft.com/pricing/details/data-transfers/)
