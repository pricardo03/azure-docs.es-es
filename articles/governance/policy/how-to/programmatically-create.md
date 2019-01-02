---
title: Creación de directivas mediante programación y visualización de datos de cumplimiento
description: Este artículo le guiará a través de la creación y administración de directivas para Azure Policy mediante programación.
services: azure-policy
author: DCtheGeek
ms.author: dacoulte
ms.date: 12/06/2018
ms.topic: conceptual
ms.service: azure-policy
manager: carmonm
ms.custom: seodec18
ms.openlocfilehash: 3c8fd185feff9a580e2d23926dcf60cb33121122
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2018
ms.locfileid: "53312483"
---
# <a name="programmatically-create-policies-and-view-compliance-data"></a>Creación de directivas mediante programación y visualización de datos de cumplimiento

Este artículo le guiará a través de la creación y administración de directivas mediante programación. Las definiciones de directivas aplican reglas y efectos diferentes sobre los recursos. Su aplicación garantiza que los recursos cumplan los estándares corporativos y los acuerdos de nivel de servicio.

Para información sobre el cumplimiento, consulte cómo [obtener datos de cumplimiento](getting-compliance-data.md).

## <a name="prerequisites"></a>Requisitos previos

Asegúrese de que se cumplen los siguientes requisitos previos antes de empezar:

