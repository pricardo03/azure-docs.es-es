---
title: Uso de una característica Managed Service Identity asignada por el usuario en una máquina virtual Windows para acceder a Azure Resource Manager
description: Tutorial que le guía por el proceso para usar una identidad de Managed Service Identity asignada por el usuario con una máquina virtual Windows para acceder a Azure Resource Manager.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: daveba
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/10/2018
ms.author: daveba
ms.openlocfilehash: 30eb40967b2fd8a6b5e18cf0074a68fb24fd0744
ms.sourcegitcommit: f1e6e61807634bce56a64c00447bf819438db1b8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/24/2018
ms.locfileid: "42886389"
---
# <a name="tutorial-use-a-user-assigned-managed-service-identity-on-a-windows-vm-to-access-azure-resource-manager"></a>Tutorial: Uso de una característica Managed Service Identity asignada por el usuario en una máquina virtual Windows para acceder a Azure Resource Manager

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

En este tutorial, se explica cómo crear una identidad asignada por el usuario, asignarla a una máquina virtual Windows y después usar esa identidad para acceder a la API de Azure Resource Manager. Azure administra automáticamente las identidades de servicio administradas. Estas identidades permiten autenticarse en servicios que admiten la autenticación de Azure AD, sin necesidad de incluir credenciales en el código. 

Aprenderá a:

> [!div class="checklist"]
> * Creación de una identidad asignada por el usuario
> * Asignación de su identidad asignada por el usuario a la máquina virtual Windows
> * Conceder a la identidad asignada por el usuario acceso a un grupo de recursos en Azure Resource Manager 
> * Obtener un token de acceso mediante la identidad asignada por el usuario y usarla para llamar a Azure Resource Manager 
> * Leer las propiedades de un grupo de recursos

## <a name="prerequisites"></a>Requisitos previos

[!INCLUDE [msi-qs-configure-prereqs](../../../includes/active-directory-msi-qs-configure-prereqs.md)]

[!INCLUDE [msi-tut-prereqs](../../../includes/active-directory-msi-tut-prereqs.md)]

