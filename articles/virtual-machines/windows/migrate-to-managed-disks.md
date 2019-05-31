---
title: Migración de máquinas virtuales de Azure a Managed Disks | Microsoft Docs
description: Migre máquinas virtuales de Azure creadas con discos no administrados en cuentas de almacenamiento para usar Managed Disks.
services: virtual-machines-windows
documentationcenter: ''
author: roygara
manager: twooley
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/30/2019
ms.author: rogarana
ms.subservice: disks
ms.openlocfilehash: 7a390c5231f715ce778c0cc267211b8bfb5934d2
ms.sourcegitcommit: c05618a257787af6f9a2751c549c9a3634832c90
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/30/2019
ms.locfileid: "66418401"
---
# <a name="migrate-azure-vms-to-managed-disks-in-azure"></a>Migración de VM de Azure a Managed Disks en Azure

Azure Managed Disks simplifica la administración de almacenamiento al eliminar la necesidad de administrar cuentas de almacenamiento por separado.  También puede migrar las VM de Azure existentes a Managed Disks para aprovechar la mejor confiabilidad de las VM en un conjunto de disponibilidad. Se asegura de que los discos de máquinas virtuales diferentes en un conjunto de disponibilidad están suficientemente aislados entre sí para evitar puntos únicos de errores. Coloca automáticamente discos de distintas VM en un conjunto de disponibilidad en unidades de escalado de almacenamiento diferentes (marcas de tiempo), lo que limita el impacto de errores únicos de unidad de escalado de almacenamiento generados debido a errores de hardware y software.
En virtud de sus necesidades, puede elegir entre cuatro tipos de opciones de almacenamiento. Para información acerca de los tipos de disco disponibles, consulte nuestro artículo [Selección de un tipo de disco](disks-types.md).

## <a name="migration-scenarios"></a>Escenarios de migración

Puede migrar a Managed Disks en los escenarios siguientes:

|Escenario  |Artículo  |
|---------|---------|
|Conversión de VM independientes y VM de un conjunto de disponibilidad para establecer discos administrados     |[Conversión de VM para usar discos administrados](convert-unmanaged-to-managed-disks.md)         |
|Convertir una sola máquina virtual del modelo clásico a Resource Manager en discos administrados     |[Creación de una VM desde un disco duro virtual clásico](create-vm-specialized-portal.md)         |
|Convertir todas las máquinas virtuales en una red virtual desde el modelo clásico a Resource Manager en discos administrados     |[Migración de recursos de IaaS desde el modelo clásico a Resource Manager](migration-classic-resource-manager-ps.md) y luego [Conversión de una VM desde discos no administrados a discos administrados](convert-unmanaged-to-managed-disks.md)         |
|Actualizar máquinas virtuales con discos no administrados estándares a máquinas virtuales con discos administrados premium     | Primero, [convertir una máquina virtual de Windows de unmanaged disks a managed disks](convert-unmanaged-to-managed-disks.md). A continuación, [actualizar el tipo de almacenamiento de un disco administrado](convert-disk-storage.md).         |

## <a name="next-steps"></a>Pasos siguientes

- Más información sobre [Managed Disks](managed-disks-overview.md)
- Revise el [precio de Managed Disks](https://azure.microsoft.com/pricing/details/managed-disks/).
