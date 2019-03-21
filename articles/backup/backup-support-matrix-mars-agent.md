---
title: Matriz de compatibilidad para la copia de seguridad de máquinas que ejecutan el agente de Microsoft Azure Recovery Services (MARS) con Azure Backup
description: Este artículo resume la compatibilidad con Azure Backup al realizar copias de seguridad de máquinas que se ejecutan al agente de Microsoft Azure Recovery Services (MARS).
services: backup
author: rayne-wiselman
ms.service: backup
ms.date: 02/17/2019
ms.topic: conceptual
ms.author: raynew
manager: carmonm
ms.openlocfilehash: 3e2c6a550a9358656fd0870c7e785d131c5b6380
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/18/2019
ms.locfileid: "57894400"
---
# <a name="support-matrix-for-backup-with-the-microsoft-azure-recovery-services-mars-agent"></a>Matriz de compatibilidad para la copia de seguridad con el agente de Microsoft Azure Recovery Services (MARS)

Puede usar el [servicio Azure Backup](backup-overview.md) para realizar una copia de seguridad de máquinas locales y las aplicaciones y realizar copias de seguridad de máquinas virtuales (VM). Este artículo resumen las opciones de soporte técnico y limitaciones cuando se usa el agente de Microsoft Azure Recovery Services (MARS) para realizar una copia de seguridad de máquinas.

## <a name="the-mars-agent"></a>El agente de MARS

Azure Backup utiliza al agente de MARS para copias de seguridad de máquinas locales y máquinas virtuales de Azure en un almacén de Recovery Services copia de seguridad de Azure. El agente de MARS hacer lo siguiente:
- Ejecutar en equipos de Windows local para que puede realizar copias de seguridad directamente en un almacén de Recovery Services copia de seguridad en Azure.
- Ejecutar en máquinas virtuales de Windows para que puede realizar copias de seguridad directamente en un almacén.
- Ejecutar en Microsoft Azure Backup Server (MABS) o un servidor de System Center Data Protection Manager (DPM). En este escenario, las máquinas y cargas de trabajo de copia de seguridad a MABS o en el servidor DPM. El agente de MARS, a continuación, realiza copias de seguridad en este servidor en un almacén de Azure. 

