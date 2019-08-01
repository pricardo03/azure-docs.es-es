---
title: Introducción a los discos administrados de Azure Disk Storage para máquinas virtuales Windows | Microsoft Docs
description: Introducción a los discos administrados de Azure, que controlan las cuentas de almacenamiento automáticamente cuando se usan máquinas virtuales Windows de Azure
author: roygara
ms.service: virtual-machines-windows
ms.topic: overview
ms.date: 04/22/2019
ms.author: rogarana
ms.subservice: disks
ms.openlocfilehash: f86937feb7061950084a8b5bdb532075c7f93869
ms.sourcegitcommit: 800f961318021ce920ecd423ff427e69cbe43a54
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/31/2019
ms.locfileid: "68693720"
---
# <a name="introduction-to-azure-managed-disks"></a>Introducción a los discos administrados de Azure

Un disco administrado de Azure es un disco duro virtual (VHD). Se puede considerar como un disco físico en un servidor en el entorno local, pero virtualizado. Los discos administrados de Azure se almacenan como blobs en páginas, que son un objeto de almacenamiento de E/S aleatorio en Azure. Llamamos a administrados a estos discos porque es una abstracción sobre los blobs en páginas, los contenedores de blobs y las cuentas de almacenamiento de Azure. Con los discos administrados, lo único que debe hacer es aprovisionar el disco y Azure se encarga del resto.

Cuando se selecciona usar discos administrados de Azure con las cargas de trabajo, Azure crea y administra el disco automáticamente. Los tipos de discos disponibles son unidades de estado sólido ultra (SSD) (versión preliminar), SSD Premium, SSD estándar y unidades de disco duro (HDD) estándar. Para más información acerca de cada tipo de disco individual, consulte [Selección de un tipo de disco para máquinas virtuales IaaS](disks-types.md).

[!INCLUDE [virtual-machines-managed-disks-overview.md](../../../includes/virtual-machines-managed-disks-overview.md)]

> [!div class="nextstepaction"]
> [Selección de un tipo de disco para máquinas virtuales de IaaS](disks-types.md)