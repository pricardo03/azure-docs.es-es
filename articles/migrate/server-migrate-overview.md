---
title: Selección de una opción de migración de VMware con Azure Migrate Server Migration | Microsoft Docs
description: Proporciona información general sobre las opciones para migrar máquinas virtuales de VMware a Azure con Azure Migrate Server Migration.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 07/09/2019
ms.author: raynew
ms.openlocfilehash: f8bfbe26dc4c6ddbcf622f91938ba060de00b2ec
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2019
ms.locfileid: "67810151"
---
# <a name="select-a-vmware-migration-option"></a>Selección de una opción de migración de VMware

Puede migrar las máquinas virtuales de VMware a Azure mediante la herramienta Azure Migrate Server Migration. Esta herramienta ofrece un par de opciones para la migración de máquinas virtuales de VMware:

- Migración mediante la replicación sin agente. Las máquinas virtuales se migran sin necesidad de instalar nada en ellas.
- Migración con un agente de replicación. Instale un agente en la máquina virtual para su replicación.


Si bien la replicación sin agente es más fácil desde una perspectiva de implementación, actualmente tiene una serie de limitaciones.

## <a name="agentless-migration-limitations"></a>Limitaciones de la migración sin agente

Las limitaciones son las siguientes:

- **Replicación simultánea**: se puede replicar un máximo de 50 máquinas virtuales simultáneamente desde una instancia de vCenter Server.<br/> Si tiene más de 50 máquinas virtuales para la migración, cree varios lotes de máquinas.<br/> Si replica más de cada vez, afectará al rendimiento.
- **Discos de máquina virtual**: si quiere migrar una máquina virtual, debe tener 60 discos o menos.
- **Sistemas operativos de máquina virtual**: en general, Azure Migrate pueden migrar cualquier sistema operativo Windows Server o Linux, pero es posible que sea necesario realizar cambios en las máquinas virtuales para que puedan ejecutarse en Azure. Azure Migrate realiza los cambios automáticamente en estos sistemas operativos:
    - Red Hat Enterprise Linux 6.5+, 7.0+
    - CentOS 6.5+, 7.0+
    - SUSE Linux Enterprise Server 12 SP1+
    - Ubuntu 14.04LTS, 16.04LTS, 18.04LTS
    - Debian 7, 8
    - En el caso de otros sistemas operativos, debe hacer los ajustes manualmente antes de la migración. En el [tutorial de migración](tutorial-migrate-vmware.md) se explica cómo hacerlo.
- **Arranque de Linux**: si /boot está en una partición dedicada, debe residir en el disco del sistema operativo y no distribuirse en varios discos.<br/> Si /boot forma parte de la partición raíz (/), la partición "/" debe estar en el disco del sistema operativo y no abarcar otros discos.
- **Arranque UEFI**: las máquinas virtuales con arranque UEFI no se admiten para la migración.
- **Volúmenes y discos cifrados (BitLocker, cryptfs**): las máquinas virtuales con volúmenes o discos cifrados no se admiten para la migración.
- **Discos RDM/de acceso directo**: si las máquinas virtuales tienen discos RDM o de acceso directo, estos discos no se replicarán en Azure.
- **NFS**: los volúmenes NFS montados como volúmenes en las máquinas virtuales no se replicarán.
- **Almacenamiento de destino**: solo se pueden migrar máquinas virtuales de VMware a máquinas virtuales de Azure con discos administrados (HDD estándar, SSD Premium).



## <a name="deployment-steps-comparison"></a>Comparación de los pasos de implementación

Después de revisar las limitaciones, una descripción de los pasos necesarios para implementar cada solución puede ayudarle a decidir qué opción elegir.

**Task** | **Detalles** |**Sin agente** | **Basado en agente**
--- | --- | --- | ---
**Preparación de máquinas virtuales y servidor de VMware para la migración** | Configure diversas opciones en máquinas virtuales y servidores de VMware. | Obligatorio | Obligatorio
**Incorporación de la herramienta Server Migration** | Agregue la herramienta Azure Migrate Server Migration en el proyecto de Azure Migrate. | Obligatorio | Obligatorio
**Implementación del dispositivo de Azure Migrate** | Configure un dispositivo ligero en una máquina virtual de VMware para la detección y la valoración de máquinas virtuales. | Obligatorio | No se requiere.
**Instalación de Mobility Service en máquinas virtuales** | Instale Mobility Service en cada máquina que quiera replicar. | No se requiere | Obligatorio
**Implementación del dispositivo de replicación de Azure Migrate Server Migration** | Configure un dispositivo en una máquina virtual de VMware para detectar máquinas virtuales y para que actúe como puente entre la instancia de Mobility Service que se ejecuta en las máquinas virtuales y Azure Migrate Server Migration. | No se requiere | Obligatorio
**Replicación de máquinas virtuales**, habilitación de la replicación de máquinas virtuales | Configure las opciones de replicación y seleccione las máquinas virtuales que se van a replicar. | Obligatorio | Obligatorio
**Ejecución de una migración de prueba** | Ejecute una migración de prueba para asegurarse de que todo funciona de la forma esperada. | Obligatorio | Obligatorio
**Ejecución de una migración completa** | Migre las máquinas virtuales. | Obligatorio | Obligatorio




## <a name="next-steps"></a>Pasos siguientes

[Migre máquinas virtuales de VMware](tutorial-migrate-vmware.md) con migración sin agentes.



