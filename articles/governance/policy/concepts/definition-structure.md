---
title: Estructura de definición de Azure Policy
description: Describe cómo Azure Policy usa la definición de directiva de recursos para establecer convenciones para los recursos de su organización al describir cuándo se aplica la directiva y qué efecto tiene.
services: azure-policy
author: DCtheGeek
ms.author: dacoulte
ms.date: 09/18/2018
ms.topic: conceptual
ms.service: azure-policy
manager: carmonm
ms.openlocfilehash: 0ff56b86243956d1fa6b51a6dfd14af9e00d8367
ms.sourcegitcommit: 6e09760197a91be564ad60ffd3d6f48a241e083b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/29/2018
ms.locfileid: "50212784"
---
# <a name="azure-policy-definition-structure"></a>Estructura de definición de Azure Policy

La definición de directiva de recursos que Azure Policy usa le permite establecer convenciones para los recursos de su organización al describir cuándo se aplica la directiva y qué efecto tiene. La definición de convenciones permite controlar los costes y administrar los recursos más fácilmente. Por ejemplo, puede especificar que se permitan solo determinados tipos de máquinas virtuales. O puede obligar a que todos los recursos tengan una etiqueta concreta. Todos los recursos secundarios heredan las directivas. De este modo, si una directiva se aplica a un grupo de recursos, será aplicable a todos los recursos de dicho grupo de recursos.

