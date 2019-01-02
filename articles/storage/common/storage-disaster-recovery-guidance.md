---
title: Qué hacer si se produce una interrupción del servicio de Azure Storage | Microsoft Docs
description: Qué hacer si se produce una interrupción del servicio Azure Storage
services: storage
author: tamram
ms.service: storage
ms.devlang: dotnet
ms.topic: article
ms.date: 12/12/2018
ms.author: tamram
ms.component: common
ms.openlocfilehash: 39a938d45c8f15c21b44bb5b04b1429fb4733b5a
ms.sourcegitcommit: e37fa6e4eb6dbf8d60178c877d135a63ac449076
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/13/2018
ms.locfileid: "53323275"
---
# <a name="what-to-do-if-an-azure-storage-outage-occurs"></a>Qué hacer si se produce una interrupción del servicio Azure Storage
En Microsoft, hacemos todo lo posible para garantizar que los servicios siempre estén disponibles. A veces, debido a factores externos que escapan de nuestro control, se producen interrupciones de servicio no planeadas en una o varias regiones. Para ayudarlo a gestionar estos raros imprevistos, ofrecemos la siguiente guía general para los servicios de Azure Storage.

## <a name="how-to-prepare"></a>Preparación
Es fundamental que todos los clientes preparen su propio plan de recuperación ante desastres. Para recuperarse de una interrupción de los servicios de almacenamiento, normalmente hay que realizar procedimientos automatizados e implicar al personal de operaciones con el objetivo de reactivar las aplicaciones para que funcionen con normalidad. Consulte la documentación de Azure para crear su propio plan de recuperación ante desastres:

* [Lista de comprobación de disponibilidad](https://docs.microsoft.com/azure/architecture/checklist/availability)
* [Diseño de aplicaciones resistentes de Azure](https://docs.microsoft.com/azure/architecture/resiliency/)
* [Servicio Azure Site Recovery](https://azure.microsoft.com/services/site-recovery/)
* [Replicación de Azure Storage](https://docs.microsoft.com/azure/storage/common/storage-redundancy)
* [Azure Backup](https://azure.microsoft.com/services/backup/)

## <a name="how-to-detect"></a>Detección
La manera recomendada de determinar el estado del servicio de Azure es suscribirse al [panel de estado del servicio de Azure](https://azure.microsoft.com/status/).

## <a name="what-to-do-if-a-storage-outage-occurs"></a>Qué hacer si se produce una interrupción de los servicios de almacenamiento
Si uno o varios servicios de almacenamiento no están disponibles temporalmente en una o varias regiones, hay dos opciones para tener en cuenta. Si desea acceder de inmediato a los datos, plantéese la opción 2.

### <a name="option-1-wait-for-recovery"></a>Opción 1: Espera para la recuperación
En este caso, no se requieren acciones por su parte. Trabajaremos con rapidez para que el servicio de Azure vuelva a estar disponible. Puede supervisar el estado del servicio en el [panel de estado del servicio de Azure](https://azure.microsoft.com/status/).

### <a name="option-2-copy-data-from-secondary"></a>Opción 2: Copia de los datos de la región secundaria
Si eligió el [almacenamiento con redundancia geográfica con acceso de lectura (RA-GRS)](storage-redundancy-grs.md#read-access-geo-redundant-storage) (recomendado) en las cuentas de almacenamiento, tendrá acceso de lectura a los datos de la región secundaria. Puede usar herramientas como [AzCopy](storage-use-azcopy.md), [Azure PowerShell](storage-powershell-guide-full.md) y la [biblioteca de movimiento de datos de Azure](https://azure.microsoft.com/blog/introducing-azure-storage-data-movement-library-preview-2/) para copiar los datos de la región secundaria en otra cuenta de almacenamiento de una región donde no se haya producido la interrupción. Después, haga que las aplicaciones apunten a esa cuenta de almacenamiento para proporcionar acceso de lectura y escritura.

## <a name="what-to-expect-if-a-storage-failover-occurs"></a>Qué esperar si se produce una conmutación por error de almacenamiento
Si eligió el [almacenamiento con redundancia geográfica (GRS)](storage-redundancy-grs.md) o el [almacenamiento geográficamente redundante con acceso de lectura (RA-GRS)](storage-redundancy-grs.md#read-access-geo-redundant-storage) (recomendado), Azure Storage conservará los datos en dos regiones (la principal y la secundaria). En las dos regiones, Azure Storage mantendrá constantemente réplicas de los datos.

Cuando se produce un desastre regional que afecta a la región primaria, se tratará en primer lugar de restaurar el servicio en esa región para ofrecer la mejor combinación de RTO y RPO. Según la naturaleza de los desastres y sus repercusiones, en raras ocasiones es posible que no podamos restaurar la región primaria. En ese momento, realizaremos una conmutación por error geográfica. La replicación de datos entre regiones es un proceso asincrónico que puede provocar retrasos, por lo que es posible que se pierdan los cambios que todavía no se hayan replicado a la región secundaria.

Ahora hablaremos de dos aspectos relativos a la experiencia de conmutación por error geográfica de almacenamiento:

* La conmutación por error geográfica de almacenamiento solo la activa el equipo de Azure Storage. El cliente no tiene que hacer nada. La conmutación por error se desencadena cuando el equipo de Azure Storage ha agotado todas las opciones de restauración de los datos en la misma región, para ofrecer la mejor combinación de RTO y RPO.
* Los puntos de conexión del servicio de almacenamiento para blobs, tablas, colas y archivos seguirán siendo los mismos después de la conmutación por error; la entrada DNS suministrada por Microsoft deberá actualizarse para cambiar de la región primaria a la secundaria. Microsoft realizará esta actualización automáticamente como parte del proceso de conmutación por error geográfica.
* Antes de la conmutación por error geográfica y durante este proceso, no tendrá acceso de escritura a la cuenta de almacenamiento debido a las repercusiones del desastre. Sin embargo, sí que podrá seguir realizando operaciones de lectura en la base de datos secundaria si la cuenta de almacenamiento se ha configurado como RA-GRS.
* Cuando se haya completado la conmutación por error geográfica y se propaguen los cambios de DNS, se reanudará el acceso de lectura y escritura de la cuenta de almacenamiento si tiene GRS o RA-GRS. El punto de conexión que antes era el punto de conexión secundario se convierte en el punto de conexión principal. 
* Puede comprobar el estado de la ubicación principal y consultar la última hora en que se produjo una conmutación por error con redundancia geográfica de la cuenta de almacenamiento. Para más información, consulte [Storage Accounts - Get Properties](https://docs.microsoft.com/rest/api/storagerp/storageaccounts/getproperties) (Cuentas de almacenamiento: propiedad Get).
* Después de la conmutación por error, la cuenta de almacenamiento volverá a estar completamente funcional, pero con un estado degradado, ya que se hospeda en una región independiente donde no se pueden realizar replicaciones geográficas. Para mitigar este riesgo, se restaurará la región primaria original y, luego, se realizará una conmutación por error geográfica para restaurar el estado original. Si la región primaria original es irrecuperable, asignamos otra región secundaria.

## <a name="best-practices-for-protecting-your-data"></a>Prácticas recomendadas para proteger los datos
Para hacer una copia de seguridad de los datos de almacenamiento de forma periódico, hay algunos enfoques que recomendamos.

* Discos de máquina virtual: use el [servicio Azure Backup](https://azure.microsoft.com/services/backup/) para realizar una copia de seguridad de los discos de máquina virtual que emplean Azure Virtual Machines.
* Blobs en bloques: active la [eliminación temporal](../blobs/storage-blob-soft-delete.md) para proteger contra eliminaciones de nivel de objeto y sobrescrituras o copie los blobs en otra cuenta de almacenamiento de otra región mediante [AzCopy](storage-use-azcopy.md), [Azure PowerShell](storage-powershell-guide-full.md) o la [biblioteca de movimiento de datos de Azure](https://azure.microsoft.com/blog/introducing-azure-storage-data-movement-library-preview-2/).
* Tablas: use [AzCopy](storage-use-azcopy.md) para exportar los datos de tabla en otra cuenta de almacenamiento de otra región.
* Archivos: use [AzCopy](storage-use-azcopy.md) o [Azure PowerShell](storage-powershell-guide-full.md) para copiar los archivos en otra cuenta de almacenamiento de otra región.

Para información sobre cómo crear aplicaciones que aprovechan totalmente la característica RA-GRS, consulte [Diseño de aplicaciones de alta disponibilidad mediante RA-GRS](../storage-designing-ha-apps-with-ragrs.md).
