---
title: 'Cambio del identificador de inquilino del almacén de claves después de mover una suscripción: Azure Key Vault | Microsoft Docs'
description: Aprenda a cambiar el identificador de inquilino de Key Vault después de que una suscripción se haya movido a otro inquilino
services: key-vault
author: amitbapat
manager: rkarlin
tags: azure-resource-manager
ms.service: key-vault
ms.topic: tutorial
ms.date: 08/12/2019
ms.author: ambapat
ms.openlocfilehash: 2159b5b515e22458edf3ba0eb5b6f23f3f37ce95
ms.sourcegitcommit: 5b76581fa8b5eaebcb06d7604a40672e7b557348
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/13/2019
ms.locfileid: "68990105"
---
# <a name="change-a-key-vault-tenant-id-after-a-subscription-move"></a>Cambio del identificador de inquilino de Key Vault después de mover una suscripción

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="q-my-subscription-was-moved-from-tenant-a-to-tenant-b-how-do-i-change-the-tenant-id-for-my-existing-key-vault-and-set-correct-acls-for-principals-in-tenant-b"></a>P: Mi suscripción se ha movido del inquilino A al inquilino B. ¿Cómo cambio el identificador de inquilino de Key Vault existente y establezco las ACL correctas para las entidades de servicio del inquilino B?

Cuando se crea un Key Vault nuevo en una suscripción, se asocia automáticamente a un identificador de inquilino de Azure Active Directory predeterminado de dicha suscripción. Las entradas de la directiva de acceso también se asocian con este identificador de inquilino. Al mover una suscripción de Azure del inquilino A al inquilino B, las entidades de seguridad (usuarios y aplicaciones) de este último inquilino no pueden acceder a las instancias de Key Vault existentes. Para corregir este problema, es preciso:

* Cambiar el identificador de inquilino asociado a todas las instancias de Key Vault existentes en esta suscripción al inquilino B.
* Quitar todas las entradas de la directiva de acceso existentes.
* Agregar nuevas entradas de la directiva de acceso asociadas al inquilino B.

Por ejemplo, si tiene el Key Vault 'myvault' en una suscripción que se ha movido del inquilino A al inquilino B, con el siguiente código podrá cambiar el identificador de inquilino de este Key Vault y quitar directivas de acceso antiguas.

<pre>
Select-AzSubscription -SubscriptionId YourSubscriptionID                   # Select your Azure Subscription
$vaultResourceId = (Get-AzKeyVault -VaultName myvault).ResourceId          # Get your Keyvault's Resource ID 
$vault = Get-AzResource –ResourceId $vaultResourceId -ExpandProperties     # Get the properties for your Keyvault
$vault.Properties.TenantId = (Get-AzContext).Tenant.TenantId               # Change the Tenant that your Keyvault resides in
$vault.Properties.AccessPolicies = @()                                     # Accesspolicies can be updated with real
                                                                           # applications/users/rights so that it does not need to be                                                                              # done after this whole activity. Here we are not setting 
                                                                           # any access policies. 
Set-AzResource -ResourceId $vaultResourceId -Properties $vault.Properties  # Modifies the kevault's properties.
</pre>

Dado que este almacén estaba en el inquilino A antes de mover la suscripción original, el valor original de **$vault. Properties.TenantId** es el inquilino A, mientras que **(Get-AzContext).Tenant.TenantId** es el inquilino B.

Ahora que el almacén está asociado al identificador de inquilino correcto y que se han quitado las entradas de la directiva de acceso antiguas, establezca nuevas entradas de la directiva de acceso con [Set-AzKeyVaultAccessPolicy](https://docs.microsoft.com/powershell/module/az.keyvault/Set-azKeyVaultAccessPolicy).

## <a name="next-steps"></a>Pasos siguientes

Si le queda alguna duda al respecto de Azure Key Vault, visite los [foros de Azure Key Vault](https://social.msdn.microsoft.com/forums/azure/home?forum=AzureKeyVault).
