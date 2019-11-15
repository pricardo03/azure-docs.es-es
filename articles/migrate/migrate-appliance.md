---
title: Arquitectura del dispositivo de Azure Migrate | Microsoft Docs
description: Proporciona información general sobre el dispositivo de Azure Migrate
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 07/04/2019
ms.author: raynew
ms.openlocfilehash: 249cbea173afe1671118446e0714b721b8c7f72b
ms.sourcegitcommit: 609d4bdb0467fd0af40e14a86eb40b9d03669ea1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2019
ms.locfileid: "73685100"
---
# <a name="azure-migrate-appliance"></a>Dispositivo con Azure Migrate

En este artículo se describe el dispositivo de Azure Migrate. El dispositivo se implementa cuando se usan las herramientas de evaluación y migración de Azure Migrate para detectar, evaluar y migrar aplicaciones, infraestructura y cargas de trabajo a Microsoft Azure. 

[Azure Migrate](migrate-services-overview.md) proporciona un centro de conectividad para realizar el seguimiento de la detección, evaluación y migración a Azure de las aplicaciones y cargas de trabajo locales, así como de VM en las nubes privadas o públicas. El centro proporciona herramientas de Azure Migrate para la evaluación y la migración, así como ofertas de proveedores de software independientes (ISV).



## <a name="appliance-overview"></a>Información general del dispositivo

Los tipos y usos de los dispositivos de Azure Migrate son los siguientes.

**Implementado como** | **Se usa para** | **Detalles**
--- | --- |  ---
Máquina virtual de VMware | Evaluación de VM de VMware con la herramienta de evaluación de Azure Migrate.<br/><br/> Migración sin agente de VM de VMware con la herramienta Azure Migrate Server Migration. | Descargue la plantilla OVA e impórtela en vCenter Server para crear la VM del dispositivo.
Máquina virtual de Hyper-V | Evaluación de VM de Hyper-V con la herramienta de evaluación de Azure Migrate. | Descargue el disco duro virtual comprimido e impórtelo a Hyper-V para crear la VM del dispositivo.

## <a name="appliance-access"></a>Acceso al dispositivo

Después de configurar el dispositivo, puede acceder de forma remota a la VM del dispositivo a través del puerto TCP 3389. También puede acceder de forma remota a la aplicación de administración web para el dispositivo, en el puerto 44368 con la dirección URL: `https://<appliance-ip-or-name>:44368`.

## <a name="appliance-license"></a>Licencia del dispositivo
El dispositivo viene con una licencia de evaluación de Windows Server 2016 que es válida durante 180 días. Si el período de evaluación está a punto de expirar, se recomienda descargar e implementar un nuevo dispositivo, o bien activar la licencia del sistema operativo de la VM del dispositivo.

## <a name="appliance-agents"></a>Agentes del dispositivo
El dispositivo tiene instalados estos agentes.

**Agent** | **Detalles**
--- | ---
Agente de detección | Recopila datos de configuración de máquinas virtuales locales.
Agente de evaluación | Genera perfiles del entorno local para recopilar datos de rendimiento de las VM.
Adaptador de migración | Orquesta la replicación de VM y coordina la comunicación entre las VM y Azure.
Puerta de enlace de migración | Envía los datos de VM replicadas a Azure.


## <a name="appliance-deployment-requirements"></a>Requisitos de implementación del dispositivo

