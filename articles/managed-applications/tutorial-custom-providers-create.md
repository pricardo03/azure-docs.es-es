---
title: Creación y uso de proveedores personalizados de Azure
description: En este tutorial se explica cómo crear y utilizar un proveedor personalizado.
author: jjbfour
ms.service: managed-applications
ms.topic: tutorial
ms.date: 06/19/2019
ms.author: jobreen
ms.openlocfilehash: 65a8e60d8216e1da16af987c9e699e24ecaec3ec
ms.sourcegitcommit: 66237bcd9b08359a6cce8d671f846b0c93ee6a82
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2019
ms.locfileid: "67799134"
---
# <a name="authoring-a-restful-endpoint-for-custom-providers"></a>Creación de un punto de conexión de RESTful para proveedores personalizados

Los proveedores personalizados le permiten personalizar los flujos de trabajo en Azure. Un proveedor personalizado es un contrato entre Azure y un `endpoint`. Este tutorial le guiará por el proceso de creación de un proveedor personalizado. Si no está familiarizado con los proveedores personalizados de Azure, consulte [la introducción a los proveedores de recursos personalizados](./custom-providers-overview.md).

Este tutorial se divide en los pasos siguientes:

- Descripción de un proveedor personalizado
- Definición de las acciones y los recursos personalizados
- Implementación del proveedor personalizado

Este tutorial se basará en los siguientes tutoriales:

- [Creación de un punto de conexión de RESTful para proveedores personalizados](./tutorial-custom-providers-function-authoring.md)

## <a name="creating-a-custom-provider"></a>Creación de un proveedor personalizado

> [!NOTE]
> En este tutorial no se va a hacer referencia a la creación de un punto de conexión. Siga el [tutorial sobre la creación de puntos de conexión RESTful](./tutorial-custom-providers-function-authoring.md) si no tiene ninguno.

Una vez creado `endpoint`, puede generar la creación de un proveedor personalizado para generar un contrato entre él y `endpoint`. Un proveedor personalizado le permite especificar una lista de definiciones de puntos de conexión.

```JSON
{
  "name": "myEndpointDefinition",
  "routingType": "Proxy",
  "endpoint": "https://<yourapp>.azurewebsites.net/api/<funcname>?code=<functionkey>"
}
```

Propiedad | Obligatorio | DESCRIPCIÓN
---|---|---
Nombre | *sí* | El nombre de la definición del punto de conexión. Azure expondrá este nombre a través de su API en "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/<br>resourceProviders/{resourceProviderName}/{endpointDefinitionName}"
routingType | *no* | Determina el tipo de contrato con el `endpoint`. Si no se especifica, el valor predeterminado será "Proxy".
punto de conexión | *sí* | El punto de conexión al que enrutar las solicitudes. Esto controlará la respuesta, así como los efectos secundarios de la solicitud.

En este caso, `endpoint` es la dirección URL del desencadenador de la función de Azure. `<yourapp>`, `<funcname>`, y `<functionkey>` deben reemplazarse por los valores de la función creada.

## <a name="defining-custom-actions-and-resources"></a>Definición de las acciones y los recursos personalizados

El proveedor personalizado contiene una lista de definiciones de punto de conexión que se modelan con `actions` y `resourceTypes`. `actions` se asigna a las acciones personalizadas expuestas por el proveedor personalizado, mientras que `resourceTypes` son los recursos personalizados. Para este tutorial, definiremos un proveedor personalizado con un elemento `action` llamado `myCustomAction` y un elemento `resourceType` llamado `myCustomResources`.

```JSON
{
  "properties": {
    "actions": [
      {
        "name": "myCustomAction",
        "routingType": "Proxy",
        "endpoint": "https://<yourapp>.azurewebsites.net/api/<funcname>?code=<functionkey>"
      }
    ],
    "resourceTypes": [
      {
        "name": "myCustomResources",
        "routingType": "Proxy",
        "endpoint": "https://<yourapp>.azurewebsites.net/api/<funcname>?code=<functionkey>"
      }
    ]
  },
  "location": "eastus"
}
```

Reemplace `endpoint` por la dirección URL del desencadenador desde la función que creó en el tutorial anterior.

## <a name="deploying-the-custom-provider"></a>Implementación del proveedor personalizado

> [!NOTE]
> `endpoint` debe reemplazarse por la dirección URL de la función.

El proveedor personalizado anterior puede implementarse mediante una plantilla de Azure Resource Manager.

```JSON
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
        {
            "type": "Microsoft.CustomProviders/resourceProviders",
            "name": "myCustomProvider",
            "apiVersion": "2018-09-01-preview",
            "location": "eastus",
            "properties": {
                "actions": [
                    {
                        "name": "myCustomAction",
                        "routingType": "Proxy",
                        "endpoint": "https://<yourapp>.azurewebsites.net/api/<funcname>?code=<functionkey>"
                    }
                ],
                "resourceTypes": [
                    {
                        "name": "myCustomResources",
                        "routingType": "Proxy",
                        "endpoint": "https://<yourapp>.azurewebsites.net/api/<funcname>?code=<functionkey>"
                    }
                ]
            }
        }
    ]
}
```

