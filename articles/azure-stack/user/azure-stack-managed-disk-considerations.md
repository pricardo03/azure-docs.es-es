---
title: Diferencias y consideraciones para Managed Disks en Azure Stack | Microsoft Docs
description: Obtenga información sobre las diferencias y consideraciones al trabajar con Managed Disks en Azure Stack.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/31/2019
ms.author: sethm
ms.reviewer: jiahan
ms.lastreviewed: 01/05/2019
ms.openlocfilehash: 60f633049d6fdbb59744c8003e742ff649d48ea7
ms.sourcegitcommit: b3d74ce0a4acea922eadd96abfb7710ae79356e0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/14/2019
ms.locfileid: "56243400"
---
# <a name="azure-stack-managed-disks-differences-and-considerations"></a>Managed Disks de Azure Stack: diferencias y consideraciones

En este artículo se resumen las diferencias conocidas entre [Managed Disks de Azure Stack](azure-stack-manage-vm-disks.md) y [Managed Disks para Azure](../../virtual-machines/windows/managed-disks-overview.md). Para obtener información acerca de las diferencias de alto nivel entre Azure y Azure Stack, consulte el artículo [Key considerations](azure-stack-considerations.md) (Consideraciones clave).

Managed Disks simplifica la administración de discos para las máquinas virtuales de IaaS, ya que administra las [cuentas de almacenamiento](../azure-stack-manage-storage-accounts.md) asociadas a los discos de la máquina virtual.

> [!Note]  
> Managed Disks en Azure Stack está disponible desde la actualización 1808. Está habilitado de forma predeterminada al crear máquinas virtuales mediante el portal de Azure Stack a partir de la actualización 1811.
  
## <a name="cheat-sheet-managed-disk-differences"></a>Hoja de referencia rápida: diferencias de Managed Disks

| Característica | Azure (global) | Azure Stack |
| --- | --- | --- |
|Cifrado de datos en reposo |Storage Service Encryption (SSE) de Azure, Azure Disk Encryption (ADE)     |Cifrado AES de 128 bits de BitLocker      |
|Imagen          | Compatibilidad con la imagen personalizada administrada |Compatible|
|Opciones de copia de seguridad |Compatibilidad con el servicio Azure Backup |Todavía no se admite |
|Opciones de recuperación ante desastres |Compatibilidad con Azure Site Recovery |Todavía no se admite|
|Tipos de disco     |SSD Premium, SSD estándar (versión preliminar) y HDD estándar |SSD Premium, HDD estándar |
|Discos premium  |Totalmente compatible |Se pueden aprovisionar, pero no hay límite de rendimiento o garantía  |
|E/S por segundo de discos premium  |Depende del tamaño del disco  |2300 E/S por segundo por disco |
|Rendimiento de discos premium |Depende del tamaño del disco |145 MB/segundo por disco |
|Tamaño del disco  |Disco Premium de Azure: P4 (32 GiB) a P80 (32 TiB)<br>Disco SSD estándar de Azure: E10 (128 GiB) a E80 (32 TiB)<br>Disco HDD estándar de Azure: S4 (32 GiB) a S80 (32 TiB) |M4: 32 GiB<br>M6: 64 GiB<br>M10: 128 GB<br>M15: 256 GiB<br>M20: 512 GB<br>M30: 1024 GiB |
|Copia de instantáneas de discos|Discos administrados de Azure de instantáneas conectados a una máquina virtual en ejecución compatible|Todavía no se admite |
|Análisis de rendimiento de discos |Compatibilidad con métricas agregadas y por disco |Todavía no se admite |
|Migración      |Proporciona herramientas para migrar desde máquinas virtuales de Azure Resource Manager no administradas existentes sin necesidad de volver a crear la máquina virtual  |Todavía no se admite |

> [!NOTE]  
> Las E/S por segundo y el rendimiento de Managed Disks en Azure Stack es un número extremo, en lugar de un número aprovisionado, que puede resultar afectado por el hardware y las cargas de trabajo que se ejecutan en Azure Stack.

## <a name="metrics"></a>Métricas

También hay diferencias en las métricas de almacenamiento:

- Con Azure Stack, los datos de transacción de las métricas de almacenamiento no distinguen el ancho de banda de red interna ni externa.
- Los datos de transacción de Azure Stack en las métricas de almacenamiento no incluyen el acceso de la máquina virtual a los discos montados.

## <a name="api-versions"></a>Versiones de API

Managed Disks de Azure Stack admite las versiones de API siguientes:

- 2017-03-30
- 2017-12-01

## <a name="managed-images"></a>Imágenes administradas

Azure Stack admite las *imágenes administradas*, que le permiten crear un objeto de imagen administrada en una VM generalizada (tanto administrada como no administrada) que, de aquí en adelante, solo puede crear VM de discos administrados. Las imágenes administradas permiten los dos escenarios siguientes:

