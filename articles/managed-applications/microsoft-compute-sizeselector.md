---
title: Elemento de interfaz de usuario SizeSelector de Azure | Microsoft Docs
description: Describe el elemento de la interfaz de usuario Microsoft.Compute.SizeSelector para Azure Portal. Se usa para seleccionar el tamaño de una máquina virtual.
services: managed-applications
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: managed-applications
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/27/2018
ms.author: tomfitz
ms.openlocfilehash: f3a8426856e1345306acff69946beb4860d5f905
ms.sourcegitcommit: 5cfe977783f02cd045023a1645ac42b8d82223bd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/17/2019
ms.locfileid: "74151522"
---
# <a name="microsoftcomputesizeselector-ui-element"></a>Elemento de interfaz de usuario Microsoft.Compute.SizeSelector

Control para seleccionar un tamaño para una o varias instancias de máquina virtual.

## <a name="ui-sample"></a>Ejemplo de interfaz de usuario

El usuario ve un selector con los valores predeterminados de la definición del elemento.

![Microsoft.Compute.SizeSelector](./media/managed-application-elements/microsoft.compute.sizeselector.png)

Después de seleccionar el control, el usuario ve una vista expandida de los tamaños disponibles.

![Microsoft.Compute.SizeSelector expandido](./media/managed-application-elements/microsoft.compute.sizeselector-expanded.png)

## <a name="schema"></a>Schema

```json
{
  "name": "element1",
  "type": "Microsoft.Compute.SizeSelector",
  "label": "Size",
  "toolTip": "",
  "recommendedSizes": [
    "Standard_D1",
    "Standard_D2",
    "Standard_D3"
  ],
  "constraints": {
    "allowedSizes": [],
    "excludedSizes": [],
    "numAvailabilityZonesRequired": 3,
    "zone": "3"
  },
  "options": {
    "hideDiskTypeFilter": false
  },
  "osPlatform": "Windows",
  "imageReference": {
    "publisher": "MicrosoftWindowsServer",
    "offer": "WindowsServer",
    "sku": "2012-R2-Datacenter"
  },
  "count": 2,
  "visible": true
}
```

## <a name="sample-output"></a>Salida de ejemplo

```json
"Standard_D1"
```

## <a name="remarks"></a>Comentarios

- `recommendedSizes` debe tener al menos un tamaño. El primer tamaño recomendado se utiliza como valor predeterminado. La lista de los tamaños disponibles no se ordena por el estado recomendado. El usuario puede seleccionar esa columna para ordenar por estado recomendado.
- Si no hay un tamaño recomendado en la ubicación seleccionada, el tamaño se omite automáticamente. En su lugar, se utiliza el siguiente tamaño recomendado.
- Tanto `constraints.allowedSizes` como `constraints.excludedSizes` son opcionales, pero no se pueden usar simultáneamente. Para determinar la lista de los tamaños disponibles, consulte la [lista de tamaños de máquina virtual disponibles para una suscripción](/rest/api/compute/virtualmachines/virtualmachines-list-sizes-region). Los tipos no especificados en `constraints.allowedSizes` se ocultan, mientras que los tipos no especificados en `constraints.excludedSizes` se muestran.
- `osPlatform` debe especificarse y puede ser **Windows** o **Linux**. Se usa para determinar los costos de hardware de las máquinas virtuales.
- `imageReference` se omite para las imágenes propias, pero se proporciona para las imágenes de terceros. Se usa para determinar los costos de software de las máquinas virtuales.
- `count` se usa para establecer el multiplicador apropiado para el elemento. Admite un valor estático, como **2**, o un valor dinámico de otro elemento, como `[steps('step1').vmCount]`. El valor predeterminado es **1**.
- `numAvailabilityZonesRequired` puede ser 1, 2 o 3.
- El valor predeterminado de `hideDiskTypeFilter` es **false**. El filtro de tipo de disco permite al usuario ver todos los tipos de disco o solo SSD.

## <a name="next-steps"></a>Pasos siguientes

* Para ver una introducción sobre la creación de definiciones de interfaz de usuario, consulte [Introducción a CreateUiDefinition](create-uidefinition-overview.md).
* Para ver una descripción de las propiedades comunes de los elementos de interfaz de usuario, consulte [Elementos CreateUiDefinition](create-uidefinition-elements.md).
