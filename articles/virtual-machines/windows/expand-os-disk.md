---
title: Expansión de la unidad de sistema operativo de una máquina virtual de Windows en Azure | Microsoft Docs
description: Expanda el tamaño de la unidad de sistema operativo de una máquina virtual con Azure PowerShell en el modelo de implementación de Resource Manager.
services: virtual-machines-windows
documentationcenter: ''
author: kirpasingh
manager: roshar
editor: ''
tags: azure-resource-manager
ms.assetid: d9edfd9f-482f-4c0b-956c-0d2c2c30026c
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 07/05/2018
ms.author: kirpas
ms.component: disks
ms.openlocfilehash: 1c96f51a49cd21c00f866af0b767ceb3e0a9ce2d
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/23/2019
ms.locfileid: "54470022"
---
# <a name="how-to-expand-the-os-drive-of-a-virtual-machine"></a>Cómo ampliar la unidad de sistema operativo de una máquina virtual

Cuando se crea una nueva máquina virtual (VM) en un grupo de recursos mediante la implementación de una imagen de [Azure Marketplace](https://azure.microsoft.com/marketplace/), la unidad del sistema operativo predeterminada suele tener 127 GB (algunas imágenes son más pequeñas de manera predeterminada). Aunque es posible agregar discos de datos a la máquina virtual (la cantidad depende de la SKU que haya elegido) y, además, se recomienda instalar aplicaciones y cargas de trabajo intensivas de CPU en estos discos de anexo, a menudo los clientes necesitan expandir la unidad del sistema operativo para admitir determinados escenarios, como los siguientes:

- Compatibilidad con aplicaciones heredadas que instalan componentes en la unidad del sistema operativo.
- Migración de una máquina virtual o un equipo físico desde local con una unidad del sistema operativo más grande.


> [!IMPORTANT]
> Cambiar el tamaño del disco del sistema operativo de una máquina virtual de Azure hará que se reinicie.
>
> Después de expandir los discos, necesita [expandir el volumen dentro del sistema operativo](#expand-the-volume-within-the-os) para aprovechar el disco más grande.
> 

## <a name="resize-a-managed-disk"></a>Cambio de tamaño de un disco administrado

Abra la ventana de Powershell o Powershell ISE en el modo administrativo y siga estos pasos:

1. Inicie sesión en su cuenta de Microsoft Azure en el modo de administración de recursos y seleccione su suscripción de la siguiente manera:
   
   ```powershell
   Connect-AzureRmAccount
   Select-AzureRmSubscription –SubscriptionName 'my-subscription-name'
   ```
2. Configure el nombre de grupo de recursos y el nombre de la máquina virtual de la siguiente forma:
   
   ```powershell
   $rgName = 'my-resource-group-name'
   $vmName = 'my-vm-name'
   ```
3. Obtenga una referencia a la máquina virtual de la siguiente manera:
   
   ```powershell
   $vm = Get-AzureRmVM -ResourceGroupName $rgName -Name $vmName
   ```
4. Detenga la máquina virtual antes de cambiar el tamaño del disco de la siguiente forma:
   
    ```Powershell
    Stop-AzureRmVM -ResourceGroupName $rgName -Name $vmName
    ```
5. Obtenga una referencia al disco del sistema operativo administrado. Configure el tamaño del disco del sistema operativo en el valor deseado y actualice el disco de la siguiente manera:
   
   ```Powershell
   $disk= Get-AzureRmDisk -ResourceGroupName $rgName -DiskName $vm.StorageProfile.OsDisk.Name
   $disk.DiskSizeGB = 1023
   Update-AzureRmDisk -ResourceGroupName $rgName -Disk $disk -DiskName $disk.Name
   ```   
   > [!WARNING]
   > El nuevo tamaño debe ser mayor que el tamaño de disco existente. El máximo permitido es 2048 GB para discos del sistema operativo. (el blob de VHD se puede expandir más, pero el sistema operativo solo podrá trabajar con los primeros 2048 GB).
   > 
   > 
6. La actualización de la máquina virtual puede tardar unos segundos. Una vez que el comando acabe de ejecutarse, reinicie la máquina virtual de la siguiente forma:
   
   ```Powershell
   Start-AzureRmVM -ResourceGroupName $rgName -Name $vmName
   ```

Eso es todo. Ahora RDP en la máquina virtual, abra Administración de equipos (o Administración de discos) y expanda la unidad utilizando el espacio recién asignado.

## <a name="resize-an-unmanaged-disk"></a>Cambio de tamaño de un disco no administrado

Abra la ventana de Powershell o Powershell ISE en el modo administrativo y siga estos pasos:

1. Inicie sesión en su cuenta de Microsoft Azure en el modo de administración de recursos y seleccione su suscripción de la siguiente manera:
   
   ```Powershell
   Connect-AzureRmAccount
   Select-AzureRmSubscription –SubscriptionName 'my-subscription-name'
   ```
2. Configure el nombre de grupo de recursos y el nombre de la máquina virtual de la siguiente forma:
   
   ```Powershell
   $rgName = 'my-resource-group-name'
   $vmName = 'my-vm-name'
   ```
3. Obtenga una referencia a la máquina virtual de la siguiente manera:
   
   ```Powershell
   $vm = Get-AzureRmVM -ResourceGroupName $rgName -Name $vmName
   ```
4. Detenga la máquina virtual antes de cambiar el tamaño del disco de la siguiente forma:
   
    ```Powershell
    Stop-AzureRmVM -ResourceGroupName $rgName -Name $vmName
    ```
5. Configure el tamaño del disco del sistema operativo no administrado en el valor deseado y actualice la máquina virtual de la siguiente manera:
   
   ```Powershell
   $vm.StorageProfile.OSDisk.DiskSizeGB = 1023
   Update-AzureRmVM -ResourceGroupName $rgName -VM $vm
   ```
   
   > [!WARNING]
   > El nuevo tamaño debe ser mayor que el tamaño de disco existente. El máximo permitido es 2048 GB para discos del sistema operativo. (el blob de VHD se puede expandir más, pero el sistema operativo solo podrá trabajar con los primeros 2048 GB).
   > 
   > 
   
6. La actualización de la máquina virtual puede tardar unos segundos. Una vez que el comando acabe de ejecutarse, reinicie la máquina virtual de la siguiente forma:
   
   ```Powershell
   Start-AzureRmVM -ResourceGroupName $rgName -Name $vmName
   ```


## <a name="scripts-for-os-disk"></a>Scripts para el disco del sistema operativo

A continuación se muestra el script completo para su referencia tanto para discos administrados como no administrados:


**Discos administrados**

```Powershell
Connect-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName 'my-subscription-name'
$rgName = 'my-resource-group-name'
$vmName = 'my-vm-name'
$vm = Get-AzureRmVM -ResourceGroupName $rgName -Name $vmName
Stop-AzureRmVM -ResourceGroupName $rgName -Name $vmName
$disk= Get-AzureRmDisk -ResourceGroupName $rgName -DiskName $vm.StorageProfile.OsDisk.Name
$disk.DiskSizeGB = 1023
Update-AzureRmDisk -ResourceGroupName $rgName -Disk $disk -DiskName $disk.Name
Start-AzureRmVM -ResourceGroupName $rgName -Name $vmName
```

**Discos no administrados**

```powershell
Connect-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName 'my-subscription-name'
$rgName = 'my-resource-group-name'
$vmName = 'my-vm-name'
$vm = Get-AzureRmVM -ResourceGroupName $rgName -Name $vmName
Stop-AzureRmVM -ResourceGroupName $rgName -Name $vmName
$vm.StorageProfile.OSDisk.DiskSizeGB = 1023
Update-AzureRmVM -ResourceGroupName $rgName -VM $vm
Start-AzureRmVM -ResourceGroupName $rgName -Name $vmName
```

## <a name="resizing-data-disks"></a>Cambio de tamaño de los discos de datos

Este artículo se centra principalmente en expandir el disco del sistema operativo de la máquina virtual, pero el script también puede utilizarse para expandir los discos de datos asociados a la máquina virtual. Por ejemplo, para expandir el primer disco de datos conectado a la máquina virtual, reemplace el objeto `OSDisk` de `StorageProfile` por la matriz `DataDisks` y utilice un índice numérico para obtener una referencia al primer disco de datos conectado, como se muestra a continuación:

**Disco administrado**

```powershell
$disk= Get-AzureRmDisk -ResourceGroupName $rgName -DiskName $vm.StorageProfile.DataDisks[0].Name
$disk.DiskSizeGB = 1023
```


**Disco no administrado**

```powershell
$vm.StorageProfile.DataDisks[0].DiskSizeGB = 1023
```



Del mismo modo, puede hacer referencia a otros discos de datos conectados a la máquina virtual, ya sea mediante un índice, como se muestra arriba, o con la propiedad del disco **Name**:


**Disco administrado**

```powershell
(Get-AzureRmDisk -ResourceGroupName $rgName -DiskName ($vm.StorageProfile.DataDisks | Where ({$_.Name -eq 'my-second-data-disk'})).Name).DiskSizeGB = 1023
```

**Disco no administrado**

```powershell
($vm.StorageProfile.DataDisks | Where ({$_.Name -eq 'my-second-data-disk'}).DiskSizeGB = 1023
```

## <a name="expand-the-volume-within-the-os"></a>Expansión del volumen en el sistema operativo

Cuando haya expandido el disco para la máquina virtual, deberá entrar en el sistema operativo y expandir el volumen para abarcar el nuevo espacio. Existen varios métodos para expandir una partición. En esta sección se trata la conexión de la máquina virtual mediante una conexión RDP para expandir la partición mediante **DiskPart**.

1. Abra una conexión RDP a la máquina virtual.

2.  Abra un símbolo del sistema y escriba **diskpart**.

2.  En el símbolo del sistema **DISKPART**, escriba `list volume`. Tome nota del volumen que desea extender.

3.  En el símbolo del sistema **DISKPART**, escriba `select volume <volumenumber>`. Este comando selecciona el volumen *númeroDeVolumen* que desea extender en el espacio vacío contiguo del mismo disco.

4.  En el símbolo del sistema **DISKPART**, escriba `extend [size=<size>]`. Este comando extiende el volumen seleccionado por *tamaño* en megabytes (MB).


## <a name="next-steps"></a>Pasos siguientes

También puede asociar discos mediante [Azure Portal](attach-managed-disk-portal.md).
