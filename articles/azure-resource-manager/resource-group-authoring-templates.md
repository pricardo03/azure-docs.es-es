---
title: Estructura y sintaxis de las plantillas de Azure Resource Manager | Microsoft Docs
description: Describe la estructura y las propiedades de plantillas de Azure Resource Manager mediante la sintaxis declarativa de JSON.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
ms.assetid: 19694cb4-d9ed-499a-a2cc-bcfc4922d7f5
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/14/2019
ms.author: tomfitz
ms.openlocfilehash: 34f34545e4511c4f8bc4af95f906f2871480bd47
ms.sourcegitcommit: f7be3cff2cca149e57aa967e5310eeb0b51f7c77
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/15/2019
ms.locfileid: "56310171"
---
# <a name="understand-the-structure-and-syntax-of-azure-resource-manager-templates"></a>Nociones sobre la estructura y la sintaxis de las plantillas de Azure Resource Manager

En este artículo se describe la estructura de una plantilla de Azure Resource Manager. Presenta las distintas secciones de una plantilla y las propiedades que están disponibles en esas secciones. La plantilla consta de JSON y expresiones que puede usar para generar valores para su implementación. Para obtener instrucciones detalladas sobre cómo crear una plantilla, consulte [Creación de la primera plantilla de Azure Resource Manager](resource-manager-create-first-template.md).

## <a name="template-format"></a>Formato de plantilla

En la estructura más simple, una plantilla tiene los siguientes elementos:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "",
    "parameters": {  },
    "variables": {  },
    "functions": [  ],
    "resources": [  ],
    "outputs": {  }
}
```

| Nombre del elemento | Obligatorio | DESCRIPCIÓN |
|:--- |:--- |:--- |
| $schema |Sí |Ubicación del archivo de esquema JSON que describe la versión del idioma de la plantilla.<br><br> Para las implementaciones de grupos de recursos, use: `https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#`.<br><br>Para las implementaciones de suscripciones, use: `https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#`. |
| contentVersion |Sí |Versión de la plantilla (por ejemplo, 1.0.0.0). Puede especificar cualquier valor para este elemento. Use este valor para documentar los cambios importantes de la plantilla. Al implementar los recursos con la plantilla, este valor se puede usar para asegurarse de que se está usando la plantilla correcta. |
| parameters |Sin  |Valores que se proporcionan cuando se ejecuta la implementación para personalizar la implementación de recursos. |
| variables |Sin  |Valores que se usan como fragmentos JSON en la plantilla para simplificar expresiones de idioma de la plantilla. |
| functions |Sin  |Funciones definidas por el usuario que están disponibles dentro de la plantilla. |
| resources |Sí |Tipos de servicios que se implementan o actualizan en un grupo de recursos o suscripción. |
| outputs |Sin  |Valores que se devuelven después de la implementación. |

