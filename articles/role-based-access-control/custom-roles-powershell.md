---
title: Creación de roles personalizados con Azure PowerShell | Microsoft Docs
description: Obtenga información sobre cómo crear roles para el control de acceso basado en roles (RBAC) con Azure PowerShell. Esto incluye cómo enumerar, crear, actualizar y eliminar roles personalizados.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.assetid: 9e225dba-9044-4b13-b573-2f30d77925a9
ms.service: role-based-access-control
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/20/2018
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: c8e5f34bb6b38a3f187d86a1ebc0c7019c7f1046
ms.sourcegitcommit: e0834ad0bad38f4fb007053a472bde918d69f6cb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37437025"
---
# <a name="create-custom-roles-using-azure-powershell"></a>Creación de roles personalizados con Azure PowerShell

Si los [roles integrados](built-in-roles.md) no cumplen las necesidades específicas de su organización, puede crear sus propios roles personalizados. En este artículo se describe cómo crear y administrar roles personalizados con Azure PowerShell.

## <a name="prerequisites"></a>requisitos previos

Para crear roles personalizados, necesita:

- Permisos para crear roles personalizados, como [Propietario](built-in-roles.md#owner) o [Administrador de acceso de usuarios](built-in-roles.md#user-access-administrator)
- [Azure PowerShell](/powershell/azure/install-azurerm-ps) instalado localmente

## <a name="list-custom-roles"></a>Lista de roles personalizados

Para enumerar los roles que están disponibles para la asignación en un ámbito, use el comando [Get-AzureRmRoleDefinition](/powershell/module/azurerm.resources/get-azurermroledefinition). En el ejemplo siguiente se enumeran todos los roles disponibles para la asignación en la suscripción seleccionada.

```azurepowershell
Get-AzureRmRoleDefinition | FT Name, IsCustom
```

```Example
Name                                              IsCustom
----                                              --------
Virtual Machine Operator                              True
AcrImageSigner                                       False
AcrQuarantineReader                                  False
AcrQuarantineWriter                                  False
API Management Service Contributor                   False
...
```

En el ejemplo siguiente se enumeran solo los roles personalizados disponibles para la asignación en la suscripción seleccionada.

```azurepowershell
Get-AzureRmRoleDefinition | ? {$_.IsCustom -eq $true} | FT Name, IsCustom
```

```Example
Name                     IsCustom
----                     --------
Virtual Machine Operator     True
```

Si la suscripción seleccionada no se encuentra en la `AssignableScopes` del rol, el rol personalizado no se incluye.

## <a name="create-a-custom-role"></a>Crear un rol personalizado

Para crear un rol personalizado, use el comando [New-AzureRmRoleDefinition](/powershell/module/azurerm.resources/new-azurermroledefinition). Existen dos métodos para estructurar el rol: por medio de `PSRoleDefinition` o una plantilla JSON. 

### <a name="get-operations-for-a-resource-provider"></a>Obtención de operaciones para un proveedor de recursos

A la hora de crear roles personalizados, es importante conocer todas las operaciones posibles de los proveedores de recursos.
Puede ver la lista de [operaciones de proveedor de recursos](resource-provider-operations.md) o puede usar el comando [Get AzureRMProviderOperation](/powershell/module/azurerm.resources/get-azurermprovideroperation) para obtener esta información.
Por ejemplo, si quiere comprobar todas las operaciones disponibles para las máquinas virtuales, use este comando:

```azurepowershell
Get-AzureRMProviderOperation <operation> | FT OperationName, Operation, Description -AutoSize
```

```Example
PS C:\> Get-AzureRMProviderOperation "Microsoft.Compute/virtualMachines/*" | FT OperationName, Operation, Description -AutoSize

OperationName                                  Operation                                                      Description
-------------                                  ---------                                                      -----------
Get Virtual Machine                            Microsoft.Compute/virtualMachines/read                         Get the propertie...
Create or Update Virtual Machine               Microsoft.Compute/virtualMachines/write                        Creates a new vir...
Delete Virtual Machine                         Microsoft.Compute/virtualMachines/delete                       Deletes the virtu...
Start Virtual Machine                          Microsoft.Compute/virtualMachines/start/action                 Starts the virtua...
...
```

### <a name="create-a-custom-role-with-the-psroledefinition-object"></a>Creación de un rol personalizado con el objeto PSRoleDefinition

Al usar PowerShell para crear un rol personalizado, puede usar uno de los [roles integrados](built-in-roles.md) como punto de partida o puede empezar desde cero. El primer ejemplo de esta sección empieza con un rol integrado y, luego, se personaliza con más permisos. Edite los atributos para agregar cualquier elemento `Actions`, `NotActions` o `AssignableScopes` que quiera y guarde los cambios como un nuevo rol.

El ejemplo siguiente se inicia con el rol integrado [Colaborador de la máquina virtual](built-in-roles.md#virtual-machine-contributor) para crear un rol personalizado denominado *Operador de máquina virtual*. El nuevo rol concede acceso a todas las operaciones de lectura de los proveedores de recursos *Microsoft.Compute*, *Microsoft.Storage* y *Microsoft.Network*, y concede acceso para iniciar, reiniciar y supervisar las máquinas virtuales. El rol personalizado se puede usar en dos suscripciones.

```azurepowershell
$role = Get-AzureRmRoleDefinition "Virtual Machine Contributor"
$role.Id = $null
$role.Name = "Virtual Machine Operator"
$role.Description = "Can monitor and restart virtual machines."
$role.Actions.Clear()
$role.Actions.Add("Microsoft.Storage/*/read")
$role.Actions.Add("Microsoft.Network/*/read")
$role.Actions.Add("Microsoft.Compute/*/read")
$role.Actions.Add("Microsoft.Compute/virtualMachines/start/action")
$role.Actions.Add("Microsoft.Compute/virtualMachines/restart/action")
$role.Actions.Add("Microsoft.Authorization/*/read")
$role.Actions.Add("Microsoft.Resources/subscriptions/resourceGroups/read")
$role.Actions.Add("Microsoft.Insights/alertRules/*")
$role.Actions.Add("Microsoft.Support/*")
$role.AssignableScopes.Clear()
$role.AssignableScopes.Add("/subscriptions/00000000-0000-0000-0000-000000000000")
$role.AssignableScopes.Add("/subscriptions/11111111-1111-1111-1111-111111111111")
New-AzureRmRoleDefinition -Role $role
```

En el ejemplo siguiente muestra otra forma de crear el rol personalizado de *Operador de máquina virtual*. Se inicia al crear un nuevo objeto `PSRoleDefinition`. Las operaciones de acción se especifican en la variable `perms` y se establecen en la propiedad `Actions`. La propiedad `NotActions` se establece al leer `NotActions` desde el rol integrado [Colaborador de la máquina virtual](built-in-roles.md#virtual-machine-contributor). Puesto que el [colaborador de la máquina Virtual](built-in-roles.md#virtual-machine-contributor) no tiene ningún `NotActions`, esta línea no es necesaria, pero muestra cómo se puede recuperar la información desde otro rol.

```azurepowershell
$role = [Microsoft.Azure.Commands.Resources.Models.Authorization.PSRoleDefinition]::new()
$role.Name = 'Virtual Machine Operator 2'
$role.Description = 'Can monitor and restart virtual machines.'
$role.IsCustom = $true
$perms = 'Microsoft.Storage/*/read','Microsoft.Network/*/read','Microsoft.Compute/*/read'
$perms += 'Microsoft.Compute/virtualMachines/start/action','Microsoft.Compute/virtualMachines/restart/action'
$perms += 'Microsoft.Authorization/*/read','Microsoft.Resources/subscriptions/resourceGroups/read'
$perms += 'Microsoft.Insights/alertRules/*','Microsoft.Support/*'
$role.Actions = $perms
$role.NotActions = (Get-AzureRmRoleDefinition -Name 'Virtual Machine Contributor').NotActions
$subs = '/subscriptions/00000000-0000-0000-0000-000000000000','/subscriptions/11111111-1111-1111-1111-111111111111'
$role.AssignableScopes = $subs
New-AzureRmRoleDefinition -Role $role
```

### <a name="create-a-custom-role-with-json-template"></a>Creación de un rol personalizado con una plantilla JSON

Se puede usar una plantilla JSON como definición de origen para el rol personalizado. En el ejemplo siguiente se crea un rol personalizado que permite el acceso de lectura a recursos de almacenamiento y proceso y el acceso a la asistencia técnica, y además agrega ese rol a dos suscripciones. Cree un archivo `C:\CustomRoles\customrole1.json` con el siguiente ejemplo. El identificador se debe establecer en `null` durante la creación de rol inicial ya que se generará un nuevo identificador automáticamente. 

```json
{
  "Name": "Custom Role 1",
  "Id": null,
  "IsCustom": true,
  "Description": "Allows for read access to Azure storage and compute resources and access to support",
  "Actions": [
    "Microsoft.Compute/*/read",
    "Microsoft.Storage/*/read",
    "Microsoft.Support/*"
  ],
  "NotActions": [
  ],
  "AssignableScopes": [
    "/subscriptions/00000000-0000-0000-0000-000000000000",
    "/subscriptions/11111111-1111-1111-1111-111111111111"
  ]
}
```

Para agregar el rol a las suscripciones, ejecute el siguiente comando de PowerShell:

```azurepowershell
New-AzureRmRoleDefinition -InputFile "C:\CustomRoles\customrole1.json"
```

## <a name="update-a-custom-role"></a>Actualización de un rol personalizado

De modo similar a la creación de un rol personalizado, puede modificar un rol personalizado existente por medio de `PSRoleDefinition` o una plantilla JSON.

### <a name="update-a-custom-role-with-the-psroledefinition-object"></a>Actualización de un rol personalizado con el objeto PSRoleDefinition

Para modificar un rol personalizado, use en primer lugar el comando [Get-AzureRmRoleDefinition](/powershell/module/azurerm.resources/get-azurermroledefinition) para recuperar la definición de rol. Después, haga los cambios que desee en la definición de rol. Por último, use el comando [Set-AzureRmRoleDefinition](/powershell/module/azurerm.resources/set-azurermroledefinition) para guardar la definición de rol modificada.

En el ejemplo siguiente se agrega la operación `Microsoft.Insights/diagnosticSettings/*` al rol personalizado *Operador de máquina virtual* .

```azurepowershell
$role = Get-AzureRmRoleDefinition "Virtual Machine Operator"
$role.Actions.Add("Microsoft.Insights/diagnosticSettings/*")
Set-AzureRmRoleDefinition -Role $role
```

```Example
PS C:\> $role = Get-AzureRmRoleDefinition "Virtual Machine Operator"
PS C:\> $role.Actions.Add("Microsoft.Insights/diagnosticSettings/*")
PS C:\> Set-AzureRmRoleDefinition -Role $role

Name             : Virtual Machine Operator
Id               : 88888888-8888-8888-8888-888888888888
IsCustom         : True
Description      : Can monitor and restart virtual machines.
Actions          : {Microsoft.Storage/*/read, Microsoft.Network/*/read, Microsoft.Compute/*/read,
                   Microsoft.Compute/virtualMachines/start/action...}
NotActions       : {}
AssignableScopes : {/subscriptions/00000000-0000-0000-0000-000000000000,
                   /subscriptions/11111111-1111-1111-1111-111111111111}
```

En el ejemplo siguiente se agrega una suscripción de Azure a los ámbitos asignables del rol personalizado *Operador de máquina virtual* .

```azurepowershell
Get-AzureRmSubscription -SubscriptionName Production3

$role = Get-AzureRmRoleDefinition "Virtual Machine Operator"
$role.AssignableScopes.Add("/subscriptions/22222222-2222-2222-2222-222222222222")
Set-AzureRmRoleDefinition -Role $role
```

```Example
PS C:\> Get-AzureRmSubscription -SubscriptionName Production3

Name     : Production3
Id       : 22222222-2222-2222-2222-222222222222
TenantId : 99999999-9999-9999-9999-999999999999
State    : Enabled

PS C:\> $role = Get-AzureRmRoleDefinition "Virtual Machine Operator"
PS C:\> $role.AssignableScopes.Add("/subscriptions/22222222-2222-2222-2222-222222222222")
PS C:\> Set-AzureRmRoleDefinition -Role $role

Name             : Virtual Machine Operator
Id               : 88888888-8888-8888-8888-888888888888
IsCustom         : True
Description      : Can monitor and restart virtual machines.
Actions          : {Microsoft.Storage/*/read, Microsoft.Network/*/read, Microsoft.Compute/*/read,
                   Microsoft.Compute/virtualMachines/start/action...}
NotActions       : {}
AssignableScopes : {/subscriptions/00000000-0000-0000-0000-000000000000,
                   /subscriptions/11111111-1111-1111-1111-111111111111,
                   /subscriptions/22222222-2222-2222-2222-222222222222}
```

### <a name="update-a-custom-role-with-a-json-template"></a>Actualización de un rol personalizado con una plantilla JSON

Con la anterior plantilla JSON, puede modificar fácilmente un rol personalizado existente para agregar o quitar acciones. Actualice la plantilla JSON y agregue la acción de lectura para las redes tal y como se muestra en el siguiente ejemplo. Las definiciones que aparecen en la plantilla no se aplican de manera acumulativa a una definición existente, lo que significa que el rol aparecerá exactamente como lo especifique en la plantilla. También que necesita actualizar el identificador con el identificador del rol. Si no está seguro de cuál es este valor, puede usar el cmdlet [Get-AzureRmRoleDefinition](/powershell/module/azurerm.resources/get-azurermroledefinition) para obtener esta información.

```json
{
  "Name": "Custom Role 1",
  "Id": "acce7ded-2559-449d-bcd5-e9604e50bad1",
  "IsCustom": true,
  "Description": "Allows for read access to Azure storage and compute resources and access to support",
  "Actions": [
    "Microsoft.Compute/*/read",
    "Microsoft.Storage/*/read",
    "Microsoft.Network/*/read",
    "Microsoft.Support/*"
  ],
  "NotActions": [
  ],
  "AssignableScopes": [
    "/subscriptions/00000000-0000-0000-0000-000000000000",
    "/subscriptions/11111111-1111-1111-1111-111111111111"
  ]
}
```

Para actualizar el rol existente, ejecute el siguiente comando de PowerShell:

```azurepowershell
Set-AzureRmRoleDefinition -InputFile "C:\CustomRoles\customrole1.json"
```

## <a name="delete-a-custom-role"></a>Eliminación de un rol personalizado

Para eliminar un rol personalizado, use el comando [Remove-AzureRmRoleDefinition](/powershell/module/azurerm.resources/remove-azurermroledefinition).

En el ejemplo siguiente se quita el rol personalizado *Operador de máquina virtual* .

```azurepowershell
Get-AzureRmRoleDefinition "Virtual Machine Operator"
Get-AzureRmRoleDefinition "Virtual Machine Operator" | Remove-AzureRmRoleDefinition
```

```Example
PS C:\> Get-AzureRmRoleDefinition "Virtual Machine Operator"

Name             : Virtual Machine Operator
Id               : 88888888-8888-8888-8888-888888888888
IsCustom         : True
Description      : Can monitor and restart virtual machines.
Actions          : {Microsoft.Storage/*/read, Microsoft.Network/*/read, Microsoft.Compute/*/read,
                   Microsoft.Compute/virtualMachines/start/action...}
NotActions       : {}
AssignableScopes : {/subscriptions/00000000-0000-0000-0000-000000000000,
                   /subscriptions/11111111-1111-1111-1111-111111111111}

PS C:\> Get-AzureRmRoleDefinition "Virtual Machine Operator" | Remove-AzureRmRoleDefinition

Confirm
Are you sure you want to remove role definition with name 'Virtual Machine Operator'.
[Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): Y
```

## <a name="next-steps"></a>Pasos siguientes

- [Tutorial: Creación de un rol personalizado con Azure PowerShell](tutorial-custom-role-powershell.md)
- [Roles personalizados en Azure](custom-roles.md)
- [Operaciones del proveedor de recursos de Azure Resource Manager](resource-provider-operations.md)
