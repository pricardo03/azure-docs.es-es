---
title: Creación, enumeración y eliminación de una identidad asignada por el usuario (MSI) mediante Azure PowerShell
description: Instrucciones paso a paso sobre cómo crear, enumerar y eliminar una instancia de Managed Service Identity asignada por el usuario mediante Azure PowerShell.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/16/2018
ms.author: daveba
ms.openlocfilehash: a9f684eccefab3e43d9b2b7a364b245a53519f76
ms.sourcegitcommit: e3d5de6d784eb6a8268bd6d51f10b265e0619e47
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/01/2018
ms.locfileid: "39389692"
---
# <a name="create-list-or-delete-a-user-assigned-identity-using-azure-powershell"></a>Creación, enumeración o eliminación de una identidad asignada por el usuario mediante Azure PowerShell

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

La identidad de servicio administrada proporciona a los servicios de Azure una identidad administrada en Azure Active Directory. Puede usar esta identidad para autenticar a cualquier servicio que admita la autenticación de Azure AD, sin necesidad de tener credenciales en el código. 

En este artículo, aprenderá a crear, enumerar y eliminar una identidad asignada por el usuario mediante Azure PowerShell.

## <a name="prerequisites"></a>Requisitos previos

- Si no está familiarizado con Managed Service Identity, consulte la [sección de introducción](overview.md). **No olvide revisar la [diferencia entre una identidad asignada por el sistema y una asignada por el usuario](overview.md#how-does-it-work)**.
- Si aún no tiene una cuenta de Azure, [regístrese para una cuenta gratuita](https://azure.microsoft.com/free/) antes de continuar.
- Instale [la versión más reciente de Azure PowerShell](https://www.powershellgallery.com/packages/AzureRM) si aún no lo ha hecho.
- Si decide instalar y usar PowerShell de forma local, en este tutorial necesitará la versión 5.7.0 del módulo de Azure PowerShell o cualquier versión posterior. Ejecute ` Get-Module -ListAvailable AzureRM` para encontrar la versión. Si necesita actualizarla, consulte [Instalación del módulo de Azure PowerShell](/powershell/azure/install-azurerm-ps). Si PowerShell se ejecuta localmente, también debe ejecutar `Login-AzureRmAccount` para crear una conexión con Azure.
- Para llevar a cabo las operaciones de administración de este artículo, su cuenta debe tener las siguientes asignaciones de roles:
    - Rol [Colaborador de identidad administrada](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) para crear, leer (listar), actualizar y eliminar una identidad asignada por el usuario.
    - Rol [Operador de identidad administrada](/azure/role-based-access-control/built-in-roles#managed-identity-operator) para leer (listar) las propiedades de una identidad asignada por el usuario.

> [!NOTE]
> Aunque las identidades asignadas por el usuario se encuentran aún en versión preliminar, tiene que instalar manualmente el módulo AzureRM.ManagedServiceIdentity con el siguiente comando. 
```azurepowershell-interactive
Install-Module -Name AzureRM.ManagedServiceIdentity -AllowPrerelease
```

## <a name="create-a-user-assigned-identity"></a>Creación de una identidad asignada por el usuario

Para crear una identidad asignada por el usuario, use el comando [New-AzureRmUserAssignedIdentity](/powershell/module/azurerm.managedserviceidentity/new-azurermuserassignedidentity). El parámetro `ResourceGroupName` especifica el grupo de recursos donde se creará la identidad asignada por el usuario, mientras que el parámetro `-Name` especifica su nombre. Reemplace los valores de los parámetros `<RESOURCE GROUP>` y `<USER ASSIGNED IDENTITY NAME>` con sus propios valores:

[!INCLUDE[ua-character-limit](~/includes/managed-identity-ua-character-limits.md)]

 ```azurepowershell-interactive
New-AzureRmUserAssignedIdentity -ResourceGroupName <RESOURCEGROUP> -Name <USER ASSIGNED IDENTITY NAME>
```
## <a name="list-user-assigned-identities"></a>Enumeración de identidades asignadas por el usuario

Para enumerar las identidades asignadas por el usuario, use el comando [Get-AzureRmUserAssigned](/powershell/module/azurerm.managedserviceidentity/get-azurermuserassignedidentity).  El parámetro `-ResourceGroupName` especifica el grupo de recursos donde se creó la identidad asignada por el usuario. Reemplace `<RESOURCE GROUP>` por su propio valor:

```azurepowershell-interactive
Get-AzureRmUserAssignedIdentity -ResourceGroupName <RESOURCE GROUP>
```
En la respuesta, a las identidades de usuario se devuelve el valor `"Microsoft.ManagedIdentity/userAssignedIdentities"` para la clave `Type`.

`Type :Microsoft.ManagedIdentity/userAssignedIdentities`

## <a name="delete-a-user-assigned-identity"></a>Eliminación de una identidad asignada por el usuario

Para eliminar una identidad asignada por el usuario, use el comando [Remove-AzureRmUserAssignedIdentity](/powershell/module/azurerm.managedserviceidentity/remove-azurermuserassignedidentity).  El parámetro `-ResourceGroupName` especifica el grupo de recursos donde se creó la identidad asignada por el usuario, mientras que el parámetro `-Name` especifica su nombre. Reemplace los valores de los parámetros `<RESOURCE GROUP>` y `<USER ASSIGNED IDENTITY NAME>` con sus propios valores:

 ```azurecli-interactive
Remove-AzurRmUserAssignedIdentity -ResourceGroupName <RESOURCE GROUP> -Name <USER ASSIGNED IDENTITY NAME>
```
> [!NOTE]
> La eliminación de una identidad asignada por el usuario no quitará la referencia de ningún recurso al que se haya asignado. Las asignaciones de identidad deben quitarse por separado.

## <a name="related-content"></a>Contenido relacionado

Para obtener una lista completa y más detalles sobre los comandos de MSI de Azure PowerShell, consulte [AzureRM.ManagedServiceIdentity](/powershell/module/azurerm.managedserviceidentity#managed_service_identity).


 