1. Si aún no lo ha hecho, instale [ARMClient](https://github.com/projectkudu/ARMClient). Esta es una herramienta que envía solicitudes HTTP a las API basadas en Azure Resource Manager.

1. Actualice el módulo AzureRM de PowerShell a la versión más reciente. Para más información acerca de la versión más reciente, consulte [Azure PowerShell](https://github.com/Azure/azure-powershell/releases).

1. Registre el proveedor de recursos de Policy Insights con Azure PowerShell para validar que la suscripción funciona con el proveedor de recursos. Para registrar un proveedor de recursos, debe tener permiso para ejecutar la operación de registro de una acción para él. Esta operación está incluida en los roles Colaborador y Propietario. Para registrar el proveedor de recursos, ejecute el siguiente comando:

   ```azurepowershell-interactive
   Register-AzureRmResourceProvider -ProviderNamespace 'Microsoft.PolicyInsights'
   ```

   Para más información acerca del registro y la visualización de los proveedores de recursos, consulte [Tipos y proveedores de recursos](../../../azure-resource-manager/resource-manager-supported-services.md).

1. Si aún no lo ha hecho, instale la CLI de Azure. Puede obtener la versión más reciente en [Instalación de la CLI de Azure en Windows](/cli/azure/install-azure-cli-windows).

## <a name="create-and-assign-a-policy-definition"></a>Creación y asignación de una definición de directiva

El primer paso hacia una mejor visibilidad de los recursos es crear y asignar directivas sobre los recursos. El siguiente paso es aprender a crear y asignar una directiva mediante programación. La directiva de ejemplo audita cuentas de almacenamiento abiertas a todas las redes públicas con PowerShell, la CLI de Azure y solicitudes HTTP.

### <a name="create-and-assign-a-policy-definition-with-powershell"></a>Creación y asignación de una definición de directiva con PowerShell

1. Utilice el siguiente fragmento JSON para crear un archivo JSON con el nombre AuditStorageAccounts.json.

   ```json
   {
       "if": {
           "allOf": [{
                   "field": "type",
                   "equals": "Microsoft.Storage/storageAccounts"
               },
               {
                   "field": "Microsoft.Storage/storageAccounts/networkAcls.defaultAction",
                   "equals": "Allow"
               }
           ]
       },
       "then": {
           "effect": "audit"
       }
   }
   ```

   Para más información acerca de la creación de una definición de directiva, consulte [Estructura de definición de Azure Policy](../concepts/definition-structure.md).

1. Ejecute el siguiente comando para crear una definición de directiva mediante el archivo AuditStorageAccounts.json.

   ```azurepowershell-interactive
   New-AzureRmPolicyDefinition -Name 'AuditStorageAccounts' -DisplayName 'Audit Storage Accounts Open to Public Networks' -Policy 'AuditStorageAccounts.json'
   ```

   El comando crea una definición de directiva denominada _Audit Storage Accounts Open to Public Networks_ (Auditoría de cuentas de almacenamiento abiertas a las redes públicas).
   Para más información acerca de otros parámetros que puede utilizar, consulte [New-AzureRmPolicyDefinition](/powershell/module/azurerm.resources/new-azurermpolicydefinition).

   Cuando se llama sin parámetros de ubicación, `New-AzureRmPolicyDefinition` elige de forma predeterminada guardar la definición de directiva en la suscripción seleccionada del contexto de sesiones. Para guardar la definición en una ubicación diferente, use los siguientes parámetros:

   - **SubscriptionId**: se guarda en una suscripción diferente. Requiere un valor de _GUID_.
   - **ManagementGroupName**: se guarda en un grupo de administración. Requiere un valor de _cadena_.

1. Después de crear la definición de directiva, puede crear una asignación de directiva mediante la ejecución de los siguientes comandos:

   ```azurepowershell-interactive
   $rg = Get-AzureRmResourceGroup -Name 'ContosoRG'
   $Policy = Get-AzureRmPolicyDefinition -Name 'AuditStorageAccounts'
   New-AzureRmPolicyAssignment -Name 'AuditStorageAccounts' -PolicyDefinition $Policy -Scope $rg.ResourceId
   ```

   Reemplace _ContosoRG_ por el nombre del grupo de recursos que desee.

   El parámetro **Scope** en `New-AzureRmPolicyAssignment` también funciona con las suscripciones y los grupos de administración. El parámetro utiliza una ruta de acceso de recurso completo, que devuelve la propiedad **ResourceId** en `Get-AzureRmResourceGroup`. El patrón de **Scope** para cada contenedor es como sigue.
   Reemplace `{rgName}`, `{subId}` y `{mgName}` con el nombre del grupo de recursos, el identificador de suscripción y el nombre del grupo de administración, respectivamente.

   - Grupo de recursos `/subscriptions/{subId}/resourceGroups/{rgName}`
   - Suscripción `/subscriptions/{subId}/`
   - Grupo de administración `/providers/Microsoft.Management/managementGroups/{mgName}`

Para más información acerca de cómo administrar las directivas de recursos mediante el módulo de PowerShell de Azure Resource Manager, consulte [AzureRM.Resources](/powershell/module/azurerm.resources/#policies).

### <a name="create-and-assign-a-policy-definition-using-armclient"></a>Creación y asignación de una definición de directiva con ARMClient

Use el procedimiento siguiente para crear una definición de directiva.

1. Copie el siguiente fragmento de código JSON para crear un archivo JSON. En el paso siguiente llamará al archivo.

   ```json
   "properties": {
       "displayName": "Audit Storage Accounts Open to Public Networks",
       "policyType": "Custom",
       "mode": "Indexed",
       "description": "This policy ensures that storage accounts with exposure to Public Networks are audited.",
       "parameters": {},
       "policyRule": {
           "if": {
               "allOf": [{
                       "field": "type",
                       "equals": "Microsoft.Storage/storageAccounts"
                   },
                   {
                       "field": "Microsoft.Storage/storageAccounts/networkAcls.defaultAction",
                       "equals": "Allow"
                   }
               ]
           },
           "then": {
               "effect": "audit"
           }
       }
   }
   ```

1. Cree la definición de directiva con una de las llamadas siguientes:

   ```
   # For defining a policy in a subscription
   armclient PUT "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/policyDefinitions/AuditStorageAccounts?api-version=2016-12-01" @<path to policy definition JSON file>

   # For defining a policy in a management group
   armclient PUT "/providers/Microsoft.Management/managementgroups/{managementGroupId}/providers/Microsoft.Authorization/policyDefinitions/AuditStorageAccounts?api-version=2016-12-01" @<path to policy definition JSON file>
   ```

   Sustituya el valor de {subscriptionId} anterior por el identificador de su suscripción o {managementGroupId} por el identificador de su [grupo de administración](../../management-groups/overview.md).

   Para más información acerca de la estructura de la consulta, consulte [Policy Definitions – Create or Update](/rest/api/resources/policydefinitions/createorupdate) (Definiciones de directiva: Creación o actualización) y [Policy Definitions – Create or Update At Management Group](/rest/api/resources/policydefinitions/createorupdateatmanagementgroup) (Definiciones de directiva: Creación o actualización en el grupo de administración).

Utilice el procedimiento siguiente para crear una asignación de directiva y asignar la definición de directiva en el nivel de grupo de recursos.

1. Copie el siguiente fragmento de código JSON para crear un archivo de asignación de directiva JSON. Reemplace la información de ejemplo de los símbolos &lt;&gt; por sus propios valores.

   ```json
   {
       "properties": {
           "description": "This policy assignment makes sure that storage accounts with exposure to Public Networks are audited.",
           "displayName": "Audit Storage Accounts Open to Public Networks Assignment",
           "parameters": {},
           "policyDefinitionId": "/subscriptions/<subscriptionId>/providers/Microsoft.Authorization/policyDefinitions/Audit Storage Accounts Open to Public Networks",
           "scope": "/subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>"
       }
   }
   ```

1. Cree la asignación de directiva con la llamada siguiente:

   ```
   armclient PUT "/subscriptions/<subscriptionID>/resourceGroups/<resourceGroupName>/providers/Microsoft.Authorization/policyAssignments/Audit Storage Accounts Open to Public Networks?api-version=2017-06-01-preview" @<path to Assignment JSON file>
   ```

   Reemplace la información de ejemplo de los símbolos &lt;&gt; por sus propios valores.

   Para más información acerca de cómo realizar llamadas HTTP a la API de REST, consulte [Recursos de la API de REST de Azure](/rest/api/resources/).

### <a name="create-and-assign-a-policy-definition-with-azure-cli"></a>Creación y asignación de una definición de directiva con la CLI de Azure

Use el procedimiento siguiente para crear una definición de directiva:

1. Copie el siguiente fragmento de código JSON para crear un archivo de asignación de directiva JSON.

  ```json
  {
      "if": {
          "allOf": [{
                  "field": "type",
                  "equals": "Microsoft.Storage/storageAccounts"
              },
              {
                  "field": "Microsoft.Storage/storageAccounts/networkAcls.defaultAction",
                  "equals": "Allow"
              }
          ]
      },
      "then": {
          "effect": "audit"
      }
  }
  ```

1. Para crear una definición de directiva, ejecute el siguiente comando:

   ```azurecli-interactive
   az policy definition create --name 'audit-storage-accounts-open-to-public-networks' --display-name 'Audit Storage Accounts Open to Public Networks' --description 'This policy ensures that storage accounts with exposures to public networks are audited.' --rules '<path to json file>' --mode All
   ```

1. Use el siguiente comando para crear una asignación de directiva. Reemplace la información de ejemplo de los símbolos &lt;&gt; por sus propios valores.

   ```azurecli-interactive
   az policy assignment create --name '<name>' --scope '<scope>' --policy '<policy definition ID>'
   ```

Puede obtener el identificador de definición de directiva si usa PowerShell con el comando siguiente:

```azurecli-interactive
az policy definition show --name 'Audit Storage Accounts with Open Public Networks'
```

El identificador de definición de directiva para la definición de directiva que ha creado debe parecerse al ejemplo siguiente:

```
"/subscription/<subscriptionId>/providers/Microsoft.Authorization/policyDefinitions/Audit Storage Accounts Open to Public Networks"
```

Para más información acerca de cómo administrar las directivas de recursos con la CLI de Azure, consulte las [directivas de recursos de la CLI de Azure](/cli/azure/policy?view=azure-cli-latest).

## <a name="next-steps"></a>Pasos siguientes

Revise los artículos siguientes para más información sobre los comandos y las consultas en este artículo.

- [Recursos de la API de REST de Azure](/rest/api/resources/)
- [Módulos de Azure RM PowerShell](/powershell/module/azurerm.resources/#policies)
- [Comandos de directiva de la CLI de Azure](/cli/azure/policy?view=azure-cli-latest)
- [Referencia de API de REST de proveedor de recursos de Policy Insights](/rest/api/policy-insights)
- [Organización de los recursos con grupos de administración de Azure](../../management-groups/overview.md)