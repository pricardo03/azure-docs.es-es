---
title: 'Azure Active Directory Domain Services: sincronización con ámbito | Microsoft Docs'
description: Configuración de la sincronización con ámbito entre Azure AD y un dominio administrado
services: active-directory-ds
documentationcenter: ''
author: eringreenlee
manager: mtillman
editor: curtand
ms.assetid: 9389cf0f-0036-4b17-95da-80838edd2225
ms.service: active-directory
ms.component: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2018
ms.author: ergreenl
ms.openlocfilehash: 7726a28e0254f4316759c3d8c74f57db6a9e399e
ms.sourcegitcommit: dbfd977100b22699823ad8bf03e0b75e9796615f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2018
ms.locfileid: "50242201"
---
# <a name="configure-scoped-synchronization-from-azure-ad-to-your-managed-domain"></a>Configuración de la sincronización con ámbito entre Azure AD y un dominio administrado
En este artículo se muestra cómo configurar solo cuentas de usuario específicas para sincronizarlas entre el directorio de Azure AD y el dominio administrado de Azure AD Domain Services.


## <a name="group-based-scoped-synchronization"></a>Sincronización con ámbito basada en grupos
De forma predeterminada, todos los usuarios y grupos del directorio de Azure AD se sincronizan con el dominio administrado. Si solo unos cuantos usuarios usan el dominio administrado, solo puede sincronizar esas cuentas de usuario. La sincronización con ámbito basada en grupos le permite hacer eso. Cuando está configurada, solo las cuentas de usuario que pertenecen a los grupos que haya especificado se sincronizan con el dominio administrado.

La tabla siguiente le ayuda a determinar cómo usar la sincronización con ámbito:

| **Estado actual** | **Estado deseado** | **Configuración necesaria** |
| --- | --- | --- |
| El dominio administrado existente está configurado para sincronizar todas las cuentas de usuario y grupos. | Quiere sincronizar solo las cuentas de usuario que pertenecen a grupos específicos con su dominio administrado. | [Elimine el dominio administrado existente](active-directory-ds-disable-aadds.md). A continuación, siga las instrucciones de este artículo para volver a crearlo con la sincronización con ámbito configurada. |
| No tiene un dominio administrado existente. | Quiere crear un dominio administrado y sincronizar solo las cuentas de usuario que pertenecen a grupos específicos. | Siga las instrucciones de este artículo para crear un dominio administrado con la sincronización con ámbito configurada. |
| El dominio administrado existente está configurado para sincronizar únicamente las cuentas que pertenecen a grupos específicos. | Quiere modificar la lista de grupos cuyos usuarios se deben sincronizar con el dominio administrado. | Siga las instrucciones de este artículo para modificar la sincronización con ámbito. |

> [!WARNING]
> **Al cambiar el ámbito de la sincronización, el dominio administrado pasa por una resincronización.**
>
 * Al cambiar el ámbito de sincronización de un dominio administrado, se produce una resincronización completa.
 * Los objetos que ya no son necesarios en el dominio administrado se eliminan. Se crean objetos en el dominio administrado.
 * La resincronización puede tardar mucho tiempo en completarse, según el número de objetos (usuarios, grupos y pertenencia a grupos) en el dominio administrado y el directorio de Azure AD. En directorios grandes con muchos cientos de miles de objetos, la resincronización puede tardar algunos días.
>
>


## <a name="create-a-new-managed-domain-and-enable-group-based-scoped-synchronization"></a>Creación de un dominio administrado y habilitación de la sincronización con ámbito basada en grupos
Use PowerShell para completar este conjunto de pasos. Consulte las instrucciones para [habilitar Azure Active Directory Domain Services mediante PowerShell](active-directory-ds-enable-using-powershell.md). Un par de pasos de este artículo se han modificado ligeramente para configurar la sincronización con ámbito.

Lleva a cabo los pasos siguientes para configurar la sincronización de ámbito basadas en grupos con el dominio administrado:

