---
title: Aplicación administrada de Azure con la identidad administrada
description: Obtenga información sobre cómo configurar una aplicación administrada con una identidad administrada. Identidad administrada puede usarse para implementar aplicaciones administradas de vinculado a los recursos existentes, conceder a las aplicaciones administradas para administrar recursos de Azure fuera del grupo de recursos administrados y proporcionar una identidad de aplicaciones administradas de operativa para el registro de actividad y otros servicios dentro de Azure.
services: managed-applications
ms.service: managed-applications
ms.topic: conceptual
ms.reviewer: ''
ms.author: jobreen
author: jjbfour
ms.date: 05/13/2019
ms.openlocfilehash: 5ef653e825a5f1eb0f5df52f9c2544a5224b34cf
ms.sourcegitcommit: 13cba995d4538e099f7e670ddbe1d8b3a64a36fb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/22/2019
ms.locfileid: "66003452"
---
# <a name="azure-managed-application-with-managed-identity"></a>Aplicación administrada de Azure con la identidad administrada

> [!NOTE]
> Soporte técnico de identidad administrada para aplicaciones administradas está actualmente en versión preliminar. Use la versión de api 2018-09-01-preview para utilizar la identidad administrada.

Obtenga información sobre cómo configurar una aplicación administrada para contener una identidad administrada. Identidad administrada puede utilizarse para permitir que el cliente conceder el acceso a las aplicaciones administradas a los recursos existentes adicionales. La identidad está administrada por la plataforma Azure y no requiere que aprovisione o rote los secretos. Para obtener más información acerca de las identidades administradas en Azure Active Directory (AAD), consulte [administra las identidades de los recursos de Azure](../active-directory/managed-identities-azure-resources/overview.md).

La aplicación puede tener dos tipos de identidades:

- Una **identidad asignada por el sistema** está asociada a la aplicación y se elimina si se elimina la aplicación. Una aplicación solo puede tener una identidad asignada por el sistema.
- Un **identidad asignada por el usuario** es un recurso de Azure que se puede asignar a la aplicación independiente. Una aplicación puede tener varias identidades asignadas por el usuario.

## <a name="how-to-use-managed-identity"></a>Cómo usar la identidad administrada

Identidad administrada permite muchos escenarios para aplicaciones administradas. Algunos escenarios comunes que pueden resolverse son:

- Implementar una aplicación administrada vinculados a los recursos de Azure existentes. Un ejemplo es implementar una máquina virtual (VM) de Azure dentro de la aplicación administrada que se adjunta a un [interfaz de red existente](../virtual-network/virtual-network-network-interface-vm.md).
- Conceder acceso a la aplicación administrada y el publicador a los recursos de Azure fuera la **grupo de recursos administrado**.
- Proporcionar una identidad de aplicaciones administradas de operativa para el registro de actividad y otros servicios dentro de Azure.

## <a name="adding-managed-identity"></a>Agregar identidad administrada

Crear una aplicación administrada con una identidad administrada requiere una propiedad adicional debe establecerse en el recurso de Azure. El ejemplo siguiente muestra un ejemplo **identidad** propiedad:

```json
{
"identity": {
    "type": "SystemAssigned, UserAssigned",
    "userAssignedIdentities": {
        "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testRG/providers/Microsoft.ManagedIdentity/userassignedidentites/myuserassignedidentity": {}
    }
}
```

Hay dos formas habituales para crear una aplicación administrada con **identidad**: [CreateUIDefinition.json](./create-uidefinition-overview.md) y [plantillas Azure Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md). Para que el único simple crear escenarios, CreateUIDefinition debe usarse para habilitar la identidad administrada, ya que proporciona una experiencia más enriquecida. Sin embargo, cuando se trabaja con avanzadas o complejas automatizadas de sistemas que requieren o se pueden usar varias implementaciones de aplicaciones administradas, las plantillas.

### <a name="using-createuidefinition"></a>Uso de CreateUIDefinition

