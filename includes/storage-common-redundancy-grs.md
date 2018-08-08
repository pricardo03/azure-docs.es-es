---
title: archivo de inclusión
description: archivo de inclusión
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 03/26/2018
ms.author: jeking
ms.custom: include file
ms.openlocfilehash: f5b6e0e74bbab33b9ae6fbacca5c55ea434d3e41
ms.sourcegitcommit: d4c076beea3a8d9e09c9d2f4a63428dc72dd9806
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/01/2018
ms.locfileid: "39399964"
---
El almacenamiento con redundancia geográfica(GRS) está diseñado para proporcionar al menos el 99.99999999999999 % (dieciséis nueves) de durabilidad de objetos a lo largo de un año. Para ello, replica los datos a una región secundaria que se encuentra a cientos de kilómetros de la región primaria. Si la cuenta de almacenamiento tiene habilitado GRS, sus datos se mantienen incluso ante un apagón regional completo o un desastre del cual la región principal no se puede recuperar.

Si opta por GRS, tiene dos opciones relacionadas para elegir:

* GRS replica los datos en otro centro de datos en una región secundaria, pero esos datos están disponibles para ser de solo lectura si Microsoft inicia una conmutación por error desde la región primaria a la región secundaria. 
* El almacenamiento con redundancia geográfica con acceso de lectura (RA-GRS) se basa en GRS. RA-GRS replica los datos en otro centro de datos de una región secundaria y también proporciona la opción para leer desde la región secundaria. Con RA-GRS, puede leer desde la base de datos secundaria sin importar si Microsoft inicia una conmutación por error desde la región primaria a la región secundaria. 

Para una cuenta de almacenamiento con que tiene habilitado GRS o RA-GRS, todos los datos primero se replican con el almacenamiento con redundancia local (LRS). Una actualización se confirma primero en la ubicación principal y se replican mediante LRS. La actualización luego se replica de manera asincrónica en la región secundaria con GRS. Cuando los datos se escriben en la ubicación secundaria, también se replican dentro de esa ubicación con LRS. 

Las regiones primarias y secundarias administran las réplicas entre dominios de error y de actualización diferentes dentro de una unidad de escalado de almacenamiento. La unidad de escalado de almacenamiento es la unidad de replicación básica dentro del centro de datos. LRS proporciona la replicación en este nivel. Para más información, consulte [Almacenamiento con redundancia local (LRS): redundancia de datos de bajo costo para Azure Storage](../articles/storage/common/storage-redundancy-lrs.md).

Tenga en cuenta estos puntos cuando decida qué opción de replicación usar:

* El almacenamiento con redundancia de zona (ZRS) ofrece alta disponibilidad con replicación sincrónica y puede ser una mejor opción para algunos escenarios que GRS o RA-GRS. Para más información sobre ZRS, consulte [ZRS](../articles/storage/common/storage-redundancy-zrs.md).
* Como la replicación asincrónica implica un retraso, ante la eventualidad de un desastre regional es posible que los cambios que todavía no se hayan replicado en la región secundaria se pierdan si no es posible recuperar los datos de la región primaria.
* Con GRS, la réplica no está disponible a menos que Microsoft inicie la conmutación por error para la región secundaria. Si Microsoft inicia una conmutación por error en la región secundaria, tendrá acceso de lectura y escritura a esos datos una vez completada la conmutación por error. Para más información, consulte la [guía de recuperación ante desastres](../articles/storage/common/storage-disaster-recovery-guidance.md).
* Si la aplicación necesita leer desde la región secundaria, habilite RA-GRS.

## <a name="read-access-geo-redundant-storage"></a>Almacenamiento con redundancia geográfica con acceso de lectura

El almacenamiento con redundancia geográfica con acceso de lectura (RA-GRS) maximiza la disponibilidad de la cuenta de almacenamiento. RA-GRS proporciona acceso de solo lectura a los datos de la ubicación secundaria, además de ofrecer replicación geográfica entre dos regiones.

Cuando habilita el acceso de solo lectura para los datos de la región secundaria, los datos quedan disponibles en un punto de conexión secundario, además del punto de conexión principal de la cuenta de almacenamiento. El punto de conexión secundario es similar al punto de conexión principal, pero anexa el sufijo `–secondary` al nombre de la cuenta. Por ejemplo, si el punto de conexión principal de Blob service es `myaccount.blob.core.windows.net`, el punto de conexión secundario es `myaccount-secondary.blob.core.windows.net`. Las claves de acceso de la cuenta de almacenamiento son iguales para los extremos principal y secundario.

