---
title: Novedades de Azure Site Recovery | Microsoft Docs
description: Proporciona un resumen de las características nuevas presentadas en Azure Site Recovery
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: conceptual
ms.date: 01/28/2019
ms.author: raynew
ms.openlocfilehash: 61e66a19b625141c69a9473373d3d5d808e18fde
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2019
ms.locfileid: "55211124"
---
# <a name="whats-new-in-site-recovery"></a>Novedades de Site Recovery

El servicio [Azure Site Recovery](site-recovery-overview.md) se actualiza y mejora de manera continua. Para ayudarlo a mantenerse actualizado, en este artículo encontrará información sobre las versiones más recientes, las características nuevas y el nuevo contenido. Esta página se actualiza de manera periódica.

Si tiene sugerencias sobre las características de Site Recovery, nos encantaría [conocer sus comentarios](https://feedback.azure.com/forums/256299-site-recovery).

## <a name="q1-2019"></a>T1 2019

### <a name="linux-support"></a>Linux Support

El [paquete acumulativo de actualizaciones 32](https://support.microsoft.com/help/4485985/update-rollup-32-for-azure-site-recovery) brinda una actualización a los agentes y proveedores de Site Recovery. La actualización agrega compatibilidad con Linux de la siguiente manera:

- **Recuperación ante desastres de máquinas virtuales de Azure**: RedHat Workstation 6/7; versiones nuevas de kernel para Ubuntu, Debian y SUSE.
- **Recuperación ante desastres de servidores físicos o máquinas virtuales de VMware en Azure**: RedHat Enterprise Linux 7.6; RedHat Workstation 6/7; Oracle Linux 6.10/7.6. Versiones nuevas de kernel para Ubuntu, Debian y SUSE.



## <a name="q4-2018"></a>T4 2018

## <a name="pricing-calculator-for-azure-vm-disaster-recovery"></a>Calculadora de precios para la recuperación ante desastres de máquinas virtuales de Azure

La recuperación ante desastres de máquinas virtuales de Azure incurre en costos de licencias de VM y en costos de red y almacenamiento. Azure brinda una [calculadora de precios](https://aka.ms/a2a-cost-estimator) para ayudarlo a establecer estos costos. Site Recovery proporciona ahora una [estimación de precios de ejemplo](https://aka.ms/a2a-cost-estimator) que valora una implementación de ejemplo en función de una aplicación de tres nivel que usa seis máquinas virtuales con 12 discos HDD estándar y 6 discos SSD Premium. En el ejemplo se supone un cambio de datos de 10 GB al día para un disco estándar y de 20 GB para Premium. En el caso de su implementación en particular, puede cambiar las variables para calcular los costos. Puede especificar el número de máquinas virtuales, el número y el tipo de discos administrados y la tasa de cambio de datos total esperada en todas las máquinas virtuales. Además, puede aplicar un factor de compresión para estimar los costos de ancho de banda. [Lea](https://azure.microsoft.com/blog/know-exactly-how-much-it-will-cost-for-enabling-dr-to-your-azure-vm/) el anuncio.

### <a name="support-for-azure-vms-in-zones"></a>Compatibilidad con máquinas virtuales de Azure en zonas

Las zonas de disponibilidad de Azure son ubicaciones físicas exclusivas dentro de una región de Azure. Cada zona de disponibilidad consta de uno o varios centros de datos equipados con alimentación, refrigeración y redes independientes. Ahora puede habilitar la replicación de una máquina virtual de Azure y establecer el destino de una conmutación por error en una instancia de máquina virtual única, una máquina virtual en un conjunto de disponibilidad o una máquina virtual en una zona de disponibilidad. La configuración no afecta la replicación. [Lea](https://azure.microsoft.com/blog/disaster-recovery-of-zone-pinned-azure-virtual-machines-to-another-region/) el anuncio.
 
### <a name="disaster-recovery-for-encrypted-vms"></a>Recuperación ante desastres para máquinas virtuales cifradas

Site Recovery admite máquinas virtuales de Azure cifradas con Azure Disk Encryption (ADE) con la aplicación de Azure AD. [Más información](azure-to-azure-how-to-enable-replication-ade-vms.md).

### <a name="disaster-recovery-for-vms-using-accelerated-networking"></a>Recuperación ante desastres para máquinas virtuales con redes aceleradas

Las redes aceleradas habilitan la virtualización de E/S de raíz única (SR-IOV) en una máquina virtual, lo que mejora su rendimiento en la red. Al habilitar la replicación de una máquina virtual de Azure, Site Recovery detecta si las redes aceleradas están habilitadas. Si es así, después de la conmutación por error, Site Recovery configura automáticamente las redes aceleradas en la máquina virtual de Azure de réplica de destino, tanto para [Windows](https://docs.microsoft.com/azure/virtual-network/create-vm-accelerated-networking-powershell#enable-accelerated-networking-on-existing-vms) como para [Linux](https://docs.microsoft.com/azure/virtual-network/create-vm-accelerated-networking-cli#enable-accelerated-networking-on-existing-vms). [Más información](azure-vm-disaster-recovery-with-accelerated-networking.md).

### <a name="automatic-updates-for-the-mobility-service-extension"></a>Actualizaciones automáticas de la extensión de Mobility Service

Site Recovery agregó una opción para actualizaciones automáticas de la extensión de Mobility Service. La extensión de Mobility Service se instala en cada máquina virtual de Azure replicada por Site Recovery. Al habilitar la replicación, puede seleccionar si permitir que Site Recovery administre las actualizaciones de la extensión. Las actualizaciones no requieren reiniciar la máquina virtual ni tampoco afectan la replicación. [Más información](azure-to-azure-autoupdate.md).

### <a name="support-for-standard-ssd-disks"></a>Compatibilidad con discos SSD estándar

Azure presentó los discos [Unidades de estado sólido (SSD) estándar](https://docs.microsoft.com/azure/virtual-machines/windows/disks-standard-ssd) para ofrecer una solución de almacenamiento rentable para aplicaciones como servidores web que necesitan un rendimiento constante, pero no tienen niveles altos de IOPS de disco. Estos combinan elementos de discos SSD Premium y HDD estándar. Site Recovery proporciona recuperación ante desastres para máquinas virtuales de Azure con disco SSD estándar. De manera predeterminada, el tipo de disco se conserva después de la conmutación por error en la región de destino.

### <a name="support-for-azure-storage-firewall"></a>Compatibilidad con el firewall de Azure Storage

Puede proteger las cuentas de almacenamiento de Azure en un conjunto específico de redes si activa reglas de firewall para la cuenta. Puede configurar las cuentas de almacenamiento para denegar de manera predeterminada el tráfico de las redes internas y de Internet y luego conceder acceso al tráfico desde redes virtuales específicas. Site Recovery admite la replicación de máquinas virtuales con discos no administrados en cuentas de almacenamiento compatibles con firewall en una región secundaria. En la región de destino, para los discos administrados, puede seleccionar cuentas de almacenamiento con firewalls habilitados. También puede restringir el acceso a la cuenta de almacenamiento en caché si restringe el acceso de red solo a la red en que se encuentran las máquinas virtuales de origen. Tenga en cuenta que debe [permitir el acceso](https://docs.microsoft.com/azure/storage/common/storage-network-security#exceptions) para los servicios de Microsoft de confianza.

## <a name="q3-2018"></a>T3 2018 

### <a name="linux-support"></a>Linux Support

#### <a name="update-rollup-28"></a>Paquete acumulativo de actualizaciones 28

El [paquete acumulativo de actualizaciones 28](https://support.microsoft.com/help/4460079/update-rollup-28-for-azure-site-recovery) brinda una actualización a los agentes y proveedores de Site Recovery. La actualización agrega compatibilidad con Linux de la siguiente manera:

- **Recuperación ante desastres de máquinas virtuales de Azure**: RedHat Enterprise Linux 6.10; CentOS 6.10; ahora se admiten las máquinas virtuales Linux que usan el estilo de partición de tabla de particiones de GUID (GPT) en el modo de compatibilidad BIOS heredado.
- **Recuperación ante desastres de servidores físicos o máquinas virtuales de VMware en Azure**: RedHat Enterprise Linux 6.10; CentOS 6.10; ahora se admiten las máquinas virtuales Linux que usan el estilo de partición de tabla de particiones de GUID (GPT) en el modo de compatibilidad BIOS heredado.

#### <a name="update-rollup-27"></a>Paquete acumulativo de actualizaciones 27

El [paquete acumulativo de actualizaciones 28](https://support.microsoft.com/help/4460079/update-rollup-28-for-azure-site-recovery) brinda una actualización a los agentes y proveedores de Site Recovery. La actualización agrega compatibilidad con Linux de la siguiente manera:

- **Recuperación ante desastres de máquinas virtuales de Azure**: Red Hat Enterprise Linux 7.5
- **Recuperación ante desastres de servidores físicos o máquinas virtuales de VMware en Azure**: SUSE Linux Enterprise Server 12, Red Hat Enterprise Linux 7.5

### <a name="support-for-azure-vms-running-on-windows-server-2016"></a>Compatibilidad con máquinas virtuales de Azure que se ejecutan en Windows Server 2016

Las máquinas virtuales de Azure que se ejecutan en Windows Server 2016 se puede replicar en todas las regiones de Azure con Azure Site Recovery.

### <a name="support-for-azure-vms-running-storage-spaces-direct"></a>Compatibilidad con máquinas virtuales de Azure que ejecutan Espacios de almacenamiento directo

[Espacios de almacenamiento directo](https://docs.microsoft.com/windows-server/storage/storage-spaces/storage-spaces-direct-overview) (disponible desde Windows Server 2016 en adelante) agrupa unidades en un grupo de almacenamiento y, luego, usa la capacidad del grupo para crear espacios de almacenamiento. Los espacios de almacenamiento se pueden usar en una máquina virtual independiente o en un [clúster invitado de máquinas virtuales de Azure](https://docs.microsoft.com/windows-server/storage/storage-spaces/storage-spaces-direct-in-vm) con almacenamiento local en cada nodo de clúster, o bien almacenamiento compartido en todo el clúster.

## <a name="next-steps"></a>Pasos siguientes

Manténgase al día con las actualizaciones en la página [Actualizaciones de Azure](https://azure.microsoft.com/updates/?product=site-recovery).


