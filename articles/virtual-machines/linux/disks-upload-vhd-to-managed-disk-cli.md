---
title: Carga de un disco duro virtual mediante la CLI de Azure
description: Aprenda a cargar un disco duro virtual en un disco administrado de Azure mediante la CLI de Azure.
services: virtual-machines-linux,storage
author: roygara
ms.author: rogarana
ms.date: 09/20/2019
ms.topic: article
ms.service: virtual-machines-linux
ms.tgt_pltfrm: linux
ms.subservice: disks
ms.openlocfilehash: bd4d3b9b34f951896e838d5f6f50ca204d329568
ms.sourcegitcommit: 3f22ae300425fb30be47992c7e46f0abc2e68478
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/25/2019
ms.locfileid: "71266489"
---
# <a name="upload-a-vhd-to-azure-using-azure-cli"></a>Carga de un disco duro virtual mediante la CLI de Azure

En este artículo se explica cómo cargar un disco duro virtual de una máquina local en un disco administrado de Azure. Antes había que seguir un proceso más complicado, que incluía el almacenamiento provisional de los datos en una cuenta de almacenamiento y la administración de dicha cuenta. Ya no es preciso administrar una cuenta de almacenamiento ni almacenar en ella datos temporalmente para cargar un disco duro virtual. En su lugar, cree un disco administrado vacío y cargue un disco duro virtual directamente en él. Así se simplifica la carga de máquinas virtuales locales en Azure y permite cargar un disco duro virtual hasta 32 TiB directamente en un disco administrado de gran tamaño.

Si va a proporcionar una solución de copia de seguridad para las máquinas virtuales de IaaS en Azure, se recomienda usar la carga directa para restaurar las copias de seguridad de clientes en discos administrados. Si va a cargar un disco duro virtual desde una máquina externa a Azure, las velocidades dependerán del ancho de banda local. Si usa una máquina virtual de Azure, el ancho de banda será el mismo que el de los discos duros estándar.

Actualmente, la carga directa es compatible con los HDD estándar, la unidad de estado sólido estándar y los discos administrados SSD prémium. Aún no es compatible con el almacenamiento en discos Ultra.

## <a name="prerequisites"></a>Requisitos previos

- Descargue la [versión más reciente de AzCopy, v10](../../storage/common/storage-use-azcopy-v10.md#download-and-install-azcopy).
- [Instalación de la CLI de Azure](/cli/azure/install-azure-cli).
- Un archivo VHD almacenado localmente

## <a name="create-an-empty-managed-disk"></a>Creación de un disco administrado vacío

Para cargar un disco duro virtual en Azure, deberá crear un disco administrado vacío que esté configurado específicamente para este proceso de carga. Antes de crearlo, hay información adicional que debe saber acerca de estos discos.

Este tipo de disco administrado tiene dos estados únicos:

- ReadToUpload, que significa que el disco está listo para recibir una carga pero no se ha generado ninguna [firma de acceso seguro](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1).
- ActiveUpload, que significa que el disco está listo para recibir una carga y se ha generado la SAS.

Mientras esté en cualquiera de estos estados, el disco administrado se facturará al [precio de un HDD estándar](https://azure.microsoft.com/pricing/details/managed-disks/), independientemente del tipo real de disco. Por ejemplo, un P10 se facturará como un S10. Esto sucederá hasta que se llame a `revoke-access` en el disco administrado, que es necesario para conectar el disco a una máquina virtual.

Cree un HDD estándar vacío para la carga mediante la especificación tanto del parámetro **-–for-upload** como del parámetro **--upload-size-bytes** en un cmdlet [disk create](/cli/azure/disk#az-disk-create):

```azurecli-interactive
az disk create -n mydiskname -g resourcegroupname -l westus2 --for-upload --upload-size-bytes 34359738880 --sku standard_lrs
```

Si desea cargar un disco SSD prémium o un disco SSD estándar, reemplace **standard_lrs** por **premium_LRS** o **standardssd_lrs**. Aún no se admite el almacenamiento en disco Ultra.

Ahora ha creado un disco administrado vacío que está configurado para el proceso de carga. Para cargar un disco duro virtual en ese disco, necesitará una SAS grabable, con el fin de que pueda hacer referencia a ella como el destino de la carga.

Para generar una SAS grabable de un disco administrado vacío, use el siguiente comando:

```azurecli-interactive
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

```
AzCopy.exe copy "c:\somewhere\mydisk.vhd" "sas-URI" --blob-type PageBlob
```

Si la SAS expira durante la carga y aún no ha llamado a `revoke-access`, puede obtener una nueva SAS para continuar con la carga mediante `grant-access`.

Cuando finalice la carga y ya no necesite escribir más datos en el disco, revoque la SAS. Al revocar la SAS, cambiará el estado del disco administrado y podrá conectar el disco a una máquina virtual.

```azurecli-interactive
az disk revoke-access -n mydiskname -g resourcegroupname
```

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha cargado correctamente un disco duro virtual en un disco administrado, puede conectar el disco a una máquina virtual y empezar a usarlo.

Para aprender a conectar un disco a una máquina virtual, consulte nuestro artículo al respecto: [Adición de un disco a una máquina virtual Linux](add-disk.md).
