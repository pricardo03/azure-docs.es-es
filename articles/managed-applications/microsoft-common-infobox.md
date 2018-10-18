---
title: Elemento de interfaz de usuario InfoBox de Azure | Microsoft Docs
description: Describe el elemento de la interfaz de usuario Microsoft.Common.TextBlock para Azure Portal.
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
ms.date: 06/15/2018
ms.author: tomfitz
ms.openlocfilehash: abd1329f2ebac90bf846dfd5fc5b307ddb5e52bd
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/25/2018
ms.locfileid: "47095487"
---
# <a name="microsoftcommoninfobox-ui-element"></a>Elemento de interfaz de usuario Microsoft.Common.InfoBox
Un control que agrega un cuadro de información. El cuadro contiene texto importante o advertencias que explican a los usuarios los valores que se están proporcionando. También puede vincular a un URI para ampliar la información.

## <a name="ui-sample"></a>Ejemplo de interfaz de usuario
![Microsoft.Common.InfoBox](./media/managed-application-elements/microsoft.common.infobox.png)


## <a name="schema"></a>Esquema
```json
{
  "name": "text1",
  "type": "Microsoft.Common.InfoBox",
  "visible": true,
  "options": {
    "icon": "None",
    "text": "Nullam eros mi, mollis in sollicitudin non, tincidunt sed enim. Sed et felis metus, rhoncus ornare nibh. Ut at magna leo.",
    "uri": "https://www.microsoft.com"
  }
}
```

## <a name="remarks"></a>Comentarios

* Para `icon`, use **Ninguno**, **Información**, **Advertencia** o **Error**.
* La propiedad `uri` es opcional.

## <a name="sample-output"></a>Salida de ejemplo

```json
"Nullam eros mi, mollis in sollicitudin non, tincidunt sed enim. Sed et felis metus, rhoncus ornare nibh. Ut at magna leo."
```

## <a name="next-steps"></a>Pasos siguientes
* Para ver una introducción sobre la creación de definiciones de interfaz de usuario, consulte [Introducción a CreateUiDefinition](create-uidefinition-overview.md).
* Para ver una descripción de las propiedades comunes de los elementos de interfaz de usuario, consulte [Elementos CreateUiDefinition](create-uidefinition-elements.md).
