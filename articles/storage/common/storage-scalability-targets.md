---
title: Escalabilidad y rendimiento objetivos de Azure Storage - cuentas de almacenamiento
description: Obtenga información sobre los objetivos de escalabilidad y rendimiento, incluida la capacidad, tasa de solicitud y ancho de banda entrante y saliente para las cuentas de almacenamiento de Azure.
services: storage
author: roygara
ms.service: storage
ms.topic: conceptual
ms.date: 03/23/2019
ms.author: rogarana
ms.subservice: common
ms.openlocfilehash: ca9c4c959d21f26369600129f3897b7624dd84f2
ms.sourcegitcommit: 49c8204824c4f7b067cd35dbd0d44352f7e1f95e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/22/2019
ms.locfileid: "58371181"
---
# <a name="azure-storage-scalability-and-performance-targets-for-storage-accounts"></a>Azure objetivos de escalabilidad y rendimiento de almacenamiento para las cuentas de almacenamiento

En este artículo se detallan los objetivos de escalabilidad y rendimiento de las cuentas de Azure Storage. Los objetivos de escalabilidad y rendimiento que mencionamos aquí son objetivos exigentes, pero se pueden lograr. En todos los casos, la velocidad de solicitudes y el ancho de banda obtenido por la cuenta de almacenamiento depende del tamaño de los objetos almacenados, de los patrones de acceso utilizados y del tipo de carga de trabajo que realiza la aplicación.

Asegúrese de probar el servicio para determinar si el rendimiento se ajusta a sus requisitos. Si es posible, evite picos en la tasa de tráfico y asegúrese de que este se distribuya equitativamente entre las particiones.

Cuando la aplicación alcanza el límite de lo que puede administrar una partición para la carga de trabajo, Azure Storage comienza a responder con el código de error 503 (servidor ocupado) o el código de error 500 (tiempo de espera de operación). Si se producen errores 503, considere la posibilidad de modificar la aplicación para utilizar una directiva de retroceso exponencial para los reintentos. El retroceso exponencial permite que disminuya la carga de la partición y evita los picos de tráfico en esa partición.

## <a name="standard-performance-storage-account-scale-limits"></a>Límites de escala de cuenta de almacenamiento estándar de rendimiento

[!INCLUDE [azure-storage-limits](../../../includes/azure-storage-limits.md)]

## <a name="premium-performance-storage-account-scale-limits"></a>Límites de escala de cuenta de almacenamiento de rendimiento de Premium

[!INCLUDE [azure-premium-limits](../../../includes/azure-storage-limits-premium.md)]

## <a name="storage-resource-provider-scale-limits"></a>Límites de escala del proveedor de recursos de almacenamiento

[!INCLUDE [azure-storage-limits-azure-resource-manager](../../../includes/azure-storage-limits-azure-resource-manager.md)]

## <a name="azure-blob-storage-scale-targets"></a>Objetivos de escalabilidad de Azure Blob Storage

[!INCLUDE [storage-blob-scale-targets](../../../includes/storage-blob-scale-targets.md)]

## <a name="azure-files-scale-targets"></a>Objetivos de escalabilidad de Azure Files

Para obtener más información sobre los objetivos de escalabilidad y rendimiento para Azure Files y Azure File Sync, consulte [Azure Files scalability and performance targets](../files/storage-files-scale-targets.md) (Objetivos de escalabilidad y rendimiento de Azure Files).

[!INCLUDE [storage-files-scale-targets](../../../includes/storage-files-scale-targets.md)]

### <a name="azure-file-sync-scale-targets"></a>Objetivos de escalabilidad de Azure File Sync

Azure File Sync se ha diseñado con el objetivo de un uso sin límites, pero el uso sin límites no siempre es posible. La tabla siguiente indica los límites de las pruebas de Microsoft y también indica qué objetivos son límites estrictos:

[!INCLUDE [storage-sync-files-scale-targets](../../../includes/storage-sync-files-scale-targets.md)]

## <a name="azure-queue-storage-scale-targets"></a>Objetivos de escalabilidad de Azure Queue Storage

[!INCLUDE [storage-queues-scale-targets](../../../includes/storage-queues-scale-targets.md)]

## <a name="azure-table-storage-scale-targets"></a>Objetivos de escalabilidad de Azure Table Storage

[!INCLUDE [storage-table-scale-targets](../../../includes/storage-tables-scale-targets.md)]

## <a name="see-also"></a>Vea también

- [Detalles de precios de almacenamiento](https://azure.microsoft.com/pricing/details/storage/)
- [Límites, cuotas y restricciones de suscripción y servicios de Microsoft Azure](../../azure-subscription-service-limits.md)
- [Replicación de Azure Storage](../storage-redundancy.md)
- [Lista de comprobación de rendimiento y escalabilidad de Microsoft Azure Storage](../storage-performance-checklist.md)