Dependen de las opciones de copia de seguridad donde está instalado el agente. Para obtener más información, consulte [arquitectura de Azure Backup mediante el agente de MARS](backup-architecture.md#architecture-direct-backup-of-on-premises-windows-server-machines-or-azure-vm-files-or-folders). Para obtener información sobre la arquitectura de copia de seguridad de MABS y DPM, consulte [copia de seguridad en DPM o MABS](backup-architecture.md#architecture-back-up-to-dpmmabs). Consulte también [requisitos](backup-support-matrix-mabs-dpm.md) para la arquitectura de copia de seguridad.

**Instalación** | **Detalles**
--- | ---
Descargue al último agente de MARS | Puede descargar la última versión del agente desde el almacén o [directamente](https://aka.ms/azurebackup_agent).
Instalar directamente en un equipo | Puede instalar el agente de MARS directamente en un servidor de Windows local o en una máquina virtual de Windows que se está ejecutando cualquiera de los [sistemas operativos compatibles con](https://docs.microsoft.com/en-us/azure/backup/backup-support-matrix-mabs-dpm#supported-mabs-and-dpm-operating-systems).
Instalar en un servidor de copia de seguridad | Al configurar DPM o MABS para realizar una copia de seguridad en Azure, descargue e instale al agente de MARS en el servidor. Puede instalar el agente en [sistemas operativos compatibles](backup-support-matrix-mabs-dpm.md#supported-mabs-and-dpm-operating-systems) en la matriz de compatibilidad de servidor de copia de seguridad.

> [!NOTE]
> De forma predeterminada, las máquinas virtuales de Azure que están habilitadas para la copia de seguridad tiene una instalación de la extensión de copia de seguridad de Azure. Esta extensión realiza una copia de seguridad de toda la máquina virtual. Puede instalar y ejecutar al agente de MARS en una máquina virtual de Azure junto con la extensión si desea realizar copias de seguridad de carpetas y archivos específicos y no de la máquina virtual completa.
> Cuando se ejecuta el agente de MARS en una máquina virtual de Azure, copia los archivos o carpetas que se encuentran en el almacenamiento temporal en la máquina virtual. Las copias de seguridad producirá un error si se quitan los archivos o carpetas de almacenamiento temporal o si se quita el almacenamiento temporal.


## <a name="cache-folder-support"></a>Compatibilidad con la carpeta de caché

Cuando se usa el agente de MARS para realizar una copia de seguridad de los datos, el agente toma una instantánea de los datos y lo almacena en una carpeta de caché local antes de enviar los datos en Azure. La carpeta de caché (cero) tiene varios requisitos:

**Memoria caché** | **Detalles**
--- | ---
Tamaño |  Espacio libre en la carpeta de caché debe ser al menos del 5 al 10 por ciento del tamaño global de los datos de copia de seguridad. 
Ubicación | La carpeta de caché debe almacenarse localmente en el equipo que la copia de seguridad y deben estar en línea. No debería ser la carpeta de caché en un recurso compartido de red, en un medio extraíble o en un volumen sin conexión. 
Carpeta | La carpeta de caché debe cifrarse en un volumen desduplicado o en una carpeta que se comprime, que es dispersa o que tiene una repetición de análisis de punto.
Cambios de ubicación | Puede cambiar la ubicación de caché, detenga el motor de copia de seguridad (`net stop bengine`) y copiar la carpeta de caché en una nueva unidad. (Asegúrese de que la nueva unidad tiene espacio suficiente). A continuación, actualice dos entradas del registro bajo **HKLM\SOFTWARE\Microsoft\Windows Azure Backup** (**Config/ScratchLocation** y **CloudBackupProvider/Config/ScratchLocation**) a la nueva ubicación y reinicie el motor.

## <a name="networking-and-access-support"></a>Compatibilidad con redes y acceso

### <a name="url-access"></a>Acceso a direcciones URL

El agente de MARS necesita acceder a estas direcciones URL:

- http://www.msftncsi.com/ncsi.txt
- *.Microsoft.com
- *.WindowsAzure.com
- *.MicrosoftOnline.com
- *.Windows.net

### <a name="throttling-support"></a>Limitaciones de compatibilidad

**Característica** | **Detalles**
--- | ---
Control del ancho de banda |  Se admite. En el agente de MARS, utilice **cambiar las propiedades** para ajustar el ancho de banda.
Limitación de la red | No está disponible para las máquinas de copia de seguridad que ejecuten Windows Server 2008 R2, Windows Server 2008 SP2 o Windows 7.

## <a name="support-for-direct-backups"></a>Compatibilidad con copias de seguridad directas

Puede utilizar al agente de MARS para realizar copias de seguridad directamente en Azure en algunos sistemas operativos que se ejecutan en máquinas locales y máquinas virtuales de Azure. Los sistemas operativos deben ser de 64 bits y debe ser la ejecución de los módulos más recientes de servicios y las actualizaciones. En la tabla siguiente se resume estos sistemas operativos:

**Sistema operativo** | **Archivos/carpetas** | **Estado del sistema**
--- | --- | ---
Windows 10 (Enterprise, Pro, Home) | Sí | Sin 
Windows 8.1 (Enterprise, Pro)| Sí |Sin 
Windows 8 (Enterprise, Pro) | Sí | Sin 
Windows 7 (Ultimate, Enterprise, Pro, Home Premium/Basic, Starter) | Sí | Sin 
Windows Server 2016 (Standard, Datacenter, Essentials) | Sí | Sí
Windows Server 2012 R2 (Standard, Datacenter, Foundation, Essentials) | Sí | Sí
Windows Server 2012 (Standard, Datacenter, Foundation) | Sí | Sí
Windows Server 2008 R2 (Standard, Enterprise, Datacenter, Foundation) | Sí | Sí
Windows Server 2008 SP2 (Standard, Datacenter, Foundation) | Sí | Sin 
Windows Storage Server 2016/2012 R2/2012 (Standard, Workgroup) | Sí | Sin 

Para obtener más información, consulte [los sistemas operativos admiten MABS y DPM](backup-support-matrix-mabs-dpm.md#supported-mabs-and-dpm-operating-systems).

## <a name="backup-limits"></a>Límites de Backup

Azure Backup limita el tamaño de un origen de datos de archivo o carpeta que puede ser una copia de seguridad. Los elementos de copias de seguridad de un único volumen no pueden superar los tamaños que se resume en esta tabla:

**Sistema operativo** | **Límite de tamaño**
--- | ---
Windows Server 2012 o superior |  54 400 GB
Windows Server 2008 R2 SP1 |    1700 GB
Windows Server 2008 SP2 | 1700 GB
Windows 8 o posterior  | 54 400 GB
Windows 7   | 1700 GB


## <a name="supported-file-types-for-backup"></a>Tipos de archivo compatibles para copia de seguridad

**Tipo** | **Soporte técnico** 
--- | --- 
Cifrados   |  Se admite. 
Comprimidos |  Se admite.
Dispersos |  Se admite.
Comprimidos y dispersos |  Se admite.
Vínculos físicos  | No compatible. Omitido.
Punto de repetición de análisis   | No compatible. Omitido.
Cifrados y dispersos |  No compatible. Omitido.
Flujo comprimido   | No compatible. Omitido.
Flujo disperso   | No compatible. Omitido.
OneDrive (archivos sincronizados están dispersas secuencias)  | No compatible. 

## <a name="supported-drives-or-volumes-for-backup"></a>Las unidades compatibles o volúmenes de copia de seguridad

**Unidad/volumen** | **Soporte técnico** | **Detalles**
--- | --- | ---
Volúmenes de solo lectura   | No compatible | Servicio de instantáneas de copia de volumen (VSS) sólo funciona si el volumen es grabable.
Volúmenes sin conexión | No compatible |   VSS sólo funciona si el volumen está en línea.
Recurso compartido de red   | No compatible |   El volumen debe ser local en el servidor.
Volúmenes protegidos por BitLocker | No compatible |   El volumen debe desbloquearse antes de que comience la copia de seguridad.
Identificación del sistema de archivos  | No compatible |   Solo se admite NTFS.
Medios extraíbles | No compatible |   Todos los orígenes de elementos de copia de seguridad deben tener un *fijo* estado.
Unidades desduplicadas | Compatible | Azure Backup convierte los datos desduplicados en datos normales. Optimiza, cifra, almacena y envía los datos en el almacén.

## <a name="support-for-initial-offline-backup"></a>Compatibilidad con copia de seguridad inicial sin conexión

Azure Backup admite *propagación sin conexión* para transferir datos de copia de seguridad iniciales a Azure mediante el uso de discos. Esta compatibilidad es útil si la copia de seguridad inicial suele estar en el intervalo de tamaño de terabytes (TB). La copia de seguridad sin conexión es compatible para:

- Copia de seguridad directa de archivos y carpetas en las máquinas locales que se ejecutan al agente de MARS.
- Copia de seguridad de cargas de trabajo y archivos desde un servidor DPM o MABS.

Copia de seguridad sin conexión no se puede usar para archivos de estado del sistema.

## <a name="support-for-data-restoration"></a>Compatibilidad con restauración de datos

Mediante el uso de la [la restauración instantánea](backup-instant-restore-capability.md) característica de copia de seguridad de Azure, puede restaurar los datos antes de copiarlos en el almacén. La máquina que se hace copia de seguridad debe ejecutar .NET Framework 4.5.2 o superior.

No se puede restaurar las copias de seguridad en un equipo de destino que se está ejecutando una versión anterior del sistema operativo. Por ejemplo, se puede restaurar una copia de seguridad realizada desde un equipo que ejecuta Windows 7 en Windows 8 o posterior. Pero no se puede restaurar una copia de seguridad realizada desde un equipo que ejecute Windows 8 en un equipo que ejecuta Windows 7.

## <a name="next-steps"></a>Pasos siguientes
- Obtenga más información sobre [copia de seguridad de arquitectura que utiliza el agente de MARS](backup-architecture.md#architecture-direct-backup-of-on-premises-windows-server-machines-or-azure-vm-files-or-folders).
- Obtenga información sobre lo que admite cuando se [ejecutar el agente de MARS en el servidor DPM o MABS](backup-support-matrix-mabs-dpm.md).
