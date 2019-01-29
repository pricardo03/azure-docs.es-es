---
title: Uso de una identidad administrada asignada por el sistema de una máquina virtual Windows para acceder a Azure AD Graph API
description: Este tutorial contiene directrices acerca de cómo utilizar una identidad administrada asignada por el sistema de una máquina virtual Windows para acceder a Azure AD Graph API.
services: active-directory
documentationcenter: ''
author: daveba
manager: daveba
editor: daveba
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/20/2018
ms.author: daveba
ms.openlocfilehash: 4396ab093b20e4a7fb89ddbfe821be117bb8ebfb
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/22/2019
ms.locfileid: "54438684"
---
# <a name="tutorial-use-a-windows-vm-system-assigned-managed-identity-to-access-azure-ad-graph-api"></a>Tutorial: Uso de una identidad administrada asignada por el sistema de una máquina virtual Windows para acceder a Azure AD Graph API

[!INCLUDE [preview-notice](~/includes/active-directory-msi-preview-notice.md)]

En este tutorial, se explica cómo se utiliza una identidad administrada asignada por el sistema de una máquina virtual Windows para acceder a Microsoft Graph API para recuperar su pertenencia al grupo. Azure administra automáticamente Managed Identities for Azure Resources, que le permiten autenticar los servicios que admiten la autenticación de Azure AD sin necesidad de insertar credenciales en el código.  Para este tutorial, deberá consultar la pertenencia de su identidad de máquina virtual a los grupos de Azure AD. Por ejemplo, la información de grupo se usa a menudo en las decisiones de autorización. En segundo plano, la identidad administrada de la máquina virtual se representa mediante una **entidad de servicio** en Azure AD. Antes de realizar la consulta del grupo, agregue la entidad de servicio que representa la identidad de la máquina virtual a un grupo en Azure AD. Para ello, puede usar Azure PowerShell, Azure AD PowerShell o la CLI de Azure.

> [!div class="checklist"]
> * Conectarse a Azure
> * Adición de la identidad de máquina virtual a un grupo en Azure AD 
> * Concesión del acceso a la identidad de la máquina virtual a Azure AD Graph 
> * Obtención de un token de acceso mediante la identidad de la máquina virtual y úselo para llamar a Azure AD Graph

## <a name="prerequisites"></a>Requisitos previos

[!INCLUDE [msi-tut-prereqs](../../../includes/active-directory-msi-tut-prereqs.md)]

- Para conceder acceso a la identidad de una máquina virtual a Azure AD Graph, la cuenta debe tener asignado el rol **administrador global** en Azure AD.

## <a name="connect-to-azure-ad"></a>Conectarse a Azure

Necesita conectarse a Azure AD para asignar la máquina virtual a un grupo, así como concederle permiso para recuperar su pertenencia a un grupo.

```powershell
Connect-AzureAD
```

## <a name="add-your-vm-identity-to-a-group-in-azure-ad"></a>Adición de la identidad de máquina virtual a un grupo en Azure AD

Cuando habilitó la identidad administrada asignada por el sistema en la máquina virtual Windows, creó una entidad de servicio en Azure AD.  Ahora debe agregar la máquina virtual a un grupo.  En el ejemplo siguiente se crea un grupo en Azure AD y se agrega la entidad de servicio de la máquina virtual a dicho grupo:

```powershell
New-AzureADGroup -DisplayName "myGroup" -MailEnabled $false -SecurityEnabled $true -MailNickName "NotSet"
$AzureADGroup = Get-AzureADGroup -Filter "displayName eq 'myGroup'"
$ManagedIdentitiesServicePrincipal = Get-AzureADServicePrincipal -Filter "displayName eq 'myVM'"
Add-AzureADGroupMember -ObjectId $AzureADGroup.ObjectID -RefObjectId $ManagedIdentitiesServicePrincipal.ObjectId
```
## <a name="grant-your-vm-access-to-the-azure-ad-graph-api"></a>Concesión de acceso a la máquina virtual a Azure AD Graph API

Mediante Managed Identities for Azure Resources, el código puede obtener tokens de acceso para autenticarse en aquellos recursos que admitan la autenticación de Azure AD. Microsoft Azure AD Graph API admite la autenticación de Azure AD. En este paso, concederá la entidad de servicio de la identidad de máquina virtual acceso a Azure AD Graph para que pueda consultar la pertenencia al grupo. A las entidades de servicio se les concede acceso a Microsoft o Azure AD Graph mediante los **permisos de aplicación**. El tipo de permiso de aplicación que necesita conceder depende de la entidad a la que desea acceder en Microsoft o Azure AD Graph.

