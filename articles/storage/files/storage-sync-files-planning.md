---
title: Planeamiento de una implementación de Azure File Sync |Microsoft Docs
description: Conozca los aspectos que debe tener en cuenta al planear una implementación de Azure Files.
author: roygara
ms.service: storage
ms.topic: conceptual
ms.date: 01/15/2020
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: 483603b8ff2f4b51f85d21d6ff4f02ad6f8a8272
ms.sourcegitcommit: 76bc196464334a99510e33d836669d95d7f57643
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/12/2020
ms.locfileid: "77162096"
---
# <a name="planning-for-an-azure-file-sync-deployment"></a>Planeamiento de una implementación de Azure Files Sync
Use Azure File Sync para centralizar los recursos compartidos de archivos de su organización en Azure Files sin renunciar a la flexibilidad, el rendimiento y la compatibilidad de un servidor de archivos local. Azure File Sync transforma Windows Server en una caché rápida de los recursos compartidos de archivos de Azure. Puede usar cualquier protocolo disponible en Windows Server para acceder a sus datos localmente, como SMB, NFS y FTPS. Puede tener todas las cachés que necesite en todo el mundo.

Este artículo describe las consideraciones importantes para una implementación de Azure File Sync. Se recomienda que lea [Planeamiento de una implementación de Azure Files](storage-files-planning.md). 

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="azure-file-sync-terminology"></a>Terminología de Azure File Sync
Antes de entrar en detalles para planear una implementación de Azure File Sync, es importante comprender la terminología.

### <a name="storage-sync-service"></a>Servicio de sincronización de almacenamiento
El servicio de sincronización de almacenamiento es el recurso de Azure de nivel superior para Azure File Sync. El recurso del servicio de sincronización de almacenamiento es un homólogo del recurso de la cuenta de almacenamiento, y se puede implementar igualmente en grupos de recursos de Azure. Se requiere un recurso de nivel superior distinto del recurso de la cuenta de almacenamiento dado que el servicio de sincronización de almacenamiento puede crear relaciones de sincronización con varias cuentas de almacenamiento mediante varios grupos de sincronización. Una suscripción puede tener implementados varios recursos del servicio de sincronización de almacenamiento.

### <a name="sync-group"></a>Grupo de sincronización
Un grupo de sincronización define la topología de sincronización de un conjunto de archivos. Los puntos de conexión dentro de un grupo de sincronización se mantienen sincronizados entre sí. Si, por ejemplo, tiene dos conjuntos distintos de archivos que desea administrar con Azure File Sync, podría crear dos grupos de sincronización y agregar a cada uno puntos de conexión diferentes. Un servicio de sincronización de almacenamiento puede hospedar tantos grupos de sincronización como sean necesarios.  

### <a name="registered-server"></a>Servidor registrado
El objeto de servidor registrado representa una relación de confianza entre el servidor (o clúster) y el servicio de sincronización de almacenamiento. Puede registrar tantos servidores como desee en una instancia del servicio de sincronización de almacenamiento. Sin embargo, un servidor (o clúster) se puede registrar con un solo servicio de sincronización de almacenamiento a la vez.

### <a name="azure-file-sync-agent"></a>Agente de Azure File Sync
El agente de Azure File Sync es un paquete descargable que permite la sincronización de Windows Server con un recurso compartido de archivos de Azure. El agente de Azure File Sync consta de tres componentes principales: 
- **FileSyncSvc.exe**: El servicio en segundo plano es responsable de supervisar los cambios en los puntos de conexión de servidor y de iniciar las sesiones de sincronización en Azure.
- **StorageSync.sys**: el filtro del sistema de archivos de Azure File Sync, responsable de apilar los archivos en Azure Files (cuando está habilitada la característica de nube por niveles).
- **Cmdlets de administración de PowerShell**: cmdlets de PowerShell para la interacción con el proveedor de recursos de Azure Microsoft.StorageSync. Puede encontrar estos elementos en las siguientes ubicaciones (valor predeterminado):
    - C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.PowerShell.Cmdlets.dll
    - C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.ServerCmdlets.dll

