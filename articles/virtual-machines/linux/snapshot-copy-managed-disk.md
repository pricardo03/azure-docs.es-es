---
title: Creación de una instantánea de un disco duro virtual en Azure | Microsoft Docs
description: Aprenda a crear una copia de un disco duro virtual en Azure como copia de seguridad o para solucionar problemas.
documentationcenter: ''
author: roygara
manager: twooley
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 07/11/2018
ms.author: rogarana
ms.subservice: disks
ms.openlocfilehash: 9f2f3ac3668f0e48716fc30fb69cd1782dbd4e56
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2019
ms.locfileid: "64706980"
---
# <a name="create-a-snapshot"></a>Crear una instantánea 

Tome una instantánea de un disco de datos o del sistema operativo para realizar una copia de seguridad o para solucionar problemas de la máquina virtual. Una instantánea es una copia completa de solo lectura de un disco duro virtual. 

## <a name="use-azure-cli"></a>Uso de CLI de Azure 

En el ejemplo siguiente, se requiere el uso de [Cloud Shell](https://shell.azure.com/bash) o tener instalada la CLI de Azure.

Los pasos siguientes muestran cómo realizar una instantánea mediante el comando **az snapshot create** con el parámetro **--source-disk**. En el siguiente ejemplo se supone que hay una máquina virtual denominada *myVM* en el grupo de recursos *myResourceGroup*.

Obtenga el Id. de disco mediante [az vm show](/cli/azure/vm#az-vm-show).

```azurecli-interactive
osDiskId=$(az vm show \
   -g myResourceGroup \
   -n myVM \
   --query "storageProfile.osDisk.managedDisk.id" \
   -o tsv)
```

Realice una instantánea denominada *osDisk-backup* mediante [az snapshot create](/cli/azure/snapshot#az-snapshot-create).

```azurecli-interactive
az snapshot create \
    -g myResourceGroup \
    --source "$osDiskId" \
    --name osDisk-backup
```

> [!NOTE]
> Si desea almacenar la instantánea en un almacenamiento resistente a zonas, debe crearla en una región que admita [zonas de disponibilidad ](../../availability-zones/az-overview.md) e incluir el parámetro **--sku Standard_ZRS**.

Puede ver una lista de las instantáneas con [az snapshot list](/cli/azure/snapshot#az-snapshot-list).

```azurecli-interactive
az snapshot list \
   -g myResourceGroup \
   - table
```

## <a name="use-azure-portal"></a>Usar Azure Portal 

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
2. En la parte superior izquierda, haga clic en **Crear un recurso** y busque **instantánea**. Seleccione **Instantánea** desde los resultados de búsqueda.
3. En la hoja **Instantánea**, haga clic en **Crear**.
4. Escriba un **nombre** para la instantánea.
5. Seleccione un valor de grupo de recursos existente o escriba el nombre para crear uno. 
7. Como **disco de origen**, seleccione el disco administrado para la instantánea.
8. Seleccione el **tipo de cuenta** que se usará para almacenar la instantánea. Use **HDD estándar**, a menos que necesite almacenarla en un disco SSD de alto rendimiento.
9. Haga clic en **Create**(Crear).


## <a name="next-steps"></a>Pasos siguientes

 Cree una máquina virtual a partir de una instantánea; para ello, cree primero un disco administrado con la instantánea y asocie el nuevo disco administrado como disco del sistema operativo. Para más información, consulte el script que se incluye en [Creación de una máquina virtual a partir de una instantánea](./../scripts/virtual-machines-linux-cli-sample-create-vm-from-snapshot.md?toc=%2fcli%2fmodule%2ftoc.json).

