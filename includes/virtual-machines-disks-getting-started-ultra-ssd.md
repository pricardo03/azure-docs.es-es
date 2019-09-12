---
title: archivo de inclusión
description: archivo de inclusión
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 08/15/2019
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 3f910a3d0466153bd60fe23ef2f9f656cac292ee
ms.sourcegitcommit: 083aa7cc8fc958fc75365462aed542f1b5409623
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/11/2019
ms.locfileid: "70919743"
---
# <a name="using-azure-ultra-disks"></a>Uso de discos Ultra de Azure

Los discos Ultra de Azure ofrecen un alto rendimiento, IOPS elevadas y un almacenamiento en disco coherente y de baja latencia para máquinas virtuales IaaS de Azure. En esta nueva oferta se proporciona un rendimiento exclusivo que se encuentra en los mismos niveles de disponibilidad que nuestras ofertas de discos existentes. Una ventaja importante de los discos Ultra es la posibilidad de cambiar dinámicamente el rendimiento del disco SSD junto con sus cargas de trabajo sin tener que reiniciar las máquinas virtuales. Además, los discos Ultra son adecuados para cargas de trabajo con grandes cantidades de datos, como SAP HANA, bases de datos de nivel superior y cargas de trabajo que admitan muchas transacciones.

## <a name="check-if-your-subscription-has-access"></a>Compruebe si la suscripción tiene acceso

Si ya se registró para los discos Ultra y desea verificar si su suscripción está habilitada para dichos discos, use cualquiera de los siguientes comandos: 

CLI: `az feature show --namespace Microsoft.Compute --name UltraSSD`

PowerShell: `Get-AzProviderFeature -ProviderNamespace Microsoft.Compute -FeatureName UltraSSD`

Si la suscripción está habilitada, la salida debe tener un aspecto similar al siguiente:

```bash
{
  "id": "/subscriptions/<yoursubID>/providers/Microsoft.Features/providers/Microsoft.Compute/features/UltraSSD",
  "name": "Microsoft.Compute/UltraSSD",
  "properties": {
    "state": "Registered"
  },
  "type": "Microsoft.Features/providers/features"
}
```

## <a name="determine-your-availability-zone"></a>Determinación de la zona de disponibilidad

Una vez aprobada, deberá determinar en qué zona de disponibilidad se encuentra para poder usar los discos Ultra. Ejecute cualquiera de los siguientes comandos para determinar la zona en la que se va a implementar el disco Ultra y asegúrese de reemplazar primero los valores de **region**, **vmSize** y **subscription**:

CLI:

```bash
$subscription = "<yourSubID>"
$region = "<yourLocation>, example value is southeastasia"
$vmSize = "<yourVMSize>, example value is Standard_E64s_v3"

az vm list-skus --resource-type virtualMachines  --location $region --query "[?name=='$vmSize'].locationInfo[0].zoneDetails[0].Name" --subscription $subscription
```

PowerShell:

```powershell
$region = "southeastasia"
$vmSize = "Standard_E64s_v3"
(Get-AzComputeResourceSku | where {$_.Locations.Contains($region) -and ($_.Name -eq $vmSize) -and $_.LocationInfo[0].ZoneDetails.Count -gt 0})[0].LocationInfo[0].ZoneDetails
```

La respuesta será similar al formulario siguiente, donde X es la zona que se utilizará para la implementación en la región elegida. X podría ser 1, 2 o 3. Actualmente, solo las tres regiones siguientes admiten discos Ultra: Este de EE. UU. 2, Sudeste Asiático y Norte de Europa.

Conserve el valor de **Zones**, ya que representa la zona de disponibilidad y la necesitará para implementar un disco Ultra.

|ResourceType  |NOMBRE  |Location  |Zones  |Restricción  |Capacidad  |Valor  |
|---------|---------|---------|---------|---------|---------|---------|
|disks     |UltraSSD_LRS         |eastus2         |X         |         |         |         |

> [!NOTE]
> Si no hubo respuesta del comando, eso significa que el registro de la característica está pendiente o que se está usando una versión antigua de la CLI o PowerShell.

Ahora que sabe en qué zona se va a realizar la implementación, siga los pasos de implementación de este artículo para implementar una máquina virtual con un disco Ultra conectado o conectar un disco Ultra a una máquina virtual existente.

## <a name="deploy-an-ultra-disk-using-azure-resource-manager"></a>Implementación de un disco Ultra con Azure Resource Manager

