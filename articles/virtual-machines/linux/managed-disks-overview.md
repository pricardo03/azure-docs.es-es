---
title: Introducción a los discos administrados de Azure Disk Storage para máquinas virtuales Linux | Microsoft Docs
description: Introducción a los discos administrados de Azure, que controlan las cuentas de almacenamiento cuando se usan máquinas virtuales Linux
services: virtual-machines-linux,storage
author: roygara
ms.service: virtual-machines-linux
ms.tgt_pltfrm: vm-linux
ms.topic: overview
ms.date: 04/22/2019
ms.author: rogarana
ms.subservice: disks
ms.openlocfilehash: 18dc1bd2eea232d0c2eb73d496dd4bd9d2d5016e
ms.sourcegitcommit: 57a7d4f67635212f5bf0c56e58fd87c8ec366f2c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/22/2019
ms.locfileid: "64707806"
---
# <a name="introduction-to-azure-managed-disks"></a>Introducción a los discos administrados de Azure

Un disco administrado de Azure es un disco duro virtual (VHD). Se puede considerar como un disco físico en un servidor en el entorno local, pero virtualizado. Los discos administrados de Azure se almacenan como blobs en páginas, que son un objeto de almacenamiento de E/S aleatorio en Azure. Llamamos a administrados a estos discos porque es una abstracción sobre los blobs en páginas, los contenedores de blobs y las cuentas de almacenamiento de Azure. Con los discos administrados, lo único que debe hacer es aprovisionar el disco y Azure se encarga del resto.

Cuando se selecciona usar discos administrados de Azure con las cargas de trabajo, Azure crea y administra el disco automáticamente. Los tipos de discos disponibles son discos ultra (versión preliminar), unidades de estado sólido (SSD) prémium, SSD estándar y unidades de disco duro estándar (HDD). Para más información acerca de cada tipo de disco individual, consulte [Selección de un tipo de disco para máquinas virtuales IaaS](disks-types.md).

[!INCLUDE [virtual-machines-managed-disks-overview.md](../../../includes/virtual-machines-managed-disks-overview.md)]

> [!div class="nextstepaction"]
> [Selección de un tipo de disco para máquinas virtuales de IaaS](disks-types.md)
