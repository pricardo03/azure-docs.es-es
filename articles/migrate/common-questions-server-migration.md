---
title: Preguntas comunes sobre Azure Migrate Server Migration
description: Obtenga respuestas a preguntas habituales sobre la migración de máquinas con Azure Migrate Server Migration
ms.topic: conceptual
ms.date: 02/17/2020
ms.openlocfilehash: 0c967027457b925b45ea19d994cfadfdbd0b8ab3
ms.sourcegitcommit: b8f2fee3b93436c44f021dff7abe28921da72a6d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/18/2020
ms.locfileid: "77425840"
---
# <a name="azure-migrate-server-migration-common-questions"></a>Azure Migrate Server Migration: Preguntas frecuentes

Este artículo contiene respuestas a algunas preguntas frecuentes sobre la herramienta Azure Migrate: Server Migration. Si tiene más preguntas después de leer este artículo, consulte estos artículos:

- [Preguntas generales](resources-faq.md) sobre Azure Migrate.
- [Preguntas](common-questions-appliance.md) sobre el dispositivo de Azure Migrate.
- [Preguntas](common-questions-discovery-assessment.md) sobre la detección, valoración y visualización de dependencias.
- Publique preguntas en el [foro de Azure Migrate](https://aka.ms/AzureMigrateForum).


## <a name="how-does-agentless-vmware-replication-work"></a>¿Cómo funciona la replicación sin agente de VMware?

El método de replicación sin agente de VMware emplea instantáneas de VMware y seguimiento de bloques modificados (CBT) en VMware.

1. Cuando inicia la replicación, se programa un ciclo de replicación inicial. En el ciclo inicial, se toma una instantánea de la VM. Esta instantánea se usa para replicar los VMDK (discos) de la VM. 
2. Una vez finalizado el ciclo de replicación inicial, se programan ciclos de replicación diferencial periódicamente.
    - Durante la replicación diferencial, se toma una instantánea y se replican los bloques de datos que han cambiado desde el ciclo de replicación anterior.
    - El seguimiento de bloques modificados en VMware se usa para determinar los bloques que han cambiado desde el último ciclo.
    - Azure Migrate administra automáticamente la frecuencia de los ciclos de replicación periódicos en función de cuántas otras VM o discos estén replicando a la vez el mismo almacén de datos. En condiciones ideales, la replicación finalmente converge en un ciclo por hora para cada VM.

Al migrar, se programa un ciclo de replicación a petición para que la máquina capture el resto de los datos. Puede optar por apagar la máquina durante la migración para garantizar la coherencia de la aplicación y que no haya pérdida de datos.

## <a name="why-isnt-resynchronization-exposed"></a>¿Por qué no se expone la resincronización?

Durante la migración sin agente, en cada ciclo diferencial se escribe la diferencia entre la instantánea actual y la instantánea tomada previamente. Puesto que siempre es la diferencia entre las instantáneas, los datos se incorporan. De modo que si un sector determinado se escribe N veces entre instantáneas, solo se debe transferir la última escritura, ya que solo nos interesa la última sincronización. Esto difiere de la replicación basada en agente, donde se realiza un seguimiento de todas las escrituras y se aplican. Esto significa que cada ciclo diferencial es una resincronización. Por lo tanto, no se expone ninguna opción de resincronización. Si alguna vez los discos no están sincronizados debido a un error, se corrige en el siguiente ciclo. 

## <a name="how-does-churn-rate-impact-agentless-replication"></a>¿Cómo afecta la tasa de modificación a la replicación sin agente?

Dado que la replicación sin agente incorpora la fecha, el patrón de modificación es más importante que la tasa de modificación. Cuando un archivo se escribe una y otra vez, la tasa no tiene un gran efecto. Sin embargo, un patrón en el que se escriben todos los demás sectores produce una gran renovación en el ciclo siguiente. Dado que se reduce la cantidad de datos que se transfieren, se permite que los datos se plieguen lo más posible antes de programar el siguiente ciclo.  

## <a name="how-frequently-is-a-replication-cycle-scheduled"></a>¿Con qué frecuencia se programa un ciclo de replicación?

Fórmula para programar el siguiente ciclo de replicación: (Tiempo de ciclo anterior/2) o 1 hora, lo que sea mayor.

Por ejemplo, si una VM tarda cuatro horas en un ciclo diferencial, su siguiente ciclo se programa dos horas después y no en la próxima hora. Esto es distinto inmediatamente después de la replicación inicial, en la que el primer ciclo diferencial se programa inmediatamente.

## <a name="how-does-agentless-replication-impact-vmware-servers"></a>¿Cómo afecta la replicación sin agente a los servidores de VMware?

Hay un impacto en el rendimiento de los hosts de vCenter Server/ESXi. Dado que la replicación sin agente utiliza instantáneas, consume IOPS en el almacenamiento y se necesita ancho de banda de almacenamiento de IOPS. No se recomienda usar la replicación sin agente si hay restricciones en el almacenamiento/IOPS en el entorno.

## <a name="can-i-do-agentless-migration-of-uefi-vms-to-azure-gen-2"></a>¿Puedo realizar la migración sin agente de VM UEFI a Azure Gen 2?

No. Use Azure Site Recovery para migrar estas VM a VM de Azure Gen 2. 

## <a name="can-i-pin-vms-to-azure-availability-zones-when-i-migrate"></a>¿Se pueden anclar VM a Azure Availability Zones al migrarlas?

No, no se admite Azure Availability Zones.

## <a name="which-transport-protocol-is-used-by-azure-migrate-during-replication"></a>¿Qué protocolo de transporte usa Azure Migrate durante la replicación?

Azure Migrate usa el protocolo de dispositivo de bloque de red (NBD) con cifrado SSL.

## <a name="what-is-the-minimum-vcenter-server-version-required-for-migration"></a>¿Cuál es la versión mínima de vCenter Server necesaria para la migración?

Debe tener al menos vCenter Server 5.5 y la versión 5.5 del host de ESXi de VMware vSphere.

## <a name="can-customers-migrate-their-vms-to-unmanaged-disks"></a>¿Los clientes pueden migrar sus máquinas virtuales a discos no administrados?

No. Azure Migrate solo admite la migración a discos administrados (HDD estándar, SSD premium).

## <a name="how-many-vms-can-i-replicate-together-with-agentless-migration"></a>¿Cuántas VM se pueden replicar con la migración sin agente?

Actualmente, se pueden migrar 100 máquinas virtuales por vCenter Server a la vez. Realice la migración en lotes de 10 VM.
 



