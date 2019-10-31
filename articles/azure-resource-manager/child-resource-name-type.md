---
title: 'Recursos secundarios: plantillas de Azure Resource Manager'
description: Describe cómo establecer el nombre y el tipo de los recursos secundarios en una plantilla de Azure Resource Manager.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 08/26/2019
ms.author: tomfitz
ms.openlocfilehash: 3a90b2155b11d4c12bc1f571af3f15fdbceb12b9
ms.sourcegitcommit: 6eecb9a71f8d69851bc962e2751971fccf29557f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2019
ms.locfileid: "72532289"
---
# <a name="set-name-and-type-for-child-resources"></a>Establecimiento del nombre y el tipo de los recursos secundarios

Los recursos secundarios son recursos que solo existen en el contexto de otro recurso. Por ejemplo, una [extensión de máquina virtual](/azure/templates/microsoft.compute/2019-03-01/virtualmachines/extensions) no puede existir sin una [máquina virtual](/azure/templates/microsoft.compute/2019-03-01/virtualmachines). El recurso de extensión es un elemento secundario de la máquina virtual.

En una plantilla de Resource Manager, puede especificar el recurso secundario dentro del recurso primario o fuera de él. En el ejemplo siguiente se muestra el recurso secundario incluido en la propiedad resources del recurso primario.

```json
"resources": [
  {
    <parent-resource>
    "resources": [
      <child-resource>
    ]
  }
]
```

En el ejemplo siguiente se muestra el recurso secundario fuera del recurso primario. Este enfoque puede usarse si el recurso primario no está implementado en la misma plantilla o si quiere usar [copy](resource-group-create-multiple.md) para crear más de un recurso secundario.

```json
"resources": [
  {
    <parent-resource>
  },
  {
    <child-resource>
  }
]
```

Los valores proporcionados para el nombre y el tipo de recurso varían en función de si el recurso secundario se ha definido dentro del recurso primario o fuera de él.

## <a name="within-parent-resource"></a>Dentro del recurso primario

Cuando el recurso secundario se define dentro del tipo de recurso primario, los valores type y name se indican como una sola palabra sin barras diagonales.

```json
"type": "{child-resource-type}",
"name": "{child-resource-name}",
```

En el ejemplo siguiente se muestra una red virtual con una subred. Observe que la subred se incluye en la matriz de recursos de la red virtual. El nombre se establece en **Subnet1** y el tipo se establece en **subnets**. El recurso secundario se marca como dependiente del recurso primario porque este debe existir antes de que se pueda implementar el recurso secundario.

```json
"resources": [
  {
    "apiVersion": "2018-10-01",
    "type": "Microsoft.Network/virtualNetworks",
    "name": "VNet1",
    "location": "[parameters('location')]",
    "properties": {
      "addressSpace": {
        "addressPrefixes": [
          "10.0.0.0/16"
        ]
      }
    },
    "resources": [
      {
        "apiVersion": "2018-10-01",
        "type": "subnets",
        "location": "[parameters('location')]",
        "name": "Subnet1",
        "dependsOn": [
          "VNet1"
        ],
        "properties": {
          "addressPrefix": "10.0.0.0/24"
        }
      }
    ]
  }
]
```

El tipo de recurso completo sigue siendo **Microsoft.Network/virtualNetworks/subnets**. No se proporciona **Microsoft.Network/virtualNetworks/** porque se asume a partir del tipo de recurso primario.

El nombre del recurso secundario se establece en **Subnet1**, pero el nombre completo incluye el primario. No se proporciona **VNet1** porque se asume a partir del recurso primario.

## <a name="outside-parent-resource"></a>Fuera del recurso primario

Cuando el recurso secundario se define fuera del recurso primario, los valores type y name se indican con barras diagonales para incluir el tipo y el nombre primarios.

```json
"type": "{resource-provider-namespace}/{parent-resource-type}/{child-resource-type}",
"name": "{parent-resource-name}/{child-resource-name}",
```

En el ejemplo siguiente se muestra una red virtual y una subred que se definen en el nivel raíz. Observe que la subred no está incluida dentro de la matriz de recursos de la red virtual. El nombre se establece en **VNet1/Subnet1** y el tipo se establece en **Microsoft.Network/virtualNetworks/subnets**. El recurso secundario se marca como dependiente del recurso primario porque este debe existir antes de que se pueda implementar el recurso secundario.

```json
"resources": [
  {
    "apiVersion": "2018-10-01",
    "type": "Microsoft.Network/virtualNetworks",
    "name": "VNet1",
    "location": "[parameters('location')]",
    "properties": {
      "addressSpace": {
        "addressPrefixes": [
          "10.0.0.0/16"
        ]
      }
    }
  },
  {
    "apiVersion": "2018-10-01",
    "type": "Microsoft.Network/virtualNetworks/subnets",
    "location": "[parameters('location')]",
    "name": "VNet1/Subnet1",
    "dependsOn": [
      "VNet1"
    ],
    "properties": {
      "addressPrefix": "10.0.0.0/24"
    }
  }
]
```

## <a name="next-steps"></a>Pasos siguientes

* Para más información sobre la creación de plantillas del Administrador de recursos de Azure, consulte [Creación de plantillas](resource-group-authoring-templates.md). 

* Para información sobre el formato del nombre del recurso al hacer referencia a él, consulte la [función reference](resource-group-template-functions-resource.md#reference).
