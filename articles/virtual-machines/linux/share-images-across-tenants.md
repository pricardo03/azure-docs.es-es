---
title: Compartir imágenes de la galería en inquilinos de Azure | Microsoft Docs
description: Obtenga información sobre cómo compartir imágenes de máquinas virtuales entre los inquilinos de Azure con galerías de imágenes compartidas.
services: virtual-machines-linux
author: cynthn
manager: jeconnoc
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.topic: article
ms.date: 04/05/2019
ms.author: cynthn
ms.openlocfilehash: 1578ba840c6dca93feb68754863439811d7ef099
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65158736"
---
# <a name="share-gallery-vm-images-across-azure-tenants"></a>Compartir imágenes de máquina virtual de la galería en inquilinos de Azure

[!INCLUDE [virtual-machines-share-images-across-tenants](../../../includes/virtual-machines-share-images-across-tenants.md)]


## <a name="create-a-vm-using-azure-cli"></a>Crear una máquina virtual mediante la CLI de Azure

Inicie sesión en la entidad de servicio del inquilino mediante el appID, la clave de aplicación y el identificador del inquilino 1 de 1. Puede usar `az account show --query "tenantId"` para obtener el inquilino de identificadores, si es necesario.

```azurecli-interactive
az account clear
az login --service-principal -u '<app ID>' -p '<Secret>' --tenant '<tenant 1 ID>'
az account get-access-token 
```
 
Inicie sesión en la entidad de servicio del inquilino mediante el appID, la clave de aplicación y el identificador del inquilino 2 de 2:

```azurecli-interactive
az login --service-principal -u '<app ID>' -p '<Secret>' --tenant '<tenant 2 ID>'
az account get-access-token
```

Cree la máquina virtual. Reemplace la información en el ejemplo por los suyos propios.

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