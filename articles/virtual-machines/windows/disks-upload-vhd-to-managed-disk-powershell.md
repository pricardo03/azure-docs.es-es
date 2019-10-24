---
title: Carga de un disco duro virtual en Azure mediante Azure PowerShell
description: Aprenda a cargar un disco duro virtual en un disco administrado de Azure y copiar un disco administrado entre regiones mediante Azure PowerShell.
author: roygara
ms.author: rogarana
ms.date: 05/06/2019
ms.topic: article
ms.service: virtual-machines-linux
ms.tgt_pltfrm: linux
ms.subservice: disks
ms.openlocfilehash: d193dcd0c0539c2daa7220d915fdc3e02c8ea798
ms.sourcegitcommit: 12de9c927bc63868168056c39ccaa16d44cdc646
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2019
ms.locfileid: "72512431"
---
# <a name="upload-a-vhd-to-azure-using-azure-powershell"></a>Carga de un disco duro virtual en Azure mediante Azure PowerShell

En este artículo se explica cómo cargar un disco duro virtual de una máquina local en un disco administrado de Azure. Antes había que seguir un proceso más complicado, que incluía el almacenamiento provisional de los datos en una cuenta de almacenamiento y la administración de dicha cuenta. Ya no es preciso administrar una cuenta de almacenamiento ni almacenar en ella datos temporalmente para cargar un disco duro virtual. En su lugar, cree un disco administrado vacío y cargue un disco duro virtual directamente en él. Así se simplifica la carga de máquinas virtuales locales en Azure y permite cargar un disco duro virtual hasta 32 TiB directamente en un disco administrado de gran tamaño.

Si va a proporcionar una solución de copia de seguridad para las máquinas virtuales de IaaS en Azure, se recomienda usar la carga directa para restaurar las copias de seguridad de clientes en discos administrados. Si va a cargar un disco duro virtual desde una máquina externa a Azure, las velocidades dependerán del ancho de banda local. Si usa una máquina virtual de Azure, el ancho de banda será el mismo que el de los discos duros estándar.

Actualmente, la carga directa es compatible con los HDD estándar, la unidad de estado sólido estándar y los discos administrados SSD prémium. Aún no es compatible con el almacenamiento en discos Ultra.

## <a name="prerequisites"></a>Requisitos previos

