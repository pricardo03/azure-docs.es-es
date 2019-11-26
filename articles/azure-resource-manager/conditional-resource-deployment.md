---
title: Implementación condicional con plantillas de Azure Resource Manager
description: En este artículo se describe cómo realizar la implementación condicional de un recurso en una plantilla de Azure Resource Manager.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 09/03/2019
ms.author: tomfitz
ms.openlocfilehash: b6d707fc4bbc5fa57ffb0c809d7f70efebef99e9
ms.sourcegitcommit: 7efb2a638153c22c93a5053c3c6db8b15d072949
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/24/2019
ms.locfileid: "72881659"
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

## <a name="condition-with-complete-mode"></a>Condición con el modo completo

Si implementa una plantilla con [modo completo](deployment-modes.md) y no se implementa un recurso porque la condición se evalúa como false, el resultado depende de la versión de API REST que use para implementar la plantilla. Si usa una versión anterior a 2019-05-10, el recurso **no se elimina**. Con 2019-05-10 o posterior, el recurso **se elimina**. Las versiones más recientes de Azure PowerShell y la CLI de Azure eliminan el recurso cuando la condición es falsa.

## <a name="next-steps"></a>Pasos siguientes

* Para más recomendaciones sobre creación de platillas, consulte [Azure Resource Manager template best practices](template-best-practices.md) (Procedimientos recomendados para plantillas de Azure Resource Manager).
* Para crear varias instancias de un recurso, consulte [Recurso, propiedad o iteración de variables en las plantillas de Azure Resource Manager](resource-group-create-multiple.md).