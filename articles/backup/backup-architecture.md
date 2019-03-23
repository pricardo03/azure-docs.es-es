---
title: Arquitectura de Azure Backup
description: Proporciona información general sobre la arquitectura, los componentes y los procesos usados por el servicio Azure Backup.
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 02/19/2019
ms.author: raynew
ms.openlocfilehash: 98ffe145103b4be04014627ed04d04dcf7542015
ms.sourcegitcommit: 49c8204824c4f7b067cd35dbd0d44352f7e1f95e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/22/2019
ms.locfileid: "58368968"
---
# <a name="azure-backup-architecture"></a>Arquitectura de Azure Backup

Puede usar el [servicio Azure Backup](backup-overview.md) para realizar una copia de seguridad de los datos a la plataforma de nube de Microsoft Azure. En este artículo se resume la arquitectura, los componentes y los procesos de Azure Backup. 

## <a name="what-does-azure-backup-do"></a>¿Qué hace Azure Backup?

Azure Backup realiza copias de seguridad de los datos, estado de la máquina y las cargas de trabajo que se ejecutan en máquinas locales e instancias de máquina virtual de Azure (VM). Son varios los escenarios de uso de Azure Backup.

## <a name="how-does-azure-backup-work"></a>¿Cómo funciona Azure Backup?

Puede realizar copias de seguridad de máquinas y los datos mediante el uso de una serie de métodos:

- **Copia de seguridad de máquinas locales**:
    - Puede realizar copias de seguridad de máquinas de Windows local directamente en Azure usando el agente de Azure Backup Microsoft Azure Recovery Services (MARS). No se admiten máquinas Linux.
    - Puede realizar copias de seguridad de máquinas locales a un servidor de copia de seguridad (System Center Data Protection Manager (DPM) o Microsoft Azure Backup Server (MABS)). A continuación, puede hacer la copia de seguridad del servidor de copia de seguridad en un almacén de Recovery Services en Azure.

- **Copia de seguridad de máquinas virtuales de Azure**:
    - Puede hacer una copia de seguridad de las máquinas virtuales de Azure directamente. Azure Backup instala una extensión de copia de seguridad para el agente de máquina virtual de Azure que se ejecuta en la máquina virtual. Esta extensión realiza una copia de seguridad de toda la máquina virtual.
    - Puede hacer una copia de seguridad de determinados archivos y carpetas en la máquina virtual de Azure mediante la ejecución del agente de MARS.
    - Puede realizar copias de seguridad de máquinas virtuales de Azure a la que se ejecuta en Azure MABS y, a continuación, hacer copias de seguridad el MABS a un almacén de Recovery Services.

Obtenga más información sobre [lo que puede realizar copias de seguridad](backup-overview.md) y aproximadamente [admite escenarios de copia de seguridad](backup-support-matrix.md).

## <a name="where-is-data-backed-up"></a>¿Dónde están los datos de copia de seguridad?

Azure Backup almacena los datos de copia de seguridad en un almacén de Recovery Services. Un almacén es una entidad de almacenamiento en línea en Azure que se usa para almacenar los datos, como copias de seguridad, puntos de recuperación y las directivas de copia de seguridad.

Almacenes de Recovery Services tienen las siguientes características:

