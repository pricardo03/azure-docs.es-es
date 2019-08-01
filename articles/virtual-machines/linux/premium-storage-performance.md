---
title: 'Azure Premium Storage: Diseño de alto rendimiento en máquinas virtuales de Windows | Microsoft Docs'
description: Diseñe aplicaciones de alto rendimiento con Azure Premium Storage. Premium Storage le ofrece compatibilidad con discos de alto rendimiento y baja latencia para cargas de trabajo con un uso intensivo de E/S, que se ejecutan en Azure Virtual Machines.
author: roygara
ms.service: virtual-machines-linux
ms.topic: conceptual
ms.date: 06/27/2017
ms.author: rogarana
ms.subservice: disks
ms.openlocfilehash: 70da3509ce44fe260f8010ccf6eb1d2192ca6e73
ms.sourcegitcommit: 800f961318021ce920ecd423ff427e69cbe43a54
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/31/2019
ms.locfileid: "68695495"
---
[!INCLUDE [virtual-machines-common-premium-storage-introduction](../../../includes/virtual-machines-common-premium-storage-introduction.md)]

> [!NOTE]
> A veces, lo que parece ser un problema de rendimiento del disco es realmente un cuello de botella de red. En estos casos, debería optimizarse el [rendimiento de la red](../../virtual-network/virtual-network-optimize-network-bandwidth.md).
>
> Si desea realizar pruebas comparativas de su disco, consulte nuestro artículo sobre cómo [realizar pruebas comparativas realizadas de un disco](disks-benchmarks.md).
>
> Si la VM admite redes aceleradas, debe asegurarse de que esta opción esté habilitada. Si no está habilitada, puede habilitarla en VM ya implementadas, tanto en [Windows](../../virtual-network/create-vm-accelerated-networking-powershell.md#enable-accelerated-networking-on-existing-vms) como en [Linux](../../virtual-network/create-vm-accelerated-networking-cli.md#enable-accelerated-networking-on-existing-vms).

Antes de comenzar, si no está familiarizado con Premium Storage, lea primero los artículos [¿Qué tipos de disco están disponibles en Azure?](disks-types.md) y [Objetivos de escalabilidad y rendimiento de Azure Storage para cuentas de almacenamiento](../../storage/common/storage-scalability-targets.md).


[!INCLUDE [virtual-machines-common-premium-storage-performance.md](../../../includes/virtual-machines-common-premium-storage-performance.md)]
