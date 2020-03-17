---
title: archivo de inclusión
description: archivo de inclusión
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 12/06/2019
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 2316b52115cc23d11dfb26d951a7f1820f2737cc
ms.sourcegitcommit: 8f4d54218f9b3dccc2a701ffcacf608bbcd393a6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/09/2020
ms.locfileid: "78945937"
---
Las instantáneas incrementales (versión preliminar) son copias de seguridad en un momento dado de los discos administrados que, cuando se realizan, solo constan de todos los cambios desde la última instantánea. Al intentar descargar o usar una instantánea incremental, se utiliza el VHD completo. Esta nueva funcionalidad para las instantáneas de discos administrados puede permitir que sean más rentables, ya que no es necesario almacenar todo el disco con cada instantánea individual a menos que decida hacerlo expresamente. Al igual que las instantáneas normales, las instantáneas incrementales se pueden usar para crear un disco administrado completo o para realizar una instantánea normal.

Hay algunas diferencias entre una instantánea incremental y una instantánea normal. Las instantáneas incrementales usan siempre el almacenamiento de discos HDD estándar, independientemente del tipo de almacenamiento del disco, mientras que las instantáneas periódicas pueden usar discos SSD Premium. Si usa instantáneas periódicas en Premium Storage para escalar verticalmente implementaciones de máquinas virtuales, le recomendamos que use imágenes personalizadas en el almacenamiento estándar de [Shared Image Gallery](../articles/virtual-machines/linux/shared-image-galleries.md). Le ayudará a lograr una escala más masiva con un costo más bajo. Además, las instantáneas incrementales pueden ofrecer mejor confiabilidad con el [almacenamiento con redundancia de zona](../articles/storage/common/storage-redundancy-zrs.md). Si el almacenamiento con redundancia de zona está disponible en la región seleccionada, una instantánea incremental lo usará automáticamente. Si el almacenamiento con redundancia de zona no está disponible en la región, la instantánea tendrá como valor predeterminado el [almacenamiento con redundancia local](../articles/storage/common/storage-redundancy-lrs.md). Puede invalidar este comportamiento y seleccionar uno manualmente, pero no es recomendable.

Las instantáneas incrementales también ofrecen una funcionalidad diferencial, que está disponible de forma única para los discos administrados. Permiten obtener los cambios entre dos instantáneas incrementales de los mismos discos administrados hasta el nivel de bloque. Puede usar esta funcionalidad para reducir la superficie de los datos al copiar instantáneas entre regiones.

### <a name="supported-regions"></a>Regiones admitidas

Actualmente, solo se admiten las siguientes regiones:

- Disponible como una oferta de disponibilidad general (GA) en las regiones Centro-oeste de EE. UU., Este de EE. UU., Este de EE. UU. 2, Este de Canadá, Centro de Canadá, Norte de Europa y Asia Suroriental.

## <a name="restrictions"></a>Restricciones
- Actualmente, las instantáneas incrementales no se pueden crear después de cambiar el tamaño de un disco (solo durante la versión preliminar).
- Las instantáneas incrementales no se pueden mover entre suscripciones.
- En este momento, solo se pueden generar URI de SAS de hasta cinco instantáneas de una determinada familia de instantáneas en un momento dado.
- No se puede crear una instantánea incremental para un disco determinado fuera de la suscripción de ese disco.
- Se pueden crear hasta siete instantáneas incrementales por disco cada cinco minutos.
- Se pueden crear un total de 200 instantáneas incrementales para un único disco.

## <a name="powershell"></a>PowerShell

Puede usar Azure PowerShell para crear y administrar recursos compartidos de archivos. Necesitará la versión más reciente de Azure PowerShell y el siguiente comando la instalará o actualizará la instalación existente a la más reciente:

```PowerShell
Install-Module -Name Az -AllowClobber -Scope CurrentUser
```

Una vez instalada, inicie sesión en PowerShell con `az login`.

