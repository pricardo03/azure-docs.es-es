---
title: Bloquear una imagen en Azure Container Registry
description: Establecer atributos de una imagen de contenedor o el repositorio por lo que no pueden eliminarse ni sobrescribirse en Azure container registry.
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: article
ms.date: 02/19/2019
ms.author: danlep
ms.openlocfilehash: ebbfaba158e7ddb669111f097eb1adde2373aa6c
ms.sourcegitcommit: 223604d8b6ef20a8c115ff877981ce22ada6155a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/22/2019
ms.locfileid: "58361292"
---
# <a name="lock-a-container-image-in-an-azure-container-registry"></a>Bloquear una imagen de contenedor en Azure container registry

En Azure container registry, puede bloquear una versión de la imagen o un repositorio para que no puede eliminarse o actualizarse. Para bloquear una imagen o un repositorio, actualizar sus atributos mediante el comando de CLI de Azure [actualización del repositorio de az acr][az-acr-repository-update]. 

Este artículo es preciso que ejecute la CLI de Azure en Azure Cloud Shell o localmente (versión 2.0.55 o se recomiendan más adelante). Ejecute `az --version` para encontrar la versión. Si necesita instalarla o actualizarla, consulte [Instalación de la CLI de Azure][azure-cli].

## <a name="scenarios"></a>Escenarios

De forma predeterminada, es una imagen etiquetada en Azure Container Registry *mutable*, por lo que con los permisos adecuados repetidamente puede actualizar e insertar una imagen con la misma etiqueta en un registro. Imágenes de contenedor también pueden ser [eliminado](container-registry-delete.md) según sea necesario. Este comportamiento es útil al desarrollar las imágenes y es necesario mantener un tamaño para el registro.

Sin embargo, cuando implemente una imagen de contenedor en producción, puede que necesite un *inmutable* imagen de contenedor. Una imagen inmutable es aquel que no puede eliminar o sobrescribir accidentalmente. Use la [actualización del repositorio de az acr] [ az-acr-repository-update] comando para establecer los atributos del repositorio para que pueda:

* Bloquear una versión de la imagen o la totalidad del repositorio

* Proteger un repositorio o la versión de la imagen de la eliminación, pero permitir actualizaciones

* Evitar operaciones de lectura (extracción) en una versión de la imagen o la totalidad del repositorio

Consulte las secciones siguientes para obtener ejemplos.

## <a name="lock-an-image-or-repository"></a>Bloquear una imagen o un repositorio 

### <a name="show-the-current-repository-attributes"></a>Mostrar los atributos del almacén actual
Para ver los atributos de un repositorio actuales, ejecute el siguiente [show de az acr repositorio] [ az-acr-repository-show] comando:

```azurecli
az acr repository show \
    --name myregistry --repository myrepo
    --output jsonc
```

### <a name="show-the-current-image-attributes"></a>Mostrar los atributos de imagen actual
Para ver los atributos de una etiqueta actuales, ejecute el siguiente [show de az acr repositorio] [ az-acr-repository-show] comando:

```azurecli
az acr repository show \
    --name myregistry --image image:tag \
    --output jsonc
```

### <a name="lock-an-image-by-tag"></a>Bloquear una imagen por etiqueta

Para bloquear la *myrepo / myimage:tag* en la imagen *myregistry*, ejecute el siguiente [actualización del repositorio de az acr] [ az-acr-repository-update] comando:

```azurecli
az acr repository update \
    --name myregistry --image myrepo/myimage:tag \
    --write-enabled false
```

### <a name="lock-an-image-by-manifest-digest"></a>Bloquear una imagen mediante la síntesis del manifiesto

Para bloquear un *myrepo/myimage* imagen identificada por la síntesis del manifiesto (hash de SHA-256, representado como `sha256:...`), ejecute el siguiente comando. (Para obtener el resumen de manifiesto asociado con una o varias etiquetas de imagen, ejecute el [az acr repositorio show-manifiestos] [ az-acr-repository-show-manifests] comandos.)

