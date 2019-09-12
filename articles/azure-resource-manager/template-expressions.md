---
title: Sintaxis y expresiones de las plantillas de Azure Resource Manager
description: En este artículo se describe la sintaxis JSON declarativa de las plantillas de Azure Resource Manager.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 09/03/2019
ms.author: tomfitz
ms.openlocfilehash: 1e7288da19e2e81d609b952e03d5143b03a65c63
ms.sourcegitcommit: 267a9f62af9795698e1958a038feb7ff79e77909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/04/2019
ms.locfileid: "70259436"
---
# <a name="syntax-and-expressions-in-azure-resource-manager-templates"></a>Sintaxis y expresiones de las plantillas de Azure Resource Manager

La sintaxis básica de la plantilla es JSON. Sin embargo, puede utilizar expresiones para ampliar los valores JSON disponibles en la plantilla.  Las expresiones empiezan y terminan con corchetes: `[` y `]`, respectivamente. El valor de la expresión se evalúa cuando se implementa la plantilla. Una expresión puede devolver una cadena, un entero, un booleano, una matriz o un objeto.

Las expresiones de plantilla no pueden superar los 24 576 caracteres.

## <a name="use-functions"></a>Uso de las funciones

En el ejemplo siguiente se muestra una expresión en el valor predeterminado de un parámetro:

```json
"parameters": {
  "location": {
    "type": "string",
    "defaultValue": "[resourceGroup().location]"
  }
},
```

En de la expresión, la sintaxis `resourceGroup()` llama a una de las funciones que Resource Manager proporciona para utilizarla dentro de una plantilla. En este caso, se trata de la función [resourceGroup](resource-group-template-functions-resource.md#resourcegroup). Al igual que en JavaScript, las llamadas de función tienen el formato `functionName(arg1,arg2,arg3)`. La sintaxis `.location` recupera una propiedad del objeto devuelto por esta función.

Las funciones de plantilla y sus parámetros no distinguen mayúsculas de minúsculas. Por ejemplo, Resource Manager resuelve **variables('var1')** y **VARIABLES('VAR1')** de la misma manera. Cuando se evalúa, a menos que la función modifique expresamente las mayúsculas (como toUpper o toLower), la función conserva las mayúsculas. Es posible que determinados tipos de recursos tengan requisitos de mayúsculas independientemente de cómo se evalúen las funciones.

Para pasar un valor de cadena como parámetro a una función, use comillas simples.

```json
"name": "[concat('storage', uniqueString(resourceGroup().id))]"
```

## <a name="escape-characters"></a>Carácter de escape

Para que una cadena literal empiece con un corchete izquierdo `[` y termine con un corchete derecho `]`, pero no se interprete como una expresión, agregue otro corchete para que la cadena comience con `[[`. Por ejemplo, la variable:

```json
"demoVar1": "[[test value]"
```

Se resuelve como `[test value]`.

Sin embargo, si la cadena literal no termina con un corchete de cierre, no utilice un carácter de escape en el primer corchete. Por ejemplo, la variable:

```json
"demoVar2": "[test] value"
```

Se resuelve como `[test] value`.

Para utilizar un carácter de escape en las comillas dobles en una expresión, como, por ejemplo, al agregar un objeto JSON a la plantilla, use la barra diagonal inversa.

```json
"tags": {
    "CostCenter": "{\"Dept\":\"Finance\",\"Environment\":\"Production\"}"
},
```

## <a name="next-steps"></a>Pasos siguientes

* Para obtener la lista completa de las funciones de plantilla, consulte [Funciones de la plantilla del Administrador de recursos de Azure](resource-group-template-functions.md).
* Para más información sobre los archivos de plantilla, consulte [Nociones sobre la estructura y la sintaxis de las plantillas de Azure Resource Manager](resource-group-authoring-templates.md).
