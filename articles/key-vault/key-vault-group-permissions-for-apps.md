---
title: 'Concesión de permisos para que muchas aplicaciones tengan acceso a un almacén de claves de Azure: Azure Key Vault | Microsoft Docs'
description: Obtenga información sobre cómo conceder permisos para que muchas aplicaciones tengan acceso a almacén de claves.
services: key-vault
author: amitbapat
manager: rkarlin
tags: azure-resource-manager
ms.service: key-vault
ms.topic: tutorial
ms.date: 08/12/2019
ms.author: ambapat
ms.openlocfilehash: 07ee544057ffeb0a5859cc771b124523ec79c9c0
ms.sourcegitcommit: 62bd5acd62418518d5991b73a16dca61d7430634
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/13/2019
ms.locfileid: "68976396"
---
# <a name="grant-several-applications-access-to-a-key-vault"></a>Concesión de acceso a varias aplicaciones a un almacén de claves

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

La directiva de control de acceso puede usarse para conceder acceso a varias aplicaciones a un almacén de claves. Una directiva de control de acceso puede admitir hasta 1024 aplicaciones y se configura como sigue:

1. Cree un grupo de seguridad de Azure Active Directory. 
2. Agregue todas las entidades de servicio asociadas de las aplicaciones al grupo de seguridad.
3. Conceda acceso al grupo de seguridad al almacén de claves.

## <a name="prerequisites"></a>Requisitos previos

Estos son los requisitos previos:
* [Instale Azure PowerShell](/powershell/azure/overview).
* [Instale el módulo PowerShell de Azure Active Directory V2](https://www.powershellgallery.com/packages/AzureAD).
* Permisos para crear o editar grupos en el inquilino de Azure Active Directory. Si no tiene permisos, debe ponerse en contacto con el administrador de Azure Active Directory. Consulte [Información acerca de claves, secretos y certificados de Azure Key Vault](about-keys-secrets-and-certificates.md) para obtener más información sobre los permisos de directiva de acceso de Key Vault.

## <a name="granting-key-vault-access-to-applications"></a>Conceder acceso de Key Vault a aplicaciones

Ejecute los siguientes comandos en PowerShell:

```powershell
# Connect to Azure AD 
Connect-AzureAD 
 
# Create Azure Active Directory Security Group 
$aadGroup = New-AzureADGroup -Description "Contoso App Group" -DisplayName "ContosoAppGroup" -MailEnabled 0 -MailNickName none -SecurityEnabled 1 
 
# Find and add your applications (ServicePrincipal ObjectID) as members to this group 
$spn = Get-AzureADServicePrincipal –SearchString "ContosoApp1" 
Add-AzureADGroupMember –ObjectId $aadGroup.ObjectId -RefObjectId $spn.ObjectId 
 
# You can add several members to this group, in this fashion. 
 
# Set the Key Vault ACLs 
Set-AzKeyVaultAccessPolicy –VaultName ContosoVault –ObjectId $aadGroup.ObjectId `
-PermissionsToKeys decrypt,encrypt,unwrapKey,wrapKey,verify,sign,get,list,update,create,import,delete,backup,restore,recover,purge `
–PermissionsToSecrets get,list,set,delete,backup,restore,recover,purge `
–PermissionsToCertificates get,list,delete,create,import,update,managecontacts,getissuers,listissuers,setissuers,deleteissuers,manageissuers,recover,purge,backup,restore `
-PermissionsToStorage get,list,delete,set,update,regeneratekey,getsas,listsas,deletesas,setsas,recover,backup,restore,purge 
 
# Of course you can adjust the permissions as required 
```

Si tiene que conceder un conjunto de permisos diferente para un grupo de aplicaciones, cree un grupo de seguridad de Azure Active Directory independiente para dichas aplicaciones.

## <a name="next-steps"></a>Pasos siguientes

Más información sobre cómo [proteger el almacén de claves](key-vault-secure-your-key-vault.md).
