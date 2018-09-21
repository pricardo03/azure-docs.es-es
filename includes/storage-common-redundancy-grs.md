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
ms.openlocfilehash: 922f4d5dd8c71bc9365523b1d539d1b0754fff15
ms.sourcegitcommit: e2ea404126bdd990570b4417794d63367a417856
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/14/2018
ms.locfileid: "45580946"
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