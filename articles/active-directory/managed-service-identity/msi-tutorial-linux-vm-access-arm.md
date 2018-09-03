---
title: Uso de una característica Managed Service Identity asignada por el usuario en una máquina virtual Linux para acceder a Azure Resource Manager
description: Tutorial que le guía por el proceso para usar una identidad de Managed Service Identity asignada por el usuario con una máquina virtual Linux para acceder a Azure Resource Manager.
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
ms.date: 12/22/2017
ms.author: daveba
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 98be87d62861295536a75201ff93e809c9ba120b
ms.sourcegitcommit: f1e6e61807634bce56a64c00447bf819438db1b8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/24/2018
ms.locfileid: "42886859"
---
# <a name="tutorial-use-a-user-assigned-identity-on-a-linux-vm-to-access-azure-resource-manager"></a>Tutorial: usar una identidad que asignó el usuario en una máquina virtual de Linux para obtener acceso a Azure Resource Manager

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

En este tutorial se explica cómo crear una identidad asignada por el usuario, asignarla a una máquina virtual Linux y, a continuación, utilizar esa identidad para acceder a la API de Azure Resource Manager. Azure administra automáticamente las identidades de servicio administradas. Estas identidades permiten autenticarse en servicios que admiten la autenticación de Azure AD, sin necesidad de incluir credenciales en el código. 

Azure administra automáticamente las identidades de servicio administradas. Estas identidades permiten autenticarse en servicios que admiten la autenticación de Azure AD, sin necesidad de incluir credenciales en el código.

Aprenderá a:

> [!div class="checklist"]
> * Creación de una identidad asignada por el usuario
> * Asignar una identidad asignada por el usuario a una VM Linux 
> * Conceder a la identidad asignada por el usuario acceso a un grupo de recursos en Azure Resource Manager 
> * Obtener un token de acceso mediante la identidad asignada por el usuario y usarla para llamar a Azure Resource Manager 

## <a name="prerequisites"></a>Requisitos previos

[!INCLUDE [msi-qs-configure-prereqs](../../../includes/active-directory-msi-qs-configure-prereqs.md)]

[!INCLUDE [msi-tut-prereqs](../../../includes/active-directory-msi-tut-prereqs.md)]

