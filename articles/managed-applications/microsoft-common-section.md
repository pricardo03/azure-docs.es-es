---
title: Elemento de interfaz de usuario Section de Azure | Microsoft Docs
description: Describe el elemento de la interfaz de usuario Microsoft.Common.Section para Azure Portal.
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
ms.openlocfilehash: 90ffae3dd8b05041c34d766e464eb68f793f6066
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/27/2018
ms.locfileid: "37062985"
---
# <a name="microsoftcommonsection-ui-element"></a>Elemento de interfaz de usuario Microsoft.Common.Section
Control que agrupa uno o varios elementos en un encabezado.

## <a name="ui-sample"></a>Ejemplo de interfaz de usuario
![Microsoft.Common.Section](./media/managed-application-elements/microsoft.common.section.png)

## <a name="schema"></a>Esquema
```json
{
  "name": "section1",
  "type": "Microsoft.Common.Section",
  "label": "Example section",
  "elements": [
    {
      "name": "text1",
      "type": "Microsoft.Common.TextBox",
      "label": "Example text box 1"
    },
    {
      "name": "text2",
      "type": "Microsoft.Common.TextBox",
      "label": "Example text box 2"
    }
  ],
  "visible": true
}
```

## <a name="remarks"></a>Comentarios
- `elements` debe tener al menos un elemento y puede tener todos los tipos de elementos, excepto `Microsoft.Common.Section`.
- Este elemento no admite la propiedad `toolTip`.

## <a name="sample-output"></a>Salida de ejemplo
Para obtener acceso a los valores de salida de los elementos en `elements`, use las funciones [basics()](create-uidefinition-functions.md#basics) o [steps()](create-uidefinition-functions.md#steps) y la notación de puntos:

```json
steps('configuration').section1.text1
```

Los elementos del tipo `Microsoft.Common.Section` no tienen valores de salida por sí mismos.

## <a name="next-steps"></a>Pasos siguientes
* Para ver una introducción sobre la creación de definiciones de interfaz de usuario, consulte [Introducción a CreateUiDefinition](create-uidefinition-overview.md).
* Para ver una descripción de las propiedades comunes de los elementos de interfaz de usuario, consulte [Elementos CreateUiDefinition](create-uidefinition-elements.md).