- Los almacenes facilitan la tarea de organizar los datos de copia de seguridad, al mismo tiempo que reducen al mínimo su sobrecarga administrativa.
- En cada suscripción de Azure, puede crear hasta 500 almacenes.
- Puede supervisar elementos de copia de seguridad en un almacén, incluidas las máquinas locales y máquinas virtuales de Azure.
- Puede administrar el acceso del almacén con [control de acceso basado en rol (RBAC)](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal) de Azure.
- Especificará cómo se replican los datos en el almacén para la redundancia:
    - **Almacenamiento con redundancia local (LRS)**: Para protegerse frente a errores en un centro de datos, puede usar LRS. LRS replica los datos en una unidad de escalado de almacenamiento. [Más información](https://docs.microsoft.com/azure/storage/common/storage-redundancy-lrs).
    - **Almacenamiento con redundancia geográfica (GRS)**: Para protegerse contra las interrupciones de toda la región, puede usar GRS. GRS replica los datos en una región secundaria. [Más información](https://docs.microsoft.com/azure/storage/common/storage-redundancy-grs). 
    - De forma predeterminada, los almacenes de Recovery Services usan GRS. 

## <a name="backup-agents"></a>Agentes de copia de seguridad

Copia de seguridad de Azure proporciona diferentes agentes de copia de seguridad, según el tipo de máquina de la copia de seguridad:

**Agent** | **Detalles** 
--- | --- 
**Agente de MARS** | <ul><li>Se ejecuta en máquinas de Windows Server locales para realizar una copia de seguridad de archivos, carpetas y el estado del sistema.</li> <li>Se ejecuta en máquinas virtuales de Azure para realizar una copia de seguridad de archivos, carpetas y el estado del sistema.</li> <li>Se ejecuta en servidores DPM/MABS para realizar copias de seguridad del disco de almacenamiento local de DPM/MABS a Azure.</li></ul> 
**Extensión de máquina virtual de Azure** | Se ejecuta en máquinas virtuales de Azure para copiarlas en un almacén.

## <a name="backup-types"></a>Tipos de copia de seguridad

La siguiente tabla explica los distintos tipos de copias de seguridad y cuando se usan:

**Tipo de copia de seguridad** | **Detalles** | **Uso**
--- | --- | ---
**Completa** | Una copia de seguridad completa contiene el origen de datos completo. Toma más ancho de banda de red que las copias de seguridad incrementales o diferenciales. | Se usa para la copia de seguridad inicial.
**Diferencial** |  Una copia de seguridad diferencial almacena los bloques que cambiaron desde la copia de seguridad completa inicial. Usa una menor cantidad de almacenamiento y red y no mantener copias redundantes de datos sin modificar.<br/><br/> Ineficaz porque se transfieren y almacenan los bloques de datos que han cambiado entre las copias de seguridad posteriores. | Azure Backup no usa este tipo.
**Incremental** | Una copia de seguridad incremental almacena solo los bloques de datos que ha cambiado desde la copia de seguridad anterior. Alta eficacia de almacenamiento y red. <br/><br/> Con la copia de seguridad incremental, no hay ninguna necesidad para complementar con copias de seguridad completas. | Se usa en DPM o MABS para copias de seguridad de disco y se emplea en todas las copias de seguridad en Azure.

## <a name="sql-server-backup-types"></a>Tipos de copia de seguridad de SQL Server

La siguiente tabla explica los distintos tipos de copias de seguridad utilizados para las bases de datos de SQL Server y con qué frecuencia se usan:

**Tipo de copia de seguridad** | **Detalles** | **Uso**
--- | --- | ---
**Copia de seguridad completa** | una copia de seguridad completa de la base de datos realiza una copia de seguridad de toda la base de datos. Contiene todos los datos en una base de datos específica o en un conjunto de archivos o grupos de archivos. Una copia de seguridad completa también contiene suficientes registros para recuperar esos datos. | A lo sumo, puede desencadenar una copia de seguridad completa al día.<br/><br/> Puede elegir hacer una completa de copia de seguridad en un intervalo diario o semanal.
**Copia de seguridad diferencial**: | Una copia de seguridad diferencial se basa en el máximo anterior completo datos copia de seguridad reciente.<br/><br/> Captura solo los datos que han cambiado desde la copia de seguridad completa. |  A lo sumo, puede desencadenar una copia de seguridad diferencial al día.<br/><br/> No se puede configurar una copia de seguridad completa y una copia de seguridad diferencial en el mismo día.
**Copia de seguridad del registro de transacciones**: | una copia de seguridad de registros permite realizar la restauración a un momento dado con una precisión de un segundo. | A lo sumo, puede configurar las copias de seguridad del registro de transacciones cada 15 minutos.

### <a name="comparison-of-backup-types"></a>Comparación de tipos de copia de seguridad

El consumo de almacenamiento, el objetivo de tiempo de recuperación (RTO) y el consumo de red varían según el tipo de copia de seguridad. La siguiente imagen muestra una comparación de los tipos de copia de seguridad:

- Origen de datos A se compone de 10 bloques de almacenamiento, A1-A10, que se copian de seguridad mensual.
- Los bloques A2, A3, A4 y A9 cambian en el primer mes y el bloque A5 cambia en el siguiente mes.
- Para las copias de seguridad diferenciales, en el segundo mes, se realiza una copia de seguridad de los bloques modificados A2, A3, A4 y A9. En el tercer mes, se vuelve a hacer una copia de seguridad de estos mismos bloques, junto con el bloque A5 modificado. Los bloques modificados se siguen copiando hasta que tiene lugar la siguiente copia de seguridad completa.
- Copias de seguridad incrementales, en el segundo mes, bloques A2, A3, A4 y A9 se marcan como cambiado y se transfieren. En el tercer mes, solo el bloque A5 modificado se marca y se transfiere. 

![Imagen que muestra las comparaciones de métodos de copia de seguridad](./media/backup-architecture/backup-method-comparison.png)

## <a name="backup-features"></a>Características de copia de seguridad

En la tabla siguiente se resume las características admitidas para los distintos tipos de copia de seguridad:

**Característica** | **Máquinas de Windows Server local (directa)** | **Máquinas virtuales de Azure** | **Equipos o aplicaciones con DPM/MABS**
--- | --- | --- | ---
Copia de seguridad en almacén | ![Sí][green] | ![Sí][green] | ![Sí][green] 
Copia de seguridad en disco DPM/MABS, a continuación, en Azure | | | ![Sí][green] 
Compresión de los datos enviados para copia de seguridad | ![Sí][green] | No se usa compresión al transferir los datos. El almacenamiento aumenta ligeramente, pero la restauración es más rápida.  | ![Sí][green] 
Ejecución de copia de seguridad incremental |![Sí][green] |![Sí][green] |![Sí][green] 
Copia de seguridad de discos desduplicados | | | ![Parcialmente][yellow]<br/><br/> Solo para servidores DPM o MABS implementados en el entorno local. 

![Clave de la tabla](./media/backup-architecture/table-key.png)

## <a name="architecture-direct-backup-of-azure-vms"></a>Arquitectura: copia de seguridad directa de máquinas virtuales de Azure

1. Cuando se habilita la copia de seguridad para una máquina virtual de Azure, una copia de seguridad se ejecuta según la programación que especifique.
1. Durante la primera copia de seguridad, se instala una extensión de copia de seguridad en la máquina virtual si se está ejecutando la máquina virtual.
    - Para las máquinas virtuales de Windows, se instala la extensión VMSnapshot.
    - Para las máquinas virtuales de Linux, se instala la extensión VMSnapshot Linux.
1. La extensión toma una instantánea de nivel de almacenamiento. 
    - Para máquinas virtuales de Windows que ejecutan, copia de seguridad se coordina con el Windows Volume Shadow Copy Service (VSS) para tomar una instantánea coherente con la aplicación de la máquina virtual. De forma predeterminada, la copia de seguridad toma copias de seguridad completas de VSS. Si Azure Backup no puede tomar una instantánea coherente con la aplicación, toma una instantánea coherente con el archivo.
    - Las máquinas virtuales de Linux, copia de seguridad toma una instantánea coherente con archivo. En las instantáneas coherentes con la aplicación, debe personalizar manualmente los scripts anteriores y posteriores.
    - Azure Backup se optimiza mediante la copia de seguridad de cada disco de máquina virtual en paralelo. Este servicio lee los bloques de cada disco que se va a copiar y solo almacena los datos cambiados. 
1. Después de tomar la instantánea, los datos se transfieren al almacén. 
    - Solo los bloques de datos que ha cambiado desde la última copia de seguridad se copian.
    - Los datos no se cifran. Copia de seguridad de Azure puede hacer una copia de seguridad de máquinas virtuales de Azure que se cifraron mediante el uso de Azure Disk Encryption.
    - Es posible que los datos de las instantáneas no se copien inmediatamente en el almacén. En las horas punta, la copia de seguridad puede tardar algunas horas. Tiempo total de copia de seguridad para una máquina virtual será de menos de 24 horas para las directivas de copia de seguridad diarias.
1. Después de que los datos se envían en el almacén, se elimina la instantánea y se crea un punto de recuperación.

Máquinas virtuales de Azure necesitan acceso a internet para los comandos de control. Si hace copia de seguridad las cargas de trabajo dentro de la máquina virtual (por ejemplo, copias de seguridad de SQL Server database), los datos de back-end también necesitan acceso a internet. 

![Copia de seguridad de máquinas virtuales de Azure](./media/backup-architecture/architecture-azure-vm.png)

## <a name="architecture-direct-backup-of-on-premises-windows-server-machines-or-azure-vm-files-or-folders"></a>Arquitectura: Copia de seguridad directa de equipos de Windows Server locales o los archivos de máquina virtual de Azure o carpetas

1. Para configurar el escenario, descargue e instale al agente de MARS en la máquina. A continuación, selecciona qué copia de seguridad, cuándo se ejecutarán las copias de seguridad y cuánto tiempo se deberá mantener en Azure.
1. La copia de seguridad inicial se ejecuta según la configuración de copia de seguridad.
1. El agente de MARS usa VSS para tomar una instantánea en el momento de los volúmenes seleccionados para copia de seguridad.
    - El agente de MARS usa solo la operación de escritura de Windows del sistema para capturar la instantánea.
    - Dado que el agente no usar los escritores de VSS de aplicación, no captura las instantáneas coherentes con la aplicación.
1. Después de tomar la instantánea de VSS, el agente de MARS crea un disco duro virtual (VHD) en la carpeta de caché especificado al configurar la copia de seguridad. El agente también almacena las sumas de comprobación para cada bloque de datos.
1. Copias de seguridad incrementales se ejecutan según la programación que especifique, a menos que ejecute una copia de seguridad ad hoc.
1. En las copias de seguridad incrementales, se identifican los archivos modificados y se crea un disco duro virtual. El disco duro virtual se comprimen y cifran, y, a continuación, se envía en el almacén.
1. Una vez finalizada la copia de seguridad incremental, el nuevo disco duro virtual se combina con el VHD creado después de la replicación inicial. Este disco duro virtual combinado proporciona el estado más reciente que se usará para la comparación de copia de seguridad en curso.

![Copia de seguridad de máquinas de Windows Server locales con el agente de MARS](./media/backup-architecture/architecture-on-premises-mars.png)

## <a name="architecture-back-up-to-dpmmabs"></a>Arquitectura: copia de seguridad en DPM o MABS

1. Instale al agente de protección DPM o MABS en las máquinas que desea proteger. A continuación, agregue las máquinas a un grupo de protección de DPM.
    - Para proteger las máquinas locales, el servidor DPM o MABS debe ubicarse en el entorno local.
    - Para proteger las máquinas virtuales de Azure, el servidor MABS debe ubicarse en Azure, de modo que se ejecuta como una máquina virtual de Azure.
    - Puede proteger con DPM/MABS, carpetas, recursos compartidos, los archivos y volúmenes de copia de seguridad. También puede proteger el estado del sistema de un equipo (sin sistema operativo), y puede proteger aplicaciones específicas con la configuración de copia de seguridad basadas en la aplicación.
1. Al configurar la protección para una máquina o aplicación en DPM/MABS, se seleccione esta opción para realizar copias de seguridad en el disco local de DPM/MABS para almacenamiento a corto plazo y a Azure para la protección en línea. También especifica cuándo se debe ejecutar la copia de seguridad en el almacenamiento local de DPM/MABS y cuándo se debe ejecutar la copia de seguridad en línea en Azure.
1. El disco de la carga de trabajo protegida se copia en los discos locales de DPM/MABS, según la programación especificada.
4. Los discos DPM/MABS se copian en el almacén mediante el agente de MARS se está ejecutando en el servidor DPM/MABS.

![Copia de seguridad de equipos y cargas de trabajo protegidos por DPM o MABS](./media/backup-architecture/architecture-dpm-mabs.png)

## <a name="azure-vm-storage"></a>Almacenamiento de máquinas virtuales de Azure

Las máquinas virtuales de Azure usan discos para almacenar su sistema operativo, sus aplicaciones y sus datos. Cada máquina virtual de Azure tiene al menos dos discos: un disco para el sistema operativo y un disco temporal. Máquinas virtuales de Azure también pueden tener discos de datos para datos de la aplicación. Los discos se almacenan como discos duros virtuales.

- Los discos duros virtuales se almacenan como blobs en páginas en las cuentas de almacenamiento estándar o premium de Azure:
    - **Almacenamiento estándar:** compatibilidad confiable y de bajo costo con máquinas virtuales que ejecutan cargas de trabajo que no son sensibles a la latencia. Almacenamiento estándar puede usar discos estándar unidades de estado sólido (SSD) o unidad de disco duro (HDD) estándar.
    - **Almacenamiento Premium:** compatibilidad con discos de alto rendimiento. Usa discos SSD premium.
- Existen distintos niveles de rendimiento para los discos:
    - **Disco de unidad de disco duro estándar:** están respaldado por HDD y se usan cuando se busca un almacenamiento rentable.
    - **Disco SSD estándar:** Combina elementos de los discos SSD premium y estándares discos de la unidad de disco duro. Ofrece más coherente rendimiento y confiabilidad que la unidad de disco duro, pero todavía rentable.
    - **Disco SSD Premium:** Respaldado por SSD y proporciona baja latencia y alto rendimiento para las máquinas virtuales que ejecutan cargas de trabajo de E/s intensivas.
- Los discos pueden ser administrados o no administrados:
    - **Discos no administrados:** Tipo tradicional de discos usados por las máquinas virtuales. Con estos discos, creará la propia cuenta de almacenamiento y especificará esa cuenta al crear el disco. A continuación, deberá averiguar cómo maximizar los recursos de almacenamiento para las máquinas virtuales.
    - **Discos administrados:** Azure crea y administra las cuentas de almacenamiento en su nombre. Especifique el nivel de rendimiento y tamaño de disco y Azure crea discos administrados automáticamente. Agregar discos y escalado de máquinas virtuales, Azure controla las cuentas de almacenamiento.

Para obtener más información sobre el almacenamiento en disco y los tipos de disco disponible para las máquinas virtuales, consulte estos artículos:

- [Azure managed disks para máquinas virtuales de Windows](../virtual-machines/windows/managed-disks-overview.md)
- [Azure managed disks para máquinas virtuales Linux](../virtual-machines/linux/managed-disks-overview.md)
- [Tipos de disco disponible para las máquinas virtuales](../virtual-machines/windows/disks-types.md)

### <a name="back-up-and-restore-azure-vms-with-premium-storage"></a>Copia de seguridad y restauración de máquinas virtuales de Azure con premium storage 

Puede realizar copias de seguridad de máquinas virtuales de Azure con premium storage con Azure Backup:

- Durante el proceso de copia de seguridad de máquinas virtuales con premium storage, el servicio de copia de seguridad crea una ubicación de ensayo temporal, denominada *AzureBackup -*, en la cuenta de almacenamiento. El tamaño de la ubicación de ensayo es igual al tamaño de la instantánea de punto de recuperación.
- Asegúrese de que haya espacio disponible suficiente en la cuenta de Premium Storage para dar cabida a la ubicación de almacenamiento provisional. [Más información](../storage/common/storage-scalability-targets.md#premium-performance-storage-account-scale-limits). No modifique la ubicación de almacenamiento provisional.
- Una vez finalizado el trabajo de copia de seguridad, se elimina esta ubicación.
- El precio del almacenamiento usado para la ubicación de almacenamiento provisional es coherente con todos los [precios de Premium Storage](../virtual-machines/windows/disks-types.md#billing).

Al restaurar máquinas virtuales de Azure mediante premium storage, puede restaurarlos a almacenamiento estándar o premium. Normalmente, se podría restaurar a premium storage. Pero si necesita solo un subconjunto de archivos de la máquina virtual, podría ser más rentable para restaurarlas al almacenamiento estándar.

### <a name="back-up-and-restore-managed-disks"></a>Copia de seguridad y restauración de discos administrados

Puede realizar copias de seguridad de máquinas virtuales de Azure con discos administrados:

- La copia de seguridad de máquinas virtuales con discos administrados se hace de la misma manera que con cualquier otra máquina virtual de Azure. Se puede hacer una copia de seguridad de la máquina virtual directamente desde la configuración de dicha máquina, o se puede habilitar la copia de seguridad para las máquinas virtuales en el almacén de Recovery Services.
- Puede realizar la copia de seguridad de máquinas virtuales en discos administrados mediante colecciones de RestorePoint basadas en discos administrados.
- Azure Backup también admite la copia de seguridad de máquinas virtuales con discos administrados que se cifraron mediante el uso de Azure Disk Encryption.

Al restaurar las máquinas virtuales con discos administrados, puede restaurar a una máquina virtual completa con discos administrados o a una cuenta de almacenamiento:

- Durante el proceso de restauración, Azure encarga de los discos administrados. Si usa la opción de cuenta de almacenamiento, administrar la cuenta de almacenamiento que se crea durante el proceso de restauración.
- Si restaura una máquina virtual administrada que está cifrada, asegúrese de que las claves de la máquina virtual y los secretos existen en el almacén de claves antes de iniciar el proceso de restauración.

## <a name="next-steps"></a>Pasos siguientes

- Revise la matriz de compatibilidad para [Obtenga información sobre las características admitidas y las limitaciones para escenarios de copia de seguridad](backup-support-matrix.md).
- Configurar copia de seguridad para uno de estos escenarios:
    - [Copia de seguridad de máquinas virtuales de Azure](backup-azure-arm-vms-prepare.md).
    - [Copia de seguridad directa de máquinas Windows](tutorial-backup-windows-server-to-azure.md), sin un servidor de copia de seguridad.
    - [Configuración de MABS](backup-azure-microsoft-azure-backup.md) para la copia de seguridad en Azure y, luego, copia de seguridad de las cargas de trabajo en MABS.
    - [Configuración de DPM](backup-azure-dpm-introduction.md) para la copia de seguridad en Azure y, luego, copia de seguridad de las cargas de trabajo en DPM.


[green]: ./media/backup-architecture/green.png
[yellow]: ./media/backup-architecture/yellow.png
[red]: ./media/backup-architecture/red.png

