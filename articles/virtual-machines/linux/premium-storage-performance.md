---
title: 'Azure Premium Storage: Diseño del rendimiento en máquinas virtuales Windows '
description: Diseñe aplicaciones de alto rendimiento con Azure Premium Storage. Premium Storage le ofrece compatibilidad con discos de alto rendimiento y baja latencia para cargas de trabajo con un uso intensivo de E/S, que se ejecutan en Azure Virtual Machines.
author: roygara
ms.service: virtual-machines-linux
ms.topic: conceptual
ms.date: 06/27/2017
ms.author: rogarana
ms.subservice: disks
ms.openlocfilehash: 56836b81ad63c3a2170606dc64988aab8dc7f47f
ms.sourcegitcommit: 49cf9786d3134517727ff1e656c4d8531bbbd332
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/13/2019
ms.locfileid: "74035281"
---
# <a name="azure-premium-storage-design-for-high-performance"></a>Azure Premium Storage: diseño de alto rendimiento
[!INCLUDE [virtual-machines-common-premium-storage-introduction](../../../includes/virtual-machines-common-premium-storage-introduction.md)]

> [!NOTE]
> A veces, lo que parece ser un problema de rendimiento del disco es realmente un cuello de botella de red. En estos casos, debería optimizarse el [rendimiento de la red](../../virtual-network/virtual-network-optimize-network-bandwidth.md).
>
> Si desea realizar pruebas comparativas de su disco, consulte nuestro artículo sobre cómo [realizar pruebas comparativas realizadas de un disco](disks-benchmarks.md).
>
> Si la VM admite redes aceleradas, debe asegurarse de que esta opción esté habilitada. Si no está habilitada, puede habilitarla en VM ya implementadas, tanto en [Windows](../../virtual-network/create-vm-accelerated-networking-powershell.md#enable-accelerated-networking-on-existing-vms) como en [Linux](../../virtual-network/create-vm-accelerated-networking-cli.md#enable-accelerated-networking-on-existing-vms).

Antes de comenzar, si no está familiarizado con Premium Storage, lea primero los artículos [¿Qué tipos de disco están disponibles en Azure?](disks-types.md) y [Objetivos de escalabilidad y rendimiento de Azure Storage para cuentas de almacenamiento](../../storage/common/storage-scalability-targets.md).


[!INCLUDE [virtual-machines-common-premium-storage-performance.md](../../../includes/virtual-machines-common-premium-storage-performance.md)]

Si desea realizar pruebas comparativas de su disco, consulte nuestro artículo sobre cómo [realizar pruebas comparativas realizadas de un disco](disks-benchmarks.md).

Más información sobre los tipos de disco disponibles: [Selección de un tipo de disco](disks-types.md)  

Para los usuarios de SQL Server, lea artículos sobre procedimientos recomendados para SQL Server:

* [Procedimientos recomendados para SQL Server en Azure Virtual Machines](../windows/sql/virtual-machines-windows-sql-performance.md)
* [Azure Premium Storage proporciona el máximo rendimiento para SQL Server en una máquina virtual de Azure](https://blogs.technet.com/b/dataplatforminsider/archive/2015/04/23/azure-premium-storage-provides-highest-performance-for-sql-server-in-azure-vm.aspx)