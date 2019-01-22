---
title: Matriz de compatibilidad de Azure Backup
description: Proporciona un resumen de opciones de compatibilidad y limitaciones para el servicio Azure Backup.
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: overview
ms.date: 01/09/2019
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: b1fa723863e6485e977e075986c3779efed1e689
ms.sourcegitcommit: a408b0e5551893e485fa78cd7aa91956197b5018
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/17/2019
ms.locfileid: "54360649"
---
# <a name="azure-backup-support-matrix"></a>Matriz de compatibilidad de Azure Backup

Puede usar el [servicio Azure Backup](backup-overview.md) realiza copias de seguridad de los datos en la nube de Microsoft Azure. En este artículo se resumen las configuraciones y limitaciones de compatibilidad para los escenarios e implementaciones de Azure Backup.

## <a name="vault-support"></a>Compatibilidad con almacenes

Azure Backup utiliza los almacenes de Recovery Services para organizar y administrar las copias de seguridad, y para almacenar los datos con copia de seguridad realizada.

**Configuración** | **Detalles**
--- | ---
Número de almacenes | Hasta 500 almacenes de Recovery Services en una suscripción única.
Máquinas en un almacén | Hasta 1000 máquinas virtuales de Azure en un único almacén.<br/><br/> Se pueden registrar hasta 50 máquinas locales que ejecuten el agente de Azure Backup (agente de Microsoft Azure Recovery Services [MABS]) en un único almacén.
Origen de datos en almacenamiento de almacén | Máximo 54 400 GB. No hay límite para las copias de seguridad de las máquinas virtuales de Azure.
Copias de seguridad en el almacén | Máquinas virtuales de Azure: una vez al día; máquinas con protección de DPM/MABS: dos veces al día; máquinas con copia de seguridad realizada directamente con el agente MARS: tres veces al día.  
Traslado del almacén | Puede mover los almacenes de Recovery Services de copia de seguridad entre suscripciones y grupos de recursos. [Más información](backup-azure-move-recovery-services-vault.md).
Movimiento de datos entre almacenes | No se admite el traslado de datos con copia de seguridad realizada entre almacenes.
Tipo de replicación de almacenamiento | Puede modificar el tipo de replicación de almacenamiento (GRS/LRS) de un almacén antes de que se almacenen las copias de seguridad. Una vez iniciadas las copias de seguridad en el almacén, el tipo de replicación no se puede modificar.



## <a name="on-premises-backup-support"></a>Compatibilidad con las copias de seguridad locales 

Esto es lo que se admite si quiere hacer copias de seguridad de equipos locales.

**Máquina** | **Ubicación** | **Copias de seguridad** | **Características**
--- | --- | --- | ---
**Física o virtual Windows (sin servidor de copia de seguridad)** | Archivos, carpetas, estado del sistema | Copia de seguridad realizada en el almacén de Recovery Services | Copia de seguridad tres veces al día.<br/><br/> Copia de seguridad sin reconocimiento de aplicaciones.<br/><br/> Restauración de archivos, carpetas, volúmenes.
**Física o virtual Linux (sin servidor de copia de seguridad)** | No se admite la copia de seguridad.
**Física o virtual con DPM** | Archivos, carpetas, volúmenes, estado del sistema, datos de la aplicación. | Copia de seguridad realizada en DPM (en un disco asociado localmente al servidor DPM o a una cinta).<br/><br/> DPM, después, realiza copias de seguridad en el almacén. | Instantáneas con reconocimiento de aplicaciones<br/><br/> Granularidad completa para la copia de seguridad y recuperación.<br/><br/> Linux admitido en máquinas virtuales (Hyper-V o VMware).<br/><br/>. Oracle no se admite.
**Física o virtual con MABS** | Archivos, carpetas, volúmenes, estado del sistema, datos de la aplicación. | Copia de seguridad realizada en MABS (en un disco asociado localmente al servidor MABS). La cinta no se admite<br/><br/> Después, MABS realiza una copia de seguridad en el almacén. | Instantáneas con reconocimiento de aplicaciones<br/><br/> Granularidad completa para la copia de seguridad y recuperación.<br/><br/> Linux admitido en máquinas virtuales (Hyper-V o VMware).<br/><br/>. Oracle no se admite.


## <a name="azure-vms"></a>Máquinas virtuales de Azure

### <a name="azure-vm-limits"></a>Límites de máquina virtual de Azure

**Límite** | **Detalles**
--- | ---
Discos de datos de máquinas virtuales de Azure | Límite de 16.
Tamaño del disco de datos de máquina virtual de Azure | El disco individual puede tener hasta 4095 GB.


### <a name="azure-vm-backup-options"></a>Opciones de copia de seguridad de máquinas virtuales de Azure

Esto es lo que se admite si desea hacer una copia de seguridad de las máquinas virtuales de Azure.

