---
title: Arquitectura de Azure Backup
description: Proporciona información general sobre la arquitectura, los componentes y los procesos usados por el servicio Azure Backup.
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 01/15/2019
ms.author: raynew
ms.openlocfilehash: 84890c0658970aa9f61a06764cf902a5e5ee4379
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/23/2019
ms.locfileid: "54812573"
---
# <a name="azure-backup-architecture"></a>Arquitectura de Azure Backup

Puede usar el [servicio Azure Backup](backup-overview.md) para realizar una copia de seguridad de los datos en la nube de Microsoft Azure. En este artículo se resume la arquitectura, los componentes y los procesos de Azure Backup. 


## <a name="what-does-azure-backup-do"></a>¿Qué hace Azure Backup?

Azure Backup realiza copias de seguridad de los datos, del estado de la máquina y de las cargas de trabajo que se ejecutan en máquinas locales y máquinas virtuales de Azure. Son varios los escenarios de uso de Azure Backup.

- **Copia de seguridad de máquinas locales**:
    - puede hacer una copia de seguridad de las máquinas locales directamente en Azure mediante Azure Backup.
    - Puede proteger las máquinas locales con System Center Data Protection Manager (DPM) o Microsoft Azure Backup Server (MABS) y, luego, hacer a su vez una copia de seguridad de los datos protegidos en DPM o MABS en Azure mediante Azure Backup.
- **Copia de seguridad de máquinas virtuales de Azure**:
    - puede hacer una copia de seguridad de las máquinas virtuales de Azure directamente con Azure Backup.
    - Puede proteger las máquinas virtuales de Azure con DPM o MABS en Azure y, luego, hacer a su vez copia de seguridad de los datos protegidos en DPM o MABS con Azure Backup.

Más información sobre los [elementos de lo que puede hacer una copia de seguridad](backup-overview.md) y los [escenarios de copia de seguridad admitidos](backup-support-matrix.md).


## <a name="where-is-data-backed-up"></a>¿Dónde están los datos de copia de seguridad?

Azure Backup almacena los datos de copia de seguridad en un almacén de Recovery Services. Un almacén es una entidad de almacenamiento en línea de Azure que se usa para contener datos, como copias de seguridad, puntos de recuperación y directivas de copia de seguridad.

