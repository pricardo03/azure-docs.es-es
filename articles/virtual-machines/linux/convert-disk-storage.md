---
title: Convertir Azure managed disks el almacenamiento de estándar a Premium a estándar o Premium | Microsoft Docs
description: Cómo convertir Azure managed disks almacenamiento de estándar a Premium a estándar o Premium mediante el uso de la CLI de Azure.
services: virtual-machines-linux
documentationcenter: ''
author: roygara
manager: twooley
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 07/12/2018
ms.author: rogarana
ms.subservice: disks
ms.openlocfilehash: 2e7eb455a53abbe2df6ff72f091a599665732429
ms.sourcegitcommit: 37343b814fe3c95f8c10defac7b876759d6752c3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/24/2019
ms.locfileid: "63765748"
---
# <a name="convert-azure-managed-disks-storage-from-standard-to-premium-or-premium-to-standard"></a>Convertir Azure managed disks el almacenamiento de estándar a Premium a estándar o Premium

Hay cuatro [tipos de disco](disks-types.md) para Azure managed disks: Estándar, Premium SSD, SSD estándar y almacenamiento en disco Ultra Azure HDD. Puede cambiar fácilmente entre SSD Premium, estándar de SSD y HDD estándar según sus necesidades de rendimiento con poco tiempo de inactividad. Esta funcionalidad no se admite para discos no administrados o Ultra almacenamiento en disco. Pero le resultará muy fácil [convert no administrados a discos administrados](convert-unmanaged-to-managed-disks.md) para poder cambiar entre los tipos de disco.

En este artículo se muestra cómo convertir managed disks de estándar a Premium a estándar o Premium mediante el uso de la CLI de Azure. Para instalar o actualizar la herramienta, consulte [instalar la CLI de Azure](/cli/azure/install-azure-cli).

## <a name="before-you-begin"></a>Antes de empezar

* Conversión de disco requiere un reinicio de la máquina virtual (VM), por lo que programe la migración de su almacenamiento en disco durante una ventana de mantenimiento existente previamente.
* Discos no administrados, primera [conversión a discos administrados](convert-unmanaged-to-managed-disks.md) por lo que puede alternar entre las opciones de almacenamiento.


## <a name="switch-all-managed-disks-of-a-vm-between-premium-and-standard"></a>Cambiar todos los discos administrados de una máquina virtual entre estándar y Premium

En este ejemplo se muestra cómo convertir todos los discos de la máquina virtual de estándar a Premium storage o Premium a Standard storage. Para usar Managed Disks premium, la máquina virtual debe usar un [Tamaño de máquina virtual](sizes.md) que admita Premium Storage. En este ejemplo también se pasa a un tamaño que admita Premium storage.

 ```azurecli

#resource group that contains the virtual machine
rgName='yourResourceGroup'

#Name of the virtual machine
vmName='yourVM'

#Premium capable size 
#Required only if converting from Standard to Premium
size='Standard_DS2_v2'

#Choose between Standard_LRS and Premium_LRS based on your scenario
sku='Premium_LRS'

#Deallocate the VM before changing the size of the VM
az vm deallocate --name $vmName --resource-group $rgName

#Change the VM size to a size that supports Premium storage 
#Skip this step if converting storage from Premium to Standard
az vm resize --resource-group $rgName --name $vmName --size $size

#Update the SKU of all the data disks 
az vm show -n $vmName -g $rgName --query storageProfile.dataDisks[*].managedDisk -o tsv \
 | awk -v sku=$sku '{system("az disk update --sku "sku" --ids "$1)}'

#Update the SKU of the OS disk
az vm show -n $vmName -g $rgName --query storageProfile.osDisk.managedDisk -o tsv \
| awk -v sku=$sku '{system("az disk update --sku "sku" --ids "$1)}'

az vm start --name $vmName --resource-group $rgName

```
## <a name="switch-individual-managed-disks-between-standard-and-premium"></a>Cambiar los discos administrados individuales entre estándar y Premium

La carga de trabajo de desarrollo y pruebas, puede tener una combinación de los discos estándar y Premium para reducir los costos. Puede elegir actualizar sólo esos discos que tienen un mejor rendimiento. En este ejemplo se muestra cómo convertir un único disco de máquina virtual de estándar a Premium storage o Premium a Standard storage. Para usar Managed Disks premium, la máquina virtual debe usar un [Tamaño de máquina virtual](sizes.md) que admita Premium Storage. En este ejemplo también se pasa a un tamaño que admita Premium storage.

 ```azurecli

#resource group that contains the managed disk
rgName='yourResourceGroup'

#Name of your managed disk
diskName='yourManagedDiskName'

#Premium capable size 
#Required only if converting from Standard to Premium
size='Standard_DS2_v2'

#Choose between Standard_LRS and Premium_LRS based on your scenario
sku='Premium_LRS'

#Get the parent VM Id 
vmId=$(az disk show --name $diskName --resource-group $rgName --query managedBy --output tsv)

#Deallocate the VM before changing the size of the VM
az vm deallocate --ids $vmId 

#Change the VM size to a size that supports Premium storage 
#Skip this step if converting storage from Premium to Standard
az vm resize --ids $vmId --size $size

# Update the SKU
az disk update --sku $sku --name $diskName --resource-group $rgName 

az vm start --ids $vmId 
```

## <a name="switch-managed-disks-between-standard-hdd-and-standard-ssd"></a>Cambiar los discos administrados entre estándar HDD y SSD estándar

En este ejemplo se muestra cómo convertir un único disco de máquina virtual de estándar de HDD a SSD estándar o de estándar SSD a HDD estándar.

 ```azurecli

#resource group that contains the managed disk
rgName='yourResourceGroup'

#Name of your managed disk
diskName='yourManagedDiskName'

#Choose between Standard_LRS and StandardSSD_LRS based on your scenario
sku='StandardSSD_LRS'

#Get the parent VM ID 
vmId=$(az disk show --name $diskName --resource-group $rgName --query managedBy --output tsv)

#Deallocate the VM before changing the disk type
az vm deallocate --ids $vmId 

# Update the SKU
az disk update --sku $sku --name $diskName --resource-group $rgName 

az vm start --ids $vmId 
```

## <a name="switch-managed-disks-between-standard-and-premium-in-azure-portal"></a>Cambiar los discos administrados entre estándar y Premium en Azure portal

Siga estos pasos:

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
2. Seleccione la máquina virtual en la lista de **máquinas virtuales**.
3. Si no se ha detenido la máquina virtual, seleccione **detener** en la parte superior de la máquina virtual **Overview** panel y espere a que la máquina virtual a detener.
4. En el panel de la máquina virtual, seleccione **discos** en el menú.
5. Seleccione el disco que se va a convertir.
6. Seleccione **configuración** en el menú.
7. Cambiar el **tipo de cuenta** desde **HDD estándar** a **SSD Premium** o desde **SSD Premium** a **HDD estándar**.
8. Seleccione **guardar**y cerrar el panel de disco.

La actualización del tipo de disco es instantánea. Puede reiniciar la máquina virtual después de la conversión.

## <a name="next-steps"></a>Pasos siguientes

Realizar una copia de solo lectura de una máquina virtual mediante el uso de [instantáneas](snapshot-copy-managed-disk.md).
