---
title: Sección del parámetro de plantilla de Azure Resource Manager | Microsoft Docs
description: Describe la sección de parámetros de plantillas de Azure Resource Manager mediante la sintaxis declarativa de JSON.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/03/2019
ms.author: tomfitz
ms.openlocfilehash: dc817302ab39d12ccd1d1a20d4dd72f94352c796
ms.sourcegitcommit: a65b424bdfa019a42f36f1ce7eee9844e493f293
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/04/2019
ms.locfileid: "55695682"
---
# <a name="parameters-section-of-azure-resource-manager-templates"></a>Sección de parámetros de plantillas de Azure Resource Manager
En la sección de parámetros de la plantilla, especifique los valores que el usuario puede introducir al implementar los recursos. Estos valores de parámetros permiten personalizar la implementación al proporcionar valores que son específicos para un entorno concreto (por ejemplo, desarrollo, prueba y producción). No tiene que especificar parámetros en la plantilla, pero sin parámetros la plantilla implementaría siempre los mismos recursos con los mismos nombres, ubicaciones y propiedades.

Está limitado a 256 parámetros en una plantilla. Puede reducir el número de parámetros mediante el uso de objetos que contienen varias propiedades, tal como se muestra en este artículo.

## <a name="define-and-use-a-parameter"></a>Definición y uso de un parámetro

En el ejemplo siguiente se muestra una definición simple de parámetro. Se define el nombre del parámetro y se especifica que toma un valor de cadena. El parámetro solo acepta valores que tienen sentido para el uso previsto. Cuando no se proporciona ningún valor durante la implementación, se especifica un valor predeterminado. Por último, el parámetro incluye una descripción de su uso. 

```json
"parameters": {
  "storageSKU": {
    "type": "string",
    "allowedValues": [
      "Standard_LRS",
      "Standard_ZRS",
      "Standard_GRS",
      "Standard_RAGRS",
      "Premium_LRS"
    ],
    "defaultValue": "Standard_LRS",
    "metadata": {
      "description": "The type of replication to use for the storage account."
    }
  }   
}
```

En la plantilla,debe hacer referencia al valor del parámetro con la sintaxis siguiente:

```json
"resources": [
  {
    "type": "Microsoft.Storage/storageAccounts",
    "sku": {
      "name": "[parameters('storageSKU')]"
    },
    ...
  }
]
```

## <a name="available-properties"></a>Propiedades disponibles

El ejemplo anterior mostraba solo algunas de las propiedades que puede utilizar en la sección de parámetros. Las propiedades disponibles son:

```json
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
}
```

