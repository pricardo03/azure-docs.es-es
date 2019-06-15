---
title: Actualización de contenedores en Azure Container Instances
description: Aprenda a actualizar contenedores en ejecución en los grupos de contenedores de Azure Container Instances.
services: container-instances
author: dlepow
ms.service: container-instances
ms.topic: article
ms.date: 08/01/2018
ms.author: danlep
ms.openlocfilehash: 2df6a2724cbdcd6bbb6c6ca6636256b7e399da8e
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "60686898"
---
# <a name="update-containers-in-azure-container-instances"></a>Actualización de contenedores en Azure Container Instances

Durante el funcionamiento normal de las instancias de contenedor, puede que sea necesario actualizar los contenedores de un grupo de contenedores. Por ejemplo, podría querer actualizar la versión de la imagen, cambiar un nombre DNS, actualizar las variables de entorno o el estado de un contenedor cuya aplicación se ha bloqueado.

## <a name="update-a-container-group"></a>Actualización de un grupo de contenedores

Para actualizar los contenedores de un grupo de contenedores, vuelva a implementar un grupo existente con al menos una propiedad modificada. Cuando se actualiza un grupo de contenedores, todos los contenedores en ejecución se reinician localmente.

Para reimplementar un grupo de contenedores existente, emita el comando create (o use Azure Portal) y especifique el nombre de un grupo existente. Modifique al menos una propiedad válida del grupo al emitir el comando create para desencadenar la reimplementación. No todas las propiedades del grupo de contenedores son válidas para una reimplementación. Para ver una lista de propiedades no admitidas, consulte [Propiedades que requieren la eliminación del contenedor](#properties-that-require-container-delete).

En el siguiente ejemplo de la CLI de Azure se actualiza un grupo de contenedores con una nueva etiqueta de nombre DNS. Dado que se modifica la propiedad de etiqueta del nombre DNS del grupo, se vuelve a implementar el grupo de contenedores y se reinician sus contenedores.

Implementación inicial con la etiqueta de nombre DNS *myapplication-staging*:

```azurecli-interactive
# Create container group
az container create --resource-group myResourceGroup --name mycontainer \
    --image nginx:alpine --dns-name-label myapplication-staging
```

Actualice el grupo de contenedores con una nueva etiqueta de nombre DNS, *myapplication*:

```azurecli-interactive
# Update container group (restarts container)
az container create --resource-group myResourceGroup --name mycontainer \
    --image nginx:alpine --dns-name-label myapplication
```

## <a name="update-benefits"></a>Ventajas de la actualización

La principal ventaja de actualizar un grupo de contenedores existente es una implementación más rápida. Al volver a implementar un grupo de contenedores existente, sus capas de imagen de contenedor se extraen de las almacenados en caché por la implementación anterior. En lugar de extraer todas las capas de imagen actualizadas del Registro como se hace con las nuevas implementaciones, solo se extraen las capas modificadas (si existen).

Las aplicaciones basadas en imágenes de contenedor más grandes, como Windows Server Core, pueden experimentar una considerable mejora en la velocidad de implementación al actualizarlas, en lugar de eliminarlas y volver a implementarlas.

## <a name="limitations"></a>Limitaciones

No todas las propiedades de un grupo de contenedores admiten actualizaciones. Para cambiar algunas propiedades de un grupo de contenedores, primero debe eliminar y volver a implementar el grupo. Para más información, consulte [Propiedades que requieren la eliminación del contenedor](#properties-that-require-container-delete).

Todos los contenedores de un grupo de contenedores se reinician al actualizar el grupo de contenedores. No se puede realizar una actualización o reinicio local de un contenedor específico en un grupo de varios contenedores.

La dirección IP de un contenedor no cambia normalmente entre actualizaciones, pero no se garantiza que permanezca igual. Mientras el grupo de contenedores esté implementado en el mismo host subyacente, conservará su dirección IP. Aunque es poco habitual, y Azure Container Instances hace todo lo posible para volver a realizar la implementación en el mismo host, algunos eventos internos de Azure pueden provocar la reimplementación en otro host. Para mitigar este problema, use siempre una etiqueta de nombre DNS para las instancias de contenedor.

No se pueden actualizar los grupos de contenedores finalizados o eliminados. Una vez que se ha detenido un grupo de contenedores (se encuentra en estado *Terminado* estado) o se ha eliminado, el grupo se implementa como nuevo.

## <a name="properties-that-require-container-delete"></a>Propiedades que requieren la eliminación del contenedor

Como se mencionó anteriormente, no todas las propiedades del grupo de contenedores se pueden actualizar. Por ejemplo, para cambiar los puertos o reiniciar la directiva de un contenedor, primero debe eliminar el grupo de contenedores y luego volver a crearlo.

Estas propiedades requieren la eliminación del grupo de contenedores antes de volver a implementarlo:

* Tipo de SO
* CPU
* Memoria
* Directiva de reinicio
* Puertos

Al eliminar un grupo de contenedores y volver a crearlo, no se reimplementa sino que se crea uno nuevo. Todas las capas de imagen se extraen actualizadas del Registro, y no de las almacenadas en caché por una implementación anterior. La dirección IP del contenedor también podría cambiar debido a que se implementa en un host subyacente diferente.

## <a name="next-steps"></a>Pasos siguientes

Se ha mencionado varias veces en este artículo el **grupo de contenedores**. Cada contenedor de Azure Container Instances se implementa en un grupo de contenedores y los grupos de contenedores pueden contener más de un contenedor.

[Grupos de contenedores en Azure Container Instances](container-instances-container-groups.md)

[Implementación de grupos de varios contenedores](container-instances-multi-container-group.md)

<!-- LINKS - External -->

<!-- LINKS - Internal -->
[az-container-create]: /cli/azure/container?view=azure-cli-latest#az-container-create
[az-container-logs]: /cli/azure/container?view=azure-cli-latest#az-container-logs
[az-container-show]: /cli/azure/container?view=azure-cli-latest#az-container-show
[azure-cli-install]: /cli/azure/install-azure-cli