## <a name="using-custom-actions-and-resources"></a>Uso de acciones y recursos personalizados

Cuando hayamos creado un proveedor personalizado, podemos utilizar las nuevas API de Azure. En la siguiente sección se explica cómo llamar a un proveedor personalizado y cómo utilizarlo.

### <a name="custom-actions"></a>Acciones personalizadas

# <a name="azure-clitabazure-cli"></a>[CLI de Azure](#tab/azure-cli)

> [!NOTE]
> Los elementos `{subscriptionId}` y `{resourceGroupName}` deben reemplazarse por la suscripción y el grupo de recursos donde se implementó el proveedor personalizado.

```azurecli-interactive
az resource invoke-action --action myCustomAction \
                          --ids /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/myCustomProvider \
                          --request-body
                            '{
                                "hello": "world"
                            }'
```

Parámetro | Obligatorio | DESCRIPCIÓN
---|---|---
action | *sí* | El nombre de la acción definida en el proveedor personalizado creado.
ids | *sí* | El identificador de recurso del proveedor personalizado creado.
request-body | *no* | El cuerpo de la solicitud que se enviarán a `endpoint`.

# <a name="templatetabtemplate"></a>[Plantilla](#tab/template)

Ninguno.

---

### <a name="custom-resources"></a>Recursos personalizados

# <a name="azure-clitabazure-cli"></a>[CLI de Azure](#tab/azure-cli)

> [!NOTE]
> Los elementos `{subscriptionId}` y `{resourceGroupName}` deben reemplazarse por la suscripción y el grupo de recursos donde se implementó el proveedor personalizado.

Cree un recurso personalizado:

```azurecli-interactive
az resource create --is-full-object \
                   --id /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/myCustomProvider/myCustomResources/myTestResourceName1 \
                   --properties
                    '{
                        "location": "eastus",
                        "properties": {
                            "hello" : "world"
                        }
                    }'
```

Parámetro | Obligatorio | DESCRIPCIÓN
---|---|---
is-full-object | *sí* | Indica que el objeto properties incluye otras opciones, como ubicación, etiquetas, sku o plan.
id | *sí* | El identificador de recurso del recurso personalizado. Debería existir fuera del proveedor personalizado creado.
properties | *sí* | El cuerpo de la solicitud que se enviarán a `endpoint`.

Elimine un recurso personalizado de Azure:

```azurecli-interactive
az resource delete --id /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/myCustomProvider/myCustomResources/myTestResourceName1
```

Parámetro | Obligatorio | DESCRIPCIÓN
---|---|---
id | *sí* | El identificador de recurso del recurso personalizado. Debería existir fuera del proveedor personalizado creado.

Recupere un recurso personalizado de Azure:

```azurecli-interactive
az resource show --id /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/myCustomProvider/myCustomResources/myTestResourceName1
```

Parámetro | Obligatorio | DESCRIPCIÓN
---|---|---
id | *sí* | El identificador de recurso del recurso personalizado. Debería existir fuera del proveedor personalizado creado.

# <a name="templatetabtemplate"></a>[Plantilla](#tab/template)

Plantilla de Azure Resource Manager de ejemplo:

```JSON
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
        {
            "type": "Microsoft.CustomProviders/resourceProviders/myCustomResources",
            "name": "myCustomProvider/myTestResourceName1",
            "apiVersion": "2018-09-01-preview",
            "location": "eastus",
            "properties": {
                "hello": "world"
            }
        }
    ]
}
```

Parámetro | Obligatorio | DESCRIPCIÓN
---|---|---
resourceTypeName | *sí* | El elemento `name` del parámetro *resourceType* definido en el proveedor personalizado.
resourceProviderName | *sí* | El nombre de instancia del proveedor personalizado.
customResourceName | *sí* | El nombre del recurso personalizado.

---

> [!NOTE]
> Cuando haya terminado de implementar y usar el proveedor personalizado, recuerde limpiar los recursos creados, incluida la función de Azure.

## <a name="next-steps"></a>Pasos siguientes

En este artículo, ha obtenido información sobre los proveedores personalizados. Vaya al artículo siguiente para crear un proveedor personalizado.

- [How To: Adding Custom Actions to Azure REST API](./custom-providers-action-endpoint-how-to.md) (Procedimientos: Adición de acciones personalizadas a la API REST de Azure)
- [How To: Adding Custom Resources to Azure REST API](./custom-providers-resources-endpoint-how-to.md) (Procedimientos: Adición de recursos personalizados a la API REST de Azure)
