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
ms.date: 12/01/2016
ms.author: ambapat
ms.openlocfilehash: 421ceca1453b9e3b97c5ede520ec92372baf2020
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2018
ms.locfileid: "44299668"
---
# <a name="grant-permission-to-many-applications-to-access-a-key-vault"></a>Concesión de permisos para que muchas aplicaciones tengan acceso a almacén de claves

## <a name="q-i-have-several-applications-that-need-to-access-a-key-vault-how-can-i-give-these-applications-up-to-1024-access-to-key-vault"></a>P: Tengo varias aplicaciones que necesitan tener acceso a un almacén de claves. ¿Cómo puedo dar acceso a estas aplicaciones (máximo de 1024) a Key Vault?

La directiva de control de acceso de Key Vault admite un máximo de 1024 entradas. Sin embargo, puede crear un grupo de seguridad de Azure Active Directory. Agregue todas las entidades de servicio asociadas a este grupo de seguridad y, luego, conceda acceso a este grupo de seguridad a Key Vault.

Instalación de los requisitos previos:
* [Instale el módulo PowerShell de Azure Active Directory V2](https://www.powershellgallery.com/packages/AzureAD).
* [Instale Azure PowerShell](/powershell/azure/overview).
* Para ejecutar los siguientes comandos, necesita permisos para crear y editar grupos en el inquilino de Azure Active Directory. Si no tiene permisos, debe ponerse en contacto con el administrador de Azure Active Directory.

Ejecute los siguientes comandos en PowerShell.

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
Set-AzureRmKeyVaultAccessPolicy –VaultName ContosoVault –ObjectId $aadGroup.ObjectId -PermissionsToKeys all –PermissionsToSecrets all –PermissionsToCertificates all 
 
# Of course you can adjust the permissions as required 
```

Si tiene que conceder un conjunto de permisos diferente para un grupo de aplicaciones, cree un grupo de seguridad de Azure Active Directory independiente para dichas aplicaciones.

## <a name="next-steps"></a>Pasos siguientes

Más información sobre cómo [proteger el almacén de claves](key-vault-secure-your-key-vault.md).