Cada elemento tiene propiedades que puede configurar. En el ejemplo siguiente se muestra la sintaxis completa de una plantilla:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "",
    "parameters": {  
        "<parameter-name>" : {
            "type" : "<type-of-parameter-value>",
            "defaultValue": "<default-value-of-parameter>",
            "allowedValues": [ "<array-of-allowed-values>" ],
            "minValue": <minimum-value-for-int>,
            "maxValue": <maximum-value-for-int>,
            "minLength": <minimum-length-for-string-or-array>,
            "maxLength": <maximum-length-for-string-or-array-parameters>,
            "metadata": {
                "description": "<description-of-the parameter>" 
            }
        }
    },
    "variables": {
        "<variable-name>": "<variable-value>",
        "<variable-object-name>": {
            <variable-complex-type-value>
        },
        "<variable-object-name>": {
            "copy": [
                {
                    "name": "<name-of-array-property>",
                    "count": <number-of-iterations>,
                    "input": <object-or-value-to-repeat>
                }
            ]
        },
        "copy": [
            {
                "name": "<variable-array-name>",
                "count": <number-of-iterations>,
                "input": <object-or-value-to-repeat>
            }
        ]
    },
    "functions": [
      {
        "namespace": "<namespace-for-your-function>",
        "members": {
          "<function-name>": {
            "parameters": [
              {
                "name": "<parameter-name>",
                "type": "<type-of-parameter-value>"
              }
            ],
            "output": {
              "type": "<type-of-output-value>",
              "value": "<function-expression>"
            }
          }
        }
      }
    ],
    "resources": [
      {
          "condition": "<boolean-value-whether-to-deploy>",
          "apiVersion": "<api-version-of-resource>",
          "type": "<resource-provider-namespace/resource-type-name>",
          "name": "<name-of-the-resource>",
          "location": "<location-of-resource>",
          "tags": {
              "<tag-name1>": "<tag-value1>",
              "<tag-name2>": "<tag-value2>"
          },
          "comments": "<your-reference-notes>",
          "copy": {
              "name": "<name-of-copy-loop>",
              "count": "<number-of-iterations>",
              "mode": "<serial-or-parallel>",
              "batchSize": "<number-to-deploy-serially>"
          },
          "dependsOn": [
              "<array-of-related-resource-names>"
          ],
          "properties": {
              "<settings-for-the-resource>",
              "copy": [
                  {
                      "name": ,
                      "count": ,
                      "input": {}
                  }
              ]
          },
          "resources": [
              "<array-of-child-resources>"
          ]
      }
    ],
    "outputs": {
        "<outputName>" : {
            "condition": "<boolean-value-whether-to-output-value>",
            "type" : "<type-of-output-value>",
            "value": "<output-value-expression>"
        }
    }
}
```

En este artículo se describen las secciones de la plantilla con más detalle.

## <a name="syntax"></a>Sintaxis

La sintaxis básica de la plantilla es JSON. Sin embargo, las expresiones y funciones amplían los valores JSON disponibles en la plantilla.  Las expresiones se escriben en los literales de cadena JSON cuyo primer y último caracteres son los corchetes `[` y `]`, respectivamente. El valor de la expresión se evalúa cuando se implementa la plantilla. Mientras se escribe como un literal de cadena, el resultado de evaluar la expresión puede ser de un tipo diferente de JSON, como una matriz o un entero, dependiendo de la expresión real.  Para que una cadena literal empiece con un corchete `[`, pero no se interprete como una expresión, agregue otro corchete para que la cadena comience con `[[`.

Normalmente, se usan expresiones con funciones para realizar operaciones con el fin de configurar la implementación. Al igual que en JavaScript, las llamadas de función tienen el formato `functionName(arg1,arg2,arg3)`. Se hace referencia a las propiedades mediante los operadores dot e [index] .

En el ejemplo siguiente se muestra cómo utilizar algunas de las funciones para la construcción de valores:

```json
"variables": {
    "storageName": "[concat(toLower(parameters('storageNamePrefix')), uniqueString(resourceGroup().id))]"
}
```

Para obtener la lista completa de las funciones de plantilla, consulte [Funciones de la plantilla del Administrador de recursos de Azure](resource-group-template-functions.md). 

## <a name="parameters"></a>Parámetros

En la sección de parámetros de la plantilla, especifique los valores que el usuario puede introducir al implementar los recursos. Estos valores de parámetros permiten personalizar la implementación al proporcionar valores que son específicos para un entorno concreto (por ejemplo, desarrollo, prueba y producción). No tiene que especificar parámetros en la plantilla, pero sin parámetros la plantilla implementaría siempre los mismos recursos con los mismos nombres, ubicaciones y propiedades.

En el ejemplo siguiente se muestra una definición simple de parámetro:

```json
"parameters": {
  "siteNamePrefix": {
    "type": "string",
    "metadata": {
      "description": "The name prefix of the web app that you wish to create."
    }
  },
},
```

Para obtener más información acerca de cómo definir parámetros, consulte [Sección de parámetros de plantillas de Azure Resource Manager](resource-manager-templates-parameters.md).

## <a name="variables"></a>variables

En la sección de variables, se crean valores que pueden usarse en toda la plantilla. No es necesario definir las variables, pero a menudo simplifican la plantilla reduciendo expresiones complejas.

En el ejemplo siguiente se muestra una definición de variable simple:

```json
"variables": {
  "webSiteName": "[concat(parameters('siteNamePrefix'), uniqueString(resourceGroup().id))]",
},
```

Para obtener más información acerca de cómo definir variables, consulte [Sección de variables de plantillas de Azure Resource Manager](resource-manager-templates-variables.md).

## <a name="functions"></a>Functions

Dentro de la plantilla, puede crear sus propias funciones. Estas funciones están disponibles para su uso en la plantilla. Normalmente, definirá una expresión compleja que no desea repetir en toda la plantilla. Creará las funciones definidas por el usuario a partir de las expresiones y [funciones](resource-group-template-functions.md) que se admiten en las plantillas.

Al definir una función de usuario, hay algunas restricciones:

* La función no puede acceder a las variables.
* La función solo puede usar los parámetros que se definen en la función. Cuando usa la [función de parámetros](resource-group-template-functions-deployment.md#parameters) dentro de una función definida por el usuario, los parámetros de esa función serán los que limiten sus acciones.
* La función no puede llamar a otras funciones definidas por el usuario.
* La función no puede usar la [función de referencia](resource-group-template-functions-resource.md#reference).
* Los parámetros de la función no pueden tener valores predeterminados.

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

Se llama a la función con:

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

## <a name="resources"></a>Recursos
En la sección de recursos, se define que los recursos se implementan o se actualizan. Esta sección se puede complicar porque debe comprender los tipos que va a implementar para proporcionar los valores adecuados.

```json
"resources": [
  {
    "apiVersion": "2016-08-01",
    "name": "[variables('webSiteName')]",
    "type": "Microsoft.Web/sites",
    "location": "[resourceGroup().location]",
    "properties": {
      "serverFarmId": "/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/Microsoft.Web/serverFarms/<plan-name>"
    }
  }
],
```

Para incluir o excluir condicionalmente un recurso durante la implementación, use el [elemento Condition](resource-manager-templates-resources.md#condition). Para obtener más información sobre la sección de recursos, consulte [Sección de recursos de plantillas de Azure Resource Manager](resource-manager-templates-resources.md).

## <a name="outputs"></a>Salidas
En la sección de salidas, especifique valores que se devuelven de la implementación. Por ejemplo, podría devolver el URI para acceder a un recurso implementado.

```json
"outputs": {
  "newHostName": {
    "type": "string",
    "value": "[reference(variables('webSiteName')).defaultHostName]"
  }
}
```

Para obtener más información, consulte [Outputs section of Azure Resource Manager templates](resource-manager-templates-outputs.md) (Sección de elementos de salida de plantillas de Azure Resource Manager).

<a id="comments" />

## <a name="comments-and-metadata"></a>Comentarios y metadatos

Tiene unas cuantas opciones para agregar comentarios y metadatos a la plantilla.

Puede agregar un objeto `metadata` prácticamente en cualquier parte de la plantilla. Resource Manager omite el objeto, pero el editor JSON puede advertirle de que la propiedad no es válida. En el objeto, defina las propiedades que necesita.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "metadata": {
        "comments": "This template was developed for demonstration purposes.",
        "author": "Example Name"
    },
```

Para **parameters**, agregue un objeto `metadata` con una propiedad `description`.

```json
"parameters": {
    "adminUsername": {
      "type": "string",
      "metadata": {
        "description": "User name for the Virtual Machine."
      }
    },
```

Al implementar la plantilla a través del portal, el texto que proporciona en la descripción se usa automáticamente como sugerencia para ese parámetro.

![Mostrar sugerencia de parámetros](./media/resource-group-authoring-templates/show-parameter-tip.png)

Para **resources**, agregue un elemento `comments` o un objeto de metadatos. En el ejemplo siguiente se muestra un elemento de comentarios y un objeto de metadatos.

```json
"resources": [
  {
    "comments": "Storage account used to store VM disks",
    "apiVersion": "2018-07-01",
    "type": "Microsoft.Storage/storageAccounts",
    "name": "[concat('storage', uniqueString(resourceGroup().id))]",
    "location": "[parameters('location')]",
    "metadata": {
      "comments": "These tags are needed for policy compliance."
    },
    "tags": {
      "Dept": "[parameters('deptName')]",
      "Environment": "[parameters('environment')]"
    },
    "sku": {
      "name": "Standard_LRS"
    },
    "kind": "Storage",
    "properties": {}
  }
]
```

Para **outputs**, agregue un objeto de metadatos para el valor de salida.

```json
"outputs": {
    "hostname": {
      "type": "string",
      "value": "[reference(variables('publicIPAddressName')).dnsSettings.fqdn]",
      "metadata": {
        "comments": "Return the fully qualified domain name"
      }
    },
```

No se puede agregar un objeto de metadatos a las funciones definidas por el usuario.

Para los comentarios en línea, puede usar `//`, pero esta sintaxis no funciona con todas las herramientas. No se puede usar la CLI de Azure para implementar la plantilla con comentarios insertados. Y no se puede usar el editor de plantillas del portal para trabajar en las plantillas con comentarios insertados. Si agrega este estilo de comentarios, asegúrese de que las herramientas que use admitan los comentarios JSON insertados.

```json
{
  "type": "Microsoft.Compute/virtualMachines",
  "name": "[variables('vmName')]", // to customize name, change it in variables
  "location": "[parameters('location')]", //defaults to resource group location
  "apiVersion": "2018-10-01",
  "dependsOn": [ // storage account and network interface must be deployed first
      "[resourceId('Microsoft.Storage/storageAccounts/', variables('storageAccountName'))]",
      "[resourceId('Microsoft.Network/networkInterfaces/', variables('nicName'))]"
  ],
```

En VS Code, puede establecer el modo de lenguaje en JSON con comentarios. Los comentarios insertados ya no están marcados como no válidos. Para cambiar el modo:

1. Abra la selección del modo de lenguaje (presione Ctrl + K M)

1. Seleccione **JSON with Comments** (JSON con comentarios).

   ![Seleccionar modo de lenguaje](./media/resource-group-authoring-templates/select-json-comments.png)

## <a name="template-limits"></a>Límites de plantilla

Limite el tamaño de la plantilla a 1 MB y cada archivo de parámetros a 64 KB. El límite de 1 MB se aplica al estado final de la plantilla una vez se ha ampliado con definiciones de recursos iterativas y los valores de variables y parámetros. 

También está limitado a:

* 256 parámetros
* 256 variables
* 800 recursos (incluido el recuento de copia)
* 64 valores de salida
* 24 576 caracteres en una expresión de plantilla

Puede superar algunos límites de plantilla utilizando una plantilla anidada. Para más información, consulte [Uso de plantillas vinculadas en la implementación de recursos de Azure](resource-group-linked-templates.md). Para reducir el número de parámetros, variables o salidas, puede combinar varios valores en un objeto. Para más información, consulte [Objetos como parámetros](resource-manager-objects-as-parameters.md).

[!INCLUDE [arm-tutorials-quickstarts](../../includes/resource-manager-tutorials-quickstarts.md)]

## <a name="next-steps"></a>Pasos siguientes
* Para ver plantillas completas de muchos tipos diferentes de soluciones, consulte [Plantillas de inicio rápido de Azure](https://azure.microsoft.com/documentation/templates/).
* Para obtener información detallada sobre las funciones que se pueden usar dentro de una plantilla, consulte [Funciones de plantilla de Azure Resource Manager](resource-group-template-functions.md).
* Para combinar varias plantillas durante la implementación, consulte [Uso de plantillas vinculadas con Azure Resource Manager](resource-group-linked-templates.md).
* Para más recomendaciones sobre creación de platillas, consulte [Azure Resource Manager template best practices](template-best-practices.md) (Procedimientos recomendados para plantillas de Azure Resource Manager).
* Para recomendaciones sobre cómo crear plantillas de Resource Manager que puede usar en todos los entornos de Azure y Azure Stack, consulte [Desarrollo de plantillas de Azure Resource Manager para mantener la coherencia en la nube](templates-cloud-consistency.md).