Se puede configurar una aplicación administrada con la identidad administrada a través de la [CreateUIDefinition.json](./create-uidefinition-overview.md). En el [da como resultado de la sección](./create-uidefinition-overview.md#outputs), la clave `managedIdentity` puede usarse para reemplazar la propiedad identity de la plantilla de aplicación administrada. El siguiente ejemplo permitirá **asignado por el sistema** identidad en la aplicación administrada. Los objetos de identidad más complejos se pueden formar mediante el uso de elementos CreateUIDefinition para pedir al consumidor entradas. Estas entradas se pueden usar para construir aplicaciones administradas con **identidad asignada por el usuario**.

```json
"outputs": {
    "managedIdentity": "[parse('{\"Type\":\"SystemAssigned\"}')]"
}
```

#### <a name="when-to-use-createuidefinition-for-managed-identity"></a>Cuándo usar CreateUIDefinition para identidad administrada

A continuación se muestran algunas recomendaciones sobre cuándo usar CreateUIDefinition para habilitar la identidad administrada en aplicaciones administradas.

- La creación de aplicaciones administradas que se va a través de Azure portal o marketplace.
- La identidad administrada requiere la intervención del consumidor complejas.
- La identidad administrada es necesario en la creación de la aplicación administrada.

#### <a name="systemassigned-createuidefinition"></a>SystemAssigned CreateUIDefinition

Una instancia de CreateUIDefinition básico que habilita la identidad SystemAssigned para la aplicación administrada.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/0.1.2-preview/CreateUIDefinition.MultiVm.json#",
  "handler": "Microsoft.Azure.CreateUIDef",
  "version": "0.1.2-preview",
    "parameters": {
        "basics": [
            {}
        ],
        "steps": [
        ],
        "outputs": {
            "managedIdentity": "[parse('{\"Type\":\"SystemAssigned\"}')]"
        }
    }
}
```

#### <a name="userassigned-createuidefinition"></a>UserAssigned CreateUIDefinition

Una instancia de CreateUIDefinition básica que toma un **identidad asignada por el usuario** recursos como entrada y habilita la identidad UserAssigned para la aplicación administrada.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/0.1.2-preview/CreateUIDefinition.MultiVm.json#",
  "handler": "Microsoft.Azure.CreateUIDef",
  "version": "0.1.2-preview",
    "parameters": {
        "basics": [
            {}
        ],
        "steps": [
            {
                "name": "manageIdentity",
                "label": "Identity",
                "subLabel": {
                    "preValidation": "Manage Identities",
                    "postValidation": "Done"
                },
                "bladeTitle": "Identity",
                "elements": [
                    {
                        "name": "userAssignedText",
                        "type": "Microsoft.Common.TextBox",
                        "label": "User assigned managed identity",
                        "defaultValue": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testRG/providers/Microsoft.ManagedIdentity/userassignedidentites/myuserassignedidentity",
                        "visible": true
                    }
                ]
            }
        ],
        "outputs": {
            "managedIdentity": "[parse(concat('{\"Type\":\"UserAssigned\",\"UserAssignedIdentities\":{',string(steps('manageIdentity').userAssignedText),':{}}}'))]"
        }
    }
}
```

El CreateUIDefinition.json anterior genera una experiencia de usuario de creación que tiene un cuadro de texto para un consumidor introducir el **identidad asignada por el usuario** identificador de recurso de Azure. La experiencia generada tendría el siguiente aspecto:

![Identidad asignada por el usuario de ejemplo CreateUIDefinition](./media/publish-managed-identity/user-assigned-identity.png)

### <a name="using-azure-resource-manager-templates"></a>Uso de plantillas del Administrador de recursos de Azure

> [!NOTE]
> Las plantillas de aplicación administrada de Marketplace se generan automáticamente para los clientes que se va a través del portal de Azure, cree experiencia.
> Para estos escenarios, la `managedIdentity` clave de salida en la instancia de CreateUIDefinition debe usarse para habilitar identidad.

También se puede habilitar la identidad administrada a través de plantillas de Azure Resource Manager. El siguiente ejemplo permitirá **asignado por el sistema** identidad en la aplicación administrada. Los objetos de identidad más complejos se pueden formar mediante los parámetros de plantilla de Azure Resource Manager para proporcionar entradas. Estas entradas se pueden usar para construir aplicaciones administradas con **identidad asignada por el usuario**.

#### <a name="when-to-use-azure-resource-manager-templates-for-managed-identity"></a>Cuándo usar las plantillas de Azure Resource Manager para la identidad administrada

A continuación se muestran algunas recomendaciones sobre cuándo usar las plantillas de Azure Resource Manager para habilitar la identidad administrada en aplicaciones administradas.

- Las aplicaciones administradas se pueden implementar mediante programación basada en una plantilla.
- Asignaciones de roles personalizados para la identidad administrada son necesarios para aprovisionar la aplicación administrada.
- El flujo de creación de marketplace y de portal de Azure no es necesario en la aplicación administrada.

