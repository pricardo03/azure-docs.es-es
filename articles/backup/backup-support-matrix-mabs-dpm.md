---
title: Matriz de compatibilidad de MABS y System Center DPM
description: En este artículo se resume la compatibilidad de Azure Backup al usar Microsoft Azure Backup Server (MABS) o System Center DPM para realizar copias de seguridad de recursos locales y de VM de Azure.
ms.date: 02/17/2019
ms.topic: conceptual
ms.openlocfilehash: 2bec2818eaabaa2d2d74ab7181db0eabcba092ec
ms.sourcegitcommit: 4821b7b644d251593e211b150fcafa430c1accf0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/19/2019
ms.locfileid: "74172048"
---
# <a name="support-matrix-for-backup-with-microsoft-azure-backup-server-or-system-center-dpm"></a>Matriz de compatibilidad para copia de seguridad con Microsoft Azure Backup Server o System Center DPM

Puede usar el [servicio Azure Backup](backup-overview.md) para realizar copias de seguridad de cargas de trabajo y máquinas locales y de las máquinas virtuales (VM) de Azure. En este artículo se resumen la configuración y las limitaciones de compatibilidad para realizar copias de seguridad de máquinas con Microsoft Azure Backup Server (MABS) o System Center Data Protection Manager (DPM) y Azure Backup.

## <a name="about-dpmmabs"></a>Acerca de DPM/MABS

