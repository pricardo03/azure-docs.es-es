---
title: 'Rol de acceso al controlador personalizado: Avere vFXT for Azure'
description: Cómo crear un rol de acceso personalizado para el controlador de clústeres de Avere vFXT
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 01/29/2019
ms.author: v-erkell
ms.openlocfilehash: c408efa7ca01928e25ac03f5ca63d0aef7d88839
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/12/2019
ms.locfileid: "57770050"
---
# <a name="customized-controller-access-role"></a>Rol de acceso al controlador personalizado

El controlador de clústeres de Avere vFXT for Azure usa una identidad administrada y el control de acceso basado en rol (RBAC) para permitirle crear y administrar el clúster. 

De manera predeterminada, se asigna el [rol de propietario integrado](../role-based-access-control/built-in-roles.md#owner) al controlador de clústeres. Además, el acceso del controlador se limita a su grupo de recursos; no puede modificar los elementos que estén fuera del grupo de recursos del clúster.

En este artículo se explica cómo crear su propio rol de acceso para el controlador de clústeres en lugar de usar la configuración predeterminada. 

## <a name="edit-the-role-prototype"></a>Edición del prototipo de rol

Inicie desde el rol prototipo disponible en <https://github.com/Azure/Avere/blob/master/src/vfxt/src/roles/AvereContributor.txt>.

```json
{
  "AssignableScopes": [
    "/subscriptions/YOUR SUBSCRIPTION ID HERE"
  ],
  "Name": "Avere custom contributor",
  "IsCustom": true,
  "Description": "Can create and manage an Avere vFXT cluster.",
  "NotActions": [],
  "Actions": [
    "Microsoft.Authorization/*/read",
    "Microsoft.Compute/*/read",
    "Microsoft.Compute/availabilitySets/*",
    "Microsoft.Compute/virtualMachines/*",
    "Microsoft.Compute/disks/*",
    "Microsoft.Insights/alertRules/*",
    "Microsoft.Network/*/read",
    "Microsoft.Network/networkInterfaces/*",
    "Microsoft.Network/virtualNetworks/subnets/join/action",
    "Microsoft.Network/virtualNetworks/subnets/read",
    "Microsoft.Resources/deployments/*",
    "Microsoft.Resources/subscriptions/resourceGroups/read",
    "Microsoft.Resources/subscriptions/resourceGroups/resources/read",
    "Microsoft.Storage/*/read",
    "Microsoft.Storage/storageAccounts/listKeys/action",
    "Microsoft.Support/*"
  ],
  "DataActions": []
}
```

Agregue el identificador de suscripción para la implementación de Avere vFXT for Azure en la instrucción AssignableScopes. Personalice el nombre y agregue o modifique las definiciones según sea necesario. 

Tenga cuidado si restringe privilegios. La creación de clústeres puede producir un error si el controlador no tiene suficientes permisos de acceso. 

Para obtener ayuda sobre los privilegios que el controlador de clústeres necesita para crear un clúster, [abra una incidencia de soporte técnico](avere-vfxt-open-ticket.md#open-a-support-ticket-for-your-avere-vfxt). 

Guarde la definición de roles personalizados como archivo JSON. 

## <a name="define-the-role"></a>Definición del rol 

Siga estos pasos para agregar la definición de roles personalizados a la suscripción. 

1. Abra Azure Cloud Shell en Azure Portal o vaya a [https://shell.azure.com](https://shell.azure.com).

1. Use el comando de la CLI de Azure para cambiar a su suscripción de vFXT:

   ```azurecli
   az account set --subscription YOUR_SUBSCRIPTION_ID
   ```

1. Cree el rol:

   ```azurecli
   az role definition create --role-definition /avere-contributor-custom.json
   ```

   Use el nombre de archivo y la ruta de acceso en lugar de ```/avere-contributor-custom.json``` en este ejemplo. 

Guarde la salida del comando de definición de roles; contiene el identificador de rol que deberá suministrar para la plantilla de creación del clúster. 

## <a name="find-the-role-id"></a>Búsqueda del identificador de rol

La plantilla de implementación de Avere vFXT necesita el identificador único global (GUID) del rol para asignar al controlador un rol personalizado. 

El GUID del rol es una cadena de 32 caracteres con este formato: 8e3af657-a8ff-443c-a75c-2fe8c4bcb635

Para buscar el GUID del rol, use este comando con el nombre del rol en el parámetro ```--name```.

```azurecli
az role definition list --query '[*].{roleName:roleName, name:name}' -o table --name 'YOUR ROLE NAME'
```
Escriba esta cadena en el campo **Avere cluster create role ID** (Identificador del rol de creación del clúster de Avere) al implementar Avere vFXT for Azure.

## <a name="next-steps"></a>Pasos siguientes

Obtenga información sobre cómo implementar Avere vFXT for Azure en [Implementación del clúster de vFXT](avere-vfxt-deploy.md).
