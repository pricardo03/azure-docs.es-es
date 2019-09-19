---
title: Funciones definidas por el usuario de la plantilla de Azure Resource Manager
description: Se describe cómo definir y usar funciones definidas por el usuario en una plantilla de Azure Resource Manager.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 09/05/2019
ms.author: tomfitz
ms.openlocfilehash: 227ecb57b125264e7d0eba56e634966d677e0f58
ms.sourcegitcommit: 88ae4396fec7ea56011f896a7c7c79af867c90a1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/06/2019
ms.locfileid: "70387282"
---
# <a name="user-defined-functions-in-azure-resource-manager-template"></a>Funciones definidas por el usuario de la plantilla de Azure Resource Manager

Dentro de la plantilla, puede crear sus propias funciones. Estas funciones están disponibles para su uso en la plantilla. Las funciones definidas por el usuario son independientes de las [funciones de plantilla estándares](resource-group-template-functions.md) que están disponibles automáticamente dentro de la plantilla. Cree sus propias funciones cuando tenga expresiones complicadas que se usen repetidamente en la plantilla.

En este artículo se describe cómo agregar funciones definidas por el usuario a una plantilla de Azure Resource Manager.

## <a name="define-the-function"></a>Definición de la función

Las funciones requieren un valor de espacio de nombres para evitar conflictos de nomenclatura con las funciones de plantilla. En el ejemplo siguiente se muestra una función que devuelve un nombre de cuenta de almacenamiento:

```json
"functions": [
  {
    "namespace": "contoso",
    "members": {
      "uniqueName": {
        "parameters": [
          {
            "name": "namePrefix",
            "type": "string"
          }
        ],
        "output": {
          "type": "string",
          "value": "[concat(toLower(parameters('namePrefix')), uniqueString(resourceGroup().id))]"
        }
      }
    }
  }
],
```

## <a name="use-the-function"></a>Uso de la función

En el ejemplo siguiente se muestra cómo llamar a su función.

```json
"resources": [
  {
    "name": "[contoso.uniqueName(parameters('storageNamePrefix'))]",
    "type": "Microsoft.Storage/storageAccounts",
    "apiVersion": "2016-01-01",
    "sku": {
      "name": "Standard_LRS"
    },
    "kind": "Storage",
    "location": "South Central US",
    "tags": {},
    "properties": {}
  }
]
```

## <a name="limitations"></a>Limitaciones

Al definir una función de usuario, hay algunas restricciones:

* La función no puede acceder a las variables.
* La función solo puede usar los parámetros que se definen en la función. Cuando usa la función [parameters](resource-group-template-functions-deployment.md#parameters) dentro de una función definida por el usuario, los parámetros de esa función serán los que limiten sus acciones.
* La función no puede llamar a otras funciones definidas por el usuario.
* La función no puede usar la función [reference](resource-group-template-functions-resource.md#reference) ni ninguna de las funciones [list](resource-group-template-functions-resource.md#list).
* Los parámetros de la función no pueden tener valores predeterminados.


## <a name="next-steps"></a>Pasos siguientes

* Para más información sobre las propiedades disponibles para funciones definidas por el usuario, consulte [Información sobre la estructura y la sintaxis de las plantillas de Azure Resource Manager](resource-group-authoring-templates.md).
* Para obtener una lista de las funciones de plantilla disponibles, consulte [Funciones de la plantilla de Azure Resource Manager](resource-group-template-functions.md).