[System Center DPM](https://docs.microsoft.com/system-center/dpm/dpm-overview?view=sc-dpm-1807) es una solución empresarial que configura, facilita y administra la copia de seguridad y recuperación de máquinas y datos corporativos. Forma parte del conjunto de productos de [System Center](https://www.microsoft.com/cloud-platform/system-center-pricing).

MABS es un producto de servidor que se puede usar para realizar una copia de seguridad de servidores físicos locales, máquinas virtuales y aplicaciones que se ejecutan en ellos.

MABS se basa en System Center DPM y proporciona una funcionalidad similar con unas cuantas diferencias:

- No se requiere ninguna licencia de System Center para ejecutar MABS.
- Azure proporciona almacenamiento de copia de seguridad a largo plazo tanto para MABS como para DPM. Además, DPM permite realizar una copia de seguridad de los datos para su almacenamiento en cinta a largo plazo. MABS no proporciona esta funcionalidad.
- Puede realizar una copia de seguridad de un servidor DPM principal con un servidor DPM secundario. El servidor secundario protegerá la base de datos del servidor principal y las réplicas del origen de datos almacenadas en el servidor principal. Si se produce un error en el servidor principal, el servidor secundario puede continuar protegiendo las cargas de trabajo que protege el servidor principal, hasta que este vuelva a estar disponible.  MABS no proporciona esta funcionalidad.

Puede descargar MABS del [Centro de descarga de Microsoft](https://www.microsoft.com/download/details.aspx?id=57520). Se puede ejecutar de forma local o en una máquina virtual de Azure.

DPM y MABS admiten la copia de seguridad de una amplia variedad de aplicaciones y sistemas operativos cliente y de servidor. Proporcionan varios escenarios de copia de seguridad:

- Puede realizar una copia de seguridad a nivel de máquina con copia de seguridad de reconstrucción completa o de estado del sistema.
- Puede realizar copias de seguridad de volúmenes, recursos compartidos, carpetas y archivos específicos.
- Puede realizar una copia de seguridad de aplicaciones específicas con una configuración optimizada basada en aplicaciones.

## <a name="dpmmabs-backup"></a>Copia de seguridad de DPM/MABS

La copia de seguridad con DPM/MABS y Azure Backup funciona como se explica a continuación:

1. El agente de protección de DPM/MABS se instala en cada máquina de la que se realizará una copia de seguridad.
1. Se realiza una copia de seguridad de máquinas y aplicaciones en un almacenamiento local en DPM/MABS.
1. El agente de Microsoft Azure Recovery Services (MARS) se instala en el servidor de DPM/MABS.
1. El agente de MARS realiza una copia de seguridad de los discos de DPM/MABS en un almacén de Recovery Services de copia de seguridad en Azure mediante Azure Backup.

Para obtener más información:

- [Obtenga más información](backup-architecture.md#architecture-back-up-to-dpmmabs) sobre la arquitectura de MABS.
- [Revise qué es compatible](backup-support-matrix-mars-agent.md) para el agente de MARS.

## <a name="supported-scenarios"></a>Escenarios admitidos

**Escenario** | **Agent** | **Ubicación**
--- | --- | ---
**Copia de seguridad de cargas de trabajo/máquinas locales** | El agente de protección de DPM/MABS se ejecuta en las máquinas de las que desea realizar una copia de seguridad.<br/><br/> El agente de MARS en el servidor de DPM/MABS.<br/> La versión mínima del agente de Microsoft Azure Recovery Services, o agente de Azure Backup, que se requiere para habilitar esta característica es 2.0.8719.0.  | DPM/MABS debe ejecutarse en el entorno local.

## <a name="supported-deployments"></a>Implementaciones admitidas

DPM/MABS puede implementarse como se resume en la siguiente tabla.

**Implementación** | **Soporte técnico** | **Detalles**
--- | --- | ---
**Implementación local** | Servidor físico<br/><br/>Máquina virtual de Hyper-V<br/><br/> Máquina virtual de VMware | Si DPM/MABS está instalado como una máquina virtual de VMware, solo realiza una copia de seguridad de las máquinas virtuales de VMware y las cargas de trabajo que se ejecutan en dichas máquinas virtuales.
**Implementación como una máquina virtual de Azure Stack** | Solo MABS | DPM no puede utilizarse para realizar una copia de seguridad de máquinas virtuales de Azure Stack.
**Implementación como una máquina virtual de Azure** | Protege máquinas virtuales de Azure y las cargas de trabajo que se ejecutan en esas máquinas virtuales. | DPM/MABS en Azure no puede realizar una copia de seguridad de las máquinas locales.

## <a name="supported-mabs-and-dpm-operating-systems"></a>Sistemas operativos de MABS y DPM compatibles

Azure Backup puede realizar una copia de seguridad de instancias de DPM/MABS en cualquiera de los siguientes sistemas operativos. Los sistemas operativos deben ejecutar los últimos Service Pack y actualizaciones.

**Escenario** | **DPM/MABS**
--- | ---
**MABS en una máquina virtual de Azure** | Windows Server 2012 R2.<br/><br/> Windows 2016 Datacenter.<br/><br/> Windows 2019 Datacenter.<br/><br/> Se recomienda comenzar con una imagen de Marketplace.<br/><br/> A2 estándar como mínimo con dos núcleos y 3,5 GB de RAM.
**DPM en una máquina virtual de Azure** | System Center 2012 R2 con Update 3 o posterior.<br/><br/> Sistema operativo Windows [según requiera System Center](https://docs.microsoft.com/system-center/dpm/prepare-environment-for-dpm?view=sc-dpm-1807#dpm-server).<br/><br/> Se recomienda comenzar con una imagen de Marketplace.<br/><br/> A2 estándar como mínimo con dos núcleos y 3,5 GB de RAM.
**MABS local** | Sistemas operativos compatibles de 64 bits:<br/><br/> MABS v3 y versiones posteriores: Windows Server 2019 (Standard, Datacenter y Essentials). <br/><br/> MABS v2 y versiones posteriores: Windows Server 2016 (Standard, Datacenter y Essentials).<br/><br/> Todas las versiones de MABS:  Windows Server 2012 R2.<br/><br/>Todas las versiones de MABS: Windows Storage Server 2012 R2.
**DPM local** | Servidor físico/máquina virtual de Hyper-V: System Center 2012 SP1 o posterior.<br/><br/> Máquina virtual de VMware: System Center 2012 R2 con Update 5 o posterior.

## <a name="management-support"></a>Compatibilidad con la administración

**Problema** | **Detalles**
--- | ---
**Instalación** | Instale DPM/MABS en una máquina de propósito único.<br/><br/> No instale DPM/MABS en un controlador de dominio, en una máquina con la instalación del rol de servidor de aplicaciones, en una máquina que ejecuta Microsoft Exchange Server o System Center Operations Manager o en un nodo de clúster.<br/><br/> [Revise todos los requisitos del sistema DPM](https://docs.microsoft.com/system-center/dpm/prepare-environment-for-dpm?view=sc-dpm-1807#dpm-server).
**Dominio** | DPM/MABS debe estar unido a un dominio. En primero lugar, instale DPM/MABS y, después, únalo a un dominio. Mueva DPM/MABS a un dominio nuevo después de que no se admita la implementación.
**Storage** | Modern Backup Storage (MBS) se admite desde DPM 2016/MABS v2 y versiones posteriores. No está disponible para MABS v1.
**Actualización de MABS** | Puede instalar MABS v3 directamente o actualizar a MABS v3 desde MABS v2. [Más información](backup-azure-microsoft-azure-backup.md#upgrade-mabs).
**Mover MABS** | Se admite la transferencia de MABS a un nuevo servidor mientras se conserva el almacenamiento si usa MBS.<br/><br/> El servidor debe tener el mismo nombre que el original. No se puede cambiar el nombre si desea mantener el mismo grupo de almacenamiento y usar la misma base de datos de MABS para almacenar puntos de recuperación de datos.<br/><br/> Necesitará una copia de seguridad de la base de datos de MABS, ya que necesitará restaurarla.

## <a name="mabs-support-on-azure-stack"></a>Compatibilidad con MABS en Azure Stack

Puede implementar MABS en una máquina virtual de Azure Stack para poder administrar la copia de seguridad de máquinas virtuales de Azure Stack y las cargas de trabajo desde una sola ubicación.

**Componente** | **Detalles**
--- | ---
**MABS en máquinas virtuales de Azure Stack** | Tamaño mínimo de A2. Se recomienda que empiece con una imagen de Windows Server 2012 R2 o Windows Server 2016 de Azure Marketplace.<br/><br/> No instale ningún otro elemento en la máquina virtual de MABS.
**Almacenamiento de MABS** | Use una cuenta de almacenamiento independiente para la máquina virtual de MABS. El agente de MARS en MABS necesita almacenamiento temporal para una ubicación de caché, así como para hospedar los datos restaurados desde la nube.
**Grupo de almacenamiento de MABS** | El tamaño del grupo de almacenamiento de MABS lo determinan el número y tamaño de los discos que están conectados a la máquina virtual de MABS. Cada tamaño de máquina virtual de Azure Stack tiene un número máximo de discos. Por ejemplo, A2 son cuatro discos.
**Retención de MABS** | No conserve los datos de copia de seguridad de los discos locales de MABS durante más de cinco días.
**Escalado vertical de MABS** | Para escalar verticalmente la implementación, puede aumentar el tamaño de la máquina virtual de MABS. Por ejemplo, puede cambiar de la serie A a la D.<br/><br/> También puede asegurarse de estar descargando datos con copia de seguridad en Azure de forma regular. Si es necesario, puede implementar servidores MABS adicionales.
**.NET Framework en MABS** | La máquina virtual de MABS necesita tener instalado .NET Framework 3.3 SP1 o una versión posterior.
**Dominio de MABS** | La máquina virtual de MABS debe estar unida a un dominio. El usuario de un dominio con privilegios de administrador debe instalar MABS en la máquina virtual.
**Copia de seguridad de datos de máquina virtual de Azure Stack** | Puede hacer copias de seguridad de archivos, carpetas y aplicaciones.
**Copia de seguridad compatible** | Los sistemas operativos compatibles para las máquinas virtuales de las que desea realizar una copia de seguridad son:<br/><br/> Canal semianual de Windows Server (Datacenter, Enterprise y Standard)<br/><br/> Windows Server 2016, Windows Server 2012 R2, Windows Server 2008 R2
**Compatibilidad de SQL Server para máquinas virtuales de Azure Stack** | Realice una copia de seguridad de SQL Server 2016, SQL Server 2014 y SQL Server 2012 SP1.<br/><br/> Realice una copia de seguridad y recuperación de una base de datos.
**Compatibilidad de SharePoint para máquinas virtuales de Azure Stack** | SharePoint 2016, SharePoint 2013 y SharePoint 2010.<br/><br/> Realice una copia de seguridad y recuperación de una granja, una base de datos, un front-end y un servidor web.
**Requisitos de red para máquinas virtuales con copia de seguridad** | Todas las máquinas virtuales de carga de trabajo de Azure Stack deben pertenecer a la misma red virtual y a la misma suscripción.

## <a name="dpmmabs-networking-support"></a>Compatibilidad de red con DPM/MABS

### <a name="url-access"></a>Acceso a direcciones URL

El servidor DPM/MABS también necesita acceder a estas direcciones URL:

- http://www.msftncsi.com/ncsi.txt
- *.Microsoft.com
- *.WindowsAzure.com
- *.microsoftonline.com
- \* .windows.net

### <a name="dpmmabs-connectivity-to-azure-backup"></a>Conectividad DPM/MABS a Azure Backup

Se requiere conectividad al servicio Azure Backup para que las copias de seguridad funcionen correctamente, y la suscripción de Azure debe estar activa. En la tabla siguiente se muestra el comportamiento si no se cumplen estos dos requisitos.

**MABS en Azure** | **Suscripción** | **Copia de seguridad/Restauración**
--- | --- | ---
Conectado | Active | Copia de seguridad en disco DPM/MABS.<br/><br/> Copia de seguridad en Azure.<br/><br/> Restauración desde disco.<br/><br/> Restauración desde Azure.
Conectado | Expirada/desaprovisionada | Ninguna copia de seguridad en disco o en Azure.<br/><br/> Si la suscripción ha expirado, puede restaurar desde el disco o Azure.<br/><br/> Si la suscripción se ha retirado, no puede restaurar desde el disco o Azure. Los puntos de recuperación de Azure se han eliminado.
No hay conectividad durante más de 15 días | Active | Ninguna copia de seguridad en disco o en Azure.<br/><br/> Puede restaurar desde el disco o Azure.
No hay conectividad durante más de 15 días | Expirada/desaprovisionada | Ninguna copia de seguridad en disco o en Azure.<br/><br/> Si la suscripción ha expirado, puede restaurar desde el disco o Azure.<br/><br/> Si la suscripción se ha retirado, no puede restaurar desde el disco o Azure. Los puntos de recuperación de Azure se han eliminado.

## <a name="dpmmabs-storage-support"></a>Compatibilidad con almacenamiento en DPM/MABS

Los datos de los que se ha realizado una copia de seguridad en DPM/MABS se almacenan en el almacenamiento en disco local.

**Storage** | **Detalles**
--- | ---
**MBS** | Modern Backup Storage (MBS) se admite desde DPM 2016/MABS v2 y versiones posteriores. No está disponible para MABS v1.
**Almacenamiento de MABS en máquina virtual de Azure** | Los datos se almacenan en discos de Azure que están conectados a la máquina virtual de DPM/MABS y que se administran en DPM/MABS. El número de discos que se pueden usar para el grupo de almacenamiento de DPM/MABS está limitado por el tamaño de la máquina virtual.<br/><br/> VM A2: 4 discos; VM A3: 8 discos; VM A4: 16 discos con un tamaño máximo de 1 TB para cada disco. Esto determina el grupo de almacenamiento de copia de seguridad total que se encuentra disponible.<br/><br/> La cantidad de datos de los que puede realizar copias de seguridad depende del número y tamaño de los discos conectados.
**Retención de datos de MABS en un máquina virtual de Azure** | Se recomienda conservar datos durante un día en el disco de Azure de DPM/MABS y realizar copias de seguridad de DPM/MABS en el almacén para una retención más prolongada. Por tanto, puede proteger un volumen de datos mayor si lo descarga en Azure Backup.

### <a name="modern-backup-storage-mbs"></a>Modern Backup Storage (MBS)

A partir de DPM 2016/MABS v2 (en Windows Server 2016) y versiones posteriores, puede beneficiarse de Modern Backup Storage (MBS).

- Las copias de seguridad de MBS se almacenan en un disco del Sistema de archivos resistente (ReFS).
- MBS usa la clonación del bloque de ReFS para una copia de seguridad más rápida y un uso más eficaz del espacio de almacenamiento.
- Cuando agrega volúmenes al grupo de almacenamiento local de DPM/MABS, se configuran con letras de unidad. Después, puede configurar el almacenamiento de la carga de trabajo en distintos volúmenes.
- Al crear grupos de protección para realizar copias de seguridad de los datos en DPM/MABS, seleccione la unidad que desea usar. Por ejemplo, podría almacenar las copias de seguridad de SQL u otras cargas de trabajo con un número elevado de IOPS en una unidad de rendimiento alto y las cargas de trabajo de las que se realiza una copia de seguridad con menos frecuencia en una unidad de rendimiento más bajo.

## <a name="supported-backups-to-mabs"></a>Copias de seguridad admitidas en MABS

En la tabla siguiente se resume de lo que se puede realizar una copia de seguridad en MABS desde máquinas locales y máquinas virtuales de Azure.

**Copia de seguridad** | **Versiones** | **MABS** | **Detalles** |
--- | --- | --- | --- |
**Windows 10<br/>Windows 8.1<br/>Windows 8<br/>Windows 7**<br/><br/>(32/64 bits) | MABS v3, v2 | Local. | Volumen, recurso compartido, carpeta y archivo.<br/><br/> Volúmenes desduplicados admitidos.<br/><br/> Los volúmenes deben ser de al menos 1 GB y NTFS. |
**Windows Server 2016 (Datacenter, Standard, no Nano)**<br/><br/> 64/32 bits | MABS v3, v2 | Máquina virtual local y en Azure.| Volumen, recurso compartido, carpeta, archivo, estado del sistema y reconstrucción completa.<br/><br/> Volúmenes desduplicados admitidos. |
**Windows Server 2012 R2 (Datacenter y Standard)**<br/><br/> 64/32 bits | MABS v3, v2 | Máquina virtual local y en Azure. | **Protección local**: Volumen, recurso compartido, carpeta, archivo, estado del sistema y reconstrucción completa.<br/><br/> **Protección de máquina virtual de Azure**: Volumen, recurso compartido, carpeta y archivo.<br/><br/> Volúmenes desduplicados admitidos. |
**Windows Server 2012 con SP1 (Datacenter y Standard)**<br/><br/> 64/32 bits | MABS v3, v2 <br/><br/> [Windows Management Framework 4.0](https://www.microsoft.com/download/details.aspx?id=40855) debe estar instalado. | Máquina virtual local y en Azure. | **Protección local**: Volumen, recurso compartido, carpeta, archivo, estado del sistema y reconstrucción completa.<br/><br/> **Protección de máquina virtual de Azure**: Volumen, recurso compartido, carpeta y archivo.<br/><br/> Volúmenes desduplicados admitidos. |
**Windows 2008 R2 con SP1 (Standard y Enterprise)**<br/><br/> 64/32 bits | Compatible con MABS v3, v2.<br/><br/> [Windows Management Framework 4.0](https://www.microsoft.com/download/details.aspx?id=40855) debe estar instalado. | Máquina virtual local y en Azure. |   **Protección local**: Volumen, recurso compartido, carpeta, archivo, estado del sistema y reconstrucción completa.<br/><br/> **Protección de máquina virtual de Azure**: Volumen, recurso compartido, carpeta y archivo.<br/><br/> Volúmenes desduplicados admitidos. |
**Windows 2008 R2 (Standard y Enterprise)**<br/><br/> 64/32 bits | Para MABS v3 o v2, el sistema operativo debe ejecutar SP1. | Máquina virtual local y en Azure. | **Protección local**: Volumen, recurso compartido, carpeta, archivo, estado del sistema y reconstrucción completa.<br/><br/> **Protección de máquina virtual de Azure**: Volumen, recurso compartido, carpeta y archivo.<br/><br/> Volúmenes desduplicados admitidos. |
**Windows Server 2008 con SP2**<br/><br/> 64/32 bits | MABS v3, v2 | MABS v2 y v3 es compatible cuando MABS se implementa como una máquina virtual de VMware.<br/><br/> No se admite para MABS en ejecución en máquinas virtuales de Azure. | Volumen, recurso compartido, carpeta, archivo, estado del sistema y reconstrucción completa. |
**Windows Storage Server 2008** | MABS v3, v2 | MABS como servidor físico local/máquina virtual de Hyper-V. <br/><br/> No se admite para MABS en ejecución en máquinas virtuales de Azure. | Volumen, recurso compartido, carpeta, archivo, estado del sistema y reconstrucción completa.
**SQL Server 2017** | MABS v3 | Máquina virtual local y en Azure.| Copia de seguridad de base de datos SQL Server.<br/><br/> Copia de seguridad del clúster de SQL Server admitida.<br/><br/>No se admiten las bases de datos almacenadas en volúmenes compartidos de clúster (CSV). |
**SQL Server 2016/2016 con SP1** | MABS v3, v2 | Máquina virtual local y en Azure.| Copia de seguridad de base de datos SQL Server.<br/><br/> Copia de seguridad del clúster de SQL Server admitida.<br/><br/>No se admiten las bases de datos almacenadas en volúmenes compartidos de clúster (CSV). |
**SQL Server 2014**<br/><br/> **SQL Server 2012/SP1/SP2**<br/><br/> **SQL Server 2008 R2**<br/><br/> **SQL Server 2008** | MABS v3, v2 | Máquina virtual local y en Azure.| Copia de seguridad de base de datos SQL Server.<br/><br/> Copia de seguridad del clúster de SQL Server admitida.<br/><br/>No se admiten las bases de datos almacenadas en volúmenes compartidos de clúster (CSV). |
**Exchange 2016**<br/><br/> **Exchange 2013**<br/><br/> **Exchange 2010** | MABS v3, v2 | Local. | Copia de seguridad de instancia independiente de Exchange Server y base de datos en grupo de disponibilidad de base de datos (DAG).<br/><br/> Recuperación de buzón de correo y bases de datos de buzón de correo en un grupo de disponibilidad de base de datos (DAG).<br/><br/> ReFS no admitido.<br/><br/> Copia de seguridad de clústeres de discos no compartidos.<br/><br/> Copia de seguridad de Exchange Server configurado para replicación continua. |
**SharePoint 2016**<br/><br/> **SharePoint 2013**<br/><br/> **SharePoint 2010** | MABS v3, v2 | Máquina virtual local y en Azure. | Copia de seguridad de granja y del servidor web front-end.<br/><br/> Recuperación de granja, base de datos, aplicación web, elemento de lista o archivo, búsqueda de SharePoint y servidor web front-end.<br/><br/> No puede hacer copias de seguridad de una granja con SQL Server AlwaysOn para las bases de datos de contenido. |
**Hyper-V en Windows Server 2016**<br/><br/> **Windows Server 2008 R2 (con SP1)** | MABS v3, v2 | Local. | **Agente de MABS en el host de Hyper-V**: Copia de seguridad de máquinas virtuales completas y archivos de datos de host. Copia de seguridad de máquinas virtuales con almacenamiento local, máquinas virtuales en clúster con almacenamiento CSV y máquinas virtuales con almacenamiento de servidor de archivos de SMB.<br/><br/> **Agente de MABS en máquina virtual invitada**: Copia de seguridad de cargas de trabajo que se ejecutan en la máquina virtual. CSV.<br/><br/> **Recuperación**: Máquina virtual, recuperación de nivel de elemento de disco duro virtual, volúmenes, carpetas y archivos.<br/><br/> **Máquinas virtuales Linux**: Copia de seguridad cuando se ejecuta Hyper-V en Windows Server 2012 R2 y versiones posteriores. La recuperación de máquinas virtuales Linux es para toda la máquina. |
**Máquinas virtuales de VMware: vCenter/vSphere ESXi 5.5/6.0/6.5** | MABS v3, v2 | Local. | Copia de seguridad de máquinas virtuales de VMware en el almacenamiento CSV, NFS y SAN.<br/><br/> Recuperación de máquina virtual completa.<br/><br/> Copia de seguridad en Windows/Linux.<br/><br/> Recuperación a nivel de elemento de carpetas y archivos solo para máquinas virtuales Windows.<br/><br/> VMware vApps no compatible.<br/><br/> La recuperación de máquinas virtuales Linux es para toda la máquina. |

## <a name="supported-backups-to-dpm"></a>Copias de seguridad admitidas en DPM

En la tabla siguiente se resume de lo que se puede realizar una copia de seguridad en DPM desde máquinas locales y máquinas virtuales de Azure.

**Copia de seguridad** | **DPM** | **Detalles**
--- | --- | ---
**Windows 10<br/>Windows 8.1<br/>Windows 8<br/>Windows 7**<br/><br/>(32/64 bits) | Solo en entornos locales.<br/><br/> Para copias de seguridad de Windows 10 con DPM 2012 R2, se recomienda instalar [Update 11](https://support.microsoft.com/help/3209592/update-rollup-12-for-system-center-2012-r2-data-protection-manager). | Volumen, recurso compartido, carpeta y archivo.<br/><br/> Volúmenes desduplicados admitidos.<br/><br/> Los volúmenes deben ser de al menos 1 GB y NTFS.
**Windows Server 2016 (Datacenter, Standard, no Nano)**<br/><br/> 64/32 bits | Máquina virtual local y en Azure.<br/><br/> DPM 2016 solo.| Volumen, recurso compartido, carpeta, archivo, estado del sistema y reconstrucción completa.<br/><br/> Volúmenes desduplicados admitidos.
**Windows Server 2012 R2 (Datacenter y Standard)**<br/><br/> 64/32 bits | Máquina virtual local y en Azure. | **Protección local**: Volumen, recurso compartido, carpeta, archivo, estado del sistema y reconstrucción completa.<br/><br/> **Protección de máquina virtual de Azure**: Volumen, recurso compartido, carpeta y archivo.<br/><br/> Volúmenes desduplicados compatibles con DPM 2012 R2 y versiones posteriores.
**Windows Server 2012 con SP1 (Datacenter y Standard)**<br/><br/> 64/32 bits | Máquina virtual local y en Azure. | **Protección local**: Volumen, recurso compartido, carpeta, archivo, estado del sistema y reconstrucción completa.<br/><br/> **Protección de máquina virtual de Azure**: Volumen, recurso compartido, carpeta y archivo.<br/><br/> Volúmenes desduplicados compatibles con DPM 2012 R2 y versiones posteriores.
**Windows 2008 R2 con SP1 (Standard y Enterprise)**<br/><br/> 64/32 bits | Máquina virtual local y en Azure.<br/><br/> [Windows Management Framework 4.0](https://www.microsoft.com/download/details.aspx?id=40855) debe estar instalado. |   **Protección local**: Volumen, recurso compartido, carpeta, archivo, estado del sistema y reconstrucción completa.<br/><br/> **Protección de máquina virtual de Azure**: Volumen, recurso compartido, carpeta y archivo.
**Windows 2008 R2 (Standard y Enterprise)**<br/><br/> 64/32 bits | Local.<br/><br/> No se puede instalar DPM como una máquina virtual de VMware.<br/><br/> No se admite DPM en ejecución en una máquina virtual de Azure. | **Protección local**: Volumen, recurso compartido, carpeta, archivo, estado del sistema y reconstrucción completa.
**Windows Server 2008 con SP2**<br/><br/> 64/32 bits | Solo en entornos locales.<br/><br/> DPM se admite cuando se ejecuta como una máquina virtual de VMware. No se admite la ejecución como un servidor físico o como una máquina virtual de Hyper-V. | Volumen, recurso compartido, carpeta, archivo, estado del sistema y reconstrucción completa.
**Windows Storage Server 2008** | DPM en el entorno local que se ejecuta como una máquina virtual de Hyper-V o como un servidor físico. | Volumen, recurso compartido, carpeta, archivo, estado del sistema y reconstrucción completa.
**SQL Server 2017** | DPM SAC; DPM 2016 con el paquete acumulativo de actualizaciones 5 o posterior.<br/><br/> Máquina virtual local y en Azure.| Copia de seguridad de base de datos SQL Server.<br/><br/> Copia de seguridad del clúster de SQL Server admitida.<br/><br/>No se admiten las bases de datos almacenadas en volúmenes compartidos de clúster (CSV).
**SQL Server 2016 con SP1** | No se admite para DPM 2012 R2; se admite para DPM SAC, DPM 2016 con el paquete acumulativo de actualizaciones 4 o posterior.<br/><br/> Máquina virtual local y en Azure.| Copia de seguridad de base de datos SQL Server.<br/><br/> Copia de seguridad del clúster de SQL Server admitida.<br/><br/>No se admiten las bases de datos almacenadas en volúmenes compartidos de clúster (CSV).
**SQL Server 2016** | No se admite para DPM 2012 R2. Se admite para DPM SAC, DPM 2016 con el paquete acumulativo de actualizaciones 2 y versiones posteriores.<br/><br/> Máquina virtual local y en Azure.| Copia de seguridad de base de datos SQL Server.<br/><br/> Copia de seguridad del clúster de SQL Server admitida.<br/><br/>No se admiten las bases de datos almacenadas en volúmenes compartidos de clúster (CSV).
**SQL Server 2014**<br/><br/> **SQL Server 2012/SP1/SP2**<br/><br/> **SQL Server 2008 R2**<br/><br/> **SQL Server 2008** | SQL Server 2014 con DPM 2012 R2 con el paquete acumulativo de actualizaciones 4 y versiones posteriores.<br/><br/> Máquina virtual local y en Azure.| Copia de seguridad de base de datos SQL Server.<br/><br/> Copia de seguridad del clúster de SQL Server admitida.<br/><br/>No se admiten las bases de datos almacenadas en volúmenes compartidos de clúster (CSV).
**Exchange 2016**<br/><br/> **Exchange 2013**<br/><br/> **Exchange 2010** | Para Exchange 2016, DPM 2012 R2 necesita el paquete acumulativo de actualizaciones 9 o posterior.<br/><br/> Local | Copia de seguridad de instancia independiente de Exchange Server y base de datos en grupo de disponibilidad de base de datos (DAG).<br/><br/> Recuperación de buzón de correo y bases de datos de buzón de correo en un grupo de disponibilidad de base de datos (DAG).<br/><br/> ReFS no admitido.<br/><br/> Copia de seguridad de clústeres de discos no compartidos.<br/><br/> Copia de seguridad de Exchange Server configurado para replicación continua.
**SharePoint 2016**<br/><br/> **SharePoint 2013**<br/><br/> **SharePoint 2010** | SharePoint 2016 en DPM 2016 y versiones posteriores.<br/><br/>Máquina virtual local y en Azure. | Copia de seguridad de granja y del servidor web front-end.<br/><br/> Recuperación de granja, base de datos, aplicación web, elemento de lista o archivo, búsqueda de SharePoint y servidor web front-end.<br/><br/> No puede hacer copias de seguridad de una granja con SQL Server AlwaysOn para las bases de datos de contenido.
**Hyper-V en Windows Server 2016**<br/><br/> **Windows Server 2012 R2/2012** (Datacenter/Standard)<br/><br/> **Windows Server 2008 R2 (con SP1)** | Hyper-V en 2016 compatible para DPM 2016 y versiones posteriores.<br/><br/> Local. | **Agente de MABS en el host de Hyper-V**: Copia de seguridad de máquinas virtuales completas y archivos de datos de host. Copia de seguridad de máquinas virtuales con almacenamiento local, máquinas virtuales en clúster con almacenamiento CSV y máquinas virtuales con almacenamiento de servidor de archivos de SMB.<br/><br/> **Agente de MABS en máquina virtual invitada**: Copia de seguridad de cargas de trabajo que se ejecutan en la máquina virtual. CSV.<br/><br/> **Recuperación**: Máquina virtual, recuperación de nivel de elemento de disco duro virtual, volúmenes, carpetas y archivos.<br/><br/> **Máquinas virtuales Linux**: Copia de seguridad cuando se ejecuta Hyper-V en Windows Server 2012 R2 y versiones posteriores. La recuperación de máquinas virtuales Linux es para toda la máquina.
**Máquinas virtuales de VMware: vCenter/vSphere ESXi 5.5/6.0/6.5** | MABS v3, v2 <br/><br/> DPM 2012 R2 necesita el paquete acumulativo de actualizaciones 1 de System Center <br/><br/>Local. | Copia de seguridad de máquinas virtuales de VMware en el almacenamiento CSV, NFS y SAN.<br/><br/> Recuperación de máquina virtual completa.<br/><br/> Copia de seguridad en Windows/Linux.<br/><br/> Recuperación a nivel de elemento de carpetas y archivos solo para máquinas virtuales Windows.<br/><br/> VMware vApps no compatible.<br/><br/> La recuperación de máquinas virtuales Linux es para toda la máquina.

- Las cargas de trabajo en clúster con copia de seguridad por DPM/MABS deben estar en el mismo dominio que DPM/MABS o en un dominio secundario o de confianza.
- Puede usar la autenticación de certificado/NTLM para realizar copias de seguridad de datos en grupos de trabajo o dominios que no son de confianza.

## <a name="next-steps"></a>Pasos siguientes

- [Obtenga más información](backup-architecture.md#architecture-back-up-to-dpmmabs) sobre la arquitectura de MABS.
- [Revise](backup-support-matrix-mars-agent.md) qué es compatible para el agente de MARS.
- [Configure](backup-azure-microsoft-azure-backup.md) un servidor de MABS.
- [Configure DPM](https://docs.microsoft.com/system-center/dpm/install-dpm?view=sc-dpm-180).
