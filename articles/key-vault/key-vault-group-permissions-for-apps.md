---
title: Concesión de permisos para que muchas aplicaciones tengan acceso a un almacén de Azure Key Vault | Microsoft Docs
description: Obtenga información sobre cómo conceder permisos para que muchas aplicaciones tengan acceso a almacén de claves.
services: key-vault
documentationcenter: ''
author: amitbapat
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: 785d4e40-fb7b-485a-8cbc-d9c8c87708e6
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 10/12/2018
ms.author: ambapat
ms.openlocfilehash: 4ad6a18f9937fcc7d24bebc3ac197e23990ff59e
ms.sourcegitcommit: 3a02e0e8759ab3835d7c58479a05d7907a719d9c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/13/2018
ms.locfileid: "49309253"
---
# <a name="grant-several-applications-access-to-a-key-vault"></a>Concesión de acceso a varias aplicaciones a un almacén de claves

La directiva de control de acceso puede usarse para conceder acceso a varias aplicaciones a un almacén de claves. Una directiva de control de acceso puede admitir hasta 1024 aplicaciones y se configura como sigue:

1. Cree un grupo de seguridad de Azure Active Directory. 
2. Agregue todas las entidades de servicio asociadas de las aplicaciones al grupo de seguridad.
3. Conceda acceso al grupo de seguridad al almacén de claves.

Instalación de los requisitos previos:
* [Instale el módulo PowerShell de Azure Active Directory V2](https://www.powershellgallery.com/packages/AzureAD).
* [Instale Azure PowerShell](/powershell/azure/overview).
* Para ejecutar los siguientes comandos, necesita permisos para crear y editar grupos en el inquilino de Azure Active Directory. Si no tiene permisos, debe ponerse en contacto con el administrador de Azure Active Directory. Consulte [Información acerca de claves, secretos y certificados de Azure Key Vault](about-keys-secrets-and-certificates.md) para obtener más información sobre los permisos de directiva de acceso de Key Vault.

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
Set-AzureRmKeyVaultAccessPolicy –VaultName ContosoVault –ObjectId $aadGroup.ObjectId `
-PermissionsToKeys decrypt,encrypt,unwrapKey,wrapKey,verify,sign,get,list,update,create,import,delete,backup,restore,recover,purge `
–PermissionsToSecrets get,list,set,delete,backup,restore,recover,purge `
–PermissionsToCertificates get,list,delete,create,import,update,managecontacts,getissuers,listissuers,setissuers,deleteissuers,manageissuers,recover,purge,backup,restore `
-PermissionsToStorage get,list,delete,set,update,regeneratekey,getsas,listsas,deletesas,setsas,recover,backup,restore,purge 
 
# Of course you can adjust the permissions as required 
```

Si tiene que conceder un conjunto de permisos diferente para un grupo de aplicaciones, cree un grupo de seguridad de Azure Active Directory independiente para dichas aplicaciones.

## <a name="next-steps"></a>Pasos siguientes

Más información sobre cómo [proteger el almacén de claves](key-vault-secure-your-key-vault.md).
