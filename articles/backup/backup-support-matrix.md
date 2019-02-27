---
title: Matriz de compatibilidad de Azure Backup
description: Proporciona un resumen de opciones de compatibilidad y limitaciones para el servicio Azure Backup.
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 02/17/2019
ms.author: raynew
ms.openlocfilehash: ff4ee1d88bd13e647d0f6218d7e9c9b2c57a5a01
ms.sourcegitcommit: 9aa9552c4ae8635e97bdec78fccbb989b1587548
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/20/2019
ms.locfileid: "56429577"
---
# <a name="azure-backup-support-matrix"></a>Matriz de compatibilidad de Azure Backup

Puede usar el [servicio Azure Backup](backup-overview.md) para realizar una copia de seguridad de los datos en la nube de Microsoft Azure. En este artículo se resumen las configuraciones y limitaciones de compatibilidad generales para los escenarios e implementaciones de Azure Backup.

Otras matrices de compatibilidad están disponibles:

[Matriz de compatibilidad](backup-support-matrix-iaas.md) para copia de seguridad de máquina virtual de Azure, [Matriz de compatibilidad](backup-support-matrix-mabs-dpm.md) para copia de seguridad con System Center DPM o Microsoft Azure Backup Server (MABS), [Matriz de compatibilidad](backup-support-matrix-mars-agent.md) para copia de seguridad con el agente de Microsoft Azure Recovery Services (MARS)

## <a name="vault-support"></a>Compatibilidad con almacenes

Azure Backup utiliza los almacenes de Recovery Services para organizar y administrar las copias de seguridad, y para almacenar los datos con copia de seguridad realizada.