Para este tutorial, concederá a su identidad de máquina virtual la posibilidad de consultar la pertenencia a un grupo mediante el permiso ```Directory.Read.All``` de la aplicación. Para conceder este permiso, necesitará una cuenta de usuario a la que se le asigne el rol de administrador global en Azure AD. Normalmente se concedería un permiso de aplicación al visitar el registro de la aplicación en Azure Portal y agregando allí el permiso. Sin embargo, el servicio Managed Identities for Azure Resources no registra objetos de aplicación en Azure AD, solo registra las entidades de servicio. Para registrar el permiso de la aplicación, se utilizará la herramienta de línea de comandos de PowerShell de Azure AD. 

Azure AD Graph:
- Identificador de la aplicación de la entidad de servicio (se usa al conceder el permiso de aplicación): 00000002-0000-0000-c000-000000000000
- Identificador de recurso (se usa cuando se solicita el token de acceso de Managed Identities for Azure Resources): https://graph.windows.net
- Referencia de ámbito de permiso: [Referencia de permisos de Azure AD Graph](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-permission-scopes)

### <a name="grant-application-permissions-using-azure-ad-powershell"></a>Concesión de permisos de aplicación mediante Azure AD PowerShell

Para usar esta opción necesitará Azure AD PowerShell. Si no lo tiene instalado, [descargue la versión más reciente](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2) antes de continuar.

1. Abra una ventana de PowerShell y conéctese a Azure AD:

   ```powershell
   Connect-AzureAD
   ```

2. Ejecute los siguientes comandos de PowerShell para asignar el permiso de aplicación ``Directory.Read.All`` a la entidad de servicio que representa la identidad de la máquina virtual.

   ```powershell
   $ManagedIdentitiesServicePrincipal = Get-AzureADServicePrincipal -Filter "displayName eq 'myVM'"
   $GraphAppId = "00000002-0000-0000-c000-000000000000"
   $GraphServicePrincipal = Get-AzureADServicePrincipal -Filter "appId eq '$GraphAppId'"
   $PermissionName = "Directory.Read.All"
   $AppRole = $GraphServicePrincipal.AppRoles | Where-Object {$_.Value -eq $PermissionName -and $_.AllowedMemberTypes -contains "Application"}
   New-AzureAdServiceAppRoleAssignment -ObjectId $ManagedIdentitiesServicePrincipal.ObjectId -PrincipalId $ManagedIdentitiesServicePrincipal.ObjectId -ResourceId $GraphServicePrincipal.ObjectId -Id $AppRole.Id
   ``` 

   La salida del comando final debería ser como esta, debería devolver el identificador de la asignación:
        
   `ObjectId`:`gzR5KyLAiUOTiqFhNeWZWBtK7ZKqNJxAiWYXYVHlgMs`

   `ResourceDisplayName`:`Windows Azure Active Directory`

   `PrincipalDisplayName`:`myVM` 

   Si la llamada a `New-AzureAdServiceAppRoleAssignment` genera el error `bad request, one or more properties are invalid`, es posible que el permiso de aplicación ya esté asignado a la entidad de servicio de la identidad de la máquina virtual. Puede usar los siguientes comandos de PowerShell para comprobar si el permiso de aplicación ya existe entre la identidad de la máquina virtual y Azure AD Graph:

   ```powershell
   $ManagedIdentitiesServicePrincipal = Get-AzureADServicePrincipal -Filter "displayName eq '<VM-NAME>'"
   $GraphAppId = "00000002-0000-0000-c000-000000000000"
   $GraphServicePrincipal = Get-AzureADServicePrincipal -Filter "appId eq '$GraphAppId'"
   $PermissionName = "Directory.Read.All"
   $AppRole = $GraphServicePrincipal.AppRoles | Where-Object {$_.Value -eq $PermissionName -and $_.AllowedMemberTypes -contains "Application"}
   Get-AzureADServiceAppRoleAssignment -ObjectId $GraphServicePrincipal.ObjectId | Where-Object {$_.Id -eq $AppRole.Id -and $_.PrincipalId -eq $ManagedIdentitiesServicePrincipal.ObjectId}
   ```

   Puede usar los siguientes comandos de PowerShell para enumerar todos los permisos de aplicación que se han concedido a Azure AD Graph:

   ```powershell
   $GraphAppId = "00000002-0000-0000-c000-000000000000"
   $GraphServicePrincipal = Get-AzureADServicePrincipal -Filter "appId eq '$GraphAppId'"
   Get-AzureADServiceAppRoleAssignment -ObjectId $GraphServicePrincipal.ObjectId
   ``` 

   Puede usar los siguientes comandos de PowerShell para eliminar todos los permisos de aplicación que se han concedido a la identidad de su máquina virtual para Azure AD Graph:

   ```powershell
   $ManagedIdentitiesServicePrincipal = Get-AzureADServicePrincipal -Filter "displayName eq '<VM-NAME>'"
   $GraphAppId = "00000002-0000-0000-c000-000000000000"
   $GraphServicePrincipal = Get-AzureADServicePrincipal -Filter "appId eq '$GraphAppId'"
   $PermissionName = "Directory.Read.All"
   $AppRole = $GraphServicePrincipal.AppRoles | Where-Object {$_.Value -eq $PermissionName -and $_.AllowedMemberTypes -contains "Application"}   
   $ServiceAppRoleAssignment = Get-AzureADServiceAppRoleAssignment -ObjectId $GraphServicePrincipal.ObjectId | Where-Object {$_.Id -eq $AppRole.Id -and $_.PrincipalId -eq $ManagedIdentitiesServicePrincipal.ObjectId}
   Remove-AzureADServiceAppRoleAssignment -AppRoleAssignmentId $ServiceAppRoleAssignment.ObjectId -ObjectId $ManagedIdentitiesServicePrincipal.ObjectId
   ```
 
