---
title: archivo de inclusión
description: archivo de inclusión
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 05/10/2019
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 742e0028b1f92beb8300cc97f09d8292259fbc0a
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/09/2019
ms.locfileid: "67712460"
---
# <a name="enable-and-deploy-azure-ultra-ssds-preview"></a>Habilitación e implementación de disco SSD ultra de Azure (versión preliminar)

Las unidades de estado sólido (SSD) ultra de Azure (versión preliminar) ofrece un alto rendimiento, IOPS elevadas y un almacenamiento en disco coherente y de baja latencia para máquinas virtuales IaaS de Azure. En esta nueva oferta se proporciona un rendimiento exclusivo que se encuentra en los mismos niveles de disponibilidad que nuestras ofertas de discos existentes. Una ventaja importante de los discos SSD Ultra es la posibilidad de cambiar dinámicamente el rendimiento del disco SSD junto con sus cargas de trabajo sin tener que reiniciar las máquinas virtuales. Asimismo, los discos SSD Ultra son adecuados para cargas de trabajo con grandes cantidades de datos, como SAP HANA, bases de datos de nivel superior y cargas de trabajo que admitan muchas transacciones.

Actualmente, los discos SSD Ultra están en versión preliminar y se debe [inscribir](https://aka.ms/UltraSSDPreviewSignUp) en la vista previa con el fin de tener acceso a ellos.

## <a name="determine-your-availability-zone"></a>Determinación de la zona de disponibilidad

Una vez aprobada, deberá determinar en qué zona de disponibilidad se encuentra, para poder usar los discos SSD Ultra. Ejecute uno de los comandos siguientes para determinar en qué zona de Este de EE. UU. 2 va a implementar el disco Ultra:

PowerShell: `Get-AzComputeResourceSku | where {$_.ResourceType -eq "disks" -and $_.Name -eq "UltraSSD_LRS" }`

CLI: `az vm list-skus --resource-type disks --query "[?name=='UltraSSD_LRS'].locationInfo"`

La respuesta será similar al formulario siguiente, donde X es la zona que se utilizará para la implementación en Este de EE. UU. 2. X podría ser 1, 2 o 3.

Conserve el valor de **Zonas**, ya que representa la zona de disponibilidad y la necesitará para implementar un disco SSD Ultra.

|ResourceType  |NOMBRE  |Location  |Zones  |Restricción  |Capacidad  |Valor  |
|---------|---------|---------|---------|---------|---------|---------|
|disks     |UltraSSD_LRS         |eastus2         |X         |         |         |         |

> [!NOTE]
> Si no hubo respuesta del comando, eso significa que el registro de la característica está pendiente o todavía no se ha aprobado.

Ahora que conoce la zona de implementación, siga los pasos de implementación de este artículo para obtener sus primeras máquinas virtuales implementadas con discos SSD Ultra.

## <a name="deploy-an-ultra-ssd-using-azure-resource-manager"></a>Implementación de un disco SSD Ultra con Azure Resource Manager

En primer lugar, determine el tamaño de la máquina virtual que se va a implementar. Como parte de esta versión preliminar, se admiten solo las familias de máquinas virtuales DsV3 y EsV3. Consulte la segunda tabla de este [blog](https://azure.microsoft.com/blog/introducing-the-new-dv3-and-ev3-vm-sizes/) para obtener más información acerca de estos tamaños de máquina virtual.

Si quiere crear una máquina virtual con varios discos SSD Ultra, vea el ejemplo [Creación de una máquina virtual con varios discos SSD Ultra](https://aka.ms/UltraSSDTemplate).

Si piensa usar su propia plantilla, asegúrese de que **apiVersion** para `Microsoft.Compute/virtualMachines` y `Microsoft.Compute/Disks` se establece como `2018-06-01` (o posterior).

Establezca la SKU de disco en **UltraSSD_LRS**, luego establezca la capacidad de disco, IOPS, zona de disponibilidad y rendimiento en MBps para crear un disco ultra.

Cuando se aprovisiona la máquina virtual, puede realizar una partición y dar formato a los discos de datos y configurarlos para las cargas de trabajo.

## <a name="deploy-an-ultra-ssd-using-cli"></a>Implementación de un disco SSD Ultra mediante la CLI

En primer lugar, determine el tamaño de la máquina virtual que se va a implementar. Como parte de esta versión preliminar, se admiten solo las familias de máquinas virtuales DsV3 y EsV3. Consulte la segunda tabla de este [blog](https://azure.microsoft.com/blog/introducing-the-new-dv3-and-ev3-vm-sizes/) para obtener más información acerca de estos tamaños de máquina virtual.

Para usar discos SSD Ultra, debe crear una máquina virtual que sea capaz de usar discos SSD Ultra.

Reemplace o establezca las variables **$vmname**, **$rgname**, **$diskname**, **$location**, **$password** y **$user** con sus propios valores. Establezca **$zone** en el valor de la zona de disponibilidad que ha obtenido al [inicio de este artículo](#determine-your-availability-zone). Después, ejecute el siguiente comando de la CLI para crear una máquina virtual habilitada para ultra:

```azurecli-interactive
az vm create --subscription $subscription -n $vmname -g $rgname --image Win2016Datacenter --ultra-ssd-enabled true --zone $zone --authentication-type password --admin-password $password --admin-username $user --attach-data-disks $diskname --size Standard_D4s_v3 --location $location
```

### <a name="create-an-ultra-ssd-using-cli"></a>Creación de un disco SSD Ultra mediante la CLI

Ahora que ya tiene una máquina virtual que puede usar discos SSD Ultra, puede crear y adjuntarle un disco SSD Ultra.

```azurecli-interactive
location="eastus2"
subscription="xxx"
rgname="ultraRG"
diskname="ssd1"
vmname="ultravm1"
zone=123

#create an Ultra SSD disk
az disk create `
--subscription $subscription `
-n $diskname `
-g $rgname `
--size-gb 4 `
--location $location `
--zone $zone `
--sku UltraSSD_LRS `
--disk-iops-read-write 1000 `
--disk-mbps-read-write 50
```

### <a name="adjust-the-performance-of-an-ultra-ssd-using-cli"></a>Ajuste del rendimiento de un disco SSD Ultra mediante la CLI

Los discos SSD Ultra ofrecen una capacidad única que permite ajustar su rendimiento. El comando siguiente describe cómo se usa esta característica:

```azurecli-interactive
az disk update `
--subscription $subscription `
--resource-group $rgname `
--name $diskName `
--set diskIopsReadWrite=80000 `
--set diskMbpsReadWrite=800
```

## <a name="deploy-an-ultra-ssd-using-powershell"></a>Implementación de un disco SSD Ultra mediante PowerShell

En primer lugar, determine el tamaño de la máquina virtual que se va a implementar. Como parte de esta versión preliminar, se admiten solo las familias de máquinas virtuales DsV3 y EsV3. Consulte la segunda tabla de este [blog](https://azure.microsoft.com/blog/introducing-the-new-dv3-and-ev3-vm-sizes/) para obtener más información acerca de estos tamaños de máquina virtual.

Para usar discos SSD Ultra, debe crear una máquina virtual que sea capaz de usar discos SSD Ultra. Reemplace o establezca las variables **$resourcegroup** y **$vmName** con sus propios valores. Establezca **$zone** en el valor de la zona de disponibilidad que ha obtenido al [inicio de este artículo](#determine-your-availability-zone). Después, ejecute el siguiente comando de [New-AzVm](/powershell/module/az.compute/new-azvm) para crear una máquina virtual ultra habilitada:

```powershell
New-AzVm `
    -ResourceGroupName $resourcegroup `
    -Name $vmName `
    -Location "eastus2" `
    -Image "Win2016Datacenter" `
    -EnableUltraSSD `
    -size "Standard_D4s_v3" `
    -zone $zone
```

### <a name="create-an-ultra-ssd-using-powershell"></a>Creación de un disco SSD Ultra mediante PowerShell

Ahora que ya tiene una máquina virtual que puede usar discos SSD Ultra, puede crear y adjuntarle un disco SSD Ultra:

```powershell
$diskconfig = New-AzDiskConfig `
-Location 'EastUS2' `
-DiskSizeGB 8 `
-DiskIOPSReadWrite 1000 `
-DiskMBpsReadWrite 100 `
-AccountType UltraSSD_LRS `
-CreateOption Empty `
-zone $zone;

New-AzDisk `
-ResourceGroupName $resourceGroup `
-DiskName 'Disk02' `
-Disk $diskconfig;
```

### <a name="adjust-the-performance-of-an-ultra-ssd-using-powershell"></a>Ajuste del rendimiento de un disco SSD Ultra mediante PowerShell

Los discos SSD Ultra tienen una capacidad única que permiten ajustar su rendimiento. El comando siguiente es un ejemplo que ajusta el rendimiento sin tener que desasociar el disco:

```powershell
$diskupdateconfig = New-AzDiskUpdateConfig -DiskMBpsReadWrite 2000
Update-AzDisk -ResourceGroupName $resourceGroup -DiskName $diskName -DiskUpdate $diskupdateconfig
```

## <a name="next-steps"></a>Pasos siguientes

Si quiere probar el nuevo tipo de disco, [solicite acceso a la versión preliminar con esta encuesta](https://aka.ms/UltraSSDPreviewSignUp).