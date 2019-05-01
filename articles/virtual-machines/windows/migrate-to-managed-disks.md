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
ms.date: 01/03/2018
ms.author: rogarana
ms.subservice: disks
ms.openlocfilehash: 12cd1caa4cb96dbd5862776589d4a34aeb294ca1
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2019
ms.locfileid: "64689733"
---
# <a name="migrate-azure-vms-to-managed-disks-in-azure"></a>Migración de VM de Azure a Managed Disks en Azure

Azure Managed Disks simplifica la administración de almacenamiento al eliminar la necesidad de administrar cuentas de almacenamiento por separado.  También puede migrar las VM de Azure existentes a Managed Disks para aprovechar la mejor confiabilidad de las VM en un conjunto de disponibilidad. Esto garantiza que los discos de las distintas VM de un conjunto de disponibilidad estén lo suficientemente aislados entre sí como para evitar un único punto de error. Coloca automáticamente discos de distintas VM en un conjunto de disponibilidad en unidades de escalado de almacenamiento diferentes (marcas de tiempo), lo que limita el impacto de errores únicos de unidad de escalado de almacenamiento generados debido a errores de hardware y software.
En virtud de sus necesidades, puede elegir entre cuatro tipos de opciones de almacenamiento. Para información acerca de los tipos de disco disponibles, consulte nuestro artículo [Selección de un tipo de disco](disks-types.md).

## <a name="migrate-scenarios"></a>Escenarios de migración

Puede migrar a Managed Disks en los escenarios siguientes:

| **Migrar...**                                            | **Vínculo a documentación**                                                                                                                                                                                                                                                                  |
|----------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Conversión de VM independientes y VM de un conjunto de disponibilidad para establecer discos administrados   | [Conversión de VM para usar discos administrados](convert-unmanaged-to-managed-disks.md) |
| Una VM única desde el modelo clásico a Resource Manager en discos administrados     | [Creación de una VM desde un disco duro virtual clásico](create-vm-specialized-portal.md)  | 
| Todas las VM de una red virtual desde el modelo clásico a Resource Manager en discos administrados     | [Migración de recursos de IaaS desde el modelo clásico a Resource Manager](migration-classic-resource-manager-ps.md) y luego [Conversión de una VM desde discos no administrados a discos administrados](convert-unmanaged-to-managed-disks.md) | 


## <a name="next-steps"></a>Pasos siguientes

- Más información sobre [Managed Disks](managed-disks-overview.md)
- Revise el [precio de Managed Disks](https://azure.microsoft.com/pricing/details/managed-disks/).
