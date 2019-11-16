---
title: Matriz de compatibilidad de Azure Migrate para la evaluación y migración de VMware
description: Resume la configuración de compatibilidad y las limitaciones para evaluar y migrar máquinas virtuales de VMware a Azure mediante el servicio Azure Migrate.
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 09/17/2019
ms.author: raynew
ms.openlocfilehash: 4b07252aed2205917f6b43e3e09a2877663e5bab
ms.sourcegitcommit: 35715a7df8e476286e3fee954818ae1278cef1fc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/08/2019
ms.locfileid: "73838912"
---
# <a name="support-matrix-for-vmware-assessment-and-migration"></a>Matriz de compatibilidad para la evaluación y migración de VMware

Puede usar [Azure Migrate](migrate-overview.md) para evaluar y migrar máquinas a la nube de Microsoft Azure. En este artículo se resumen los valores de compatibilidad y las limitaciones para evaluar y migrar máquinas virtuales de VMware locales.


## <a name="vmware-scenarios"></a>Escenarios de VMware

En la tabla se resumen los escenarios admitidos para las máquinas virtuales de VMware.

**Implementación** | **Detalles**
--- | ---
**Evaluación de máquinas virtuales locales de VMware** | [Configure](tutorial-prepare-vmware.md) la primera evaluación.<br/><br/> [Ejecute](scale-vmware-assessment.md) una evaluación a gran escala.
**Migración de máquinas virtuales de VMware** | Puede usar la migración sin agente o con agente. [Más información](server-migrate-overview.md)


## <a name="azure-migrate-projects"></a>Proyectos de Azure Migrate

