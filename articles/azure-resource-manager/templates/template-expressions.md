---
title: Sintaxis y expresiones de plantillas
description: En este artículo se describe la sintaxis JSON declarativa de las plantillas de Azure Resource Manager.
ms.topic: conceptual
ms.date: 02/10/2020
ms.openlocfilehash: 42649d4b04b03de32b82335fce68401192de75a3
ms.sourcegitcommit: 7c18afdaf67442eeb537ae3574670541e471463d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/11/2020
ms.locfileid: "77120603"
---
# <a name="syntax-and-expressions-in-azure-resource-manager-templates"></a>Sintaxis y expresiones de las plantillas de Azure Resource Manager

La sintaxis básica de la plantilla es JSON. Sin embargo, puede utilizar expresiones para ampliar los valores JSON disponibles en la plantilla.  Las expresiones empiezan y terminan con corchetes: `[` y `]`, respectivamente. El valor de la expresión se evalúa cuando se implementa la plantilla. Una expresión puede devolver una cadena, un entero, un booleano, una matriz o un objeto.

Las expresiones de plantilla no pueden superar los 24 576 caracteres.

Las expresiones admiten json('null') y las propiedades admiten un valor literal null. En ambos casos, las plantillas de Resource Manager lo tratan como si la propiedad no estuviera presente.

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

En de la expresión, la sintaxis `resourceGroup()` llama a una de las funciones que Resource Manager proporciona para utilizarla dentro de una plantilla. En este caso, se trata de la función [resourceGroup](template-functions-resource.md#resourcegroup). Al igual que en JavaScript, las llamadas de función tienen el formato `functionName(arg1,arg2,arg3)`. La sintaxis `.location` recupera una propiedad del objeto devuelto por esta función.

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

* Para obtener la lista completa de las funciones de plantilla, consulte [Funciones de la plantilla del Administrador de recursos de Azure](template-functions.md).
* Para más información sobre los archivos de plantilla, consulte [Nociones sobre la estructura y la sintaxis de las plantillas de Azure Resource Manager](template-syntax.md).