[Iniciar sesión en Azure Portal](https://portal.azure.com)

[Crear una máquina virtual Linux](/azure/virtual-machines/linux/quick-create-portal)

Si decide instalar y usar la CLI localmente, para esta guía de inicio rápido es preciso que ejecute la CLI de Azure versión 2.0.4 o posterior. Ejecute `az --version` para encontrar la versión. Si necesita instalarla o actualizarla, consulte [Instalación de la CLI de Azure 2.0]( /cli/azure/install-azure-cli).

## <a name="create-a-user-assigned-identity"></a>Creación de una identidad asignada por el usuario

1. Si usa la consola de la CLI (en lugar de una sesión de Azure Cloud Shell), primero inicie sesión en Azure. Use una cuenta asociada a la suscripción de Azure en la que desee crear la nueva identidad asignada por el usuario:

    ```azurecli
    az login
    ```

2. Cree una identidad asignada por el usuario mediante el cmdlet [az identity create](/cli/azure/identity#az-identity-create). El parámetro `-g` especifica el grupo de recursos en que se crea la característica Managed Service Identity, mientras que el parámetro `-n` especifica su nombre. Asegúrese de reemplazar los valores de los parámetros `<RESOURCE GROUP>` y `<MSI NAME>` por sus propios valores:
    
[!INCLUDE[ua-character-limit](~/includes/managed-identity-ua-character-limits.md)]


```azurecli-interactive
az identity create -g <RESOURCE GROUP> -n <MSI NAME>
```

La respuesta contiene detalles de la identidad asignada por el usuario que se ha creado, de forma similar al ejemplo siguiente. Anote el valor `id` para la identidad asignada por el usuario, ya que se usará en el paso siguiente:

```json
{
"clientId": "73444643-8088-4d70-9532-c3a0fdc190fz",
"clientSecretUrl": "https://control-westcentralus.identity.azure.net/subscriptions/<SUBSCRIPTON ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<MSI NAME>/credentials?tid=5678&oid=9012&aid=12344643-8088-4d70-9532-c3a0fdc190fz",
"id": "/subscriptions/<SUBSCRIPTON ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<MSI NAME>",
"location": "westcentralus",
"name": "<MSI NAME>",
"principalId": "9012",
"resourceGroup": "<RESOURCE GROUP>",
"tags": {},
"tenantId": "733a8f0e-ec41-4e69-8ad8-971fc4b533bl",
"type": "Microsoft.ManagedIdentity/userAssignedIdentities"
}
```

## <a name="assign-a-user-assigned-identity-to-your-linux-vm"></a>Asignación de una identidad asignada por el usuario a la VM Linux

Los clientes pueden usar una identidad asignada por el usuario en varios recursos de Azure. Use los siguientes comandos para asignar la identidad asignada por el usuario a una única máquina virtual. Use la propiedad `Id` devuelta en el paso anterior para el parámetro `-IdentityID`.

Asigne la característica Managed Service Identity asignada por el usuario a la máquina virtual Linux mediante el cmdlet [az vm assign-identity](/cli/azure/vm#az-vm-assign-identity). Asegúrese de reemplazar los valores de los parámetros `<RESOURCE GROUP>` y `<VM NAME>` con sus propios valores. Use la propiedad `id` devuelta en el paso anterior para el valor del parámetro `--identities`.

```azurecli-interactive
az vm assign-identity -g <RESOURCE GROUP> -n <VM NAME> --identities "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<MSI NAME>"
```

## <a name="grant-your-user-assigned-identity-access-to-a-resource-group-in-azure-resource-manager"></a>Concesión a la identidad asignada por el usuario de acceso a un grupo de recursos en Azure Resource Manager 

Managed Service Identity (MSI) proporciona las identidades que el código puede usar para solicitar tokens de acceso para autenticarse en APIs de recursos que admiten la autenticación de Azure AD. En este tutorial, el código accederá a la API de Azure Resource Manager.  

Antes de que el código pueda acceder a la API, debe conceder a la identidad acceso a un recurso en Azure Resource Manager. En este caso, se trata del grupo de recursos que contiene la máquina virtual. Actualice los valores de `<SUBSCRIPTION ID>` y `<RESOURCE GROUP>` según corresponda en su entorno. Además, asegúrese de reemplazar `<MSI PRINCIPALID>` por la propiedad `principalId` devuelta por el comando `az identity create` en [Creación de una MSI asignada por el usuario](#create-a-user-assigned-msi):

```azurecli-interactive
az role assignment create --assignee <MSI PRINCIPALID> --role 'Reader' --scope "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/<RESOURCE GROUP> "
```

La respuesta contiene detalles de la asignación de roles que se ha creado, de forma similar al ejemplo siguiente:

```json
{
  "id": "/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.Authorization/roleAssignments/b402bd74-157f-425c-bf7d-zed3a3a581ll",
  "name": "b402bd74-157f-425c-bf7d-zed3a3a581ll",
  "properties": {
    "principalId": "f5fdfdc1-ed84-4d48-8551-999fb9dedfbl",
    "roleDefinitionId": "/subscriptions/<SUBSCRIPTION ID>/providers/Microsoft.Authorization/roleDefinitions/acdd72a7-3385-48ef-bd42-f606fba81ae7",
    "scope": "/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>"
  },
  "resourceGroup": "<RESOURCE GROUP>",
  "type": "Microsoft.Authorization/roleAssignments"
}

```

## <a name="get-an-access-token-using-the-vms-identity-and-use-it-to-call-resource-manager"></a>Obtenga un token de acceso con la identidad de la máquina virtual y úselo para llamar a Resource Manager 

En el resto del tutorial, vamos a trabajar desde la máquina virtual que se creó anteriormente.

Para completar estos pasos, necesitará un cliente SSH. Si usa Windows, puede usar el cliente SSH en el [Subsistema de Windows para Linux](https://msdn.microsoft.com/commandline/wsl/about). 

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
2. En el portal, vaya a **Máquinas virtuales** y diríjase a la máquina virtual Linux y, en **Introducción**, haga clic en **Conectar**. Copie la cadena para conectarse a la máquina virtual.
3. Conéctese a la máquina virtual con el cliente SSH que elija. Si usa Windows, puede usar el cliente SSH en el [Subsistema de Windows para Linux](https://msdn.microsoft.com/commandline/wsl/about). Si necesita ayuda para configurar las claves del cliente de SSH, consulte [Uso de SSH con Windows en Azure](~/articles/virtual-machines/linux/ssh-from-windows.md) o [Creación y uso de un par de claves SSH pública y privada para máquinas virtuales Linux en Azure](~/articles/virtual-machines/linux/mac-create-ssh-keys.md).
4. En la ventana del terminal, con CURL, realice una solicitud con el punto de conexión de Azure Instance Metadata Service (IMDS) para obtener un token de acceso para Azure Resource Manager.  

   En el ejemplo siguiente, se muestra la solicitud CURL para adquirir un token de acceso. Asegúrese de reemplazar `<CLIENT ID>` por la propiedad `clientId` devuelta por el comando `az identity create` en [Creación de una identidad asignada por el usuario](#create-a-user-assigned-msi): 
    
   ```bash
   curl -H Metadata:true "http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fmanagement.azure.com/&client_id=<MSI CLIENT ID>"   
   ```
    
    > [!NOTE]
    > El valor del parámetro `resource` debe coincidir exactamente con el que Azure AD espera. Al usar el identificador de recurso de Azure Resource Manager, debe incluir la barra diagonal final en el URI. 
    
    La respuesta incluye el token de acceso que necesita para acceder a Azure Resource Manager. 
    
    Ejemplo de respuesta:  

    ```bash
    {
    "access_token":"eyJ0eXAiOi...",
    "refresh_token":"",
    "expires_in":"3599",
    "expires_on":"1504130527",
    "not_before":"1504126627",
    "resource":"https://management.azure.com",
    "token_type":"Bearer"
    } 
    ```

5. Use este token de acceso para acceder a Azure Resource Manager y lea las propiedades del grupo de recursos al que previamente concedió acceso a la identidad asignada por el usuario. No olvide reemplazar `<SUBSCRIPTION ID>` y `<RESOURCE GROUP>` por los valores especificados anteriormente, y `<ACCESS TOKEN>` por el token devuelto en el paso anterior.

    > [!NOTE]
    > La dirección URL distingue mayúsculas de minúsculas, por lo tanto, asegúrese de que usa las mismas mayúsculas y minúsculas que al asignar el nombre al grupo de recursos, así como la "G" mayúscula de `resourceGroups`.  

    ```bash 
    curl https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>?api-version=2016-09-01 -H "Authorization: Bearer <ACCESS TOKEN>" 
    ```

    La respuesta contiene la información específica del grupo de recursos, de forma similar al ejemplo siguiente: 

    ```bash
    {
    "id":"/subscriptions/<SUBSCRIPTION ID>/resourceGroups/DevTest",
    "name":"DevTest",
    "location":"westus",
    "properties":{"provisioningState":"Succeeded"}
    } 
    ```
    
## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha aprendido a crear una identidad asignada por el usuario y a adjuntarla a una máquina virtual Linux para acceder a la API de Azure Resource Manager.  Para obtener más información sobre Azure Resource Manager, vea:

> [!div class="nextstepaction"]
>[Azure Resource Manager](/azure/azure-resource-manager/resource-group-overview)

