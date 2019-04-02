---
title: 'Creación de entidades de servicio de la versión preliminar de Windows Virtual Desktop y de asignaciones de roles con PowerShell: Azure'
description: Creación de entidades de servicio y asignación de roles con PowerShell en la versión preliminar de Windows Virtual Desktop.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: tutorial
ms.date: 03/21/2019
ms.author: helohr
ms.openlocfilehash: 1bbe89484d72a21c4432d452d4ddae83ea2d2553
ms.sourcegitcommit: 81fa781f907405c215073c4e0441f9952fe80fe5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/25/2019
ms.locfileid: "58400023"
---
# <a name="tutorial-create-service-principals-and-role-assignments-with-powershell"></a>Tutorial: Creación de entidades de servicio y asignaciones de roles con PowerShell

Las entidades de servicio son identidades que puede crear en Azure Active Directory para asignar roles y permisos para un propósito específico. En la versión preliminar de Windows Virtual Desktop, puede crear una entidad de servicio para:

- Automatizar tareas de administración específicas de Windows Virtual Desktop
- Usarla como credenciales en lugar de los usuarios necesarios para la autenticación multifactor al ejecutar cualquier plantilla de Azure Resource Manager en Windows Virtual Desktop

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Creación de una entidad de servicio en Azure Active Directory
> * Creación de una asignación de roles en Windows Virtual Desktop
> * Inicio de sesión en Windows Virtual Desktop con la entidad de servicio

## <a name="prerequisites"></a>Requisitos previos

Para poder crear entidades de servicio y asignaciones de roles, necesita hacer tres cosas:

1. Instalar el módulo de Azure AD. Instalar el módulo, ejecutar PowerShell como administrador y ejecutar el siguiente cmdlet:

    ```powershell
    Install-Module AzureAD
    ```

2. Ejecutar los siguientes cmdlet con los valores entre comillas, sustituidos por los valores de su sesión.

    ```powershell
    $myTenantGroupName = "<my-tenant-group-name>"
    $myTenantName = "<my-tenant-name>"
    ```

3. Seguir todas las instrucciones de este artículo en la misma sesión de PowerShell. Si se cierra la ventana y se intenta volver a ella después, podría no funcionar.

## <a name="create-a-service-principal-in-azure-active-directory"></a>Creación de una entidad de servicio en Azure Active Directory

Una vez cumplidos los requisitos previos en la sesión de PowerShell, ejecute los siguientes cmdlets de PowerShell para crear una entidad de servicio multiinquilino en Azure.

```powershell
Import-Module AzureAD
$aadContext = Connect-AzureAD
$svcPrincipal = New-AzureADApplication -AvailableToOtherTenants $true -DisplayName "Windows Virtual Desktop Svc Principal"
$svcPrincipalCreds = New-AzureADApplicationPasswordCredential -ObjectId $svcPrincipal.ObjectId
```

## <a name="create-a-role-assignment-in-windows-virtual-desktop-preview"></a>Creación de una asignación de roles en la versión preliminar de Windows Virtual Desktop

Ahora que ha creado una entidad de servicio, puede usarla para iniciar sesión en Windows Virtual Desktop. Asegúrese de que iniciar sesión con una cuenta que tenga permisos para crear la asignación de roles.

En primer lugar y, si aún no lo ha hecho, [descargue e importe el módulo de PowerShell para Windows Virtual Desktop](https://docs.microsoft.com/powershell/windows-virtual-desktop/overview) que se usará en la sesión de PowerShell.

Ejecute los siguientes cmdlets de PowerShell para conectarse a Windows Virtual Desktop y crear una asignación de roles para la entidad de servicio.

```powershell
Add-RdsAccount -DeploymentUrl "https://rdbroker.wvd.microsoft.com"
Set-RdsContext -TenantGroupName $myTenantGroupName
New-RdsRoleAssignment -RoleDefinitionName "RDS Owner" -ApplicationId $svcPrincipal.AppId -TenantGroupName $myTenantGroupName -TenantName $myTenantName
```

## <a name="sign-in-with-the-service-principal"></a>Inicio de sesión con la entidad de servicio

Después de crear una asignación de roles para la entidad de servicio debe asegurarse de que esta puede iniciar sesión en Windows Virtual Desktop mediante la ejecución del siguiente cmdlet:

```powershell
$creds = New-Object System.Management.Automation.PSCredential($svcPrincipal.AppId, (ConvertTo-SecureString $svcPrincipalCreds.Value -AsPlainText -Force))
Add-RdsAccount -DeploymentUrl "https://rdbroker.wvd.microsoft.com" -Credential $creds -ServicePrincipal -AadTenantId $aadContext.TenantId.Guid
```

Una vez iniciada la sesión, compruebe algunos cmdlets de PowerShell en Windows Virtual Desktop con la entidad de servicio para asegurarse de que todo funciona.

## <a name="view-your-credentials-in-powershell"></a>Visualización de las credenciales en PowerShell

Antes de finalizar la sesión de PowerShell, debe visualizar las credenciales y anotarlas para futura referencia. La contraseña es especialmente importante, porque no podrá recuperarla una vez que cierre esta sesión de PowerShell.

Estas son las tres credenciales que debe anotar y los cmdlets que necesita para ejecutar para obtenerlas:

- Contraseña:

    ```powershell
    $svcPrincipalCreds.Value
    ```

- Identificador de inquilino:

    ```powershell
    $aadContext.TenantId.Guid
    ```

- Identificador de aplicación:

    ```powershell
    $svcPrincipal.AppId
    ```

## <a name="next-steps"></a>Pasos siguientes

En este tutorial aprendió a crear una entidad de servicio y a iniciar sesión en Windows Virtual Desktop con ella. Para más información sobre el inicio de sesión en Windows Virtual Desktop, continúe con los procedimientos de conexión a Windows Virtual Desktop.

- [Conexión al cliente de Escritorio remoto en Windows 7 y Windows 10](connect-windows-7-and-10.md)
- [Conexión al cliente web de la versión preliminar de Windows Virtual Desktop](connect-web.md)
