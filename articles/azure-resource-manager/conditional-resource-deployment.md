---
title: Implementación condicional con plantillas de Azure Resource Manager
description: En este artículo se describe cómo realizar la implementación condicional de un recurso en una plantilla de Azure Resource Manager.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 09/03/2019
ms.author: tomfitz
ms.openlocfilehash: 88f8b6a8dcce0e498a7b81b8741072bcf4cfcad8
ms.sourcegitcommit: 267a9f62af9795698e1958a038feb7ff79e77909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/04/2019
ms.locfileid: "70259444"
---
# <a name="conditional-deployment-in-resource-manager-templates"></a>Implementación condicional con plantillas de Resource Manager

En algunas ocasiones, es necesario implementar opcionalmente un recurso en una plantilla. Use el elemento `condition` para especificar si se implementó el recurso. El valor de este elemento se resuelve como true o false. Cuando el valor es true, el recurso se crea. Cuando el valor es false, el recurso no se crea. El valor solo se puede aplicar a todo el recurso.

## <a name="new-or-existing-resource"></a>Recurso nuevo o existente

Puede usar la implementación condicional para crear un recurso nuevo o usar uno existente. En el ejemplo siguiente se muestra cómo usar una condición para implementar una cuenta de almacenamiento nueva o usar una existente.

```json
{
    "condition": "[equals(parameters('newOrExisting'),'new')]",
    "type": "Microsoft.Storage/storageAccounts",
    "name": "[variables('storageAccountName')]",
    "apiVersion": "2017-06-01",
    "location": "[parameters('location')]",
    "sku": {
        "name": "[variables('storageAccountType')]"
    },
    "kind": "Storage",
    "properties": {}
}
```

Cuando el parámetro **newOrExisting** está establecido en **new**, la condición se evalúa como true. Se implementa la cuenta de almacenamiento. Sin embargo, cuando **newOrExisting** está establecido **existing**, la condición se evalúa como false y no se implementa la cuenta de almacenamiento.

Para una plantilla de ejemplo completo que usa el elemento `condition`, consulte [VM with a new or existing Virtual Network, Storage, and Public IP](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-new-or-existing-conditions) (Máquina virtual con una red virtual nueva o existente, almacenamiento y dirección IP pública).

## <a name="allow-condition"></a>Permitir una condición

Puede pasar un valor de parámetro que indica si se permite una condición. En el ejemplo siguiente se implementa un servidor SQL Server y, opcionalmente, se permiten las direcciones IP de Azure.

```json
{
    "type": "Microsoft.Sql/servers",
    "name": "[parameters('serverName')]",
    "apiVersion": "2015-05-01-preview",
    "location": "[parameters('location')]",
    "properties": {
        "administratorLogin": "[parameters('administratorLogin')]",
        "administratorLoginPassword": "[parameters('administratorLoginPassword')]",
        "version": "12.0"
    },
    "resources": [
        {
            "condition": "[parameters('allowAzureIPs')]",
            "type": "firewallRules",
            "name": "AllowAllWindowsAzureIps",
            "apiVersion": "2015-05-01-preview",
            "location": "[parameters('location')]",
            "dependsOn": [
                "[resourceId('Microsoft.Sql/servers/', parameters('serverName'))]"
            ],
            "properties": {
                "endIpAddress": "0.0.0.0",
                "startIpAddress": "0.0.0.0"
            }
        }
    ]
}
```

Para ver la plantilla completa, consulte el artículo sobre el [servidor lógico de Azure SQL](https://github.com/Azure/azure-quickstart-templates/tree/master/101-sql-logical-server).

## <a name="runtime-functions"></a>Funciones en tiempo de ejecución

Si usa una función [reference](resource-group-template-functions-resource.md#reference) o [list](resource-group-template-functions-resource.md#list) con un recurso que se implementa de forma condicional, se puede evaluar la función incluso si el recurso no está implementado. Se genera un error si la función hace referencia a un recurso que no existe.

Use la función [if](resource-group-template-functions-logical.md#if) para asegurarse de que la función se evalúa solo para las condiciones en las que se implementa el recurso. Consulte la función [if](resource-group-template-functions-logical.md#if) para una plantilla de ejemplo que use if y reference con un recurso implementado de forma condicional.

## <a name="next-steps"></a>Pasos siguientes

* Para más recomendaciones sobre creación de platillas, consulte [Azure Resource Manager template best practices](template-best-practices.md) (Procedimientos recomendados para plantillas de Azure Resource Manager).
* Para crear varias instancias de un recurso, consulte [Recurso, propiedad o iteración de variables en las plantillas de Azure Resource Manager](resource-group-create-multiple.md).