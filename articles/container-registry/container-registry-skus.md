---
title: SKU de Azure Container Registry
description: Comparación de los diferentes niveles de servicio disponibles en Azure Container Registry.
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: article
ms.date: 08/30/2018
ms.author: danlep
ms.openlocfilehash: 8fc364eac619c2f23ad2db1051a9b3ffa1bd38d6
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/18/2019
ms.locfileid: "58138089"
---
# <a name="azure-container-registry-skus"></a>SKU de Azure Container Registry

Azure Container Registry (ACR) está disponible en varios niveles de servicio, conocidos como SKU. Estas SKU proporcionan precios predecibles y varias opciones para alinearse con la capacidad y patrones de uso de su registro de Docker privado en Azure.

| SKU | Administrado | DESCRIPCIÓN |
| --- | :-------: | ----------- |
| **Básico** | Sí | Un punto de entrada optimizado para los costos para que los desarrolladores aprendan sobre Azure Container Registry. Los registros básicos tienen las mismas funcionalidades de programación que los estándar y premium (integración de la autenticación de Azure Active Directory, eliminación de imágenes y webhooks). Sin embargo, el almacenamiento incluido y el rendimiento de las imágenes son más adecuadas para escenarios de uso inferior. |
| **Estándar** | Sí | Los registros estándar ofrecen las mismas funcionalidades que los básicos, pero con más almacenamiento y un mayor rendimiento de las imágenes. Los registros estándar deberían satisfacer las necesidades de la mayoría de los escenarios de producción. |
| **Premium** | Sí | Los registros premium proporcionan la mayor cantidad de almacenamiento incluido y operaciones simultáneas, por lo que permiten trabajar con escenarios de mayor volumen. Además de mayor rendimiento de las imágenes, el nivel Premium agrega características como [georreplicación] [ container-registry-geo-replication] para administrar un único registro en varias regiones, [(versiónpreliminar)deconfianzadecontenido](container-registry-content-trust.md) para la firma de etiqueta de imagen, y [firewalls y redes virtuales (versión preliminar)](container-registry-vnet.md) para restringir el acceso al registro. |
| Clásico<sup>1</sup> | Sin  | Con esta SKU se habilitó la versión inicial del servicio Azure Container Registry en Azure. Los registros clásicos se basan en una cuenta de almacenamiento que Azure crea en su suscripción, lo que limita la capacidad de ACR de proporcionar funcionalidades de nivel superior, como el aumento del rendimiento y la replicación geográfica. |

<sup>1</sup> La SKU Clásico quedará **en desuso** en **marzo de 2019**. Utilice Básica, Estándar o Premium con todos los nuevos registros de contenedor.

Elegir una SKU de nivel superior proporciona más rendimiento y escalado, sin embargo, todas las SKU administradas proporcionan las mismas capacidades de programación. Con varios niveles de servicio, puede empezar a trabajar con Basic, a continuación, pasar a Standard y luego Premium a medida que aumente su uso del registro.

## <a name="managed-vs-unmanaged"></a>Registros administrados frente a no administrados

Las SKU Básico, Estándar y Premium se conocen colectivamente como registros *administrados* y los registros clásicos como *no administrados*. La diferencia principal entre los dos es cómo se almacenan las imágenes de contenedor.

### <a name="managed-basic-standard-premium"></a>Administrados (Básico, Estándar y Premium)

Los registros administrados aprovechan el almacenamiento de imágenes administrado totalmente por Azure. Es decir, una cuenta de almacenamiento que almacena las imágenes no aparece en su suscripción a Azure. El uso de una de las SKU de registro administrado tiene varias ventajas que se analizan en profundidad en [Almacenamiento de imágenes de contenedor en Azure Container Registry][container-registry-storage]. En este artículo se centra en las SKU de registro administrado y sus capacidades.

### <a name="unmanaged-classic"></a>No administrados (clásicos)

> [!IMPORTANT]
> La SKU clásica está en desuso y dejará de estar disponible a partir de marzo de 2019. Utilice Básico, Estándar o Premium con todos los nuevos registros de contenedor.