#### <a name="systemassigned-template"></a>Plantilla SystemAssigned

Una plantilla básica de Azure Resource Manager que implementa una aplicación administrada con **asignado por el sistema** identidad.

```json
"resources": [
    {
        "type": "Microsoft.Solutions/applications",
        "name": "[parameters('applicationName')]",
        "apiVersion": "2018-09-01-preview",
        "location": "[parameters('location')]",
        "identity": {
            "type": "SystemAssigned"
        },
        "properties": {
            "ManagedResourceGroupId": "[parameters('managedByResourceGroupId')]",
            "parameters": { }
        }
    }
]
```

### <a name="userassigned-template"></a>Plantilla UserAssigned

Una plantilla básica de Azure Resource Manager que implementa una aplicación administrada con un **identidad asignada por el usuario**.

```json
"resources": [
    {
      "type": "Microsoft.ManagedIdentity/userAssignedIdentities",
      "name": "[parameters('managedIdentityName')]",
      "apiVersion": "2018-11-30",
      "location": "[parameters('location')]"
    },
    {
        "type": "Microsoft.Solutions/applications",
        "name": "[parameters('applicationName')]",
        "apiVersion": "2018-09-01-preview",
        "location": "[parameters('location')]",
        "identity": {
            "type": "UserAssigned",
            "userAssignedIdentities": {
                "[resourceID('Microsoft.ManagedIdentity/userAssignedIdentities/',parameters('managedIdentityName'))]": {}
            }
        },
        "properties": {
            "ManagedResourceGroupId": "[parameters('managedByResourceGroupId')]",
            "parameters": { }
        }
    }
]
```

## <a name="granting-access-to-azure-resources"></a>Conceder acceso a recursos de Azure

Una vez que una aplicación administrada se concede una identidad, se puede conceder acceso a recursos de azure existentes. Este proceso puede realizarse a través de la interfaz de Access control (IAM) en el portal de Azure. El nombre de la aplicación administrada o **identidad asignada por el usuario** se pueden buscar para agregar una asignación de roles.

![Agregar asignación de roles para aplicaciones administradas](./media/publish-managed-identity/identity-role-assignment.png)

## <a name="linking-existing-azure-resources"></a>Vinculación de recursos de Azure existentes

> [!NOTE]
> Un **identidad asignada por el usuario** debe ser [configurado](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal.md) antes de implementar la aplicación administrada. Además, los recursos vinculados de implementación de aplicaciones administradas solo se admite para la **marketplace** tipo.

Identidad administrada también puede utilizarse para implementar una aplicación administrada que requiere acceso a los recursos existentes durante su implementación. Cuando se aprovisiona la aplicación administrada por el cliente, **las identidades asignadas por el usuario** pueden agregarse para ofrecer autorizaciones adicionales a la **maintemplate se** implementación.

### <a name="authoring-the-createuidefinition-with-a-linked-resource"></a>Creación de la instancia de CreateUIDefinition con un recurso vinculado