- Tiene VM no administradas generalizadas y quiere usar discos administrados a partir de ahora.
- Tiene una VM administrada generalizada y quiere crear varias VM administradas similares.

### <a name="migrate-unmanaged-vms-to-managed-disks"></a>Migración de VM a discos administrados

Siga las instrucciones que se indican [aquí](../../virtual-machines/windows/capture-image-resource.md#create-an-image-from-a-vhd-in-a-storage-account) para crear una imagen administrada a partir de un VHD generalizado en una cuenta de almacenamiento. Esta imagen puede usarse para crear VM administradas en el futuro.

### <a name="create-managed-image-from-vm"></a>Creación de una imagen administrada a partir de una VM

Después de crear una imagen a partir de una VM de disco administrado existente mediante el script que se indica [aquí](../../virtual-machines/windows/capture-image-resource.md#create-an-image-from-a-managed-disk-using-powershell), el siguiente script de ejemplo crea una VM de Linux similar a partir de un objeto de imagen existente:

```powershell
# Variables for common values
$resourceGroup = "myResourceGroup"
$location = "redmond"
$vmName = "myVM"
$imagerg = "managedlinuxrg"
$imagename = "simplelinuxvmm-image-2019122"

# Create user object
$cred = Get-Credential -Message "Enter a username and password for the virtual machine."

# Create a resource group
New-AzureRmResourceGroup -Name $resourceGroup -Location $location

# Create a subnet configuration
$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name mySubnet -AddressPrefix 192.168.1.0/24

# Create a virtual network
$vnet = New-AzureRmVirtualNetwork -ResourceGroupName $resourceGroup -Location $location `
  -Name MYvNET -AddressPrefix 192.168.0.0/16 -Subnet $subnetConfig

# Create a public IP address and specify a DNS name
$pip = New-AzureRmPublicIpAddress -ResourceGroupName $resourceGroup -Location $location `
  -Name "mypublicdns$(Get-Random)" -AllocationMethod Static -IdleTimeoutInMinutes 4

# Create an inbound network security group rule for port 3389
$nsgRuleRDP = New-AzureRmNetworkSecurityRuleConfig -Name myNetworkSecurityGroupRuleRDP  -Protocol Tcp `
  -Direction Inbound -Priority 1000 -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * `
  -DestinationPortRange 3389 -Access Allow

# Create a network security group
$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $resourceGroup -Location $location `
  -Name myNetworkSecurityGroup -SecurityRules $nsgRuleRDP

# Create a virtual network card and associate with public IP address and NSG
$nic = New-AzureRmNetworkInterface -Name myNic -ResourceGroupName $resourceGroup -Location $location `
  -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id -NetworkSecurityGroupId $nsg.Id

$image = get-azurermimage -ResourceGroupName $imagerg -ImageName $imagename
# Create a virtual machine configuration
$vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize Standard_D1 | `
Set-AzureRmVMOperatingSystem -Linux -ComputerName $vmName -Credential $cred | `
Set-AzureRmVMSourceImage -Id $image.Id | `
Add-AzureRmVMNetworkInterface -Id $nic.Id

# Create a virtual machine
New-AzureRmVM -ResourceGroupName $resourceGroup -Location $location -VM $vmConfig
```

Para obtener más información, consulte los artículos de imagen administrada de Azure [Captura de una imagen administrada de una máquina virtual generalizada en Azure](../../virtual-machines/windows/capture-image-resource.md) y [Creación de una máquina virtual a partir de una imagen administrada](../../virtual-machines/windows/create-vm-generalized-managed.md).

## <a name="configuration"></a>Configuración

Después de aplicar la actualización 1808 o posterior, debe realizar la siguiente configuración antes de usar Managed Disks:

- Si una suscripción se ha creado antes de la actualización 1808, siga estos pasos para actualizar la suscripción. En caso contrario, la implementación de VM en esta suscripción podría producir un error con un mensaje de error "Error interno en el administrador de discos".
   1. En el portal del inquilino, vaya a **Suscripciones** y busque la suscripción. Haga clic en **Proveedores de recursos**, después en **Microsoft.Compute** y luego en **Volver a registrar**.
   2. En la misma suscripción, vaya a **Control de acceso (IAM)**, y compruebe que **Azure Stack – Managed Disk** (Azure Stack - Disco administrado) aparece en la lista.
- Si usa un entorno de varios inquilinos, pida a su operador de nube (puede ser de su propia organización o del proveedor de servicios) que vuelva a configurar cada uno de los directorios de invitado siguiendo los pasos que se indican en [este artículo](../azure-stack-enable-multitenancy.md#registering-azure-stack-with-the-guest-directory). En caso contrario, la implementación de VM en una suscripción asociada con ese directorio de invitado podría producir un error con un mensaje de error "Error interno en el administrador de discos".

## <a name="next-steps"></a>Pasos siguientes

- [Más información sobre máquinas virtuales de Azure Stack](azure-stack-compute-overview.md)
