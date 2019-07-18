---
title: Uso compartido de las imágenes de la galería entre inquilinos de Azure | Microsoft Docs
description: Obtenga información sobre cómo compartir imágenes de máquina virtual entre inquilinos de Azure mediante Galerías de imágenes compartidas.
services: virtual-machines-linux
author: cynthn
manager: jeconnoc
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.topic: article
ms.date: 04/05/2019
ms.author: cynthn
ms.openlocfilehash: 88596a4bda3c9ebae99403e95b3539f068cad665
ms.sourcegitcommit: c63e5031aed4992d5adf45639addcef07c166224
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/28/2019
ms.locfileid: "67465469"
---
# <a name="share-gallery-vm-images-across-azure-tenants"></a>Uso compartido de imágenes de la máquina virtual de la galería entre inquilinos de Azure

[!INCLUDE [virtual-machines-share-images-across-tenants](../../../includes/virtual-machines-share-images-across-tenants.md)]

> [!IMPORTANT]
> No se puede usar el portal para implementar una VM desde una imagen en otro inquilino de Azure. Para crear una VM desde una imagen que se comparte entre los inquilinos, debe usar la CLI de Azure o [Powershell](../windows/share-images-across-tenants.md).

## <a name="create-a-vm-using-azure-cli"></a>Creación de una máquina virtual mediante la CLI de Azure

Inicie sesión en la entidad de servicio del inquilino 1 mediante el id. de la aplicación, la clave de aplicación y el identificador del inquilino 1. Puede usar `az account show --query "tenantId"` para obtener el identificador del inquilino, si es necesario.

```azurecli-interactive
az account clear
az login --service-principal -u '<app ID>' -p '<Secret>' --tenant '<tenant 1 ID>'
az account get-access-token 
```
 
Inicie sesión en la entidad de servicio del inquilino 2 mediante el id. de la aplicación, la clave de aplicación y el identificador del inquilino 2:

```azurecli-interactive
az login --service-principal -u '<app ID>' -p '<Secret>' --tenant '<tenant 2 ID>'
az account get-access-token
```

Cree la máquina virtual. Reemplace la información del ejemplo por la suya propia.

```azurecli-interactive
az vm create \
  --resource-group myResourceGroup \
  --name myVM \
  --image "/subscriptions/<Tenant 1 subscription>/resourceGroups/<Resource group>/providers/Microsoft.Compute/galleries/<Gallery>/images/<Image definition>/versions/<version>" \
  --admin-username azureuser \
  --generate-ssh-keys
```

## <a name="next-steps"></a>Pasos siguientes

Si experimenta algún problema, puede [solucionar problemas de galerías de imágenes compartidas](troubleshooting-shared-images.md).