### <a name="server-endpoint"></a>Punto de conexión de servidor
Un punto de conexión de servidor representa una ubicación específica en un servidor registrado, como una carpeta en un volumen de servidor. Pueden existir varios puntos de conexión de servidor en el mismo volumen si sus espacios de nombres no se superponen (por ejemplo, `F:\sync1` y `F:\sync2`). Además, puede configurar directivas de niveles de nube de manera individual para cada punto de conexión de servidor. 

Puede crear un punto de conexión de servidor a través de un punto de montaje. Tenga en cuenta que se omiten los puntos de montaje en el punto de conexión de servidor.  

Puede crear un punto de conexión de servidor en el volumen del sistema, pero si lo hace existen dos limitaciones:
* No se pueden habilitar los niveles de nube.
* La restauración rápida del espacio de nombres (en la que el sistema desactiva todo el espacio de nombres y, a continuación, empieza a recuperar contenido) no se realiza.


> [!Note]  
> Solo se admiten volúmenes no extraíbles.  Las unidades asignadas desde un recurso compartido remoto no son compatibles con rutas de acceso de punto de conexión de servidor.  Además, un punto de conexión de servidor puede encontrarse en el volumen del sistema de Windows, aunque los niveles de nube no se admitan en dicho volumen.

Si agrega una ubicación de servidor con un conjunto de archivos existente como punto de conexión de servidor a un grupo de sincronización, esos archivos se combinarán con otros archivos que ya se encuentren en otros puntos de conexión del grupo de sincronización.

### <a name="cloud-endpoint"></a>Punto de conexión de nube
Un punto de conexión de nube es un recurso compartido de archivos de Azure que forma parte de un grupo de sincronización. El recurso compartido de archivos de Azure entero se sincroniza; un recurso compartido de archivos de Azure solo puede ser miembro de un punto de conexión de nube. Por lo tanto, un recurso compartido de archivos de Azure puede ser un miembro de un solo grupo de sincronización. Si agrega un recurso compartido de archivos de Azure con un conjunto de archivos existente como punto de conexión de nube a un grupo de sincronización, esos archivos se combinarán con otros archivos que ya se encuentren en otros puntos de conexión del grupo de sincronización.

