---
title: Almacenamiento con redundancia geográfica (GRS) para la durabilidad interregional en Azure Storage | Microsoft Docs
description: El almacenamiento con redundancia geográfica (GRS) replica sus datos entre dos regiones que se encuentran a cientos de millas la una de la otra. GRS protege frente a errores de hardware en el centro de datos, así como frente a desastres regionales.
services: storage
author: tolandmike
ms.service: storage
ms.topic: article
ms.date: 10/20/2018
ms.author: jeking
ms.component: common
ms.openlocfilehash: 85d69db2f94e4bddf1258233c34c64dcf78a3eeb
ms.sourcegitcommit: 1b186301dacfe6ad4aa028cfcd2975f35566d756
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2018
ms.locfileid: "51219229"
---
# <a name="geo-redundant-storage-grs-cross-regional-replication-for-azure-storage"></a>Almacenamiento con redundancia geográfica (GRS): replicación entre regiones para Azure Storage
[!INCLUDE [storage-common-redundancy-GRS](../../../includes/storage-common-redundancy-grs.md)]

## <a name="read-access-geo-redundant-storage"></a>Almacenamiento con redundancia geográfica con acceso de lectura
El almacenamiento con redundancia geográfica con acceso de lectura (RA-GRS) maximiza la disponibilidad de la cuenta de almacenamiento. RA-GRS proporciona acceso de solo lectura a los datos de la ubicación secundaria, además de ofrecer replicación geográfica entre dos regiones.

Cuando habilita el acceso de solo lectura para los datos de la región secundaria, los datos quedan disponibles en un punto de conexión secundario, además del punto de conexión principal de la cuenta de almacenamiento. El punto de conexión secundario es similar al punto de conexión principal, pero anexa el sufijo `–secondary` al nombre de la cuenta. Por ejemplo, si el punto de conexión principal de Blob service es `myaccount.blob.core.windows.net`, el punto de conexión secundario es `myaccount-secondary.blob.core.windows.net`. Las claves de acceso de la cuenta de almacenamiento son iguales para los extremos principal y secundario.

Tenga en cuenta lo siguiente cuando use RA-GRS:

* La aplicación tiene que administrar el punto de conexión con el que interactúa al usar RA-GRS.
* Como la replicación asincrónica implica un retraso, los cambios que todavía no se hayan replicado a la región secundaria pueden perderse si los datos no se pueden recuperar desde la región principal.
* Puede comprobar la hora de última sincronización de la cuenta de almacenamiento. La hora de la última sincronización es un valor de fecha y hora GMT. Todas las escrituras primarias anteriores a la hora de la última sincronización se han escrito correctamente en la ubicación secundaria, lo que significa que están disponibles para leerse desde la ubicación secundaria. Las escrituras primarias posteriores a la hora de última sincronización pueden o no estar disponibles para lecturas todavía. Puede consultar este valor utilizando [Azure Portal](https://portal.azure.com/), [Azure PowerShell](storage-powershell-guide-full.md) o una de las bibliotecas cliente de Azure Storage.
* Si Microsoft inicia la conmutación por error en la región secundaria, tendrá acceso de lectura y escritura a esos datos una vez completada la conmutación por error. Para más información, vea la [Guía de recuperación ante desastres](storage-disaster-recovery-guidance.md).
* Para información sobre cómo cambiar a la región secundaria, consulte [Qué hacer si se produce una interrupción del servicio Azure Storage](storage-disaster-recovery-guidance.md).
* RA-GRS está pensado para fines de alta disponibilidad. Para obtener instrucciones sobre escalabilidad, revise la [lista de comprobación de rendimiento](storage-performance-checklist.md).
* Para sugerencias sobre cómo diseñar la alta disponibilidad con RA-GRS, consulte [Diseño de aplicaciones de alta disponibilidad mediante RA-GRS](storage-designing-ha-apps-with-ragrs.md).

## <a name="what-is-the-rpo-and-rto-with-grs"></a>¿Qué es el RPO y el RTO en GRS?
**Objetivo de punto de recuperación (RPO)**: en GRS y RA-GRS, el servicio de almacenamiento realiza un replicación geográfica asincrónica de los datos, desde la ubicación primaria a la secundaria. Si se produce un desastre regional de gran magnitud en la región principal, Microsoft realiza una conmutación por error en la región secundaria. Si se produce una conmutación por error, se pueden perder los cambios recientes que aún no se hayan replicado geográficamente. El número de minutos de posibles pérdidas de datos se conoce como el objetivo de punto de recuperación (RPO). El RPO indica el punto en el tiempo al que se pueden recuperar los datos. Normalmente, Azure Storage tiene un RPO inferior a 15 minutos, aunque actualmente no hay ningún contrato de nivel de servicio sobre cuánto tiempo tarda la replicación geográfica.

**Objetivo de tiempo de recuperación (RTO)**: se trata de una medida que indica cuánto tiempo tarda en realizarse la conmutación por error y tener la cuenta de almacenamiento de nuevo en línea. El tiempo para realizar la conmutación por error incluye lo siguiente:

   * El tiempo que Microsoft necesita para determinar si se pueden recuperar los datos en la ubicación principal, o si es necesario realizar una conmutación por error.
   * El tiempo necesario para realizar la conmutación por error de la cuenta de almacenamiento mediante el cambio de las entradas DNS principales hacia la ubicación secundaria.

Microsoft tiene la responsabilidad de conservar los datos. Si hay alguna posibilidad de recuperar los datos en la región primaria, Microsoft retrasa la conmutación por error y se centra en la recuperación de los datos. 

## <a name="paired-regions"></a>Regiones emparejadas 
Cuando crea una cuenta de almacenamiento, selecciona la región principal de la cuenta. La región secundaria emparejada se determina según la región primaria y no es posible cambiarla. Para información actualizada sobre las regiones que admite Azure, vea [Continuidad empresarial y recuperación ante desastres (BCDR): regiones emparejadas de Azure](../../best-practices-availability-paired-regions.md).

## <a name="see-also"></a>Otras referencias
- [Replicación de Azure Storage](storage-redundancy.md)
- [Almacenamiento con redundancia local (LRS): redundancia de datos de bajo costo para Azure Storage](storage-redundancy-lrs.md)
- [Almacenamiento con redundancia de zona (ZRS): aplicaciones de Azure Storage de alta disponibilidad](storage-redundancy-zrs.md)