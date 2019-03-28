---
title: Uso de identidades administradas asignadas por el sistema de una máquina virtual Linux para acceder a Azure AD Graph API
description: Este tutorial contiene directrices acerca de cómo utilizar una identidad administrada asignada por el sistema de una máquina virtual Linux para acceder a Azure AD Graph API.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: daveba
ms.service: active-directory
ms.subservice: msi
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/20/2018
ms.author: markvi
ms.collection: M365-identity-device-management
ms.openlocfilehash: 481cb560daa26e59de2c78cc64bab9fb168eed58
ms.sourcegitcommit: f0f21b9b6f2b820bd3736f4ec5c04b65bdbf4236
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2019
ms.locfileid: "58445394"
---
# <a name="tutorial-use-a-linux-vm-system-assigned-managed-identity-to-access-azure-ad-graph-api"></a>Tutorial: Uso de identidades administradas asignadas por el sistema de una máquina virtual Linux para acceder a Azure AD Graph API

[!INCLUDE [preview-notice](~/includes/active-directory-msi-preview-notice.md)]

En este tutorial, se explica cómo se usar una identidad administrada asignada por el sistema de una máquina virtual Linux para acceder a Azure AD Graph API para recuperar su pertenencia al grupo. Azure administra automáticamente Managed Identities for Azure Resources, que le permiten autenticar los servicios que admiten la autenticación de Azure AD sin necesidad de insertar credenciales en el código.  

Para este tutorial, deberá consultar la pertenencia de su identidad de máquina virtual a los grupos de Azure AD. La información de grupo se usa a menudo en las decisiones de autorización. En segundo plano, la identidad administrada de la máquina virtual se representa mediante una **entidad de servicio** en Azure AD. 

> [!div class="checklist"]
> * Conectarse a Azure
> * Adición de la identidad de máquina virtual a un grupo en Azure AD 
> * Concesión del acceso a la identidad de la máquina virtual a Azure AD Graph 
> * Obtención de un token de acceso mediante la identidad de la máquina virtual y úselo para llamar a Azure AD Graph

## <a name="prerequisites"></a>Requisitos previos

[!INCLUDE [msi-tut-prereqs](../../../includes/active-directory-msi-tut-prereqs.md)]

