---
title: Implementación de una directiva que se pueda corregir
description: Obtenga información sobre cómo incorporar un cliente a la administración de recursos delegados de Azure, lo que permite administrar sus recursos y acceder a ellos desde su propio inquilino.
ms.date: 10/11/2019
ms.topic: overview
ms.openlocfilehash: 662daeb305856fb36bfb84f98e80bedf48b22756
ms.sourcegitcommit: 2d3740e2670ff193f3e031c1e22dcd9e072d3ad9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/16/2019
ms.locfileid: "74132476"
---
# <a name="deploy-a-policy-that-can-be-remediated-within-a-delegated-subscription"></a>Implementación de una directiva que se pueda corregir en una suscripción delegada

[Azure Lighthouse](../overview.md) permite a los proveedores de servicios crear y editar definiciones de directivas en una suscripción delegada. Sin embargo, para implementar directivas que usen una [tarea de corrección](https://docs.microsoft.com/azure/governance/policy/how-to/remediate-resources) (es decir, directivas con[deployIfNotExists ](https://docs.microsoft.com/azure/governance/policy/concepts/effects#deployifnotexists) o el efecto [Modify](https://docs.microsoft.com/azure/governance/policy/concepts/effects#modify)), deberá crear una [identidad administrada](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview) en el inquilino del cliente. Azure Policy puede usar esta identidad administrada para implementar la plantilla dentro de la directiva. Para habilitar este escenario hay varios pasos obligatorios, tanto al incorporar el cliente para la administración de recursos delegados de Azure como al implementar la propia directiva.

## <a name="create-a-user-who-can-assign-roles-to-a-managed-identity-in-the-customer-tenant"></a>Creación de un usuario que pueda asignar roles a una identidad administrada en el inquilino del cliente

Al incorporar un cliente para la administración de recursos delegados de Azure, use una [plantilla de Azure Resource Manager](https://docs.microsoft.com/azure/lighthouse/how-to/onboard-customer#create-an-azure-resource-manager-template), junto con un archivo de parámetros que define los usuarios, grupos de usuarios y entidades de servicio en el inquilino de administración que podrán acceder a los recursos delegados en el inquilino del cliente. En el archivo de parámetros, a cada uno de estos usuarios (**principalId**) se le asigna un [rol integrado](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles) (**roleDefinitionId**) que define el nivel de acceso.

Para permitir que **principalId**  cree una identidad administrada en el inquilino del cliente, debe establecer su **roleDefinitionId**  en **Administrador de acceso de usuario**. Aunque por lo general este rol no se admite, se puede usar en este escenario concreto, lo que permite a los usuarios con este permiso asignar uno o varios roles integrados específicos a identidades administradas. Estos roles se definen en la propiedad **delegatedRoleDefinitionIds** . Aquí puede incluir todos los roles integrados, excepto Administrador de acceso de usuario o Propietario.

Una vez que se incorpora el cliente, el **principalId**  creado en esta autorización podrá asignar estos roles integrados a identidades administradas en el inquilino del cliente. Sin embargo, no tendrán ningún otro permiso asociado normalmente al rol Administrador de acceso de usuario.

En el ejemplo siguiente se muestra un **principalId**  que tendrá el rol Administrador de acceso de usuario. Este usuario podrá asignar dos roles integrados a identidades administradas en el inquilino del cliente: Colaborador y Colaborador de Log Analytics.

```json
{
    "principalId": "3kl47fff-5655-4779-b726-2cf02b05c7c4",
    "principalIdDisplayName": "Policy Automation Account",
    "roleDefinitionId": "18d7d88d-d35e-4fb5-a5c3-7773c20a72d9",
    "delegatedRoleDefinitionIds": [
         "b24988ac-6180-42a0-ab88-20f7382dd24c",
         "92aaf0da-9dab-42b6-94a3-d43ce8d16293"
    ]
}
```

## <a name="deploy-policies-that-can-be-remediated"></a>Implementación de directivas que se pueden corregir

Una vez que haya creado el usuario con los permisos necesarios, tal y como se ha descrito anteriormente, el usuario puede implementar en el inquilino del cliente directivas que usan tareas de corrección.

Por ejemplo, supongamos que desea habilitar diagnósticos en los recursos de Azure Key Vault en el inquilino del cliente, tal como se muestra en este [ejemplo ](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/Azure-Delegated-Resource-Management/templates/policy-enforce-keyvault-monitoring). Un usuario del inquilino de administración con los permisos adecuados (como se ha descrito anteriormente) implementaría una [plantilla de Azure Resource Manager](https://github.com/Azure/Azure-Lighthouse-samples/blob/master/Azure-Delegated-Resource-Management/templates/policy-enforce-keyvault-monitoring/enforceAzureMonitoredKeyVault.json) para habilitar este escenario.

Tenga en cuenta que la creación de la asignación de directiva que se va a usar con una suscripción delegada debe realizarse actualmente a través de las API, no en Azure Portal. Al hacerlo, **apiVersion** se debe establecer en **2019-04-01-preview**, que incluye la nueva propiedad **delegatedManagedIdentityResourceId**. Esta propiedad permite incluir una identidad administrada que reside en el inquilino del cliente (en una suscripción o un grupo de recursos que se ha incorporado a la administración de recursos delegados de Azure).

En el ejemplo siguiente se muestra una asignación de roles con **delegatedManagedIdentityResourceId**.

```json
"type": "Microsoft.Authorization/roleAssignments",
            "apiVersion": "2019-04-01-preview",
            "name": "[parameters('rbacGuid')]",
            "dependsOn": [
                "[variables('policyAssignment')]"
            ],
            "properties": {
                "roleDefinitionId": "[concat(subscription().id, '/providers/Microsoft.Authorization/roleDefinitions/', variables('rbacContributor'))]",
                "principalType": "ServicePrincipal",
                "delegatedManagedIdentityResourceId": "[concat(subscription().id, '/providers/Microsoft.Authorization/policyAssignments/', variables('policyAssignment'))]",
                "principalId": "[toLower(reference(concat('/providers/Microsoft.Authorization/policyAssignments/', variables('policyAssignment')), '2018-05-01', 'Full' ).identity.principalId)]"
            }
```

> [!TIP]
> Hay disponible un [ejemplo similar](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/Azure-Delegated-Resource-Management/templates/policy-add-or-replace-tag) en el que se muestra cómo implementar una directiva que agrega o elimina una etiqueta (mediante el efecto Modify) en una suscripción delegada.

## <a name="next-steps"></a>Pasos siguientes

- Más información acerca de [Azure Policy](https://docs.microsoft.com/azure/governance/policy/).
- Más información acerca de las [identidades administradas de recursos de Azure](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview).
