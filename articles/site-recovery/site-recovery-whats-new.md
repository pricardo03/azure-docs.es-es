---
title: Novedades de Azure Site Recovery | Microsoft Docs
description: Proporciona un resumen de las características nuevas presentadas en Azure Site Recovery
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: conceptual
ms.date: 03/12/2019
ms.author: raynew
ms.openlocfilehash: 61bcc0565d57f9c64c453f79f319fc56d5a6de18
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/30/2019
ms.locfileid: "64925060"
---
# <a name="whats-new-in-site-recovery"></a>Novedades de Site Recovery

El servicio [Azure Site Recovery](site-recovery-overview.md) se actualiza y mejora de manera continua. Para ayudarlo a mantenerse actualizado, en este artículo encontrará información sobre las versiones más recientes, las características nuevas y el nuevo contenido. Esta página se actualiza de manera periódica.

Si tiene sugerencias sobre las características de Site Recovery, nos encantaría [conocer sus comentarios](https://feedback.azure.com/forums/256299-site-recovery).

## <a name="q1-2019"></a>T1 2019 

### <a name="update-rollup-34-february-2019"></a>Paquete acumulativo de actualizaciones 34 (febrero de 2019)

[Paquete acumulativo de actualizaciones 34](https://support.microsoft.com/help/4490016/update-rollup-34-for-azure-site-recovery) proporciona las siguientes actualizaciones.

**Actualizar** | **Detalles**
--- | ---
**Proveedores y agentes** | Una actualización de agentes de recuperación de sitio y proveedores (como se detalla en el paquete acumulativo de actualizaciones)
**Correcciones del problema** | Un número de correcciones y mejoras (tal y como se detalla en el paquete acumulativo de actualizaciones)



### <a name="update-rollup-33-february-2019"></a>Paquete acumulativo de actualizaciones (febrero de 2019) 33

[Paquete acumulativo de actualizaciones 33](https://support.microsoft.com/help/4489582/update-rollup-33-for-azure-site-recovery) proporciona las siguientes actualizaciones.

**Actualizar** | **Detalles**
--- | ---
**Proveedores y agentes** | Una actualización de agentes de recuperación de sitio y proveedores (como se detalla en el paquete acumulativo de actualizaciones)
**Correcciones del problema** | Un número de correcciones y mejoras (tal y como se detalla en el paquete acumulativo de actualizaciones)
**Asignación de red** | Recuperación ante desastres de máquinas virtuales de Azure, ahora puede usar cualquier red de destino disponibles cuando se habilita la replicación. 
**SSD estándar** | Ahora puede configurar la recuperación ante desastres para las máquinas virtuales de Azure con [discos SSD estándar](https://docs.microsoft.com/azure/virtual-machines/windows/disks-standard-ssd).
**Espacios de almacenamiento directo** | Puede configurar la recuperación ante desastres para aplicaciones que se ejecutan en aplicaciones de la máquina virtual de Azure mediante el uso de [espacios de almacenamiento directo](https://docs.microsoft.com/windows-server/storage/storage-spaces/storage-spaces-direct-overview) para lograr alta disponibilidad.
**Sistema de archivos BRTFS** | Se admite para máquinas virtuales de VMware, además de máquinas virtuales de Azure.<br/><br/> No se admite si: El volumen en subcarpetas del sistema de archivos BTRFS se cambia después de habilitar la replicación, el sistema de archivos esté repartido entre varios discos, o si el sistema de archivos de la BTRFS es compatible con RAID.



### <a name="update-rollup-32-january-2019"></a>Paquete acumulativo de actualizaciones 32 (enero de 2019)

[Paquete acumulativo de actualizaciones 31](https://support.microsoft.com/help/4485985/update-rollup-32-for-azure-site-recovery) proporciona las siguientes actualizaciones.

**Actualizar** | **Detalles**
--- | ---
**Proveedores y agentes** | Una actualización de agentes de recuperación de sitio y proveedores (como se detalla en el paquete acumulativo de actualizaciones)
**Correcciones del problema** | Un número de correcciones y mejoras (tal y como se detalla en el paquete acumulativo de actualizaciones)
**Recuperación ante desastres para Linux** | **Máquinas virtuales de Azure**: Estación de trabajo de Red Hat 6 o 7; compatibilidad con las nuevas versiones de kernel de Ubuntu, Debian y SUSE.<br/><br/> **Servidores físicos o máquinas virtuales de VMware**: Red Hat Enterprise Linux 7.6; Estación de trabajo de Red Hat 6 o 7; Oracle Linux 6.10/7.6; admite todas las nuevas versiones de kernel de Ubuntu, Debian y SUSE.


### <a name="update-rollup-31-january-2019"></a>Paquete acumulativo de actualizaciones 31 (enero de 2019)

[Paquete acumulativo de actualizaciones 31](https://support.microsoft.com/help/4478871/update-rollup-31-for-azure-site-recovery) proporciona las siguientes actualizaciones.

**Actualizar** | **Detalles**
--- | ---
**Proveedores y agentes** | Una actualización de agentes de recuperación de sitio y proveedores (como se detalla en el paquete acumulativo de actualizaciones)
**Correcciones del problema** | Un número de correcciones y mejoras (tal y como se detalla en el paquete acumulativo de actualizaciones)
**Recuperación ante desastres para Linux** | **Máquinas virtuales de Azure**: Oracle Linux 6.8 y 7.0/6.9; compatibilidad con los kernels UEK5.<br/><br/> **Servidores físicos o máquinas virtuales de VMware**: Oracle Linux 6.8 y 7.0/6.9; compatibilidad con UEK5 kernel.
**Sistema de archivos BRTFS** | Se admite para máquinas virtuales de Azure.
**LVM** | Ha agregado compatibilidad para volúmenes LVM y LVM2.<br/><br/> Se admite el directorio/Boot, en una partición de disco y en volúmenes LVM.
**Directorios** | Ha agregado compatibilidad para estos vea directorios como particiones independientes o sistemas de archivos que no están en el mismo disco del sistema: / (root), / Boot, / usr, / usr/local, / var, /etc.
**Windows Server 2008** | Ha agregado compatibilidad para los discos dinámicos.
**Conmutación por error de VM de VMware** | Tiempo de conmutación por error mejorado para las máquinas virtuales de VMware donde storvsc y vsbus no son controladores de arranque.
**Compatibilidad con UEFI** | Máquinas virtuales de Azure no admiten el tipo de arranque UEFI. Ahora puede migrar los servidores físicos locales con UEFI en Azure con Site Recovery. Site Recovery migra el servidor mediante la conversión del tipo de arranque a BIOS antes de la migración. Site Recovery previamente solo admite esta conversión de máquinas virtuales. Soporte técnico está disponible para los servidores físicos que ejecutan Windows Server 2012 o posterior.
**Máquinas virtuales de Azure en las zonas de disponibilidad** | Puede habilitar la replicación en otra región de Azure Virtual Machines implementadas en las zonas de disponibilidad. unidad organizativa ahora puede habilitar la replicación de una máquina virtual de Azure y establecer el destino de conmutación por error en una sola instancia de máquina virtual, una máquina virtual en un conjunto de disponibilidad o una máquina virtual en una zona de disponibilidad. La configuración no afecta la replicación. [Lea](https://azure.microsoft.com/blog/disaster-recovery-of-zone-pinned-azure-virtual-machines-to-another-region/) el anuncio.


## <a name="q4-2018"></a>T4 2018

### <a name="update-rollup-30-october-2018"></a>Paquete acumulativo de actualizaciones 30 (octubre de 2018)

[Paquete acumulativo de 30](https://support.microsoft.com/help/4468181/azure-site-recovery-update-rollup-30) proporciona las siguientes actualizaciones.

**Actualizar** | **Detalles**
--- | ---
**Proveedores y agentes** | Una actualización de agentes de recuperación de sitio y proveedores (como se detalla en el paquete acumulativo de actualizaciones)
**Correcciones del problema** | Un número de correcciones y mejoras (tal y como se detalla en el paquete acumulativo de actualizaciones)
**Compatibilidad con regiones** | Compatibilidad de Site Recovery agregada para Australia Central 1 y Australia Central 2.
**Soporte técnico para el cifrado de disco** | Compatibilidad agregada para la recuperación ante desastres de máquinas virtuales de Azure cifradas con Azure Disk Encryption (ADE) con la aplicación de Azure AD. [Más información](azure-to-azure-how-to-enable-replication-ade-vms.md).
**Exclusión del disco** | Discos no inicializados ahora se excluyen automáticamente durante la replicación de máquinas virtuales de Azure.
**Almacenamiento Firewall habilitado** | Ha agregado compatibilidad para [cuentas de almacenamiento habilitado firewall](https://docs.microsoft.com/azure/storage/common/storage-network-security).<br/><br/> Puede replicar máquinas virtuales de Azure con discos no administrados en cuentas de almacenamiento firewall habilitado en otra región de Azure para recuperación ante desastres.<br/><br/> Puede usar las cuentas de almacenamiento firewall habilitado como cuentas de almacenamiento de destino para discos no administrados.<br/><br/> Admite solo mediante PowerShell.


### <a name="update-rollup-29-october-2018"></a>Paquete acumulativo de actualizaciones 29 (octubre de 2018)

[Paquete acumulativo de actualizaciones 29](https://support.microsoft.com/help/4466466/update-rollup-29-for-azure-site-recovery) proporciona las siguientes actualizaciones.

**Actualizar** | **Detalles**
--- | ---
**Proveedores y agentes** | Una actualización de agentes de recuperación de sitio y proveedores (como se detalla en el paquete acumulativo de actualizaciones)
**Correcciones del problema** | Un número de correcciones y mejoras (tal y como se detalla en el paquete acumulativo de actualizaciones)

### <a name="automatic-updates-for-the-mobility-service-extension"></a>Actualizaciones automáticas de la extensión de Mobility Service

Site Recovery agregó una opción para actualizaciones automáticas de la extensión de Mobility Service. La extensión de Mobility Service se instala en cada máquina virtual de Azure replicada por Site Recovery. Al habilitar la replicación, puede seleccionar si permitir que Site Recovery administre las actualizaciones de la extensión. Las actualizaciones no requieren reiniciar la máquina virtual ni tampoco afectan la replicación. [Más información](azure-to-azure-autoupdate.md).

### <a name="disaster-recovery-for-vms-using-accelerated-networking"></a>Recuperación ante desastres para máquinas virtuales con redes aceleradas

Las redes aceleradas habilitan la virtualización de E/S de raíz única (SR-IOV) en una máquina virtual, lo que mejora su rendimiento en la red. Al habilitar la replicación de una máquina virtual de Azure, Site Recovery detecta si las redes aceleradas están habilitadas. Si es así, después de la conmutación por error, Site Recovery configura automáticamente las redes aceleradas en la máquina virtual de Azure de réplica de destino, tanto para [Windows](https://docs.microsoft.com/azure/virtual-network/create-vm-accelerated-networking-powershell#enable-accelerated-networking-on-existing-vms) como para [Linux](https://docs.microsoft.com/azure/virtual-network/create-vm-accelerated-networking-cli#enable-accelerated-networking-on-existing-vms). [Más información](azure-vm-disaster-recovery-with-accelerated-networking.md).

### <a name="pricing-calculator-for-azure-vm-disaster-recovery"></a>Calculadora de precios para la recuperación ante desastres de máquinas virtuales de Azure

La recuperación ante desastres de máquinas virtuales de Azure incurre en costos de licencias de VM y en costos de red y almacenamiento. Azure brinda una [calculadora de precios](https://aka.ms/a2a-cost-estimator) para ayudarlo a establecer estos costos. Site Recovery proporciona ahora una [estimación de precios de ejemplo](https://aka.ms/a2a-cost-estimator) que valora una implementación de ejemplo en función de una aplicación de tres nivel que usa seis máquinas virtuales con 12 discos HDD estándar y 6 discos SSD Premium. En el ejemplo se supone un cambio de datos de 10 GB al día para un disco estándar y de 20 GB para Premium. En el caso de su implementación en particular, puede cambiar las variables para calcular los costos. Puede especificar el número de máquinas virtuales, el número y el tipo de discos administrados y la tasa de cambio de datos total esperada en todas las máquinas virtuales. Además, puede aplicar un factor de compresión para estimar los costos de ancho de banda. [Lea](https://azure.microsoft.com/blog/know-exactly-how-much-it-will-cost-for-enabling-dr-to-your-azure-vm/) el anuncio.



## <a name="q3-2018"></a>T3 2018 


### <a name="update-rollup-28-august-2018"></a>Paquete acumulativo de actualizaciones 28 (agosto de 2018)

[Paquete acumulativo de actualizaciones 28](https://support.microsoft.com/help/4460079/update-rollup-28-for-azure-site-recovery) proporciona las siguientes actualizaciones.

**Actualizar** | **Detalles**
--- | ---
**Proveedores y agentes** | Una actualización de agentes de recuperación de sitio y proveedores (como se detalla en el paquete acumulativo de actualizaciones)
**Recuperación ante desastres para Linux** | **Máquinas virtuales de Azure**: Agregado soporte para Red Hat Enterprise Linux 6.10; CentOS 6.10.<br/><br/> **Máquinas virtuales de VMware**: RedHat Enterprise Linux 6.10; CentOS 6.10.<br/><br/> Basado en Linux, las máquinas virtuales que use el estilo de particiones GUID (GPT) de la tabla de partición en modo de compatibilidad BIOS heredado ahora son compatibles. Consulte [preguntas más frecuentes acerca de los discos de máquina virtual de IaaS de Azure](https://docs.microsoft.com/azure/virtual-machines/linux/faq-for-disks) para obtener más información. 
**Soporte técnico en la nube** | Admite la recuperación ante desastres para máquinas virtuales de Azure en la nube de Alemania.
**Recuperación ante desastres entre suscripciones** | Soporte técnico para la replicación de máquinas virtuales de Azure en una región a otra región de una suscripción diferente, en el mismo inquilino de Azure Active Directory. [Más información](https://aka.ms/cross-sub-blog).
**Windows Server 2008** | Compatibilidad para migrar máquinas de ejecución de Windows Server 2008 R2/2008 de 64 bits y 32 bits.<br/><br/> Solo en la migración (conmutación por error y replicación). No se admite la conmutación por recuperación.

### <a name="update-rollup-27-july-2018"></a>Paquete acumulativo de actualizaciones 27 (julio de 2018)

[Paquete acumulativo de actualizaciones 27](https://support.microsoft.com/help/4055712/update-rollup-27-for-azure-site-recovery) proporciona las siguientes actualizaciones.

**Actualizar** | **Detalles**
--- | ---
**Proveedores y agentes** | Una actualización de agentes de recuperación de sitio y proveedores (como se detalla en el paquete acumulativo de actualizaciones)
**Recuperación ante desastres para Linux** | **Máquinas virtuales de Azure**: Red Hat Enterprise Linux 7.5<br/><br/> **Servidores físicos o máquinas virtuales de VMware**: Red Hat Enterprise Linux 7.5, SUSE Linux Enterprise Server 12




## <a name="next-steps"></a>Pasos siguientes

Manténgase al día con las actualizaciones en la página [Actualizaciones de Azure](https://azure.microsoft.com/updates/?product=site-recovery).




 









