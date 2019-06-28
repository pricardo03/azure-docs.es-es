---
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: include
ms.date: 03/11/2019
ms.author: tomfitz
ms.openlocfilehash: 104bd13a0cf97a8605670adde479c2a2eeb29c15
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "66128990"
---
Para etiquetar un recurso durante la implementación, agregue el elemento `tags` al recurso que se va a implementar. Proporcione el nombre y el valor de la etiqueta.

### <a name="apply-a-literal-value-to-the-tag-name"></a>Aplicación de un valor literal al nombre de etiqueta
En el ejemplo siguiente se muestra una cuenta de almacenamiento con dos etiquetas (`Dept` y `Environment`) que se establecen en valores literales:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
    {
      "apiVersion": "2016-01-01",
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[concat('storage', uniqueString(resourceGroup().id))]",
      "location": "[resourceGroup().location]",
      "tags": {
        "Dept": "Finance",
        "Environment": "Production"
      },
      "sku": {
        "name": "Standard_LRS"
      },
      "kind": "Storage",
      "properties": { }
    }
    ]
}
```

Para establecer una etiqueta en un valor de datetime, use la [función utcNow](../articles/azure-resource-manager/resource-group-template-functions-string.md#utcnow).

### <a name="apply-an-object-to-the-tag-element"></a>Aplicación de un objeto al elemento de etiqueta
Puede definir un parámetro de objeto que almacene varias etiquetas y aplicar ese objeto al elemento de etiqueta. Cada propiedad del objeto se convierte en una etiqueta independiente para el recurso. El siguiente ejemplo tiene un parámetro denominado `tagValues` que se aplica al elemento de etiqueta.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "tagValues": {
      "type": "object",
      "defaultValue": {
        "Dept": "Finance",
        "Environment": "Production"
      }
    }
  },
  "resources": [
    {
      "apiVersion": "2016-01-01",
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[concat('storage', uniqueString(resourceGroup().id))]",
      "location": "[resourceGroup().location]",
      "tags": "[parameters('tagValues')]",
      "sku": {
        "name": "Standard_LRS"
      },
      "kind": "Storage",
      "properties": {}
    }
  ]
}
```

### <a name="apply-a-json-string-to-the-tag-name"></a>Aplicación de una cadena JSON al nombre de etiqueta

Para almacenar muchos valores en una única etiqueta, aplique una cadena JSON que represente los valores. Toda la cadena JSON se almacena como una etiqueta que no puede superar los 256 caracteres. En el ejemplo siguiente se muestra una etiqueta denominada `CostCenter` que contiene varios valores de una cadena JSON:  

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
    {
      "apiVersion": "2016-01-01",
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[concat('storage', uniqueString(resourceGroup().id))]",
      "location": "[resourceGroup().location]",
      "tags": {
        "CostCenter": "{\"Dept\":\"Finance\",\"Environment\":\"Production\"}"
      },
      "sku": {
        "name": "Standard_LRS"
      },
      "kind": "Storage",
      "properties": { }
    }
    ]
}
```