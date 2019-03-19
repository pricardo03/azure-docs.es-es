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
ms.openlocfilehash: 51bd4b935b32bea20d3f5de0b8cda62dfdbf07b8
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2019
ms.locfileid: "57898727"
---
# <a name="azure-backup-support-matrix"></a>Matriz de compatibilidad de Azure Backup

Puede usar [Azure Backup](backup-overview.md) para realizar una copia de seguridad de los datos a la plataforma de nube de Microsoft Azure. En este artículo se resume las opciones generales de soporte técnico y limitaciones para las implementaciones y los escenarios de copia de seguridad de Azure.

Otras matrices de compatibilidad están disponibles:

- Matriz de compatibilidad para [copia de seguridad de máquina virtual de Azure (VM)](backup-support-matrix-iaas.md)
- Matriz de compatibilidad para la copia de seguridad mediante el uso de [System Center Data Protection Manager (DPM) o Microsoft Azure Backup Server (MABS)](backup-support-matrix-mabs-dpm.md)
- Matriz de compatibilidad para la copia de seguridad mediante el uso de la [agente Microsoft Azure Recovery Services (MARS)](backup-support-matrix-mars-agent.md)

## <a name="vault-support"></a>Compatibilidad con almacenes

Azure Backup utiliza almacenes de Recovery Services para organizar y administrar las copias de seguridad. También utiliza almacenes para almacenar los datos de copia de seguridad. 

En la tabla siguiente se describe las características de los almacenes de Recovery Services:

