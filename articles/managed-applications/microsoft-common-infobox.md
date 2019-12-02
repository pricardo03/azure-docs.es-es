---
title: Elemento de interfaz de usuario InfoBox de Azure | Microsoft Docs
description: Describe el elemento de la interfaz de usuario Microsoft.Common.InfoB para Azure Portal. Se usa para agregar texto o advertencias al implementar una aplicación administrada.
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
ms.openlocfilehash: 3a72aaaa15b55b2f0fbc0a227c36a4b2f624d43b
ms.sourcegitcommit: 5cfe977783f02cd045023a1645ac42b8d82223bd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/17/2019
ms.locfileid: "74151325"
---
# <a name="microsoftcommoninfobox-ui-element"></a>Elemento de interfaz de usuario Microsoft.Common.InfoBox

Un control que agrega un cuadro de información. El cuadro contiene texto importante o advertencias que explican a los usuarios los valores que se están proporcionando. También puede vincular a un URI para ampliar la información.

## <a name="ui-sample"></a>Ejemplo de interfaz de usuario

![Microsoft.Common.InfoBox](./media/managed-application-elements/microsoft.common.infobox.png)


## <a name="schema"></a>Schema

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

## <a name="sample-output"></a>Salida de ejemplo

```json
"Nullam eros mi, mollis in sollicitudin non, tincidunt sed enim. Sed et felis metus, rhoncus ornare nibh. Ut at magna leo."
```

## <a name="remarks"></a>Comentarios

* Para `icon`, use **Ninguno**, **Información**, **Advertencia** o **Error**.
* La propiedad `uri` es opcional.

## <a name="next-steps"></a>Pasos siguientes

* Para ver una introducción sobre la creación de definiciones de interfaz de usuario, consulte [Introducción a CreateUiDefinition](create-uidefinition-overview.md).
* Para ver una descripción de las propiedades comunes de los elementos de interfaz de usuario, consulte [Elementos CreateUiDefinition](create-uidefinition-elements.md).