Cuando se vincula la implementación de la aplicación administrada a los recursos existentes, tanto el recurso de Azure existente y un **identidad asignada por el usuario** con el rol aplicable se debe proporcionar la asignación en dicho recurso.

 Un ejemplo de CreateUIDefinition que requiere dos entradas: un identificador de recurso de interfaz de red y un identificador de recurso de identidad asignada por el usuario.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/0.1.2-preview/CreateUIDefinition.MultiVm.json#",
    "handler": "Microsoft.Compute.MultiVm",
    "version": "0.1.2-preview",
    "parameters": {
        "basics": [
            {}
        ],
        "steps": [
            {
                "name": "managedApplicationSetting",
                "label": "Managed Application Settings",
                "subLabel": {
                    "preValidation": "Managed Application Settings",
                    "postValidation": "Done"
                },
                "bladeTitle": "Managed Application Settings",
                "elements": [
                    {
                        "name": "networkInterfaceId",
                        "type": "Microsoft.Common.TextBox",
                        "label": "network interface resource id",
                        "defaultValue": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testRG/providers/Microsoft.Network/networkInterfaces/existingnetworkinterface",
                        "toolTip": "Must represent the identity as an Azure Resource Manager resource identifer format ex. /subscriptions/sub1/resourcegroups/myGroup/providers/Microsoft.Network/networkInterfaces/networkinterface1",
                        "visible": true
                    },
                    {
                        "name": "userAssignedId",
                        "type": "Microsoft.Common.TextBox",
                        "label": "user assigned identity resource id",
                        "defaultValue": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testRG/providers/Microsoft.ManagedIdentity/userassignedidentites/myuserassignedidentity",
                        "toolTip": "Must represent the identity as an Azure Resource Manager resource identifer format ex. /subscriptions/sub1/resourcegroups/myGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/identity1",
                        "visible": true
                    }
                ]
            }
        ],
        "outputs": {
            "existingNetworkInterfaceId": "[steps('managedApplicationSetting').networkInterfaceId]",
            "managedIdentity": "[parse(concat('{\"Type\":\"UserAssigned\",\"UserAssignedIdentities\":{',string(steps('managedApplicationSetting').userAssignedId),':{}}}'))]"
        }
    }
}
```

Este CreateUIDefinition.json genera una experiencia de usuario de creación que tiene dos campos. El primer campo permite al usuario que escriba en el identificador de recurso de Azure para el recurso que se vincula a la implementación de aplicación administrada. La segunda es para que un consumidor introducir el **identidad asignada por el usuario** identificador de recursos de Azure, que tiene acceso al recurso de Azure vinculado. La experiencia generada tendría el siguiente aspecto:

![CreateUIDefinition de ejemplo con dos entradas: una interfaz de red, identificador de recurso y un identificador de recurso de identidad asignada por el usuario](./media/publish-managed-identity/network-interface-cuid.png)

### <a name="authoring-the-maintemplate-with-a-linked-resource"></a>Creación en maintemplate se con un recurso vinculado

Además de actualizar la instancia de CreateUIDefinition, la plantilla principal también debe actualizarse para que acepte pasado el identificador de recurso vinculado. La plantilla principal puede actualizarse para que acepte la nueva salida mediante la adición de un nuevo parámetro. Puesto que el `managedIdentity` salida invalida el valor en la plantilla de aplicación administrada generada, no se pasa a la plantilla principal y no deben incluirse en la sección de parámetros.

Una plantilla principal de ejemplo que establece el perfil de red a una interfaz de red existente proporcionada por la instancia de CreateUIDefinition.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
      "existingNetworkInterfaceId": { "type": "string" }
    },
    "variables": {
    },
    "resources": [
        {
            "apiVersion": "2016-04-30-preview",
            "type": "Microsoft.Compute/virtualMachines",
            "name": "myLinkedResourceVM",
            "location": "[resourceGroup().location]",
            "properties": {
                …,
                "networkProfile": {
                    "networkInterfaces": [
                        {
                            "id": "[parameters('existingNetworkInterfaceId')]"
                        }
                    ]
                }
            }
        }
    ]
}
```

### <a name="consuming-the-managed-application-with-a-linked-resource"></a>Consumo de la aplicación administrada con un recurso vinculado

Una vez creado el paquete de aplicación administrada, la aplicación administrada se puede consumir a través del portal de Azure. Antes de que se puede consumir, hay varios pasos de requisitos previos.

- Debe crearse una instancia del recurso de Azure vinculado necesario.
- El **identidad asignada por el usuario** debe ser [crea y proporciona las asignaciones de roles](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal.md) del recurso vinculado.
- Los vínculos existentes Id. de recurso y el **identidad asignada por el usuario** ID se proporcionan para la instancia de CreateUIDefinition.

## <a name="accessing-the-managed-identity-token"></a>Obtener acceso a lo token de identidad administrada

Ahora se puede acceder el token de la aplicación administrada mediante el `listTokens` api desde el inquilino del publicador. Una solicitud de ejemplo podría ser similar:

``` HTTP
POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroup}/providers/Microsoft.Solutions/applications/{applicationName}?api-version=2018-09-01-preview HTTP/1.1
```

Una respuesta de ejemplo podría ser similar:

``` HTTP
HTTP/1.1 200 OK
Content-Type: application/json

{
    "value": [
        {
            "access_token": "eyJ0eXAi…",
            "expires_in": "2…",
            "expires_on": "1557…",
            "not_before": "1557…",
            "authorizationAudience": "https://management.azure.com/",
            "resourceId": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroup}/providers/Microsoft.Solutions/applications/{applicationName}",
            "token_type": "Bearer"
        }
    ]
}
```

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Cómo configurar una aplicación administrada con un proveedor personalizado](./custom-providers-overview.md)