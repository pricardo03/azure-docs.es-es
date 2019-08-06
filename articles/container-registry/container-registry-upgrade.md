---
title: Actualización de un registro clásico de Azure Container Registry
description: Benefíciese del conjunto de características ampliado de los registros de contenedores administrados Básico, Estándar y Premium mediante la actualización de su registro de contenedor clásico no administrado.
services: container-registry
author: dlepow
manager: gwallace
ms.service: container-registry
ms.topic: article
ms.date: 03/26/2019
ms.author: danlep
ms.openlocfilehash: 05c227e7de078c6bb371049f16e191598b9ca4e5
ms.sourcegitcommit: f5075cffb60128360a9e2e0a538a29652b409af9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2019
ms.locfileid: "68310372"
---
# <a name="upgrade-a-classic-container-registry"></a>Actualización de un registro clásico

Azure Container Registry (ACR) está disponible en varios niveles de servicio, [conocidos como SKU](container-registry-skus.md). La versión inicial de ACR ofrecía una única SKU, clásica, que carece de varias características inherentes a las SKU de los niveles Básico, Estándar y Premium (de forma colectiva, conocidos como registros *administrados*).

La SKU clásica está en desuso y dejará de estar disponible a partir de abril de 2019. En este artículo se describe cómo migrar el registro clásico administrado a una de las SKU administradas, a fin de poder beneficiarse de su conjunto de características mejoradas.

## <a name="why-upgrade"></a>¿Por qué actualizar?

La SKU del registro clásico está **en desuso** y dejará de estar disponible a partir de **abril de 2019**. Todos los registros clásicos existentes se deben actualizar antes de abril de 2019. Las características de administración del portal de los registros clásicos acabarán por desaparecer. La creación de registros clásicos se deshabilitará después de abril de 2019.

Debido al aviso de desuso programado y a las funcionalidades limitadas de los registros clásicos no administrados, se deben actualizar todos los registros clásicos a registros administrados (niveles Básico, Estándar o Premium). Estas SKU de nivel superior integran mejor el registro en las funcionalidades de Azure. Para más información sobre los precios y las funcionalidades de los diversos niveles de servicio, consulte [SKU de Container Registry](container-registry-skus.md).

El registro clásico depende de la cuenta de almacenamiento que Azure aprovisiona automáticamente en su suscripción cuando crea el registro. Por el contrario, las SKU Basic, Standard y Premium aprovechan las [características de almacenamiento avanzadas](container-registry-storage.md) de Azure, para lo cual controlan de manera transparente el almacenamiento de las imágenes por usted. No se crea otra cuenta de almacenamiento en su propia suscripción.

El almacenamiento de registros administrados proporciona las siguientes ventajas:

* Las imágenes de contenedor se [cifran en reposo](container-registry-storage.md#encryption-at-rest).
* Las imágenes se almacenan mediante el [almacenamiento con redundancia geográfica](container-registry-storage.md#geo-redundant-storage), lo que garantiza la copia de seguridad de las imágenes con replicación en varias regiones (solo SDK Premium).
* Capacidad de [cambiar de SKU](container-registry-skus.md#changing-skus) con libertad, lo que aumenta el rendimiento al elegir una SKU de nivel superior. Con cada SKU, ACR puede cumplir los requisitos de rendimiento a medida que aumentan sus necesidades.
* El modelo de seguridad unificado para el registro y su almacenamiento ofrece una administración de derechos simplificada. Administrar permisos solo para el registro de contenedor, sin tener que administrar permisos para una cuenta de almacenamiento independiente.

Para obtener más detalles sobre el almacenamiento de imágenes en ACR, consulte [Almacenamiento de imágenes de contenedor en Azure Container Registry](container-registry-storage.md).

## <a name="migration-considerations"></a>Consideraciones sobre la migración

Al actualizar un registro clásico a uno administrado, Azure debe copiar todas las imágenes de contenedor existentes de la cuenta de almacenamiento creada para ACR en su suscripción en un cuenta de almacenamiento administrada por Azure. Según el tamaño del registro, este proceso puede tardar desde unos minutos hasta varias horas. Con fines de estimación, espere un tiempo de migración de aproximadamente 0,5 GiB por minuto.

Durante el proceso de conversión, las operaciones `docker push` están deshabilitadas durante el último 10 % de la migración. `docker pull` sigue funcionando con normalidad.

No elimine ni modifique el contenido de la cuenta de almacenamiento en que se almacena el registro clásico durante el proceso de conversión. Si lo hace, se pueden dañar las imágenes de contenedor.

Una vez finalizada la migración, Azure Container Registry deja de usar la cuenta de almacenamiento de su suscripción que hospedaba inicialmente el registro clásico. Después de haber verificado que la migración se realizó correctamente, considere la posibilidad de eliminar la cuenta de almacenamiento para minimizar los costos.

>[!IMPORTANT]
> La actualización de un registro clásico a una de las SKU administradas es un **proceso unidireccional**. Una vez que se convierte un registro clásico a un registro del nivel Básico, Estándar o Premium, ya no se puede revertir a clásico. Sin embargo, puede cambiar libremente entre las SKU administradas que tengan capacidad suficiente para el registro.

## <a name="how-to-upgrade"></a>Procedimiento de actualización

Puede actualizar un registro clásico no administrado a una de las SKU administradas de varias maneras. En las siguientes secciones, se describe el proceso para utilizar la [CLI de Azure][azure-cli] and the [Azure portal][azure-portal].

## <a name="upgrade-in-azure-cli"></a>Actualización en la CLI de Azure

Para actualizar un registro clásico en la CLI de Azure, ejecute el comando [az acr update][az-acr-update] y especifique la SKU nueva para el registro. En el ejemplo siguiente, un registro clásico llamado *myclassicregistry* se actualiza a la SKU Premium:

```azurecli-interactive
az acr update --name myclassicregistry --sku Premium
```

Una vez completada la migración, debería ver un resultado similar al siguiente. Tenga en cuenta que `sku` es "Premium" y `storageAccount` es `null`, lo que significa que Azure ahora administra el almacenamiento de imágenes de este registro.

```JSON
{
  "adminUserEnabled": false,
  "creationDate": "2017-12-12T21:23:29.300547+00:00",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myresourcegroup/providers/Microsoft.ContainerRegistry/registries/myregistry",
  "location": "eastus",
  "loginServer": "myregistry.azurecr.io",
  "name": "myregistry",
  "provisioningState": "Succeeded",
  "resourceGroup": "myresourcegroup",
  "sku": {
    "name": "Premium",
    "tier": "Premium"
  },
  "status": null,
  "storageAccount": null,
  "tags": {},
  "type": "Microsoft.ContainerRegistry/registries"
}
```

Si especifica una SKU de registro administrado cuya capacidad máxima es inferior al tamaño del registro clásico, recibirá un mensaje de error similar al siguiente.

`Cannot update the registry SKU due to reason: Registry size 12936251113 bytes exceeds the quota value 10737418240 bytes for SKU Basic. The suggested SKU is Standard.`

Si recibe un error similar, vuelva a ejecutar el comando [az acr update][az-acr-update] y especifique la SKU recomendada, que es la SKU del siguiente nivel más alto que puede almacenar las imágenes.

## <a name="upgrade-in-azure-portal"></a>Actualización en Azure Portal

Cuando se actualiza un registro clásico mediante Azure Portal, Azure selecciona automáticamente la SKU Estándar o Premium, según aquella en la que se puedan alojar las imágenes. Por ejemplo, si el registro contiene 100 GiB de imágenes, Azure selecciona automáticamente el registro clásico y lo convierte a Estándar (100 GiB como máximo).

Para actualizar el registro clásico en Azure Portal, vaya al registro de contenedor **Introducción** y seleccione **Actualizar al registro administrado**.

![Botón de actualización del registro clásico de la interfaz de usuario de Azure Portal][update-classic-01-upgrade]

Seleccione **Aceptar** para confirmar que desea actualizar a un registro administrado.

Durante la migración, el portal indica que el **estado de aprovisionamiento** del registro es *Actualizando*. Como se mencionó anteriormente, las operaciones `docker push` están deshabilitadas durante el último 10 % de la migración. No elimine ni actualice la cuenta de almacenamiento usada con el registro clásico mientras la migración está en curso, ya que las imágenes pueden resultar dañadas.

![Progreso de actualización del registro clásico en la interfaz de usuario de Azure Portal][update-classic-03-updating]

Una vez completada la migración, el **estado de aprovisionamiento** indica *Correcto*, y puede reanudar las operaciones normales con su registro.

![Estado de finalización de la actualización del registro clásico en la interfaz de usuario de Azure Portal][update-classic-04-updated]

## <a name="next-steps"></a>Pasos siguientes

Una vez actualizado un registro clásico a registro administrado, Azure deja de usar la cuenta de almacenamiento que originalmente almacenaba el registro clásico. Para reducir los costos, considere la posibilidad de eliminar la cuenta de almacenamiento o el contenedor de blobs de la cuenta que contiene las imágenes de contenedor anteriores.

<!-- IMAGES -->
[update-classic-01-upgrade]: ./media/container-registry-upgrade/update-classic-01-upgrade.png
[update-classic-02-confirm]: ./media/container-registry-upgrade/update-classic-02-confirm.png
[update-classic-03-updating]: ./media/container-registry-upgrade/update-classic-03-updating.png
[update-classic-04-updated]: ./media/container-registry-upgrade/update-classic-04-updated.png

<!-- LINKS - internal -->
[az-acr-update]: /cli/azure/acr#az-acr-update
[azure-cli]: /cli/azure/install-azure-cli
[azure-portal]: https://portal.azure.com