- [Iniciar sesión en Azure Portal](https://portal.azure.com)

- [Crear una máquina virtual Windows](/azure/virtual-machines/windows/quick-create-portal)

- Para cumplir con los pasos necesarios de creación de recursos y administración de roles de este tutorial, la cuenta debe tener permisos de "Propietario" en el ámbito adecuado (en su suscripción o grupo de recursos). Si necesita ayuda con la asignación de roles, consulte [Uso del control de acceso basado en rol para administrar el acceso a los recursos de la suscripción de Azure](/azure/role-based-access-control/role-assignments-portal).
- Si decide instalar y usar PowerShell de forma local, en este tutorial necesitará la versión 5.7.0 del módulo de Azure PowerShell, o cualquier versión posterior. Ejecute ` Get-Module -ListAvailable AzureRM` para encontrar la versión. Si necesita actualizarla, consulte [Instalación del módulo de Azure PowerShell](/powershell/azure/install-azurerm-ps). 
- Si ejecuta PowerShell localmente, también necesita realizar las siguientes operaciones: 
    - Ejecute `Login-AzureRmAccount` para crear una conexión con Azure.
    - Instalar la [versión más reciente de PowerShellGet](/powershell/gallery/installing-psget#for-systems-with-powershell-50-or-newer-you-can-install-the-latest-powershellget).
    - Ejecutar `Install-Module -Name PowerShellGet -AllowPrerelease` para obtener la versión preliminar del módulo `PowerShellGet` (es posible que deba usar `Exit` para salir de la sesión actual de PowerShell después de ejecutar este comando para instalar el módulo `AzureRM.ManagedServiceIdentity`).
    - Ejecutar `Install-Module -Name AzureRM.ManagedServiceIdentity -AllowPrerelease` para instalar la versión preliminar del módulo `AzureRM.ManagedServiceIdentity`, con el fin de realizar las operaciones de identidad asignadas por el usuario de este artículo.

## <a name="create-a-user-assigned-identity"></a>Creación de una identidad asignada por el usuario

Se crea una identidad asignada por el usuario como recurso de Azure independiente. Mediante [New-AzureRmUserAssignedIdentity](/powershell/module/azurerm.managedserviceidentity/get-azurermuserassignedidentity), Azure crea una identidad en el inquilino de Azure AD que puede asignarse a una o varias instancias de servicio de Azure.

[!INCLUDE[ua-character-limit](~/includes/managed-identity-ua-character-limits.md)]

```azurepowershell-interactive
New-AzureRmUserAssignedIdentity -ResourceGroupName myResourceGroupVM -Name ID1
```

La respuesta contiene detalles de la identidad asignada por el usuario que se ha creado, de forma similar al ejemplo siguiente. Anote los valores `Id` y `ClientId` de la identidad asignada por el usuario, ya que se usan en los siguientes pasos:

```azurepowershell
{
Id: /subscriptions/<SUBSCRIPTIONID>/resourcegroups/myResourceGroupVM/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1
ResourceGroupName : myResourceGroupVM
Name: ID1
Location: westus
TenantId: 733a8f0e-ec41-4e69-8ad8-971fc4b533f8
PrincipalId: e591178e-b785-43c8-95d2-1397559b2fb9
ClientId: af825a31-b0e0-471f-baea-96de555632f9
ClientSecretUrl: https://control-westus.identity.azure.net/subscriptions/<SUBSCRIPTIONID>/resourcegroups/myResourceGroupVM/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1/credentials?tid=733a8f0e-ec41-4e69-8ad8-971fc4b533f8&oid=e591178e-b785-43c8-95d2-1397559b2fb9&aid=af825a31-b0e0-471f-baea-96de555632f9
Type: Microsoft.ManagedIdentity/userAssignedIdentities
}
```

## <a name="assign-the-user-assigned-identity-to-a-windows-vm"></a>Asignar una identidad asignada por el usuario a una máquina virtual Windows

Los clientes pueden usar una identidad asignada por el usuario en varios recursos de Azure. Use los siguientes comandos para asignar la identidad asignada por el usuario a una única máquina virtual. Use la propiedad `Id` devuelta en el paso anterior para el parámetro `-IdentityID`.

```azurepowershell-interactive
$vm = Get-AzureRmVM -ResourceGroupName myResourceGroup -Name myVM
Update-AzureRmVM -ResourceGroupName TestRG -VM $vm -IdentityType "UserAssigned" -IdentityID "/subscriptions/<SUBSCRIPTIONID>/resourcegroups/myResourceGroupVM/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1"
```

## <a name="grant-your-user-assigned-managed-service-identity-access-to-a-resource-group-in-azure-resource-manager"></a>Concesión a la característica Managed Service Identity acceso a un grupo de recursos en Azure Resource Manager 

Managed Service Identity proporciona las identidades que el código puede usar para solicitar tokens de acceso para autenticarse en las API de recursos que admiten la autenticación de Azure AD. En este tutorial, el código accederá a la API de Azure Resource Manager. 

Antes de que el código pueda acceder a la API, debe conceder a la identidad acceso a un recurso en Azure Resource Manager. En este caso, se trata del grupo de recursos que contiene la máquina virtual. Actualice el valor de `<SUBSCRIPTION ID>` según corresponda en su entorno.

```azurepowershell-interactive
$spID = (Get-AzureRmUserAssignedIdentity -ResourceGroupName myResourceGroupVM -Name ID1).principalid
New-AzureRmRoleAssignment -ObjectId $spID -RoleDefinitionName "Reader" -Scope "/subscriptions/<SUBSCRIPTIONID>/resourcegroups/myResourceGroupVM/"
```

La respuesta contiene detalles de la asignación de roles que se ha creado, de forma similar al ejemplo siguiente:

```azurepowershell
RoleAssignmentId: /subscriptions/80c696ff-5efa-4909-a64d-f1b616f423ca/resourcegroups/myResourceGroupVM/providers/Microsoft.Authorization/roleAssignments/f9cc753d-265e-4434-ae19-0c3e2ead62ac
Scope: /subscriptions/80c696ff-5efa-4909-a64d-f1b616f423ca/resourcegroups/myResourceGroupVM
DisplayName: ID1
SignInName:
RoleDefinitionName: Reader
RoleDefinitionId: acdd72a7-3385-48ef-bd42-f606fba81ae7
ObjectId: e591178e-b785-43c8-95d2-1397559b2fb9
ObjectType: ServicePrincipal
CanDelegate: False
```

## <a name="get-an-access-token-using-the-vms-identity-and-use-it-to-call-resource-manager"></a>Obtenga un token de acceso con la identidad de la máquina virtual y úselo para llamar a Resource Manager 

En el resto del tutorial, trabajará desde la máquina virtual que ha creado anteriormente.

1. Inicie sesión en Azure Portal en [https://portal.azure.com](https://portal.azure.com).

2. En el portal, vaya a **Virtual Machines** y diríjase a la máquina virtual Windows. A continuación, en **Introducción**, haga clic en **Conectar**.

3. Escriba el **nombre de usuario** y la **contraseña** que usó al crear la máquina virtual Windows.

4. Ahora que ha creado una **conexión a Escritorio remoto** con la máquina virtual, abra **PowerShell** en la sesión remota.

5. Mediante `Invoke-WebRequest` de PowerShell, realice una solicitud al punto de conexión de Managed Service Identity local para obtener un token de acceso para Azure Resource Manager.  El valor `client_id` es el valor que se devolvió al [crear la identidad administrada asignada por el usuario](#create-a-user-assigned-identity).

    ```azurepowershell
    $response = Invoke-WebRequest -Uri 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&client_id=af825a31-b0e0-471f-baea-96de555632f9&resource=https://management.azure.com/' -Method GET -Headers @{Metadata="true"}
    $content = $response.Content | ConvertFrom-Json
    $ArmToken = $content.access_token
    ```

## <a name="read-the-properties-of-a-resource-group"></a>Leer las propiedades de un grupo de recursos

Use el token de acceso que ha recuperado en el paso anterior para acceder a Azure Resource Manager y lea las propiedades del grupo de recursos al que ha concedido acceso a la identidad asignada por el usuario. Reemplace <SUBSCRIPTION ID> con el identificador de la suscripción de su entorno.

```azurepowershell
(Invoke-WebRequest -Uri https://management.azure.com/subscriptions/80c696ff-5efa-4909-a64d-f1b616f423ca/resourceGroups/myResourceGroupVM?api-version=2016-06-01 -Method GET -ContentType "application/json" -Headers @{Authorization ="Bearer $ArmToken"}).content
```
La respuesta contiene la información específica del grupo de recursos, de forma similar al ejemplo siguiente:

```json
{"id":"/subscriptions/<SUBSCRIPTIONID>/resourceGroups/myResourceGroupVM","name":"myResourceGroupVM","location":"eastus","properties":{"provisioningState":"Succeeded"}}
```

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha obtenido información sobre cómo crear una identidad asignada por el usuario y a adjuntarla a una instancia de Azure Virtual Machines para acceder a la API de Azure Resource Manager.  Para obtener más información sobre Azure Resource Manager, vea:

> [!div class="nextstepaction"]
>[Azure Resource Manager](/azure/azure-resource-manager/resource-group-overview)