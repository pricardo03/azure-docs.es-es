---
title: Matriz de compatibilidad de Azure Site Recovery para la recuperación ante desastres de máquinas virtuales de IaaS de Azure entre regiones de Azure con Azure Site Recovery | Microsoft Docs
description: Se resumen los sistemas operativos compatibles y las configuraciones para la replicación de Azure Site Recovery de máquinas virtuales (VM) de Azure de una región a otra en caso de que sea necesario realizar una recuperación ante desastres.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: article
ms.date: 01/21/2019
ms.author: raynew
ms.openlocfilehash: fa25c144737c4d070b2dbd08700c68e617950e0d
ms.sourcegitcommit: 9b6492fdcac18aa872ed771192a420d1d9551a33
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/22/2019
ms.locfileid: "54452265"
---
# <a name="support-matrix-for-replicating-from-one-azure-region-to-another"></a>Matriz de compatibilidad para replicar desde una región de Azure a otra

En este artículo se resumen las configuraciones y los componentes admitidos cuando implementa la recuperación ante desastres con replicación, conmutación por error y recuperación de máquinas virtuales de Azure de una región de Azure a otra mediante el servicio [Azure Site Recovery](site-recovery-overview.md).


## <a name="deployment-method-support"></a>Compatibilidad de método de implementación

**Método de implementación** |  **Se admite/no se admite**
--- | ---
**Azure Portal** | Compatible
**PowerShell** | [Replicación de Azure a Azure con PowerShell](azure-to-azure-powershell.md)
**API DE REST** | Compatible
**CLI** | No se admite actualmente.


## <a name="resource-support"></a>Compatibilidad de recursos

**Acción de recursos** | **Detalles**
--- | --- | ---
**Mover el almacén entre grupos de recursos** | No compatible
**Mover recursos de proceso, almacenamiento y red entre grupos de recursos** | No compatible.<br/><br/> Si mueve una máquina virtual o los componentes asociados, como el almacenamiento o la red, después de la replicación de esta, tendrá que deshabilitar la replicación y volver a habilitarla para la máquina virtual.
**Replicar máquinas virtuales de Azure de una suscripción a otra para la recuperación ante desastres** | Se admite en el mismo inquilino de Azure Active Directory.
**Migración de máquinas virtuales entre regiones dentro de los clústeres geográficos admitidos (dentro y entre suscripciones)** | Se admite en el mismo inquilino de Azure Active Directory.
**Migrar máquinas virtuales dentro de la misma región** | No compatible.

## <a name="region-support"></a>Regiones admitidas

Puede replicar y recuperar máquinas virtuales entre dos regiones cualesquiera dentro del mismo clúster geográfico. Los clústeres geográficos se definen teniendo presente la latencia y la soberanía de datos.


**Clúster geográfico** | **Regiones de Azure**
-- | --
América | Centro de Canadá y este de Canadá, centro-sur de EE. UU., centro-oeste de EE. UU., este de EE. UU., este de EE. UU. 2, oeste de EE. UU., oeste de EE. UU. 2 centro de EE. UU., centro-norte de EE. UU.
Europa | Oeste de Reino Unido, Sur de Reino Unido, Europa del Norte, Europa Occidental, Centro de Francia, Sur de Francia
Asia | India del Sur, centro de la India, Sudeste Asiático, Asia Oriental, Japón Oriental, Japón Occidental, Corea Central, Corea del Sur
Australia   | Este de Australia, Sudeste de Australia, Centro de Australia, Centro de Australia 2
Azure Government    | Virginia Gob. EE. UU., Iowa Gob. EE. UU., US Gov de Arizona, US Gov de Texas, US DOD este, US DOD central
Alemania | Centro de Alemania y Noreste de Alemania
China | Este de China, Norte de China, Norte de China 2 y Este de China 2

