---
title: Directiva para conservar los manifiestos sin etiqueta en Azure Container Registry
description: Aprenda a habilitar una política de retención en Azure Container Registry para la eliminación automática de manifiestos sin etiqueta después de un período definido.
services: container-registry
author: dlepow
manager: gwallace
ms.service: container-registry
ms.topic: article
ms.date: 09/25/2019
ms.author: danlep
ms.openlocfilehash: 36d27bc6089bbe3f4ada6862a9c1be1fa0bdbae7
ms.sourcegitcommit: 29880cf2e4ba9e441f7334c67c7e6a994df21cfe
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/26/2019
ms.locfileid: "71305923"
---
# <a name="set-a-retention-policy-for-untagged-manifests"></a>Establecimiento de una directiva de retención para manifiestos sin etiqueta

Azure Container Registry ofrece la opción de establecer una *directiva de retención* para los manifiestos de imagen almacenados que no tienen etiquetas asociadas (*manifiestos sin etiqueta*). Cuando se habilita una directiva de retención, los manifiestos sin etiqueta del registro se eliminan automáticamente después de un número de días establecido. Esta característica evita que el registro se llene con artefactos que no son necesarios y ayuda a ahorrar en los costos de almacenamiento. Si el atributo `delete-enabled` de un manifiesto sin etiqueta se establece en `false`, no se puede eliminar el manifiesto y no se aplica la directiva de retención.

Puede usar Azure Cloud Shell o una instalación local de la CLI de Azure para ejecutar los ejemplos de comando de este artículo. Si quiere usarla de forma local, necesitará la versión 2.0.74 u otra posterior. Ejecute `az --version` para encontrar la versión. Si necesita instalarla o actualizarla, vea [Instalación de la CLI de Azure][azure-cli].

> [!IMPORTANT]
> Esta funcionalidad actualmente está en su versión preliminar y se [aplican algunas limitaciones](#preview-limitations). Las versiones preliminares están a su disposición con la condición de que acepte los [términos de uso adicionales][terms-of-use]. Es posible que algunos de los aspectos de esta característica cambien antes de ofrecer disponibilidad general.

> [!WARNING]
> Establezca una directiva de retención con cuidado: los datos de imagen eliminados son IRRECUPERABLES. Si tiene sistemas que extraen imágenes por hash de manifiesto (en lugar de por nombre de la imagen), no debe establecer una política de retención para los manifiestos sin etiqueta. La eliminación de imágenes sin etiqueta impedirá que esos sistemas extraigan las imágenes del registro. En lugar de extraer por manifiesto, considere la posibilidad de adoptar un esquema de *etiquetado único*, un [procedimiento recomendado](container-registry-image-tag-version.md).

Si quiere eliminar los manifiestos o las etiquetas de una sola imagen mediante comandos de la CLI de Azure, consulte [Eliminación de imágenes de contenedor en Azure Container Registry](container-registry-delete.md).

## <a name="preview-limitations"></a>Limitaciones de vista previa

* Solo los registros de contenedor **Premium** pueden configurarse con una directiva de retención. Para obtener información acerca de los niveles de servicio de registro, consulte [SKU de Azure Container Registry](container-registry-skus.md).
* Solo puede establecer una directiva de retención para manifiestos sin etiqueta.

## <a name="set-a-retention-policy---cli"></a>Establecimiento de una directiva de retención (CLI)

En el ejemplo siguiente se muestra cómo usar la CLI de Azure para establecer una directiva de retención para manifiestos sin etiqueta de un registro.

### <a name="enable-a-retention-policy"></a>Habilitación de una directiva de retención

De forma predeterminada, no se establece ninguna directiva de retención en un registro de contenedor. Para establecer o actualizar una directiva de retención, ejecute el comando [az acr config retention update][az-acr-config-retention-update] en la CLI de Azure. Puede especificar un número de días entre 0 y 365 para conservar los manifiestos sin etiqueta. Si no especifica un número de días, el comando establece un valor predeterminado de 7 días. Después del período de retención, se eliminarán automáticamente todos los manifiestos sin etiqueta del registro.

En el ejemplo siguiente se establece una directiva de retención de 30 días para los manifiestos sin etiqueta del registro *myregistry*:

```azurecli
az acr config retention update --name myregistry --status enabled --days 30 --type UntaggedManifests
```

En el ejemplo siguiente se establece una directiva para eliminar cualquier manifiesto del registro en cuanto no esté etiquetado. Para crear esta directiva, establezca un período de retención de 0 días:

```azurecli
az acr config retention update --name myregistry --status enabled --days 0 --type UntaggedManifests
```

### <a name="disable-a-retention-policy"></a>Deshabilitación de una directiva de retención

Para ver la directiva de retención establecida en un registro, ejecute el comando [az acr config retention show][az-acr-config-retention-show]:

```azurecli
az acr config retention show --name myregistry
```

Para deshabilitar una directiva de retención de un registro, ejecute el comando [az acr config retention update][az-acr-config-retention-update] y establezca `--status disabled`:

```azurecli
az acr config retention update --name myregistry --status disabled
```

## <a name="set-a-retention-policy---portal"></a>Establecimiento de una directiva de retención (portal)

También puede establecer la directiva de retención de un registro en [Azure Portal](https://portal.azure.com). En el ejemplo siguiente se muestra cómo usar el portal para establecer una directiva de retención para manifiestos sin etiqueta de un registro.

### <a name="enable-a-retention-policy"></a>Habilitación de una directiva de retención

1. Navegue a la instancia de Azure Container Registry. En **Directivas**, seleccione **Retención** (versión preliminar).
1. En **Estado**, seleccione **Habilitado**.
1. Seleccione un número de días entre 0 y 365 para conservar los manifiestos sin etiqueta. Seleccione **Guardar**.

![Habilitación de una directiva de retención en Azure Portal](media/container-registry-retention-policy/container-registry-retention-policy01.png)

### <a name="disable-a-retention-policy"></a>Deshabilitación de una directiva de retención

1. Navegue a la instancia de Azure Container Registry. En **Directivas**, seleccione **Retención** (versión preliminar).
1. En **Estado**, seleccione **Deshabilitado**. Seleccione **Guardar**.

## <a name="next-steps"></a>Pasos siguientes

* Más información sobre opciones de [eliminación de imágenes y repositorios](container-registry-delete.md) en Azure Container Registry

* Más información sobre cómo [purgar automáticamente ](container-registry-auto-purge.md) imágenes y manifiestos seleccionados de un registro

* Más información sobre opciones de [bloqueo de imágenes y manifiestos](container-registry-image-lock.md) en un registro

<!-- LINKS - external -->
[terms-of-use]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/


<!-- LINKS - internal -->
[azure-cli]: /cli/azure/install-azure-cli
[az-acr-config-retention-update]: /cli/azure/acr/config/retention#az-acr-config-retention-update
[az-acr-config-retention-show]: /cli/azure/acr/config/retention#az-acr-config-retention-show
