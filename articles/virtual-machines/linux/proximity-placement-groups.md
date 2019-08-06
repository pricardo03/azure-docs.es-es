---
title: Uso de la versión preliminar de los grupos de selección de ubicación de proximidad para máquinas virtuales Linux | Microsoft Docs
description: Obtenga información sobre la creación y el uso de los grupos de selección de ubicación de proximidad para máquinas virtuales Linux en Azure.
services: virtual-machines-linux
author: cynthn
manager: jeconnoc
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 07/01/2019
ms.author: cynthn
ms.openlocfilehash: a3009c9863d8322e3fe34bd99d64b93f0aa3d858
ms.sourcegitcommit: 04ec7b5fa7a92a4eb72fca6c6cb617be35d30d0c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/22/2019
ms.locfileid: "68385736"
---
# <a name="preview-deploy-vms-to-proximity-placement-groups-using-azure-cli"></a>Vista previa: Implementación de máquinas virtuales en grupos de selección de ubicación de proximidad con la CLI de Azure

Para acercar las máquinas virtuales lo máximo posible con la menor latencia, debe implementarlas dentro de un [grupo de selección de ubicación de proximidad](co-location.md#preview-proximity-placement-groups).

Un grupo de selección de ubicación de proximidad es una agrupación lógica que se usa para asegurarse de que los recursos de proceso de Azure se encuentran físicamente cercanos entre sí. Los grupos de selección de ubicación de proximidad son útiles para las cargas de trabajo en las que la latencia baja es un requisito.

> [!IMPORTANT]
> Los grupos de selección de ubicación de proximidad se encuentran actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
>
> Los grupos de selección de ubicación de proximidad no están disponibles en estas regiones durante la versión preliminar: **Japón Oriental**, **Este de Australia** e **India central**.

## <a name="create-the-proximity-placement-group"></a>Creación de un grupo de selección de ubicación de proximidad
Cree un grupo de selección de ubicación de proximidad con [`az ppg create`](/cli/azure/ppg#az-ppg-create). 

```azurecli-interactive
az group create --name myPPGGroup --location westus
az ppg create \
   -n myPPG \
   -g myPPGGroup \
   -l westus \
   -t standard 
```

## <a name="list-proximity-placement-groups"></a>Enumeración de los grupos de selección de ubicación de proximidad

Puede enumerar todos los grupos de selección de ubicación de proximidad con [az ppg list](/cli/azure/ppg#az-ppg-list).

```azurecli-interactive
az ppg list -o table
```

## <a name="create-a-vm"></a>Crear una VM

Cree una máquina virtual dentro del grupo de selección de ubicación de proximidad con [new az vm](/cli/azure/vm#az-vm-create).
```azurecli-interactive
az vm create \
   -n myVM \
   -g myPPGGroup \
   --image UbuntuLTS \
   --ppg myPPG  \
   --generate-ssh-keys \
   --size Standard_D1_v2  \
   -l westus
```

Puede ver la máquina virtual en el grupo de selección de ubicación de proximidad mediante [az ppg show](/cli/azure/ppg#az-ppg-show).

```azurecli-interactive
az ppg show --name myppg --resource-group myppggroup --query "virtualMachines"
```

## <a name="availability-sets"></a>Conjuntos de disponibilidad
También puede crear un conjunto de disponibilidad en el grupo de selección de ubicación de proximidad. Use el mismo parámetro `--ppg` con [az vm availability-set create](/cli/azure/vm/availability-set#az-vm-availability-set-create) para crear un conjunto de disponibilidad, y todas las máquinas virtuales del conjunto de disponibilidad también se crearán en el mismo grupo de selección de ubicación de proximidad.

## <a name="scale-sets"></a>Conjuntos de escalado

También puede crear un conjunto de escalado en el grupo de selección de ubicación de proximidad. Use el mismo parámetro `--ppg` con [az vmss create](/cli/azure/vmss?view=azure-cli-latest#az-vmss-create) para crear un conjunto de escalado, y todas las instancias se crearán en el mismo grupo de selección de ubicación de proximidad.

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre los comandos de la [CLI de Azure](/cli/azure/ppg) para los grupos de selección de ubicación de proximidad.
