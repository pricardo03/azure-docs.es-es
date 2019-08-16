---
title: Novedades de Azure Site Recovery | Microsoft Docs
description: Proporciona un resumen de las características nuevas presentadas en Azure Site Recovery
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: conceptual
ms.date: 07/30/2019
ms.author: raynew
ms.openlocfilehash: 1ce71665a924c6681af418075d8cc635c73f8a1a
ms.sourcegitcommit: a52f17307cc36640426dac20b92136a163c799d0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/01/2019
ms.locfileid: "68717670"
---
# <a name="whats-new-in-site-recovery"></a>Novedades de Site Recovery

El servicio [Azure Site Recovery](site-recovery-overview.md) se actualiza y mejora de manera continua. Para ayudarlo a mantenerse actualizado, en este artículo encontrará información sobre las versiones más recientes, las características nuevas y el nuevo contenido. Esta página se actualiza de manera periódica.

Si tiene sugerencias sobre las características de Site Recovery, nos encantaría [conocer sus comentarios](https://feedback.azure.com/forums/256299-site-recovery).


## <a name="installing-updates"></a>Instalación de actualizaciones

[Revise este artículo](service-updates-how-to.md) para instrucciones y soporte técnico para las actualizaciones de Site Recovery.

## <a name="updates-july-2019"></a>Actualizaciones (julio de 2019)

### <a name="update-rollup-38"></a>Paquete acumulativo de actualizaciones 38

El [Paquete acumulativo de actualizaciones 38](https://support.microsoft.com/help/4513507/) proporciona las siguientes actualizaciones.

**Actualizar** | **Detalles**
--- | ---
**Proveedores y agentes** | Actualizaciones de los proveedores y agentes de Site Recovery (como se detalla en el paquete acumulativo de actualizaciones)
**Mejoras y correcciones de problemas** | Un número de correcciones y mejoras (tal como se detalla en el paquete acumulativo)


### <a name="general"></a>General

Site Recovery ahora admite la utilización de cuentas de almacenamiento de uso general versión 2 para el almacenamiento en caché o el almacenamiento de destino. Anteriormente solo se admitía la versión 1.

### <a name="vmware-to-azure-disaster-recovery"></a>Recuperación ante desastres de VMware a Azure

Ahora puede replicar discos de hasta 8 TB al replicar en una máquina virtual de Azure con discos administrados.


## <a name="updates-june-2019"></a>Actualizaciones (junio de 2019)

### <a name="update-rollup-37"></a>Paquete acumulativo de actualizaciones 37

El [Paquete acumulativo de actualizaciones 37](https://support.microsoft.com/help/4508614/) proporciona las siguientes actualizaciones.

**Actualizar** | **Detalles**
--- | ---
**Proveedores y agentes** | Actualizaciones de los proveedores y agentes de Site Recovery (como se detalla en el paquete acumulativo de actualizaciones)
**Mejoras y correcciones de problemas** | Un número de correcciones y mejoras (tal como se detalla en el paquete acumulativo)


### <a name="vmwarephysical-server-disaster-recovery"></a>Recuperación ante desastres de un servidor físico/VMware

Las características que se agregaron este mes se resumen en la tabla.

**Característica** | **Detalles**
--- | ---
**Particiones GPT** | A partir del Paquete acumulativo de actualizaciones 37 y versiones posteriores (versión 9.25.5241.1 de Mobility Service), se admiten hasta cinco particiones GPT en UEFI. Antes de esta actualización, se admitían cuatro.



## <a name="updates-may-2019"></a>Actualizaciones (mayo de 2019)

### <a name="update-rollup-36"></a>Paquete acumulativo de actualizaciones 36

El [Paquete acumulativo de actualizaciones 36](https://support.microsoft.com/help/4503156) proporciona las siguientes actualizaciones.

**Actualizar** | **Detalles**
--- | ---
**Proveedores y agentes** | Actualización de los proveedores y agentes de Site Recovery (como se detalla en el paquete acumulativo de actualizaciones)
**Mejoras y correcciones de problemas** | Un número de correcciones y mejoras (tal como se detalla en el paquete acumulativo)

### <a name="azure-vm-disaster-recovery"></a>Recuperación ante desastres en VM de Azure

Las características que se agregaron este mes se resumen en la tabla.

**Característica** | **Detalles**
--- | ---
**Replicar discos agregados** | La habilitación de la replicación para disco de datos se agregó a una instancia de Azure Virtual Machines que ya está habilitada para la recuperación ante desastres. [Más información](azure-to-azure-enable-replication-added-disk.md).
**Actualizaciones automáticas** | Ahora, cuando configure las actualizaciones automáticas de la extensión Mobility Service que se ejecute en instancias de Azure Virtual Machines habilitadas para la recuperación ante desastres, puede seleccionar una cuenta de automatización que ya tenga para usarla en lugar de usar la cuenta predeterminada que crea Site Recovery. [Más información](azure-to-azure-autoupdate.md).


### <a name="vmwarephysical-server-disaster-recovery"></a>Recuperación ante desastres de un servidor físico/VMware

Las características que se agregaron este mes se resumen en la tabla.

**Característica** | **Detalles**
--- | ---
**Supervisión del servidor de procesos** | Ahora, para la recuperación ante desastres de máquinas virtuales VMware y servidores físicos en el entorno local, puede supervisar y solucionar problemas de los servidores de procesos con funcionalidad mejorada de alertas e informes sobre el estado de mantenimiento de los servidores. [Más información](vmware-physical-azure-monitor-process-server.md). 





## <a name="updates-march-2019"></a>Actualizaciones (marzo de 2019)

### <a name="update-rollup-35"></a>Paquete acumulativo de actualizaciones 35

El [Paquete acumulativo de actualizaciones 35](https://support.microsoft.com/en-us/help/4494485/update-rollup-35-for-azure-site-recovery) proporciona las siguientes actualizaciones.

**Actualizar** | **Detalles**
--- | ---
**Proveedores y agentes** | Actualización de los proveedores y agentes de Site Recovery (como se detalla en el paquete acumulativo de actualizaciones)
**Mejoras y correcciones de problemas** | Un número de correcciones y mejoras (tal como se detalla en el paquete acumulativo)

### <a name="vmwarephysical-server-disaster-recovery"></a>Recuperación ante desastres de un servidor físico/VMware

Las características que se agregaron este mes se resumen en la tabla.

**Característica** | **Detalles**
--- | ---
**Discos administrados** | La replicación de servidores físicos y máquinas virtuales de VMware locales es ahora directamente en discos administrados en Azure. Los datos locales se envían a una cuenta de almacenamiento de caché de Azure y los puntos de recuperación se crean en discos administrados en la ubicación de destino. Esto garantiza que no es necesario administrar varias cuentas de almacenamiento de destino.
**Servidor de configuración** | Site Recovery ahora admite servidores de configuración con diferentes NIC. Agregue adaptadores adicionales a la máquina virtual del servidor de configuración antes de registrar este en el almacén. Si lo agrega más tarde, deberá volver a registrar el servidor en el almacén.


## <a name="updates-february-2019"></a>Actualizaciones (febrero de 2019)

### <a name="update-rollup-34"></a>Paquete acumulativo de actualizaciones 34 

El [Paquete acumulativo de actualizaciones 34](https://support.microsoft.com/help/4490016/update-rollup-34-for-azure-site-recovery) proporciona las siguientes actualizaciones.

**Actualizar** | **Detalles**
--- | ---
**Proveedores y agentes** | Actualización de los proveedores y agentes de Site Recovery (como se detalla en el paquete acumulativo de actualizaciones).
**Mejoras y correcciones de problemas** | Un número de correcciones y mejoras (tal como se detalla en el paquete acumulativo).


### <a name="update-rollup-33"></a>Paquete acumulativo de actualizaciones 33 

El [Paquete acumulativo de actualizaciones 33](https://support.microsoft.com/help/4489582/update-rollup-33-for-azure-site-recovery) proporciona las siguientes actualizaciones.

**Actualizar** | **Detalles**
--- | ---
**Proveedores y agentes** | Actualización de los proveedores y agentes de Site Recovery (como se detalla en el paquete acumulativo de actualizaciones).
**Mejoras y correcciones de problemas** | Un número de correcciones y mejoras (tal como se detalla en el paquete acumulativo).


### <a name="azure-vm-disaster-recovery"></a>Recuperación ante desastres en VM de Azure 
Las características que se agregaron este mes se resumen en la tabla.

**Característica** | **Detalles**
--- | ---
**Asignación de red** | Para la recuperación ante desastres de VM de Azure, puede usar cualquier red de destino disponible cuando se habilite la replicación. 
**SSD estándar** | Ahora puede configurar la recuperación ante desastres para VM de Azure con [discos SSD estándar](https://docs.microsoft.com/azure/virtual-machines/windows/disks-standard-ssd).
**Espacios de almacenamiento directo** | Puede configurar la recuperación ante desastres para aplicaciones que se ejecutan en aplicaciones de la VM de Azure mediante el uso de [Espacios de almacenamiento directo ](https://docs.microsoft.com/windows-server/storage/storage-spaces/storage-spaces-direct-overview) para lograr alta disponibilidad.  Usar los Espacios de almacenamiento directo (S2D) junto con Site Recovery proporciona una protección exhaustiva de las cargas de trabajo de una VM de Azure. S2D le permite hospedar un clúster invitado en Azure. Esto es especialmente útil cuando una VM hospeda una aplicación crítica, como capa de ASCS de SAP, SQL Server o un servidor de archivos de escalabilidad horizontal.


### <a name="vmwarephysical-server-disaster-recovery"></a>Recuperación ante desastres de un servidor físico/VMware
Las características que se agregaron este mes se resumen en la tabla.

**Característica** | **Detalles**
--- | ---
**Sistema de archivos BRTFS de Linux** | Site Recovery ahora admite la replicación de VM de VMware con el sistema de archivos BRTFS. No se admite la replicación si:<br/><br/>- El subvolumen del sistema de archivos BTRFS se cambia después de habilitar la replicación.<br/><br/>- El sistema de archivos está repartido entre varios discos.<br/><br/>- El sistema de archivos BTRFS es compatible con RAID.
**Windows Server 2019** | Compatibilidad agregada para máquinas que ejecutan Windows Server 2019.


## <a name="updates-january-2019"></a>Actualizaciones (enero de 2019)


### <a name="accelerated-networking-azure-vms"></a>Redes aceleradas (VM de Azure)

Redes aceleradas habilita la virtualización de E/S de raíz única (SR-IOV) en una máquina virtual, lo que mejora su rendimiento en la red. Al habilitar la replicación de una máquina virtual de Azure, Site Recovery detecta si las redes aceleradas están habilitadas. Si es así, después de la conmutación por error, Site Recovery configura automáticamente las redes aceleradas en la máquina virtual de Azure de réplica de destino, tanto para [Windows](https://docs.microsoft.com/azure/virtual-network/create-vm-accelerated-networking-powershell#enable-accelerated-networking-on-existing-vms) como para [Linux](https://docs.microsoft.com/azure/virtual-network/create-vm-accelerated-networking-cli#enable-accelerated-networking-on-existing-vms).

[Más información](azure-vm-disaster-recovery-with-accelerated-networking.md).

### <a name="update-rollup-32"></a>Paquete acumulativo de actualizaciones 32 

El [Paquete acumulativo de actualizaciones 32](https://support.microsoft.com/help/4485985/update-rollup-32-for-azure-site-recovery) proporciona las siguientes actualizaciones.

**Actualizar** | **Detalles**
--- | ---
**Proveedores y agentes** | Actualización de los proveedores y agentes de Site Recovery (como se detalla en el paquete acumulativo de actualizaciones).
**Mejoras y correcciones de problemas** | Un número de correcciones y mejoras (tal como se detalla en el paquete acumulativo).

### <a name="azure-vm-disaster-recovery"></a>Recuperación ante desastres en VM de Azure

Las características que se agregaron este mes se resumen en la tabla.

**Característica** | **Detalles**
--- | ---
**Soporte técnico de Linux** | Se agregó soporte para RedHat Workstation 6/7; versiones nuevas de kernel para Ubuntu, Debian y SUSE.
**Espacios de almacenamiento directo** | Site Recovery admite VM de Azure que usan Espacios de almacenamiento directo (S2D).

### <a name="vmware-vmsphysical-servers-disaster-recovery"></a>Recuperación ante desastres de servidores físicos/VM de VMware

Las características que se agregaron este mes se resumen en la tabla.
 
**Característica** | **Detalles**
--- | ---
**Soporte técnico de Linux** | Se agregó soporte para RedHat Enterprise Linux 7.6; RedHat Workstation 6/7; Oracle Linux 6.10/7.6 y nuevas versiones de kernel para Ubuntu, Debian y SUSE.


### <a name="update-rollup-31"></a>Paquete acumulativo de actualizaciones 31 

El [Paquete acumulativo de actualizaciones 31](https://support.microsoft.com/help/4478871/update-rollup-31-for-azure-site-recovery) proporciona las siguientes actualizaciones.

**Actualizar** | **Detalles**
--- | ---
**Proveedores y agentes** | Actualización de los proveedores y agentes de Site Recovery (como se detalla en el paquete acumulativo de actualizaciones).
**Mejoras y correcciones de problemas** | Un número de correcciones y mejoras (tal como se detalla en el paquete acumulativo).

### <a name="vmware-vmsphysical-servers-replication"></a>Replicación de servidores físicos/VM de VMware 
Las características que se agregaron este mes se resumen en la tabla.
**Característica** | **Detalles**
--- | ---
**Soporte técnico de Linux** | Se agregó compatibilidad para Oracle Linux 6.8 y 6.9/7.0 y para el kernel de UEK5.
**LVM** | Se agregó compatibilidad para los volúmenes LVM y LVM2.<br/><br/> Ahora se admite el directorio /boot, en una partición de disco y en volúmenes LVM.
**Directorios** | Se agregó compatibilidad para estos directorios configurados como particiones independientes o sistemas de archivos que no están en el mismo disco del sistema:<br/><br/> /(root), /boot, /usr, /usr/local, /var, /etc.
**Windows Server 2008** | Se agregó compatibilidad para discos dinámicos.
**Conmutación por error** | Tiempo de conmutación por error mejorado para las VM de VMware donde storvsc y vsbus no son controladores de arranque.
**Compatibilidad con UEFI** | Las VM de Azure no admiten el tipo de arranque UEFI. Ahora puede migrar servidores físicos locales con UEFI a Azure con Site Recovery. Site Recovery migra el servidor mediante la conversión del tipo de arranque a BIOS antes de la migración. Antes, Site Recovery solo admitía esta conversión para VM. La compatibilidad está disponible para los servidores físicos que ejecutan Windows Server 2012 o posterior.

### <a name="azure-vm-disaster-recovery"></a>Recuperación ante desastres en VM de Azure
Las características que se agregaron este mes se resumen en la tabla.

**Característica** | **Detalles**
--- | ---
**Soporte técnico de Linux** | Se agregó compatibilidad para Oracle Linux 6.8 y 6.9/7.0 y para el kernel de UEK5.
**Sistema de archivos BRTFS de Linux** | Compatibilidad con las máquinas virtuales de Azure.
**Máquinas virtuales de Azure en zonas de disponibilidad** | Puede habilitar la replicación en otra región para VM de Azure implementadas en zonas de disponibilidad. Ahora puede habilitar la replicación de una máquina virtual de Azure y establecer el destino de una conmutación por error en una instancia de máquina virtual única, una máquina virtual en un conjunto de disponibilidad o una máquina virtual en una zona de disponibilidad. La configuración no afecta la replicación. [Lea](https://azure.microsoft.com/blog/disaster-recovery-of-zone-pinned-azure-virtual-machines-to-another-region/) el anuncio.
**Almacenamiento habilitado para firewall (portal/PowerShell)** | Se agregó compatibilidad para [cuentas de almacenamiento habilitadas para firewall](https://docs.microsoft.com/azure/storage/common/storage-network-security).<br/><br/> Puede replicar VM de Azure con discos no administrados en cuentas de almacenamiento habilitadas mediante firewall en otra región de Azure para recuperación ante desastres.<br/><br/> También puede usar cuentas de almacenamiento habilitadas mediante firewall como cuentas de almacenamiento de destino para discos no administrados.<br/><br/> Compatible con el portal y usando PowerShell.

## <a name="updates-december-2018"></a>Actualizaciones (diciembre de 2018)

### <a name="automatic-updates-for-the-mobility-service-azure-vms"></a>Actualizaciones automáticas de Mobility Service (VM de Azure)

Site Recovery agregó una opción para actualizaciones automáticas de la extensión de Mobility Service. La extensión de Mobility Service se instala en cada máquina virtual de Azure replicada por Site Recovery. Al habilitar la replicación, puede seleccionar si permitir que Site Recovery administre las actualizaciones de la extensión.

Las actualizaciones no requieren reiniciar la máquina virtual ni tampoco afectan la replicación. [Más información](azure-to-azure-autoupdate.md).

### <a name="pricing-calculator-for-azure-vm-disaster-recovery"></a>Calculadora de precios para la recuperación ante desastres de máquinas virtuales de Azure

La recuperación ante desastres de VM de Azure incurre en costos de licencias de VM y en costos de red y almacenamiento. Azure brinda una [calculadora de precios](https://aka.ms/a2a-cost-estimator) para ayudarlo a establecer estos costos. Site Recovery proporciona ahora una [estimación de precios de ejemplo](https://aka.ms/a2a-cost-estimator) que valora una implementación de ejemplo en función de una aplicación de tres nivel que usa seis máquinas virtuales con 12 discos HDD estándar y 6 discos SSD Premium.

- En el ejemplo se supone un cambio de datos de 10 GB al día para un disco estándar y de 20 GB para Premium.
- En el caso de su implementación en particular, puede cambiar las variables para calcular los costos.
- Puede especificar el número de máquinas virtuales, el número y el tipo de discos administrados y la tasa de cambio de datos total esperada en todas las máquinas virtuales.
- Además, puede aplicar un factor de compresión para estimar los costos de ancho de banda.

[Lea](https://azure.microsoft.com/blog/know-exactly-how-much-it-will-cost-for-enabling-dr-to-your-azure-vm/) el anuncio.


## <a name="updates-october-2018"></a>Actualizaciones (octubre de 2018)

### <a name="update-rollup-30"></a>Paquete acumulativo de actualizaciones 30 

El [Paquete acumulativo de actualizaciones 30](https://support.microsoft.com/help/4468181/azure-site-recovery-update-rollup-30) proporciona las siguientes actualizaciones.

**Actualizar** | **Detalles**
--- | ---
**Proveedores y agentes** | Actualización de los proveedores y agentes de Site Recovery (como se detalla en el paquete acumulativo de actualizaciones).
**Mejoras y correcciones de problemas** | Un número de correcciones y mejoras (tal como se detalla en el paquete acumulativo).

### <a name="azure-vm-disaster-recovery"></a>Recuperación ante desastres en VM de Azure
Las características que se agregaron este mes se resumen en la tabla.

**Característica** | **Detalles**
--- | ---
**Regiones admitidas** | Compatibilidad de Site Recovery agregada para Centro de Australia 1 y Centro de Australia 2.
**Compatibilidad con cifrado del disco** | Se ha agregado compatibilidad para recuperación ante desastres de VM de Azure cifradas con Azure Disk Encryption (ADE) con la aplicación de Azure AD. [Más información](azure-to-azure-how-to-enable-replication-ade-vms.md).
**Exclusión de disco** | Ahora, los discos no inicializados se excluyen automáticamente durante la replicación de VM de Azure.
**Almacenamiento habilitado para firewall (PowerShell)** | Se agregó compatibilidad para [cuentas de almacenamiento habilitadas para firewall](https://docs.microsoft.com/azure/storage/common/storage-network-security).<br/><br/> Puede replicar VM de Azure con discos no administrados en cuentas de almacenamiento habilitadas mediante firewall en otra región de Azure para recuperación ante desastres.<br/><br/> También puede usar cuentas de almacenamiento habilitadas mediante firewall como cuentas de almacenamiento de destino para discos no administrados.<br/><br/> Compatible solo con PowerShell.


### <a name="update-rollup-29"></a>Paquete acumulativo de actualizaciones 29 

El [Paquete acumulativo de actualizaciones 29](https://support.microsoft.com/help/4466466/update-rollup-29-for-azure-site-recovery) proporciona las siguientes actualizaciones.

**Actualizar** | **Detalles**
--- | ---
**Proveedores y agentes** | Actualización de los proveedores y agentes de Site Recovery (como se detalla en el paquete acumulativo de actualizaciones).
**Mejoras y correcciones de problemas** | Un número de correcciones y mejoras (tal como se detalla en el paquete acumulativo).


## <a name="updates-august-2018"></a>Actualizaciones (agosto de 2018)

### <a name="update-rollup-28"></a>Paquete acumulativo de actualizaciones 28 

El [Paquete acumulativo de actualizaciones 28](https://support.microsoft.com/help/4460079/update-rollup-28-for-azure-site-recovery) proporciona las siguientes actualizaciones.

**Actualizar** | **Detalles**
--- | ---
**Proveedores y agentes** | Actualización de los proveedores y agentes de Site Recovery (como se detalla en el paquete acumulativo de actualizaciones).
**Mejoras y correcciones de problemas** | Un número de correcciones y mejoras (tal como se detalla en el paquete acumulativo).

### <a name="azure-vm-disaster-recovery"></a>Recuperación ante desastres en VM de Azure 
Las características que se agregaron este mes se resumen en la tabla.

**Característica** | **Detalles**
--- | ---
**Soporte técnico de Linux** | Se agregó compatibilidad para RedHat Enterprise Linux 6.10; CentOS 6.10.<br/><br/>
**Soporte técnico en la nube** | Compatibilidad con recuperación ante desastres para VM de Azure en la nube de Alemania.
**Recuperación ante desastres entre suscripciones** | Se admite la replicación de VM de Azure de una región a otra de una suscripción diferente dentro del mismo inquilino de Azure Active Directory. [Más información](https://aka.ms/cross-sub-blog).

### <a name="vmware-vmphysical-server-disaster-recovery"></a>Recuperación ante desastres de un servidor físico/VM de VMware 
Las características que se agregaron este mes se resumen en la tabla.

**Característica** | **Detalles**
--- | ---
**Soporte técnico de Linux** | Se agregó compatibilidad para RedHat Enterprise Linux 6.10; CentOS 6.10.<br/><br/> Ahora se admiten las máquinas virtuales Linux que usan el estilo de partición de tabla de particiones de GUID (GPT) en el modo de compatibilidad BIOS heredado. Revise las [preguntas más frecuentes de VM de Azure](https://docs.microsoft.com/azure/virtual-machines/linux/faq-for-disks) para obtener más información. 
**Recuperación ante desastres para VM después de la migración** | Compatibilidad para habilitar la recuperación ante desastres en una región secundaria para una VM de VMware local migrada a Azure, sin necesidad de desinstalar el servicio Mobility en la VM antes de habilitar la replicación.
**Windows Server 2008** | Compatibilidad para migrar máquinas que ejecutan Windows Server 2008 R2/2008 de 64 bits y 32 bits.<br/><br/> Solo migración (conmutación por error y replicación). No se admite la conmutación por recuperación.

## <a name="updates-july-2018"></a>Actualizaciones (julio de 2018)

### <a name="update-rollup-27-july-2018"></a>Paquete acumulativo de actualizaciones 27 (julio de 2018)

El [Paquete acumulativo de actualizaciones 27](https://support.microsoft.com/help/4055712/update-rollup-27-for-azure-site-recovery) proporciona las siguientes actualizaciones.

**Actualizar** | **Detalles**
--- | ---
**Proveedores y agentes** | Actualización de los proveedores y agentes de Site Recovery (como se detalla en el paquete acumulativo de actualizaciones).
**Mejoras y correcciones de problemas** | Un número de correcciones y mejoras (tal como se detalla en el paquete acumulativo).

### <a name="azure-vm-disaster-recovery"></a>Recuperación ante desastres en VM de Azure 

Las características que se agregaron este mes se resumen en la tabla.

**Característica** | **Detalles**
--- | ---
**Soporte técnico de Linux** | Compatibilidad agregada para Red Hat Enterprise Linux 7.5.

### <a name="vmware-vmphysical-server-disaster-recovery"></a>Recuperación ante desastres de un servidor físico/VM de VMware 

Las características que se agregaron este mes se resumen en la tabla.

**Característica** | **Detalles**
--- | ---
**Soporte técnico de Linux** | Se ha agregado compatibilidad para Red Hat Enterprise Linux 7.5, SUSE Linux Enterprise Server 12.



## <a name="next-steps"></a>Pasos siguientes

Manténgase al día con las actualizaciones en la página [Actualizaciones de Azure](https://azure.microsoft.com/updates/?product=site-recovery).
