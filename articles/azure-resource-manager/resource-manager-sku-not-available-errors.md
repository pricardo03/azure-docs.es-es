---
title: Errores de no disponibilidad de la SKU de Azure| Microsoft Docs
description: Describe cómo solucionar problemas relacionados con el error de SKU no disponible al implementar recursos con Azure Resource Manager.
services: azure-resource-manager
documentationcenter: ''
author: tfitzmac
manager: timlt
editor: ''
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 10/19/2018
ms.author: tomfitz
ms.openlocfilehash: fca028412052a9a1520e1178f5d182a9987a9a85
ms.sourcegitcommit: bb65043d5e49b8af94bba0e96c36796987f5a2be
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/16/2019
ms.locfileid: "72390230"
---
# <a name="resolve-errors-for-sku-not-available"></a>Resolución de los errores de no disponibilidad de la SKU

Este artículo describe cómo resolver el error **SkuNotAvailable**. Si no puede encontrar una SKU adecuada en esa región ni en ninguna región alternativa que satisfaga las necesidades de su negocio, envíe una [solicitud de SKU](https://aka.ms/skurestriction) al soporte técnico de Azure.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="symptom"></a>Síntoma

Al implementar un recurso (normalmente una máquina virtual), puede recibir el siguiente código y mensaje de error:

```
Code: SkuNotAvailable
Message: The requested tier for resource '<resource>' is currently not available in location '<location>' 
for subscription '<subscriptionID>'. Please try another tier or deploy to a different location.
```

## <a name="cause"></a>Causa

Recibe este error si la SKU del recurso que ha seleccionado (como el tamaño de máquina virtual) no está disponible para la ubicación seleccionada.

## <a name="solution-1---powershell"></a>Solución 1: PowerShell

Para determinar qué SKU están disponibles en una región, use el comando [Get-AzComputeResourceSku](/powershell/module/az.compute/get-azcomputeresourcesku). Filtre los resultados por ubicación. Debe tener la versión más reciente de PowerShell para que funcione este comando.

```azurepowershell-interactive
Get-AzComputeResourceSku | where {$_.Locations -icontains "centralus"}
```

Los resultados incluyen una lista de SKU para la ubicación y las restricciones de esas SKU. Tenga en cuenta que una SKU debe aparecer como `NotAvailableForSubscription`.

```powershell
ResourceType          Name        Locations   Restriction                      Capability           Value
------------          ----        ---------   -----------                      ----------           -----
virtualMachines       Standard_A0 centralus   NotAvailableForSubscription      MaxResourceVolumeMB   20480
virtualMachines       Standard_A1 centralus   NotAvailableForSubscription      MaxResourceVolumeMB   71680
virtualMachines       Standard_A2 centralus   NotAvailableForSubscription      MaxResourceVolumeMB  138240
```

## <a name="solution-2---azure-cli"></a>Solución 2: CLI de Azure

Para determinar qué SKU están disponibles en una región, use el comando `az vm list-skus`. Use el parámetro `--location` para filtrar la salida a la ubicación que esté usando. Use el parámetro `--size` para buscar un nombre de tamaño parcial.

```azurecli-interactive
az vm list-skus --location southcentralus --size Standard_F --output table
```

El comando devuelve resultados como:

```azurecli
ResourceType     Locations       Name              Zones    Capabilities    Restrictions
---------------  --------------  ----------------  -------  --------------  --------------
virtualMachines  southcentralus  Standard_F1                ...             None
virtualMachines  southcentralus  Standard_F2                ...             None
virtualMachines  southcentralus  Standard_F4                ...             None
...
```


## <a name="solution-3---azure-portal"></a>Solución 3: Azure Portal

Para determinar qué SKU están disponibles en una región, use el [portal](https://portal.azure.com). Inicie sesión en el portal y agregue un recurso mediante la interfaz. A medida que establezca los valores, verá las SKU disponibles para ese recurso. No es necesario completar la implementación.

Por ejemplo, inicie el proceso de creación de una máquina virtual. Para ver otro tamaño disponible, seleccione **Cambiar tamaño**.

![Creación de una máquina virtual](./media/resource-manager-sku-not-available-errors/create-vm.png)

Puede filtrar y desplazarse por los tamaños disponibles.

![SKU disponibles](./media/resource-manager-sku-not-available-errors/available-sizes.png)

## <a name="solution-4---rest"></a>Solución 4: REST

Para determinar qué SKU están disponibles en una región, use la operación [Resource Skus - List](/rest/api/compute/resourceskus/list).

Se devuelven las SKU y las regiones disponibles en el formato siguiente:

```json
{
  "value": [
    {
      "resourceType": "virtualMachines",
      "name": "Standard_A0",
      "tier": "Standard",
      "size": "A0",
      "locations": [
        "eastus"
      ],
      "restrictions": []
    },
    {
      "resourceType": "virtualMachines",
      "name": "Standard_A1",
      "tier": "Standard",
      "size": "A1",
      "locations": [
        "eastus"
      ],
      "restrictions": []
    },
    ...
  ]
}
```

