---
title: Elemento de la interfaz de usuario de DropDown de Azure | Microsoft Docs
description: Describe el elemento de la interfaz de usuario Microsoft.Common.DropDown para Azure Portal. Se usa para seleccionar entre las opciones disponibles al implementar una aplicación administrada.
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
ms.openlocfilehash: e1db91362d17d9100199f0b30119f8024f00a903
ms.sourcegitcommit: 5cfe977783f02cd045023a1645ac42b8d82223bd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/17/2019
ms.locfileid: "74151345"
---
# <a name="microsoftcommondropdown-ui-element"></a>Elemento de interfaz de usuario Microsoft.Common.DropDown

Control de selección con una lista desplegable.

## <a name="ui-sample"></a>Ejemplo de interfaz de usuario

![Microsoft.Common.DropDown](./media/managed-application-elements/microsoft.common.dropdown.png)

## <a name="schema"></a>Schema

```json
{
  "name": "element1",
  "type": "Microsoft.Common.DropDown",
  "label": "Example drop down",
  "defaultValue": "Value two",
  "toolTip": "",
  "constraints": {
    "allowedValues": [
      {
        "label": "Value one",
        "value": "one"
      },
      {
        "label": "Value two",
        "value": "two"
      }
    ],
    "required": true
  },
  "visible": true
}
```

## <a name="sample-output"></a>Salida de ejemplo

```json
"two"
```

## <a name="remarks"></a>Comentarios

- La etiqueta de `constraints.allowedValues` es el texto para mostrar de un elemento, y su valor es el valor de salida del elemento cuando se selecciona.
- Si se especifica, el valor predeterminado debe ser una etiqueta presente en `constraints.allowedValues`. Si no se especifica, se selecciona el primer elemento de `constraints.allowedValues`. El valor predeterminado es **null**.
- `constraints.allowedValues` debe tener al menos un elemento.
- Para emular un valor que no es obligatorio, agregue un elemento con una etiqueta y el valor `""` (cadena vacía) a `constraints.allowedValues`.

## <a name="next-steps"></a>Pasos siguientes

* Para ver una introducción sobre la creación de definiciones de interfaz de usuario, consulte [Introducción a CreateUiDefinition](create-uidefinition-overview.md).
* Para ver una descripción de las propiedades comunes de los elementos de interfaz de usuario, consulte [Elementos CreateUiDefinition](create-uidefinition-elements.md).