>[!NOTE]
>
> - Para la región **Sur de Brasil**, puede replicar y conmutar por error en una de las siguientes regiones: Centro-sur de EE. UU., Centro-oeste de EE. UU., Este de EE. UU., Este de EE. UU. 2, Oeste de EE. UU., Oeste de EE. UU. 2 y Centro-norte de EE. UU. Debe tenerse en cuenta que Site Recovery solo ha habilitado el Sur de Brasil para su uso como región de origen desde donde se pueden proteger las VM. **No puede usarse como región de recuperación ante desastres de destino** en ninguna de las regiones de Azure, como Centro-sur de EE. UU. Se debe a que se recomienda que la latencia observada debido a la distancia geográfica seleccione una región americana que no sea el Sur de Brasil.
> 
> - Si **no puede ver una región** donde quiere **crear un almacén**, asegúrese de que su suscripción tenga acceso para crear recursos en esa región. Por ejemplo:  Si no puede crear un almacén en el Sur de Francia, la suscripción no tiene acceso a la región Sur de Francia. Complete la incidencia de soporte técnico para tipo de problema "administración de suscripciones" y tipo de problema "otras preguntas generales", asunto "incluir en la lista blanca la suscripción a la región de Azure XXX"
> 
> - Si **no puede ver una región** dentro de un clúster geográfico **durante la habilitación de la replicación**, asegúrese de que su suscripción tiene acceso para crear máquinas virtuales en dicha región. Por ejemplo:  Si intenta proteger máquinas virtuales del Centro de Francia al Sur de Francia y no ve Sur de Francia en la lista desplegable de regiones, su suscripción no tiene acceso para implementar VM en esa región. Complete la incidencia de soporte técnico para tipo de problema "administración de suscripciones" y tipo de problema "otras preguntas generales", asunto "incluir en la lista blanca la suscripción a la región de Azure XXX"
> - No puede seleccionar las regiones en los clústeres geográficos mencionados anteriormente.


## <a name="cache-storage"></a>Almacenamiento en caché

Esta tabla resume la compatibilidad con la cuenta de almacenamiento de la memoria caché que usa Site Recovery durante la replicación.

