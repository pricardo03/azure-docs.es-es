---
title: Novedades de Azure Site Recovery | Microsoft Docs
description: Proporciona un resumen de las características nuevas presentadas en Azure Site Recovery
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: conceptual
ms.date: 04/30/2019
ms.author: raynew
ms.openlocfilehash: e2145fbbb5fa09aa3321742ca8a786822f6f0641
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65148665"
---
# <a name="whats-new-in-site-recovery"></a>Novedades de Site Recovery

El servicio [Azure Site Recovery](site-recovery-overview.md) se actualiza y mejora de manera continua. Para ayudarlo a mantenerse actualizado, en este artículo encontrará información sobre las versiones más recientes, las características nuevas y el nuevo contenido. Esta página se actualiza de manera periódica.

Si tiene sugerencias sobre las características de Site Recovery, nos encantaría [conocer sus comentarios](https://feedback.azure.com/forums/256299-site-recovery).


## <a name="updates-march-2019"></a>Actualizaciones (marzo de 2019)

### <a name="update-rollup-35"></a>Paquete acumulativo de actualizaciones 35

[Paquete acumulativo de actualizaciones 35](https://support.microsoft.com/en-us/help/4494485/update-rollup-35-for-azure-site-recovery) proporciona las siguientes actualizaciones.

**Actualizar** | **Detalles**
--- | ---
**Proveedores y agentes** | Una actualización de agentes de recuperación de sitio y proveedores (como se detalla en el paquete acumulativo de actualizaciones)
**Problema correcciones y mejoras** | Un número de correcciones y mejoras (tal y como se detalla en el paquete acumulativo de actualizaciones)

#### <a name="vmwarephysical-server-disaster-recovery"></a>Recuperación ante desastres de VMware/servidor físico
Nuevas características agregadas en la actualización.

**Característica** | **Detalles**
--- | ---
**Discos administrados** | Replicación de máquinas virtuales de VMware locales y servidores físicos está ahora directamente a discos administrados en Azure. En el entorno local se envían datos a una cuenta de almacenamiento de caché de Azure y puntos de recuperación se crean en discos administrados en la ubicación de destino. Esto garantiza que no es necesario administrar varias cuentas de almacenamiento de destino.
**Servidor de configuración** | Site Recovery ahora admite servidores de una configuración con varias NIC. Debe agregar adaptadores adicionales a la máquina virtual del servidor de configuración antes de registrar el servidor de configuración en el almacén. Si agrega posteriormente, deberá volver a registrar el servidor en el almacén.


## <a name="updates-february-2019"></a>Actualizaciones (febrero de 2019)

### <a name="update-rollup-34"></a>Paquete acumulativo de actualizaciones 34 

[Paquete acumulativo de actualizaciones 34](https://support.microsoft.com/help/4490016/update-rollup-34-for-azure-site-recovery) proporciona las siguientes actualizaciones.

**Actualizar** | **Detalles**
--- | ---
**Proveedores y agentes** | Una actualización de agentes de recuperación de sitio y proveedores (como se detalla en el paquete acumulativo de actualizaciones).
**Problema correcciones y mejoras** | Un número de correcciones y mejoras (tal y como se detalla en el paquete acumulativo de actualizaciones).


### <a name="update-rollup-33"></a>Paquete acumulativo de actualizaciones 33 

[Paquete acumulativo de actualizaciones 33](https://support.microsoft.com/help/4489582/update-rollup-33-for-azure-site-recovery) proporciona las siguientes actualizaciones.

**Actualizar** | **Detalles**
--- | ---
**Proveedores y agentes** | Una actualización de agentes de recuperación de sitio y proveedores (como se detalla en el paquete acumulativo de actualizaciones).
**Problema correcciones y mejoras** | Un número de correcciones y mejoras (tal y como se detalla en el paquete acumulativo de actualizaciones).


#### <a name="azure-vm-disaster-recovery"></a>Recuperación ante desastres de máquinas virtuales Azure 
Nuevas características agregadas en la actualización.

**Característica** | **Detalles**
--- | ---
**Asignación de red** | Recuperación ante desastres de máquinas virtuales de Azure, ahora puede usar cualquier red de destino disponibles cuando se habilita la replicación. 
**SSD estándar** | Ahora puede configurar la recuperación ante desastres para las máquinas virtuales de Azure con [discos SSD estándar](https://docs.microsoft.com/azure/virtual-machines/windows/disks-standard-ssd).
**Espacios de almacenamiento directo** | Puede configurar la recuperación ante desastres para aplicaciones que se ejecutan en aplicaciones de la máquina virtual de Azure mediante el uso de [espacios de almacenamiento directo](https://docs.microsoft.com/windows-server/storage/storage-spaces/storage-spaces-direct-overview) para lograr alta disponibilidad.  Usar los espacios de almacenamiento directo (S2D) junto con Site Recovery proporciona una protección exhaustiva de las cargas de trabajo de máquina virtual de Azure. S2D le permite hospedar un clúster invitado de Azure. Esto es especialmente útil cuando una máquina virtual hospeda una aplicación crítica, como capa de ASCS de SAP, SQL Server o servidor de archivos de escalabilidad horizontal.


#### <a name="vmwarephysical-server-disaster-recovery"></a>Recuperación ante desastres de VMware/servidor físico
Nuevas características agregadas en la actualización.

**Característica** | **Detalles**
--- | ---
**Sistema de archivos de Linux BRTFS** | Site Recovery ahora admite la replicación de máquinas virtuales de VMware con el sistema de archivos BRTFS. Si no se admite la replicación:<br/><br/>-El volumen en subcarpetas del sistema de archivos BTRFS se cambia después de habilitar la replicación.<br/><br/>-El sistema de archivos esté repartido entre varios discos.<br/><br/>-El sistema de archivos BTRFS es compatible con RAID.
**Windows Server 2019** | Ha agregado compatibilidad para las máquinas que ejecutan Windows Server 2019.


## <a name="updates-january-2019"></a>Actualizaciones (enero de 2019)

### <a name="accelerated-networking-azure-vms"></a>Redes aceleradas (máquinas virtuales de Azure)

Las redes aceleradas habilitan la virtualización de E/S de raíz única (SR-IOV) en una máquina virtual, lo que mejora su rendimiento en la red. Al habilitar la replicación de una máquina virtual de Azure, Site Recovery detecta si las redes aceleradas están habilitadas. Si es así, después de la conmutación por error, Site Recovery configura automáticamente las redes aceleradas en la máquina virtual de Azure de réplica de destino, tanto para [Windows](https://docs.microsoft.com/azure/virtual-network/create-vm-accelerated-networking-powershell#enable-accelerated-networking-on-existing-vms) como para [Linux](https://docs.microsoft.com/azure/virtual-network/create-vm-accelerated-networking-cli#enable-accelerated-networking-on-existing-vms).

[Más información](azure-vm-disaster-recovery-with-accelerated-networking.md).

### <a name="update-rollup-32"></a>Paquete acumulativo de actualizaciones 32 

[Paquete acumulativo de actualizaciones 32](https://support.microsoft.com/help/4485985/update-rollup-32-for-azure-site-recovery) proporciona las siguientes actualizaciones.

**Actualizar** | **Detalles**
--- | ---
**Proveedores y agentes** | Una actualización de agentes de recuperación de sitio y proveedores (como se detalla en el paquete acumulativo de actualizaciones).
**Problema correcciones y mejoras** | Un número de correcciones y mejoras (tal y como se detalla en el paquete acumulativo de actualizaciones).

#### <a name="azure-vm-disaster-recovery"></a>Recuperación ante desastres de máquinas virtuales Azure

Nuevas características agregadas en la actualización.

**Característica** | **Detalles**
--- | ---
**Compatibilidad con Linux** | Se agregó compatibilidad para la estación de trabajo de Red Hat 6 y 7 y las nuevas versiones de kernel de Ubuntu, Debian y SUSE.
**Espacios de almacenamiento directo** | Site Recovery admite máquinas virtuales de Azure mediante espacios de almacenamiento directo (S2D).

#### <a name="vmware-vmsphysical-servers-replication"></a>Replicación de servidores físicos o máquinas virtuales de VMware 
**Característica** | **Detalles**
--- | ---
**Compatibilidad con Linux** | Se agregó compatibilidad para Red Hat Enterprise Linux 7.6, estación de trabajo de Red Hat 6 y 7, Oracle Linux 6.10/7.6 y nuevas versiones de kernel de Ubuntu, Debian y SUSE.


### <a name="update-rollup-31"></a>Paquete acumulativo de actualizaciones 31 

[Paquete acumulativo de actualizaciones 31](https://support.microsoft.com/help/4478871/update-rollup-31-for-azure-site-recovery) proporciona las siguientes actualizaciones.

**Actualizar** | **Detalles**
--- | ---
**Proveedores y agentes** | Una actualización de agentes de recuperación de sitio y proveedores (como se detalla en el paquete acumulativo de actualizaciones).
**Problema correcciones y mejoras** | Un número de correcciones y mejoras (tal y como se detalla en el paquete acumulativo de actualizaciones).

#### <a name="vmware-vmsphysical-servers-replication"></a>Replicación de servidores físicos o máquinas virtuales de VMware 
Nuevas características agregadas en la actualización.

**Característica** | **Detalles**
--- | ---
**Compatibilidad con Linux** | Se agregó compatibilidad para Oracle Linux 6.8 y 6.9 y 7.0 y para el kernel UEK5.
**LVM** | Ha agregado compatibilidad para volúmenes LVM y LVM2.<br/><br/> Ahora se admite el directorio/Boot, en una partición de disco y en volúmenes LVM.
**Directorios** | Se agregó compatibilidad para estos directorios configurado como particiones independientes o sistemas de archivos que no están en el mismo disco del sistema:<br/><br/> /(root), /boot, /usr, /usr/local, /var, /etc.
**Windows Server 2008** | Ha agregado compatibilidad para los discos dinámicos.
**Conmutación por error** | Tiempo de conmutación por error mejorado para las máquinas virtuales de VMware donde storvsc y vsbus no son controladores de arranque.
**Compatibilidad con UEFI** | Máquinas virtuales de Azure no admiten el tipo de arranque UEFI. Ahora puede migrar los servidores físicos locales con UEFI en Azure con Site Recovery. Site Recovery migra el servidor mediante la conversión del tipo de arranque a BIOS antes de la migración. Site Recovery previamente solo admite esta conversión de máquinas virtuales. Soporte técnico está disponible para los servidores físicos que ejecutan Windows Server 2012 o posterior.

#### <a name="azure-vm-disaster-recovery"></a>Recuperación ante desastres de máquinas virtuales Azure
Nuevas características agregadas en la actualización.

**Característica** | **Detalles**
--- | ---
**Compatibilidad con Linux** | Admite se agregó para Oracle Linux 6.8 y 7.0/6.9; y para el kernel UEK5.
**Sistema de archivos de Linux BRTFS** | Se admite para máquinas virtuales de Azure.
**Máquinas virtuales de Azure en las zonas de disponibilidad** | Puede habilitar la replicación en otra región de Azure Virtual Machines implementadas en las zonas de disponibilidad. Ahora puede habilitar la replicación de una máquina virtual de Azure y establecer el destino de una conmutación por error en una instancia de máquina virtual única, una máquina virtual en un conjunto de disponibilidad o una máquina virtual en una zona de disponibilidad. La configuración no afecta la replicación. [Lea](https://azure.microsoft.com/blog/disaster-recovery-of-zone-pinned-azure-virtual-machines-to-another-region/) el anuncio.
**Almacenamiento Firewall habilitado (portal/PowerShell)** | Ha agregado compatibilidad para [cuentas de almacenamiento habilitado firewall](https://docs.microsoft.com/azure/storage/common/storage-network-security).<br/><br/> Puede replicar máquinas virtuales de Azure con discos no administrados en cuentas de almacenamiento firewall habilitado en otra región de Azure para recuperación ante desastres.<br/><br/> Puede usar las cuentas de almacenamiento firewall habilitado como cuentas de almacenamiento de destino para discos no administrados.<br/><br/> Admitido en el portal y mediante PowerShell.

## <a name="updates-december-2018"></a>Actualizaciones (diciembre de 2018)

### <a name="automatic-updates-for-the-mobility-service-azure-vms"></a>Actualizaciones automáticas para el servicio de movilidad (máquinas virtuales de Azure)

Site Recovery agregó una opción para actualizaciones automáticas de la extensión de Mobility Service. La extensión de Mobility Service se instala en cada máquina virtual de Azure replicada por Site Recovery. Al habilitar la replicación, puede seleccionar si permitir que Site Recovery administre las actualizaciones de la extensión.

Las actualizaciones no requieren reiniciar la máquina virtual ni tampoco afectan la replicación. [Más información](azure-to-azure-autoupdate.md).

### <a name="pricing-calculator-for-azure-vm-disaster-recovery"></a>Calculadora de precios para la recuperación ante desastres de máquinas virtuales de Azure

Disaster Recovery de Azure Virtual Machines incurre en costos de licencia de la máquina virtual y los costos de red y almacenamiento. Azure brinda una [calculadora de precios](https://aka.ms/a2a-cost-estimator) para ayudarlo a establecer estos costos. Site Recovery proporciona ahora una [estimación de precios de ejemplo](https://aka.ms/a2a-cost-estimator) que valora una implementación de ejemplo en función de una aplicación de tres nivel que usa seis máquinas virtuales con 12 discos HDD estándar y 6 discos SSD Premium.

- El ejemplo se da por supuesto un cambio de datos de 10 GB días para standard y premium de 20 GB.
- En el caso de su implementación en particular, puede cambiar las variables para calcular los costos.
- Puede especificar el número de máquinas virtuales, el número y el tipo de discos administrados y la tasa de cambio de datos total esperada en todas las máquinas virtuales.
- Además, puede aplicar un factor de compresión para estimar los costos de ancho de banda.

[Lea](https://azure.microsoft.com/blog/know-exactly-how-much-it-will-cost-for-enabling-dr-to-your-azure-vm/) el anuncio.


## <a name="updates-october-2018"></a>Actualizaciones (octubre de 2018)

### <a name="update-rollup-30"></a>Paquete acumulativo de actualizaciones 30 

[Paquete acumulativo de 30](https://support.microsoft.com/help/4468181/azure-site-recovery-update-rollup-30) proporciona las siguientes actualizaciones.

**Actualizar** | **Detalles**
--- | ---
**Proveedores y agentes** | Una actualización de agentes de recuperación de sitio y proveedores (como se detalla en el paquete acumulativo de actualizaciones).
**Problema correcciones y mejoras** | Un número de correcciones y mejoras (tal y como se detalla en el paquete acumulativo de actualizaciones).

#### <a name="azure-vm-disaster-recovery"></a>Recuperación ante desastres de máquinas virtuales Azure
Nuevas características agregadas en la actualización.

**Característica** | **Detalles**
--- | ---
**Compatibilidad con regiones** | Compatibilidad de Site Recovery agregada para Australia Central 1 y Australia Central 2.
**Soporte técnico para el cifrado de disco** | Compatibilidad agregada para la recuperación ante desastres de máquinas virtuales de Azure cifradas con Azure Disk Encryption (ADE) con la aplicación de Azure AD. [Más información](azure-to-azure-how-to-enable-replication-ade-vms.md).
**Exclusión del disco** | Sin inicializar discos ahora se excluyen automáticamente durante la replicación de máquinas virtuales de Azure.
**Almacenamiento Firewall habilitado (PowerShell)** | Ha agregado compatibilidad para [cuentas de almacenamiento habilitado firewall](https://docs.microsoft.com/azure/storage/common/storage-network-security).<br/><br/> Puede replicar máquinas virtuales de Azure con discos no administrados en cuentas de almacenamiento firewall habilitado en otra región de Azure para recuperación ante desastres.<br/><br/> Puede usar las cuentas de almacenamiento firewall habilitado como cuentas de almacenamiento de destino para discos no administrados.<br/><br/> Admite solo mediante PowerShell.


### <a name="update-rollup-29"></a>Paquete acumulativo de actualizaciones 29 

[Paquete acumulativo de actualizaciones 29](https://support.microsoft.com/help/4466466/update-rollup-29-for-azure-site-recovery) proporciona las siguientes actualizaciones.

**Actualizar** | **Detalles**
--- | ---
**Proveedores y agentes** | Una actualización de agentes de recuperación de sitio y proveedores (como se detalla en el paquete acumulativo de actualizaciones).
**Problema correcciones y mejoras** | Un número de correcciones y mejoras (tal y como se detalla en el paquete acumulativo de actualizaciones).


## <a name="updates-august-2018"></a>Actualizaciones (agosto de 2018)

### <a name="update-rollup-28"></a>Paquete acumulativo de actualizaciones 28 

[Paquete acumulativo de actualizaciones 28](https://support.microsoft.com/help/4460079/update-rollup-28-for-azure-site-recovery) proporciona las siguientes actualizaciones.

**Actualizar** | **Detalles**
--- | ---
**Proveedores y agentes** | Una actualización de agentes de recuperación de sitio y proveedores (como se detalla en el paquete acumulativo de actualizaciones).
**Problema correcciones y mejoras** | Un número de correcciones y mejoras (tal y como se detalla en el paquete acumulativo de actualizaciones).

#### <a name="azure-vms-disaster-recovery"></a>Recuperación ante desastres de máquinas virtuales de Azure 
Nuevas características agregadas en la actualización.

**Característica** | **Detalles**
--- | ---
**Compatibilidad con Linux** | Agregado soporte para Red Hat Enterprise Linux 6.10; CentOS 6.10.<br/><br/>
**Soporte técnico en la nube** | Admite la recuperación ante desastres para máquinas virtuales de Azure en la nube de Alemania.
**Recuperación ante desastres entre suscripciones** | Soporte técnico para la replicación de máquinas virtuales de Azure en una región a otra región de una suscripción diferente, en el mismo inquilino de Azure Active Directory. [Más información](https://aka.ms/cross-sub-blog).

#### <a name="vmware-vmphysical-server-disaster-recovery"></a>Recuperación ante desastres de máquinas virtuales de VMware/servidor físico 
Nuevas características agregadas en la actualización.

**Característica** | **Detalles**
--- | ---
**Compatibilidad con Linux** | Ha agregado compatibilidad para Red Hat Enterprise Linux 6.10, CentOS 6.10.<br/><br/> Basado en Linux, las máquinas virtuales que use el estilo de particiones GUID (GPT) de la tabla de partición en modo de compatibilidad BIOS heredado ahora son compatibles. Revise el [preguntas más frecuentes de Azure VM](https://docs.microsoft.com/azure/virtual-machines/linux/faq-for-disks) para obtener más información. 
**Recuperación ante desastres para máquinas virtuales después de la migración** | Soporte técnico para habilitar la recuperación ante desastres en una región secundaria para una máquina virtual de VMware locales migradas a Azure, sin necesidad de desinstalar la movilidad de servicio en la máquina virtual antes de habilitar la replicación.
**Windows Server 2008** | Compatibilidad para migrar máquinas de ejecución de Windows Server 2008 R2/2008 de 64 bits y 32 bits.<br/><br/> Solo en la migración (conmutación por error y replicación). No se admite la conmutación por recuperación.

## <a name="updates-july-2018"></a>Actualizaciones (julio de 2018)

### <a name="update-rollup-27-july-2018"></a>Paquete acumulativo de actualizaciones 27 (julio de 2018)

[Paquete acumulativo de actualizaciones 27](https://support.microsoft.com/help/4055712/update-rollup-27-for-azure-site-recovery) proporciona las siguientes actualizaciones.

**Actualizar** | **Detalles**
--- | ---
**Proveedores y agentes** | Una actualización de agentes de recuperación de sitio y proveedores (como se detalla en el paquete acumulativo de actualizaciones).
**Problema correcciones y mejoras** | Un número de correcciones y mejoras (tal y como se detalla en el paquete acumulativo de actualizaciones).

#### <a name="azure-vms-disaster-recovery"></a>Recuperación ante desastres de máquinas virtuales de Azure 

Nuevas características agregadas en la actualización.

**Característica** | **Detalles**
--- | ---
**Compatibilidad con Linux** | Ha agregado compatibilidad para Red Hat Enterprise Linux 7.5.

#### <a name="vmware-vmphysical-server-disaster-recovery"></a>Recuperación ante desastres de máquinas virtuales de VMware/servidor físico 

Nuevas características agregadas en la actualización.

**Característica** | **Detalles**
--- | ---
**Compatibilidad con Linux** | Ha agregado compatibilidad para Red Hat Enterprise Linux 7.5, SUSE Linux Enterprise Server 12.



## <a name="next-steps"></a>Pasos siguientes

Manténgase al día con las actualizaciones en la página [Actualizaciones de Azure](https://azure.microsoft.com/updates/?product=site-recovery).