- [Revise](migrate-support-matrix-vmware.md#assessment-appliance-requirements) los requisitos de implementación de un dispositivo de VMware y las direcciones URL a las que el dispositivo tiene que acceder.
- [Revise](migrate-support-matrix-hyper-v.md#assessment-appliance-requirements) los requisitos de implementación de un dispositivo de Hyper-V y las direcciones URL a las que el dispositivo tiene que acceder.


## <a name="collected-performance-data-vmware"></a>Datos de rendimiento recopilados: VMware

Estos son los datos de rendimiento de las VM de VMware que el dispositivo recopila y envía a Azure.

**Datos** | **Contador** | **Impacto en la evaluación**
--- | --- | ---
Uso de CPU | cpu.usage.average | Tamaño/costo de VM recomendado
Uso de memoria | mem.usage.average | Tamaño/costo de VM recomendado
Rendimiento de lectura de disco (MB por segundo) | virtualDisk.read.average | Cálculo del tamaño del disco, el costo de almacenamiento y el tamaño de VM
Rendimiento de escritura de disco (MB por segundo) | virtualDisk.write.average | Cálculo del tamaño del disco, el costo de almacenamiento y el tamaño de VM
Operaciones de lectura de disco por segundo | virtualDisk.numberReadAveraged.average | Cálculo del tamaño del disco, el costo de almacenamiento y el tamaño de VM
Operaciones de escritura de disco por segundo | virtualDisk.numberWriteAveraged.average  | Cálculo del tamaño del disco, el costo de almacenamiento y el tamaño de VM
Rendimiento de lectura de NIC (MB por segundo) | net.received.average | Cálculo del tamaño de VM
Rendimiento de escritura de NIC (MB por segundo) | net.transmitted.average  |Cálculo del tamaño de VM


## <a name="collected-metadata-vmware"></a>Metadatos recopilados: VMware

> [!NOTE]
> Los metadatos que detecta el dispositivo de Azure Migrate se usan para ayudarle a calcular el tamaño correcto de sus aplicaciones a medida que las migra a Azure, realizar análisis de idoneidad de Azure y análisis de dependencia entre aplicaciones, y planear costos. Microsoft no usa estos datos en relación con ninguna auditoría de cumplimiento de licencias.

Esta es la lista completa de metadatos de VM de VMware que el dispositivo recopila y envía a Azure.

**Datos** | **Contador**
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
NOMBRE | container.GetType().Name
Tipo de objeto secundario | container.ChildType
Información de referencia | container.MoRef
Detalles del objeto primario | Container.Parent
Detalles de la carpeta por VM | ((Folder)container).ChildEntity.Type
Detalles del centro de datos por VM | ((Datacenter)container).VmFolder
Detalles del centro de datos por carpeta de host | ((Datacenter)container).HostFolder
Detalles del clúster por host | ((ClusterComputeResource)container).Host
Detalles del host por VM | ((HostSystem)container).VM



## <a name="collected-performance-data-hyper-v"></a>Datos de rendimiento recopilados: Hyper-V

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

## <a name="collected-metadata-hyper-v"></a>Metadatos recopilados: Hyper-V

Esta es la lista completa de metadatos de VM de Hyper-V que el dispositivo recopila y envía a Azure.

**Datos** | **Clase WMI** | **Propiedad de clase WMI**
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
Tipo de disco duro virtual | Msvm_VirtualHardDiskSettingData | type
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
3. **Envío de datos**: El dispositivo envía los datos recopilados a Azure Migrate Server Assessment y Azure Migrate Server Migration a través del puerto SSL 443.
    - Para los datos de rendimiento, el dispositivo recopila los datos de uso en tiempo real.
        - Los datos de rendimiento se recopilan cada 20 segundos para VMware, y cada 30 segundos para Hyper-V, para cada métrica de rendimiento.
        - Los datos recopilados se acumulan para crear un único punto de datos durante diez minutos.
        - El valor de uso máximo se selecciona desde todos los puntos de datos a 20/30 segundos, y se envía a Azure para el cálculo de evaluación.
        - Según el valor de percentil especificado en las propiedades de evaluación (50/90/95/99), los puntos a diez minutos se clasifican en orden ascendente, y el valor de percentil adecuado se usa para calcular la evaluación.
    - Para Server Migration, el dispositivo empieza a recopilar datos de VM y los replica en Azure.
4. **Evaluación y migración**: Ahora puede crear evaluaciones a partir de los metadatos recopilados por el dispositivo mediante Azure Migrate Server Assessment. Además, también puede empezar a migrar VM de VMware mediante Azure Migrate Server Migration para orquestar la replicación de VM sin agente.


![Arquitectura](./media/migrate-appliance/architecture.png)


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

