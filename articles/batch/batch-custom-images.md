---
title: 'Uso de Shared Image Gallery para crear un grupo: Azure Batch | Microsoft Docs'
description: Cree un grupo de Batch con Shared Image Gallery para aprovisionar imágenes personalizadas para los nodos de ejecución que contienen el software y los datos que se necesitan para la aplicación. Las imágenes personalizadas son una manera eficaz de configurar los nodos de proceso para ejecutar las cargas de Batch.
services: batch
author: laurenhughes
manager: gwallace
ms.service: batch
ms.topic: article
ms.date: 08/14/2019
ms.author: lahugh
ms.openlocfilehash: 00da17512cbc2e713955ea83c7d9fa7517958169
ms.sourcegitcommit: 18061d0ea18ce2c2ac10652685323c6728fe8d5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/15/2019
ms.locfileid: "69036695"
---
# <a name="use-the-shared-image-gallery-to-create-a-pool"></a>Uso de Shared Image Gallery para crear un grupo

Al crear un grupo en Azure Batch con Configuración de máquina virtual, se especifica una imagen de máquina virtual (VM) que proporciona el sistema operativo para cada nodo de proceso en el grupo. Puede crear un grupo de máquinas virtuales con una imagen de Azure Marketplace compatible o crear una imagen personalizada con [Shared Image Gallery](../virtual-machines/windows/shared-image-galleries.md).

## <a name="benefits-of-the-shared-image-gallery"></a>Ventajas de Shared Image Gallery

Al usar Shared Image Gallery para la imagen personalizada, tiene el control sobre la configuración y el tipo del sistema operativo, así como el tipo de discos de datos. La imagen de Shared Image puede incluir los datos de referencia y las aplicaciones que están disponibles en los nodo de grupo de Batch en cuanto se aprovisionan.

También puede tener varias versiones de una imagen según sea necesario para su entorno. Cuando se usa una versión de la imagen para crear una VM, la versión de la imagen se usa para crear nuevos discos para la VM. 

El uso de una imagen de Shared Image le ahorra tiempo a la hora de preparar los nodos de ejecución del grupo para ejecutar la carga de trabajo de Batch. Se puede usar una imagen de Azure Marketplace e instalar el software en cada nodo de ejecución una vez que se haya aprovisionado, pero el uso de una imagen de Shared Image podría ser más eficiente. Además, puede especificar varias réplicas para la imagen de Shared Image, por lo que al crear grupos con muchas VM (más de 600), ahorrará tiempo en la creación del grupo.

El uso de una imagen de Shared Image configurada para su escenario puede proporcionar varias ventajas:

