---
title: Incorporación de acciones personalizadas a la API REST de Azure
description: Obtenga información sobre cómo agregar acciones personalizadas a la API REST de Azure. En este artículo le guiaremos a través de los requisitos y procedimientos recomendados para los puntos de conexión que quieren implementar acciones personalizadas.
services: managed-applications
ms.service: managed-applications
ms.topic: conceptual
ms.author: jobreen
author: jjbfour
ms.date: 06/20/2019
ms.openlocfilehash: 1bfc0be81d42e922c47755543fb65aa413ec73a9
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/29/2019
ms.locfileid: "67479061"
---
# <a name="adding-custom-actions-to-azure-rest-api"></a>Incorporación de acciones personalizadas a la API REST de Azure

Este artículo le guiará a través de los requisitos y procedimientos recomendados para crear puntos de conexión del proveedor de recursos personalizados de Azure que implementan acciones personalizadas. Si no está familiarizado con los proveedores de recursos personalizados de Azure, consulte [la introducción a los proveedores de recursos personalizados](./custom-providers-overview.md).

## <a name="how-to-define-an-action-endpoint"></a>Definición de un punto de conexión de acción

Un **punto de conexión** es una dirección URL que apunta a un servicio que implementa el contrato subyacente entre él y Azure. El punto de conexión se define en el proveedor de recursos personalizados y puede ser cualquier dirección URL accesible públicamente. En el ejemplo siguiente hay una **acción** denominada `myCustomAction` implementada por `endpointURL`.

**ResourceProvider** de ejemplo:

```JSON
{
  "properties": {
    "actions": [
      {
        "name": "myCustomAction",
        "routingType": "Proxy",
        "endpoint": "https://{endpointURL}/"
      }
    ]
  },
  "location": "eastus",
  "type": "Microsoft.CustomProviders/resourceProviders",
  "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}",
  "name": "{resourceProviderName}"
}
```

## <a name="building-an-action-endpoint"></a>Creación de un punto de conexión de acción

Un **punto de conexión** que implementa una **acción** debe controlar la solicitud y la respuesta de la nueva API en Azure. Cuando se crea un proveedor de recursos personalizados con una **acción**, generará un nuevo conjunto de API en Azure. En este caso, la acción generará una API de acción de Azure nueva para las llamadas `POST`:

``` JSON
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/myCustomAction
```

Solicitud entrante de la API de Azure:

``` HTTP
POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/myCustomAction?api-version=2018-09-01-preview
Authorization: Bearer eyJ0e...
Content-Type: application/json

{
    "myProperty1": "myPropertyValue1",
    "myProperty2": {
        "myProperty3" : "myPropertyValue3"
    }
}
```

Esta solicitud se reenviará al **punto de conexión** de esta forma:

``` HTTP
POST https://{endpointURL}/?api-version=2018-09-01-preview
Content-Type: application/json
X-MS-CustomProviders-RequestPath: /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/myCustomAction

{
    "myProperty1": "myPropertyValue1",
    "myProperty2": {
        "myProperty3" : "myPropertyValue3"
    }
}
```

De manera similar, la respuesta del **punto de conexión** se reenvía de vuelta al cliente. La respuesta del punto de conexión debería devolver:

- Un documento de objeto JSON válido. Todas las matrices y cadenas deben estar anidadas bajo un objeto superior.
- El encabezado `Content-Type` se debe establecer en "application/json; charset=utf-8".

``` HTTP
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8

{
    "myProperty1": "myPropertyValue1",
    "myProperty2": {
        "myProperty3" : "myPropertyValue3"
    }
}
```

Respuesta del proveedor de recursos personalizado de Azure:

``` HTTP
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8

{
    "myProperty1": "myPropertyValue1",
    "myProperty2": {
        "myProperty3" : "myPropertyValue3"
    }
}
```

## <a name="calling-a-custom-action"></a>Llamada a una acción personalizada

Hay dos formas principales de llamar a una acción personalizada fuera de un proveedor de recursos personalizado:

- CLI de Azure
- Plantillas del Administrador de recursos de Azure

### <a name="azure-cli"></a>CLI de Azure

```azurecli-interactive
az resource invoke-action --action {actionName} \
                          --ids /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName} \
                          --request-body \
                            '{
                                "myProperty1": "myPropertyValue1",
                                "myProperty2": {
                                    "myProperty3": "myPropertyValue3"
                                }
                            }'
```

Parámetro | Obligatorio | DESCRIPCIÓN
---|---|---
action | *sí* | El nombre de la acción que se definió en el **ResourceProvider**.
ids | *sí* | El identificador del recurso del **ResourceProvider**.
request-body | *no* | El cuerpo de la solicitud que se enviará al **punto de conexión**.

### <a name="azure-resource-manager-template"></a>Plantilla de Azure Resource Manager

> [!NOTE]
> Las acciones tienen compatibilidad limitada con las plantillas de Azure Resource Manager. Para que sea posible llamar a la acción dentro de una plantilla, debe contener el prefijo [`list`](../azure-resource-manager/resource-group-template-functions-resource.md#list) en el nombre.

**ResourceProvider** de ejemplo con acción de lista:

```JSON
{
  "properties": {
    "actions": [
      {
        "name": "listMyCustomAction",
        "routingType": "Proxy",
        "endpoint": "https://{endpointURL}/"
      }
    ]
  },
  "location": "eastus"
}
```

Plantilla de Azure Resource Manager de ejemplo:

``` JSON
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "variables": {
        "resourceIdentifier": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}",
        "apiVersion": "2018-09-01-preview",
        "functionValues": {
            "myProperty1": "myPropertyValue1",
            "myProperty2": {
                "myProperty3": "myPropertyValue3"
            }
        }
    },
    "resources": [],
    "outputs": {
        "myCustomActionOutput": {
            "type": "object",
            "value": "[listMyCustomAction(variables('resourceIdentifier'), variables('apiVersion'), variables('functionValues'))]"
        }
    }
}
```

Parámetro | Obligatorio | DESCRIPCIÓN
---|---|---
resourceIdentifier | *sí* | El identificador del recurso del **ResourceProvider**.
apiVersion | *sí* | La versión de la API del entorno en tiempo de ejecución de un recurso. Siempre debe ser "2018-09-01-preview".
functionValues | *no* | El cuerpo de la solicitud que se enviará al **punto de conexión**.

## <a name="next-steps"></a>Pasos siguientes

- [Introducción a los proveedores de recursos personalizados de Azure](./custom-providers-overview.md)
- [Tutorial: Creación de un proveedor de recursos personalizados de Azure e implementación de recursos personalizados](./create-custom-provider.md)
- [How To: Incorporación de recursos personalizados a la API REST de Azure](./custom-providers-resources-endpoint-how-to.md)