Los registros clásicos son "no administrados" en el sentido de que la cuenta de almacenamiento que respalda a un registro clásico se encuentra en *su* suscripción de Azure. Por lo tanto, usted es responsable de la administración de la cuenta de almacenamiento en el que se almacenan las imágenes de contenedor. Con los registros no administrados, no puede cambiar entre las SKU a medida que sus necesidades cambian (excepto [actualizar][container-registry-upgrade] a un registro administrado), y varias características de los registros administrados no están disponibles (por ejemplo, la eliminación de imágenes de contenedor, [la replicación geográfica][container-registry-geo-replication] y [los webhooks][container-registry-webhook]).

Para más información acerca de cómo actualizar un registro clásico a uno de SKU administradas, consulte [Actualización de un registro clásico][container-registry-upgrade].

## <a name="sku-feature-matrix"></a>Matriz de características de la SKU

En la tabla siguiente se detallan las características y los límites de los niveles de servicio Básico, Estándar y Premium.

[!INCLUDE [container-instances-limits](../../includes/container-registry-limits.md)]

## <a name="changing-skus"></a>Cambio de SKU

Puede cambiar la SKU de un registro con la CLI de Azure o en Azure Portal. Puede moverse libremente entre SKU administradas siempre que la SKU a la que se vaya a cambiar tenga la capacidad de almacenamiento máximo necesaria. Si cambia a una de las SKU administradas desde un registro clásico, ya no podrá volver al registro clásico: se trata de una conversión unidireccional.

### <a name="azure-cli"></a>Azure CLI

Para mover entre las SKU en la CLI de Azure, use el comando [az acr update][az-acr-update]. Por ejemplo, para cambiar a Premium:

```azurecli
az acr update --name myregistry --sku Premium
```

### <a name="azure-portal"></a>Azure Portal

En la **información general** del registro de contenedor en Azure Portal, seleccione **Actualizar** y después seleccione una **SKU** nueva en la lista desplegable de SKU.

![Actualización de la SKU del registro de contenedor en Azure Portal][update-registry-sku]

Si tiene un registro clásico, no puede seleccionar una SKU administrada en Azure Portal. En su lugar, primero tiene que [actualizar][container-registry-upgrade] a un registro administrado (consulte [Cambio a partir del nivel Clásico](#changing-from-classic)).

## <a name="changing-from-classic"></a>Cambio a partir del nivel Clásico

Existen consideraciones adicionales para tener en cuenta al migrar un registro clásico no administrado a una de las SKU administradas Básico, Estándar o Premium. Si el registro clásico contiene un gran número de imágenes y tiene muchos gigabytes de tamaño, el proceso de migración puede tardar algún tiempo. Además, las operaciones `docker push` se deshabilitan hasta que se complete la migración.

Para información más detallada acerca de cómo actualizar un registro clásico a uno de SKU administradas, consulte [Actualización de un registro clásico][container-registry-upgrade].

## <a name="pricing"></a>Precios

Para más información acerca de cómo precios para cada una de las SKU de Azure Container Registry, consulte [Precios de Container Registry][container-registry-pricing].

Para obtener más información acerca de los precios de las transferencias de datos, consulte [Detalles de precios de Bandwidth](https://azure.microsoft.com/pricing/details/bandwidth/). 

## <a name="next-steps"></a>Pasos siguientes

**Mapa de ruta de Azure Container Registry**

Visite el [mapa de ruta de ACR][acr-roadmap] en GitHub para más información acerca de las próximas características del servicio.

**UserVoice en Azure Container Registry**

Envíe y vote las sugerencias de nuevas características de [UserVoice en ACR][container-registry-uservoice].

<!-- IMAGES -->
[update-registry-sku]: ./media/container-registry-skus/update-registry-sku.png

<!-- LINKS - External -->
[acr-roadmap]: https://aka.ms/acr/roadmap
[container-registry-pricing]: https://azure.microsoft.com/pricing/details/container-registry/
[container-registry-uservoice]: https://feedback.azure.com/forums/903958-azure-container-registry

<!-- LINKS - Internal -->
[az-acr-update]: /cli/azure/acr#az-acr-update
[container-registry-geo-replication]: container-registry-geo-replication.md
[container-registry-upgrade]: container-registry-upgrade.md
[container-registry-storage]: container-registry-storage.md
[container-registry-webhook]: container-registry-webhook.md