* **Uso de las mismas imágenes en todas las regiones.** Puede crear réplicas de imágenes de Shared Image en distintas regiones para que todos los grupos usen la misma imagen.
* **Configuración del sistema operativo (SO).** La configuración del disco de sistema operativo de la imagen se puede personalizar.
* **Preinstalar las aplicaciones.** Preinstalar las aplicaciones en el disco del sistema operativo es más eficaz y menos propenso a errores que instalar las aplicaciones después de aprovisionar los nodos de ejecución con una tarea de inicio.
* **Copia de grandes cantidades de datos una vez.** Convierta en estáticos parte de los datos de la imagen de Shared Image administrada copiándolos en los discos de datos de una imagen administrada. Solo debe hacerse una vez y los datos estarán disponibles para cada nodo del grupo.
* **Aumento de los grupos a tamaños más grandes.** Con Shared Image Gallery puede crear grupos más grandes con sus imágenes personalizadas junto con más réplicas de Shared Image.
* **Mejor rendimiento que la imagen personalizada.** Mediante el uso de imágenes de Shared Image, el tiempo que tarda el grupo en alcanzar el estado estable es hasta un 25 % más rápido y la latencia de inactividad de la VM es hasta un 30 % más breve.
* **Control de versiones y agrupación de imágenes para facilitar su administración.** La definición de la agrupación de imágenes contiene información acerca de por qué se creó la imagen, para qué sistema operativo sirve e información acerca del uso de la imagen. La agrupación de imágenes permite una administración más sencilla de las imágenes. Para más información, consulte [Definiciones de imagen](../virtual-machines/windows/shared-image-galleries.md#image-definitions).

## <a name="prerequisites"></a>Requisitos previos

* **Una cuenta de Azure Batch** Para crear una cuenta de Batch, consulte las guías de inicio rápido de Batch con [Azure Portal](quick-create-portal.md) o la [CLI de Azure](quick-create-cli.md).

* **Una imagen de Shared Image Gallery**. Para obtener más información y conocer los pasos necesarios para preparar una imagen de Shared Image, consulte [Creación de una imagen de Shared Image Gallery con la CLI de Azure](../virtual-machines/linux/shared-images.md) o [Creación de una imagen de Shared Image Gallery mediante Azure Portal](../virtual-machines/linux/shared-images-portal.md).

> [!NOTE]
> La imagen de Shared Image debe estar en la misma suscripción que la cuenta de Batch. La imagen de Shared Image puede estar en distintas regiones, siempre que tenga réplicas en la misma región que la cuenta de Batch.

## <a name="create-a-pool-from-a-shared-image-using-the-azure-cli"></a>Creación de un grupo a partir de una imagen de Shared Image con la CLI de Azure

Para crear un grupo a partir de la imagen de Shared Image mediante la CLI de Azure, use el comando `az batch pool create`. Especifique el identificador de la imagen de Shared Image en el campo `--image`. Asegúrese de que el tipo de sistema operativo y la SKU coinciden con las versiones especificadas por `--node-agent-sku-id`.

```azurecli
az batch pool create \
    --id mypool --vm-size Standard_A1_v2 \
    --target-dedicated-nodes 2 \
    --image "/subscriptions/{sub id}/resourceGroups/{resource group name}/providers/Microsoft.Compute/galleries/{gallery name}/images/{image definition name}/versions/{version id}" \
    --node-agent-sku-id "batch.node.ubuntu 16.04"
```

## <a name="create-a-pool-from-a-shared-image-using-c"></a>Creación de un grupo a partir de una imagen de Shared Image mediante C#

Como alternativa, puede crear un grupo a partir de una imagen de Shared Image mediante el SDK de C#.

```csharp
private static VirtualMachineConfiguration CreateVirtualMachineConfiguration(ImageReference imageReference)
{
    return new VirtualMachineConfiguration(
        imageReference: imageReference,
        nodeAgentSkuId: "batch.node.windows amd64");
}

private static ImageReference CreateImageReference()
{
    return new ImageReference(
        virtualMachineImageId: "/subscriptions/{sub id}/resourceGroups/{resource group name}/providers/Microsoft.Compute/galleries/{gallery name}/images/{image definition name}/versions/{version id}");
}

private static void CreateBatchPool(BatchClient batchClient, VirtualMachineConfiguration vmConfiguration)
{
    try
    {
        CloudPool pool = batchClient.PoolOperations.CreatePool(
            poolId: PoolId,
            targetDedicatedComputeNodes: PoolNodeCount,
            virtualMachineSize: PoolVMSize,
            virtualMachineConfiguration: vmConfiguration);

        pool.Commit();
    }
    ...
}
```

## <a name="considerations-for-large-pools"></a>Consideraciones para los grupos grandes

Si tiene previsto crear un grupo con cientos o miles de VM o más mediante una imagen de Shared Image, use la siguiente guía.

* **Números de réplicas de Shared Image Gallery.**  Para cada grupo con hasta 600 instancias, se recomienda conservar al menos una réplica. Por ejemplo, si va a crear un grupo con 3000 VM, debe mantener al menos cinco réplicas de la imagen. Siempre se recomienda mantener más réplicas que las indicadas por los requisitos mínimos para un mejor rendimiento.

* **Tiempo de espera del cambio de tamaño**: si el grupo contiene un número de nodos fijo (no se escala automáticamente), aumente la propiedad `resizeTimeout` del grupo en función de su tamaño. Por cada 1000 VM, el tiempo de espera del cambio de tamaño recomendado es de al menos 15 minutos. Por ejemplo, el tiempo de espera del cambio de tamaño recomendado para un grupo con 2000 VM es de al menos 30 minutos.

## <a name="next-steps"></a>Pasos siguientes

* Para información general más detallada acerca de Batch, consulte [Desarrollo de soluciones de procesos paralelos a gran escala con Batch](batch-api-basics.md).
