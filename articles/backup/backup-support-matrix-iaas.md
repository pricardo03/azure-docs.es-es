---
title: Matriz de compatibilidad de Azure Backup para copias de seguridad de máquinas virtuales de Azure
description: Proporciona un resumen de opciones de compatibilidad y limitaciones para realizar copias de seguridad de máquinas virtuales de Azure con el servicio Azure Backup.
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 02/24/2019
ms.author: raynew
ms.openlocfilehash: aacfe725310b3c8e4785e24b80728f0e60694814
ms.sourcegitcommit: 1a19a5845ae5d9f5752b4c905a43bf959a60eb9d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/11/2019
ms.locfileid: "59496102"
---
# <a name="support-matrix-for-azure-vm-backup"></a>Matriz de compatibilidad para copias de seguridad de máquinas virtuales de Azure
Puede usar el [servicio Azure Backup](backup-overview.md) para realizar una copia de seguridad de máquinas locales y las cargas de trabajo y máquinas virtuales (VM). Este artículo resumen las opciones de soporte técnico y las limitaciones al realizar copias de seguridad de máquinas virtuales de Azure con Azure Backup.

Otras matrices de compatibilidad:

- [Matriz de compatibilidad general](backup-support-matrix.md) para copia de seguridad de Azure
- [Matriz de compatibilidad](backup-support-matrix-mabs-dpm.md) para Azure Backup server y System Center Data Protection Manager (DPM) de copia de seguridad
- [Matriz de compatibilidad](backup-support-matrix-mars-agent.md) para copia de seguridad con el agente de Microsoft Azure Recovery Services (MARS)

## <a name="supported-scenarios"></a>Escenarios admitidos

A continuación, se muestra cómo puede realizar copias de seguridad y restauraciones de máquinas virtuales de Azure con el servicio Azure Backup.