El esquema utilizado por Azure Policy puede encontrarse aquí: [https://schema.management.azure.com/schemas/2018-05-01/policyDefinition.json](https://schema.management.azure.com/schemas/2018-05-01/policyDefinition.json)

Para crear una definición de directiva se utiliza JSON. La definición de directiva contiene elementos para:

- modo
- parameters
- nombre para mostrar
- description
- regla de directiva
  - evaluación lógica
  - efecto

Por ejemplo, el siguiente JSON muestra una directiva que limita las ubicaciones donde se implementan los recursos:

```json
{
    "properties": {
        "mode": "all",
        "parameters": {
            "allowedLocations": {
                "type": "array",
                "metadata": {
                    "description": "The list of locations that can be specified when deploying resources",
                    "strongType": "location",
                    "displayName": "Allowed locations"
                }
            }
        },
        "displayName": "Allowed locations",
        "description": "This policy enables you to restrict the locations your organization can specify when deploying resources.",
        "policyRule": {
            "if": {
                "not": {
                    "field": "location",
                    "in": "[parameters('allowedLocations')]"
                }
            },
            "then": {
                "effect": "deny"
            }
        }
    }
}
```

Todos los ejemplos de Azure Policy están en [Ejemplos de directivas](../samples/index.md).

## <a name="mode"></a>Mode

El **modo** determina qué tipos de recurso se evaluarán para una directiva. Los modos admitidos son:

- `all`: evalúe los grupos de recursos y todos los tipos de recurso
- `indexed`: evalúe solo los tipos de recurso que admitan las etiquetas y la ubicación

Se recomienda que establezca **mode** en `all` en la mayoría de los casos. Todas las definiciones de directivas creadas a través del portal usan el modo `all`. Si usa PowerShell o la CLI de Azure, puede especificar el parámetro **mode** de forma manual. Si la definición de directiva no contiene un valor **mode**, utiliza `all` en Azure PowerShell y `null` en la CLI de Azure, como valor predeterminado, lo que es equivalente a `indexed`, para compatibilidad con versiones anteriores.

`indexed` debe usarse al crear directivas que apliquen etiquetas o ubicaciones. Esto no es obligatorio, pero impedirá que los recursos que no son compatibles con etiquetas y ubicaciones aparezcan como no conformes en los resultados de cumplimiento. La única excepción a esto es **grupos de recursos**. Las directivas que intentan aplicar la ubicación o etiquetas en un grupo de recursos deben establecer **mode** en `all` y tener como destino específico el tipo `Microsoft.Resources/subscriptions/resourceGroup`. Para obtener un ejemplo, consulte [Aplicar etiqueta y su valor en grupos de recursos](../samples/enforce-tag-rg.md).

## <a name="parameters"></a>Parámetros

Los parámetros ayudan a simplificar la administración de directivas mediante la reducción del número de definiciones de directiva. Piense en los parámetros como si fueran los campos de un formulario: `name`, `address`, `city`, `state`. Estos parámetros no cambian, pero sí sus valores en función del individuo que rellena el formulario.
Los parámetros funcionan del mismo modo al crear las directivas. Con la inclusión de parámetros en una definición de directiva, puede volver a usar esa directiva en distintos escenarios con valores diferentes.

> [!NOTE]
> La definición de parámetros para una directiva o la definición de la iniciativa solo se pueden configurar durante la creación inicial de la directiva o iniciativa. La definición de parámetros no se puede cambiar más adelante.
> Así se evita que las asignaciones existentes de la directiva o la iniciativa realizadas indirectamente no sean válidas.

Por ejemplo, podría definir una directiva de una propiedad de recurso para limitar las ubicaciones en las que se pueden implementar los recursos. En este caso, podría declarar los parámetros siguientes en el momento de crear la directiva:

```json
"parameters": {
    "allowedLocations": {
        "type": "array",
        "metadata": {
            "description": "The list of allowed locations for resources.",
            "displayName": "Allowed locations",
            "strongType": "location"
        }
    }
}
```

El tipo de un parámetro puede ser cadena o matriz. La propiedad de metadatos se usa para herramientas como Azure Portal para mostrar información intuitiva.

Dentro de la propiedad de metadatos, puede usar **strongType** para proporcionar una lista de opciones de selección múltiple en Azure Portal. Los valores permitidos para **strongType** actualmente incluyen:

- `"location"`
- `"resourceTypes"`
- `"storageSkus"`
- `"vmSKUs"`
- `"existingResourceGroups"`
- `"omsWorkspace"`

En la regla de directiva, se hace referencia a los parámetros con la siguiente sintaxis de función con el valor de implementación `parameters`:

```json
{
    "field": "location",
    "in": "[parameters('allowedLocations')]"
}
```

## <a name="definition-location"></a>Ubicación de definición

Al crear una definición de directiva o iniciativa, es importante especificar la ubicación de la definición.

La ubicación de la definición determina el ámbito al que pueden asignarse la definición de directiva o la iniciativa. La ubicación puede especificarse como un grupo de administración o una suscripción.

> [!NOTE]
> Si planea aplicar esta definición de directiva a varias suscripciones, la ubicación debe ser un grupo de administración que contenga las suscripciones a las que asignará la iniciativa o directiva.

## <a name="display-name-and-description"></a>Nombre para mostrar y descripción

Puede usar los valores **displayName** y **description** para identificar la definición de directiva y proporcionar el contexto para su uso.

## <a name="policy-rule"></a>Regla de directiva

La regla de directiva se compone de los bloques **If** y **Then**. En el bloque **If**, defina una o varias condiciones que especifican cuándo se aplica la directiva. Puede aplicar operadores lógicos a estas condiciones para definir con precisión el escenario de una directiva.

En el bloque **Then**, defina el efecto que se produce cuando se cumplen las condiciones de **If**.

```json
{
    "if": {
        <condition> | <logical operator>
    },
    "then": {
        "effect": "deny | audit | append | auditIfNotExists | deployIfNotExists"
    }
}
```

### <a name="logical-operators"></a>Operadores lógicos

Los operadores lógicos admitidos son:

- `"not": {condition  or operator}`
- `"allOf": [{condition or operator},{condition or operator}]`
- `"anyOf": [{condition or operator},{condition or operator}]`

La sintaxis **not** invierte el resultado de la condición. La sintaxis **allOf** (similar a la operación lógica **And**) requiere que se cumplan todas las condiciones. La sintaxis **anyOf** (similar a la operación lógica **Or**) requiere que se cumplan una o varias condiciones.

Puede anidar los operadores lógicos. El ejemplo siguiente muestra una operación **not** que está anidada dentro de una operación **allOf**.

```json
"if": {
    "allOf": [{
            "not": {
                "field": "tags",
                "containsKey": "application"
            }
        },
        {
            "field": "type",
            "equals": "Microsoft.Storage/storageAccounts"
        }
    ]
},
```

### <a name="conditions"></a>Condiciones

Una condición evalúa si un **campo** cumple determinados criterios. Estas son las condiciones que se admiten:

- `"equals": "value"`
- `"notEquals": "value"`
- `"like": "value"`
- `"notLike": "value"`
- `"match": "value"`
- `"notMatch": "value"`
- `"contains": "value"`
- `"notContains": "value"`
- `"in": ["value1","value2"]`
- `"notIn": ["value1","value2"]`
- `"containsKey": "keyName"`
- `"notContainsKey": "keyName"`
- `"exists": "bool"`

Cuando se usan las condiciones **like** y **notLike**, puede incluir un carácter comodín (`*`) en el valor.
El valor no debe contener más de un carácter comodín `*`.

Cuando se usan las condiciones **match** y **notMatch**, proporcione `#` para representar un dígito, `?` para una letra, `.` para que coincidan todos los caracteres y cualquier otro carácter para representar ese carácter en sí. Por ejemplo, consulte [Permitir varios patrones de nombre](../samples/allow-multiple-name-patterns.md).

### <a name="fields"></a>Fields

Para crear condiciones se usan campos. Un campo representa las propiedades de la carga de solicitud de recursos que se usa para describir el estado del recurso.

Se admiten los siguientes campos:

- `name`
- `fullName`
  - Devuelve el nombre completo del recurso. El nombre completo de un recurso es el nombre del recurso precedido por los nombres de recurso principal (por ejemplo "myServer/myDatabase").
- `kind`
- `type`
- `location`
- `tags`
- `tags.<tagName>`
  - Donde **\<tagName\>** es el nombre de la etiqueta para validar la condición.
  - Ejemplo: `tags.CostCenter` donde **CostCenter** es el nombre de la etiqueta.
- `tags[<tagName>]`
  - Esta sintaxis con corchetes admite nombres de etiqueta que contengan puntos.
  - Donde **\<tagName\>** es el nombre de la etiqueta para validar la condición.
  - Ejemplo: `tags[Acct.CostCenter]` donde **Acct.CostCenter** es el nombre de la etiqueta.
- alias de propiedad: para obtener una lista, vea [Alias](#aliases).

### <a name="effect"></a>Efecto

La directiva admite tres tipos de efectos:

- **Deny**: genera un evento en el registro de actividad y genera un error en la solicitud.
- **Audit**: genera un evento de advertencia en el registro de actividad pero no genera un error en la solicitud.
- **Append**: agrega el conjunto de campos definido a la solicitud.
- **AuditIfNotExists**: habilita la auditoría si un recurso no existe.
- **DeployIfNotExists**: implementa un recurso si todavía no existe.

En el caso de **append**, debe proporcionar los detalles tal y como se muestra a continuación:

```json
"effect": "append",
"details": [{
    "field": "field name",
    "value": "value of the field"
}]
```

El valor puede ser una cadena o un objeto con formato JSON.

Con **AuditIfNotExists** y **DeployIfNotExists**, puede evaluar la existencia de un recurso relacionado y aplicar una regla y un efecto correspondiente si no existe ese recurso. Por ejemplo, puede requerir que un monitor de red se implemente para todas las redes virtuales. Para un ejemplo de auditoría cuando no se implementa una extensión de máquina virtual, consulte el artículo sobre la [auditoría si la extensión no existe](../samples/audit-ext-not-exist.md).

Para obtener información detallada sobre cada efecto, el orden de evaluación, las propiedades y algunos ejemplos, consulte [Descripción de los efectos de la directiva](effects.md).

### <a name="policy-functions"></a>Funciones de directiva

Se encuentra disponible un subconjunto de [funciones de plantilla de Resource Manager](../../../azure-resource-manager/resource-group-template-functions.md) para usarlo en una regla de directiva. Actualmente se admiten estas funciones:

- [parameters](../../../azure-resource-manager/resource-group-template-functions-deployment.md#parameters)
- [concat](../../../azure-resource-manager/resource-group-template-functions-array.md#concat)
- [resourceGroup](../../../azure-resource-manager/resource-group-template-functions-resource.md#resourcegroup)
- [suscripción](../../../azure-resource-manager/resource-group-template-functions-resource.md#subscription)

Además, la función `field` está disponible para las reglas de directiva. Esta función se usa principalmente con **AuditIfNotExists** y **DeployIfNotExists** para hacer referencia a los campos del recurso que se va a evaluar. Esto se puede observar en el [ejemplo de DeployIfNotExists](effects.md#deployifnotexists-example).

#### <a name="policy-function-examples"></a>Ejemplos de función de directiva

Este ejemplo de regla de directiva usa la función de recurso `resourceGroup` para obtener la propiedad **nombre**, combinada con la matriz `concat` y la función de objeto para compilar una condición `like` que exige que el nombre del recurso empiece con el nombre del grupo de recursos.

```json
{
    "if": {
        "not": {
            "field": "name",
            "like": "[concat(resourceGroup().name,'*')]"
        }
    },
    "then": {
        "effect": "deny"
    }
}
```

Este ejemplo de regla de directiva usa la función de recurso `resourceGroup` para obtener el valor de la matriz de la propiedad **etiquetas** de la etiqueta **CostCenter** del grupo de recursos y adjuntarlo a la etiqueta **CostCenter** del nuevo recurso.

```json
{
    "if": {
        "field": "tags.CostCenter",
        "exists": "false"
    },
    "then": {
        "effect": "append",
        "details": [{
            "field": "tags.CostCenter",
            "value": "[resourceGroup().tags.CostCenter]"
        }]
    }
}
```

## <a name="aliases"></a>Alias

Los alias de propiedad se usan para tener acceso a propiedades específicas de un tipo de recurso. Los alias le permiten restringir los valores o condiciones que se permiten para una propiedad en un recurso. Cada alias se asigna a las rutas de acceso en las diferentes versiones de la API para un tipo de recurso determinado. Durante la evaluación de directivas, el motor de directiva obtiene la ruta de acceso de propiedad para esa versión de la API.

La lista de alias siempre está en aumento. Para descubrir qué alias son compatibles actualmente con Azure Policy, use uno de los métodos siguientes:

- Azure PowerShell

  ```azurepowershell-interactive
  # Login first with Connect-AzureRmAccount if not using Cloud Shell

  # Use Get-AzureRmPolicyAlias to list available providers
  Get-AzureRmPolicyAlias -ListAvailable

  # Use Get-AzureRmPolicyAlias to list aliases for a Namespace (such as Azure Automation -- Microsoft.Automation)
  Get-AzureRmPolicyAlias -NamespaceMatch 'automation'
  ```

- Azure CLI

  ```azurecli-interactive
  # Login first with az login if not using Cloud Shell

  # List namespaces
  az provider list --query [*].namespace

  # Get Azure Policy aliases for a specific Namespace (such as Azure Automation -- Microsoft.Automation)
  az provider show --namespace Microsoft.Automation --expand "resourceTypes/aliases" --query "resourceTypes[].aliases[].name"
  ```

- API de REST/ARMClient

  ```http
  GET https://management.azure.com/providers/?api-version=2017-08-01&$expand=resourceTypes/aliases
  ```

### <a name="understanding-the--alias"></a>Descripción del alias [*]

Algunos de los alias disponibles tienen una versión que aparece como un nombre "normal" y otra que tiene agregado **[\*]**. Por ejemplo: 

- `Microsoft.Storage/storageAccounts/networkAcls.ipRules`
- `Microsoft.Storage/storageAccounts/networkAcls.ipRules[*]`

El primer ejemplo se utiliza para evaluar toda la matriz, donde el alias **[\*]** evalúa cada uno de sus elementos.

Echemos un vistazo a una regla de directiva como ejemplo. Esta directiva **denegará** una cuenta de almacenamiento que tenga ipRules configuradas, si **ninguna** de las ipRules tiene el valor "127.0.0.1".

```json
"policyRule": {
    "if": {
        "allOf": [{
                "field": "Microsoft.Storage/storageAccounts/networkAcls.ipRules",
                "exists": "true"
            },
            {
                "field": "Microsoft.Storage/storageAccounts/networkAcls.ipRules[*].value",
                "notEquals": "127.0.0.1"
            }
        ]
    },
    "then": {
        "effect": "deny",
    }
}
```

La matriz **ipRules** es como sigue para el ejemplo:

```json
"ipRules": [{
        "value": "127.0.0.1",
        "action": "Allow"
    },
    {
        "value": "192.168.1.1",
        "action": "Allow"
    }
]
```

Le mostramos cómo se procesa este ejemplo:

- `networkAcls.ipRules`: se comprueba que la matriz es distinta de null. Se evalúa como true, de forma que la evaluación sigue.

  ```json
  {
    "field": "Microsoft.Storage/storageAccounts/networkAcls.ipRules",
    "exists": "true"
  }
  ```

- `networkAcls.ipRules[*].value`: comprueba cada propiedad _value_ en la matriz **ipRules**.

  ```json
  {
    "field": "Microsoft.Storage/storageAccounts/networkAcls.ipRules[*].value",
    "notEquals": "127.0.0.1"
  }
  ```

  - Como matriz, se procesará cada elemento.

    - "127.0.0.1" != "127.0.0.1" se evalúa como falso.
    - "127.0.0.1" != "192.168.1.1" se evalúa como true.
    - Al menos una propiedad _value_ de la matriz **ipRules** se evalúa como false, por lo que la evaluación se detendrá.

Como una condición se evaluó como false, el efecto de la **denegación** no se desencadena.

## <a name="initiatives"></a>Iniciativas

Las iniciativas le permiten agrupan varias definiciones de directivas relacionadas para simplificar las asignaciones y la administración, porque se trabaja con un grupo como un elemento único. Por ejemplo, puede agrupar todas las definiciones de directivas de etiquetado relacionadas en una sola iniciativa. En lugar de asignar individualmente cada directiva, la aplica.

En el ejemplo siguiente se muestra cómo crear una iniciativa para controlar dos etiquetas: `costCenter` y `productName`. Usa dos directivas integradas para aplicar el valor de etiqueta predeterminado.

```json
{
    "properties": {
        "displayName": "Billing Tags Policy",
        "policyType": "Custom",
        "description": "Specify cost Center tag and product name tag",
        "parameters": {
            "costCenterValue": {
                "type": "String",
                "metadata": {
                    "description": "required value for Cost Center tag"
                }
            },
            "productNameValue": {
                "type": "String",
                "metadata": {
                    "description": "required value for product Name tag"
                }
            }
        },
        "policyDefinitions": [{
                "policyDefinitionId": "/providers/Microsoft.Authorization/policyDefinitions/1e30110a-5ceb-460c-a204-c1c3969c6d62",
                "parameters": {
                    "tagName": {
                        "value": "costCenter"
                    },
                    "tagValue": {
                        "value": "[parameters('costCenterValue')]"
                    }
                }
            },
            {
                "policyDefinitionId": "/providers/Microsoft.Authorization/policyDefinitions/2a0e14a6-b0a6-4fab-991a-187a4f81c498",
                "parameters": {
                    "tagName": {
                        "value": "costCenter"
                    },
                    "tagValue": {
                        "value": "[parameters('costCenterValue')]"
                    }
                }
            },
            {
                "policyDefinitionId": "/providers/Microsoft.Authorization/policyDefinitions/1e30110a-5ceb-460c-a204-c1c3969c6d62",
                "parameters": {
                    "tagName": {
                        "value": "productName"
                    },
                    "tagValue": {
                        "value": "[parameters('productNameValue')]"
                    }
                }
            },
            {
                "policyDefinitionId": "/providers/Microsoft.Authorization/policyDefinitions/2a0e14a6-b0a6-4fab-991a-187a4f81c498",
                "parameters": {
                    "tagName": {
                        "value": "productName"
                    },
                    "tagValue": {
                        "value": "[parameters('productNameValue')]"
                    }
                }
            }
        ]
    },
    "id": "/subscriptions/<subscription-id>/providers/Microsoft.Authorization/policySetDefinitions/billingTagsPolicy",
    "type": "Microsoft.Authorization/policySetDefinitions",
    "name": "billingTagsPolicy"
}
```

## <a name="next-steps"></a>Pasos siguientes

- Puede consultar ejemplos en [Ejemplos de Azure Policy](../samples/index.md)
- Consulte [Descripción de los efectos de directivas](effects.md)
- Entender cómo se pueden [crear directivas mediante programación](../how-to/programmatically-create.md)
- Obtenga información sobre cómo [obtener datos de cumplimiento](../how-to/getting-compliance-data.md)
- Descubrir cómo se pueden [corregir recursos no compatibles](../how-to/remediate-resources.md)
- En [Organización de los recursos con grupos de administración de Azure](../../management-groups/overview.md), obtendrá información sobre lo que es un grupo de administración.