```azurecli
az acr repository update \
    --name myregistry --image myrepo/myimage@sha256:123456abcdefg \
    --write-enabled false
```

### <a name="lock-a-repository"></a>Bloquear un repositorio

Para bloquear la *myrepo/myimage* repositorio y todas las imágenes en ella, ejecute el siguiente comando:

```azurecli
az acr repository update \
    --name myregistry --repository myrepo/myimage \
    --write-enabled false
```

## <a name="protect-an-image-or-repository-from-deletion"></a>Proteger una imagen o un repositorio de la eliminación

### <a name="protect-an-image-from-deletion"></a>Proteger una imagen de la eliminación

Para permitir la *myrepo / myimage:tag* imagen se actualiza pero no se elimina, ejecute el siguiente comando:

```azurecli
az acr repository update \
    --name myregistry --image myrepo/myimage:tag \
    --delete-enabled false --write-enabled true
```

### <a name="protect-a-repository-from-deletion"></a>Proteger un repositorio de la eliminación

El comando siguiente establece la *myrepo/myimage* repositorio, por lo que no se puede eliminar. Todavía se pueden actualizar o eliminar imágenes individuales.

```azurecli
az acr repository update \
    --name myregistry --repository myrepo/myimage \
    --delete-enabled false --write-enabled true
```

## <a name="prevent-read-operations-on-an-image-or-repository"></a>Evitar operaciones de lectura en una imagen o un repositorio

Para evitar que las operaciones de lectura (extracción) en el *myrepo / myimage:tag* imagen, ejecute el siguiente comando:

```azurecli
az acr repository update \
    --name myregistry --image myrepo/myimage:tag \
    --read-enabled false
```

Para evitar que las operaciones de lectura en todas las imágenes en el *myrepo/myimage* repositorio, ejecute el siguiente comando:

```azurecli
az acr repository update \
    --name myregistry --repository myrepo/myimage \
    --read-enabled false
```

## <a name="unlock-an-image-or-repository"></a>Desbloquear una imagen o un repositorio

Para restaurar el comportamiento predeterminado de la *myrepo / myimage:tag* la imagen para que pueda ser eliminado y actualizado, ejecute el siguiente comando:

```azurecli
az acr repository update \
    --name myregistry --image myrepo/myimage:tag \
    --delete-enabled true --write-enabled true
```

Para restaurar el comportamiento predeterminado de la *myrepo/myimage* repositorio y todas las imágenes para que se pueden eliminar y actualizados, ejecutan el siguiente comando:

```azurecli
az acr repository update \
    --name myregistry --repository myrepo/myimage \
    --delete-enabled true --write-enabled true
```

## <a name="next-steps"></a>Pasos siguientes

En este artículo, ha aprendido acerca de cómo utilizar el [actualización del repositorio de az acr] [ az-acr-repository-update] comandos para evitar la eliminación o actualización de versiones de imágenes en un repositorio. Para establecer atributos adicionales, consulte el [actualización del repositorio de az acr] [ az-acr-repository-update] referencia del comando.

Para ver los atributos establecido para un repositorio o la versión de la imagen, use el [show de az acr repositorio] [ az-acr-repository-show] comando.

Para obtener más información acerca de las operaciones de eliminación, consulte [eliminar imágenes de contenedor en Azure Container Registry][container-registry-delete].

<!-- LINKS - Internal -->
[az-acr-repository-update]: /cli/azure/acr/repository#az-acr-repository-update
[az-acr-repository-show]: /cli/azure/acr/repository#az-acr-repository-show
[az-acr-repository-show-manifests]: /cli/azure/acr/repository#az-acr-repository-show-manifests
[azure-cli]: /cli/azure/install-azure-cli
[container-registry-delete]: container-registry-delete.md

