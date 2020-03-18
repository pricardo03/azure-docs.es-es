---
title: Carga de un disco duro virtual mediante la CLI de Azure
description: Obtenga información sobre cómo cargar un disco virtual en un disco administrado de Azure y copiar un disco administrado en todas las regiones, mediante la CLI de Azure, a través de una carga directa.
services: virtual-machines,storage
author: roygara
ms.author: rogarana
ms.date: 09/20/2019
ms.topic: article
ms.service: virtual-machines
ms.subservice: disks
ms.openlocfilehash: 2a5bfec08546d6cf00b1e04017b3879db8f016ee
ms.sourcegitcommit: 5f39f60c4ae33b20156529a765b8f8c04f181143
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/10/2020
ms.locfileid: "78970335"
---
# <a name="upload-a-vhd-to-azure-using-azure-cli"></a>Carga de un disco duro virtual mediante la CLI de Azure

En este artículo se explica cómo cargar un disco duro virtual de una máquina local en un disco administrado de Azure. Antes había que seguir un proceso más complicado, que incluía el almacenamiento provisional de los datos en una cuenta de almacenamiento y la administración de dicha cuenta. Ya no es preciso administrar una cuenta de almacenamiento ni almacenar en ella datos temporalmente para cargar un disco duro virtual. En su lugar, cree un disco administrado vacío y cargue un disco duro virtual directamente en él. Así se simplifica la carga de máquinas virtuales locales en Azure y permite cargar un disco duro virtual hasta 32 TiB directamente en un disco administrado de gran tamaño.

Si va a proporcionar una solución de copia de seguridad para las máquinas virtuales de IaaS en Azure, se recomienda usar la carga directa para restaurar las copias de seguridad de clientes en discos administrados. Si va a cargar un disco duro virtual desde una máquina externa en Azure, las velocidades dependerán del ancho de banda local. Si usa una máquina virtual de Azure, el ancho de banda será el mismo que el de los discos duros estándar.

Actualmente, la carga directa es compatible con los HDD estándar, la unidad de estado sólido estándar y los discos administrados SSD prémium. Aún no es compatible con el almacenamiento en discos Ultra.

## <a name="prerequisites"></a>Prerrequisitos

