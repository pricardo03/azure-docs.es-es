---
title: Directivas de autor para las propiedades de la matriz en los recursos de Azure
description: Aprenda a crear los parámetros de matriz, crear reglas para la matriz de las expresiones de lenguaje, evaluar el alias [*] y para anexar elementos en una matriz existente con las reglas de definición de directiva de Azure.
author: DCtheGeek
ms.author: dacoulte
ms.date: 03/06/2019
ms.topic: conceptual
ms.service: azure-policy
manager: carmonm
ms.openlocfilehash: 38cf6decb8e61768faa9680058f6366e1550ba40
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2019
ms.locfileid: "59274730"
---
# <a name="author-policies-for-array-properties-on-azure-resources"></a>Directivas de autor para las propiedades de la matriz en los recursos de Azure

Propiedades del Administrador de recursos de Azure normalmente se definen como cadenas y valores booleanos. Cuando existe una relación uno a varios, las propiedades complejas en su lugar, se definen como matrices. En la directiva de Azure, las matrices se utilizan de varias maneras diferentes:

- El tipo de un [parámetro definition](../concepts/definition-structure.md#parameters), para proporcionar varias opciones
- Parte de un [regla de directiva](../concepts/definition-structure.md#policy-rule) con las condiciones **en** o **notIn**
- Parte de una regla de directiva que se evalúa como el [ \[ \* \] alias](../concepts/definition-structure.md#understanding-the--alias) para evaluar los escenarios específicos, tales como **ninguno**, **cualquier**, o  **todo**
- En el [anexar efecto](../concepts/effects.md#append) para reemplazar o agregar a una matriz existente

En este artículo se explica cada uso por la directiva de Azure y proporciona varias definiciones de ejemplo.

## <a name="parameter-arrays"></a>Matrices de parámetros

### <a name="define-a-parameter-array"></a>Definir una matriz de parámetros

Define un parámetro como una matriz, permite la flexibilidad de la directiva cuando se necesita más de un valor.
Esta definición de directiva permite a cualquier ubicación única para el parámetro **allowedLocations** y el valor predeterminado es _eastus2_:

```json
"parameters": {
    "allowedLocations": {
        "type": "string",
        "metadata": {
            "description": "The list of allowed locations for resources.",
            "displayName": "Allowed locations",
            "strongType": "location"
        },
        "defaultValue": "eastus2"
    }
}
```

Como **tipo** era _cadena_, solo se puede establecer un valor al asignar la directiva. Si se asigna esta directiva, los recursos en el ámbito solo se permiten en una única región de Azure. La mayoría de las definiciones de directivas deben permitir para obtener una lista de opciones aprobadas, como permitir _eastus2_, _eastus_, y _westus2_.

Para crear la definición de directiva para permitir que varias opciones, utilice el _matriz_ **tipo**. La misma directiva se puede reescribir del siguiente modo:

```json
"parameters": {
    "allowedLocations": {
        "type": "array",
        "metadata": {
            "description": "The list of allowed locations for resources.",
            "displayName": "Allowed locations",
            "strongType": "location"
        },
        "defaultValue": "eastus2",
        "allowedValues": [
            "eastus2",
            "eastus",
            "westus2"
        ]

    }
}
```

> [!NOTE]
> Una vez que se guarda una definición de directiva, el **tipo** no se puede cambiar la propiedad en un parámetro.

Esta nueva definición de parámetro tarda más de un valor durante la asignación de directiva. Con la propiedad de matriz **allowedValues** definido, los valores disponibles durante la asignación son más limitado a la lista predefinida de opciones. El uso de **allowedValues** es opcional.

### <a name="pass-values-to-a-parameter-array-during-assignment"></a>Pasar valores a una matriz de parámetros durante la asignación

Al asignar la directiva a través del portal de Azure, un parámetro de **tipo** _matriz_ se muestra como un único cuadro de texto. La sugerencia dice "usar; para separar los valores. (por ejemplo, Londres; New York) ". Para pasar los valores permitidos de ubicación de _eastus2_, _eastus_, y _westus2_ al parámetro, use la siguiente cadena:

`eastus2;eastus;westus2`

El formato para el valor del parámetro es distinto cuando se usa la API de REST, Azure PowerShell o CLI de Azure. Los valores se pasan a través de una cadena JSON que también incluye el nombre del parámetro.

```json
{
    "allowedLocations": {
        "value": [
            "eastus2",
            "eastus",
            "westus2"
        ]
    }
}
```

Para usar esta cadena con cada SDK, use los siguientes comandos:

- CLI de Azure: Comando [crear la asignación de directivas az](/cli/azure/policy/assignment?view=azure-cli-latest#az-policy-assignment-create) con el parámetro **params**
- Azure PowerShell: Cmdlet [New AzPolicyAssignment](/powershell/module/az.resources/New-Azpolicyassignment) con el parámetro **PolicyParameter**
- API REST: En el _colocar_ [crear](/rest/api/resources/policyassignments/create) operación como parte del cuerpo de solicitud como el valor de la **properties.parameters** propiedad

## <a name="policy-rules-and-arrays"></a>Matrices y las reglas de directiva

### <a name="array-conditions"></a>Condiciones de la matriz

La regla de directiva [condiciones](../concepts/definition-structure.md#conditions) que un _matriz_
**tipo** del parámetro puede usarse con está limitada a `in` y `notIn`. Realizar la siguiente definición de directiva con la condición `equals` como ejemplo:

```json
{
  "policyRule": {
    "if": {
      "not": {
        "field": "location",
        "equals": "[parameters('allowedLocations')]"
      }
    },
    "then": {
      "effect": "audit"
    }
  },
  "parameters": {
    "allowedLocations": {
      "type": "Array",
      "metadata": {
        "description": "The list of allowed locations for resources.",
        "displayName": "Allowed locations",
        "strongType": "location"
      }
    }
  }
}
```

Al intentar crear esta definición de directiva a través de los responsables de portal de Azure a un error como este mensaje de error:

- "No se pudo parametrizar la directiva '{GUID}' debido a errores de validación. Compruebe si los parámetros de la directiva están definidos correctamente. La excepción interna 'evaluación resultado de la expresión de lenguaje '[parameters('allowedLocations')]' es de tipo 'Matriz', tipo esperado es 'String' '."

Esperado **tipo** de condición `equals` es _cadena_. Puesto que **allowedLocations** se define como **tipo** _matriz_, el motor de directiva se evalúa la expresión de lenguaje y genera el error. Con el `in` y `notIn` de condición, el motor de directiva espera el **tipo** _matriz_ en la expresión de lenguaje. Para resolver este mensaje de error, cambie `equals` como `in` o `notIn`.

### <a name="evaluating-the--alias"></a>Evaluar el alias [*]

Los alias que tienen **[\*]** conectado a sus nombres de indicar la **tipo** es un _matriz_. En lugar de evaluar el valor de la matriz completa, **[\*]** hace que sea posible evaluar cada elemento de la matriz. Hay tres escenarios en que esto por evaluación de elementos es útil: Ninguno, cualquiera y todos.

Los desencadenadores de motor de directiva el **efecto** en **, a continuación,** solo cuando el **si** regla se evalúa como true.
Este hecho es importante comprender en contexto de la manera **[\*]** evalúa cada elemento individual de la matriz.

La regla de directiva de ejemplo para la tabla de escenario siguiente:

```json
"policyRule": {
    "if": {
        "allOf": [
            {
                "field": "Microsoft.Storage/storageAccounts/networkAcls.ipRules",
                "exists": "true"
            },
            <-- Condition (see table below) -->
        ]
    },
    "then": {
        "effect": "[parameters('effectType')]"
    }
}
```

El **ipRules** matriz es como sigue para la tabla de escenario siguiente:

```json
"ipRules": [
    {
        "value": "127.0.0.1",
        "action": "Allow"
    },
    {
        "value": "192.168.1.1",
        "action": "Allow"
    }
]
```

Para cada ejemplo de la condición siguiente, reemplace `<field>` con `"field": "Microsoft.Storage/storageAccounts/networkAcls.ipRules[*].value"`.

Los siguientes resultados son el resultado de la combinación de la condición y la regla de directiva de ejemplo y la matriz de valores existentes anteriores:

|Condición |Resultado |Explicación |
|-|-|-|
|`{<field>,"notEquals":"127.0.0.1"}` |Nada |Un elemento de matriz se evalúa como false (127.0.0.1! = 127.0.0.1) y otra como true (127.0.0.1! = 192.168.1.1), por lo que **notEquals** condición es _false_ y no se desencadena el efecto. |
|`{<field>,"notEquals":"10.0.4.1"}` |Efecto de la directiva |Ambos elementos de la matriz se evaluarán como verdadera (10.0.4.1! = 127.0.0.1 y 10.0.4.1! = 192.168.1.1), por lo que **notEquals** condición es _true_ y se desencadena el efecto. |
|`"not":{<field>,"Equals":"127.0.0.1"}` |Efecto de la directiva |Un elemento de matriz se evalúa como true (127.0.0.1 == 127.0.0.1) y otra como false (127.0.0.1 == 192.168.1.1), por lo que **es igual a** condición es _false_. El operador lógico se evalúa como true (**no** _false_), por lo que se desencadena el efecto. |
|`"not":{<field>,"Equals":"10.0.4.1"}` |Efecto de la directiva |Ambos elementos de matriz se evaluarán como false (10.0.4.1 == 127.0.0.1 y 10.0.4.1 == 192.168.1.1), por lo que **es igual a** condición es _false_. El operador lógico se evalúa como true (**no** _false_), por lo que se desencadena el efecto. |
|`"not":{<field>,"notEquals":"127.0.0.1" }` |Efecto de la directiva |Un elemento de matriz se evalúa como false (127.0.0.1! = 127.0.0.1) y otra como true (127.0.0.1! = 192.168.1.1), por lo que **notEquals** condición es _false_. El operador lógico se evalúa como true (**no** _false_), por lo que se desencadena el efecto. |
|`"not":{<field>,"notEquals":"10.0.4.1"}` |Nada |Ambos elementos de la matriz se evaluarán como verdadera (10.0.4.1! = 127.0.0.1 y 10.0.4.1! = 192.168.1.1), por lo que **notEquals** condición es _true_. El operador lógico se evalúa como false (**no** _true_), por lo que no se desencadena el efecto. |
|`{<field>,"Equals":"127.0.0.1"}` |Nada |Un elemento de matriz se evalúa como true (127.0.0.1 == 127.0.0.1) y otra como false (127.0.0.1 == 192.168.1.1), por lo que **es igual a** condición es _false_ y no se desencadena el efecto. |
|`{<field>,"Equals":"10.0.4.1"}` |Nada |Ambos elementos de matriz se evaluarán como false (10.0.4.1 == 127.0.0.1 y 10.0.4.1 == 192.168.1.1), por lo que **es igual a** condición es _false_ y no se desencadena el efecto. |

## <a name="the-append-effect-and-arrays"></a>El efecto de anexar y matrices

El [anexar efecto](../concepts/effects.md#append) se comporta de manera diferente dependiendo de si el **details.field** es un **[\*]** alias o no.

- Cuando no un **[\*]** alias, anexar reemplaza toda la matriz con el **valor** propiedad
- Cuando un **[\*]** anexar alias, agrega el **valor** de matriz de propiedad existente o crea la nueva matriz

Para obtener más información, consulte el [anexar ejemplos](../concepts/effects.md#append-examples).

## <a name="next-steps"></a>Pasos siguientes

- Puede consultar ejemplos en [Ejemplos de Azure Policy](../samples/index.md)
- Consulte la [Estructura de definición de directivas](../concepts/definition-structure.md)
- Consulte [Descripción de los efectos de directivas](../concepts/effects.md)
- Entender cómo se pueden [crear directivas mediante programación](programmatically-create.md)
- Más información sobre cómo [corregir recursos no compatibles](remediate-resources.md)
- En [Organización de los recursos con grupos de administración de Azure](../../management-groups/overview.md), obtendrá información sobre lo que es un grupo de administración.