> [!Important]  
> Azure File Sync admite realizar cambios directamente en el recurso compartido de archivos de Azure. Sin embargo, los cambios realizados en el recurso compartido de archivos de Azure primero deben ser detectados por un trabajo de detección de cambios de Azure File Sync. Se inicia un trabajo de detección de cambios para un punto de conexión en la nube solo una vez cada 24 horas. Además, los cambios realizados en un recurso compartido de archivos de Azure a través del protocolo de REST no actualizarán la hora de la última modificación de SMB y no se verán como cambios derivados de la sincronización. Para obtener más información, consulte [Preguntas más frecuentes de Azure Files](storage-files-faq.md#afs-change-detection).

### <a name="cloud-tiering"></a>Niveles de nube 
La nube por niveles es una característica opcional de Azure File Sync por la que los archivos a los que se tiene acceso con frecuencia se almacenan en caché localmente en el servidor mientras que todos los demás archivos se organizan en niveles en Azure Files, según la configuración de directiva. Para saber más, vea [Cloud Tiering Overview](storage-sync-cloud-tiering.md) (Introducción a nube por niveles).

## <a name="azure-file-sync-system-requirements-and-interoperability"></a>Requisitos del sistema de Azure File Sync e interoperabilidad 
En esta sección se tratan los requisitos del sistema del agente de Azure File Sync y la interoperabilidad con las características y roles de Windows Server y las soluciones de terceros.

### <a name="evaluation-cmdlet"></a>Cmdlet de evaluación
Antes de implementar Azure File Sync, debe evaluar si es compatible con el sistema mediante el cmdlet de evaluación de Azure File Sync. Este cmdlet busca posibles problemas con el sistema de archivos y el conjunto de datos, tales como caracteres no admitidos o una versión de sistema operativo no compatible. Las comprobaciones incluyen la mayoría de las características que se mencionan a continuación, pero no todas; se recomienda que lea el resto de esta sección detenidamente para asegurarse de que la implementación se realiza sin problemas. 

El cmdlet de evaluación se puede instalar mediante el módulo Az de PowerShell, que se puede instalar siguiendo estas instrucciones: [Instale y configure Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-Az-ps).

#### <a name="usage"></a>Uso  
Puede invocar la herramienta de evaluación de varias maneras diferentes: puede realizar las comprobaciones del sistema, las comprobaciones del conjunto de datos o ambas. Para realizar las comprobaciones del sistema y el conjunto de datos: 

```powershell
    Invoke-AzStorageSyncCompatibilityCheck -Path <path>
```

Para probar solo el conjunto de datos:
```powershell
    Invoke-AzStorageSyncCompatibilityCheck -Path <path> -SkipSystemChecks
```
 
Para probar solo los requisitos del sistema:
```powershell
    Invoke-AzStorageSyncCompatibilityCheck -ComputerName <computer name>
```
 
Para mostrar los resultados en CSV:
```powershell
    $errors = Invoke-AzStorageSyncCompatibilityCheck […]
    $errors | Select-Object -Property Type, Path, Level, Description | Export-Csv -Path <csv path>
```

### <a name="system-requirements"></a>Requisitos del sistema
- Un servidor que ejecute una de las siguientes versiones de sistema operativo:

    | Versión | SKU compatibles | Opciones de implementación compatibles |
    |---------|----------------|------------------------------|
    | Windows Server 2019 | Datacenter y Standard | Full y Core |
    | Windows Server 2016 | Datacenter y Standard | Full y Core |
    | Windows Server 2012 R2 | Datacenter y Standard | Full y Core |
    | Windows Server IoT 2019 para Storage| Datacenter y Standard | Full y Core |
    | Windows Storage Server 2016| Datacenter y Standard | Full y Core |
    | Windows Storage Server 2012 R2| Datacenter y Standard | Full y Core |

    Las versiones futuras de Windows Server se agregarán tan pronto como se publiquen.

    > [!Important]  
    > Se recomienda mantener sincronizadas todas las instancias que use con Azure File Sync con las actualizaciones más recientes de Windows Update. 

- Un servidor con un mínimo de 2 GiB de memoria.

    > [!Important]  
    > Si el servidor se ejecuta en una máquina virtual con la memoria dinámica habilitada, la máquina virtual debe configurarse con un mínimo de 2048 MiB de memoria.
    
- Un volumen asociado localmente con formato del sistema de archivos NTFS.

### <a name="file-system-features"></a>Características del sistema de archivos

| Característica | Compatibilidad con el estado | Notas |
|---------|----------------|-------|
| Listas de control de acceso (ACL) | totalmente compatible | Azure File Sync conserva las listas ACL de Windows y Windows Server las exige en los puntos de conexión de servidor. Las listas ACL de Windows no se admiten (aún) en Azure Files cuando se accede directamente a los archivos en la nube. |
| Vínculos físicos | Omitido | |
| Vínculos simbólicos | Omitido | |
| Puntos de montaje | Compatibilidad parcial | Los puntos de montaje podrían ser la raíz de un punto de conexión de servidor, pero se omiten si están incluidos en el espacio de nombres del punto de conexión de servidor. |
| Uniones | Omitido | Por ejemplo, las carpetas DfrsrPrivate y DFSRoots del Sistema de archivos distribuido. |
| Puntos de repetición de análisis | Omitido | |
| Compresión NTFS | totalmente compatible | |
| Archivos dispersos | totalmente compatible | Los archivos dispersos se sincronizan (no se bloquean), pero lo hacen con la nube como un archivo completo. Si se cambia el contenido del archivo en la nube (o en otro servidor), el archivo ya no estará disperso cuando el cambio se haya descargado. |
| Flujos de datos alternativos (ADS) | Conservados, pero no sincronizados | Por ejemplo, las etiquetas de clasificación creadas por la infraestructura de clasificación de archivos no están sincronizadas. Las etiquetas de clasificación existentes en los archivos en cada uno de los puntos de conexión del servidor se dejan como están. |

> [!Note]  
> Solo se admiten volúmenes NTFS. No se admiten ReFS, FAT, FAT32 ni otros sistemas de archivos.

### <a name="files-skipped"></a>Archivos omitidos

| Archivo/carpeta | Nota: |
|-|-|
| pagefile.sys | Archivo específico del sistema |
| Desktop.ini | Archivo específico del sistema |
| thumbs.db | Archivo temporal para miniaturas |
| ehthumbs.db | Archivo temporal para miniaturas de elementos multimedia |
| ~$\*.\* | Archivo temporal de Office |
| \*.tmp | Archivo temporal |
| \*.laccdb | Archivo de bloqueo de base de datos de Access|
| 635D02A9D91C401B97884B82B3BCDAEA.* | Archivo de sincronización interna|
| \\System Volume Information | Carpeta específica del volumen |
| $RECYCLE.BIN| Carpeta |
| \\SyncShareState | Carpeta para sincronización |

### <a name="failover-clustering"></a>Clústeres de conmutación por error
La característica de clústeres de conmutación por error de Windows es compatible con Azure File Sync en la opción de implementación "Servidor de archivos para uso general". La característica de clústeres de conmutación por error no se admite en "Servidor de archivos de escalabilidad horizontal para datos de aplicación" (SOFS) o en volúmenes compartidos de clúster (CSV).

> [!Note]  
> El agente de Azure File Sync debe estar instalado en cada nodo de un clúster de conmutación por error para que la sincronización funcione correctamente.

### <a name="data-deduplication"></a>Desduplicación de datos
**Windows Server 2016 y Windows Server 2019**   
La desduplicación de datos ahora es compatible en volúmenes con la nube por niveles habilitada en Windows Server 2016 y Windows Server 2019. Habilitar la desduplicación de datos en un volumen con la nube por niveles habilitada, le permite almacenar en caché más archivos en el entorno local sin necesidad de aprovisionar más almacenamiento. 

Cuando la desduplicación de datos está habilitada en un volumen con la nube por niveles habilitada, los archivos optimizados para desduplicación dentro de la ubicación del punto de conexión del servidor se organizan en niveles de forma similar a un archivo normal en función de la configuración de la directiva de la nube por niveles. Una vez que los archivos optimizados para la desduplicación se han organizado en niveles, el trabajo de recolección de elementos no utilizados de desduplicación de datos se ejecutará automáticamente para recuperar el espacio en disco mediante la eliminación de fragmentos innecesarios a los que ya no hacen referencia otros archivos del volumen.

Tenga en cuenta que el ahorro de volumen solo se aplica al servidor; los datos del recurso compartido de Azure no se desduplicarán.

> [!Note]  
> Para admitir la desduplicación de datos en volúmenes con una nube por niveles habilitada en Windows Server 2019, se debe instalar la actualización de Windows [KB4520062](https://support.microsoft.com/help/4520062) y se requiere el agente de Azure File Sync versión 9.0.0.0 o posterior.

**Windows Server 2012 R2**  
Azure File Sync no admite la desduplicación de datos y la nube por niveles en el mismo volumen en Windows Server 2012 R2. Si la desduplicación de datos está habilitada en un volumen, se debe deshabilitar la nube por niveles. 

**Notas**
- Si la desduplicación de datos está instalada antes de instalar el agente de Azure File Sync, es necesario reiniciar para que se admita en el mismo volumen la desduplicación de datos y la nube por niveles.
- Si habilita la desduplicación de datos en un volumen después de haber habilitado la nube por niveles, el trabajo inicial de optimización por desduplicación optimiza los archivos del volumen que no están aún en niveles, y tendrá la siguiente repercusión en la nube por niveles:
    - La directiva de espacio disponible seguirá colocando los archivos en niveles según el espacio libre en el volumen mediante el uso del mapa térmico.
    - La directiva de fecha omitirá la organización en niveles de los archivos, que podrían haber sido en otra situación aptos para niveles, ya que el trabajo de optimización por desduplicación tiene acceso a los archivos.
- Para los trabajos de optimización por desduplicación en curso, el valor de desduplicación de datos [MinimumFileAgeDays](https://docs.microsoft.com/powershell/module/deduplication/set-dedupvolume?view=win10-ps), retrasará la nube por niveles con directiva de fecha, si el archivo no está colocado ya en un nivel. 
    - Ejemplo: Si el valor MinimumFileAgeDays es de siete días y la directiva de fecha de nube por niveles es de 30 días, la directiva de fecha colocará los archivos en niveles pasados 37 días.
    - Nota: Una vez que Azure File Sync haya colocado un archivo en un nivel, el trabajo de optimización por desduplicación omitirá el archivo.
- Si un servidor que ejecuta Windows Server 2012 R2 y que tiene instalado el agente de Azure File Sync se actualiza a Windows Server 2016 o Windows Server 2019, es necesario realizar los pasos siguientes para que se pueda admitir en el mismo volumen la desduplicación de datos y la nube por niveles:  
    - Desinstalar al agente de Azure File Sync para Windows Server 2012 R2 y reiniciar el servidor.
    - Descargar al agente de Azure File Sync para la versión de sistema operativo del nuevo servidor (Windows Server 2016 o Windows Server 2019).
    - Instalar el agente de Azure File Sync y reiniciar el servidor.  
    
    Nota: Los valores de configuración de Azure File Sync en el servidor se conservan cuando el agente se desinstala y reinstala.

### <a name="distributed-file-system-dfs"></a>Sistema de archivos distribuido (DFS)
Azure File Sync admite la interoperabilidad con espacios de nombres DFS (DFS-N) y la replicación DFS (DFS-R).

**Espacios de nombres DFS (DFS-N)** : Azure File Sync es totalmente compatible con servidores de DFS-N. Puede instalar el agente de Azure File Sync en uno o varios miembros DFS-N para sincronizar datos entre los puntos de conexión del servidor y el punto de conexión en la nube. Para más información, consulte [Información general de Espacios de nombres DFS](https://docs.microsoft.com/windows-server/storage/dfs-namespaces/dfs-overview).
 
**Replicación DFS (DFS-R)** : puesto que DFS-R y Azure File Sync son soluciones de replicación, en la mayoría de los casos, se recomienda reemplazar DFS-R por Azure File Sync. Hay, sin embargo, varios escenarios donde puede que desee usar DFS-R y Azure File Sync conjuntamente:

- Va a migrar desde una implementación de DFS-R a una implementación de Azure File Sync. Para más información, consulte [Migrate a DFS Replication (DFS-R) deployment to Azure File Sync](storage-sync-files-deployment-guide.md#migrate-a-dfs-replication-dfs-r-deployment-to-azure-file-sync) (Migración de una implementación de la replicación DFS (DFS-R) a Azure File Sync).
- No todos los servidores locales que necesitan una copia de los datos de archivo pueden estar conectados directamente a Internet.
- Los servidores de sucursales consolidan los datos en un único servidor central, para el que le gustaría utilizar Servidores de sucursales consolidan los datos en un único servidor concentrador, le gustaría utilizar Azure File Sync.

Para que Azure File Sync y DFS-R trabajen en paralelo:

1. Los niveles de nube de Azure File Sync deben deshabilitarse en volúmenes con carpetas replicadas DFS-R.
2. Los puntos de conexión de servidor no se deben configurar en carpetas de replicación de solo lectura DFS-R.

Para más información, consulte [Introducción a Espacios de nombres DFS y Replicación DFS](https://technet.microsoft.com/library/jj127250).

### <a name="sysprep"></a>Sysprep
No se admite la ejecución de sysprep en un servidor que tenga instalado el agente de Azure File Sync y esto puede provocar resultados inesperados. La instalación del agente y el registro del servidor se deben realizar después de implementar la imagen del servidor y completar la instalación mínima de sysprep.

### <a name="windows-search"></a>Windows Search
Si en un punto de conexión de un servidor están habilitados los niveles en la nube, Windows Search omite y no indexa los archivos que están en capas. Los archivos que no están en capas se indexan correctamente.

### <a name="antivirus-solutions"></a>Soluciones antivirus
Dado que un antivirus funciona examinando los archivos en busca de código malintencionado conocido, puede provocar la recuperación de archivos con niveles. En las versiones 4.0 y posteriores del agente de Azure File Sync, los archivos en niveles tienen establecido el atributo seguro de Windows FILE_ATTRIBUTE_RECALL_ON_DATA_ACCESS. Se recomienda consultar con el proveedor de software cómo configurar su solución para omitir la lectura de archivos que tengan establecido este atributo (muchas realizan la omisión automáticamente). 

Las soluciones de antivirus internas de Microsoft, Windows Defender y System Center Endpoint Protection (SCEP), omiten de forma automática la lectura de archivos que tienen establecido dicho atributo. Hemos probado ambas soluciones e identificamos un problema menor: al agregar un servidor a un grupo de sincronización existente, se recuperan (descargan) los archivos de menos de 800 bytes en el nuevo servidor. Estos archivos permanecerán en el nuevo servidor y no se organizarán en niveles ya que no cumplen con el requisito de tamaño de niveles (>64 kb).

> [!Note]  
> Los proveedores de software antivirus pueden comprobar la compatibilidad entre sus productos y Azure File Sync con [Azure File Sync Antivirus Compatibility Test Suite](https://www.microsoft.com/download/details.aspx?id=58322), que está disponible para su descarga en el Centro de descarga de Microsoft.

### <a name="backup-solutions"></a>Soluciones de copia de seguridad
Al igual que sucede con las soluciones antivirus, las soluciones de backup pueden provocar la recuperación de archivos con niveles. Se recomienda usar una solución de backup en la nube para realizar la copia de seguridad del recurso compartido de archivos de Azure en lugar de usar un producto de backup local.

Si está utilizando una solución de copia de seguridad local, las copias de seguridad deben realizarse en un servidor del grupo de sincronización que tenga los niveles en la nube deshabilitados. Al realizar una restauración, use las opciones de restauración de nivel de volumen o de archivo. Los archivos restaurados con la opción de restauración a nivel de archivo se sincronizarán con todos los puntos de conexión del grupo de sincronización y los archivos existentes se reemplazarán con la versión restaurada desde la copia de seguridad.  Las restauraciones a nivel de volumen no reemplazarán las versiones de archivo más recientes en el recurso compartido de archivos de Azure u otros puntos de conexión del servidor.

> [!Note]  
> La reconstrucción completa (BMR) puede causar resultados inesperados y actualmente no se admite.

> [!Note]  
> Con la versión 9 del agente de Azure File Sync, las instantáneas de VSS (incluida la pestaña Versiones anteriores) ya se admiten en los volúmenes que tienen habilitada la nube por niveles. Sin embargo, debe habilitar la compatibilidad con la versión anterior a través de PowerShell. [Más información](storage-files-deployment-guide.md).

### <a name="encryption-solutions"></a>Soluciones de cifrado
La compatibilidad con soluciones de cifrado depende de cómo se implementen. Está comprobado que Azure File Sync funciona con:

- Cifrado de BitLocker
- Azure Information Protection, Azure Rights Management Services (Azure RMS) y Active Directory RMS

está comprobado que Azure File Sync no funciona con:

- Sistema de cifrado de archivos NTFS (EFS)

En general, Azure File Sync debe admitir la interoperabilidad con soluciones de cifrado que se encuentran a continuación en el sistema de archivos, como BitLocker, y con las soluciones que se implementan en el formato de archivo, como Azure Information Protection. No se ha realizado ninguna interoperabilidad especial para las soluciones que se encuentran por encima del sistema de archivos (por ejemplo, EFS de NTFS).

### <a name="other-hierarchical-storage-management-hsm-solutions"></a>Otras soluciones de administración de almacenamiento jerárquico (HSM)
No deben utilizarse otras soluciones HSM deben utilizarse con Azure File Sync.

## <a name="region-availability"></a>Disponibilidad en regiones
Azure File Sync solo está disponible en las siguientes regiones:

| Region | Ubicación de centro de datos |
|--------|---------------------|
| Este de Australia | Nueva Gales del Sur |
| Sudeste de Australia | Victoria |
| Sur de Brasil | Estado de Sao Paulo |
| Centro de Canadá | Toronto |
| Este de Canadá | Ciudad de Quebec |
| Centro de la India | Pune |
| Centro de EE. UU. | Iowa |
| Este de Asia | RAE de Hong Kong |
| Este de EE. UU. | Virginia |
| Este de EE. UU. 2 | Virginia |
| Centro de Francia | Paris |
| Sur de Francia* | Marsella |
| Centro de Corea del Sur | Seúl |
| Corea del Sur | Busán |
| Japón Oriental | Tokio, Saitama |
| Japón Occidental | Osaka |
| Centro-Norte de EE. UU | Illinois |
| Norte de Europa | Irlanda |
| Norte de Sudáfrica | Johannesburgo |
| Oeste de Sudáfrica* | Ciudad del Cabo |
| Centro-sur de EE. UU. | Texas |
| Sur de la India | Chennai |
| Sudeste de Asia | Singapur |
| Sur de Reino Unido 2 | London |
| Oeste de Reino Unido | Cardiff |
| US Gov: Arizona | Arizona |
| US Gov Texas | Texas |
| US Gov - Virginia | Virginia |
| Norte de Emiratos Árabes Unidos | Dubai |
| Centro de Emiratos Árabes Unidos* | Abu Dabi |
| Oeste de Europa | Países Bajos |
| Centro-Oeste de EE. UU. | Wyoming |
| Oeste de EE. UU. | California |
| Oeste de EE. UU. 2 | Washington |

Azure File Sync solo se admite la sincronización con recursos compartidos de archivos de Azure de la misma región que el servicio de sincronización de almacenamiento.

En el caso de las regiones marcadas con asteriscos, debe ponerse en contacto con el soporte técnico de Azure para solicitar acceso a Azure Storage. El proceso se describe más adelante en[ este documento](https://azure.microsoft.com/global-infrastructure/geographies/).

### <a name="azure-disaster-recovery"></a>Azure Disaster Recovery
Para protegerse contra la pérdida de una región de Azure, Azure File Sync se integra con el [almacenamiento con redundancia geográfica](../common/storage-redundancy.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json) (GRS). El almacenamiento GRS utiliza la replicación asincrónica de bloques entre el almacenamiento en la región primaria, con la que normalmente se interactúa, y el almacenamiento en la región secundaria emparejada. En caso de desastre que haga que una región de Azure se desconecte temporal o permanentemente, Microsoft conmutará por error el almacenamiento a la región emparejada. 

> [!Warning]  
> Si usa el recurso compartido de archivos de Azure como punto de conexión en la nube en una cuenta de almacenamiento GRS, no debe iniciar la conmutación por error de la cuenta de almacenamiento. Si lo hace, la sincronización dejará de funcionar y también podría provocar una pérdida inesperada de datos en el caso de archivos recién organizados en capas. En caso de pérdida de una región de Azure, Microsoft activará la conmutación por error de la cuenta de almacenamiento de forma que sea compatible con Azure File Sync.

Para admitir la integración de la conmutación por error entre el almacenamiento con redundancia geográfica y Azure File Sync, todas las regiones de Azure File Sync se emparejan con una región secundaria que coincide con la región secundaria que el almacenamiento utiliza. Se emparejan de la siguiente manera:

| Región primaria      | Región emparejada      |
|---------------------|--------------------|
| Este de Australia      | Sudeste de Australia|
| Sudeste de Australia | Este de Australia     |
| Sur de Brasil        | Centro-sur de EE. UU.   |
| Centro de Canadá      | Este de Canadá        |
| Este de Canadá         | Centro de Canadá     |
| Centro de la India       | Sur de la India        |
| Centro de EE. UU.          | Este de EE. UU. 2          |
| Este de Asia           | Sudeste de Asia     |
| Este de EE. UU.             | Oeste de EE. UU.            |
| Este de EE. UU. 2           | Centro de EE. UU.         |
| Centro de Francia      | Sur de Francia       |
| Sur de Francia        | Centro de Francia     |
| Japón Oriental          | Japón Occidental         |
| Japón Occidental          | Japón Oriental         |
| Centro de Corea del Sur       | Corea del Sur        |
| Corea del Sur         | Centro de Corea del Sur      |
| Norte de Europa        | Oeste de Europa        |
| Centro-Norte de EE. UU    | Centro-sur de EE. UU.   |
| Norte de Sudáfrica  | Oeste de Sudáfrica  |
| Oeste de Sudáfrica   | Norte de Sudáfrica |
| Centro-sur de EE. UU.    | Centro-Norte de EE. UU   |
| Sur de la India         | Centro de la India      |
| Sudeste de Asia      | Este de Asia          |
| Sur de Reino Unido 2            | Oeste de Reino Unido            |
| Oeste de Reino Unido             | Sur de Reino Unido 2           |
| US Gov: Arizona      | US Gov Texas       |
| US Gov Iowa         | US Gov - Virginia    |
| US Gov - Virginia      | US Gov Texas       |
| Oeste de Europa         | Norte de Europa       |
| Centro-Oeste de EE. UU.     | Oeste de EE. UU. 2          |
| Oeste de EE. UU.             | Este de EE. UU.            |
| Oeste de EE. UU. 2           | Centro-Oeste de EE. UU.    |

## <a name="azure-file-sync-agent-update-policy"></a>Directiva de actualización del agente de Azure File Sync
[!INCLUDE [storage-sync-files-agent-update-policy](../../../includes/storage-sync-files-agent-update-policy.md)]

## <a name="recommended-azure-file-sync-machine-configuration"></a>Configuración de máquina Azure File Sync recomendada

Los requisitos de la máquina Azure File Sync se determinan por el número de objetos en el espacio de nombres y la renovación en el conjunto de datos. Se puede adjuntar un solo servidor a varios grupos de sincronización y el número de objetos enumerados en la tabla siguiente tiene en cuenta el espacio de nombres completo al que está asociado un servidor. Por ejemplo, punto de conexión del servidor A con 10 millones de objetos + punto de conexión de servidor B con 10 millones de objetos = 20 millones de objetos. Para esa implementación de ejemplo, se recomienda 8 CPU y 16 GiB de memoria para el estado estable y, si es posible, 48 GiB de memoria para la migración inicial.
 
Los datos del espacio de nombres se almacenan en la memoria por motivos de rendimiento. Por eso, los espacios de nombres más grandes requieren más memoria para mantener un buen rendimiento, y una mayor renovación requiere más CPU para procesar. 
 
En la tabla siguiente, se proporciona tanto el tamaño del espacio de nombres como una conversión a capacidad para los recursos compartidos de archivos de uso general típicos, donde el tamaño medio de los archivos es de 512 KiB. Si el tamaño de los archivos es menor, considere la posibilidad de agregar memoria adicional para la misma cantidad de capacidad. Base la configuración de memoria en el tamaño del espacio de nombres.

| Tamaño del espacio de nombres: archivos y directorios (millones)  | Capacidad típica (TiB)  | Núcleos de CPU  | Memoria recomendada (GiB) |
|---------|---------|---------|---------|
| 3        | 1.4     | 2        | 8 (sincronización inicial)/2 (renovación típica)      |
| 5        | 2.4     | 2        | 16 (sincronización inicial)/4 (renovación típica)    |
| 10       | 4.8     | 4        | 32 (sincronización inicial)/8 (renovación típica)   |
| 30       | 14.3    | 8        | 48 (sincronización inicial)/16 (renovación típica)   |
| 50       | 23.8    | 16       | 64 (sincronización inicial)/32 (renovación típica)  |
| 100*     | 47.7   | 32       | 128 (sincronización inicial)/32 (renovación típica)  |

\*No se han probado más de 100 millones de archivos y directorios. Se trata de un límite flexible.

> [!TIP]
> La sincronización inicial de un espacio de nombres es una operación intensiva y se recomienda asignar más memoria hasta que se complete la sincronización inicial. Esto no es necesario, pero puede acelerar la sincronización inicial. 
> 
> La renovación típica es el 0,5 % del espacio de nombres que cambia por día. Para mayores niveles de renovación, considere la posibilidad de agregar más CPU. 

## <a name="next-steps"></a>Pasos siguientes
* [Tenga en cuenta los valores de proxy y firewall](storage-sync-files-firewall-and-proxy.md)
* [Planeamiento de una implementación de Azure Files](storage-files-planning.md)
* [Implementación de Azure Files](storage-files-deployment-guide.md)
* [Implementación de Azure File Sync](storage-sync-files-deployment-guide.md)
* [Supervisión de Azure File Sync](storage-sync-files-monitoring.md)
