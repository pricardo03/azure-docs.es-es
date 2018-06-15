---
title: Elemento de interfaz de usuario OptionsGroup de Azure | Microsoft Docs
description: Describe el elemento de la interfaz de usuario Microsoft.Common.OptionsGroup para Azure Portal.
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
ms.date: 03/30/2018
ms.author: tomfitz
ms.openlocfilehash: 2e0b448b5ab48e7be3429d3d3b5b898b6bf22115
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/11/2018
ms.locfileid: "35261855"
---
# <a name="microsoftcommonoptionsgroup-ui-element"></a>Elemento de interfaz de usuario Microsoft.Common.OptionsGroup
Control de selección con una fila de opciones disponibles.

## <a name="ui-sample"></a>Ejemplo de interfaz de usuario
![Microsoft.Common.OptionsGroup](./media/managed-application-elements/microsoft.common.optionsgroup.png)

## <a name="schema"></a>Esquema
```json
{
  "name": "element1",
  "type": "Microsoft.Common.OptionsGroup",
  "label": "Some options group",
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
    ]
  },
  "visible": true
}
```

## <a name="remarks"></a>Comentarios
- La etiqueta de `constraints.allowedValues` es el texto para mostrar de un elemento, y su valor es el valor de salida del elemento cuando se selecciona.
- Si se especifica, el valor predeterminado debe ser una etiqueta presente en `constraints.allowedValues`. Si no se especifica, se selecciona el primer elemento de `constraints.allowedValues` de forma predeterminada. El valor predeterminado es **null**.
- `constraints.allowedValues` debe contener al menos un dígito.
- Este elemento no admite la propiedad `constraints.required`; se debe seleccionar un elemento para que la validación sea correcta.

## <a name="sample-output"></a>Salida de ejemplo
```json
"two"
```

## <a name="next-steps"></a>Pasos siguientes
* Para ver una introducción sobre la creación de definiciones de interfaz de usuario, consulte [Introducción a CreateUiDefinition](create-uidefinition-overview.md).
* Para ver una descripción de las propiedades comunes de los elementos de interfaz de usuario, consulte [Elementos CreateUiDefinition](create-uidefinition-elements.md).