- Descargue la [versión más reciente de AzCopy, v10](../../storage/common/storage-use-azcopy-v10.md#download-and-install-azcopy).
- [Instalación de la CLI de Azure](/cli/azure/install-azure-cli).
- Un archivo VHD almacenado localmente
- Si tiene previsto cargar un disco duro virtual desde el entorno local: Un disco duro virtual que [se ha preparado para Azure](../windows/prepare-for-upload-vhd-image.md), almacenado localmente.
- O bien, un disco administrado en Azure, si desea realizar una acción de copia.

## <a name="create-an-empty-managed-disk"></a>Creación de un disco administrado vacío

Para cargar un disco duro virtual en Azure, deberá crear un disco administrado vacío que esté configurado para este proceso de carga. Antes de crearlo, hay información adicional que debe saber acerca de estos discos.

Este tipo de disco administrado tiene dos estados únicos:

- ReadToUpload, que significa que el disco está listo para recibir una carga pero no se ha generado ninguna [firma de acceso seguro](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1).
- ActiveUpload, que significa que el disco está listo para recibir una carga y se ha generado la SAS.

Mientras esté en cualquiera de estos estados, el disco administrado se facturará al [precio de un HDD estándar](https://azure.microsoft.com/pricing/details/managed-disks/), independientemente del tipo real de disco. Por ejemplo, un P10 se facturará como un S10. Esto sucederá hasta que se llame a `revoke-access` en el disco administrado, que es necesario para conectar el disco a una máquina virtual.

Antes de que pueda crear un HDD estándar vacío para cargar, necesitará tener el tamaño del archivo del disco duro virtual que desea cargar, en bytes. Para ello, puede usar `wc -c <yourFileName>.vhd` o `ls -al <yourFileName>.vhd`. Este valor se usa al especificar el parámetro **- upload-size-bytes**.

Cree un HDD estándar vacío para la carga mediante la especificación tanto del parámetro **-–for-upload** como del parámetro **--upload-size-bytes** en un cmdlet [disk create](/cli/azure/disk#az-disk-create):

```bash
az disk create -n mydiskname -g resourcegroupname -l westus2 --for-upload --upload-size-bytes 34359738880 --sku standard_lrs
```

Si desea cargar un disco SSD prémium o un disco SSD estándar, reemplace **standard_lrs** por **premium_LRS** o **standardssd_lrs**. Aún no se admite el almacenamiento en disco Ultra.

Ahora ha creado un disco administrado vacío que está configurado para el proceso de carga. Para cargar un disco duro virtual en ese disco, necesitará una SAS grabable, con el fin de que pueda hacer referencia a ella como el destino de la carga.

Para generar una SAS grabable de un disco administrado vacío, use el siguiente comando:

```bash
az disk grant-access -n mydiskname -g resourcegroupname --access-level Write --duration-in-seconds 86400
```

Valor devuelto de ejemplo:

```
{
  "accessSas": "https://md-impexp-t0rdsfgsdfg4.blob.core.windows.net/w2c3mj0ksfgl/abcd?sv=2017-04-17&sr=b&si=600a9281-d39e-4cc3-91d2-923c4a696537&sig=xXaT6mFgf139ycT87CADyFxb%2BnPXBElYirYRlbnJZbs%3D"
}
```

## <a name="upload-vhd"></a>Carga de disco duro virtual

Ahora que tiene una SAS para el disco administrado vacío, puede usarla para establecer el disco administrado como destino del comando de carga.

Use AzCopy V10 para cargar el archivo VHD local en un disco administrado, para lo que se especifica el identificador URI de SAS que ha generado.

Esta carga tiene el mismo rendimiento que el [HDD estándar](disks-types.md#standard-hdd) equivalente. Por ejemplo, si tiene un tamaño que equivale a S4, tendrá un rendimiento de hasta 60 MiB/s. Pero si tiene un tamaño que equivale a S70, tendrá un rendimiento de hasta 500 MiB/s.

```bash
AzCopy.exe copy "c:\somewhere\mydisk.vhd" "sas-URI" --blob-type PageBlob
```

Si la SAS expira durante la carga y aún no ha llamado a `revoke-access`, puede obtener una nueva SAS para continuar con la carga mediante `grant-access`.

Cuando finalice la carga y ya no necesite escribir más datos en el disco, revoque la SAS. Al revocar la SAS, cambiará el estado del disco administrado y podrá conectar el disco a una máquina virtual.

```bash
az disk revoke-access -n mydiskname -g resourcegroupname
```

## <a name="copy-a-managed-disk"></a>Copia de un disco administrado

La carga directa también simplifica el proceso de copia de un disco administrado. Puede copiar dentro de la misma región o entre regiones (a otra región).

El script siguiente lo hará automáticamente, el proceso es similar a los pasos descritos anteriormente, con algunas diferencias porque está trabajando con un disco existente.

> [!IMPORTANT]
> Debe agregar un desplazamiento de 512 cuando proporcione el tamaño de disco en bytes de un disco administrado de Azure. Esto se debe a que Azure omite el pie de página al devolver el tamaño del disco. Si no lo hace, se producirá un error en la copia. El siguiente script ya lo hace automáticamente.

Reemplace los `<sourceResourceGroupHere>`, `<sourceDiskNameHere>`, `<targetDiskNameHere>`, `<targetResourceGroupHere>` y `<yourTargetLocationHere>` (un ejemplo de un valor de ubicación sería uswest2) con sus valores y, a continuación, ejecute el siguiente script para copiar un disco administrado.

```bash
sourceDiskName = <sourceDiskNameHere>
sourceRG = <sourceResourceGroupHere>
targetDiskName = <targetDiskNameHere>
targetRG = <targetResourceGroupHere>
targetLocale = <yourTargetLocationHere>

sourceDiskSizeBytes= $(az disk show -g $sourceRG -n $sourceDiskName --query '[uniqueId]' -o tsv)

az disk create -g $targetRG -n $targetDiskName -l $targetLocale --for-upload --upload-size-bytes $(($sourceDiskSizeBytes+512)) --sku standard_lrs

targetSASURI = $(az disk grant-access -n $targetDiskName -g $targetRG  --access-level Write --duration-in-seconds 86400 -o tsv)

sourceSASURI=$(az disk grant-access -n $sourceDiskName -g $sourceRG --duration-in-seconds 86400 --query [accessSas] -o tsv)

.\azcopy copy $sourceSASURI $targetSASURI --blob-type PageBlob

az disk revoke-access -n $sourceDiskName -g $sourceRG

az disk revoke-access -n $targetDiskName -g $targetRG
```

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha cargado correctamente un disco duro virtual en un disco administrado, puede conectar el disco como [disco de datos a una máquina virtual ya existente](add-disk.md) o [conectarlo como disco del sistema operativo](upload-vhd.md#create-the-vm), para crear una nueva máquina virtual. 

