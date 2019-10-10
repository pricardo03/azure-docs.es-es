---
title: 'Niveles de rendimiento del almacenamiento de blobs en bloques de Azure: Azure Storage'
description: Niveles de rendimiento de Azure Blob Storage.
author: mhopkins-msft
ms.author: mhopkins
ms.date: 10/02/2019
ms.service: storage
ms.subservice: blobs
ms.topic: conceptual
ms.reviewer: clausjor
ms.openlocfilehash: ac483a338b7d71142b89b13e41fc048346ac037f
ms.sourcegitcommit: 4f3f502447ca8ea9b932b8b7402ce557f21ebe5a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/02/2019
ms.locfileid: "71803921"
---
# <a name="azure-block-blob-storage-performance-tiers"></a>Niveles de rendimiento del almacenamiento de blobs en bloques de Azure

A medida que las empresas implementan aplicaciones nativas en la nube sensibles al rendimiento, es importante disponer de opciones para el almacenamiento rentable de datos en diferentes niveles de rendimiento.

El almacenamiento de blobs en bloques de Azure ofrece dos niveles de rendimiento diferentes:

- Premium: optimizado para altas tasas de transacciones y latencia de almacenamiento coherente de un solo dígito
- Estándar: optimizado para una alta capacidad y un alto rendimiento

Las siguientes consideraciones se aplican a los distintos niveles de rendimiento:

- El rendimiento Estándar está disponible en todas las [regiones de Azure](https://azure.microsoft.com/global-infrastructure/services/?products=storage). El rendimiento Premium está disponible en [regiones seleccionadas](https://azure.microsoft.com/global-infrastructure/services/?products=storage).
- El rendimiento Premium ofrece precios optimizados para aplicaciones con altas tasas de transacciones para ayudar a [reducir el costo de almacenamiento total](https://azure.microsoft.com/blog/reducing-overall-storage-costs-with-azure-premium-blob-storage/) de estas cargas de trabajo.
- El rendimiento Premium requiere el uso de cuentas de almacenamiento de blobs en bloques que sean compatibles con los blobs en bloques y los blobs en anexos.
- El rendimiento Estándar está disponible en cuentas de almacenamiento de uso general v1, de uso general v2 y de Blob Storage.
- Ambos tipos de rendimiento son compatibles con los [blobs en bloques de alto rendimiento](https://azure.microsoft.com/blog/high-throughput-with-azure-blob-storage/). Los blobs en bloques de alto rendimiento están disponibles para el rendimiento Premium para tamaños mayores de 256 KiB. Los blobs en bloques de alto rendimiento están disponibles para el rendimiento Estándar para tamaños superiores de "Put Block" o "Put Blob" de 4 MiB.
- El rendimiento Premium está disponible actualmente solo con el almacenamiento con redundancia local (LRS).

## <a name="premium-performance"></a>Rendimiento Premium

El almacenamiento de blobs en bloques de rendimiento Premium hace que los datos estén disponibles a través de un hardware de alto rendimiento. Los datos se almacenan en unidades de estado sólido (SSD) que están optimizadas para baja latencia. Las SSD ofrecen mayor rendimiento en comparación con las unidades de disco duro tradicionales.

El almacenamiento de blobs en bloques de rendimiento Premium es perfecto para cargas de trabajo que requieren tiempos de respuesta rápidos y coherentes. Es mejor para las cargas de trabajo que realizan muchas transacciones pequeñas.

## <a name="standard-performance"></a>Rendimiento Estándar

El rendimiento Estándar admite distintos [niveles de acceso](storage-blob-storage-tiers.md) para almacenar los datos de la manera más rentable. Está optimizado para una alta capacidad y un alto rendimiento en grandes conjuntos de datos.

## <a name="blob-lifecycle-management"></a>Administración del ciclo de vida de blobs

La administración del ciclo de vida de Blob Storage ofrece una directiva completa basada en reglas:

- Premium: los datos expiran al final de su ciclo de vida
- Estándar: los datos experimentan una transición al mejor nivel de acceso y expiran al final de su ciclo de vida

Para más información, consulte [Administración del ciclo de vida de Azure Blob Storage](storage-lifecycle-management-concepts.md).

Los datos que se almacenan en una cuenta de almacenamiento de blobs en bloques de rendimiento Premium no se pueden migrar entre los niveles de acceso frecuente, esporádico y de archivo. Sin embargo, puede copiar blobs desde una cuenta de almacenamiento de blobs en bloques al nivel de acceso frecuente en una cuenta *diferente*. Use [Put Block From URL](/rest/api/storageservices/put-block-from-url) API o [AzCopy v10](../common/storage-use-azcopy-v10.md) para copiar los datos a una cuenta diferente. **Put Block From URL** API realiza una copia sincrónica de los datos en el servidor. La llamada se completa solo después de que todos los datos se han movido de la ubicación del servidor original a la ubicación de destino.

## <a name="next-steps"></a>Pasos siguientes

Evaluación de los niveles de acceso frecuente, esporádico y de archivo en cuentas de GPv2 y Blob Storage

- [Comprobación de la disponibilidad de los niveles de acceso frecuente, esporádico o de archivo por región](https://azure.microsoft.com/regions/#services)
- [Administración del ciclo de vida de Azure Blob Storage](storage-lifecycle-management-concepts.md)
- [Obtenga información sobre la rehidratación de datos de blob desde el nivel de archivo](storage-blob-rehydration.md)
- [Evaluación del uso de las cuentas de almacenamiento actuales mediante la habilitación de las métricas de Azure Storage](../common/storage-enable-and-view-metrics.md)
- [Comprobación de los precios de los niveles de acceso frecuente, esporádico y de archivo en cuentas de Blob Storage y de GPv2 por región](https://azure.microsoft.com/pricing/details/storage/)
- [Detalles de precios de Transferencias de datos](https://azure.microsoft.com/pricing/details/data-transfers/)