Para crear una instantánea incremental con Azure PowerShell, establezca la configuración con [New-AzSnapShotConfig](https://docs.microsoft.com/powershell/module/az.compute/new-azsnapshotconfig?view=azps-2.7.0) con el parámetro `-Incremental` y, después, páselo como una variable a [New-AzSnapshot](https://docs.microsoft.com/powershell/module/az.compute/new-azsnapshot?view=azps-2.7.0) con el parámetro `-Snapshot`.

Reemplace `<yourDiskNameHere>`, `<yourResourceGroupNameHere>` y `<yourDesiredSnapShotNameHere>` por sus valores; a continuación, puede usar el script siguiente para crear una instantánea incremental:

```PowerShell
# Get the disk that you need to backup by creating an incremental snapshot
$yourDisk = Get-AzDisk -DiskName <yourDiskNameHere> -ResourceGroupName <yourResourceGroupNameHere>

# Create an incremental snapshot by setting the SourceUri property with the value of the Id property of the disk
$snapshotConfig=New-AzSnapshotConfig -SourceUri $yourDisk.Id -Location $yourDisk.Location -CreateOption Copy -Incremental 
New-AzSnapshot -ResourceGroupName <yourResourceGroupNameHere> -SnapshotName <yourDesiredSnapshotNameHere> -Snapshot $snapshotConfig 
```

Puede identificar las instantáneas incrementales desde el mismo disco con las propiedades `SourceResourceId` y `SourceUniqueId` de las instantáneas. `SourceResourceId` es el identificador de recursos de Azure Resource Manager del disco principal. `SourceUniqueId` es el valor heredado de la propiedad `UniqueId` del disco. Si fuera a eliminar un disco y después creara otro con el mismo nombre, el valor de la propiedad `UniqueId` cambiaría.

Puede usar `SourceResourceId` y `SourceUniqueId` para crear una lista de todas las instantáneas asociadas a un disco determinado. Reemplace `<yourResourceGroupNameHere>` por su valor y, a continuación, puede usar el ejemplo siguiente para enumerar las instantáneas incrementales existentes:

```PowerShell
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

## <a name="cli"></a>CLI

Puede crear una instantánea incremental con la CLI de Azure, para lo que necesitará la versión más reciente. 

En Windows, el comando siguiente la instalará o actualizará la instalación existente a la más reciente:
```PowerShell
Invoke-WebRequest -Uri https://aka.ms/installazurecliwindows -OutFile .\AzureCLI.msi; Start-Process msiexec.exe -Wait -ArgumentList '/I AzureCLI.msi /quiet'
```
En Linux, la instalación de la CLI variará en función de la versión del sistema operativo.  Consulte [Instalación de la CLI de Azure](https://docs.microsoft.com/cli/azure/install-azure-cli) correspondiente a la versión específica de Linux.

Para crear una instantánea incremental, use [az snapshot create](https://docs.microsoft.com/cli/azure/snapshot?view=azure-cli-latest#az-snapshot-create) con el parámetro `--incremental`.

En el ejemplo siguiente se crea una instantánea incremental; reemplace `<yourDesiredSnapShotNameHere>`, `<yourResourceGroupNameHere>`, `<exampleDiskName>` y `<exampleLocation>` por sus propios valores y, a continuación, ejecute el ejemplo:

```bash
sourceResourceId=$(az disk show -g <yourResourceGroupNameHere> -n <exampleDiskName> --query '[id]' -o tsv)

az snapshot create -g <yourResourceGroupNameHere> \
-n <yourDesiredSnapShotNameHere> \
-l <exampleLocation> \
--source "$sourceResourceId" \
--incremental
```

Puede identificar las instantáneas incrementales desde el mismo disco con las propiedades `SourceResourceId` y `SourceUniqueId` de las instantáneas. `SourceResourceId` es el identificador de recursos de Azure Resource Manager del disco principal. `SourceUniqueId` es el valor heredado de la propiedad `UniqueId` del disco. Si fuera a eliminar un disco y después creara otro con el mismo nombre, el valor de la propiedad `UniqueId` cambiaría.

Puede usar `SourceResourceId` y `SourceUniqueId` para crear una lista de todas las instantáneas asociadas a un disco determinado. En el ejemplo siguiente se enumeran todas las instantáneas incrementales asociadas a un disco determinado, pero se requiere cierta configuración.

En este ejemplo se usa jq para consultar los datos. Para ejecutar el ejemplo, debe [instalar jq](https://stedolan.github.io/jq/download/).

Reemplace `<yourResourceGroupNameHere>` y `<exampleDiskName>` por sus valores, y después puede usar el ejemplo siguiente para enumerar las instantáneas incrementales existentes, siempre y cuando también haya instalado jq:

```bash
sourceUniqueId=$(az disk show -g <yourResourceGroupNameHere> -n <exampleDiskName> --query '[uniqueId]' -o tsv)

 
sourceResourceId=$(az disk show -g <yourResourceGroupNameHere> -n <exampleDiskName> --query '[id]' -o tsv)

az snapshot list -g <yourResourceGroupNameHere> -o json \
| jq -cr --arg SUID "$sourceUniqueId" --arg SRID "$sourceResourceId" '.[] | select(.incremental==true and .creationData.sourceUniqueId==$SUID and .creationData.sourceResourceId==$SRID)'
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

## <a name="next-steps"></a>Pasos siguientes

Si quiere ver un código de ejemplo que muestre la funcionalidad diferencial de las instantáneas incrementales con .NET, consulte [Copia de las copias de seguridad de Azure Managed Disks en otra región con una funcionalidad diferencial de instantáneas incrementales](https://github.com/Azure-Samples/managed-disks-dotnet-backup-with-incremental-snapshots).