**Escenario** | **Copia de seguridad** | **Agente** |**Restauración**
--- | --- | --- | ---
copia de seguridad directa de máquinas virtuales de Azure  | Copia de seguridad de toda la máquina virtual.  | No se necesita ningún agente en la máquina virtual de Azure. Azure Backup instala y usa una extensión de la [agente de máquina virtual de Azure](https://docs.microsoft.com/azure/virtual-machines/extensions/agent-windows) que se está ejecutando en la máquina virtual. | Realice la restauración como sigue:<br/><br/> - **Creación de una máquina virtual básica**. Esto es útil si la máquina virtual no tiene ninguna configuración especial, como varias direcciones IP.<br/><br/> - **Restauración del disco de máquina virtual**. Restaure el disco. A continuación, adjuntarla a una máquina virtual existente o crear una nueva máquina virtual desde el disco mediante el uso de PowerShell.<br/><br/> - **Sustitución del disco de máquina virtual**. Si existe una máquina virtual y esta usa discos administrados (sin cifrar), puede restaurar un disco y usarlo para reemplazar un disco existente en la máquina virtual.<br/><br/> - **Restauración de archivos y carpetas específicos**. Puede restaurar archivos o carpetas desde una máquina virtual en lugar de en toda la máquina virtual.
Copia de seguridad directa de máquinas virtuales de Azure (solo Windows)  | Copia de seguridad de archivos, carpetas o volúmenes específico. | Instalar el [agente Azure Recovery Services](backup-azure-file-folder-backup-faq.md).<br/><br/> Puede ejecutar al agente de MARS junto con la extensión de copia de seguridad del agente de máquina virtual de Azure para realizar copias de seguridad de la máquina virtual en el nivel de archivo o carpeta. | Restauración de archivos y carpetas específicos.
Realizar copias de seguridad de máquina virtual de Azure backup server  | Realizar una copia de seguridad de archivos/carpetas/volúmenes; archivos del sistema operativo en el estado/completa; datos de la aplicación para System Center DPM o Microsoft Azure Backup Server (MABS).<br/><br/> DPM/MABS, a continuación, realiza copias de seguridad en el almacén de copia de seguridad. | Instale al agente de protección DPM/MABS en la máquina virtual. El agente de MARS se instala en DPM/MABS.| Restauración de archivos, carpetas y volúmenes; archivos de estado del sistema y de reconstrucción completa; datos de aplicaciones.

Más información sobre la copia de seguridad [mediante un servidor de copia de seguridad](backup-architecture.md#architecture-back-up-to-dpmmabs) y aproximadamente [admitir requisitos](backup-support-matrix-mabs-dpm.md).

## <a name="supported-backup-actions"></a>Acciones de copia de seguridad admitidas

**.** | **Soporte técnico**
--- | ---
Habilitar copia de seguridad al crear una máquina virtual de Azure con Windows | Compatible para:  Windows Server 2019 (centro de datos/Datacenter Core), Windows Server 2016 (Core/centro de datos de centro de datos); Windows Server 2012 R2 Datacenter; Windows Server 2008 R2 (versiones RTM y SP1)
Habilitar copia de seguridad al crear una máquina virtual Linux | Compatible para:<br/><br/> - Ubuntu Server: 1710, 1704, 1604 (LTS), 1404 (LTS)<br/><br/> - Red Hat: RHEL 6.7, 6.8, 6.9, 7.2, 7.3, 7.4<br/><br/> - SUSE Linux Enterprise Server: 11 SP4, 12 SP2, 12 SP3, 15 <br/><br/> - Debian: 8, 9<br/><br/> - CentOS: 6.9, 7.3<br/><br/> - Oracle Linux: 6.7, 6.8, 6.9, 7.2, 7.3
Copia de seguridad de una máquina virtual que está apagado o desconectado VM |  Se admite.<br/><br/> La instantánea es coherente solo con bloqueos, no con aplicaciones.
Realizar una copia de seguridad de discos después de migrar a discos administrados |  Se admite.<br/><br/> La copia de seguridad seguirá funcionando. no se requiere ninguna acción.
Copia de seguridad de discos administrados después de habilitar el bloqueo del grupo de recursos | No compatible.<br/><br/> Copia de seguridad de Azure no puede eliminar los puntos anteriores de recursos y las copias de seguridad se iniciarán un error cuando se alcanza el límite máximo de puntos de restauración.
Modificar directiva de copia de seguridad de una máquina virtual |  Se admite.<br/><br/> La máquina virtual se hará copia mediante la configuración de retención y programación en la nueva directiva. Si la configuración de retención está extendida, los puntos de recuperación existentes se marcan y se mantienen. Si se reduzcan, puntos de recuperación existentes se van a eliminar en el siguiente trabajo de limpieza y finalmente se eliminará.
Cancelación de un trabajo de copia de seguridad | Compatible durante el proceso de instantáneas.<br/><br/> No se admite cuando se transfiere la instantánea al almacén.
Copia de seguridad de la máquina virtual en otra región o suscripción |  No compatible.
Copias de seguridad por día (mediante la extensión de máquina virtual de Azure) | Una copia de seguridad programada por día.<br/><br/> Puede realizar hasta cuatro copias de seguridad y a petición por día.
Copias de seguridad por día (mediante el agente de MARS) | Tres copias de seguridad programadas por día.
Copias de seguridad por día (mediante DPM/MABS) | Dos copias de seguridad programadas por día.
Copia de seguridad mensual o anual   | No se admite cuando la copia de seguridad se realiza con la extensión de máquina virtual de Azure. Solo se admiten copias de seguridad diarias y semanales.<br/><br/> Puede configurar la directiva para conservar las copias de seguridad diarias y semanales durante el período de retención mensual o anual.
Ajuste automático del reloj | No compatible.<br/><br/> Copia de seguridad de Azure no ajusta automáticamente los cambios del horario de verano, cuando la copia de seguridad de una máquina virtual.<br/><br/>  Modifique la directiva de forma manual según sea necesario.
[Características de seguridad para copia de seguridad híbrida](https://docs.microsoft.com/azure/backup/backup-azure-security-feature) |  No se admite la deshabilitación de características de seguridad.

## <a name="operating-system-support-windows"></a>Compatibilidad con sistema operativo (Windows)

En la tabla siguiente se resume los sistemas operativos compatibles cuando la copia de seguridad de máquinas virtuales de Windows Azure.

**Escenario** | **Sistemas operativos admitidos**
--- | ---
Copia de seguridad con la extensión del agente de máquina virtual de Azure | Cliente Windows: No compatible<br/><br/> Windows Server 2019 (centro de datos/Datacenter Core), Windows Server 2016 (Core/centro de datos de centro de datos); Windows Server 2012 R2 Datacenter; Windows Server 2008 R2 (versiones RTM y SP1)
Copia de seguridad con el agente de MARS | Sistemas operativos [compatibles](backup-support-matrix-mars-agent.md#support-for-direct-backups).
Copia de seguridad con DPM/MABS | Sistemas operativos compatibles para copia de seguridad con [MABS](backup-mabs-protection-matrix.md) y [DPM](https://docs.microsoft.com/system-center/dpm/dpm-protection-matrix?view=sc-dpm-1807).

## <a name="support-for-linux-backup"></a>Compatibilidad con copia de seguridad de Linux

Esto es lo que se admite si quiere hacer copias de seguridad de máquinas Linux.

**.** | **Soporte técnico**
--- | ---
Copia de seguridad de máquinas virtuales de Azure con Linux mediante el agente de máquina virtual de Azure para Linux | Copia de seguridad coherente con archivos.<br/><br/> Copia de seguridad coherente con la aplicación mediante [scripts personalizados](backup-azure-linux-app-consistent.md).<br/><br/> Durante la restauración, puede crear una nueva máquina virtual, restaurar un disco y usarlo para crear una máquina virtual, o restaurar un disco y úselo para reemplazar un disco en una máquina virtual existente. También puede restaurar archivos y carpetas individuales.
Copia de seguridad de máquinas virtuales de Azure con Linux mediante el agente de MARS | No compatible.<br/><br/> El agente MARS solo puede instalarse en máquinas Windows.
Copia de seguridad de máquinas virtuales de Azure con Linux mediante DPM/MABS | No compatible.

## <a name="operating-system-support-linux"></a>Compatibilidad con sistema operativo (Linux)

Para las copias de seguridad de máquinas virtuales de Azure con Linux, Azure Backup admite la [lista de distribuciones aprobadas por Azure](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros) para Linux. Tenga en cuenta lo siguiente:

- Azure Backup no admite Core OS Linux.
- Azure Backup no admite sistemas operativos de 32 bits.
- Otras distribuciones de Linux bring your own podrían funcionar siempre y cuando la [agente de máquina virtual de Azure para Linux](https://docs.microsoft.com/azure/virtual-machines/extensions/agent-linux) está disponible en la máquina virtual y, siempre que sea compatible con Python.
- Azure Backup no admite una VM de Linux proxy configurado si no tiene la versión 2.7 instalado Python.


## <a name="backup-frequency-and-retention"></a>Frecuencia y retención de copias de seguridad

**Configuración** | **límites**
--- | ---
Puntos de recuperación máximo por instancia protegida (máquina/carga de trabajo) | 9999.
Tiempo de expiración máximo de un punto de recuperación | No hay límite.
Frecuencia máxima de copia de seguridad en un almacén (extensión de máquina virtual de Azure) | Una vez al día.
Frecuencia máxima de copia de seguridad en el almacén (agente de MARS) | Tres copias de seguridad por día.
Frecuencia máxima de copia de seguridad en DPM/MABS | Cada 15 minutos para SQL Server.<br/><br/> Una vez por hora para otras cargas de trabajo.
Retención de punto de recuperación | Diariamente, semanalmente, mensualmente y anualmente.
Período de retención máximo | Depende de la frecuencia de la copia de seguridad.
Puntos de recuperación en disco DPM/MABS | 64 para servidores de archivos y 448 para los servidores de aplicaciones.<br/><br/> Los puntos de recuperación de cinta son ilimitados para DPM en el entorno local.

## <a name="supported-restore-methods"></a>Métodos de restauración admitidos

**Método de restauración** | **Detalles**
--- | ---
Creación de una máquina virtual nueva | Puede crear una máquina virtual durante el proceso de restauración. <br/><br/> Esta opción pone en marcha una máquina virtual básica. Puede especificar el nombre de la máquina virtual, el grupo de recursos, la red virtual, la subred y el almacenamiento.  
Restauración de un disco | Puede restaurar un disco y usarlo para crear una máquina virtual.<br/><br/> Cuando se selecciona esta opción, Azure Backup copia datos desde el almacén a una cuenta de almacenamiento que seleccione. El trabajo de restauración genera una plantilla. Puede descargar esta plantilla, úselo para especificar la configuración de máquina virtual personalizada y crear una máquina virtual.<br/><br/> Esta opción permite especificar más opciones de configuración que la opción anterior para crear una máquina virtual.<br/><br/>
Reemplazar un disco existente | Puede restaurar un disco y, después, usar el disco restaurado para reemplazar un disco que se encuentra actualmente en una máquina virtual.
Restaurar archivos | Puede recuperar archivos desde un punto de recuperación seleccionado. Descargue un script para montar el disco de máquina virtual a partir del punto de recuperación. A continuación, examine los volúmenes de disco para buscar los archivos o carpetas que desea recuperar y desmonte el disco cuando haya terminado.

## <a name="support-for-file-level-restore"></a>Compatibilidad con restauración de nivel de archivo

**Restauración** | **Compatible**
--- | ---
Restaurar archivos entre sistemas operativos | Puede restaurar archivos en cualquier máquina que tenga el mismo sistema operativo que la máquina virtual de copia de seguridad, o bien uno compatible. Consulte la [tabla del sistema operativo Compatible](backup-azure-restore-files-from-vm.md#system-requirements).
Restaurar archivos en máquinas virtuales clásicas | No compatible.
Restaurar archivos desde máquinas virtuales cifradas | No compatible.
Restaurar archivos desde cuentas de almacenamiento con acceso restringido a la red | No compatible.
Restaurar archivos en máquinas virtuales con espacios de almacenamiento de Windows | La restauración no se admite en la misma máquina virtual.<br/><br/> En su lugar, restaure los archivos en una máquina virtual compatible.
Restaurar archivos en máquinas virtuales Linux con matrices RAID/LVM | La restauración no se admite en la misma máquina virtual.<br/><br/> Restaure en una máquina virtual compatible.
Restaurar archivos con una configuración de red especial | La restauración no se admite en la misma máquina virtual. <br/><br/> Restaure en una máquina virtual compatible.

## <a name="support-for-vm-management"></a>Compatibilidad con la administración de máquina virtual

En la tabla siguiente se resume la compatibilidad con copia de seguridad durante las tareas de administración de máquina virtual, como agregar o reemplazar los discos de máquina virtual.

**Restauración** | **Compatible**
--- | ---
Restaurar por suscripción, región o zona. | No compatible.
Restaurar a una máquina virtual existente | Utilice la opción Reemplazar disco.
Restaurar disco con cuenta de almacenamiento habilitada para Azure Storage Service Encryption (SSE) | No compatible.<br/><br/> Restaure en una cuenta que no tenga SSE habilitado.
Restaurar en cuentas de almacenamiento mixtas | No compatible.<br/><br/> Según el tipo de cuenta de almacenamiento, todos los discos restaurados serán premium o estándar y no mixtos.
Restaurar a la cuenta de almacenamiento mediante el uso de almacenamiento con redundancia de zona (ZRS) | Compatible (para la máquina virtual que se copia de seguridad después de enero de 2019 y dónde [zona de disponibilidad](https://azure.microsoft.com/global-infrastructure/availability-zones/) están disponibles)
Restaurar la máquina virtual directamente en un conjunto de disponibilidad | Para los discos administrados, puede restaurar el disco y usar la opción de conjunto de disponibilidad de la plantilla.<br/><br/> No se admite para discos no administrados. Para discos no administrados, restaure el disco y, después, cree una máquina virtual en el conjunto de disponibilidad.
Restaurar la copia de seguridad de máquinas virtuales no administradas después de actualizar a administre la máquina virtual|  Se admite.<br/><br/> Puede restaurar discos y después crear una máquina virtual administrada.
Restaurar máquina virtual a un punto de restauración antes de que la máquina virtual se migre a discos administrados |  Se admite.<br/><br/> Restaure a discos no administrados (opción predeterminada), convierta los discos restaurados en discos administrados y cree una máquina virtual con los discos administrados.
Restaure una máquina virtual que se haya eliminado. |  Se admite.<br/><br/> Puede restaurar la máquina virtual desde un punto de recuperación.
Restaurar una máquina virtual de controlador de dominio que forma parte de una configuración de varios controladores de dominio mediante el portal | Se admite si restaurar el disco y crear una máquina virtual mediante PowerShell.
Restaurar la máquina virtual en otra red virtual |    Se admite.<br/><br/> La red virtual debe estar en la misma suscripción y región.

## <a name="vm-compute-support"></a>Compatibilidad con proceso de máquina virtual

**Proceso** | **Soporte técnico**
--- | ---
Tamaño de VM |   Cualquier tamaño de máquina virtual de Azure con al menos 2 núcleos de CPU y 1 GB de RAM<br/><br/> [Más información.](https://docs.microsoft.com/azure/virtual-machines/windows/sizes)
Copia de seguridad de máquinas virtuales en [conjuntos de disponibilidad](https://docs.microsoft.com/azure/virtual-machines/windows/regions-and-availability#availability-sets) |  Se admite.<br/><br/> No se puede restaurar una máquina virtual en un conjunto de disponibilidad mediante la opción para crear rápidamente una máquina virtual. En su lugar, cuando se restaura la máquina virtual, restaure el disco y usarlo para implementar una máquina virtual, o restaurar un disco y úselo para reemplazar un disco existente.
Copia de seguridad de máquinas virtuales en [zonas de disponibilidad](https://docs.microsoft.com/azure/availability-zones/az-overview) |  No compatible.
Copia de seguridad de máquinas virtuales que se implementan con [ventaja de uso híbrido (HUB)](https://docs.microsoft.com/azure/virtual-machines/windows/hybrid-use-benefit-licensing) |  Se admite.
Copia de seguridad de máquinas virtuales que se implementan en un [conjunto de escalado](https://docs.microsoft.com/azure/virtual-machine-scale-sets/overview) |  No compatible.
Copia de seguridad de máquinas virtuales que se implementan desde la [Azure Marketplace](https://azuremarketplace.microsoft.com/en-us/marketplace/apps?filters=virtual-machine-images)<br/><br/> (Publicado por Microsoft, terceros) |   Se admite.<br/><br/> La máquina virtual debe ejecutar un sistema operativo compatible.<br/><br/> Al recuperar archivos en la máquina virtual, puede restaurar solo en un sistema operativo compatible (no en un sistema operativo anterior ni posterior).
Realizar una copia de seguridad de máquinas virtuales que se implementan desde una imagen personalizada (terceros) |    Se admite.<br/><br/> La máquina virtual debe ejecutar un sistema operativo compatible.<br/><br/> Al recuperar archivos en la máquina virtual, puede restaurar solo en un sistema operativo compatible (no en un sistema operativo anterior ni posterior).
Realizar una copia de seguridad de máquinas virtuales que se migran a Azure  |  Se admite.<br/><br/> Para realizar copias de seguridad de la máquina virtual, el agente de máquina virtual debe estar instalado en la máquina migrada.
Realizar copias de seguridad de la coherencia de múltiples VM | Copia de seguridad de Azure no proporciona coherencia de datos y aplicaciones entre varias máquinas virtuales.


## <a name="vm-storage-support"></a>Compatibilidad con almacenamiento de máquina virtual

**Componente** | **Soporte técnico**
--- | ---
Discos de datos de máquinas virtuales de Azure | Copia de seguridad de una máquina virtual con 16 discos de datos o menos. <br/><br/> Compatibilidad con tamaños de disco de hasta 4 TB.
Tamaño del disco de datos | El disco individual puede tener hasta 4095 GB.<br/><br/> Si los almacenes se está ejecutando la versión más reciente de copia de seguridad de Azure (conocido como la restauración instantánea), los tamaños de disco de hasta 4 TB se admiten. [Más información](backup-instant-restore-capability.md).  
Tipo de almacenamiento | Premium HDD, SSD estándar, estándar SSD. <br/><br/> SSD estándar se admite si los almacenes se actualizan a la versión más reciente de copia de seguridad de máquina virtual de Azure (conocido como la restauración instantánea). [Más información](backup-instant-restore-capability.md).
Discos administrados |  Se admite.
Discos cifrados |  Se admite.<br/><br/> Máquinas virtuales de Azure habilitadas con Azure Disk Encryption pueden copia (con o sin la aplicación de Azure AD).<br/><br/> Las máquinas virtuales cifradas no se pueden recuperar a nivel de archivo o carpeta. Debe recuperar toda la máquina virtual.<br/><br/> Puede habilitar el cifrado en máquinas virtuales que ya estén protegidas con Azure Backup.
Discos con el Acelerador de escritura habilitado | No compatible.<br/><br/> Si ejecuta la última versión de copia de seguridad de máquina virtual de Azure (lo que se conoce como [restauración instantánea](backup-instant-restore-capability.md)), puede excluir discos de la copia de seguridad que tengan habilitado el Acelerador de escritura.
Copia de seguridad de discos desduplicados | No compatible.
Agregar disco a una máquina virtual protegida |  Se admite.
Cambiar tamaño de disco de una máquina virtual protegida |  Se admite.
Almacenamiento compartido| No se recomienda la copia de seguridad de máquinas virtuales con volúmenes compartidos de clúster (CSV) o el servidor de archivos de escalabilidad horizontal. Es probable que los escritores CSV a un error durante la copia de seguridad. En la restauración, los discos que contienen volúmenes CSV es posible que no vienen vertical.

> [!NOTE]
> Azure Backup no admite discos con bandas. No se recomienda cambiar el tamaño del disco de copia de seguridad de Azure.


## <a name="vm-network-support"></a>Compatibilidad con red de VM

**Componente** | **Soporte técnico**
--- | ---
Número de interfaces de red (NIC) | Hasta el número máximo de NIC admitidas por un tamaño específico de máquina virtual de Azure.<br/><br/> Las NIC se crean cuando se crea la máquina virtual durante el proceso de restauración.<br/><br/> El número de NIC de la máquina virtual restaurada refleja el número de NIC de la máquina virtual cuando se habilita la protección. Eliminación de NIC después de habilitar la protección no afectan al recuento.
Equilibrador de carga interno y externo |    Se admite. <br/><br/> [Obtenga más información](backup-azure-arm-restore-vms.md#restore-vms-with-special-configurations) sobre cómo restaurar máquinas virtuales con una configuración de red especial.
Varias direcciones IP reservadas |     Se admite. <br/><br/> [Obtenga más información](backup-azure-arm-restore-vms.md#restore-vms-with-special-configurations) sobre cómo restaurar máquinas virtuales con una configuración de red especial.
Máquinas virtuales con varios adaptadores de red  |  Se admite. <br/><br/> [Obtenga más información](backup-azure-arm-restore-vms.md#restore-vms-with-special-configurations) sobre cómo restaurar máquinas virtuales con una configuración de red especial.
Máquinas virtuales con direcciones IP públicas    |  Se admite.<br/><br/> Asociar una dirección IP pública existente a la NIC, o cree una dirección y asociarla a la NIC después de realiza la restauración.
Grupo de seguridad de red (NSG) en la NIC o la subred. |    Se admite.
Dirección IP reservada (estática) | No compatible.<br/><br/> No se puede realizar una copia de una máquina virtual con una dirección IP reservada y ningún punto de conexión definido.
Dirección IP dinámica |     Se admite.<br/><br/> Si la NIC en el origen de la máquina virtual usa una dirección IP dinámica, de forma predeterminada la NIC en la máquina virtual restaurada usará demasiado.
Administrador de tráfico de Azure   |  Se admite.<br/><br/>Si la máquina virtual de copia de seguridad está en el Administrador de tráfico, agregar manualmente la máquina virtual restaurada a la misma instancia de Traffic Manager.
Azure DNS |  Se admite.
DNS personalizado |     Se admite.
Conectividad saliente mediante un proxy HTTP |  Se admite.<br/><br/> No se admite un proxy basado autenticado.
Puntos de conexión del servicio Virtual Network   |  Se admite.<br/><br/> Firewall y la configuración de cuenta de almacenamiento de red virtual debe permitir el acceso desde todas las redes.



## <a name="vm-security-and-encryption-support"></a>Compatibilidad de cifrado y seguridad de máquina virtual

Azure Backup admite el cifrado para datos en tránsito y en reposo:

Tráfico de red en Azure:

- Tráfico de copia de seguridad de servidores en el almacén de Recovery Services se cifra mediante el uso de cifrado estándar avanzado 256.
- Los datos de copia de seguridad se envían a través de un vínculo HTTPS seguro.
- Los datos de copia de seguridad se almacenan en el almacén de Recovery Services en su forma cifrada.
- Solo el usuario tiene la frase de contraseña para desbloquear estos datos. Microsoft no puede descifrar los datos de copia de seguridad en ningún momento.

  > [!WARNING]
  > Una vez configurado el almacén, solo tendrá acceso a la clave de cifrado. Microsoft nunca mantiene una copia y no tiene acceso a la clave. Si la clave se pierde, Microsoft no puede recuperar los datos de copia de seguridad.

Seguridad de los datos:

- Cuando la copia de seguridad de máquinas virtuales de Azure, deberá configurar el cifrado *dentro de* la máquina virtual.
- Azure Backup admite Azure Disk Encryption, que usa BitLocker en las máquinas virtuales de Windows y nos **dm-crypt** en máquinas virtuales Linux.
- En el back-end, Azure Backup usa el [cifrado del servicio Azure Storage](../storage/common/storage-service-encryption.md), que protege los datos en reposo.


**Máquina** | **En tránsito** | **En reposo**
--- | --- | ---
Máquinas Windows locales sin DPM/MABS | ![Sí][green] | ![Sí][green]
Máquinas virtuales de Azure | ![Sí][green] | ![Sí][green]
Máquinas virtuales de Azure o locales con DPM | ![Sí][green] | ![Sí][green]
Máquinas virtuales de Azure o locales con MABS | ![Sí][green] | ![Sí][green]



## <a name="vm-compression-support"></a>Compatibilidad de compresión de máquina virtual

Copia de seguridad admite la compresión del tráfico de copia de seguridad, como se resume en la tabla siguiente. Tenga en cuenta lo siguiente:

- Las máquinas virtuales de Azure, la extensión de VM lee los datos directamente desde la cuenta de almacenamiento de Azure a través de la red de almacenamiento. No es necesario comprimir este tráfico.
- Si usa DPM o MABS, puede ahorrar ancho de banda mediante la compresión de los datos antes de es una copia de seguridad para DPM/MABS.

**Máquina** | **Comprimir (TCP) de DPM/MABS** | **Comprimir en el almacén (HTTPS)**
--- | --- | ---
Máquinas Windows locales sin DPM/MABS | N/D | ![Sí][green]
Máquinas virtuales de Azure | N/D | N/D
Máquinas virtuales de Azure o locales con DPM | ![Sí][green] | ![Sí][green]
Máquinas virtuales de Azure o locales con MABS | ![Sí][green] | ![Sí][green]


## <a name="next-steps"></a>Pasos siguientes

- [Copia de seguridad de máquinas virtuales de Azure](backup-azure-arm-vms-prepare.md).
- [Copia de seguridad directa de máquinas Windows](tutorial-backup-windows-server-to-azure.md), sin un servidor de copia de seguridad.
- [Configuración de MABS](backup-azure-microsoft-azure-backup.md) para la copia de seguridad en Azure y, luego, copia de seguridad de las cargas de trabajo en MABS.
- [Configuración de DPM](backup-azure-dpm-introduction.md) para la copia de seguridad en Azure y, luego, copia de seguridad de las cargas de trabajo en DPM.

[green]: ./media/backup-support-matrix/green.png
[yellow]: ./media/backup-support-matrix/yellow.png
[red]: ./media/backup-support-matrix/red.png
