---
title: 'Tutorial: Alta disponibilidad para máquinas virtuales Linux en Azure'
description: En este tutorial, aprenderá a usar la CLI de Azure para implementar máquinas virtuales de alta disponibilidad en conjuntos de disponibilidad.
documentationcenter: ''
services: virtual-machines-linux
author: cynthn
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.topic: tutorial
ms.date: 08/24/2018
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: cd0366a0029ccc4816308e280ac93b7c724bb82a
ms.sourcegitcommit: 49cf9786d3134517727ff1e656c4d8531bbbd332
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/13/2019
ms.locfileid: "74034625"
---
# <a name="tutorial-create-and-deploy-highly-available-virtual-machines-with-the-azure-cli"></a>Tutorial: Creación e implementación de máquinas virtuales de alta disponibilidad con la CLI de Azure

En este tutorial, obtendrá información sobre cómo aumentar la disponibilidad y confiabilidad de las soluciones de máquina virtual en Azure mediante una funcionalidad denominada "conjuntos de disponibilidad". Los conjuntos de disponibilidad garantizan que las máquinas virtuales implementadas en Azure se distribuyan entre varios clústeres de hardware aislados. De este modo, se asegura de que, si se produce un error de hardware o software en Azure, solo un subconjunto de las máquinas virtuales se verá afectado y que la solución seguirá disponible y en funcionamiento.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Crear un conjunto de disponibilidad
> * Crear una máquina virtual en un conjunto de disponibilidad
> * Comprobar los tamaños de máquina virtual disponibles

En este tutorial se usa la CLI dentro de [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview), que se actualiza constantemente a la versión más reciente. Para abrir Cloud Shell, seleccione **Pruébelo** en la esquina superior de cualquier bloque de código.

Si decide instalar y usar la CLI localmente, en este tutorial es preciso que ejecute la CLI de Azure de la versión 2.0.30, u otra posterior. Ejecute `az --version` para encontrar la versión. Si necesita instalarla o actualizarla, vea [Instalación de la CLI de Azure]( /cli/azure/install-azure-cli).

## <a name="high-availability-in-azure-overview"></a>Información general sobre la alta disponibilidad en Azure
La alta disponibilidad en Azure se puede crear de muchas maneras diferentes. Los conjuntos de disponibilidad y las zonas de disponibilidad son dos de las opciones con las que cuenta. Mediante el uso de conjuntos de disponibilidad, las máquinas virtuales estarán protegidas ante los errores que pueden producirse en un centro de datos. Esto incluye errores de hardware y errores de software de Azure. Mediante el uso de zonas de disponibilidad, las máquinas virtuales se situarán en una infraestructura físicamente separada sin recursos compartidos y, por tanto, estarán protegidas ante los errores en todo el centro de datos.

Use los conjuntos o las zonas de disponibilidad cuando quiera implementar en Azure soluciones confiables basadas en máquinas virtuales.

### <a name="availability-set-overview"></a>Información general sobre conjuntos de disponibilidad

Un conjunto de disponibilidad es una funcionalidad de agrupación lógica que puede usar en Azure para asegurarse de que los recursos de máquina virtual que coloque en dicho conjunto de disponibilidad estén aislados entre sí cuando se implementen en un centro de datos de Azure. Azure garantiza que las máquinas virtuales colocados en un conjunto de disponibilidad se ejecuten en varios servidores físicos, grupos de proceso, unidades de almacenamiento y conmutadores de red. Si se produce un error de hardware o software de Azure, solo un subconjunto de las máquinas virtuales se ve afectado y la aplicación se mantiene actualizada y sigue estando disponible para los clientes. Los conjuntos de disponibilidad son una funcionalidad fundamental para compilar soluciones en la nube confiables.

Veamos una solución basada en máquina virtual típica en la podría haber cuatro servidores web front-end y usar dos máquinas virtuales de back-end que hospedan una base de datos. Con Azure, desea definir 2 conjuntos de disponibilidad antes de implementar las máquinas virtuales: un conjunto de disponibilidad para el nivel de "web" y otro para el nivel de "base de datos". Al crear una máquina virtual, podrá especificar el conjunto de disponibilidad como un parámetro en el comando az vm create, y Azure garantiza automáticamente que las máquinas virtuales que cree en el conjunto de disponibilidad estén aisladas en varios recursos de hardware físico. Si el hardware físico que está ejecutando una de sus máquinas virtuales del servidor web o del servidor de base de datos tiene un problema, sabe que las demás instancias de las máquinas virtuales del servidor de base de datos y del servidor web siguen ejecutándose correctamente porque están en un hardware diferente.

### <a name="availability-zone-overview"></a>Información general sobre las zonas de disponibilidad

Las zonas de disponibilidad son una oferta que protege las aplicaciones y datos de los errores del centro de datos. Las zonas de disponibilidad son ubicaciones físicas exclusivas dentro de una región de Azure. Cada zona de disponibilidad consta de uno o varios centros de datos equipados con alimentación, refrigeración y redes independientes. Para garantizar la resistencia, en todas las regiones habilitadas hay al menos tres zonas independientes. La separación física de las zonas de disponibilidad dentro de una región protege las aplicaciones y los datos frente a los errores del centro de datos. Los servicios con redundancia de zona replican las aplicaciones y los datos entre zonas de disponibilidad para protegerlos frente a puntos de error únicos. Con las zonas de disponibilidad, Azure ofrece el mejor Acuerdo de Nivel de Servicio del sector en cuanto a tiempo de actividad de máquina virtual, con un 99,99 %.