1. Realice las siguientes tareas:
  * [Tarea 1: Instalación de los módulos de PowerShell necesarios](active-directory-ds-enable-using-powershell.md#task-1-install-the-required-powershell-modules).
  * [Tarea 2: Creación de la entidad de servicio necesaria en el directorio de Azure AD](active-directory-ds-enable-using-powershell.md#task-2-create-the-required-service-principal-in-your-azure-ad-directory).
  * [Tarea 3: Creación y configuración del grupo "Administradores de controladores de dominio de AAD"](active-directory-ds-enable-using-powershell.md#task-3-create-and-configure-the-aad-dc-administrators-group).
  * [Tarea 4: Registro del proveedor de recursos de Azure AD Domain Services](active-directory-ds-enable-using-powershell.md#task-4-register-the-azure-ad-domain-services-resource-provider).
  * [Tarea 5: Creación de un grupo de recursos](active-directory-ds-enable-using-powershell.md#task-5-create-a-resource-group).
  * [Tarea 6: Creación y configuración de la red virtual](active-directory-ds-enable-using-powershell.md#task-6-create-and-configure-the-virtual-network).

2. Seleccione los grupos que quiere sincronizar y proporcione el nombre para mostrar de los grupos que quiere que se sincronicen con el dominio administrado.

3. Guarde el [script en la sección siguiente](active-directory-ds-scoped-synchronization.md#script-to-select-groups-to-synchronize-to-the-managed-domain-select-groupstosyncps1) en un archivo llamado ```Select-GroupsToSync.ps1```. Ejecute el script como se muestra a continuación:

  ```powershell
  .\Select-GroupsToSync.ps1 -groupsToAdd @("AAD DC Administrators", "GroupName1", "GroupName2")
  ```

  > [!WARNING]
  > **No olvide incluir el grupo "Administradores de controladores de dominio de AAD".**
  >
  > Debe incluir el grupo "Administradores de controladores de dominio de AAD" en la lista de grupos configurados para la sincronización con ámbito. Si no incluye este grupo, el dominio administrado será inutilizable.
  >

4. Ahora, cree el dominio administrado y habilite para él la sincronización son ámbito basada en grupos. Incluya la propiedad ```"filteredSync" = "Enabled"``` en el parámetro ```Properties```. Por ejemplo, consulte el siguiente fragmento de script, copiado de la [Tarea 7: Aprovisionamiento del dominio administrado de Azure AD Domain Services](active-directory-ds-enable-using-powershell.md#task-7-provision-the-azure-ad-domain-services-managed-domain).

  ```powershell
  $AzureSubscriptionId = "YOUR_AZURE_SUBSCRIPTION_ID"
  $ManagedDomainName = "contoso100.com"
  $ResourceGroupName = "ContosoAaddsRg"
  $VnetName = "DomainServicesVNet_WUS"
  $AzureLocation = "westus"

  # Enable Azure AD Domain Services for the directory.
  New-AzureRmResource -ResourceId "/subscriptions/$AzureSubscriptionId/resourceGroups/$ResourceGroupName/providers/Microsoft.AAD/DomainServices/$ManagedDomainName" `
  -Location $AzureLocation `
  -Properties @{"DomainName"=$ManagedDomainName; "filteredSync" = "Enabled"; `
    "SubnetId"="/subscriptions/$AzureSubscriptionId/resourceGroups/$ResourceGroupName/providers/Microsoft.Network/virtualNetworks/$VnetName/subnets/DomainServices"} `
  -ApiVersion 2017-06-01 -Force -Verbose
  ```

  > [!TIP]
  > No olvide incluir ```"filteredSync" = "Enabled"``` en el parámetro ```-Properties```, de modo que la sincronización con ámbito esté habilitada para el dominio administrado.


## <a name="script-to-select-groups-to-synchronize-to-the-managed-domain-select-groupstosyncps1"></a>Script para seleccionar grupos para sincronizar con el dominio administrado (Select-GroupsToSync.ps1)
Guarde el siguiente script en un archivo (```Select-GroupsToSync.ps1```). Este script configura Azure AD Domain Services para sincronizar los grupos seleccionados con el dominio administrado. Todas las cuentas de usuario que pertenecen a los grupos especificados se sincronizarán con el dominio administrado.

```powershell
param (
    [Parameter(Position = 0)]
    [String[]]$groupsToAdd
)

Connect-AzureAD
$sp = Get-AzureADServicePrincipal -Filter "AppId eq '2565bd9d-da50-47d4-8b85-4c97f669dc36'"
$role = $sp.AppRoles | where-object -FilterScript {$_.DisplayName -eq "User"}

Write-Output "`n****************************************************************************"

Write-Output "Total group-assignments need to be added: $($groupsToAdd.Count)"
$newGroupIds = New-Object 'System.Collections.Generic.HashSet[string]'
foreach ($groupName in $groupsToAdd)
{
    try
    {
        $group = Get-AzureADGroup -Filter "DisplayName eq '$groupName'"
        $newGroupIds.Add($group.ObjectId)

        Write-Output "Group-Name: $groupName, Id: $($group.ObjectId)"
    }
    catch
    {
        Write-Error "Failed to find group: $groupName. Exception: $($_.Exception)."
    }
}

Write-Output "****************************************************************************`n"
Write-Output "`n****************************************************************************"

$currentAssignments = Get-AzureADServiceAppRoleAssignment -ObjectId $sp.ObjectId
Write-Output "Total current group-assignments: $($currentAssignments.Count), SP-ObjectId: $($sp.ObjectId)"

$currAssignedObjectIds = New-Object 'System.Collections.Generic.HashSet[string]'
foreach ($assignment in $currentAssignments)
{
    Write-Output "Assignment-ObjectId: $($assignment.PrincipalId)"

    if ($newGroupIds.Contains($assignment.PrincipalId) -eq $false)
    {
        Write-Output "This assignment is not needed anymore. Removing it! Assignment-ObjectId: $($assignment.PrincipalId)"
        Remove-AzureADServiceAppRoleAssignment -ObjectId $sp.ObjectId -AppRoleAssignmentId $assignment.ObjectId
    }
    else
    {
        $currAssignedObjectIds.Add($assignment.PrincipalId)
    }
}

Write-Output "****************************************************************************`n"
Write-Output "`n****************************************************************************"

foreach ($id in $newGroupIds)
{
    try
    {
        if ($currAssignedObjectIds.Contains($id) -eq $false)
        {
            Write-Output "Adding new group-assignment. Role-Id: $($role.Id), Group-Object-Id: $id, ResourceId: $($sp.ObjectId)"
            New-AzureADGroupAppRoleAssignment -Id $role.Id -ObjectId $id -PrincipalId $id -ResourceId $sp.ObjectId
        }
        else
        {
            Write-Output "Group-ObjectId: $id is already assigned."
        }
    }
    catch
    {
        Write-Error "Exception occured assigning Object-ID: $id. Exception: $($_.Exception)."
    }
}

Write-Output "****************************************************************************`n"
```


## <a name="modify-group-based-scoped-synchronization"></a>Modificación de la sincronización con ámbito basada en grupos
Para modificar la lista de grupos cuyos usuarios se deben sincronizar con el dominio administrado, vuelva a ejecutar el [script de PowerShell](active-directory-ds-scoped-synchronization.md#script-to-select-groups-to-synchronize-to-the-managed-domain-select-groupstosyncps1) y especifique la nueva lista de grupos. No olvide especificar siempre el grupo "Administradores de controladores de dominio de AAD" en esta lista.

> [!WARNING]
> **No olvide incluir el grupo "Administradores de controladores de dominio de AAD".**
>
> Debe incluir el grupo "Administradores de controladores de dominio de AAD" en la lista de grupos configurados para la sincronización con ámbito. Si no incluye este grupo, el dominio administrado será inutilizable.
>


## <a name="disable-group-based-scoped-synchronization"></a>Deshabilitación de la sincronización con ámbito basada en grupos
Use el siguiente script de PowerShell para deshabilitar la sincronización con ámbito basada en grupos para el dominio administrado:

```powershell
// Login to your Azure AD tenant
Login-AzureRmAccount

// Retrieve the Azure AD Domain Services resource.
$DomainServicesResource = Get-AzureRmResource -ResourceType "Microsoft.AAD/DomainServices"

// Disable group-based scoped synchronization.
$disableScopedSync = @{"filteredSync" = "Disabled"}

Set-AzureRmResource -Id $DomainServicesResource.ResourceId -Properties $disableScopedSync
```

## <a name="next-steps"></a>Pasos siguientes
* [Sincronización de Azure AD Domain Services](active-directory-ds-synchronization.md)
* [Habilitación de Azure Active Directory Domain Services mediante PowerShell](active-directory-ds-enable-using-powershell.md)
