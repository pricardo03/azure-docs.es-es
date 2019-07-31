---
title: archivo de inclusión
description: archivo de inclusión
services: virtual-machines
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 05/10/2019
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: 1582f61befb9b6d71adbfda9482175d67874ffb9
ms.sourcegitcommit: fa45c2bcd1b32bc8dd54a5dc8bc206d2fe23d5fb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/12/2019
ms.locfileid: "67850653"
---
# <a name="availability-options-for-virtual-machines-in-azure"></a>Opciones de disponibilidad para las máquinas virtuales de Azure
En este artículo proporciona una visión general de las características de disponibilidad de las máquinas virtuales (VM) de Azure.


## <a name="availability-sets"></a>Conjuntos de disponibilidad
Un conjunto de disponibilidad es una agrupación lógica de máquinas virtuales dentro de un centro de datos que permite a Azure conocer cómo se crea su aplicación para proporcionar redundancia y disponibilidad. Se recomienda la creación de dos, o más, máquinas virtuales en un conjunto de disponibilidad no solo para proporcionar una aplicación de alta disponibilidad sino también para cumplir el [99,95 % del Acuerdo de Nivel de Servicio de Azure](https://azure.microsoft.com/support/legal/sla/virtual-machines/). No hay ningún costo asociado con el conjunto de disponibilidad propiamente dicho, solo se paga por cada instancia de máquina virtual que cree. Cuando una sola máquina virtual usa [discos SSD Premium de Azure](../articles/virtual-machines/windows/disks-types.md#premium-ssd), se aplica el Acuerdo de Nivel de Servicio de Azure para los eventos de mantenimiento no planeados.

El conjunto de disponibilidad consta de dos agrupaciones adicionales que protegen contra errores de hardware y permiten la aplicación segura de las actualizaciones: dominios de error (FD) y dominios de actualización (UD). Puede obtener más información acerca de cómo administrar la disponibilidad de [máquinas virtuales con Linux](../articles/virtual-machines/linux/manage-availability.md) o [máquinas virtuales con Windows](../articles/virtual-machines/windows/manage-availability.md).

### <a name="fault-domains"></a>Dominios de error
Un dominio de error es un grupo lógico de hardware subyacente que comparte la fuente de alimentación y el conmutador de red, similar a un bastidor dentro de un centro de datos local. Cuando se crean máquinas virtuales en un conjunto de disponibilidad, la plataforma de Azure las distribuye automáticamente entre estos dominios de error. Este enfoque limita el impacto de potenciales errores de hardware físico, interrupciones de red o cortes de alimentación eléctrica.

### <a name="update-domains"></a>Dominios de actualización
Un dominio de actualización es un grupo lógico de hardware subyacente que puede someterse a mantenimiento o reiniciarse al mismo tiempo. Cuando se crean máquinas virtuales en un conjunto de disponibilidad, la plataforma de Azure las distribuye automáticamente entre estos dominios de error. Este enfoque garantiza que al menos una instancia de la aplicación sigue ejecutándose cuando se realiza el mantenimiento periódico de la plataforma Azure. Es posible que el orden en que se reinician los dominios de actualización no siga una secuencia durante un mantenimiento planeado, pero se reinician de uno en uno.

![Conjuntos de disponibilidad](./media/virtual-machines-common-manage-availability/ud-fd-configuration.png)

### <a name="managed-disk-fault-domains"></a>Dominios de error de Managed Disks
Para las máquinas virtuales que usen [Azure Managed Disks](../articles/virtual-machines/windows/faq-for-disks.md), las máquinas virtuales se alinean con los dominios de error de disco administrado cuando se usa un conjunto de disponibilidad administrada. Esta alineación garantiza que todos los discos administrados conectados a una máquina virtual se encuentran en el mismo dominio de error de disco administrado. Solo se pueden crear máquinas virtuales con discos administrados en un conjunto de disponibilidad administrada. El número de dominios de error de disco administrado varía según la región: dos o tres dominios de error de disco administrado por región. Para más información sobre estos dominios de error de Managed Disks consulte [máquinas virtuales Linux](../articles/virtual-machines/linux/manage-availability.md?#use-managed-disks-for-vms-in-an-availability-set) o [máquinas virtuales Windows](../articles/virtual-machines/windows/manage-availability.md?#use-managed-disks-for-vms-in-an-availability-set).

![Conjunto de disponibilidad administrado](./media/virtual-machines-common-manage-availability/md-fd-updated.png)

## <a name="availability-zones"></a>Zonas de disponibilidad

Las [zonas de disponibilidad](../articles/availability-zones/az-overview.md) son una alternativa a los conjuntos de disponibilidad que permite ampliar el nivel de control que tiene para mantener la disponibilidad de las aplicaciones y los datos en las máquinas virtuales. Una zona de disponibilidad es una zona separada físicamente dentro de una región de Azure. Hay tres zonas de disponibilidad por cada región de Azure admitida. Cada zona de disponibilidad tiene una fuente de alimentación, una red y un sistema de refrigeración distintos. Si diseña las soluciones para que utilicen máquinas virtuales replicadas en zonas, podrá proteger sus datos y aplicaciones frente a la pérdida de un centro de datos. Aunque una zona esté en peligro, las aplicaciones y los datos replicados estarán disponibles instantáneamente en otra zona. 

![Zonas de disponibilidad](./media/virtual-machines-common-regions-and-availability/three-zones-per-region.png)

Obtenga más información acerca de cómo implementar una máquina virtual [Windows](../articles/virtual-machines/windows/create-powershell-availability-zone.md) o [Linux](../articles/virtual-machines/linux/create-cli-availability-zone.md) en una zona de disponibilidad.


## <a name="next-steps"></a>Pasos siguientes
Ya puede empezar a usar estas características de disponibilidad y redundancia para crear un entorno de Azure. Para información sobre los procedimientos recomendados, consulte [Lista de comprobación de disponibilidad](../articles/best-practices-availability-checklist.md).