Tenga en cuenta lo siguiente cuando use RA-GRS:

* La aplicación tiene que administrar el punto de conexión con el que interactúa al usar RA-GRS.
* Como la replicación asincrónica implica un retraso, ante la eventualidad de un desastre regional, por ejemplo, es posible que los cambios que todavía no se hayan replicado a la región secundaria se pierdan si no se pueden recuperar los datos desde la región principal.
* Puede comprobar la hora de última sincronización de la cuenta de almacenamiento. La hora de la última sincronización es un valor de fecha y hora GMT. Todas las escrituras primarias anteriores a la hora de la última sincronización se han escrito correctamente en la ubicación secundaria, lo que significa que están disponibles para leerse desde la ubicación secundaria. Las escrituras primarias posteriores a la hora de última sincronización pueden o no estar disponibles para lecturas todavía. Puede consultar este valor utilizando [Azure Portal](https://portal.azure.com/), [Azure PowerShell](../articles/storage/common/storage-powershell-guide-full.md) o una de las bibliotecas cliente de Azure Storage.
* Si Microsoft inicia la conmutación por error en la región secundaria, tendrá acceso de lectura y escritura a esos datos una vez completada la conmutación por error. Para obtener más información, consulte la [Guía de recuperación ante desastres](../articles/storage/common/storage-disaster-recovery-guidance.md).
* Para información sobre cómo cambiar a la región secundaria, consulte [Qué hacer si se produce una interrupción del servicio Azure Storage](../articles/storage/common/storage-disaster-recovery-guidance.md).
* RA-GRS está pensado para fines de alta disponibilidad. Para obtener instrucciones sobre escalabilidad, revise la [lista de comprobación de rendimiento](../articles/storage/common/storage-performance-checklist.md).
* Para sugerencias sobre cómo diseñar la alta disponibilidad con RA-GRS, consulte [Diseño de aplicaciones de alta disponibilidad mediante RA-GRS](../articles/storage/common/storage-designing-ha-apps-with-ragrs.md).

## <a name="what-is-the-rpo-and-rto-with-grs"></a>¿Qué es el RPO y el RTO en GRS?
**Objetivo de punto de recuperación (RPO)**: en GRS y RA-GRS, el servicio de almacenamiento realiza un replicación geográfica asincrónica de los datos, desde la ubicación primaria a la secundaria. Si se produce un desastre regional de gran magnitud en la región principal, Microsoft realiza una conmutación por error en la región secundaria. Si se produce una conmutación por error, se pueden perder los cambios recientes que aún no se hayan replicado geográficamente. El número de minutos de pérdida potencial de datos se conoce como RPO e indica el momento específico en el que se pueden recuperar los datos. Normalmente, Azure Storage tiene un RPO inferior a 15 minutos, aunque actualmente no hay ningún contrato de nivel de servicio sobre cuánto tiempo tarda la replicación geográfica.

**Objetivo de tiempo de recuperación (RTO)**: se trata de una medida que indica cuánto tiempo tarda en realizarse la conmutación por error y tener la cuenta de almacenamiento de nuevo en línea. El tiempo para realizar la conmutación por error incluye lo siguiente:

   * El tiempo que Microsoft necesita para determinar si se pueden recuperar los datos en la ubicación principal, o si es necesario realizar una conmutación por error.
   * El tiempo necesario para realizar la conmutación por error de la cuenta de almacenamiento mediante el cambio de las entradas DNS principales hacia la ubicación secundaria.

   Microsoft tiene la responsabilidad de conservar los datos. Si hay alguna posibilidad de recuperar los datos en la región primaria, Microsoft retrasará la conmutación por error y se centrará en la recuperación de los datos. Una versión futura del servicio le permitirá desencadenar una conmutación por error en el nivel de cuenta, para que pueda controlar el RTO.

## <a name="paired-regions"></a>Regiones emparejadas 

Cuando crea una cuenta de almacenamiento, selecciona la región principal de la cuenta. La región secundaria emparejada se determina según la región primria y no es posible cambiarla. Para información actualizada sobre las regiones que admite Azure, consulte [Continuidad empresarial y recuperación ante desastres (BCDR): regiones emparejadas de Azure](../articles/best-practices-availability-paired-regions.md).
