---
title: Preguntas comunes sobre Azure Migrate Server Migration
description: Obtenga respuestas a preguntas comunes sobre Azure Migrate Server Migration.
ms.topic: conceptual
ms.date: 02/06/2020
ms.openlocfilehash: bae3447f0fada18de5473e1ef1a1c1d431535f63
ms.sourcegitcommit: db2d402883035150f4f89d94ef79219b1604c5ba
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/07/2020
ms.locfileid: "77065999"
---
# <a name="azure-migrate-server-migration-common-questions"></a>Azure Migrate Server Migration: Preguntas frecuentes

En este artículo se responde a preguntas habituales sobre Azure Migrate: Server Migration. Si tiene más preguntas después de leer este artículo, publíquelas en el [Foro de Azure Migrate](https://aka.ms/AzureMigrateForum). Si tiene otras preguntas, repase estos artículos:

- [Preguntas generales](resources-faq.md) sobre Azure Migrate.
- [Preguntas](common-questions-appliance.md) sobre el dispositivo de Azure Migrate.
- [Preguntas](common-questions-discovery-assessment.md) sobre la detección, valoración y visualización de dependencias.


## <a name="how-does-agentless-vmware-replication-work"></a>¿Cómo funciona la replicación sin agente de VMware?

El método de replicación sin agente de VMware emplea instantáneas de VMware y seguimiento de bloques modificados (CBT) en VMware. Cuando el usuario inicia la replicación, se programa un ciclo de replicación inicial. En este ciclo, se toma una instantánea de la máquina virtual y esta instantánea se usa para replicar los VMDK (discos) de las máquinas virtuales. Una vez completado el ciclo de replicación inicial, se programan ciclos de replicación diferencial periódicamente. En el ciclo de replicación diferencial, se toma una instantánea y se replican los bloques de datos que han cambiado desde el ciclo de replicación anterior. El seguimiento de bloques modificados en VMware se usa para determinar los bloques que han cambiado desde el último ciclo.
El servicio administra automáticamente la frecuencia de los ciclos de replicación periódicos en función de cuántas otras máquinas virtuales o discos estén replicando a la vez el mismo almacén de datos; en condiciones óptimas, se juntarán en 1 ciclo por hora por máquina virtual.

Al migrar, se programa un ciclo de replicación a petición para que la máquina virtual capture el resto de los datos. Puede optar por apagar la máquina virtual como parte de la migración para garantizar la coherencia de la aplicación y que no haya pérdida de datos.

## <a name="why-is-the-resynchronization-option-not-exposed-in-agentless-stack"></a>¿Por qué la opción de resincronización no se expone en una pila sin agente?

En la pila sin agente, en cada ciclo diferencial, se transfiere la diferencia entre la instantánea actual y la instantánea anterior que se ha tomado. Dado que siempre hay una diferencia entre las instantáneas, existe la ventaja de plegar los datos (es decir, si un sector determinado se escribe "n" veces entre las instantáneas, solo necesitamos transferir la última escritura, ya que solo nos interesa la última sincronización). Esto es diferente de la pila basada en el agente, donde se realiza el seguimiento de todas las escrituras y se aplican. Esto significa que cada ciclo diferencial es una resincronización. Por lo tanto, no se expone ninguna opción de resincronización. 

Si alguna vez los discos no están sincronizados debido a un error, se corrigen en el siguiente ciclo. 

## <a name="what-is-the-impact-of-churn-rate-if-i-use-agentless-replication"></a>¿Qué efecto tiene la tasa de renovación si se usa la replicación sin agente?

Dado que la pila depende de los datos que se van a plegar más que de la tasa de renovación, lo que importa en esta pila es el patrón de renovación. Un patrón en el que un archivo se escribe una y otra vez no tiene un gran efecto. Sin embargo, un patrón en el que se escriben todos los demás sectores producirá una gran renovación en el ciclo siguiente. Dado que se reduce la cantidad de datos que se transfieren, se permite que los datos se plieguen lo más posible antes de programar el siguiente ciclo.  

## <a name="how-frequently-is-a-replication-cycle-scheduled"></a>¿Con qué frecuencia se programa un ciclo de replicación?

La fórmula para programar el siguiente ciclo de replicación es esta: Tiempo de ciclo anterior/2 o 1 hora, lo que sea mayor. Por ejemplo, si una máquina virtual tardó cuatro horas en un ciclo diferencial, su siguiente ciclo se programará en 2 horas y no en la próxima hora. Esto varía cuando el ciclo está inmediatamente después de IR, en cuyo caso se programa el primer ciclo diferencial inmediatamente.

## <a name="what-is-the-impact-on-performance-of-vcenter-server-or-esxi-host-while-using-agentless-replication"></a>¿Qué efecto tiene en el rendimiento un host de vCenter Server o ESXi mientras se usa la replicación sin agente?

Dado que la replicación sin agente usa instantáneas, habrá consumo de IOPS en el almacenamiento y los clientes necesitarán algo de espacio de IOPS en él. No se recomienda usar esta pila en entornos con restricción de almacenamiento o IOPS.

## <a name="does-agentless-migration-stack-support-migration-of-uefi-vms-to-azure-gen-2-vms"></a>¿La pila de migración sin agente admite la migración de máquinas virtuales UEFI a máquinas virtuales de Azure Gen 2?

No, debe usar Azure Site Recovery para migrar estas máquinas virtuales a máquinas virtuales de Azure Gen 2. 

## <a name="can-i-pin-my-vms-to-azure-availability-zones-when-i-migrate"></a>¿Se pueden anclar las máquinas virtuales a Azure Availability Zones al migrarlas?

No, no existe ahí compatibilidad con Azure Availability Zones.

## <a name="which-transport-protocol-is-used-by-azure-migrate-during-replication"></a>¿Qué protocolo de transporte usa Azure Migrate durante la replicación?

Azure Migrate usa el protocolo de dispositivo de bloque de red (NBD) con cifrado SSL.

## <a name="what-is-the-minimum-vcenter-server-version-required-for-migration"></a>¿Cuál es la versión mínima de vCenter Server necesaria para la migración?

Debe tener al menos vCenter Server 5.5 y la versión 5.5 del host de ESXi de VMware vSphere.

## <a name="can-customers-migrate-their-vms-to-unmanaged-disks"></a>¿Los clientes pueden migrar sus máquinas virtuales a discos no administrados?

No. Azure Migrate solo admite la migración a discos administrados (HDD estándar, SSD premium).

## <a name="how-many-vms-can-replicate-simultaneously-using-agentless-vmware-stack"></a>¿Cuántas máquinas virtuales se pueden replicar a la vez mediante la pila de VMware sin agente?

Actualmente, los clientes pueden migrar 100 máquinas virtuales por vCenter Server a la vez. Esta operación se puede hacer en lotes de 10 máquinas virtuales.




