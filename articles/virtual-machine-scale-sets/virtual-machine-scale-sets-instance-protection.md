---
title: Instancias del conjunto de protección de la instancia de escalado de máquinas virtuales de Azure | Microsoft Docs
description: Obtenga información sobre cómo proteger instancias del conjunto de escalado de máquina virtual de Azure de las operaciones de escalado y conjunto de escalado.
services: virtual-machine-scale-sets
documentationcenter: ''
author: mayanknayar
manager: drewm
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/22/2019
ms.author: manayar
ms.openlocfilehash: 61430f5a43a04fa0e5b2f0c79ff03419c73aaf28
ms.sourcegitcommit: c05618a257787af6f9a2751c549c9a3634832c90
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/30/2019
ms.locfileid: "66416552"
---
# <a name="instance-protection-for-azure-virtual-machine-scale-set-instances-preview"></a>Protección de la instancia de escalado de máquinas virtuales de Azure establecida instances (versión preliminar)
Conjuntos de escalado de máquina virtual de Azure permiten elasticidad mejor para sus cargas de trabajo a través de [escalado automático](virtual-machine-scale-sets-autoscale-overview.md), por lo que puede configurar cuando se escala horizontalmente la infraestructura y cuando escala en. Conjuntos de escalado también permiten administrar centralmente, configurar y actualizar un gran número de máquinas virtuales a través de diferentes [directiva de actualización](virtual-machine-scale-sets-upgrade-scale-set.md#how-to-bring-vms-up-to-date-with-the-latest-scale-set-model) configuración. Puede configurar una actualización en el modelo de conjunto de escalado y la nueva configuración se aplica automáticamente a cada instancia del conjunto de escalado si ha configurado la directiva de actualización en automático o graduales.

Como su aplicación procesa el tráfico, puede haber situaciones en las que desee instancias específicas que se tratan de forma distinta del resto de la escala de la instancia de conjunto. Por ejemplo, ciertas instancias del conjunto de escalado podrían realizar las operaciones de larga ejecución, y no desea que estas instancias se escalan en hasta que se completen las operaciones. También podría especializados unas cuantas instancias en el conjunto de escalado para realizar tareas adicionales o diferentes que los demás miembros del conjunto de escalado. Se requieren estas máquinas virtuales "especiales" para que no se puede modificar con las demás instancias del conjunto de escalado. Protección de la instancia proporciona los controles adicionales para habilitar estos y otros escenarios para la aplicación.

En este artículo se describe cómo puede aplicar y utilizar las capacidades de protección de la otra instancia con instancias del conjunto de escalado.

> [!NOTE]
>Protección de la instancia está actualmente en versión preliminar pública. No es necesario ningún procedimiento opcional para usar la funcionalidad de versión preliminar pública que se describe a continuación. Vista previa de protección de instancia solo se admite con la API versión 2019-03-01 y en conjuntos de escalado con discos administrados.

## <a name="types-of-instance-protection"></a>Tipos de protección de la instancia
Conjuntos de escalado proporcionan dos tipos de capacidades de protección de instancia:

-   **Proteger desde la escala**
    - Habilita a través de **protectFromScaleIn** propiedad en la escala de la instancia de conjunto
    - Protege la instancia de la escala en escalado automático que se inició
    - Las operaciones de la instancia iniciada por el usuario (incluida la eliminación de instancia) son **no bloqueado**
    - Las operaciones iniciadas en el conjunto de escalado (actualizar, crear una nueva imagen, desasignar, etc.) son **no bloqueado**

-   **Proteger contra las acciones del conjunto de escalado**
    - Habilita a través de **protectFromScaleSetActions** propiedad en la escala de la instancia de conjunto
    - Protege la instancia de la escala en escalado automático que se inició
    - Protege la instancia de las operaciones iniciadas en el conjunto de escalado (como la actualización, la imagen inicial, desasignar, etcetera.)
    - Las operaciones de la instancia iniciada por el usuario (incluida la eliminación de instancia) son **no bloqueado**
    - La eliminación del conjunto de escalado completo está **no bloqueado**

## <a name="protect-from-scale-in"></a>Proteger desde la escala
Protección de la instancia se puede aplicar para escalar las instancias del conjunto una vez creadas las instancias. Protección se aplica y modificar solo en el [modelo de instancia](virtual-machine-scale-sets-upgrade-scale-set.md#the-scale-set-vm-model-view) y no en el [modelo de conjunto de escalado](virtual-machine-scale-sets-upgrade-scale-set.md#the-scale-set-model).

Hay varias maneras de aplicar la protección de escala en las instancias del conjunto de escalado como se detalla en los ejemplos siguientes.

### <a name="rest-api"></a>API DE REST

El ejemplo siguiente aplica protección de escala a una instancia del conjunto de escalado.

```
PUT on `/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachineScaleSets/{vmScaleSetName}/virtualMachines/{instance-id}?api-version=2019-03-01`
```

```json
{
  "properties": {
    "protectionPolicy": {
      "protectFromScaleIn": true
    }
  }        
}

```

> [!NOTE]
>Protección de la instancia solo es compatible con la API versión 2019-03-01 y versiones posteriores

### <a name="azure-powershell"></a>Azure PowerShell

Use la [actualización AzVmssVM](/powershell/module/az.compute/update-azvmssvm) para aplicar protección de escala en la escala de la instancia de conjunto.

El ejemplo siguiente aplica protección de escala a una instancia en el conjunto de escalado de Id. de instancia 0.

```azurepowershell-interactive
Update-AzVmssVM `
  -ResourceGroupName "myResourceGroup" `
  -VMScaleSetName "myVMScaleSet" `
  -InstanceId 0 `
  -ProtectFromScaleIn $true
```

### <a name="azure-cli-20"></a>CLI de Azure 2.0

Use [az vmss update](/cli/azure/vmss#az-vmss-update) para aplicar protección de escala a la instancia del conjunto de escalado.

El ejemplo siguiente aplica protección de escala a una instancia en el conjunto de escalado de Id. de instancia 0.

```azurecli-interactive
az vmss update \  
  --resource-group <myResourceGroup> \
  --name <myVMScaleSet> \
  --instance-id 0 \
  --protect-from-scale-in true
```

## <a name="protect-from-scale-set-actions"></a>Proteger contra las acciones del conjunto de escalado
Protección de la instancia se puede aplicar para escalar las instancias del conjunto una vez creadas las instancias. Protección se aplica y modificar solo en el [modelo de instancia](virtual-machine-scale-sets-upgrade-scale-set.md#the-scale-set-vm-model-view) y no en el [modelo de conjunto de escalado](virtual-machine-scale-sets-upgrade-scale-set.md#the-scale-set-model).

Proteger una instancia de una acción de conjunto de escalado también protege la instancia de la escala en escalado automático que se inició.

Hay varias formas de aplicar la escala establecer protección de las acciones en su escala instancias del conjunto tal como se detalla en los ejemplos siguientes.

### <a name="rest-api"></a>API DE REST

El ejemplo siguiente aplica la protección de las acciones del conjunto de escalado a una instancia del conjunto de escalado.

```
PUT on `/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachineScaleSets/{vMScaleSetName}/virtualMachines/{instance-id}?api-version=2019-03-01`
```

```json
{
  "properties": {
    "protectionPolicy": {
      "protectFromScaleIn": true,
      "protectFromScaleSetActions": true
    }
  }        
}

```

> [!NOTE]
>Protección de instancia solo es compatible con la API versión 2019-03-01 y versiones posteriores.</br>
Proteger una instancia de una acción de conjunto de escalado también protege la instancia de la escala en escalado automático que se inició. No se puede especificar "protectFromScaleIn": false cuando se establece en "protectFromScaleSetActions": true

### <a name="azure-powershell"></a>Azure PowerShell

Use la [actualización AzVmssVM](/powershell/module/az.compute/update-azvmssvm) para aplicar la protección frente a escalado establece acciones en la instancia del conjunto de escalado.

El ejemplo siguiente aplica la protección de las acciones del conjunto de escalado a una instancia en el conjunto de escalado de Id. de instancia 0.

```azurepowershell-interactive
Update-AzVmssVM `
  -ResourceGroupName "myResourceGroup" `
  -VMScaleSetName "myVMScaleSet" `
  -InstanceId 0 `
  -ProtectFromScaleIn $true `
  -ProtectFromScaleSetAction $true
```

### <a name="azure-cli-20"></a>CLI de Azure 2.0

Use [az vmss update](/cli/azure/vmss#az-vmss-update) para aplicar la protección de las acciones del conjunto de escalado para la instancia del conjunto de escalado.

El ejemplo siguiente aplica la protección de las acciones del conjunto de escalado a una instancia en el conjunto de escalado de Id. de instancia 0.

```azurecli-interactive
az vmss update \  
  --resource-group <myResourceGroup> \
  --name <myVMScaleSet> \
  --instance-id 0 \
  --protect-from-scale-in true \
  --protect-from-scale-set-actions true
```

## <a name="troubleshoot"></a>Solución de problemas
### <a name="no-protectionpolicy-on-scale-set-model"></a>No hay protectionPolicy en el modelo de conjunto de escalado
Protección de la instancia solo es aplicable en instancias del conjunto de escalado y no en el modelo de conjunto de escalado.

### <a name="no-protectionpolicy-on-scale-set-instance-model"></a>No hay protectionPolicy en el modelo de instancia del conjunto de escalado
De forma predeterminada, la directiva de protección no se aplica a una instancia cuando se crea.

Puede aplicar protección de instancias para escalar las instancias del conjunto una vez creadas las instancias.

### <a name="not-able-to-apply-instance-protection"></a>No se puede aplicar la protección de la instancia
Protección de instancia solo es compatible con la API versión 2019-03-01 y versiones posteriores. Comprobar la versión de API que se va a usar y actualizar según sea necesario. Es posible que también deberá actualizar su PowerShell o CLI para la versión más reciente.

## <a name="next-steps"></a>Pasos siguientes
Obtenga información sobre cómo [implementar la aplicación](virtual-machine-scale-sets-deploy-app.md) en conjuntos de escalado de máquinas virtuales.