En primer lugar, determine el tamaño de la máquina virtual que se va a implementar. Por ahora, solo las familias de máquinas virtuales DsV3 y EsV3 admiten discos Ultra. Consulte la segunda tabla de este [blog](https://azure.microsoft.com/blog/introducing-the-new-dv3-and-ev3-vm-sizes/) para obtener más información acerca de estos tamaños de máquina virtual.

Si quiere crear una máquina virtual con varios discos Ultra, vea el ejemplo [Creación de una máquina virtual con varios discos Ultra](https://aka.ms/ultradiskArmTemplate).

Si piensa usar su propia plantilla, asegúrese de que **apiVersion** para `Microsoft.Compute/virtualMachines` y `Microsoft.Compute/Disks` se establece como `2018-06-01` (o posterior).

Establezca la SKU de disco en **UltraSSD_LRS**, luego establezca la capacidad de disco, IOPS, zona de disponibilidad y rendimiento en MBps para crear un disco ultra.

Cuando se aprovisiona la máquina virtual, puede realizar una partición y dar formato a los discos de datos y configurarlos para las cargas de trabajo.

## <a name="deploy-an-ultra-disk-using-cli"></a>Implementación de un disco Ultra mediante la CLI

En primer lugar, determine el tamaño de la máquina virtual que se va a implementar. Por ahora, solo las familias de máquinas virtuales DsV3 y EsV3 admiten discos Ultra. Consulte la segunda tabla de este [blog](https://azure.microsoft.com/blog/introducing-the-new-dv3-and-ev3-vm-sizes/) para obtener más información acerca de estos tamaños de máquina virtual.

Para conectar un disco Ultra, debe crear una máquina virtual que pueda usar discos Ultra.

Reemplace o establezca las variables **$vmname**, **$rgname**, **$diskname**, **$location**, **$password** y **$user** con sus propios valores. Establezca **$zone** en el valor de la zona de disponibilidad que ha obtenido al [inicio de este artículo](#determine-your-availability-zone). Después, ejecute el siguiente comando de la CLI para crear una máquina virtual habilitada para ultra:

```azurecli-interactive
az vm create --subscription $subscription -n $vmname -g $rgname --image Win2016Datacenter --ultra-ssd-enabled true --zone $zone --authentication-type password --admin-password $password --admin-username $user --size Standard_D4s_v3 --location $location
```

### <a name="create-an-ultra-disk-using-cli"></a>Creación de un disco Ultra mediante la CLI

Ahora que ya tiene una máquina virtual en la que se pueden colocar discos Ultra, puede crear un disco Ultra y conectarlo a dicha máquina virtual.

```azurecli-interactive
$location="eastus2"
$subscription="xxx"
$rgname="ultraRG"
$diskname="ssd1"
$vmname="ultravm1"
$zone=123

#create an ultra disk
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

## <a name="attach-an-ultra-disk-to-a-vm-using-cli"></a>Conexión de un disco Ultra a una máquina virtual mediante la CLI

Como alternativa, si la máquina virtual existente se encuentra en una región o zona de disponibilidad que puede usar discos Ultra, puede usar este tipo de discos sin tener que crear una nueva máquina virtual.

```bash
$rgName = "<yourResourceGroupName>"
$vmName = "<yourVMName>"
$diskName = "<yourDiskName>"
$subscriptionId = "<yourSubscriptionID>"

az vm disk attach -g $rgName --vm-name $vmName --disk $diskName --subscription $subscriptionId
```

### <a name="adjust-the-performance-of-an-ultra-disk-using-cli"></a>Ajuste del rendimiento de un disco Ultra mediante la CLI

Los discos Ultra ofrecen una capacidad única que permite ajustar su rendimiento. El comando siguiente describe cómo se usa esta característica:

```azurecli-interactive
az disk update `
--subscription $subscription `
--resource-group $rgname `
--name $diskName `
--set diskIopsReadWrite=80000 `
--set diskMbpsReadWrite=800
```

## <a name="deploy-an-ultra-disk-using-powershell"></a>Implementación de un disco Ultra mediante PowerShell

En primer lugar, determine el tamaño de la máquina virtual que se va a implementar. Por ahora, solo las familias de máquinas virtuales DsV3 y EsV3 admiten discos Ultra. Consulte la segunda tabla de este [blog](https://azure.microsoft.com/blog/introducing-the-new-dv3-and-ev3-vm-sizes/) para obtener más información acerca de estos tamaños de máquina virtual.

Para usar discos Ultra, debe crear una máquina virtual que sea capaz de usar discos Ultra. Reemplace o establezca las variables **$resourcegroup** y **$vmName** con sus propios valores. Establezca **$zone** en el valor de la zona de disponibilidad que ha obtenido al [inicio de este artículo](#determine-your-availability-zone). Después, ejecute el siguiente comando de [New-AzVm](/powershell/module/az.compute/new-azvm) para crear una máquina virtual ultra habilitada:

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

### <a name="create-an-ultra-disk-using-powershell"></a>Creación de un disco Ultra mediante PowerShell

Ahora que ya tiene una máquina virtual que puede usar discos Ultra, puede crear un disco Ultra y conectarlo a dicha máquina virtual:

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

## <a name="attach-an-ultra-disk-to-a-vm-using-powershell"></a>Conexión de un disco Ultra a una máquina virtual mediante PowerShell

Como alternativa, si la máquina virtual existente se encuentra en una región o zona de disponibilidad que puede usar discos Ultra, puede usar este tipo de discos sin tener que crear una nueva máquina virtual.

```powershell
# add disk to VM
$subscription = "<yourSubscriptionID>"
$resourceGroup = "<yourResourceGroup>"
$vmName = "<yourVMName>"
$diskName = "<yourDiskName>"
$lun = 1
Login-AzureRMAccount -SubscriptionId $subscription
$vm = Get-AzVM -ResourceGroupName $resourceGroup -Name $vmName
$disk = Get-AzDisk -ResourceGroupName $resourceGroup -Name $diskName
$vm = Add-AzVMDataDisk -VM $vm -Name $diskName -CreateOption Attach -ManagedDiskId $disk.Id -Lun $lun
Update-AzVM -VM $vm -ResourceGroupName $resourceGroup
```

### <a name="adjust-the-performance-of-an-ultra-disk-using-powershell"></a>Ajuste del rendimiento de un disco Ultra mediante PowerShell

Los discos Ultra tienen una capacidad única que permite ajustar su rendimiento. El comando siguiente es un ejemplo que ajusta el rendimiento sin tener que desasociar el disco:

```powershell
$diskupdateconfig = New-AzDiskUpdateConfig -DiskMBpsReadWrite 2000
Update-AzDisk -ResourceGroupName $resourceGroup -DiskName $diskName -DiskUpdate $diskupdateconfig
```

## <a name="next-steps"></a>Pasos siguientes

Si quiere probar el nuevo tipo de disco, [solicite acceso con esta encuesta](https://aka.ms/UltraDiskSignup).