- Los almacenes de Recovery Services facilitan la tarea de organizar los datos de copia de seguridad, al mismo tiempo que reduce al mínimo su sobrecarga administrativa.
- En cada suscripción de Azure, puede crear hasta 500 almacenes de Recovery Services. 
- Puede supervisar elementos de copia de seguridad de un almacén, como las máquinas virtuales de Azure y las máquinas locales.
- Puede administrar el acceso del almacén con [control de acceso basado en rol (RBAC)](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal) de Azure.
- Especificará cómo se replican los datos en el almacén para la redundancia:
    - **LRS**: puede usar almacenamiento con redundancia local (LRS) para protegerse frente a errores en un centro de datos. LRS replica los datos en una unidad de escalado de almacenamiento. [Más información](https://docs.microsoft.com/azure/storage/common/storage-redundancy-lrs).
    - **GRS**: puede usar almacenamiento con redundancia geográfica (GRS) para: Protegerse frente a interrupciones en toda una región Replicar los datos en una región secundaria [Más información](https://docs.microsoft.com/azure/storage/common/storage-redundancy-grs). 
    - De forma predeterminada, los almacenes de Recovery Services para copia de seguridad usan GRS. 



## <a name="how-does-azure-backup-work"></a>¿Cómo funciona Azure Backup?

Azure Backup ejecuta trabajos de copia de seguridad en función de una directiva de copia de seguridad predeterminada o personalizada. La manera en que Azure Backup realiza una copia de seguridad depende del escenario.

**Escenario** | **Detalles** 
--- | ---
**Copia de seguridad directa de máquinas locales** | Para hacer una copia de seguridad directa de máquinas locales, Azure Backup usa el agente de Microsoft Azure Recovery Services (MARS). El agente se instala en cada máquina que quiere copiar. <br/><br/> Este tipo de copia de seguridad no está disponible para máquinas Linux locales. 
**Copia de seguridad directa de máquinas virtuales de Azure** | Para hacer una copia de seguridad directa de máquinas virtuales de Azure, se instala una extensión de máquina virtual de Azure en la máquina virtual la primera vez que se ejecuta una copia de seguridad de ella. 
**Copia de seguridad de máquinas y aplicaciones protegidas por DPM o MABS** | En este escenario, se realiza primero una copia de seguridad de la máquina o de la aplicación en almacenamiento local de DPM o MABS. Luego, Azure Backup realiza una copia de seguridad de los datos de DPM o MABS en el almacén. Las máquinas locales se pueden proteger mediante DPM o MABS que se ejecutan en el entorno local. Las máquinas virtuales de Azure se pueden proteger mediante DPM o MABS que se ejecutan en Azure.

[Obtenga información general sobre](backup-overview.md)y consulte [lo que se admite](backup-support-matrix.md) en cada escenario.


### <a name="backup-agents"></a>Agentes de copia de seguridad

Azure Backup proporciona diferentes agentes, según el tipo de copia de seguridad.

**Agent** | **Detalles** 
--- | --- 
**Agente de Microsoft Azure Recovery Services (MARS)** | Este agente se ejecuta en servidores Windows locales individuales para hacer una copia de seguridad de archivos, carpetas y el estado del sistema.<br/><br/> Este agente se ejecuta en servidores DPM o MABS para hacer una copia de seguridad del disco de almacenamiento local de DPM o MABS. Las máquinas y aplicaciones se copian localmente en este disco de DPM o MABS.
**Extensión de máquina virtual de Azure** | Para hacer una copia de seguridad de máquinas virtuales de Azure, se agrega una extensión de copia de seguridad al agente de máquina virtual de Azure que se ejecuta en las máquinas virtuales. 


## <a name="backup-types"></a>Tipos de copia de seguridad

**Tipo de copia de seguridad** | **Detalles** | **Uso**
--- | --- | ---
**Completa** | Una copia de seguridad contiene el origen de datos completo.<br/><br/> La copia de seguridad completa consume una mayor cantidad de ancho de banda de red. | Se usa para la copia de seguridad inicial.
**Diferencial** |  Almacena los bloques que cambiaron desde la copia de seguridad completa inicial. Usa una menor cantidad de almacenamiento y red y no conserva copias redundantes de datos sin modificar.<br/><br/> Ineficaz porque los bloques de datos sin modificar se transfieren y almacenan entre las copias de seguridad posteriores. | Azure Backup no usa este tipo.
**Incremental** | Alta eficacia de almacenamiento y red. Almacena solo los bloques de datos que cambiaron desde la copia de seguridad anterior. <br/><br/> No es necesario complementar las copias de seguridad incrementales con copias de seguridad completas. | Se usa en DPM o MABS para copias de seguridad de disco y se emplea en todas las copias de seguridad en Azure.

### <a name="comparison"></a>De comparación

El consumo de almacenamiento, el objetivo de tiempo de recuperación (RTO) y el consumo de red varían según el tipo de copia de seguridad. En la siguiente imagen se muestra una comparación de los tipos de copia de seguridad.
- El origen de datos A se compone de 10 bloques de almacenamiento A1-A10, y todos los meses se hace una copia de seguridad de ellos.
- Los bloques A2, A3, A4 y A9 cambian en el primer mes y el bloque A5 cambia en el siguiente mes.
- Para las copias de seguridad diferenciales, en el segundo mes, se realiza una copia de seguridad de los bloques modificados A2, A3, A4 y A9. En el tercer mes, se vuelve a hacer una copia de seguridad de estos mismos bloques, junto con el bloque A5 modificado. Los bloques modificados se siguen copiando hasta que tiene lugar la siguiente copia de seguridad completa.
- Para las copias de seguridad incrementales, después de realizar la copia de seguridad completa el primer mes, los bloques A2, A3, A4 y A9 se marcan como modificados y se transfieren al segundo mes. En el tercer mes, solo el bloque A5 modificado se marca y se transfiere. 

![imagen que muestra las comparaciones de métodos de copia de seguridad](./media/backup-architecture/backup-method-comparison.png)

## <a name="backup-features"></a>Características de copia de seguridad

En la tabla siguiente se resumen las características de los distintos tipos de copia de seguridad.

**Característica** | **Máquinas Windows locales (directa)** | **Máquinas virtuales de Azure** | **Máquinas o aplicaciones con DPM o MABS**
--- | --- | --- | ---
Copia de seguridad en almacén | ![SÍ][green] | ![Sí][green] | ![SÍ][green] 
Copia de seguridad en disco DPM o MABS y, luego, en Azure | | | ![SÍ][green] 
Compresión de los datos enviados para copia de seguridad | ![SÍ][green] | No se usa compresión al transferir los datos. El almacenamiento aumenta ligeramente, pero la restauración es más rápida.  | ![SÍ][green] 
Ejecución de copia de seguridad incremental |![SÍ][green] |![Sí][green] |![SÍ][green] 
Copia de seguridad de discos desduplicados | | | ![Parcialmente][yellow]<br/><br/> Solo para servidores DPM o MABS implementados en el entorno local. 

![clave de tabla](./media/backup-architecture/table-key.png)


## <a name="architecture-direct-backup-of-on-premises-windows-machines"></a>Arquitectura: copia de seguridad directa de máquinas Windows locales

1. Para configurar el escenario, descargará e instalará el agente de Microsoft Azure Recovery Services (MARS) en la máquina, seleccionará lo que quiere copiar, cuándo se ejecutarán las copias de seguridad y cuánto tiempo se guardarán en Azure.
2. La copia de seguridad inicial se ejecuta según la configuración de copia de seguridad.
3. El agente de MARS usa el Servicio de instantáneas de volumen (VSS) de Windows para tomar una instantánea de un momento dado de los volúmenes seleccionados para copia de seguridad.
    - El agente de MARS solo usa la escritura del sistema Windows para capturar la instantánea.
    - El agente no usa instancias de VSS Writer de aplicaciones y, por tanto, no captura instantáneas coherentes con la aplicación.
3. Después de tomar la instantánea con VSS, el agente de MARS crea un disco duro virtual en la carpeta de caché que especificó al configurar la copia de seguridad, y almacena las sumas de comprobación de cada bloque de datos. 
4. Se ejecutan copias de seguridad incrementales según la programación que especifique, a menos que ejecute una copia de seguridad ad hoc.
5. En las copias de seguridad incrementales, se identifican los archivos modificados y se crea un disco duro virtual. Este disco se comprime y cifra, y se envía al almacén.
6. Una vez finalizada la copia de seguridad incremental, el nuevo disco duro virtual se combina con el disco duro virtual creado después de la replicación inicial, de forma que se proporciona el estado más reciente que se usará para la comparación de la copia de seguridad en curso. 

![Copia de seguridad del servidor Windows local con el agente de MARS](./media/backup-architecture/architecture-on-premises-mars.png)


## <a name="architecture-direct-backup-of-azure-vms"></a>Arquitectura: copia de seguridad directa de máquinas virtuales de Azure

1. Cuando se habilita la copia de seguridad de una máquina virtual de Azure, se ejecuta una copia de seguridad con la programación que especifique.
2. Durante la primera copia de seguridad, se instala una extensión de copia de seguridad en la máquina virtual si se está ejecutando.
    - En máquinas virtuales Windows, se instala la extensión VMSnapshot.
    - En máquinas virtuales Linux, se instala la extensión VMSnapshot para Linux.
3. La extensión toma una instantánea de nivel de almacenamiento. 
    - En máquinas virtuales Windows que están en ejecución, Azure Backup se coordina con VSS para tomar una instantánea coherente con la aplicación de la máquina virtual. De forma predeterminada, Azure Backup realiza copias de seguridad de VSS completas. Si Azure Backup no puede tomar una instantánea coherente con la aplicación, toma una instantánea coherente con el archivo.
    - En máquinas virtuales Linux, Azure Backup hace una copia de seguridad coherente con el archivo. Para obtener instantáneas coherentes con la aplicación, debe personalizar manualmente los scripts previos y posteriores.
    - Azure Backup se optimiza mediante la copia de seguridad de cada disco de máquina virtual en paralelo. Este servicio lee los bloques de cada disco que se va a copiar y solo almacena los datos cambiados. 
4. Después de tomar la instantánea, los datos se transfieren al almacén. 
    - Solo se copian los bloques de datos que han cambiado desde la última copia de seguridad.
    - Los datos no se cifran. Azure Backup puede hacer una copia de seguridad de las máquinas virtuales de Azure que estén cifradas mediante Azure Disk Encryption (ADE).
    - Es posible que los datos de las instantáneas no se copien inmediatamente en el almacén. Podrían tardar horas en momentos de máxima actividad. El tiempo total de copia de seguridad de una máquina virtual será inferior a 24 horas para las directivas de copia de seguridad diarias.
5. Una vez enviados los datos al almacén, se elimina la instantánea y se crea un punto de recuperación.


![Copia de seguridad de máquinas virtuales de Azure](./media/backup-architecture/architecture-azure-vm.png)


## <a name="architecture-back-up-to-dpmmabs"></a>Arquitectura: copia de seguridad en DPM o MABS

1. Se instala el agente de protección de DPM o MABS en las máquinas que quiera proteger y se agregan las máquinas a un grupo de protección de DPM.
    - Para proteger las máquinas locales, el servidor DPM o MABS debe ubicarse en el entorno local.
    - Para proteger las máquinas virtuales de Azure, el servidor DPM o MABS debe ubicarse en Azure, de modo que se ejecuta como una máquina virtual de Azure.
    - Con DPM o MABS puede proteger volúmenes de copia de seguridad, recursos compartidos, archivos y carpetas. Puede proteger el estado del sistema o la reconstrucción completa de las máquinas, y proteger aplicaciones específicas con configuración de reconocimiento de aplicaciones.
2. Al configurar la protección para una máquina o una aplicación en DPM, seleccionará la copia de seguridad en el disco local de DPM para el almacenamiento a corto plazo, y en Azure (protección en línea). También especificará cuándo se debe ejecutar la copia de seguridad en el almacenamiento local de DPM o MABS y cuándo la copia de seguridad en línea en Azure.
3. El disco de la carga de trabajo protegida se copia en los discos de DPM locales, y en Azure, según la programación especificada.
4. La copia de seguridad en línea en el almacén se gestiona mediante el agente de MARS que se ejecuta en el servidor de DPM o MABS.

![Copia de seguridad de máquinas o cargas de trabajo protegidas mediante DPM o MABS](./media/backup-architecture/architecture-dpm-mabs.png)







## <a name="azure-vm-storage"></a>Almacenamiento de máquinas virtuales de Azure

- Las máquinas virtuales de Azure usan discos para almacenar su sistema operativo, sus aplicaciones y sus datos.
- Las máquinas virtuales de Azure tienen al menos dos discos. Uno para el sistema operativo y un disco temporal. También pueden tener discos de datos para datos de aplicación. Los discos se almacenan como discos duros virtuales.
- Los discos duros virtuales se almacenan como blobs en páginas en Standard o Premium Storage en Azure.
    - Standard Storage: compatibilidad confiable y de bajo costo con máquinas virtuales que ejecutan cargas de trabajo que no son sensibles a la latencia. Standard Storage puede usar discos SSD estándar o discos HDD estándar.
    - Premium Storage: compatibilidad con discos de alto rendimiento. Usa discos SSD premium.
- Existen distintos niveles de rendimiento para los discos:
    - Disco HDD estándar: están respaldado por HDD y se usan cuando se busca un almacenamiento rentable.
    - Disco SSD estándar: combina elementos de los discos SSD premium y los discos HDD estándar, de forma que ofrece un rendimiento más coherente y mayor confiabilidad que los discos HDD, pero todavía resulta rentable.
    - Disco SSD premium: respaldado por SSD, proporciona alto rendimiento y baja latencia para máquinas virtuales que ejecutan cargas de trabajo que consumen elevada E/S.
- Los discos pueden ser administrados o no administrados:
    - Discos no administrados: tipos tradicionales de discos usados por las máquinas virtuales. Con estos discos, creará la propia cuenta de almacenamiento y especificará esa cuenta al crear el disco. Tiene que averiguar cómo maximizar los recursos de almacenamiento de las máquinas virtuales.
    - Discos administrados: Azure gestiona la creación y la administración de cuentas de almacenamiento en su nombre. Usted especificará el tamaño de disco y el nivel de rendimiento y Azure creará y administrará los discos en su nombre. Azure gestiona el almacenamiento a medida que agrega discos y escala máquinas virtuales.

Más información:

- Más información sobre el almacenamiento en disco para máquinas virtuales [Windows](../virtual-machines/windows/about-disks-and-vhds.md) y [Linux](../virtual-machines/linux/about-disks-and-vhds.md).
- Más información sobre [Standard](../virtual-machines/windows/standard-storage.md) y [Premium](../virtual-machines/windows/premium-storage.md) Storage.


### <a name="backing-up-and-restoring-azure-vms-with-premium-storage"></a>Copia de seguridad y restauración de máquinas virtuales de Azure con Premium Storage 

Puede realizar una copia de seguridad de máquinas virtuales de Azure mediante Premium Storage con Azure Backup:

- Durante la copia de seguridad de máquinas virtuales con Premium Storage, el servicio Backup crea una ubicación de almacenamiento provisional, llamada "AzureBackup-" en la cuenta de almacenamiento. El tamaño de la ubicación de ensayo equivale al de la instantánea del punto de recuperación.
- Asegúrese de que haya espacio disponible suficiente en la cuenta de Premium Storage para dar cabida a la ubicación de almacenamiento provisional. [Más información](../virtual-machines/windows/premium-storage.md#scalability-and-performance-targets). No modifique la ubicación de almacenamiento provisional.
- Una vez finalizado el trabajo de copia de seguridad, se elimina esta ubicación.
- El precio del almacenamiento usado para la ubicación de almacenamiento provisional es coherente con todos los [precios de Premium Storage](../virtual-machines/windows/premium-storage.md#pricing-and-billing).

Al restaurar máquinas virtuales de Azure mediante Premium Storage, puede restaurarlas a Premium Storage o Standard Storage. Lo habitual sería restaurarlas a Premium, pero podría ser rentable hacerlo a Standard si solo necesita un subconjunto de archivos de la máquina virtual.

### <a name="backing-up-and-restoring-managed-disks"></a>Copia de seguridad y restauración de discos administrados

Puede realizar copias de seguridad de máquinas virtuales de Azure con discos administrados.
- La copia de seguridad de máquinas virtuales con discos administrados se hace de la misma manera que con cualquier otra máquina virtual de Azure. Se puede hacer una copia de seguridad de la máquina virtual directamente desde la configuración de dicha máquina, o se puede habilitar la copia de seguridad para las máquinas virtuales en el almacén de Recovery Services.
- Puede realizar la copia de seguridad de máquinas virtuales en discos administrados mediante colecciones de RestorePoint basadas en discos administrados.
- Azure Backup también admite la realización de copias de seguridad de máquinas virtuales de discos administrados cifradas mediante Azure Disk Encryption (ADE).

Al restaurar máquinas virtuales con discos administrados, puede restaurarlas en una máquina virtual completa con discos administrados, o en una cuenta de almacenamiento.
- Azure gestiona los discos administrados durante el proceso de restauración y, con la opción de cuenta de almacenamiento, usted gestiona la cuenta de almacenamiento que se crea durante la restauración.
- Si restaura una máquina virtual administrada que está cifrada, los secretos y claves de máquina virtual deben salir del almacén de claves antes de iniciar el proceso de restauración.




## <a name="next-steps"></a>Pasos siguientes

- [Revise](backup-support-matrix.md) la matriz de compatibilidad para conocer las características y limitaciones admitidas en escenarios de copia de seguridad.
- Configure la copia de seguridad para uno de los siguientes escenarios:
    - [Copia de seguridad de máquinas virtuales de Azure](backup-azure-arm-vms-prepare.md)
    - [Copia de seguridad directa de máquinas Windows](tutorial-backup-windows-server-to-azure.md), sin un servidor de copia de seguridad.
    - [Configuración de MABS](backup-azure-microsoft-azure-backup.md) para la copia de seguridad en Azure y, luego, copia de seguridad de las cargas de trabajo en MABS.
    - [Configuración de DPM](backup-azure-dpm-introduction.md) para la copia de seguridad en Azure y, luego, copia de seguridad de las cargas de trabajo en DPM.


[green]: ./media/backup-architecture/green.png
[yellow]: ./media/backup-architecture/yellow.png
[red]: ./media/backup-architecture/red.png

