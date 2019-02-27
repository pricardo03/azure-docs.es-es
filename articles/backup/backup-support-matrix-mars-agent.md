---
title: Matriz de compatibilidad para la copia de seguridad de máquinas que ejecutan el agente de Microsoft Azure Recovery Services (MARS) con Azure Backup
description: En este artículo se resume la compatibilidad con Azure Backup al realizar copias de seguridad de máquinas que ejecutan el agente de Microsoft Azure Recovery Services (MARS).
services: backup
author: rayne-wiselman
ms.service: backup
ms.date: 02/17/2019
ms.topic: conceptual
ms.author: raynew
manager: carmonm
ms.openlocfilehash: 9d0f751e8d0bc0275cc5fd2c47aaba7a6058931c
ms.sourcegitcommit: 9aa9552c4ae8635e97bdec78fccbb989b1587548
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/20/2019
ms.locfileid: "56430939"
---
# <a name="support-matrix-for-backup-with-the-microsoft-azure-recovery-services-mars-agent"></a>Matriz de compatibilidad para la copia de seguridad con el agente de Microsoft Azure Recovery Services (MARS)

Puede usar el [servicio Azure Backup](backup-overview.md) para realizar copias de seguridad de máquinas y aplicaciones locales y de las máquinas virtuales de Azure. En este artículo se resumen las configuraciones de compatibilidad y las limitaciones para realizar copias de seguridad de máquinas con el agente de Microsoft Azure Recovery Services (MARS).

## <a name="about-the-mars-agent"></a>Acerca del agente de MARS

Azure Backup utiliza el agente de MARS para realizar copias de seguridad de los datos de máquinas locales y máquinas virtuales de Azure en un almacén de Recovery Services de copia de seguridad en Azure. El agente de MARS se puede utilizar como sigue:
- Ejecute el agente en máquinas Windows locales, para que puedan realizar la copia de seguridad directamente en un almacén de Recovery Services de copia de seguridad en Azure.
- Ejecute el agente en máquinas virtuales de Azure con Windows para que pueda realizar copias de seguridad directamente en un almacén.
- Ejecute el agente en un servidor de Microsoft Azure Backup Server (MABS) o en un servidor de System Center Data Protection Manager (DPM). En este escenario, las copias de seguridad de las máquinas y de las cargas de trabajo se realizan en MABS/DPM y, después, la copia de seguridad de MABS/DPM se realiza en un almacén de Azure con el agente de MARS. 

Aquello de lo que puede realizar una copia de seguridad depende de dónde está instalado el agente.

