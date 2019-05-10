---
title: Funciones de plano técnico de Azure
description: Describe las funciones para su uso con Azure planos definiciones y asignaciones.
author: DCtheGeek
ms.author: dacoulte
ms.date: 04/15/2019
ms.topic: reference
ms.service: blueprints
manager: carmonm
ms.openlocfilehash: dc72113a8f5ed978d64d35c43e94dc9e19e4cdb1
ms.sourcegitcommit: 0568c7aefd67185fd8e1400aed84c5af4f1597f9
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65209403"
---
# <a name="functions-for-use-with-azure-blueprints"></a>Funciones para su uso con proyectos de Azure

Planos técnicos de Azure proporciona funciones de creación de una definición del plano técnico más dinámicas. Estas funciones son para su uso con la definición del plano técnico y los artefactos del plano. Un artefacto de la plantilla de Resource Manager admite el uso completo de funciones de Resource Manager además de obtener un valor dinámico a través de un parámetro de plano técnico.

Se admiten las siguientes funciones:

- [artifacts](#artifacts)
- [concat](#concat)
- [parameters](#parameters)
- [resourceGroup](#resourcegroup)
- [resourceGroups](#resourcegroups)
- [suscripción](#subscription)

## <a name="artifacts"></a>Artefactos

`artifacts(artifactName)`

Devuelve un objeto de propiedades que rellena con ese artefacto de plano técnico salidas.

### <a name="parameters"></a>Parámetros

| Parámetro | Obligatorio | Type | DESCRIPCIÓN |
|:--- |:--- |:--- |:--- |
| artifactName |Sí |string |El nombre de un artefacto de plano técnico. |

### <a name="return-value"></a>Valor devuelto

Un objeto de propiedades de salida. El **genera** propiedades dependen del tipo de artefacto de plano técnico que se hace referencia. Todos los tipos tienen el formato:

```json
{
  "outputs": {collectionOfOutputProperties}
}
```

#### <a name="policy-assignment-artifact"></a>Artefacto de asignación de directiva

```json
{
    "outputs": {
        "policyAssignmentId": "{resourceId-of-policy-assignment}",
        "policyAssignmentName": "{name-of-policy-assignment}",
        "policyDefinitionId": "{resourceId-of-policy-definition}",
    }
}
```

#### <a name="resource-manager-template-artifact"></a>Artefacto de la plantilla de Resource Manager

El **genera** las propiedades del objeto devuelto se definen dentro de la plantilla de Resource Manager y devuelto por la implementación.

#### <a name="role-assignment-artifact"></a>Artefacto de asignación de rol

```json
{
    "outputs": {
        "roleAssignmentId": "{resourceId-of-role-assignment}",
        "roleDefinitionId": "{resourceId-of-role-definition}",
        "principalId": "{principalId-role-is-being-assigned-to}",
    }
}
```

### <a name="example"></a>Ejemplo

Un artefacto de la plantilla de Resource Manager con el Id. de _myTemplateArtifact_ propiedad de salida que contiene el ejemplo siguiente:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    ...
    "outputs": {
        "myArray": {
            "type": "array",
            "value": ["first", "second"]
        },
        "myString": {
            "type": "string",
            "value": "my string value"
        },
        "myObject": {
            "type": "object",
            "value": {
                "myProperty": "my value",
                "anotherProperty": true
            }
        }
    }
}
```

Algunos ejemplos de recuperación de datos desde el _myTemplateArtifact_ ejemplo son:

| Expresión | Type | Valor |
|:---|:---|:---|
|`[artifacts("myTemplateArtifact").outputs.myArray]` | Matriz | \["first", "second"\] |
|`[artifacts("myTemplateArtifact").outputs.myArray[0]]` | string | "primero" |
|`[artifacts("myTemplateArtifact").outputs.myString]` | string | "Mi valor de cadena" |
|`[artifacts("myTemplateArtifact").outputs.myObject]` | Object | { "myproperty": "my value", "anotherProperty": true } |
|`[artifacts("myTemplateArtifact").outputs.myObject.myProperty]` | string | "Mi value" |
|`[artifacts("myTemplateArtifact").outputs.myObject.anotherProperty]` | Bool | True  |

## <a name="concat"></a>concat

`concat(string1, string2, string3, ...)`

Combina varios valores de cadena y devuelve la cadena concatenada.

### <a name="parameters"></a>Parámetros

| Parámetro | Obligatorio | Type | DESCRIPCIÓN |
|:--- |:--- |:--- |:--- |
| string1 |Sí |string |El primer valor para la concatenación. |
| argumentos adicionales |No |string |Valores adicionales en orden secuencial para la concatenación |

### <a name="return-value"></a>Valor devuelto

Una cadena de valores concatenados.

### <a name="remarks"></a>Comentarios

La función de Azure Blueprint difiere de la función de plantilla de Azure Resource Manager que sólo funciona con cadenas.

### <a name="example"></a>Ejemplo

`concat(parameters('organizationName'), '-vm')`

## <a name="parameters"></a>parameters

`parameters(parameterName)`

Devuelve un valor de parámetro de plano técnico. El nombre del parámetro especificado debe definirse en la definición del plano técnico o en artefactos de plano técnico.

### <a name="parameters"></a>Parámetros

| Parámetro | Obligatorio | Type | DESCRIPCIÓN |
|:--- |:--- |:--- |:--- |
| parameterName |Sí |string |El nombre del parámetro que se va a devolver. |

### <a name="return-value"></a>Valor devuelto

El valor del parámetro del artefacto de plano técnico o plano especificado.

### <a name="remarks"></a>Comentarios

La función de Azure Blueprint difiere de la función de plantilla de Azure Resource Manager que sólo funciona con los parámetros de plano técnico.

### <a name="example"></a>Ejemplo

Definir parámetro _principalIds_ en la definición del plano técnico:

```json
{
    "type": "Microsoft.Blueprint/blueprints",
    "properties": {
        ...
        "parameters": {
            "principalIds": {
                "type": "array",
                "metadata": {
                    "displayName": "Principal IDs",
                    "description": "This is a blueprint parameter that any artifact can reference. We'll display these descriptions for you in the info bubble. Supply principal IDs for the users,groups, or service principals for the RBAC assignment.",
                    "strongType": "PrincipalId"
                }
            }
        },
        ...
    }
}
```

A continuación, usar _principalIds_ como argumento para `parameters()` en un artefacto de plano técnico:

```json
{
    "type": "Microsoft.Blueprint/blueprints/artifacts",
    "kind": "roleAssignment",
    ...
    "properties": {
        "roleDefinitionId": "/providers/Microsoft.Authorization/roleDefinitions/8e3af657-a8ff-443c-a75c-2fe8c4bcb635",
        "principalIds": "[parameters('principalIds')]",
        ...
    }
}
```

## <a name="resourcegroup"></a>resourceGroup

`resourceGroup()`

Devuelve un objeto que representa el grupo de recursos actual.

### <a name="return-value"></a>Valor devuelto

El objeto devuelto está en el formato siguiente:

```json
{
  "name": "{resourceGroupName}",
  "location": "{resourceGroupLocation}",
}
```

### <a name="remarks"></a>Comentarios

La función de Azure Blueprint difiere de la función de plantilla de Azure Resource Manager. El `resourceGroup()` función no se puede usar en un artefacto de nivel de suscripción o en la definición del plano técnico. Solo puede usarse en artefactos de plano técnico que forman parte de un artefacto del grupo de recursos.

Un uso común de la `resourceGroup()` función consiste en crear recursos en la misma ubicación que el artefacto de grupo de recursos.

### <a name="example"></a>Ejemplo

Para usar la ubicación del grupo de recursos, establecido en la definición del plano técnico o durante la asignación, como la ubicación de otro artefacto, declare un objeto de marcador de posición de grupo de recursos en la definición del plano técnico. En este ejemplo, _NetworkingPlaceholder_ es el nombre del marcador de posición de grupo de recursos.

```json
{
    "type": "Microsoft.Blueprint/blueprints",
    "properties": {
        ...
        "resourceGroups": {
            "NetworkingPlaceholder": {
                "location": "eastus"
            }
        }
    }
}
```

A continuación, utilice el `resourceGroup()` función en el contexto de un artefacto de plano técnico está destinado a un objeto de marcador de posición de grupo de recursos. En este ejemplo, el artefacto de la plantilla se implementa en el _NetworkingPlaceholder_ grupo de recursos y proporciona el parámetro _resourceLocation_ completa dinámicamente con el  _NetworkingPlaceholder_ ubicación del grupo de recursos a la plantilla. La ubicación de la _NetworkingPlaceholder_ podría haberse definido estáticamente en la definición del plano técnico o definido dinámicamente durante la asignación de grupo de recursos. En cualquier caso, el artefacto de la plantilla se proporciona esa información como un parámetro y lo usa para implementar los recursos en la ubicación correcta.

```json
{
  "type": "Microsoft.Blueprint/blueprints/artifacts",
  "kind": "template",
  "properties": {
      "template": {
        ...
      },
      "resourceGroup": "NetworkingPlaceholder",
      ...
      "parameters": {
        "resourceLocation": {
          "value": "[resourceGroup().location]"
        }
      }
  }
}
```

## <a name="resourcegroups"></a>resourceGroups

`resourceGroups(placeholderName)`

Devuelve un objeto que representa el artefacto de grupo de recursos especificado. A diferencia de `resourceGroup()`, lo que requiere el contexto del artefacto, esta función se utiliza para obtener las propiedades de un marcador de posición de grupo de recursos específico cuando no esté en el contexto de ese grupo de recursos.

### <a name="parameters"></a>Parámetros

| Parámetro | Obligatorio | Type | DESCRIPCIÓN |
|:--- |:--- |:--- |:--- |
| placeholderName |Sí |string |El nombre de marcador de posición del artefacto de grupo de recursos para devolver. |

### <a name="return-value"></a>Valor devuelto

El objeto devuelto está en el formato siguiente:

```json
{
  "name": "{resourceGroupName}",
  "location": "{resourceGroupLocation}",
}
```

### <a name="example"></a>Ejemplo

Para usar la ubicación del grupo de recursos, establecido en la definición del plano técnico o durante la asignación, como la ubicación de otro artefacto, declare un objeto de marcador de posición de grupo de recursos en la definición del plano técnico. En este ejemplo, _NetworkingPlaceholder_ es el nombre del marcador de posición de grupo de recursos.

```json
{
    "type": "Microsoft.Blueprint/blueprints",
    "properties": {
        ...
        "resourceGroups": {
            "NetworkingPlaceholder": {
                "location": "eastus"
            }
        }
    }
}
```

A continuación, utilice el `resourceGroups()` función desde el contexto de cualquier artefacto de plano técnico para obtener una referencia al objeto de marcador de posición del grupo de recursos. En este ejemplo, el artefacto de la plantilla se implementa fuera del _NetworkingPlaceholder_ grupo de recursos y proporciona el parámetro _artifactLocation_ completa dinámicamente con el  _NetworkingPlaceholder_ ubicación del grupo de recursos a la plantilla. La ubicación de la _NetworkingPlaceholder_ podría haberse definido estáticamente en la definición del plano técnico o definido dinámicamente durante la asignación de grupo de recursos. En cualquier caso, el artefacto de la plantilla se proporciona esa información como un parámetro y lo usa para implementar los recursos en la ubicación correcta.

```json
{
  "kind": "template",
  "properties": {
      "template": {
          ...
      },
      ...
      "parameters": {
        "artifactLocation": {
          "value": "[resourceGroups('NetworkingPlaceholder').location]"
        }
      }
  },
  "type": "Microsoft.Blueprint/blueprints/artifacts",
  "name": "myTemplate"
}
```

## <a name="subscription"></a>suscripción

`subscription()`

Devuelve detalles acerca de la suscripción para la asignación del plano técnico actual.

### <a name="return-value"></a>Valor devuelto

El objeto devuelto está en el formato siguiente:

```json
{
    "id": "/subscriptions/{subscriptionId}",
    "subscriptionId": "{subscriptionId}",
    "tenantId": "{tenantId}",
    "displayName": "{name-of-subscription}"
}
```

### <a name="example"></a>Ejemplo

Utilice el nombre para mostrar de la suscripción y la `concat()` función para crear una convención de nomenclatura que se pasa como parámetro _resourceName_ del artefacto de la plantilla.

```json
{
  "kind": "template",
  "properties": {
      "template": {
          ...
      },
      ...
      "parameters": {
        "resourceName": {
          "value": "[concat(subscription().displayName, '-vm')]"
        }
      }
  },
  "type": "Microsoft.Blueprint/blueprints/artifacts",
  "name": "myTemplate"
}
```

## <a name="next-steps"></a>Pasos siguientes

- Más información sobre el [ciclo de vida del plano técnico](../concepts/lifecycle.md)
- Descubra cómo utilizar [parámetros estáticos y dinámicos](../concepts/parameters.md).
- Aprenda a personalizar el [orden de secuenciación de planos técnicos](../concepts/sequencing-order.md).
- Averigüe cómo usar el [bloqueo de recursos de planos técnicos](../concepts/resource-locking.md).
- Aprenda a [actualizar las asignaciones existentes](../how-to/update-existing-assignments.md).
- Puede consultar la información de [solución de problemas generales](../troubleshoot/general.md) para resolver los problemas durante la asignación de un plano técnico.