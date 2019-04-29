---
title: archivo de inclusión
description: archivo de inclusión
services: virtual-machines
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 09/20/2018
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: 7fd5b2051f81a5dc34270a608c1518e8a11678b5
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60542476"
---
## <a name="using-rbac-to-share-images"></a>Uso de RBAC para compartir imágenes

Puede compartir imágenes entre suscripciones mediante el Control de acceso basado en roles (RBAC). Cualquier usuario que tenga permisos de lectura para una versión de la imagen, incluso entre suscripciones, será capaz de implementar una máquina virtual con la versión de la imagen.

Para más información sobre cómo compartir recursos con RBAC, consulte [Administración del acceso mediante RBAC y la CLI de Azure](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-cli).


## <a name="list-information"></a>Visualización de la información

Para obtener la ubicación, el estado y demás información sobre las galerías de imágenes disponibles, use [az sig list](/cli/azure/sig#az-sig-list).

```azurecli-interactive 
az sig list -o table
```

Enumere las definiciones de las imágenes en una galería, incluida la información sobre el tipo de sistema operativo y el estado, con [az sig image-definition list](/cli/azure/sig/image-definition#az-sig-image-definition-list).

```azurecli-interactive 
az sig image-definition list -g myGalleryRG -r myGallery -o table
```

Enumere las versiones de imágenes compartidas en una galería, con [az sig image-version list](/cli/azure/sig/image-version#az-sig-image-version-list).

```azurecli-interactive
az sig image-version list -g myGalleryRG -r myGallery -i myGalleryImage -o table
```

Obtenga el identificador de una versión de imagen con [az sig image-version show](/cli/azure/sig/image-version#az-sig-image-version-show).

```
az sig image-version show \
-g myGalleryRG \     
-r myGallery \     
-i myGalleryImage \     
--gallery-image-version-name 1.0.0 \     
--query "id"
```