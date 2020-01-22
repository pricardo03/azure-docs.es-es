---
title: Dispositivo con Azure Migrate
description: Proporciona información general sobre el dispositivo de Azure Migrate usado en la evaluación y migración del servidor.
ms.topic: conceptual
ms.date: 11/19/2019
ms.openlocfilehash: efad1c48dd2c92c0fd5f268013b4a59f34b3a766
ms.sourcegitcommit: dbcc4569fde1bebb9df0a3ab6d4d3ff7f806d486
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/15/2020
ms.locfileid: "76028813"
---
# <a name="azure-migrate-appliance"></a>Dispositivo con Azure Migrate

En este artículo se describe el dispositivo de Azure Migrate. La implementación del dispositivo se realiza cuando usa la herramienta [Azure Migrate: Server Assessment](migrate-services-overview.md#azure-migrate-server-assessment-tool) para detectar y evaluar aplicaciones, la infraestructura y cargas de trabajo para la migración a Microsoft Azure. El dispositivo también se usa al migrar máquinas virtuales de VMware a Azure mediante [Azure Migrate: Server Assessment](migrate-services-overview.md#azure-migrate-server-migration-tool) con la [migración sin agentes](server-migrate-overview.md).

## <a name="appliance-overview"></a>Información general del dispositivo

El dispositivo Azure Migrate se usa en los escenarios siguientes.

**Escenario** | **Herramienta** | **Se usa para** 
--- | --- | ---
Máquina virtual de VMware | Azure Migrate: Server Assessment<br/><br/> Azure Migrate: Server Migration | Detectar máquinas virtuales de VMware<br/><br/> Detectar aplicaciones y dependencias de la máquina<br/><br/> Recopilar metadatos de máquinas y metadatos de rendimiento para las evaluaciones<br/><br/> Replicar máquinas virtuales de VMware con la migración sin agentes
Máquina virtual de Hyper-V | Azure Migrate: Server Assessment | Detectar máquinas virtuales de Hyper-V<br/><br/> Recopilar metadatos de máquinas y metadatos de rendimiento para las evaluaciones
Máquina física |  Azure Migrate: Server Assessment |  Detectar servidores físicos<br/><br/> Recopilar metadatos de máquinas y metadatos de rendimiento para las evaluaciones

## <a name="appliance---vmware"></a>Dispositivo: VMware 

**Requisito** | **VMware** 
--- | ---
**Formato de la descarga** | .OVA 
**Vínculo de la descarga** | https://aka.ms/migrate/appliance/vmware 
**Tamaño de la descarga** | 11,2 GB
**License** | La plantilla del dispositivo descargada incluye una licencia de evaluación de Windows Server 2016 que es válida durante 180 días. Si el período de evaluación está a punto de expirar, se recomienda descargar e implementar un nuevo dispositivo, o bien activar la licencia del sistema operativo de la máquina virtual del dispositivo.
**Implementación** | El dispositivo se implementa como VM de VMware. Necesita suficientes recursos en vCenter Server para asignar una máquina virtual con 32 GB de RAM, 8 vCPU, alrededor de 80 GB de almacenamiento en disco y un conmutador virtual externo.<br/><br/> El dispositivo requiere acceso a Internet, ya sea directamente o a través de un proxy.<br/> La máquina virtual del dispositivo debe implementarse en un host ESXi que ejecute la versión 5.5 o posterior.<br/><br/> El dispositivo solo puede conectarse a una instancia de vCenter Server.
**Hardware** | Recursos en vCenter para asignar una máquina virtual con 32 GB de RAM, 8 vCPU, alrededor de 80 GB de almacenamiento en disco y un conmutador virtual externo. 
**Valor del código hash** | MD5: c06ac2a2c0f870d3b274a0b7a73b78b1<br/><br/> SHA256: 4ce4faa3a78189a09a26bfa5b817c7afcf5b555eb46999c2fad9d2ebc808540c
**vCenter Server/host** | La máquina virtual del dispositivo debe implementarse en un host ESXi que ejecute la versión 5.5 o posterior.<br/><br/> vCenter Server que ejecute la versión 5.5, 6.0, 6.5 o 6.7.
**Proyecto de Azure Migrate** | Un dispositivo solo puede estar asociado a un proyecto. <br/> Se puede asociar cualquier número de dispositivos a un solo proyecto.<br/> 
**Detección** | Un dispositivo puede detectar hasta 10 000 máquinas virtuales de VMware en una instancia de vCenter Server.<br/> Un dispositivo solo puede conectarse a una instancia de vCenter Server.
**Componentes del dispositivo** | Aplicación de administración: aplicación web del dispositivo para la entrada del usuario durante la implementación.<br/> Agente de detección: reúne los datos de configuración de la máquina.<br/> Agente de evaluación: recopila datos de rendimiento.<br/> DRA: organiza la replicación de VM y coordina la comunicación entre las máquinas y Azure.<br/> Puerta de enlace: Envía los datos replicados a Azure.<br/> Servicio de actualización automática: actualiza componentes (se ejecuta cada 24 horas).
**VDDK (migración sin agentes)** | Si ejecuta una migración sin agentes con la migración de servidor de Azure Migrate, es necesario tener instalado el VDDK de VMware vSphere en la máquina virtual del dispositivo.


## <a name="appliance---hyper-v"></a>Dispositivo: Hyper-V

**Requisito** | **Hyper-V** 
--- | ---
**Formato de la descarga** | Carpeta comprimida (con VHD)
**Vínculo de la descarga** | https://aka.ms/migrate/appliance/hyperv 
**Tamaño de la descarga** | 10 GB
**License** | La plantilla del dispositivo descargada incluye una licencia de evaluación de Windows Server 2016 que es válida durante 180 días. Si el período de evaluación está a punto de expirar, se recomienda descargar e implementar un nuevo dispositivo, o bien activar la licencia del sistema operativo de la máquina virtual del dispositivo.
**Implementación del dispositivo**   |  El dispositivo se implementa como VM de Hyper-V.<br/> La máquina virtual del dispositivo proporcionada por Azure Migrate es la versión 5.0 de la máquina virtual de Hyper-V.<br/> El host de Hyper-V debe ejecutarse en Windows Server 2012 R2 o posterior.<br/> El host necesita espacio suficiente para asignar 16 GB de RAM, 8 vCPU, alrededor de 80 GB de espacio de almacenamiento y conmutador externo para la máquina virtual del dispositivo.<br/> El dispositivo requiere una dirección IP estática o dinámica y acceso a Internet.
**Hardware** | Recursos en el host de Hyper-V para asignar 16 GB de RAM, 8 vCPU, alrededor de 80 GB de espacio de almacenamiento y un conmutador externo para la máquina virtual del dispositivo.
**Valor del código hash** | MD5: 29a7531f32bcf69f32d964fa5ae950bc<br/><br/> SHA256: 37b3f27bc44f475872e355f04fcb8f38606c84534c117d1609f2d12444569b31
**Host de Hyper-V** | Debe ejecutar Windows Server 2012 R2 o una versión posterior.
**Proyecto de Azure Migrate** | Un dispositivo solo puede estar asociado a un proyecto. <br/> Se puede asociar cualquier número de dispositivos a un solo proyecto.<br/> 
**Detección** | Un dispositivo puede detectar hasta 5000 máquinas virtuales de VMware en una instancia de vCenter Server.<br/> Un dispositivo puede conectarse hasta a 300 hosts de Hyper-V.
**Componentes del dispositivo** | Aplicación de administración: aplicación web del dispositivo para la entrada del usuario durante la implementación.<br/> Agente de detección: reúne los datos de configuración de la máquina.<br/> Agente de evaluación: recopila datos de rendimiento.<br/>  Servicio de actualización automática: actualiza componentes (se ejecuta cada 24 horas).


## <a name="appliance---physical"></a>Dispositivo: físico

**Requisito** | **Físico** 
--- | ---
**Formato de la descarga** | Carpeta comprimida (con el script del instalador de PowerShell)
**Vínculo de la descarga** | [Vínculo de la descarga](https://go.microsoft.com/fwlink/?linkid=2105112)
**Tamaño de la descarga** | 59,7 MB
**Hardware** | Máquina física dedicada o máquina virtual. La máquina que ejecuta el dispositivo necesita 16 GB de RAM, 8 vCPU, alrededor de 80 GB de espacio de almacenamiento y un conmutador externo.<br/><br/> El dispositivo requiere una dirección IP estática o dinámica y acceso a Internet.
**Valor del código hash** | MD5: 96fd99581072c400aa605ab036a0a7c0<br/><br/> SHA256: f5454beef510c0aa38ac1c6be6346207c351d5361afa0c9cea4772d566fcdc36
**Software** | La máquina del dispositivo debe ejecutar Windows Server 2016. 
**Implementación del dispositivo**   |  El script del instalador del dispositivo se descarga desde el portal (en una carpeta comprimida). <br/> Descomprima la carpeta y ejecute el script de PowerShell (AzureMigrateInstaller.ps1).
**Detección** | Un dispositivo puede detectar hasta 250 servidores físicos.
**Componentes del dispositivo** | Aplicación de administración: aplicación web del dispositivo para la entrada del usuario durante la implementación.<br/> Agente de detección: reúne los datos de configuración de la máquina.<br/> Agente de evaluación: recopila datos de rendimiento.<br/>  Servicio de actualización automática: actualiza componentes (se ejecuta cada 24 horas).
**Acceso a puertos** | Después de configurar el dispositivo, establezca conexiones entrantes en el puerto TCP 3389 para permitir las conexiones del escritorio remoto al dispositivo.<br/><br/> Establezca conexiones entrantes en el puerto 44368 para acceder de forma remota a la aplicación de administración del dispositivo mediante la dirección URL: https://<IP-o-nombre-del-dispositivo>:44368<br/><br/> Conexiones salientes en el puerto 443, 5671 y 5672 para enviar metadatos de detección y rendimiento a Azure Migrate.



## <a name="url-access"></a>acceso URL

El dispositivo de Azure Migrate necesita conectividad a Internet.

- Al implementar el dispositivo, Azure Migrate hace una comprobación de conectividad con las direcciones URL que se resumen en la tabla siguiente.
- Si usa un proxy basado en URL para conectarse a Internet, permita el acceso a estas direcciones URL y asegúrese de que el proxy resuelve los registros CNAME recibidos al buscar estas direcciones.

**URL** | **Detalles**  
--- | --- |
*.portal.azure.com  | Acceda a Azure Portal.
\* .windows.net <br/> *.msftauth.net <br/> *.msauth.net <br/> *.microsoft.com <br/> *.live.com | Inicie sesión en la suscripción de Azure.
*.microsoftonline.com <br/> *.microsoftonline-p.com | Cree aplicaciones de Active Directory para que el dispositivo se comunique con Azure Migrate.
management.azure.com | Crear aplicaciones de Active Directory para que el dispositivo se comunique con el servicio Azure Migrate.
dc.services.visualstudio.com | Cargar los registros de aplicaciones que se usan para la supervisión interna.
*.vault.azure.net | Administrar secretos en Azure Key Vault.
aka.ms/* | Permiso de acceso a vínculos aka. Se usa para las actualizaciones del dispositivo de Azure Migrate.
download.microsoft.com/download | Permita descargas de Microsoft.
*.servicebus.windows.net | Se usa para la migración sin agentes de VMware.<br/><br/> Comunicación entre el dispositivo y el servicio Azure Migrate.
*.discoverysrv.windowsazure.com <br/> *.migration.windowsazure.com <br/> *.hypervrecoverymanager.windowsazure.com | Se usa para la migración sin agentes de VMware.<br/><br/> Conectarse a las direcciones URL del servicio Azure Migrate.
*.blob.core.windows.net |  Se usa para la migración sin agentes de VMware.<br/><br/>Cargue los datos que se deben almacenar.




## <a name="collected-data---vmware"></a>Datos recopilados: VMware

### <a name="collected-performance-data-vmware"></a>Datos de rendimiento recopilados: VMware

Estos son los datos de rendimiento de las VM de VMware que el dispositivo recopila y envía a Azure.

**Data** | **Contador** | **Impacto en la evaluación**
--- | --- | ---
Uso de CPU | cpu.usage.average | Tamaño/costo de VM recomendado
Uso de memoria | mem.usage.average | Tamaño/costo de VM recomendado
Rendimiento de lectura de disco (MB por segundo) | virtualDisk.read.average | Cálculo del tamaño del disco, el costo de almacenamiento y el tamaño de VM
Rendimiento de escrituras de discos (MB por segundo) | virtualDisk.write.average | Cálculo del tamaño del disco, el costo de almacenamiento y el tamaño de VM
Operaciones de lectura de disco por segundo | virtualDisk.numberReadAveraged.average | Cálculo del tamaño del disco, el costo de almacenamiento y el tamaño de VM
Operaciones de escritura de discos por segundo | virtualDisk.numberWriteAveraged.average  | Cálculo del tamaño del disco, el costo de almacenamiento y el tamaño de VM
Rendimiento de lectura de NIC (MB por segundo) | net.received.average | Cálculo del tamaño de VM
Rendimiento de escrituras de adaptadores de red (MB por segundo) | net.transmitted.average  |Cálculo del tamaño de VM


### <a name="collected-metadata-vmware"></a>Metadatos recopilados: VMware

> [!NOTE]
> Los metadatos que detecta el dispositivo de Azure Migrate se usan para ayudarle a calcular el tamaño correcto de sus aplicaciones a medida que las migra a Azure, realizar análisis de idoneidad de Azure y análisis de dependencia entre aplicaciones, y planear costos. Microsoft no usa estos datos en relación con ninguna auditoría de cumplimiento de licencias.

Esta es la lista completa de metadatos de VM de VMware que el dispositivo recopila y envía a Azure.

**Data** | **Contador**
--- | --- 
**Detalles de la máquina** | 
Id. de VM | vm.Config.InstanceUuid 
Nombre de la máquina virtual | vm.Config.Name
Id. de vCenter Server | VMwareClient.Instance.Uuid
Descripción de la VM | vm.Summary.Config.Annotation
Nombre del producto de licencia | vm.Client.ServiceContent.About.LicenseProductName
Tipo de sistema operativo | vm.SummaryConfig.GuestFullName
Tipo de arranque | vm.Config.Firmware
Número de núcleos | vm.Config.Hardware.NumCPU
Memoria (MB) | vm.Config.Hardware.MemoryMB
Número de discos | vm.Config.Hardware.Device.ToList().FindAll(x => es VirtualDisk).count
Lista de tamaños de los discos | vm.Config.Hardware.Device.ToList().FindAll(x => es VirtualDisk)
Lista de adaptadores de red | vm.Config.Hardware.Device.ToList().FindAll(x => es VirtualEthernet).count
Uso de CPU | cpu.usage.average
Uso de memoria |mem.usage.average
**Detalles por disco** | 
Valor de clave del disco | disk.Key
Número de unidades de disco | disk.UnitNumber
Valor de clave de controladora de disco | disk.ControllerKey.Value
Gigabytes aprovisionados | virtualDisk.DeviceInfo.Summary
Nombre del disco | Valor generado mediante disk.UnitNumber, disk.Key y disk.ControllerKey.Value
Operaciones de lectura por segundo | virtualDisk.numberReadAveraged.average
Operaciones de escritura por segundo | virtualDisk.numberWriteAveraged.average
Rendimiento de lectura (MB por segundo) | virtualDisk.read.average
Rendimiento de escritura (MB por segundo) | virtualDisk.write.average
**Detalles por NIC** | 
Nombre del adaptador de red | nic.Key
Dirección MAC | ((VirtualEthernetCard)nic).MacAddress
Direcciones IPv4 | vm.Guest.Net
Direcciones IPv6 | vm.Guest.Net
Rendimiento de lectura (MB por segundo) | net.received.average
Rendimiento de escritura (MB por segundo) | net.transmitted.average
**Detalles de la ruta de acceso de inventario** | 
Nombre | container.GetType().Name
Tipo de objeto secundario | container.ChildType
Información de referencia | container.MoRef
Detalles del objeto primario | Container.Parent
Detalles de la carpeta por VM | ((Folder)container).ChildEntity.Type
Detalles del centro de datos por VM | ((Datacenter)container).VmFolder
Detalles del centro de datos por carpeta de host | ((Datacenter)container).HostFolder
Detalles del clúster por host | ((ClusterComputeResource)container).Host
Detalles del host por VM | ((HostSystem)container).VM

## <a name="collected-data---hyper-v"></a>Datos recopilados: Hyper-V

### <a name="collected-performance-data-hyper-v"></a>Datos de rendimiento recopilados: Hyper-V

> [!NOTE]
> Los metadatos que detecta el dispositivo de Azure Migrate se usan para ayudarle a calcular el tamaño correcto de sus aplicaciones a medida que las migra a Azure, realizar análisis de idoneidad de Azure y análisis de dependencia entre aplicaciones, y planear costos. Microsoft no usa estos datos en relación con ninguna auditoría de cumplimiento de licencias.

Estos son los datos de rendimiento de las máquinas virtuales de Hyper-V que el dispositivo recopila y envía a Azure.

**Clase de contador de rendimiento** | **Contador** | **Impacto en la evaluación**
--- | --- | ---
Procesador virtual del hipervisor de Hyper-V | Tiempo de ejecución de invitado (%) | Tamaño/costo de VM recomendado
VM de Memoria dinámica de Hyper-V | Presión actual (%)<br/> Memoria física visible de invitado (MB) | Tamaño/costo de VM recomendado
Dispositivo de almacenamiento virtual de Hyper-V | Bytes de lectura/segundo | Cálculo del tamaño del disco, el costo de almacenamiento y el tamaño de VM
Dispositivo de almacenamiento virtual de Hyper-V | Bytes de escritura/segundo | Cálculo del tamaño del disco, el costo de almacenamiento y el tamaño de VM
Adaptador de red virtual de Hyper-V | Bytes recibidos/segundo | Cálculo del tamaño de VM
Adaptador de red virtual de Hyper-V | Bytes enviados/segundo | Cálculo del tamaño de VM

- El uso de CPU es la suma de todo el uso, para todos los procesadores virtuales conectados a una VM.
- El uso de memoria es (presión actual * memoria física visible del invitado)/100.
- Los valores de uso de disco y red se recopilan de los contadores de rendimiento de Hyper-V enumerados.

### <a name="collected-metadata-hyper-v"></a>Metadatos recopilados: Hyper-V

Esta es la lista completa de metadatos de VM de Hyper-V que el dispositivo recopila y envía a Azure.

**Data** | **Clase WMI** | **Propiedad de clase WMI**
--- | --- | ---
**Detalles de la máquina** | 
Número de serie de BIOS _ Msvm_BIOSElement | BIOSSerialNumber
Tipo de VM (gen. 1 o 2) | Msvm_VirtualSystemSettingData | VirtualSystemSubType
Nombre para mostrar de VM | Msvm_VirtualSystemSettingData | ElementName
Versión de VM | Msvm_ProcessorSettingData | VirtualQuantity
Memoria (bytes) | Msvm_MemorySettingData | VirtualQuantity
Memoria máxima que se puede consumir por VM | Msvm_MemorySettingData | Límite
Memoria dinámica habilitada | Msvm_MemorySettingData | DynamicMemoryEnabled
Nombre/versión/FQDN del sistema operativo | Msvm_KvpExchangeComponent | Datos de nombre de GuestIntrinsicExchangeItems
Estado de energía de VM | Msvm_ComputerSystem | EnabledState
**Detalles por disco** | 
Identificador de disco | Msvm_VirtualHardDiskSettingData | VirtualDiskId
Tipo de disco duro virtual | Msvm_VirtualHardDiskSettingData | Tipo
Tamaño de disco duro virtual | Msvm_VirtualHardDiskSettingData | MaxInternalSize
Primario de disco duro virtual | Msvm_VirtualHardDiskSettingData | ParentPath
**Detalles por NIC** | 
Direcciones IP (NIC sintéticas) | Msvm_GuestNetworkAdapterConfiguration | IPAddresses
DHCP habilitado (NIC sintéticas) | Msvm_GuestNetworkAdapterConfiguration | DHCPEnabled
Id. de NIC (NIC sintéticas) | Msvm_SyntheticEthernetPortSettingData | InstanceID
Dirección MAC de NIC (NIC sintéticas) | Msvm_SyntheticEthernetPortSettingData | Dirección
Id. de NIC (NIC heredadas) | Datos de MsvmEmulatedEthernetPortSetting | InstanceID
Id. de MAC de NIC (NIC heredadas) | Datos de MsvmEmulatedEthernetPortSetting | Dirección




## <a name="discovery-and-collection-process"></a>Proceso de detección y recopilación

El dispositivo se comunica con instancias de vCenter Server y hosts o clústeres de Hyper-V mediante el siguiente proceso.

1. **Inicio de la detección**:
    - Cuando inicia la detección en el dispositivo de Hyper-V, se comunica con los hosts de Hyper-V en los puertos 5985 (HTTP) y 5986 (HTTPS) de WinRM.
    - Al iniciar la detección en el dispositivo de VMware, se comunica con vCenter Server en el puerto TCP 443 de forma predeterminada. Si vCenter Server escucha en otro puerto, puede configurarlo en la aplicación web del dispositivo.
2. **Recopilación de metadatos y datos de rendimiento**:
    - El dispositivo usa una sesión del Modelo de información común (CIM) para recopilar datos de VM de Hyper-V del host de Hyper-V en los puertos 5985 y 5986.
    - De forma predeterminada, el dispositivo se comunica con el puerto 443 para recopilar datos de VM de VMware de vCenter Server.
3. **Envío de datos**: El dispositivo envía los datos recopilados a Azure Migrate Server Assessment y Azure Migrate Server Migration a través del puerto SSL 443. El dispositivo puede conectarse a Azure a través de Internet, o puede usar ExpressRoute con emparejamiento público o de Microsoft.
    - Para los datos de rendimiento, el dispositivo recopila los datos de uso en tiempo real.
        - Los datos de rendimiento se recopilan cada 20 segundos para VMware, y cada 30 segundos para Hyper-V, para cada métrica de rendimiento.
        - Los datos recopilados se acumulan para crear un único punto de datos durante 10 minutos.
        - El valor de uso máximo se selecciona desde todos los puntos de datos a 20/30 segundos, y se envía a Azure para el cálculo de evaluación.
        - Según el valor de percentil especificado en las propiedades de evaluación (50/90/95/99), los puntos a diez minutos se clasifican en orden ascendente, y el valor de percentil adecuado se usa para calcular la evaluación.
    - Para Server Migration, el dispositivo empieza a recopilar datos de VM y los replica en Azure.
4. **Evaluación y migración**: Ahora puede crear evaluaciones a partir de los metadatos recopilados por el dispositivo mediante Azure Migrate Server Assessment. Además, también puede empezar a migrar VM de VMware mediante Azure Migrate Server Migration para orquestar la replicación de VM sin agente.


![Architecture](./media/migrate-appliance/architecture.png)


## <a name="appliance-upgrades"></a>Actualizaciones del dispositivo

El dispositivo se actualiza a medida que se actualizan los agentes de Azure Migrate que se ejecutan en el dispositivo.

- Esto sucede automáticamente porque la actualización automática está habilitada en el dispositivo de forma predeterminada.
- Puede cambiar esta configuración predeterminada para actualizar los agentes manualmente.
- Para deshabilitar la actualización automática, vaya al Editor del Registro > HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\AzureAppliance y establezca la clave del Registro "AutoUpdate" en 0 (DWORD).
 
### <a name="set-agent-updates-to-manual"></a>Establecer las actualizaciones del agente en manual

En el caso de las actualizaciones manuales, asegúrese de actualizar todos los agentes del dispositivo al mismo tiempo, usando el botón **Actualizar** para cada agente no actualizado en el dispositivo. Puede volver a cambiar la configuración de actualización a actualizaciones automáticas en cualquier momento.

## <a name="next-steps"></a>Pasos siguientes

[Obtenga información](tutorial-assess-vmware.md#set-up-the-appliance-vm) sobre cómo configurar el dispositivo para VMware.
[Obtenga información](tutorial-assess-hyper-v.md#set-up-the-appliance-vm) sobre cómo configurar el dispositivo para Hyper-V.