- [Obtenga más información](backup-architecture.md#architecture-direct-backup-of-on-premises-windows-machinesazure-vm-filesfolders) sobre la arquitectura de copia de seguridad con el agente de MARS.
- [Obtenga más información]() sobre la [arquitectura de copia de seguridad](backup-architecture.md#architecture-back-up-to-dpmmabs) y los [requisitos](backup-support-matrix-mabs-dpm.md) de MABS/DPM.


## <a name="supported-installation"></a>Instalación compatible

**Instalación** | **Detalles**
--- | ---
**Descargar la última versión del agente de MARS** | Puede descargar la última versión del agente desde el almacén o [directamente](https://aka.ms/azurebackup_agent).
**Instalar directamente en una máquina** | Puede instalar el agente de MARS directamente en un servidor Windows local o en una máquina virtual de Azure con Windows que ejecute cualquiera de los [sistemas operativos compatibles]().
**Instalar en un servidor de copia de seguridad** | Al configurar DPM o MABS para realizar una copia de seguridad en Azure, descargue e instale al agente de MARS en el servidor. El agente se puede instalar según los [sistemas operativos compatibles](backup-support-matrix-mabs-dpm.md#supported-mabs-and-dpm-operating-systems) en la matriz de compatibilidad del servidor de copia de seguridad.

> [!NOTE]
> De forma predeterminada, las máquinas virtuales de Azure habilitadas para copia de seguridad tienen instalada una extensión de Azure Backup. Esta extensión realiza una copia de seguridad de toda la máquina virtual. Puede instalar y ejecutar al agente de MARS en una máquina virtual de Azure junto con la extensión si desea realizar copias de seguridad de carpetas y archivos específicos y no de la máquina virtual completa.
> Al ejecutar el agente de MARS en una máquina virtual de Azure, realiza copias de seguridad de archivos y carpetas ubicados en el almacenamiento temporal en la máquina virtual. Se producirá un error de la copia de seguridad si los archivos o las carpetas se eliminan del almacenamiento temporal, o bien en caso de que se elimine dicho almacenamiento temporal.


## <a name="cache-folder-support"></a>Compatibilidad con la carpeta de caché

Al realizar una copia de seguridad con el agente de MARS, este realiza una instantánea de los datos y los almacena en una carpeta de caché local antes de enviarlos a Azure. La carpeta de caché (vacía) tiene una serie de requisitos.

**Memoria caché** | **Detalles**
--- | ---
**Tamaño de la caché** |  El espacio libre del tamaño de la carpeta de caché debe ser de un 5-10 % como mínimo del tamaño total de los datos de copia de seguridad. 
**Ubicación en caché** | La carpeta de caché debe ser local en la máquina de copia de seguridad y debe estar en línea.<br/><br/> La carpeta de caché no debe encontrarse en un recurso compartido de red, en un soporte físico extraíble o en un volumen sin conexión. 
**Carpeta de caché** | La carpeta de caché debe estar cifrada, en un volumen desduplicado o en una carpeta comprimida, dispersa o de punto de reanálisis.
**Modificar ubicación de la caché** | Puede cambiar la ubicación de la caché al detener el motor de copia de seguridad (net stop bengine) y copiar la carpeta de caché en una nueva unidad, pero debe asegurarse de que esta tiene espacio suficiente. A continuación, actualice dos entradas del Registro en HKLM\SOFTWARE\Microsoft\Windows Azure Backup (Config/ScratchLocation y Config/CloudBackupProvider/ScratchLocation) en la nueva ubicación y reinicie el motor.

## <a name="networking-and-access-support"></a>Compatibilidad con redes y acceso

### <a name="url-access"></a>Acceso a direcciones URL

El agente de MARS necesita acceder a estas direcciones URL:

- http://www.msftncsi.com/ncsi.txt
- *.Microsoft.com
- *.WindowsAzure.com
- *.microsoftonline.com
- * .windows.net

### <a name="throttling-support"></a>Limitaciones de compatibilidad

**Característica** | **Detalles**
--- | ---
Control del ancho de banda | Compatible<br/><br/> Use **Cambiar propiedades** en el agente de MARS para ajustar el ancho de banda.
Limitación de la red | No está disponible para las máquinas de copia de seguridad que ejecutan Windows Server 2008 R2, Windows Server 2008 SP2 o Windows 7.

## <a name="support-for-direct-backups"></a>Compatibilidad con copias de seguridad directas

En la tabla siguiente se resume de qué sistemas operativos que se ejecutan en máquinas locales y en máquinas virtuales de Azure se puede realizar una copia de seguridad directamente en Azure con el agente de MARS.

- Todos los sistemas operativos son de 64 bits.
- Todos los sistemas operativos deben ejecutar los últimos Service Pack y actualizaciones.
- Para obtener información detallada sobre los servidores de DPM y de MABS de los que se puede realizar una copia de seguridad con el agente de MARS, revise [esta tabla](backup-support-matrix-mabs-dpm.md#supported-mabs-and-dpm-operating-systems).

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


## <a name="backup-limits"></a>Límites de Backup

Azure Backup establece un límite de tamaño de un origen de datos de archivo/carpeta del que se puede realizar una copia de seguridad. El tamaño de los elementos seleccionados para una copia de seguridad desde un único volumen no puede exceder los tamaños resumidos en la tabla.

**Sistema operativo** | **Límite de tamaño**
--- | ---
Windows Server 2012 o superior |  54 400 GB
Windows Server 2008 R2 SP1 |    1700 GB
Windows Server 2008 SP2 | 1700 GB
Windows 8 o posterior  | 54 400 GB
Windows 7   | 1700 GB


## <a name="supported-file-types-for-backup"></a>Tipos de archivo compatibles para copia de seguridad

**Tipo** | **Compatible** 
--- | --- 
Cifrados   | Sí 
Comprimidos | Sí
Dispersos | Sí 
Comprimidos y dispersos | Sí
Vínculos físicos  | No compatible<br/><br/> Skipped
Punto de repetición de análisis   | No compatible<br/><br/> Skipped
Cifrados y dispersos |  No compatible<br/><br/> Skipped
Flujo comprimido   | No compatible<br/><br/> Skipped
Flujo disperso   | No compatible<br/><br/> Skipped
Una unidad (los archivos sincronizados son flujos dispersos)    | No compatible 

## <a name="supported-drivesvolumes-for-backup"></a>Unidades/volúmenes compatibles para copia de seguridad

**Unidad/volumen** | **Compatible** | **Detalles**
--- | --- | ---
Volúmenes de solo lectura   | No compatible | El volumen debe ser grabable para que VSS funcione.
Volúmenes sin conexión | No compatible |   El volumen debe estar en línea para que VSS funcione.
Recurso compartido de red   | No compatible |   El volumen debe ser local en el servidor para la copia de seguridad.
Volúmenes protegidos por BitLocker | No compatible |   El volumen debe desbloquearse para que la copia de seguridad funcione.
Identificación del sistema de archivos  | No compatible |   Solo NTFS.
Medios extraíbles | No compatible |   Todos los orígenes de elementos de copia de seguridad deben tener el estado corregido.
Unidades desduplicadas |  Se admite.<br/><br/> Azure Backup convierte los datos desduplicados en datos normales. Optimiza los datos, los cifra, los almacena y los envía al almacén.

## <a name="support-for-initial-offline-backup"></a>Compatibilidad con copia de seguridad inicial sin conexión

Azure Backup es compatible con la "propagación sin conexión" para transferir datos de copia de seguridad iniciales a Azure mediante discos. Esto resulta útil si es probable que la copia de seguridad inicial esté en el intervalo de terabytes (TB). La copia de seguridad sin conexión es compatible para:

- Copia de seguridad directa de archivos y carpetas en máquinas locales que ejecutan el agente de MARS.
- Copia de seguridad de cargas de trabajo y archivos desde un servidor DPM o MABS.
- La copia de seguridad sin conexión no se puede usar para archivos de estado del sistema.


## <a name="support-for-restore"></a>Compatibilidad para restauración

- La nueva versión de [restauración instantánea](/backup-instant-restore-capability.md) de Azure Backup permite restaurar los datos antes de copiarlos al almacén.<br/><br/> Para usar esta característica, la máquina de copia de seguridad debe ejecutar .NET Framework 4.5.2 o una versión posterior.
- Las copias de seguridad no se pueden restaurar en una máquina de destino en la que se ejecuta una versión anterior del sistema operativo. Por ejemplo, una copia de seguridad perteneciente a un equipo con Windows 7 se puede restaurar en Windows 8 o posterior. Sin embargo, una copia de seguridad realizada en un equipo con Windows 8 no se puede restaurar en un equipo con Windows 7.


## <a name="next-steps"></a>Pasos siguientes
- [Obtenga más información](backup-architecture.md#architecture-direct-backup-of-on-premises-windows-machinesazure-vm-filesfolders) sobre la arquitectura de copia de seguridad con el agente de MARS.
- [Obtenga información](backup-support-matrix-mabs-dpm.md) sobre lo que se admite al ejecutar el agente de MARS en Microsoft Azure Backup Server (MABS) o System Center DPM.