- Descargue la [versión más reciente de AzCopy, v10](../../storage/common/storage-use-azcopy-v10.md#download-and-install-azcopy).
- [Instale el módulo de Azure PowerShell](/powershell/azure/install-Az-ps).
- Si tiene previsto cargar un disco duro virtual desde on-pem: Un disco duro virtual que [se ha preparado para Azure](prepare-for-upload-vhd-image.md), almacenado localmente.
- O bien, un disco administrado en Azure, si desea realizar una acción de copia.

## <a name="create-an-empty-managed-disk"></a>Creación de un disco administrado vacío

Para cargar un disco duro virtual en Azure, deberá crear un disco administrado vacío que esté configurado para este proceso de carga. Antes de crearlo, hay información adicional que debe saber acerca de estos discos.

Este tipo de disco administrado tiene dos estados únicos:

- ReadToUpload, que significa que el disco está listo para recibir una carga pero no se ha generado ninguna [firma de acceso seguro](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1).
- ActiveUpload, que significa que el disco está listo para recibir una carga y se ha generado la SAS.

Mientras esté en cualquiera de estos estados, el disco administrado se facturará al [precio de un HDD estándar](https://azure.microsoft.com/pricing/details/managed-disks/), independientemente del tipo real de disco. Por ejemplo, un P10 se facturará como un S10. Esto sucederá hasta que se llame a `revoke-access` en el disco administrado, que es necesario para conectar el disco a una máquina virtual.

Antes de crear un HDD estándar vacío para cargarlo, necesitará el tamaño de archivo en bytes del disco duro virtual que desea cargar. El código de ejemplo lo recibirá, pero para hacerlo usted puede usar: `$vhdSizeBytes = (Get-Item "<fullFilePathHere>").length`. Este valor se usa al especificar el parámetro **-UploadSizeInBytes**.

Ahora, en el shell local, cree un HDD estándar vacío para la carga mediante la especificación del valor **Upload** en el parámetro **-CreateOption**, así como el parámetro **-UploadSizeInBytes** del cmdlet [New-AzDiskConfig](https://docs.microsoft.com/powershell/module/az.compute/new-azdiskconfig?view=azps-1.8.0). Después, llame a [New-AzDisk](https://docs.microsoft.com/powershell/module/az.compute/new-azdisk?view=azps-1.8.0) para crear el disco:

```powershell
$vhdSizeBytes = (Get-Item "<fullFilePathHere>").length

$diskconfig = New-AzDiskConfig -SkuName 'Standard_LRS' -OsType 'Windows' -UploadSizeInBytes $vhdSizeBytes -Location 'West US' -CreateOption 'Upload'

New-AzDisk -ResourceGroupName 'myResourceGroup' -DiskName 'myDiskName' -Disk $diskconfig
```

Si desea cargar un disco SSD prémium o un disco SSD estándar, reemplace **Standard_LRS** por **Premium_LRS** o **StandardSSD_LRS**. Aún no se admite el almacenamiento en disco Ultra.

Ahora ha creado un disco administrado vacío que está configurado para el proceso de carga. Para cargar un disco duro virtual en ese disco, necesitará una SAS grabable, con el fin de que pueda hacer referencia a ella como el destino de la carga.

Para generar una SAS grabable de un disco administrado vacío, use el siguiente comando:

```powershell
$diskSas = Grant-AzDiskAccess -ResourceGroupName 'myResouceGroup' -DiskName 'myDiskName' -DurationInSecond 86400 -Access 'Write'

$disk = Get-AzDisk -ResourceGroupName 'myResourceGroup' -DiskName 'myDiskName'
```

## <a name="upload-vhd"></a>Carga de disco duro virtual

Ahora que tiene una SAS para el disco administrado vacío, puede usarla para establecer el disco administrado como destino del comando de carga.

Use AzCopy V10 para cargar el archivo VHD local en un disco administrado, para lo que se especifica el identificador URI de SAS que ha generado.

Esta carga tiene el mismo rendimiento que el [HDD estándar](disks-types.md#standard-hdd) equivalente. Por ejemplo, si tiene un tamaño que equivale a S4, tendrá un rendimiento de hasta 60 MiB/s. Pero si tiene un tamaño que equivale a S70, tendrá un rendimiento de hasta 500 MiB/s.

```
AzCopy.exe copy "c:\somewhere\mydisk.vhd" $diskSas.AccessSAS --blob-type PageBlob
```

Si la SAS expira durante la carga y aún no ha llamado a `revoke-access`, puede obtener una nueva SAS para continuar con la carga mediante `grant-access`.

Cuando finalice la carga y ya no necesite escribir más datos en el disco, revoque la SAS. Al revocar la SAS, cambiará el estado del disco administrado y podrá conectar el disco a una máquina virtual.

```powershell
Revoke-AzDiskAccess -ResourceGroupName 'myResourceGroup' -DiskName 'myDiskName'
```

## <a name="copy-a-managed-disk"></a>Copia de un disco administrado

La carga directa también simplifica el proceso de copia de un disco administrado. Puede copiar dentro de la misma región o entre regiones (a otra región).

El script siguiente lo hará automáticamente, el proceso es similar a los pasos descritos anteriormente, con algunas diferencias porque está trabajando con un disco existente.

> [!IMPORTANT]
> Debe agregar un desplazamiento de 512 cuando proporcione el tamaño de disco en bytes de un disco administrado de Azure. Esto se debe a que Azure omite el pie de página al devolver el tamaño del disco. Si no lo hace, se producirá un error en la copia. El siguiente script ya lo hace automáticamente.

Reemplace los `<sourceResourceGroupHere>`, `<sourceDiskNameHere>`, `<targetDiskNameHere>`, `<targetResourceGroupHere>`, `<yourOSTypeHere>` y `<yourTargetLocationHere>` (un ejemplo de un valor de ubicación sería uswest2) con sus valores y, a continuación, ejecute el siguiente script para copiar un disco administrado.

```powershell

$sourceRG = <sourceResourceGroupHere>
$sourceDiskName = <sourceDiskNameHere>
$targetDiskName = <targetDiskNameHere>
$targetRG = <targetResourceGroupHere>
$targetLocate = <yourTargetLocationHere>
#Expected value for OS is either "Windows" or "Linux"
$targetOS = <yourOSTypeHere>

$sourceDisk = Get-AzDisk -ResourceGroupName $sourceRG -DiskName $sourceDiskName

# Adding the sizeInBytes with the 512 offset, and the -Upload flag
$targetDiskconfig = New-AzDiskConfig -SkuName 'Standard_LRS' -osType $targetOS -UploadSizeInBytes $($sourceDisk.DiskSizeBytes+512) -Location $targetLocate -CreateOption 'Upload'

$targetDisk = New-AzDisk -ResourceGroupName $targetRG -DiskName $targetDiskName -Disk $targetDiskconfig

$sourceDiskSas = Grant-AzDiskAccess -ResourceGroupName $sourceRG -DiskName $sourceDiskName -DurationInSecond 86400 -Access 'Read'

$targetDiskSas = Grant-AzDiskAccess -ResourceGroupName $targetRG -DiskName $targetDiskName -DurationInSecond 86400 -Access 'Write'

azcopy copy $sourceDiskSas.AccessSAS $targetDiskSas.AccessSAS --blob-type PageBlob

Revoke-AzDiskAccess -ResourceGroupName $sourceRG -DiskName $sourceDiskName

Revoke-AzDiskAccess -ResourceGroupName $targetRG -DiskName $targetDiskName 
```

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha cargado correctamente un disco duro virtual en un disco administrado, puede conectar el disco a una máquina virtual y empezar a usarlo.

Para aprender a conectar un disco de datos a una máquina virtual, consulte nuestro artículo al respecto: [Conexión de un disco a una VM Windows con PowerShell](attach-disk-ps.md). Para usar el disco como disco del sistema operativo, consulte [Creación de una máquina virtual Windows desde un disco especializado](create-vm-specialized.md#create-the-new-vm).
