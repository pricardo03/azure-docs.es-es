---
title: archivo de inclusión
description: archivo de inclusión
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 09/23/2019
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: e39f294f7902eabef401d4c8145f4f19a07f267f
ms.sourcegitcommit: 3fa4384af35c64f6674f40e0d4128e1274083487
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2019
ms.locfileid: "71224581"
---
# <a name="creating-an-incremental-snapshot-preview-for-managed-disks"></a>Creación de una instantánea incremental (versión preliminar) para discos administrados

Las instantáneas incrementales (versión preliminar) son copias de seguridad en un momento dado de los discos administrados que, cuando se realizan, solo constan de todos los cambios desde la última instantánea. Al intentar descargar o usar una instantánea incremental, se utiliza el VHD completo. Esta nueva funcionalidad para las instantáneas de discos administrados puede permitir que sean más rentables, ya que no es necesario almacenar todo el disco con cada instantánea individual a menos que decida hacerlo expresamente. Al igual que las instantáneas normales, las instantáneas incrementales se pueden usar para crear un disco administrado completo o para realizar una instantánea normal.

Hay algunas diferencias entre una instantánea incremental y una instantánea normal. Las instantáneas incrementales usan siempre el almacenamiento de discos HDD estándar, independientemente del tipo de almacenamiento del disco, mientras que las instantáneas periódicas pueden usar discos SSD Premium. Si usa instantáneas periódicas en Premium Storage para escalar verticalmente implementaciones de máquinas virtuales, le recomendamos que use imágenes personalizadas en el almacenamiento estándar de [Shared Image Gallery](../articles/virtual-machines/linux/shared-image-galleries.md). Le ayudará a lograr una escala más masiva con un costo más bajo. Además, las instantáneas incrementales pueden ofrecer mejor confiabilidad con el [almacenamiento con redundancia de zona](../articles/storage/common/storage-redundancy-zrs.md). Si el almacenamiento con redundancia de zona está disponible en la región seleccionada, una instantánea incremental lo usará automáticamente. Si el almacenamiento con redundancia de zona no está disponible en la región, la instantánea tendrá como valor predeterminado el [almacenamiento con redundancia local](../articles/storage/common/storage-redundancy-lrs.md). Puede invalidar este comportamiento y seleccionar uno manualmente, pero no es recomendable.

Las instantáneas incrementales también ofrecen una funcionalidad diferencial, que está disponible de forma única para los discos administrados. Permiten obtener los cambios entre dos instantáneas incrementales de los mismos discos administrados hasta el nivel de bloque. Puede usar esta funcionalidad para reducir la superficie de los datos al copiar instantáneas entre regiones.

Si aún no se ha suscrito a la versión preliminar y le gustaría empezar a usar instantáneas incrementales, envíenos un correo electrónico a AzureDisks@microsoft.com para acceder a la versión preliminar pública.

## <a name="restrictions"></a>Restricciones

- Actualmente, las instantáneas incrementales no se pueden crear después de cambiar el tamaño de un disco.
- Las instantáneas incrementales no se pueden mover entre suscripciones.
- En este momento, solo se pueden generar URI de SAS de hasta cinco instantáneas de una determinada familia de instantáneas en un momento dado.
- No se puede crear una instantánea incremental para un disco determinado fuera de la suscripción de ese disco.
- Se pueden crear hasta siete instantáneas incrementales por disco cada cinco minutos.
- Se pueden crear un total de 200 instantáneas incrementales para un único disco.

## <a name="powershell"></a>PowerShell

Puede usar Azure PowerShell para crear y administrar recursos compartidos de archivos. Puede instalar localmente la versión más reciente de PowerShell. Necesitará la versión más reciente de Azure PowerShell y el siguiente comando la instalará o actualizará la instalación existente a la más reciente:

```PowerShell
Install-Module -Name Az -AllowClobber -Scope CurrentUser
```

Una vez instalada, inicie sesión en PowerShell con `az login`.

Reemplace `<yourDiskNameHere>`, `<yourResourceGroupNameHere>` y `<yourDesiredSnapShotNameHere>` por sus valores; a continuación, puede usar el script siguiente para crear una instantánea incremental:

```PowerShell
# Get the disk that you need to backup by creating an incremental snapshot
$yourDisk = Get-AzDisk -DiskName <yourDiskNameHere> -ResourceGroupName <yourResourceGroupNameHere>

# Create an incremental snapshot by setting:
# 1. Incremental property
# 2. SourceUri property with the value of the Id property of the disk
$snapshotConfig=New-AzSnapshotConfig -SourceUri $yourDisk.Id -Location $yourDisk.Location -CreateOption Copy -Incremental 
New-AzSnapshot -ResourceGroupName <yourResourceGroupNameHere> -SnapshotName <yourDesiredSnapshotNameHere> -Snapshot $snapshotConfig 

# You can identify incremental snapshots of the same disk by using the SourceResourceId and SourceUniqueId properties of snapshots. 
# SourceResourceId is the Azure Resource Manager resource ID of the parent disk. 
# SourceUniqueId is the value inherited from the UniqueId property of the disk. If you delete a disk and then create a disk with the same name, the value of the UniqueId property will change. 
# Following script shows how to get all the incremental snapshots in a resource group of same disk
$snapshots = Get-AzSnapshot -ResourceGroupName <yourResourceGroupNameHere>

$incrementalSnapshots = New-Object System.Collections.ArrayList
foreach ($snapshot in $snapshots)
{
    
    if($snapshot.Incremental -and $snapshot.CreationData.SourceResourceId -eq $yourDisk.Id -and $snapshot.CreationData.SourceUniqueId -eq $yourDisk.UniqueId){

        $incrementalSnapshots.Add($snapshot)
    }
}

$incrementalSnapshots
```

## <a name="resource-manager-template"></a>Plantilla de Resource Manager

También puede usar plantillas de Azure Resource Manager para crear una instantánea incremental. Deberá asegurarse de que apiVersion esté establecido en **2019-03-01** y que la propiedad incremental también esté establecida en true. El siguiente fragmento de código es un ejemplo de cómo crear una instantánea incremental con las plantillas de Resource Manager:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "diskName": {
      "type": "string",
      "defaultValue": "contosodisk1"
    },
  "diskResourceId": {
    "defaultValue": "<your_managed_disk_resource_ID>",
    "type": "String"
  }
  }, 
  "resources": [
  {
    "type": "Microsoft.Compute/snapshots",
    "name": "[concat( parameters('diskName'),'_snapshot1')]",
    "location": "[resourceGroup().location]",
    "apiVersion": "2019-03-01",
    "properties": {
      "creationData": {
        "createOption": "Copy",
        "sourceResourceId": "[parameters('diskResourceId')]"
      },
      "incremental": true
    }
  }
  ]
}
```

## <a name="cli"></a>CLI

Puede crear una instantánea incremental con la CLI de Azure mediante [az snapshot create](https://docs.microsoft.com/cli/azure/snapshot?view=azure-cli-latest#az-snapshot-create). Por ejemplo, podría ser similar al siguiente:

```bash
az snapshot create -g <exampleResourceGroup> \
-n <exampleSnapshotName> \
-l <exampleLocation> \
--source <exampleVMId> \
--incremental
```

También puede identificar las instantáneas que son incrementales en la CLI mediante el parámetro `--query` en [az snapshot show](https://docs.microsoft.com/cli/azure/snapshot?view=azure-cli-latest#az-snapshot-show). Puede usar ese parámetro para consultar directamente las propiedades **SourceResourceId** y **SourceUniqueId** de las instantáneas. SourceResourceId es el identificador de recursos de Azure Resource Manager del disco principal. **SourceUniqueId** es el valor heredado de la propiedad **UniqueId** del disco. Si elimina un disco y después crea un disco con el mismo nombre, cambiará el valor de la propiedad **UniqueId**.

Los ejemplos de cualquier consulta tendrían el siguiente aspecto:

```bash
az snapshot show -g <exampleResourceGroup> \
-n <yourSnapShotName> \
--query [creationData.sourceResourceId] -o tsv

az snapshot show -g <exampleResourceGroup> \
-n <yourSnapShotName> \
--query [creationData.sourceUniqueId] -o tsv
```

## <a name="next-steps"></a>Pasos siguientes

Si aún no se ha suscrito a la versión preliminar y le gustaría empezar a usar instantáneas incrementales, envíenos un correo electrónico a AzureDisks@microsoft.com para acceder a la versión preliminar pública.