| Nombre del elemento | Obligatorio | DESCRIPCIÓN |
|:--- |:--- |:--- |
| parameterName |Sí |Nombre del parámetro. Debe ser un identificador válido de JavaScript. |
| Tipo |Sí |Tipo del valor del parámetro. Los tipos y valores permitidos son **string**, **secureString**, **int**, **bool**, **objet**, **secureObject** y **array**. |
| defaultValue |Sin  |Valor predeterminado del parámetro, si no se proporciona ningún valor. |
| allowedValues |Sin  |Matriz de valores permitidos para el parámetro para asegurarse de que se proporciona el valor correcto. |
| minValue |Sin  |El valor mínimo de parámetros de tipo int, este valor es inclusivo. |
| maxValue |Sin  |El valor máximo de parámetros de tipo int, este valor es inclusivo. |
| minLength |Sin  |Longitud mínima de los parámetros de tipo cadena, cadena segura y matriz; este valor es inclusivo. |
| maxLength |Sin  |Longitud máxima de los parámetros de tipo cadena, cadena segura y matriz; este valor es inclusivo. |
| description |Sin  |Descripción del parámetro que se muestra a los usuarios a través del portal. Para más información, consulte [Comentarios en plantillas](resource-group-authoring-templates.md#comments). |

## <a name="template-functions-with-parameters"></a>Funciones de plantilla con parámetros

Cuando se especifica el valor predeterminado de un parámetro, puede usar la mayoría de las funciones de plantilla. Puede usar otro valor de parámetro para compilar un valor predeterminado. La plantilla siguiente muestra el uso de funciones en el valor predeterminado:

```json
"parameters": {
  "siteName": {
    "type": "string",
    "defaultValue": "[concat('site', uniqueString(resourceGroup().id))]",
    "metadata": {
      "description": "The site name. To use the default value, do not specify a new value."
    }
  },
  "hostingPlanName": {
    "type": "string",
    "defaultValue": "[concat(parameters('siteName'),'-plan')]",
    "metadata": {
      "description": "The host name. To use the default value, do not specify a new value."
    }
  }
}
```

No puede usar la función `reference` en la sección de parámetros. Los parámetros se evalúan antes de la implementación, por lo que la función `reference` no puede obtener el estado de tiempo de ejecución de un recurso. 

## <a name="objects-as-parameters"></a>Objetos como parámetros

Puede ser más fácil organizar los valores relacionados pasándolos como objetos. Con este enfoque también se reduce el número de parámetros de la plantilla.

Defina el parámetro en la plantilla y especifique un objeto JSON en lugar de un valor único durante la implementación. 

```json
"parameters": {
  "VNetSettings": {
    "type": "object",
    "defaultValue": {
      "name": "VNet1",
      "location": "eastus",
      "addressPrefixes": [
        {
          "name": "firstPrefix",
          "addressPrefix": "10.0.0.0/22"
        }
      ],
      "subnets": [
        {
          "name": "firstSubnet",
          "addressPrefix": "10.0.0.0/24"
        },
        {
          "name": "secondSubnet",
          "addressPrefix": "10.0.1.0/24"
        }
      ]
    }
  }
},
```

Después, haga referencia a las subpropiedades del parámetro con el operador punto.

```json
"resources": [
  {
    "apiVersion": "2015-06-15",
    "type": "Microsoft.Network/virtualNetworks",
    "name": "[parameters('VNetSettings').name]",
    "location": "[parameters('VNetSettings').location]",
    "properties": {
      "addressSpace":{
        "addressPrefixes": [
          "[parameters('VNetSettings').addressPrefixes[0].addressPrefix]"
        ]
      },
      "subnets":[
        {
          "name":"[parameters('VNetSettings').subnets[0].name]",
          "properties": {
            "addressPrefix": "[parameters('VNetSettings').subnets[0].addressPrefix]"
          }
        },
        {
          "name":"[parameters('VNetSettings').subnets[1].name]",
          "properties": {
            "addressPrefix": "[parameters('VNetSettings').subnets[1].addressPrefix]"
          }
        }
      ]
    }
  }
]
```

## <a name="example-templates"></a>Plantillas de ejemplo

Estas plantillas de ejemplo muestran algunos escenarios para usar los parámetros. Impleméntelos para probar cómo se controlan los parámetros en diferentes escenarios.

|Plantilla  |DESCRIPCIÓN  |
|---------|---------|
|[parámetros con funciones para los valores predeterminados](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/parameterswithfunctions.json) | Muestra cómo utilizar las funciones de plantilla al definir valores predeterminados para parámetros. La plantilla no implementa ningún recurso. Genera valores de parámetro y devuelve dichos valores. |
|[objeto de parámetro](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/parameterobject.json) | Muestra cómo utilizar un objeto para un parámetro. La plantilla no implementa ningún recurso. Genera valores de parámetro y devuelve dichos valores. |

## <a name="next-steps"></a>Pasos siguientes

* Para ver plantillas completas de muchos tipos diferentes de soluciones, consulte [Plantillas de inicio rápido de Azure](https://azure.microsoft.com/documentation/templates/).
* Para más información acerca de cómo especificar los valores de los parámetros durante la implementación, consulte [Deploy an application with Azure Resource Manager template](resource-group-template-deploy.md)(Implementación de aplicaciones con la plantilla de Azure Resource Manager). 
* Para más recomendaciones sobre creación de platillas, consulte [Azure Resource Manager template best practices](template-best-practices.md) (Procedimientos recomendados para plantillas de Azure Resource Manager).
* Para obtener información sobre el uso de un objeto de parámetro, consulte [Uso de un objeto como parámetro en una plantilla de Azure Resource Manager](/azure/architecture/building-blocks/extending-templates/objects-as-parameters).