## <a name="get-an-access-token-using-the-vms-identity-to-call-azure-ad-graph"></a>Obtención de un token de acceso mediante la identidad de la máquina virtual para llamar a Azure AD Graph 

Para usar la identidad administrada asignada por el sistema de la máquina virtual para la autenticación en Azure AD Graph, es preciso realizar las solicitudes desde la máquina virtual.

1. En el portal, vaya a **Máquinas virtuales**, luego a su máquina virtual Windows, y en la hoja **Información general**, haga clic en **Conectar**.  
2. Escriba el **nombre de usuario** y la **contraseña** que usó al crear la máquina virtual Windows.
3. Ahora que ha creado una conexión a Escritorio remoto con la máquina virtual, abra PowerShell en la sesión remota.  
4. Mediante el comando Invoke-WebRequest de PowerShell, realice una solicitud al punto de conexión local de Managed Identities for Azure Resources y obtenga un token de acceso para Azure AD Graph.

   ```powershell
   $response = Invoke-WebRequest -Uri 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https://graph.windows.net' -Method GET -Headers @{Metadata="true"}
   ```

   Convierta la respuesta de un objeto JSON a un objeto de PowerShell.

   ```powershell
   $content = $response.Content | ConvertFrom-Json
   ```

   Extraiga el token de acceso de la respuesta.

   ```powershell
   $AccessToken = $content.access_token
   ```

5. Con el identificador de objeto de la entidad de servicio de la máquina virtual (dicho valor se puede recuperar de la variable declarada en los pasos anteriores: ``$ManagedIdentitiesServicePrincipal.ObjectId``), puede consultar Azure AD Graph API para recuperar su pertenencia a un grupo. Reemplace <OBJECT ID> por el identificador de objeto del paso anterior y <ACCESS-TOKEN> por el token de acceso obtenido anteriormente:

   ```powershell
   Invoke-WebRequest 'https://graph.windows.net/<Tenant ID>/servicePrincipals/<VM Object ID>/getMemberGroups?api-version=1.6' -Method POST -Body '{"securityEnabledOnly":"false"}' -Headers @{Authorization="Bearer $AccessToken"} -ContentType "application/json"
   ```
   
   La respuesta contiene el elemento `Object ID` del grupo al que agregó la entidad de servicio de la máquina virtual en los pasos anteriores.

   Respuesta:

   ```powershell   
   Content : {"odata.metadata":"https://graph.windows.net/<Tenant ID>/$metadata#Collection(Edm.String)","value":["<ObjectID of VM's group membership>"]}
   ```

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha aprendido a utilizar una identidad administrada asignada por el sistema de una máquina virtual Windows para acceder a Azure AD Graph.  Para más información acerca de Azure AD Graph, consulte:

>[!div class="nextstepaction"]
>[Azure AD Graph](https://docs.microsoft.com/azure/active-directory/develop/active-directory-graph-api)
