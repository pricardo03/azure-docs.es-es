---
title: archivo de inclusión
description: archivo de inclusión
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 06/28/2019
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: 5df0fe23183b53f13d6c6545e248724324434cde
ms.sourcegitcommit: df7942ba1f28903ff7bef640ecef894e95f7f335
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/14/2019
ms.locfileid: "69029860"
---
Entre las opciones de replicación para una cuenta de almacenamiento se incluyen las siguientes:

* [Almacenamiento con redundancia local (LRS)](../articles/storage/common/storage-redundancy-lrs.md): una estrategia de replicación simple y de bajo costo. Los datos se replican de forma sincrónica tres veces dentro de la región primaria.
* [Almacenamiento con redundancia de zona (ZRS)](../articles/storage/common/storage-redundancy-zrs.md): Replicación para escenarios que requieren alta disponibilidad. Los datos se replican de forma sincrónica en tres zonas de disponibilidad de Azure en la región primaria.
* [Almacenamiento con redundancia geográfica (GRS)](../articles/storage/common/storage-redundancy-grs.md): replicación entre regiones para protegerse frente a las interrupciones regionales. Los datos se replican sincrónicamente tres veces en la región primaria y, luego, se replican de forma asincrónica en la región secundaria. Para obtener acceso de lectura a los datos de la región secundaria, habilite el almacenamiento con redundancia geográfica con acceso de lectura (RA-GRS).
* [Almacenamiento con redundancia de zona geográfica (GZRS) (versión preliminar)](../articles/storage/common/storage-redundancy-gzrs.md): replicación para escenarios que requieren alta disponibilidad y máxima durabilidad. Los datos se replican sincrónicamente en tres zonas de disponibilidad de Azure en la región primaria y, luego, se replican de forma asincrónica en la región secundaria. Para obtener acceso de lectura a los datos de la región secundaria, habilite el almacenamiento con redundancia de zona geográfica con acceso de lectura (RA-GZRS).