**Configuración** | **Detalles**
--- | ---
**Vaults in subscription** (Almacenes en la suscripción) | Hasta 500 almacenes de Recovery Services en una suscripción única.
**Machines in a vault** (Máquinas en un almacén) | Hasta 1000 máquinas virtuales de Azure en un único almacén.<br/><br/> Se pueden registrar hasta 50 servidores MABS en un único almacén.
**Data sources in vault storage** (Origen de datos en almacenamiento de almacén) | Máximo 54 400 GB. No hay límite para las copias de seguridad de las máquinas virtuales de Azure.
**Backups to vault** (Copias de seguridad en el almacén) | Máquinas virtuales de Azure: una vez al día<br/><br/>Máquinas protegidas por DPM/MABS: dos veces al día<br/><br/> Máquinas con copia de seguridad directa mediante el agente de MARS: tres veces al día. 
**Backups between vaults** (Copias de seguridad entre almacenes) | Las copias de seguridad se realizan dentro de una región.<br/><br/> Necesita un almacén en cada región de Azure que contenga máquinas virtuales de las que desee realizar copias de seguridad. No se pueden realizar copias de seguridad en una región diferente. 
**Move vault** (Movimiento del almacén) | También puede [mover almacenes](https://review.docs.microsoft.com/azure/backup/backup-azure-move-recovery-services-vault) entre suscripciones o entre grupos de recursos en la misma suscripción.
**Move data between vaults** (Movimiento de datos entre almacenes) | No se admite el traslado de datos con copia de seguridad realizada entre almacenes.
**Modify vault storage type** (Modificación del tipo de almacenamiento de almacén) | Puede modificar el tipo de replicación de almacenamiento (GRS/LRS) de un almacén antes de que se almacenen las copias de seguridad. Una vez iniciadas las copias de seguridad en el almacén, el tipo de replicación no se puede modificar.



## <a name="on-premises-backup-support"></a>Compatibilidad con las copias de seguridad locales

Esto es lo que se admite si quiere hacer copias de seguridad de equipos locales.

**Máquina** | **Copia de seguridad realizada** | **Ubicación** | **Características**
--- | --- | --- | ---
**Copia de seguridad directa de máquina Windows con el agente de MARS** | Archivos, carpetas, estado del sistema | Copia de seguridad en el almacén de Recovery Services | Copia de seguridad tres veces al día.<br/><br/> Copia de seguridad sin reconocimiento de aplicaciones.<br/><br/> Restauración de archivos, carpetas, volúmenes.
**Copia de seguridad directa de máquina Linux con el agente de MARS** | No se admite la copia de seguridad.
**Copia de seguridad en DPM** | Archivos, carpetas, volúmenes, estado del sistema, datos de la aplicación. | Copia de seguridad en almacenamiento local de DPM. DPM, después, realiza copias de seguridad en el almacén. | Instantáneas con reconocimiento de aplicaciones<br/><br/> Granularidad completa para la copia de seguridad y recuperación.<br/><br/> Linux admitido en máquinas virtuales (Hyper-V o VMware).<br/><br/>. Oracle no se admite.
**Copia de seguridad en MABS** | Archivos, carpetas, volúmenes, estado del sistema, datos de la aplicación. | Copia de seguridad en almacenamiento local de MABS. Después, MABS realiza una copia de seguridad en el almacén. | Instantáneas con reconocimiento de aplicaciones<br/><br/> Granularidad completa para la copia de seguridad y recuperación.<br/><br/> Linux admitido en máquinas virtuales (Hyper-V o VMware).<br/><br/>. Oracle no se admite.


## <a name="azure-vm-backup-support"></a>Compatibilidad de copia de seguridad de máquinas virtuales de Azure

### <a name="azure-vm-limits"></a>Límites de máquina virtual de Azure

**Límite** | **Detalles**
--- | ---
Discos de datos de máquinas virtuales de Azure | Límite de 16.
Tamaño del disco de datos de máquina virtual de Azure | El disco individual puede tener hasta 4095 GB.


### <a name="azure-vm-backup-options"></a>Opciones de copia de seguridad de máquinas virtuales de Azure

Esto es lo que se admite si desea hacer una copia de seguridad de las máquinas virtuales de Azure.

**Máquina** | **Copia de seguridad realizada** | **Ubicación** | **Características**
--- | --- | --- | ---
**Copia de seguridad de máquina virtual de Azure con la extensión de máquina virtual** | Toda la máquina virtual | Copia de seguridad en almacén | Extensión instalada cuando se habilita la copia de seguridad para una máquina virtual.<br/><br/> Copia de seguridad una vez al día.<br/><br/> Copias de seguridad con reconocimiento de aplicaciones para máquinas virtuales Windows, copias de seguridad coherentes con archivos para máquinas virtuales Linux. Puede configurar la coherencia de aplicaciones para máquinas Linux con scripts personalizados.<br/><br/> Restauración de máquina virtual o disco.<br/><br/> No se puede hacer una copia de seguridad de la máquina virtual de Azure en una ubicación local.
**Copia de seguridad de máquina virtual de Azure con el agente de MARS** | Archivos, carpetas | Copia de seguridad en almacén | Copia de seguridad tres veces al día.<br/><br/> Si desea realizar copias de seguridad de archivos o carpetas específicos en lugar de toda la máquina virtual, puede ejecutar el agente de MARS junto con la extensión de máquina virtual.
**Máquina virtual de Azure con DPM** | Archivos, carpetas, volúmenes, estado del sistema, datos de la aplicación. | Copia de seguridad en almacenamiento local de máquina virtual de Azure que se ejecuta en DPM. DPM, después, realiza copias de seguridad en el almacén. | Instantáneas con reconocimiento de aplicaciones<br/><br/> Granularidad completa para la copia de seguridad y recuperación.<br/><br/> Linux admitido en máquinas virtuales (Hyper-V o VMware).<br/><br/>. Oracle no se admite.
**Máquina virtual de Azure con MABS** | Archivos, carpetas, volúmenes, estado del sistema, datos de la aplicación. | Copia de seguridad en almacenamiento local de máquina virtual de Azure que se ejecuta en MABS. Después, MABS realiza una copia de seguridad en el almacén. | Instantáneas con reconocimiento de aplicaciones<br/><br/> Granularidad completa para la copia de seguridad y recuperación.<br/><br/> Linux admitido en máquinas virtuales (Hyper-V o VMware).<br/><br/> Oracle no se admite.





## <a name="linux-backup-support"></a>Soporte técnico de copia de seguridad de Linux

Esto es lo que se admite si quiere hacer copias de seguridad de máquinas Linux.

**Copia de seguridad** | **Linux (reconocido por Azure)**
--- | ---
**Copia de seguridad directa de máquina local que ejecuta Linux**. |  No. El agente MARS solo puede instalarse en máquinas Windows.
**Copia de seguridad de máquina virtual de Azure que ejecuta Linux (con la extensión del agente)** | Copia de seguridad coherente con la aplicación mediante [scripts personalizados](backup-azure-linux-app-consistent.md).<br/><br/> Recuperación de nivel de archivo.<br/><br/> Restauración mediante la creación de una máquina virtual desde un disco o un punto de recuperación.
**Copia de seguridad del entorno local o de máquina virtual de Azure que ejecuta Linux mediante DPM** | Copia de seguridad coherente con archivo de máquinas virtuales invitadas de Linux en Hyper-V y VMWare<br/><br/> Restauración de máquinas virtuales invitadas de Linux en Hyper-V y VMWare</br></br> Copia de seguridad coherente con archivo no disponible para las máquinas virtuales de Azure
**Copia de seguridad de máquina local/máquina virtual de Azure que ejecuta Linux con MABS** | Copia de seguridad coherente con archivo de máquinas virtuales invitadas de Linux en Hyper-V y VMWare<br/><br/> Restauración de máquinas virtuales invitadas de Linux en Hyper-V y VMWare</br></br> Copia de seguridad coherente con archivo no disponible para las máquinas virtuales de Azure.

## <a name="daylight-saving-support"></a>Compatibilidad con horario de verano

Azure Backup no admite el ajuste automático del reloj para los cambios de horario de verano para realizar copias de seguridad de máquinas virtuales de Azure. Modifique las directivas de copia de seguridad manualmente según sea necesario.


## <a name="disk-deduplication-support"></a>Compatibilidad con desduplicación de disco

La compatibilidad con la desduplicación de disco es la siguiente:
- Se admite la desduplicación de disco en el entorno local cuando utiliza DPM o MAB para hacer copias de seguridad de las máquinas virtuales de Hyper-V que ejecutan Windows. Windows Server realiza la desduplicación de los datos (en el nivel de host) en discos duros virtuales (VHD) que están conectados a la máquina virtual como almacenamiento de copia de seguridad.
- La desduplicación no se admite en Azure para ninguno de los componentes de Backup. Cuando System Center DPM y Backup Server se implementan en Azure, los discos de almacenamiento asociados a la máquina virtual no se pueden desduplicar.


## <a name="securityencryption-support"></a>Admite el cifrado o seguridad

Azure Backup admite el cifrado para datos en tránsito y en reposo:

Tráfico de red en Azure:
- El l tráfico de copia de seguridad de los servidores al almacén de Recovery Services se cifra mediante el Estándar de cifrado avanzado 256.
- Los datos de copia de seguridad se envían a través de un vínculo HTTPS seguro.
- Los datos de copia de seguridad se almacenan en el almacén de Recovery Services en su forma cifrada.
- Solo el usuario tiene la frase de contraseña para desbloquear estos datos. Microsoft no puede descifrar los datos de copia de seguridad en ningún momento.
    > [!WARNING]
    > Después de configurar el almacén, solo el usuario tiene acceso a la clave de cifrado. Microsoft nunca mantiene una copia y no tiene acceso a la clave. Si la clave se pierde, Microsoft no puede recuperar los datos de copia de seguridad.
Seguridad de los datos:
- Cuando se realiza la copia de seguridad de máquinas virtuales de Azure, se requiere la configuración del cifrado *en* la máquina virtual.
- Azure Backup admite Azure Disk Encryption, que usa BitLocker en máquinas virtuales Windows y **dm-crypt** en máquinas virtuales Linux.
- En el back-end, Azure Backup usa el [cifrado del servicio Azure Storage](../storage/common/storage-service-encryption.md), que protege los datos en reposo.


**Máquina** | **En tránsito** | **En reposo**
--- | --- | ---
Máquinas Windows locales sin DPM/MABS | ![Sí][green] | ![Sí][green]
Máquinas virtuales de Azure | ![Sí][green] | ![Sí][green]
Máquinas virtuales de Azure o locales con DPM | ![Sí][green] | ![Sí][green]
Máquinas virtuales de Azure o locales con MABS | ![Sí][green] | ![Sí][green]



## <a name="compression-support"></a>Compatibilidad de compresión

Backup admite la compresión del tráfico de copia de seguridad, tal y como se resume en la siguiente tabla. 

- Para las máquinas virtuales de Azure, la extensión de la máquina virtual lee directamente los datos de la cuenta de Azure Storage a través de la red de almacenamiento, no es necesario comprimir este tráfico.
- Si utiliza DPM o MABS, puede comprimir los datos antes de realizar la copia de seguridad en DPM/MABS, para ahorrar ancho de banda.

**Máquina** | **Comprimir a MABS/DPM (TCP)** | **Comprimir (HTTPS) al almacén**
--- | --- | ---
**Copia de seguridad directa de máquinas Windows locales** | N/D | Sí
**Copia de seguridad de máquinas virtuales de Azure mediante la extensión de máquina virtual** | N/D | N/D
** Copia de seguridad en el entorno local o en máquinas de Azure mediante DPM/MABS | ![Sí][green] | ![Sí][green]



## <a name="retention-limits"></a>Límites de retención

**Configuración** | **Límites**
--- | ---
Número máximo de puntos de recuperación por instancia protegida (máquina o carga de trabajo) | 9.999
Tiempo máximo de expiración de un punto de recuperación | Sin límite
Frecuencia máxima de copia de seguridad en DPM/MABS | Cada 15 minutos para SQL Server<br/><br/> Una vez cada hora para otras cargas de trabajo.
Frecuencia máxima de copia de seguridad en el almacén | Máquinas Windows/Máquinas virtuales de Azure locales que ejecutan MARS: tres al día<br/><br/> DPM/MABS: Dos por día<br/><br/> Copia de seguridad de la máquina virtual de Azure: Una vez al día
Retención de punto de recuperación | Diariamente, semanalmente, mensualmente y anualmente.
Período de retención máximo | Depende de la frecuencia de la copia de seguridad.
Puntos de recuperación en disco DPM/MABS | 64 para servidores de archivos, 448 para servidores de aplicaciones.<br/><br/> Los puntos de recuperación de cinta son ilimitados para DPM en el entorno local.

## <a name="next-steps"></a>Pasos siguientes

- [Revise la matriz de compatibilidad](backup-support-matrix-iaas.md) para la copia de seguridad de máquina virtual de Azure.


[green]: ./media/backup-support-matrix/green.png
[yellow]: ./media/backup-support-matrix/yellow.png
[red]: ./media/backup-support-matrix/red.png