De forma similar a los conjuntos de disponibilidad, veamos una solución basada en máquina virtual típica en la podría haber cuatro servidores web front-end y se podrían usar dos máquinas virtuales de back-end que hospedan una base de datos. Como en los conjuntos de disponibilidad, querrá implementar las máquinas virtuales en dos zonas de disponibilidad independientes: una zona de disponibilidad para el nivel "web" y una zona de disponibilidad para el nivel "base de datos". Al crear una máquina virtual y especificar la zona de disponibilidad como un parámetro en el comando az vm create, Azure garantiza automáticamente que las máquinas virtuales que se crean se aíslan en varias zonas de disponibilidad totalmente diferentes. Si todo el centro de datos en el que se está ejecutando una de las máquinas virtuales del servidor web o del servidor de bases de datos tiene un problema, sabe que las demás instancias de las máquinas virtuales del servidor de bases de datos y del servidor web siguen ejecutándose en centros de datos completamente independientes.

## <a name="create-an-availability-set"></a>Crear un conjunto de disponibilidad

Puede crear el conjunto de disponibilidad mediante [az vm availability-set create](/cli/azure/vm/availability-set). En este ejemplo, se establece el número de dominios de actualización y de error en *2* para el conjunto de disponibilidad denominado *myAvailabilitySet* en el grupo de recursos *myResourceGroupAvailability*.

En primer lugar, cree un grupo de recursos con [az group create](/cli/azure/group#az-group-create) y luego cree el conjunto de disponibilidad.

```azurecli-interactive
az group create --name myResourceGroupAvailability --location eastus

az vm availability-set create \
    --resource-group myResourceGroupAvailability \
    --name myAvailabilitySet \
    --platform-fault-domain-count 2 \
    --platform-update-domain-count 2
```

Los conjuntos de disponibilidad permiten aislar los recursos en dominios de error y dominios de actualización. Un **dominio de error** representa una colección aislada de recursos de almacenamiento, red y servidor. En el ejemplo anterior, el conjunto de disponibilidad se distribuye entre al menos dos dominios de error cuando se implementan las máquinas virtuales. El conjunto de disponibilidad también se distribuye entre dos **dominios de actualización**. El uso de dos dominios de actualización garantiza que cuando Azure lleve a cabo actualizaciones de software, los recursos de máquina virtual están aislados, lo que evita que todo el software que se ejecuta en la máquina virtual se actualice al mismo tiempo.


## <a name="create-vms-inside-an-availability-set"></a>Creación de VM dentro de un conjunto de disponibilidad

Las máquinas virtuales deben crearse en el conjunto de disponibilidad para asegurarse de que se distribuyan correctamente en el hardware. No se puede agregar una máquina virtual existente a un conjunto de disponibilidad después de que se crea.

Cuando se cree una máquina virtual con [az vm create](/cli/azure/vm), use el parámetro `--availability-set` para especificar el nombre del conjunto de disponibilidad.

```azurecli-interactive
for i in `seq 1 2`; do
   az vm create \
     --resource-group myResourceGroupAvailability \
     --name myVM$i \
     --availability-set myAvailabilitySet \
     --size Standard_DS1_v2  \
     --vnet-name myVnet \
     --subnet mySubnet \
     --image UbuntuLTS \
     --admin-username azureuser \
     --generate-ssh-keys
done
```

Ahora hay dos máquinas virtuales dentro del conjunto de disponibilidad. Como se encuentran en el mismo conjunto de disponibilidad, Azure se asegura de que las máquinas virtuales y todos sus recursos (incluidos los discos de datos) se distribuyen entre el hardware físico aislado. Esta distribución ayuda a garantizar una mayor disponibilidad de la solución general de máquina virtual.

La distribución del conjunto de disponibilidad puede verse en el portal en Grupos de recursos > myResourceGroupAvailability > myAvailabilitySet. Las máquinas virtuales se distribuyen entre los dos dominios de actualización o de error, tal como se muestra en el ejemplo siguiente:

![Conjunto de disponibilidad en el portal](./media/tutorial-availability-sets/fd-ud.png)

## <a name="check-for-available-vm-sizes"></a>Comprobación de los tamaños de VM disponibles

Más adelante, se pueden agregar máquinas virtuales adicionales al conjunto de disponibilidad, donde los tamaños de máquina virtual están disponibles en el hardware. Use [az vm availability-set list-sizes](/cli/azure/vm/availability-set#az-vm-availability-set-list-sizes) para enumerar todos los tamaños disponibles en el clúster de hardware para el conjunto de disponibilidad.

```azurecli-interactive
az vm availability-set list-sizes \
     --resource-group myResourceGroupAvailability \
     --name myAvailabilitySet \
     --output table
```

## <a name="next-steps"></a>Pasos siguientes

En este tutorial aprendió lo siguiente:

> [!div class="checklist"]
> * Crear un conjunto de disponibilidad
> * Crear una máquina virtual en un conjunto de disponibilidad
> * Comprobar los tamaños de máquina virtual disponibles

Avance al siguiente tutorial para informarse sobre los conjuntos de escalado de máquinas virtuales.

> [!div class="nextstepaction"]
> [Creación de un conjunto de escalado de máquinas virtuales](tutorial-create-vmss.md)

* Para más información sobre las zonas de disponibilidad, visite la [documentación sobre zonas de disponibilidad](../../availability-zones/az-overview.md).
* También puede encontrar más documentación sobre los conjuntos y las zonas de disponibilidad [aquí](./manage-availability.md).
* Para probar las zonas de disponibilidad, consulte [Creación de una máquina virtual Linux en una zona de disponibilidad con la CLI de Azure](./create-cli-availability-zone.md).