**Máquina** | **Ubicación** | **Copias de seguridad** | **Características**
--- | --- | --- | ---
**Máquinas virtuales de Azure (sin servidor de copia de seguridad)** | Archivos, carpetas, estado del sistema | Copia de seguridad realizada en el almacén. | Copia de seguridad una vez al día.<br/><br/> Copias de seguridad con reconocimiento de aplicaciones para máquinas virtuales Windows, copias de seguridad coherentes con archivos para máquinas virtuales Linux. Puede configurar la coherencia de aplicaciones para máquinas Linux con scripts personalizados.<br/><br/> Restauración de máquina virtual o disco.<br/><br/> No se puede hacer una copia de seguridad de la máquina virtual de Azure en una ubicación local.
**Máquina virtual de Azure con DPM** | Archivos, carpetas, volúmenes, estado del sistema, datos de la aplicación. | Copia de seguridad realizada en DPM que se ejecuta en Azure (en un disco asociado localmente al servidor DPM). La cinta no se admite.<br/><br/> DPM, después, realiza copias de seguridad en el almacén. | Instantáneas con reconocimiento de aplicaciones<br/><br/> Granularidad completa para la copia de seguridad y recuperación.<br/><br/> Linux admitido en máquinas virtuales (Hyper-V o VMware).<br/><br/>. Oracle no se admite.
**Máquina virtual de Azure con MABS** | Archivos, carpetas, volúmenes, estado del sistema, datos de la aplicación. | Copia de seguridad realizada en MABS que se ejecuta en Azure (en un disco asociado localmente al servidor MABS). La cinta no se admite<br/><br/> Después, MABS realiza una copia de seguridad en el almacén. | Instantáneas con reconocimiento de aplicaciones<br/><br/> Granularidad completa para la copia de seguridad y recuperación.<br/><br/> Linux admitido en máquinas virtuales (Hyper-V o VMware).<br/><br/>. Oracle no se admite.





## <a name="linux-backup-support"></a>Soporte técnico de copia de seguridad de Linux

Esto es lo que se admite si quiere hacer copias de seguridad de máquinas Linux.

**Copia de seguridad** | **Linux (reconocido por Azure)**
--- | --- 
**Máquina Linux local (sin DPM o MABS)**. |  No. El agente MARS solo puede instalarse en máquinas Windows. 
**Máquina virtual de Azure (sin DPM o MABS)** | Copia de seguridad coherente con la aplicación mediante [scripts personalizados](backup-azure-linux-app-consistent.md).<br/><br/> Recuperación de nivel de archivo.<br/><br/> Restauración mediante la creación de una máquina virtual desde un disco o un punto de recuperación.
**Equipo o máquina virtual locales de Azure con DPM** | Copia de seguridad coherente con archivo de máquinas virtuales invitadas de Linux en Hyper-V y VMWare<br/><br/> Restauración de máquinas virtuales invitadas de Linux en Hyper-V y VMWare</br></br> Copia de seguridad coherente con archivo no disponible para las máquinas virtuales de Azure
**Equipo o máquina virtual locales de Azure con MABS** | Copia de seguridad coherente con archivo de máquinas virtuales invitadas de Linux en Hyper-V y VMWare<br/><br/> Restauración de máquinas virtuales invitadas de Linux en Hyper-V y VMWare</br></br> Copia de seguridad coherente con archivo no disponible para las máquinas virtuales de Azure.

## <a name="disk-support"></a>Compatible con disco

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
Máquinas Windows locales sin DPM/MABS | ![SÍ][green] | ![SÍ][green]
Máquinas virtuales de Azure | ![SÍ][green] | ![SÍ][green] 
Máquinas virtuales de Azure o locales con DPM | ![SÍ][green] | ![SÍ][green]
Máquinas virtuales de Azure o locales con MABS | ![SÍ][green] | ![SÍ][green]



## <a name="compression-support"></a>Compatibilidad de compresión

Backup admite la compresión del tráfico de copia de seguridad, tal y como se resume en la siguiente tabla. Observe lo siguiente:

- Para las máquinas virtuales de Azure, la extensión de la máquina virtual lee directamente los datos de la cuenta de Azure Storage a través de la red de almacenamiento, no es necesario comprimir este tráfico.
- Si utiliza DPM o MABS, puede comprimir los datos antes de realizar la copia de seguridad en DPM/MABS, para ahorrar ancho de banda. 

**Máquina** | **Comprimir a MABS/DPM (TCP)** | **Comprimir (HTTPS) al almacén**
--- | --- | ---
Máquinas Windows locales sin DPM/MABS | N/D | SÍ
Máquinas virtuales de Azure | N/D | N/D
Máquinas virtuales de Azure o locales con DPM | ![SÍ][green] | ![SÍ][green]
Máquinas virtuales de Azure o locales con MABS | ![SÍ][green] | ![SÍ][green]



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

- [Copia de seguridad de máquinas virtuales de Azure](backup-azure-arm-vms-prepare.md)
- [Copia de seguridad de máquinas Windows directamente](tutorial-backup-windows-server-to-azure.md), sin un servidor de copia de seguridad.
- [Configuración de MABS](backup-azure-microsoft-azure-backup.md) para hacer copias de seguridad en Azure y, después, hacer copias de seguridad de las cargas de trabajo a MABS.
- [Configuración de DPM](backup-azure-dpm-introduction.md) para hacer copias de seguridad en Azure y, después, hacer copias de seguridad de las cargas de trabajo a DPM.

[green]: ./media/backup-support-matrix/green.png
[yellow]: ./media/backup-support-matrix/yellow.png
[red]: ./media/backup-support-matrix/red.png