**Característica** | **Detalles**
--- | ---
**Vaults in subscription** (Almacenes en la suscripción) | Hasta 500 almacenes de Recovery Services en una suscripción única.
**Machines in a vault** (Máquinas en un almacén) | Hasta 1000 máquinas virtuales de Azure en un único almacén.<br/><br/> Se pueden registrar hasta 50 servidores MABS en un único almacén.
**Data sources in vault storage** (Origen de datos en almacenamiento de almacén) | 54 400 GB como máximo. No hay límite para las copias de seguridad de las máquinas virtuales de Azure.
**Backups to vault** (Copias de seguridad en el almacén) | **Máquinas virtuales de Azure:** Una vez al día.<br/><br/>**Máquinas protegidas por DPM/MABS:** Dos veces al día.<br/><br/> **Copia de seguridad máquinas directamente mediante el agente de MARS:** Tres veces al día. 
**Backups between vaults** (Copias de seguridad entre almacenes) | Las copias de seguridad se realizan dentro de una región.<br/><br/> Necesita un almacén en cada región de Azure que contenga máquinas virtuales de las que desee realizar copias de seguridad. No se pueden realizar copias de seguridad en una región diferente. 
**Migrar los almacenes** | También puede [migrar los almacenes](https://review.docs.microsoft.com/azure/backup/backup-azure-move-recovery-services-vault) entre suscripciones o entre grupos de recursos en la misma suscripción.
**Move data between vaults** (Movimiento de datos entre almacenes) | No se admite el movimiento de datos de copia de seguridad entre los almacenes.
**Modify vault storage type** (Modificación del tipo de almacenamiento de almacén) | Puede modificar el tipo de replicación de almacenamiento (almacenamiento con redundancia geográfica o almacenamiento con redundancia local) para un depósito antes de que se almacenan las copias de seguridad. Una vez iniciadas las copias de seguridad en el almacén, el tipo de replicación no se puede modificar.

## <a name="on-premises-backup-support"></a>Compatibilidad con las copias de seguridad locales

Aquí es lo que se admite si desea realizar una copia de seguridad de máquinas locales:

**Máquina** | **¿Qué es una copia de seguridad** | **Ubicación** | **Características**
--- | --- | --- | ---
**Copia de seguridad directa de máquina Windows con el agente de MARS** | Archivos, carpetas, estado del sistema | Copia de seguridad en el almacén de Recovery Services. | Realizar copias de seguridad tres veces al día<br/><br/> Ninguna copia de seguridad basadas en la aplicación<br/><br/> Restaurar el archivo, carpeta, volumen
**Copia de seguridad directa de máquina Linux con el agente de MARS** | No admitidos copias de seguridad
**Copia de seguridad en DPM** | Archivos, carpetas, volúmenes, estado del sistema, datos de la aplicación | Copia de seguridad en almacenamiento local de DPM. DPM, después, realiza copias de seguridad en el almacén. | Instantáneas con reconocimiento de aplicaciones<br/><br/> Granularidad completa para la copia de seguridad y recuperación<br/><br/> Linux compatible con VM (Hyper-V o VMware)<br/><br/> Oracle no admitida
**Copia de seguridad en MABS** | Archivos, carpetas, volúmenes, estado del sistema, datos de la aplicación | Copia de seguridad en almacenamiento local de MABS. Después, MABS realiza una copia de seguridad en el almacén. | Instantáneas con reconocimiento de aplicaciones<br/><br/> Granularidad completa para la copia de seguridad y recuperación<br/><br/> Linux compatible con VM (Hyper-V o VMware)<br/><br/> Oracle no admitida

## <a name="azure-vm-backup-support"></a>Compatibilidad de copia de seguridad de máquinas virtuales de Azure

### <a name="azure-vm-limits"></a>Límites de máquina virtual de Azure

**Límite** | **Detalles**
--- | ---
**Discos de datos de máquina virtual de Azure** | Límite de 16
**Tamaño de disco de datos de Azure VM** | Los discos individuales pueden tener hasta 4095 GB

### <a name="azure-vm-backup-options"></a>Opciones de copia de seguridad de máquinas virtuales de Azure

Aquí es lo que se admite si desea realizar una copia de seguridad de máquinas virtuales de Azure:

**Máquina** | **¿Qué es una copia de seguridad** | **Ubicación** | **Características**
--- | --- | --- | ---
**Azure backup para máquinas virtuales mediante el uso de la extensión de máquina virtual** | Toda la máquina virtual | Copia de seguridad en el almacén. | Extensión instalada cuando se habilita la copia de seguridad para una máquina virtual.<br/><br/> Copia de seguridad una vez al día.<br/><br/> Copia de seguridad basadas en la aplicación para máquinas virtuales de Windows; copia de seguridad coherentes con el archivo para máquinas virtuales Linux. Puede configurar la coherencia de la aplicación para las máquinas de Linux mediante el uso de scripts personalizados.<br/><br/> Restaurar la máquina virtual o disco.<br/><br/> No se puede realizar copias de seguridad una máquina virtual de Azure en una ubicación local.
**Azure backup para máquinas virtuales mediante el agente de MARS** | Archivos, carpetas | Copia de seguridad en el almacén. | Copia de seguridad tres veces al día.<br/><br/> Si desea realizar copias de seguridad de determinados archivos o carpetas en lugar de toda la máquina virtual, puede ejecutar el agente de MARS junto con la extensión de máquina virtual.
**Máquina virtual de Azure con DPM** | Archivos, carpetas, volúmenes, estado del sistema, datos de la aplicación | Copia de seguridad en almacenamiento local de la máquina virtual de Azure que ejecuta DPM. DPM, después, realiza copias de seguridad en el almacén. | Instantáneas basadas en la aplicación.<br/><br/> Granularidad completa para la copia de seguridad y recuperación.<br/><br/> Linux admitido en máquinas virtuales (Hyper-V o VMware).<br/><br/> Oracle no se admite.
**Máquina virtual de Azure con MABS** | Archivos, carpetas, volúmenes, estado del sistema, datos de la aplicación | Copia de seguridad en almacenamiento local de la máquina virtual de Azure que se está ejecutando MABS. Después, MABS realiza una copia de seguridad en el almacén. | Instantáneas basadas en la aplicación.<br/><br/> Granularidad completa para la copia de seguridad y recuperación.<br/><br/> Linux admitido en máquinas virtuales (Hyper-V o VMware).<br/><br/> Oracle no se admite.

## <a name="linux-backup-support"></a>Soporte técnico de copia de seguridad de Linux

Aquí es lo que se admite si desea realizar una copia de seguridad de máquinas de Linux:

**Tipo de copia de seguridad** | **Linux (reconocido por Azure)**
--- | ---
**Copia de seguridad directa de la máquina local que ejecuta Linux** | No compatible. El agente de MARS puede instalarse solo en equipos de Windows.
**Uso de extensión del agente para realizar una copia de seguridad de máquina virtual de Azure que ejecuta Linux** | Copia de seguridad coherente con la aplicación mediante el uso de [scripts personalizados](backup-azure-linux-app-consistent.md).<br/><br/> Recuperación de nivel de archivo.<br/><br/> Restauración mediante la creación de una máquina virtual desde un disco o un punto de recuperación.
**Uso de DPM para realizar copias de seguridad en el entorno local o máquina virtual de Azure que ejecuta Linux** | Copia de seguridad coherentes con el archivo de las máquinas virtuales invitadas de Linux en Hyper-V y VMWare.<br/><br/> Restauración de máquinas virtuales de Hyper-V y máquinas virtuales invitadas de Linux de VMWare.<br/><br/> Coherente con archivo de copia de seguridad no está disponible para la máquina virtual de Azure.
**Uso de MABS para realizar copias de seguridad en una máquina local o máquina virtual de Azure que ejecuta Linux** | Copia de seguridad coherentes con el archivo de las máquinas virtuales invitadas de Linux en Hyper-V y VMWare.<br/><br/> Restauración de máquinas virtuales de Hyper-V y las máquinas virtuales de VMWare Linux invitadas.<br/><br/> Copia de seguridad coherente con archivo no disponible para las máquinas virtuales de Azure.

## <a name="daylight-saving-time-support"></a>Soporte técnico del horario de verano

Copia de seguridad de Azure no admite el ajuste automático del reloj del horario de verano para copias de seguridad de máquina virtual de Azure. Modifique las directivas de copia de seguridad manualmente según sea necesario.

## <a name="disk-deduplication-support"></a>Compatibilidad con desduplicación de disco

La compatibilidad con la desduplicación de disco es la siguiente:

- Desduplicación de disco es local compatible cuando se usa DPM o MABs para realizar una copia de seguridad de máquinas virtuales de Hyper-V que ejecutan Windows. Windows Server realiza la desduplicación de datos (en el nivel de host) en discos duros virtuales (VHD) que están conectados a la máquina virtual como almacenamiento de copia de seguridad.
- La desduplicación no se admite en Azure para ninguno de los componentes de Backup. Cuando DPM y MABS se implementan en Azure, no se pueden desduplicar los discos de almacenamiento conectados a la máquina virtual.

## <a name="security-and-encryption-support"></a>Compatibilidad con seguridad y cifrado

Copia de seguridad de Azure admite el cifrado de datos en tránsito y en reposo.

### <a name="network-traffic-to-azure"></a>Tráfico de red en Azure

- Tráfico de copia de seguridad de servidores en el almacén de Recovery Services se cifra mediante el uso de cifrado estándar avanzado 256.
- Los datos de copia de seguridad se envían a través de un vínculo HTTPS seguro.
- Datos de copia de seguridad se almacenan en el almacén de Recovery Services en un formato cifrado.
- Solo el usuario tiene la frase de contraseña para desbloquear estos datos. Microsoft no puede descifrar los datos de copia de seguridad en ningún momento.

    > [!WARNING]
    > Después de configurar el almacén, solo el usuario tiene acceso a la clave de cifrado. Microsoft nunca mantiene una copia y no tiene acceso a la clave. Si la clave se pierde, Microsoft no puede recuperar los datos de copia de seguridad.

### <a name="data-security"></a>Seguridad de los datos

- Cuando se hace copia de seguridad máquinas virtuales de Azure, deberá configurar el cifrado *dentro de* la máquina virtual.
- Azure Backup admite Azure Disk Encryption, que usa BitLocker en máquinas virtuales Windows y **dm-crypt** en máquinas virtuales Linux.
- En el back-end, Azure Backup usa [Azure Storage Service Encryption](../storage/common/storage-service-encryption.md), que protege los datos en reposo.

**Máquina** | **En tránsito** | **En reposo**
--- | --- | ---
**Máquinas de Windows locales sin DPM/MABS** | ![Sí][green] | ![Sí][green]
**Máquinas virtuales de Azure** | ![Sí][green] | ![Sí][green]
**Máquinas de Windows locales o máquinas virtuales de Azure con DPM** | ![Sí][green] | ![Sí][green]
**Máquinas de Windows locales o máquinas virtuales de Azure con MABS** | ![Sí][green] | ![Sí][green]

## <a name="compression-support"></a>Compatibilidad de compresión

Copia de seguridad admite la compresión del tráfico de copia de seguridad, como se resume en la tabla siguiente.

- Las máquinas virtuales de Azure, la extensión de VM lee los datos directamente desde la cuenta de almacenamiento de Azure a través de la red de almacenamiento, por lo que no es necesario comprimir este tráfico.
- Si usa DPM o MABS, puede ahorrar ancho de banda mediante la compresión de los datos antes de es una copia de seguridad.

**Máquina** | **Comprimir a MABS/DPM (TCP)** | **Comprimir en el almacén (HTTPS)**
--- | --- | ---
**Copia de seguridad directa de máquinas Windows locales** | N/D | ![Sí][green]
**Copia de seguridad de máquinas virtuales de Azure mediante el uso de la extensión de máquina virtual** | N/D | N/D
**Copia de seguridad en los equipos local y Azure con DPM/MABS** | ![Sí][green] | ![Sí][green]

## <a name="retention-limits"></a>Límites de retención

**Configuración** | **Límites**
--- | ---
**Número máximo de puntos de recuperación por instancia protegida (equipo o carga de trabajo)** | 9,999
**Tiempo máximo de expiración de un punto de recuperación** | Sin límite
**Frecuencia de copia de seguridad máxima para DPM/MABS** | Cada 15 minutos para SQL Server<br/><br/> Una vez por hora para otras cargas de trabajo
**Frecuencia máxima de copia de seguridad al almacén** | **En el entorno local máquinas de Windows o máquinas virtuales de Azure ejecutando MARS:** Tres por día<br/><br/> **DPM/MABS:** Dos por día<br/><br/> **Azure backup para máquinas virtuales:** Una por día
**Retención de punto de recuperación** | Diariamente, semanalmente, mensualmente y anualmente
**Período de retención máximo** | Depende de la frecuencia de la copia de seguridad
**Puntos de recuperación en disco DPM/MABS** | 64 para servidores de archivos. 448 para los servidores de aplicaciones <br/><br/>Puntos de recuperación de cinta ilimitado para DPM en el entorno local

## <a name="next-steps"></a>Pasos siguientes

- [Revise la matriz de compatibilidad](backup-support-matrix-iaas.md) para la copia de seguridad de máquina virtual de Azure.

[green]: ./media/backup-support-matrix/green.png
[yellow]: ./media/backup-support-matrix/yellow.png
[red]: ./media/backup-support-matrix/red.png