**Soporte técnico** | **Detalles**
--- | ---
**Permisos de Azure** | Necesita permisos de colaborador o propietario en la suscripción para crear un proyecto de Azure Migrate.
**Limitaciones de VMware**  | Evalúe hasta 35 000 máquinas virtuales de VMware en un único proyecto. Puede crear varios proyectos en una suscripción a Azure. Un proyecto puede incluir máquinas virtuales de VMware y máquinas virtuales de Hyper-V, hasta los límites de evaluación.
**Geografía** | [Revisión](migrate-support-matrix.md#supported-geographies) de las ubicaciones geográficas admitidas.

**Geografía** | **Ubicación de almacenamiento de metadatos**
--- | ---
Azure Government | Gobierno de EE. UU. - Virginia
Asia Pacífico | Asia Oriental o Sudeste Asiático
Australia | Este de Australia o Sudeste de Australia
Brasil | Sur de Brasil
Canadá | Centro de Canadá o Este de Canadá
Europa | Norte de Europa y Oeste de Europa
Francia | Centro de Francia
India | India central o India meridional
Japón |  Este de Japón u Oeste de Japón
Corea | Centro de Corea del Sur o Sur de Corea del Sur
Reino Unido | Sur de Reino Unido u Oeste de Reino Unido
Estados Unidos | Centro de EE. UU. u Oeste de EE. UU. 2


 > [!NOTE]
 > La compatibilidad con Azure Government solo está disponible actualmente para la [versión anterior](https://docs.microsoft.com/azure/migrate/migrate-services-overview#azure-migrate-versions) de Azure Migrate.


## <a name="application-discovery"></a>Detección de aplicaciones

Azure Migrate: La evaluación de servidores puede detectar aplicaciones, roles y características. La detección del inventario de aplicaciones le permite identificar y planificar una ruta de migración adaptada a sus cargas de trabajo locales. Azure Migrate: La evaluación de servidores proporciona detección sin agentes con las credenciales de invitado del equipo, de modo que accede de forma remota a las máquinas mediante llamadas WMI y SSH.

**Soporte técnico** | **Detalles**
--- | ---
Equipos compatibles | Máquinas virtuales de VMware locales
Sistema operativo de la máquina | Todas las versiones de Windows y Linux
Credenciales | Actualmente admite el uso de una credencial para todos los servidores Windows y una credencial para todos los servidores Linux. Se crea una cuenta de usuario invitado para máquinas virtuales Windows y una cuenta de usuario normal (acceso sin sudo) para todas las máquinas virtuales Linux.
Límites de la máquina para la detección de aplicaciones | 10 000 por dispositivo. 35 000 por proyecto.

## <a name="assessment-vcenter-server-requirements"></a>Evaluación de los requisitos de vCenter Server

En esta tabla se resume la compatibilidad con la evaluación y las limitaciones de los servidores de virtualización de VMware.

**Soporte técnico** | **Detalles**
--- | ---
**Servidor vCenter** | Las máquinas virtuales de VMware que desea evaluar se deben administrar mediante uno o más servidores vCenter que ejecuten 5.5, 6.0, 6.5 o 6.7.

## <a name="assessment-vcenter-server-permissions"></a>Evaluación: permisos de vCenter Server

Para la evaluación necesita una cuenta de solo lectura para vCenter Server.

## <a name="assessment-appliance-requirements"></a>Evaluación: requisitos del dispositivo

Azure Migrate ejecuta un dispositivo ligero que detecta VM de VMware y envía los metadatos de las VM y los datos de rendimiento a Azure Migrate. El dispositivo para VMware se implementa mediante una plantilla OVA importada en vCenter Server. En la tabla siguiente se resumen los requisitos del dispositivo.

**Soporte técnico** | **Detalles**
--- | ---
**Implementación del dispositivo** | El dispositivo se implementa como VM de VMware. Necesita suficientes recursos en vCenter Server para asignar una máquina virtual con 32 GB de RAM, 8 vCPU, alrededor de 80 GB de almacenamiento en disco y un conmutador virtual externo.<br/><br/> El dispositivo requiere acceso a Internet, ya sea directamente o a través de un proxy.<br/> La máquina virtual del dispositivo debe implementarse en un host ESXi que ejecute la versión 5.5 o posterior.
**Proyecto de Azure Migrate** | Un dispositivo solo puede estar asociado a un proyecto. <br/> Se puede asociar cualquier número de dispositivos a un solo proyecto.<br/> Evalúe hasta 35 000 VM en un proyecto.
**Detección** | Un dispositivo puede detectar hasta 10 000 máquinas virtuales de VMware en una instancia de vCenter Server.<br/> Un dispositivo solo puede conectarse a una instancia de vCenter Server.
**Grupo de evaluación** | Puede agregar hasta 35 000 máquinas en un solo grupo.
**Valoración** | Puede acceder hasta a 35 000 máquinas virtuales en una única evaluación.


## <a name="assessment-url-access-requirements"></a>Evaluación: requisitos de acceso a las direcciones URL

El dispositivo de Azure Migrate necesita conectividad a Internet.

- Al implementar el dispositivo, Azure Migrate hace una comprobación de conectividad con las direcciones URL que se resumen en la tabla siguiente.
- Si usa un proxy basado en URL para conectarse a Internet, permita el acceso a estas direcciones URL y asegúrese de que el proxy resuelve los registros CNAME recibidos al buscar estas direcciones.

**URL** | **Detalles**  
--- | --- |
*.portal.azure.com  | Ir a Azure Migrate en Azure Portal.
\* .windows.net <br/> *.msftauth.net <br/> *.msauth.net <br/> *.microsoft.com <br/> *.live.com | Inicie sesión en su suscripción de Azure.
*.microsoftonline.com <br/> *.microsoftonline-p.com | Crear aplicaciones de Active Directory para que el dispositivo se comunique con el servicio Azure Migrate.
management.azure.com | Crear aplicaciones de Active Directory para que el dispositivo se comunique con el servicio Azure Migrate.
dc.services.visualstudio.com | Cargar los registros de aplicaciones que se usan para la supervisión interna.
*.vault.azure.net | Administrar secretos en Azure Key Vault.
*.servicebus.windows.net | Comunicación entre el dispositivo y el servicio Azure Migrate.
*.discoverysrv.windowsazure.com <br/> *.migration.windowsazure.com <br/> *.hypervrecoverymanager.windowsazure.com | Conectarse a las direcciones URL del servicio Azure Migrate.
\* .blob.core.windows.net | Cargar los datos en las cuentas de almacenamiento.
https://aka.ms/latestapplianceservices<br/><br/> https://download.microsoft.com/download | Se usa para las actualizaciones del dispositivo de Azure Migrate.

## <a name="assessment-port-requirements"></a>Evaluación: requisitos de puertos

**Dispositivo** | **Connection**
--- | ---
Dispositivo | Conexiones entrantes en el puerto TCP 3389 para permitir las conexiones del Escritorio remoto al dispositivo.<br/><br/> Conexiones entrantes en el puerto 44368 para tener acceso de forma remota a la aplicación de administración del dispositivo mediante la dirección URL: ```https://<appliance-ip-or-name>:44368``` <br/><br/>Conexiones salientes en el puerto 443, 5671 y 5672 para enviar metadatos de detección y rendimiento a Azure Migrate.
Servidor vCenter | Conexiones entrantes en el puerto TCP 443 para permitir que el dispositivo recopile los metadatos de configuración y rendimiento de las evaluaciones. <br/><br/> De forma predeterminada, el dispositivo se conecta a vCenter en el puerto 443. Si el servidor vCenter escucha en un puerto diferente, puede modificar el puerto al configurar la detección.

## <a name="assessment-dependency-visualization"></a>Evaluación: visualización de dependencias

La visualización de dependencias le permite observar las dependencias en las máquinas que quiere evaluar y migrar. La asignación de dependencias suele usarse cuando se quiere evaluar máquinas con niveles de confianza más altos. En el caso de las máquinas virtuales de VMware, se admite la visualización de dependencias como se muestra a continuación:

- **Visualización de dependencias sin agentes**: Esta opción se encuentra actualmente en versión preliminar. No es necesario que instale ningún agente en las máquinas.
    - Funciona capturando los datos de conexión TCP de las máquinas para las que está habilitada. Una vez iniciada la detección de dependencias, el dispositivo recopila datos de las máquinas en un intervalo de sondeo de cinco minutos.
    - Se pueden recopilar los siguientes datos:
        - Conexiones TCP
        - Nombres de los procesos que tienen conexiones activas
        - Nombres de las aplicaciones instaladas que ejecutan los procesos anteriores
        - No. de conexiones detectadas en cada intervalo de sondeo
- **Visualización de dependencias basada en agente**: para usar la visualización de dependencias basada en agente, debe descargar e instalar los siguientes agentes en cada máquina local que vaya a analizar.
    - Es necesario tener instalado Microsoft Monitoring Agent (MMA) en cada máquina. [Obtenga más información](how-to-create-group-machine-dependencies.md#install-the-mma) sobre cómo instalar el agente MMA.
    - Es necesario tener instalado Dependency Agent en cada máquina. [Obtenga más información](how-to-create-group-machine-dependencies.md#install-the-dependency-agent) sobre cómo instalar el agente de dependencia.
    - Además, si tiene máquinas sin conectividad a Internet, debe descargar e instalar en ellas la puerta de enlace de Log Analytics.

## <a name="migration---limitations"></a>Migración: limitaciones
Puede seleccionar hasta 10 máquinas virtuales a la vez para la replicación. Si quiere migrar más máquinas, replique en grupos de 10. En el caso de la migración sin agente de VMware, puede ejecutar hasta 100 replicaciones simultáneamente.

## <a name="agentless-migration-vmware-server-requirements"></a>Migración sin agente: requisitos del servidor de VMware

En esta tabla se resume la compatibilidad con la evaluación y las limitaciones de los servidores de virtualización de VMware.

**Soporte técnico** | **Detalles**
--- | ---
vCenter Server | Versión 5.5, 6.0, 6.5 o 6.7.
VMware vSphere | Versión 5.5, 6.0, 6.5 o 6.7.

## <a name="agentless-migration-vcenter-server-permissions"></a>Migración sin agente: permisos de vCenter Server

**Permisos** | **Detalles**
--- | ---
Datastore.Browse | Permite examinar los archivos de registro de máquina virtual para solucionar problemas de creación y eliminación de instantáneas.
Datastore.LowLevelFileOperations | Permite operaciones de lectura, escritura, eliminación y cambio de nombre en el explorador del almacén de datos para solucionar problemas de creación y eliminación de instantáneas.
VirtualMachine.Configuration.DiskChangeTracking | Permite habilitar o deshabilitar el seguimiento de cambios de los discos de máquina virtual para extraer los bloques de datos cambiados entre instantáneas
VirtualMachine.Configuration.DiskLease | Permite operaciones de concesión de discos para una máquina virtual, para leer el disco mediante el kit de desarrollo de discos virtuales de VMware vSphere (VDDK).
VirtualMachine.Provisioning.AllowReadOnlyDiskAccess | Permite abrir un disco en una máquina virtual para leerlo mediante VDDK.
VirtualMachine.Provisioning.AllowVirtualMachineDownload  | Permite operaciones de lectura en los archivos asociados con una máquina virtual, para descargar los registros y solucionar problemas si se produce un error.
VirtualMachine.SnapshotManagement.* | Permite la creación y la administración de instantáneas de máquina virtual para la replicación.
Virtual Machine.Interaction.Power Off | Permite apagar la máquina virtual durante la migración a Azure.


## <a name="agentless-migration-vmware-vm-requirements"></a>Migración sin agente: requisitos de máquina virtual de VMware

**Soporte técnico** | **Detalles**
--- | ---
**Sistemas operativos compatibles** | Los sistemas operativos [Windows](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines) y [Linux](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros) que se admiten en Azure se pueden migrar con la migración sin agente.
**Cambios necesarios para Azure** | Es posible que algunas máquinas virtuales requieran cambios para poder ejecutarse en Azure. Azure Migrate hace estos cambios automáticamente en los siguientes sistemas operativos:<br/> - Red Hat Enterprise Linux 6.5+, 7.0+<br/> - CentOS 6.5+, 7.0+</br> - SUSE Linux Enterprise Server 12 SP1+<br/> - Ubuntu 14.04LTS, 16.04LTS, 18.04LTS<br/> - Debian 7, 8<br/><br/> En el caso de otros sistemas operativos, debe hacer los ajustes manualmente antes de la migración. Los artículos pertinentes contienen instrucciones sobre cómo hacerlo.
**Arranque de Linux** | Si/boot está en una partición dedicada, debe residir en el disco del sistema operativo y no distribuirse en varios discos.<br/> Si/boot forma parte de la partición raíz (/), la partición "/" debe estar en el disco del sistema operativo y no abarcar otros discos.
**Arranque UEFI** | Las máquinas virtuales con arranque UEFI no se admiten para la migración.
**Tamaño del disco** | Disco de sistema operativo de 2 TB y 4 TB para los discos de datos.
**Límites del disco** |  Hasta 60 discos por máquina virtual.
**Discos/volúmenes cifrados** | Las máquinas virtuales con volúmenes o discos cifrados no se admiten para la migración.
**Clúster de discos compartido** | No compatible.
**Discos independientes** | No compatible.
**Discos RDM/de acceso directo** | Si las máquinas virtuales tienen discos RDM o de acceso directo, estos discos no se replicarán en Azure.
**NFS** | Los volúmenes NFS montados como volúmenes en las máquinas virtuales no se replicarán.
**Destinos iSCSI** | Las máquinas virtuales con destinos iSCSI no se admiten para la migración sin agente.
**E/S de varias rutas** | No compatible.
**Storage vMotion** | No compatible. La replicación no funcionará si una máquina virtual usa Storage vMotion.
**NIC en equipo** | No compatible.
**IPv6** | No compatible.
**Disco de destino** | Las máquinas virtuales solo se pueden migrar a discos administrados (HDD Estándar, SSD Premium) en Azure.
**Replicación simultánea** | Cien máquinas virtuales por vCenter Server. Si tiene más, mígrelas en lotes de 100.


## <a name="agentless-migration-appliance-requirements"></a>Migración sin agente: requisitos del dispositivo


**Soporte técnico** | **Detalles**
--- | ---
**ESXi** | La máquina virtual del dispositivo debe implementarse en un host ESXi que ejecute la versión 5.5 o posterior.
**Proyecto de Azure Migrate** | Un dispositivo solo puede estar asociado a un proyecto.
**vCenter Server** | Un dispositivo puede detectar hasta 10 000 máquinas virtuales de VMware en una instancia de vCenter Server.<br/> Un dispositivo puede conectarse a una instancia de vCenter Server.
**VDDK** | Si va a ejecutar una migración sin agente con la migración de Azure Migrate Server, es necesario tener instalado el VDDK de VMware vSphere en la máquina virtual del dispositivo.

## <a name="agentless-migration-url-access-requirements"></a>Migración sin agente: requisitos de acceso a las direcciones URL

El dispositivo de Azure Migrate necesita conectividad a Internet.

- Al implementar el dispositivo, Azure Migrate hace una comprobación de conectividad con las direcciones URL que se resumen en la tabla siguiente.
- Si usa un proxy basado en una dirección URL, permita el acceso a estas direcciones URL y asegúrese de que el proxy resuelve los registros CNAME recibidos al buscar estas direcciones.

**URL** | **Detalles**  
--- | ---
*.portal.azure.com | Ir a Azure Migrate en Azure Portal.
\* .windows.net <br/> *.msftauth.net <br/> *.msauth.net <br/> *.microsoft.com <br/> *.live.com  | Inicie sesión en su suscripción de Azure.
*.microsoftonline.com <br/> *.microsoftonline-p.com | Crear aplicaciones de Active Directory para que el dispositivo se comunique con el servicio Azure Migrate.
management.azure.com | Crear aplicaciones de Active Directory para que el dispositivo se comunique con el servicio Azure Migrate.
dc.services.visualstudio.com | Cargar los registros de aplicaciones que se usan para la supervisión interna.
*.vault.azure.net | Administrar secretos en Azure Key Vault.
*.servicebus.windows.net | Comunicación entre el dispositivo y el servicio Azure Migrate.
*.discoverysrv.windowsazure.com <br/> *.migration.windowsazure.com <br/> *.hypervrecoverymanager.windowsazure.com | Conectarse a las direcciones URL del servicio Azure Migrate.
\* .blob.core.windows.net | Cargar los datos en las cuentas de almacenamiento.
https://aka.ms/latestapplianceservices<br/><br/> https://download.microsoft.com/download | Se usa para las actualizaciones del dispositivo de Azure Migrate.


## <a name="agentless-migration-port-requirements"></a>Migración sin agente: requisitos de puerto

**Dispositivo** | **Connection**
--- | ---
Dispositivo | Las conexiones salientes del puerto 443 para cargar los datos replicados en Azure y comunicarse con los servicios ce Azure Migrate que organizan la replicación y la migración.
Servidor vCenter | Conexiones salientes del puerto 443 que permiten al dispositivo organizar la replicación (crear instantáneas, copiar datos y liberar instantáneas)
Host de vSphere/EXSI | Conexiones entrantes en el puerto TCP 902 para que el dispositivo replique datos de las instantáneas.


## <a name="agent-based-migration-vmware-server-requirements"></a>Migración basada en agente: requisitos del servidor VMware

En esta tabla se resume la compatibilidad con la evaluación y las limitaciones de los servidores de virtualización de VMware.

**Soporte técnico** | **Detalles**
--- | ---
vCenter Server | Versión 5.5, 6.0, 6.5 o 6.7.
VMware vSphere | Versión 5.5, 6.0, 6.5 o 6.7.

### <a name="agent-based-migration-vcenter-server-permissions"></a>Migración basada en agente: permisos de vCenter Server

Una cuenta de solo lectura para vCenter Server.

## <a name="agent-based-migration-replication-appliance-requirements"></a>Migración basada en agente: requisitos del dispositivo de replicación

En la siguiente tabla se resumen los requisitos del [dispositivo de replicación](migrate-replication-appliance.md) usado en la migración basada en agente de máquinas virtuales VMware y servidores físicos con la migración de Azure Migrate Server.

> [!NOTE]
> Al configurar el dispositivo de replicación con la plantilla OVA proporcionada en el concentrador de Azure Migrate, el dispositivo ejecuta Windows Server 2016 y cumple los requisitos de compatibilidad. Si configura el dispositivo de replicación manualmente en un servidor físico, asegúrese de que cumple los requisitos.



**Componente** | **Requisito**
--- | ---
 | **Configuración de VMware** (dispositivo de máquina virtual de VMware)
PowerCLI | Si el dispositivo de replicación se ejecuta en una máquina virtual de VMware, se debe instalar [PowerCLI versión 6.0](https://my.vmware.com/web/vmware/details?productId=491&downloadGroup=PCLI600R1).
Tipo de NIC | VMXNET3 (si el dispositivo es una máquina virtual de VMware)
 | **Configuración de hardware**
Núcleos de CPU | 8
RAM | 16 GB
Número de discos | Tres: el disco del sistema operativo, el disco de la caché del servidor de procesos y la unidad de retención.
Espacio libre en disco (caché) | 600 GB
Espacio libre en el disco (disco de retención) | 600 GB
**Configuración de software** |
Sistema operativo | Windows Server 2016 o Windows Server 2012 R2
Configuración regional del sistema operativo | Español (es-es)
TLS | TLS 1.2 debe estar habilitado.
.NET Framework | .NET Framework 4.6 o posterior debe estar instalado en la máquina (con criptografía segura habilitada).
MySQL | MySQL debe estar instalado en el dispositivo.<br/> Se debe instalar MySQL. Se puede instalar manualmente o lo puede instalar Site Recovery durante la implementación.
Otras aplicaciones | No ejecute otras aplicaciones en el dispositivo de replicación.
Roles de Windows Server | No habilite estos roles: <br> - Active Directory Domain Services <br>- Internet Information Services <br> - Hyper-V
Directivas de grupo | No habilite estas directivas de grupo: <br> - Impedir el acceso al símbolo del sistema. <br> - Impedir el acceso a herramientas de edición del Registro. <br> - Confiar en la lógica de datos adjuntos de archivos. <br> - Activar la ejecución de scripts. <br> [Más información](https://technet.microsoft.com/library/gg176671(v=ws.10).aspx)
IIS | - Ningún sitio web predeterminado debe existir previamente <br> - Ningún sitio web o aplicación que escuche en el puerto 443 deben existir previamente <br>- Habilitar la [autenticación anónima](https://technet.microsoft.com/library/cc731244(v=ws.10).aspx) <br> - Habilitar la configuración de [FastCGI](https://technet.microsoft.com/library/cc753077(v=ws.10).aspx)
**Configuración de red** |
Tipo de dirección IP | estática
Puertos | 443 (orquestación del canal de control)<br>9443 (Transporte de datos)
Tipo de NIC | VMXNET3

### <a name="replication-appliance-url-access"></a>Acceso a la dirección URL del dispositivo de replicación

El dispositivo de replicación necesita acceso a estas direcciones URL.

**URL** | **Detalles**
--- | ---
\*.backup.windowsazure.com | Se usa para la transferencia y coordinación de datos replicados
\*.store.core.windows.net | Se usa para la transferencia y coordinación de datos replicados
\*blob.core.windows.net | Se usa para tener acceso a la cuenta de almacenamiento que almacena los datos replicados
\*.hypervrecoverymanager.windowsazure.com | Se usa para las operaciones de administración de replicación y coordinación
https:\//management.azure.com | Se usa para las operaciones de administración de replicación y coordinación
*.services.visualstudio.com | Se usa con fines de telemetría (es opcional)
time.nist.gov | Se usan para comprobar la sincronización de la hora entre el sistema y la hora global.
time.windows.com | Se usan para comprobar la sincronización de la hora entre el sistema y la hora global.
https:\//login.microsoftonline.com <br/> https:\//secure.aadcdn.microsoftonline-p.com <br/> https:\//login.live.com <br/> https:\//graph.windows.net <br/> https:\//login.windows.net <br/> https:\//www.live.com <br/> https:\//www.microsoft.com  | La configuración de OVF necesita acceder a estas direcciones URL. Azure Active Directory las usa para la administración de identidades y control de acceso
https:\//dev.mysql.com/get/Downloads/MySQLInstaller/mysql-installer-community-5.7.20.0.msi | Para completar la descarga de MySQL


#### <a name="mysql-installation-options"></a>Opciones de instalación de MySQL

MySQL se puede instalar en el dispositivo de replicación mediante uno de estos métodos.

**Método** | **Detalles**
--- | ---
Descargar e instalar manualmente | Descargue la aplicación MySQL y colóquela en la carpeta C:\Temp\ASRSetup; luego, instálela manualmente.<br/> Al configurar el dispositivo, MySQL se mostrará como ya instalado.
Sin descarga en línea | Coloque la aplicación del instalador de MySQL en la carpeta C:\Temp\ASRSetup. Al instalar el dispositivo y hacer clic para descargar e instalar MySQL, el programa de instalación usará el instalador que ha agregado.
Descargar e instalar en Azure Migrate | Cuando instale el dispositivo y se le solicite MySQL, seleccione **Descargar e instalar**.



## <a name="agent-based-migration-vmware-vm-requirements"></a>Migración basada en agente: requisitos de máquina virtual de VMware

**Soporte técnico** | **Detalles**
--- | ---
**Carga de trabajo de la máquina** | Azure Migrate admite la migración de cualquier carga de trabajo (por ejemplo, Active Directory, SQL Server, etc.) que se ejecute en una máquina compatible.
**Sistemas operativos** | Para conocer la información más reciente, revise la [compatibilidad del sistema operativo](../site-recovery/vmware-physical-azure-support-matrix.md#replicated-machines) de Site Recovery. Azure Migrate proporciona compatibilidad idéntica con el sistema operativo de máquina virtual.
**Sistema de archivos Linux/almacenamiento de invitados** | Para conocer la información más reciente, revise la [compatibilidad del sistema de archivos Linux](../site-recovery/vmware-physical-azure-support-matrix.md#linux-file-systemsguest-storage) de Site Recovery. Azure Migrate tiene compatibilidad idéntica con el sistema de archivos Linux.
**Red y almacenamiento** | Para conocer la información más reciente, revise los requisitos previos de [red](../site-recovery/vmware-physical-azure-support-matrix.md#network) y [almacenamiento](../site-recovery/vmware-physical-azure-support-matrix.md#storage) de Site Recovery. Azure Migrate proporciona requisitos idénticos de red y almacenamiento.
**Requisitos de Azure** | Para conocer la información más reciente, revise los requisitos de [red](../site-recovery/vmware-physical-azure-support-matrix.md#azure-vm-network-after-failover), [almacenamiento](../site-recovery/vmware-physical-azure-support-matrix.md#azure-storage) y [proceso](../site-recovery/vmware-physical-azure-support-matrix.md#azure-compute) de Azure para Site Recovery. Azure Migrate tiene requisitos idénticos para la migración de VMware.
**Servicio de movilidad** | El agente del servicio de movilidad se debe instalar en cada máquina virtual que quiera migrar.
**Arranque UEFI** | La máquina virtual migrada en Azure se convertirá automáticamente en una VM de arranque del BIOS.<br/><br/> El disco del sistema operativo debe tener hasta cuatro particiones y los volúmenes deben formatearse con NTFS.
**Disco de destino** | Las máquinas virtuales solo se pueden migrar a discos administrados (HDD Estándar, SSD Premium) en Azure.
**Tamaño del disco** | Disco de sistema operativo de 2 TB y 8 TB para los discos de datos.
**Límites del disco** |  Hasta 63 discos por máquina virtual.
**Discos/volúmenes cifrados** | Las máquinas virtuales con volúmenes o discos cifrados no se admiten para la migración.
**Clúster de discos compartido** | No compatible.
**Discos independientes** | Se admite.
**Discos de acceso directo** | Se admite.
**NFS** | Los volúmenes NFS montados como volúmenes en las máquinas virtuales no se replicarán.
Destinos iSCSI | Las máquinas virtuales con destinos iSCSI no se admiten para la migración sin agente.
**E/S de varias rutas** | No compatible.
**Storage vMotion** | Compatible
**NIC en equipo** | No compatible.
**IPv6** | No compatible.




## <a name="agent-based-migration-url-access-requirements"></a>Migración basada en agente: requisitos de acceso a las direcciones URL

El servicio de movilidad que se ejecuta en las máquinas virtuales de VMware necesita conexión a Internet.

Al implementar el servicio de movilidad, no se realiza una comprobación de conectividad a las direcciones URL que se resumen en la tabla siguiente.


**URL** | **Detalles**  
--- | ---
*.portal.azure.com | Ir a Azure Migrate en Azure Portal.
\* .windows.net | Inicie sesión en su suscripción de Azure.
*.microsoftonline.com | Crear aplicaciones de Active Directory para que el dispositivo se comunique con el servicio Azure Migrate.
management.azure.com | Crear aplicaciones de Active Directory para que el dispositivo se comunique con el servicio Azure Migrate.
dc.services.visualstudio.com | Cargar los registros de aplicaciones que se usan para la supervisión interna.
*.vault.azure.net | Administrar secretos en Azure Key Vault.
*.servicebus.windows.net | Comunicación entre el dispositivo y el servicio Azure Migrate.
*.discoverysrv.windowsazure.com <br/> *.migration.windowsazure.com <br/> *.hypervrecoverymanager.windowsazure.com | Conectarse a las direcciones URL del servicio Azure Migrate.
\* .blob.core.windows.net | Cargar los datos en las cuentas de almacenamiento.

## <a name="agent-based-migration-port-requirements"></a>Migración basada en agente: requisitos de puerto

**Dispositivo** | **Connection**
--- | ---
Máquinas virtuales | El servicio de movilidad que se ejecuta en las máquinas virtuales se comunica con el dispositivo de replicación local (servidor de configuración) en el puerto HTTPS 443 entrante para la administración de la replicación.<br/><br/> Las máquinas virtuales envían datos de replicación al servidor de procesos (que se ejecuta en la máquina del servidor de configuración) en el puerto HTTPS 9443 entrante. Este puerto se puede modificar.
Dispositivo de replicación | El dispositivo de replicación organiza la replicación con Azure a través del puerto HTTPS 443 saliente.
Servidor de proceso | El servidor de procesos recibe los datos de la replicación, los optimiza, los cifra y los envía a Azure Storage a través del puerto 443 de salida.<br/> De forma predeterminada, el servidor de procesos se ejecuta en el dispositivo de replicación.

## <a name="azure-vm-requirements"></a>Requisitos de VM de Azure

Todas las máquinas virtuales locales que se replican en Azure deben cumplir los requisitos de máquina virtual de Azure que se resumen en esta tabla. Cuando Site Recovery ejecuta una comprobación de requisitos previos para la replicación, se producirá un error si no se cumplen algunos de los requisitos.

**Componente** | **Requisitos** | **Detalles**
--- | --- | ---
Sistema operativo invitado | Compruebe los sistemas operativos compatibles de las [máquinas virtuales de VMware que usan la replicación sin agente](#agentless-migration-vmware-vm-requirements) y de las [máquinas virtuales de VMware que usan la replicación basada en agente](#agent-based-migration-vmware-vm-requirements).<br/> Puede migrar cualquier carga de trabajo que se ejecute en un sistema operativo compatible. | Se produce un error en la comprobación si no es compatible.
Arquitectura del sistema operativo invitado | 64 bits | Se produce un error en la comprobación si no es compatible.
Tamaño del disco del sistema operativo | Hasta 2048 GB | Se produce un error en la comprobación si no es compatible.
Número de discos del sistema operativo | 1 | Se produce un error en la comprobación si no es compatible.
Número de discos de datos | 64 o menos | Se produce un error en la comprobación si no es compatible.
Tamaño del disco de datos | Hasta 4095 GB | Se produce un error en la comprobación si no es compatible.
Adaptadores de red | Se admiten varios adaptadores. |
VHD compartido | No compatible. | Se produce un error en la comprobación si no es compatible.
Disco FC | No compatible. | Se produce un error en la comprobación si no es compatible.
BitLocker | No compatible. | Debe deshabilitar BitLocker antes de habilitar la replicación de una máquina.
Nombre de la máquina virtual | Entre 1 y 63 caracteres.<br/> Restringido a letras, números y guiones.<br/><br/> El nombre de la máquina debe empezar y terminar con una letra o un número. |  Actualice el valor de las propiedades de la máquina en Site Recovery.
Conexión después de la migración: Windows | Para conectarse a máquinas virtuales de Azure que se ejecutan en Windows después de la migración, siga estos pasos:<br/> -Antes de la migración, habilita RDP en la máquina virtual local. Asegúrese de que se hayan agregado las reglas de TCP y UDP para el perfil **Público**, y que RDP se permite en **Firewall de Windows** > **Aplicaciones permitidas** para todos los perfiles.<br/> Para el acceso a VPN de sitio a sitio, habilite RDP y permítalo en **Firewall de Windows** -> **Aplicaciones y características permitidas** para redes de **dominio y privadas**. Además, compruebe que la directiva SAN del sistema operativo está establecida en **OnlineAll**. [Más información](prepare-for-migration.md). |
Conexión después de la migración: Linux | Para conectarse a máquinas virtuales de Azure después de la migración mediante SSH, siga estos pasos:<br/> Antes de la migración, en la máquina local, compruebe que el servicio Secure Shell está establecido en Iniciar y que las reglas de firewall permiten una conexión SSH.<br/> Tras la conmutación por error, en la máquina virtual de Azure, permita conexiones entrantes al puerto SSH para las reglas del grupo de seguridad de red de la máquina virtual conmutada por error y para la subred de Azure a la que esta se conecta. Además, agregue una dirección IP pública para la máquina virtual. |  


## <a name="next-steps"></a>Pasos siguientes

[Preparación de VMware](tutorial-prepare-vmware.md) para la migración y la evaluación
