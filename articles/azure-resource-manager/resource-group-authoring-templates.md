---
title: Estructura y sintaxis de las plantillas de Azure Resource Manager | Microsoft Docs
description: Describe la estructura y las propiedades de plantillas de Azure Resource Manager mediante la sintaxis declarativa de JSON.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 19694cb4-d9ed-499a-a2cc-bcfc4922d7f5
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/22/2018
ms.author: tomfitz
ms.openlocfilehash: 28ef94113c76cd70e12a9682e1c523afc3f0a233
ms.sourcegitcommit: 9e179a577533ab3b2c0c7a4899ae13a7a0d5252b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/23/2018
ms.locfileid: "49945882"
---
# <a name="understand-the-structure-and-syntax-of-azure-resource-manager-templates"></a>Nociones sobre la estructura y la sintaxis de las plantillas de Azure Resource Manager
En este artículo se describe la estructura de una plantilla de Azure Resource Manager. Presenta las distintas secciones de una plantilla y las propiedades que están disponibles en esas secciones. La plantilla consta de JSON y expresiones que puede usar para generar valores para su implementación. Para obtener instrucciones detalladas sobre cómo crear una plantilla, consulte [Creación de la primera plantilla de Azure Resource Manager](resource-manager-create-first-template.md).

## <a name="quickstarts-and-tutorials"></a>Guías de inicio rápido y tutoriales

Use las siguientes guías de inicio rápido y tutoriales para aprender a desarrollar plantillas de Resource Manager:

- Guías de inicio rápido

  	|Título|DESCRIPCIÓN|
  	|------|-----|
  	|[Uso de Azure Portal](./resource-manager-quickstart-create-templates-use-the-portal.md)|Genere una plantilla mediante el portal y el proceso de edición e implementación de la plantilla.|
  	|[Usar Visual Studio Code](./resource-manager-quickstart-create-templates-use-visual-studio-code.md)|Use Visual Studio Code para crear y editar plantillas y use Azure Cloud Shell para implementar plantillas.|
  	|[Usar Visual Studio](./vs-azure-tools-resource-groups-deployment-projects-create-deploy.md)|Use Visual Studio para crear, editar e implementar plantillas.|

- Tutoriales

  	|Título|DESCRIPCIÓN|
  	|------|-----|
  	|[Utilización de la referencia de la plantilla](./resource-manager-tutorial-create-encrypted-storage-accounts.md)|Use la documentación de referencia de la plantilla para desarrollar las plantillas. En el tutorial, busque el esquema de la cuenta de almacenamiento y use la información para crear una cuenta de almacenamiento cifrada.|
  	|[Creación de varias instancias](./resource-manager-tutorial-create-multiple-instances.md)|Cree varias instancias de recursos de Azure. En el tutorial, puede crear varias instancias de la cuenta de almacenamiento.|
  	|[Establecimiento del orden de implementación de los recursos](./resource-manager-tutorial-create-templates-with-dependent-resources.md)|Defina las dependencias de recursos. En el tutorial, va a crear una red virtual, una máquina virtual y los recursos dependientes de Azure. Aprenderá cómo se definen las dependencias.|
  	|[Condiciones de uso](./resource-manager-tutorial-use-conditions.md)|Implemente recursos basados en algunos valores de parámetro. En el tutorial, definirá una plantilla para crear una nueva cuenta de almacenamiento o usará una cuenta de almacenamiento ya existente basada en el valor de un parámetro.|
  	|[Integración de Key Vault](./resource-manager-tutorial-use-key-vault.md)|Recupere los secretos o contraseñas de Azure Key Vault. En el tutorial, creará una máquina virtual.  La contraseña del administrador de la máquina virtual se recupera de Key Vault.|
  	|[Creación de plantillas vinculadas](./resource-manager-tutorial-create-linked-templates.md)|Modularice las plantillas y llame a otras plantillas desde una plantilla. En el tutorial, va a crear una red virtual, una máquina virtual y los recursos dependientes.  La cuenta de almacenamiento dependiente se define en una plantilla vinculada. |
  	|[Uso de procedimientos de implementación seguros](./deployment-manager-tutorial.md)|Use Azure Deployment Manager. |

## <a name="template-format"></a>Formato de plantilla

En la estructura más simple, una plantilla tiene los siguientes elementos:

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
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
| $schema |SÍ |Ubicación del archivo de esquema JSON que describe la versión del idioma de la plantilla. Use la dirección URL que se muestra en el ejemplo anterior. |
| contentVersion |SÍ |Versión de la plantilla (por ejemplo, 1.0.0.0). Puede especificar cualquier valor para este elemento. Use este valor para documentar los cambios importantes de la plantilla. Al implementar los recursos con la plantilla, este valor se puede usar para asegurarse de que se está usando la plantilla correcta. |
| parameters |Sin  |Valores que se proporcionan cuando se ejecuta la implementación para personalizar la implementación de recursos. |
| variables |Sin  |Valores que se usan como fragmentos JSON en la plantilla para simplificar expresiones de idioma de la plantilla. |
| functions |Sin  |Funciones definidas por el usuario que están disponibles dentro de la plantilla. |
| resources |SÍ |Tipos de servicios que se implementan o actualizan en un grupo de recursos. |
| outputs |Sin  |Valores que se devuelven después de la implementación. |

Cada elemento tiene propiedades que puede configurar. En el ejemplo siguiente se muestra la sintaxis completa de una plantilla:

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
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
                    "input": {
                        <properties-to-repeat>
                    }
                }
            ]
        },
        "copy": [
            {
                "name": "<variable-array-name>",
                "count": <number-of-iterations>,
                "input": {
                    <properties-to-repeat>
                }
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
* La función no puede acceder a los parámetros de plantilla. Es decir, la [función de los parámetros](resource-group-template-functions-deployment.md#parameters) está restringida a los parámetros de función.
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

## <a name="template-limits"></a>Límites de plantilla

Limite el tamaño de la plantilla a 1 MB y cada archivo de parámetros a 64 KB. El límite de 1 MB se aplica al estado final de la plantilla una vez se ha ampliado con definiciones de recursos iterativas y los valores de variables y parámetros. 

También está limitado a:

* 256 parámetros
* 256 variables
* 800 recursos (incluido el recuento de copia)
* 64 valores de salida
* 24 576 caracteres en una expresión de plantilla

Puede superar algunos límites de plantilla utilizando una plantilla anidada. Para más información, consulte [Uso de plantillas vinculadas en la implementación de recursos de Azure](resource-group-linked-templates.md). Para reducir el número de parámetros, variables o salidas, puede combinar varios valores en un objeto. Para más información, consulte [Objetos como parámetros](resource-manager-objects-as-parameters.md).

## <a name="next-steps"></a>Pasos siguientes
* Para ver plantillas completas de muchos tipos diferentes de soluciones, consulte [Plantillas de inicio rápido de Azure](https://azure.microsoft.com/documentation/templates/).
* Para obtener información detallada sobre las funciones que se pueden usar dentro de una plantilla, consulte [Funciones de plantilla de Azure Resource Manager](resource-group-template-functions.md).
* Para combinar varias plantillas en la implementación, consulte [Uso de plantillas vinculadas con Azure Resource Manager](resource-group-linked-templates.md).
* Para recomendaciones sobre cómo crear plantillas de Resource Manager que puede usar en todo Azure, nubes soberanas de Azure y Azure Stack, consulte [Desarrollo de plantillas de Azure Resource Manager para mantener la coherencia en la nube](templates-cloud-consistency.md).