- [Instalación de la versión más reciente de la CLI de Azure](https://docs.microsoft.com/cli/azure/install-azure-cli)

- Para conceder acceso a la identidad de una máquina virtual a Azure AD Graph, la cuenta debe tener asignado el rol **administrador global** en Azure AD.

## <a name="connect-to-azure-ad"></a>Conectarse a Azure

Necesita conectarse a Azure AD para asignar la máquina virtual a un grupo, así como concederle permiso para recuperar su pertenencia a un grupo.

```cli
az login
```

## <a name="add-your-vms-identity-to-a-group-in-azure-ad"></a>Adición de la identidad de la máquina virtual a un grupo en Azure AD

Cuando habilitó la identidad administrada asignada por el sistema en la máquina virtual Linux, creó una entidad de servicio en Azure AD.  Debe agregar la máquina virtual a un grupo. Consulte el siguiente artículo para obtener instrucciones sobre cómo agregar la máquina virtual a un grupo en Azure AD:

- [Adición de miembros del grupo](/cli/azure/ad/group/member?view=azure-cli-latest#az-ad-group-member-add)

## <a name="grant-your-vm-access-to-the-azure-ad-graph-api"></a>Concesión de acceso a la máquina virtual a Azure AD Graph API

Mediante Managed Identities for Azure Resources, el código puede obtener tokens de acceso para autenticarse en aquellos recursos que admitan la autenticación de Azure AD. Microsoft Azure AD Graph API admite la autenticación de Azure AD. En este paso, concederá la entidad de servicio de la identidad de máquina virtual acceso a Azure AD Graph para que pueda consultar la pertenencia al grupo. A las entidades de servicio se les concede acceso a Microsoft o Azure AD Graph mediante los **permisos de aplicación**. El tipo de permiso de aplicación que necesita conceder depende de la entidad a la que desea acceder en Microsoft o Azure AD Graph.

Para este tutorial, concederá a su identidad de máquina virtual la posibilidad de consultar la pertenencia a un grupo mediante el permiso `Directory.Read.All` de la aplicación. Para conceder este permiso, necesitará una cuenta de usuario a la que se le asigne el rol de administrador global en Azure AD. Normalmente se concedería un permiso de aplicación al visitar el registro de la aplicación en Azure Portal y agregando allí el permiso. Sin embargo, el servicio Managed Identities for Azure Resources no registra objetos de aplicación en Azure AD, solo registra las entidades de servicio. Para registrar el permiso de la aplicación, se utilizará la herramienta de línea de comandos de PowerShell de Azure AD. 

Azure AD Graph:
- Identificador de la aplicación de la entidad de servicio (se usa al conceder el permiso de aplicación): 00000002-0000-0000-c000-000000000000
- Identificador de recurso (se usa cuando se solicita el token de acceso de Managed Identities for Azure Resources): https://graph.windows.net
- Referencia de ámbito de permiso: [Referencia de permisos de Azure AD Graph](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-permission-scopes)

### <a name="grant-application-permissions-using-curl"></a>Concesión de permisos de aplicación mediante CURL

1. Recupere un token para realizar solicitudes CURL:

   ```cli
   az account get-access-token --resource "https://graph.windows.net/"
   ```

2. Necesitará recuperar y anotar el `objectId` de la máquina virtual. Se utiliza en los pasos siguientes para conceder permisos a la máquina virtual para que lea su pertenencia a un grupo. Reemplace `<ACCESS TOKEN>` por el token de acceso que se recuperó en el paso anterior.

   ```bash
   curl 'https://graph.windows.net/myorganization/servicePrincipals?$filter=startswith%28displayName%2C%27myVM%27%29&api-version=1.6' -H "Authorization: Bearer <ACCESS TOKEN>"
   ```

3. Con el appID de Azure AD Graph, 00000002-0000-0000-c000-000000000000, recupere y anote el `objectId` para `odata.type: Microsoft.DirectoryServices.ServicePrincipal` y el `id` para el permiso de función de la aplicación `Directory.Read.All`.  Reemplace `<ACCESS TOKEN>` por el token de acceso que se recuperó anteriormente.

   ```bash
   curl "https://graph.windows.net/myorganization/servicePrincipals?api-version=1.6&%24filter=appId%20eq%20'00000002-0000-0000-c000-000000000000'" -H "Authorization: Bearer <ACCESS TOKEN>"
   ```

   Respuesta:

   ```json
   "odata.metadata":"https://graph.windows.net/myorganization/$metadata#directoryObjects",
   "value":[
      {
         "odata.type":"Microsoft.DirectoryServices.ServicePrincipal",
         "objectType":"ServicePrincipal",
         "objectId":"81789304-ff96-402b-ae73-07ec0db26721",
         "deletionTimestamp":null,
         "accountEnabled":true,
         "addIns":[
         ],
         "alternativeNames":[
         ],
         "appDisplayName":"Windows Azure Active Directory",
         "appId":"00000002-0000-0000-c000-000000000000",
         "appOwnerTenantId":"f8cdef31-a31e-4b4a-93e4-5f571e91255a",
         "appRoleAssignmentRequired":false,
         "appRoles":[
            {
               "allowedMemberTypes":[
                  "Application"
               ],
               "description":"Allows the app to read data in your company or school directory, such as users, groups, and apps.",
               "displayName":"Read directory data",
               "id":"5778995a-e1bf-45b8-affa-663a9f3f4d04",
               "isEnabled":true,
               "value":"Directory.Read.All"
            },
            {
               //other appRoles values
            }
   ``` 

4. Ahora, conceda a la entidad de servicio de la máquina virtual acceso de lectura a los objetos de directorio de Azure AD mediante Azure AD Graph API.  El valor `id` es el valor para el permiso de rol de la aplicación `Directory.Read.All` y el valor `resourceId` es el valor `objectId` para la entidad de servicio `odata.type:Microsoft.DirectoryServices.ServicePrincipal` (los valores que anotó en el paso anterior).

   ```bash
   curl "https://graph.windows.net/myorganization/servicePrincipals/<VM Object ID>/appRoleAssignments?api-version=1.6" -X POST -d '{"id":"5778995a-e1bf-45b8-affa-663a9f3f4d04","principalId":"<VM Object ID>","resourceId":"81789304-ff96-402b-ae73-07ec0db26721"}'-H "Content-Type: application/json" -H "Authorization: Bearer <ACCESS TOKEN>"
   ``` 
 
## <a name="get-an-access-token-using-the-vms-identity-to-call-azure-ad-graph"></a>Obtención de un token de acceso mediante la identidad de la máquina virtual para llamar a Azure AD Graph 

Para completar estos pasos, necesitará un cliente SSH. Si usa Windows, puede usar el cliente SSH en el [Subsistema de Windows para Linux](https://msdn.microsoft.com/commandline/wsl/about). Si necesita ayuda para configurar las claves del cliente de SSH, consulte [Uso de SSH con Windows en Azure](../../virtual-machines/linux/ssh-from-windows.md) o [Creación y uso de un par de claves SSH pública y privada para máquinas virtuales Linux en Azure](../../virtual-machines/linux/mac-create-ssh-keys.md).

1. En el portal, vaya a la máquina virtual Linux y, en **Información general**, haga clic en **Conectar**.  
2. **Conéctese** a la máquina virtual con el cliente SSH que elija. 
3. En la ventana del terminal, utilice CURL para realizar una solicitud al punto de conexión de la característica Managed Identities for Azure Resources y obtener un token de acceso para Azure AD Graph.  
    
   ```bash
   curl 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https://graph.windows.net' -H Metadata:true
   ```    
   La respuesta incluye el token de acceso que necesita para acceder a Azure AD Graph.

   Respuesta:

   ```bash
   {
       "access_token":"eyJ0eXAiOiJKV...",
       "expires_in":"3599",
       "expires_on":"1519622535",
       "not_before":"1519618635",
       "resource":"https://graph.windows.net",
       "token_type":"Bearer"
   }
   ```

4. Con el identificador de objeto de la entidad de servicio de la máquina virtual (el valor que recuperó en pasos anteriores), puede consultar Azure AD Graph API para recuperar su pertenencia a un grupo. Reemplace `<OBJECT-ID>` por el identificador de objeto de la entidad de servicio de la máquina virtual y `<ACCESS-TOKEN>` por el token de acceso obtenido previamente:

   ```bash
   curl 'https://graph.windows.net/myorganization/servicePrincipals/<OBJECT-ID>/getMemberGroups?api-version=1.6' -X POST -d "{\"securityEnabledOnly\": false}" -H "Content-Type: application/json" -H "Authorization: Bearer <ACCESS-TOKEN>"
   ```

   Respuesta:

   ```bash   
   Content : {"odata.metadata":"https://graph.windows.net/myorganization/$metadata#Collection(Edm.String)","value":["<ObjectID of VM's group membership>"]}
   ```

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha aprendido a utilizar una identidad administrada asignada por el sistema de una máquina virtual Linux para acceder a Azure AD Graph.  Para más información acerca de Azure AD Graph, consulte:

>[!div class="nextstepaction"]
>[Azure AD Graph](https://docs.microsoft.com/azure/active-directory/develop/active-directory-graph-api)