**Configuración** | **Soporte técnico** | **Detalles**
--- | --- | ---
Cuentas de almacenamiento de uso general V2 (capas de acceso frecuente y esporádico) | No compatible. | Existe la limitación para el almacenamiento en caché ya que los costos de las transacciones para V2 son significativamente más altos que los de las cuentas de almacenamiento V1.
Firewalls de Azure Storage para redes virtuales  | Compatible | Si usa una cuenta de almacenamiento en caché o de destino con firewall habilitado, asegúrese de seleccionar ["Permitir servicios de Microsoft de confianza"](https://docs.microsoft.com/azure/storage/common/storage-network-security#exceptions).


## <a name="replicated-machine-operating-systems"></a>Sistemas operativos de máquinas replicadas

Site Recovery admite la replicación de máquinas virtuales de Azure que ejecutan los sistemas operativos enumerados en esta sección.

### <a name="windows"></a> Windows

**Sistema operativo** | **Detalles**
--- | ---
Windows Server 2016  | Server Core y Server con Experiencia de escritorio
Windows Server 2012 R2 |
Windows Server 2012 |
Windows Server 2008 R2 | Con ejecución de SP1 o posterior

#### <a name="linux"></a>Linux

**Sistema operativo** | **Detalles**
--- | ---
Red Hat Enterprise Linux | 6.7, 6.8, 6.9, 6.10, 7.0, 7.1, 7.2, 7.3, 7.4, 7.5, 7.6  
CentOS | 6.5, 6.6, 6.7, 6.8, 6.9, 6.10, 7.0, 7.1, 7.2, 7.3, 7.4, 7.5, 7.6
Servidor Ubuntu 14.04 LTS | [Versiones de kernel admitidas](#supported-ubuntu-kernel-versions-for-azure-virtual-machines)
Servidor Ubuntu 16.04 LTS | [Versión de kernel admitida](#supported-ubuntu-kernel-versions-for-azure-virtual-machines)<br/><br/> Los servidores Ubuntu que utilizan inicio de sesión y autenticación mediante contraseña y que utilizan el paquete cloud-init para configurar máquinas virtuales en la nube, pueden tener el inicio de sesión mediante contraseña deshabilitado tras una conmutación por error (en función de la configuración de cloudinit). Para volver a habilitar el inicio de sesión mediante contraseña en la máquina virtual, restablezca la contraseña desde el menú Soporte > Solución de problemas > Configuración (de la máquina virtual conmutada por error en Azure Portal).
Debian 7 | [Versiones de kernel admitidas](#supported-debian-kernel-versions-for-azure-virtual-machines)
Debian 8 | [Versiones de kernel admitidas](#supported-debian-kernel-versions-for-azure-virtual-machines)
SUSE Linux Enterprise Server 12 | SP1, SP2, SP3. [(Versiones de kernel admitidas)](#supported-suse-linux-enterprise-server-12-kernel-versions-for-azure-virtual-machines)
SUSE Linux Enterprise Server 11 | SP3<br/><br/> No se admite la actualización de máquinas de replicación de SP3 a SP4. Si se ha actualizado una máquina replicada, deberá deshabilitar la replicación y volverla a habilitar después de la actualización.
SUSE Linux Enterprise Server 11 | SP4
Oracle Linux | 6.4, 6.5, 6.6, 6.7, 6.8, 6.9, 6.10, 7.0, 7.1, 7.2, 7.3, 7.4, 7.5 <br/><br/> Ejecutan el kernel compatible de Red Hat o Unbreakable Enterprise Kernel Release 3 (UEK3).


#### <a name="supported-ubuntu-kernel-versions-for-azure-virtual-machines"></a>Versiones de kernel de Ubuntu admitidas para máquinas virtuales de Azure

**Versión** | **Versión de Mobility service** | **Versión de kernel** |
--- | --- | --- |
14.04 LTS | 9.21 | 3.13.0-24-generic a 3.13.0-163-generic,<br/>3.16.0-25-generic a 3.16.0-77-generic,<br/>3.19.0-18-generic a 3.19.0-80-generic,<br/>4.2.0-18-generic a 4.2.0-42-generic,<br/>4.4.0-21-generic a 4.4.0-140-generic,<br/>4.15.0-1023-azure a 4.15.0-1035-azure |
14.04 LTS | 9.20 | 3.13.0-24-generic a 3.13.0-161-generic,<br/>3.16.0-25-generic a 3.16.0-77-generic,<br/>3.19.0-18-generic a 3.19.0-80-generic,<br/>4.2.0-18-generic a 4.2.0-42-generic,<br/>4.4.0-21-generic a 4.4.0-138-generic,<br/>4.15.0-1023-azure a 4.15.0-1030-azure |
14.04 LTS | 9.19 | 3.13.0-24-generic a 3.13.0-153-generic,<br/>3.16.0-25-generic a 3.16.0-77-generic,<br/>3.19.0-18-generic a 3.19.0-80-generic,<br/>4.2.0-18-generic a 4.2.0-42-generic,<br/>4.4.0-21-generic a 4.4.0-131-generic |
14.04 LTS | 9.18 | 3.13.0-24-generic a 3.13.0-151-generic,<br/>3.16.0-25-generic a 3.16.0-77-generic,<br/>3.19.0-18-generic a 3.19.0-80-generic,<br/>4.2.0-18-generic a 4.2.0-42-generic,<br/>4.4.0-21-generic a 4.4.0-128-generic |
|||
16.04 LTS | 9.21 | 4.4.0-21-generic a 4.4.0-140-generic,<br/>4.8.0-34-generic a 4.8.0-58-generic,<br/>4.10.0-14-generic a 4.10.0-42-generic,<br/>4.11.0-13-generic to 4.11.0-14-generic,<br/>4.13.0-16-generic a 4.13.0-45-generic,<br/>4.15.0-13-generic a 4.15.0-42-generic<br/>4.11.0-1009-azure to 4.11.0-1016-azure,<br/>4.13.0-1005-azure a 4.13.0-1018-azure <br/>4.15.0-1012-azure a 4.15.0-1035-azure|
16.04 LTS | 9.20 | 4.4.0-21-generic a 4.4.0-138-generic,<br/>4.8.0-34-generic a 4.8.0-58-generic,<br/>4.10.0-14-generic a 4.10.0-42-generic,<br/>4.11.0-13-generic to 4.11.0-14-generic,<br/>4.13.0-16-generic a 4.13.0-45-generic,<br/>4.15.0-13-generic a 4.15.0-38-generic<br/>4.11.0-1009-azure to 4.11.0-1016-azure,<br/>4.13.0-1005-azure a 4.13.0-1018-azure <br/>4.15.0-1012-azure a 4.15.0-1030-azure|
16.04 LTS | 9.19 | 4.4.0-21-generic a 4.4.0-131-generic,<br/>4.8.0-34-generic a 4.8.0-58-generic,<br/>4.10.0-14-generic a 4.10.0-42-generic,<br/>4.11.0-13-generic to 4.11.0-14-generic,<br/>4.13.0-16-generic a 4.13.0-45-generic,<br/>4.15.0-13-generic a 4.15.0-30-generic<br/>4.11.0-1009-azure to 4.11.0-1016-azure,<br/>4.13.0-1005-azure a 4.13.0-1018-azure <br/>4.15.0-1012-azure a 4.15.0-1019-azure|
16.04 LTS | 9.18 | 4.4.0-21-generic a 4.4.0-128-generic,<br/>4.8.0-34-generic a 4.8.0-58-generic,<br/>4.10.0-14-generic a 4.10.0-42-generic,<br/>4.11.0-13-generic to 4.11.0-14-generic,<br/>4.13.0-16-generic a 4.13.0-45-generic,<br/>4.11.0-1009-azure to 4.11.0-1016-azure,<br/>4.13.0-1005-azure a 4.13.0-1018-azure |


#### <a name="supported-debian-kernel-versions-for-azure-virtual-machines"></a>Versiones de kernel de Debian admitidas para máquinas virtuales de Azure

**Versión** | **Versión de Mobility service** | **Versión de kernel** |
--- | --- | --- |
Debian 7 | 9.18,9.19,9.20,9.21 | 3.2.0-4-amd64 a 3.2.0-6-amd64, 3.16.0-0.bpo.4-amd64 |
|||
Debian 8 | 9.20, 9.21 | 3.16.0-4-amd64 a 3.16.0-7-amd64, 4.9.0-0.bpo.4-amd64 a 4.9.0-0.bpo.8-amd64 |
Debian 8 | 9.19 | 3.16.0-4-amd64 a 3.16.0-6-amd64, 4.9.0-0.bpo.4-amd64 a 4.9.0-0.bpo.7-amd64 |
Debian 8 | 9.18 | 3.16.0-4-amd64 a 3.16.0-6-amd64, 4.9.0-0.bpo.4-amd64 a 4.9.0-0.bpo.6-amd64 |

#### <a name="supported-suse-linux-enterprise-server-12-kernel-versions-for-azure-virtual-machines"></a>Versiones de kernel admitidas de SUSE Linux Enterprise Server 12 para máquinas virtuales de Azure

**Versión** | **Versión de Mobility service** | **Versión de kernel** |
--- | --- | --- |
SUSE Linux Enterprise Server 12 (SP1, SP2, SP3) | 9.21 | SP1 3.12.49-11-default a 3.12.74-60.64.40-default</br></br> SP1(LTSS) 3.12.74-60.64.45-default a 3.12.74-60.64.107-default</br></br> SP2 4.4.21-69-default a 4.4.120-92.70-default</br></br>SP2(LTSS) 4.4.121-92.73-default a 4.4.121-92.98-default</br></br>SP3 4.4.73-5-default a 4.4.162-94.72-default |
SUSE Linux Enterprise Server 12 (SP1, SP2, SP3) | 9.20 | SP1 3.12.49-11-default a 3.12.74-60.64.40-default</br></br> SP1(LTSS) 3.12.74-60.64.45-default a 3.12.74-60.64.107-default</br></br> SP2 4.4.21-69-default a 4.4.120-92.70-default</br></br>SP2(LTSS) 4.4.121-92.73-default a 4.4.121-92.98-default</br></br>SP3 4.4.73-5-default a 4.4.162-94.69-default |
SUSE Linux Enterprise Server 12 (SP1, SP2, SP3) | 9.19 | SP1 3.12.49-11-default a 3.12.74-60.64.40-default</br></br> SP1(LTSS) 3.12.74-60.64.45-default a 3.12.74-60.64.93-default</br></br> SP2 4.4.21-69-default a 4.4.120-92.70-default</br></br>SP2(LTSS) 4.4.121-92.73-default a 4.4.121-92.80-default</br></br>SP3 4.4.73-5-default a 4.4.140-94.42-default |
SUSE Linux Enterprise Server 12 (SP1, SP2, SP3) | 9.18 | SP1 3.12.49-11-default a 3.12.74-60.64.40-default</br></br> SP1(LTSS) 3.12.74-60.64.45-default a 3.12.74-60.64.93-default</br></br> SP2 4.4.21-69-default a 4.4.120-92.70-default</br></br>SP2(LTSS) 4.4.121-92.73-default a 4.4.121-92.80-default</br></br>SP3 4.4.73-5-default a 4.4.138-94.39-default |


## <a name="replicated-machines---linux-file-systemguest-storage"></a>Máquinas replicadas: Sistema de archivos o almacenamiento de invitado de Linux

* Sistemas de archivos: ext3, ext4, ReiserFS (solo Suse Linux Enterprise Server), XFS, BTRFS
* Administrador de volúmenes: LVM2
* Software de múltiples rutas: Mapeador de dispositivo


## <a name="replicated-machines---compute-settings"></a>Máquinas replicadas: Configuración de proceso

**Configuración** | **Soporte técnico** | **Detalles**
--- | --- | ---
Tamaño | Cualquier tamaño de máquina virtual de Azure con al menos 2 núcleos de CPU y 1 GB de RAM | Compruebe los [tamaños de máquina virtual de Azure](../virtual-machines/windows/sizes.md).
Conjuntos de disponibilidad | Compatible | Si habilita la replicación de una máquina virtual de Azure con las opciones predeterminadas, se creará automáticamente un conjunto de disponibilidad en función de la configuración de la región de origen. Puede modificar estos valores.
Zonas de disponibilidad | Compatible |
Ventaja de uso híbrido (HUB) | Compatible | Si la máquina virtual de origen tiene habilitada una licencia HUB, en la conmutación por error de prueba o la máquina virtual de conmutación por error también se usa la licencia HUB.
VM Scale Sets | No compatible |
Imágenes de la galería de Azure (publicadas por Microsoft) | Compatible | Se admiten si la máquina virtual se ejecuta en un sistema operativo compatible.
Imágenes de la Galería de Azure (publicadas por terceros) | Compatible | Se admiten si la máquina virtual se ejecuta en un sistema operativo compatible.
Imágenes personalizadas (publicadas de terceros) | Compatible | Se admiten si la máquina virtual se ejecuta en un sistema operativo compatible.
Máquinas virtuales migradas con Site Recovery | Compatible | Si se migró una máquina virtual de VMware o una física a Azure mediante Site Recovery, deberá desinstalar la versión antigua de Mobility Service que se ejecuta en la máquina y reiniciarla antes de replicarla en otra región de Azure.

## <a name="replicated-machines---disk-actions"></a>Máquinas replicadas: acciones de disco

**Acción** | **Detalles**
-- | ---
Cambiar el tamaño de disco en una máquina virtual replicada | Compatible
Agregar un disco a una máquina virtual replicada | No compatible.<br/><br/> Tiene que deshabilitar la replicación para la máquina virtual, agregar el disco y volver a habilitar la replicación.

## <a name="replicated-machines---storage"></a>Máquinas replicadas: almacenamiento

Esta tabla resume la compatibilidad con el disco del sistema operativo, el disco de datos y el disco temporal de Azure VM.

- Es importante respetar los límites y destinos de los discos de máquinas virtuales [Linux](../virtual-machines/linux/disk-scalability-targets.md) y [Windows](../virtual-machines/windows/disk-scalability-targets.md) para evitar cualquier problema de rendimiento.
- Si implementa con la configuración predeterminada, Site Recovery creará automáticamente las cuentas de almacenamiento y los discos necesarios en función de la configuración de origen.
- Si desea personalizar, asegúrese de que sigue las directrices.

**Componente** | **Soporte técnico** | **Detalles**
--- | --- | ---
Tamaño máximo del disco de sistema operativo | 2048 GB | [Más información](../virtual-machines/windows/about-disks-and-vhds.md#disks-used-by-vms) sobre discos de máquina virtual.
Disco temporal | No compatible | El disco temporal se excluye de la replicación siempre.<br/><br/> No guarde los datos persistentes en el disco temporal. [Más información](../virtual-machines/windows/about-disks-and-vhds.md#temporary-disk).
Tamaño máximo del disco de datos | 4095 GB |
Número máximo de discos de datos | Hasta 64, según la compatibilidad con un tamaño específico de máquina virtual de Azure | [Más información](../virtual-machines/windows/sizes.md) sobre tamaños de máquina virtual.
Tasa de cambio de disco de datos | Máximo de 10 MBps por disco para el almacenamiento Premium. Máximo de 2 MBps por disco para el almacenamiento Estándar. | Si la tasa media de cambio de los datos en el disco sobrepasa el máximo continuamente, la replicación no mantendrá el ritmo.<br/><br/>  Sin embargo, si se supera el máximo esporádicamente, la replicación podrá mantenerlo, aunque podría ver puntos de recuperación ligeramente retrasados.
Disco de datos: cuenta de almacenamiento Estándar | Compatible |
Disco de datos: cuenta de almacenamiento Premium | Compatible | Si una máquina virtual tiene discos repartidos entre cuentas de almacenamiento Estándar y Premium, puede seleccionar una cuenta de almacenamiento de destino diferente para cada disco a fin de garantizar que tenga la misma configuración de almacenamiento en la región de destino.
Disco administrado: estándar | Se admite en regiones de Azure en las que se pueda usar Azure Site Recovery. |
Disk administrado: premium | Se admite en regiones de Azure en las que se pueda usar Azure Site Recovery. |
SSD estándar | No compatible |
Redundancia | Se admiten LRS y GRS.<br/><br/> No se admite ZRS.
Almacenamiento esporádico y almacenamiento frecuente | No compatible | Los discos de máquina virtual no admiten el almacenamiento esporádico ni el frecuente
Espacios de almacenamiento | Compatible |
Cifrado en reposo (SSE) | Compatible | SSE es la configuración predeterminada en las cuentas de almacenamiento.   
Azure Disk Encryption (ADE) para sistemas operativos Windows | Se admiten las máquinas virtuales habilitadas para [cifrado con la aplicación de Azure AD](https://aka.ms/ade-aad-app) |
Azure Disk Encryption (ADE) para sistemas operativos Linux | No compatible |
Agregar/quitar disco en caliente | No compatible | Si agrega o quita un disco de datos en la máquina virtual, deberá deshabilitar la replicación y habilitarla de nuevo para la máquina virtual.
Excluir el disco | No compatible|   El disco temporal se excluye de forma predeterminada.
Espacios de almacenamiento directo  | No compatible|
Servidor de archivos de escalado horizontal  | No compatible|
LRS | Compatible |
GRS | Compatible |
RA-GRS | Compatible |
ZRS | No compatible |
Almacenamiento en frío y en caliente | No compatible | Los discos de máquina virtual no admiten el almacenamiento temporal y permanente.
Firewalls de Azure Storage para redes virtuales  | Compatible | Si está restringiendo el acceso de red virtual a las cuentas de almacenamiento, asegúrese de ["Permitir servicios de Microsoft de confianza"](https://docs.microsoft.com/azure/storage/common/storage-network-security#exceptions).
Cuentas de almacenamiento de uso general V2 (capas de acceso frecuente y esporádico) | Sin  | Los costos de transacción aumentan considerablemente en comparación con las cuentas de almacenamiento de uso general V1

>[!IMPORTANT]
> Asegúrese de tener en cuenta los objetivos de escalabilidad y rendimiento del disco de la máquina virtual para máquinas virtuales [Linux](../virtual-machines/linux/disk-scalability-targets.md) o [Windows](../virtual-machines/windows/disk-scalability-targets.md) para evitar cualquier problema de rendimiento. Si sigue la configuración predeterminada, Site Recovery creará las cuentas de almacenamiento y discos necesarios en función de la configuración de origen. Si personaliza y selecciona su propia configuración, asegúrese de seguir los objetivos de escalabilidad y rendimiento del disco para las máquinas virtuales de origen.

## <a name="azure-site-recovery-limits-to-replicate-data-change-rates"></a>Límites de Azure Site Recovery para replicar frecuencias de cambio de datos
En la tabla siguiente se proporcionan los límites de Azure Site Recovery. Estos límites se basan en nuestras pruebas, pero no pueden cubrir todas las combinaciones de E/S posibles de la aplicación. Los resultados reales pueden variar en función de la combinación de E/S de la aplicación. También debemos mencionar que hay dos límites que tener en cuenta: renovación de datos por disco y por máquina virtual.
Por ejemplo, si miramos el disco Premium P20 de la siguiente tabla, Site Recovery puede controlar la renovación de 5 MB/s por disco con un máximo de cinco de estos discos por VM, debido al límite de renovación total de 25 MB/s por VM.

**Destino de almacenamiento de la replicación** | **Tamaño medio de E/S de disco de origen** |**Actividad de datos media de disco de origen** | **Actividad de datos de disco de origen total por día**
---|---|---|---
Standard Storage | 8 KB | 2 MB/s | 168 GB por disco
Disco Premium P10 o P15 | 8 KB  | 2 MB/s | 168 GB por disco
Disco Premium P10 o P15 | 16 KB | 4 MB/s |  336 GB por disco
Disco Premium P10 o P15 | 32 KB, o más | 8 MB/s | 672 GB por disco
Disco Premium P20, P30, P40 o P50 | 8 KB    | 5 MB/s | 421 GB por disco
Disco Premium P20, P30, P40 o P50 | 16 KB, o más |10 MB/s | 842 GB por disco
## <a name="replicated-machines---networking"></a>Máquinas replicadas: redes
**Configuración** | **Soporte técnico** | **Detalles**
--- | --- | ---
NIC | Número máximo admitido para un tamaño específico de máquina virtual de Azure | Las NIC se crean cuando se crea la máquina virtual durante la conmutación por error.<br/><br/> El número de tarjetas NIC en la máquina virtual de conmutación por error viene determinado por el número de tarjetas NIC que haya en la máquina virtual de origen en el momento de habilitar la replicación. Si agrega o quita una tarjeta NIC después de habilitar la replicación, esto no influirá en el número de NIC de la máquina virtual replicada después de la conmutación por error.
Equilibrador de carga de Internet | Compatible | Asocie el equilibrador de carga configurado previamente con un script de Azure Automation de un plan de recuperación.
Equilibrador de carga interno | Compatible | Asocie el equilibrador de carga configurado previamente con un script de Azure Automation de un plan de recuperación.
Dirección IP pública | Compatible | Asocie una dirección IP pública existente a la NIC. O bien, cree una dirección IP pública y asóciela con la NIC mediante un script de Azure Automation de un plan de recuperación.
Grupo de seguridad de red en una tarjeta NIC | Compatible | Asocie el grupo de seguridad de red a la NIC con un script de Azure Automation de un plan de recuperación.
Grupo de seguridad de red en la subred | Compatible | Asocie el grupo de seguridad de red a la subred con un script de Azure Automation de un plan de recuperación.
Dirección IP (estática) reservada | Compatible | Si la NIC de la máquina virtual de origen tiene una dirección IP estática y la subred de destino tiene la misma dirección IP disponible, esta se asigna a la máquina virtual de conmutación por error.<br/><br/> Si la subred de destino no tiene la misma dirección IP disponible, una de las direcciones IP disponibles de la subred se reserva para esta máquina virtual.<br/><br/> También puede especificar una dirección IP fija y una subred en **Elementos replicados** > **Configuración** > **Proceso y red** > **Interfaces de red**.
Dirección IP dinámica | Compatible | Si la NIC de origen tiene una dirección IP dinámica, la NIC de la máquina virtual de conmutación por error será también dinámica de forma predeterminada.<br/><br/> Puede modificar esta a una dirección IP fija si es necesario.
Traffic Manager     | Compatible | Puede configurar previamente Traffic Manager de forma que el tráfico se dirija regularmente al punto de conexión de la región de origen y al punto de conexión de la región de destino en caso de conmutación por error.
Azure DNS | Compatible |
DNS personalizado  | Compatible |
Proxy no autenticado | Compatible | Consulte el [documento de instrucciones sobre redes](site-recovery-azure-to-azure-networking-guidance.md).    
Proxy autenticado | No compatible | Si la máquina virtual usa un proxy autenticado para la conectividad saliente, no se puede replicar mediante Azure Site Recovery.    
VPN de sitio a sitio local (con o sin ExpressRoute)| Compatible | Asegúrese de que los UDR y NSG estén configurados de manera que el tráfico de Site Recovery no se dirija al entorno local. Consulte el [documento de instrucciones sobre redes](site-recovery-azure-to-azure-networking-guidance.md).  
Conexión de red virtual a red virtual | Compatible | Consulte el [documento de instrucciones sobre redes](site-recovery-azure-to-azure-networking-guidance.md).  
Puntos de conexión del servicio de redes virtuales | Compatible | Si está restringiendo el acceso de red virtual a las cuentas de almacenamiento, asegúrese de que los servicios de Microsoft de confianza pueden acceder a la cuenta de almacenamiento.
Redes aceleradas | Compatible | Se deben habilitar las redes aceleradas en una máquina virtual de origen. [Más información](azure-vm-disaster-recovery-with-accelerated-networking.md).



## <a name="next-steps"></a>Pasos siguientes
- Aprenda más información en las [instrucciones sobre redes para replicar máquinas virtuales de Azure](site-recovery-azure-to-azure-networking-guidance.md).
- Implemente la recuperación ante desastres mediante [la replicación de máquinas virtuales de Azure](site-recovery-azure-to-azure.md).
