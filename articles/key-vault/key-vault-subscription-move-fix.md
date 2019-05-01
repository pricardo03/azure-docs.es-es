---
title: 'Cambio del identificador de inquilino del almacén de claves después de mover una suscripción: Azure Key Vault | Microsoft Docs'
description: Aprenda a cambiar el identificador de inquilino de Key Vault después de que una suscripción se haya movido a otro inquilino
services: key-vault
author: amitbapat
manager: barbkess
tags: azure-resource-manager
ms.service: key-vault
ms.topic: conceptual
ms.date: 01/07/2019
ms.author: ambapat
ms.openlocfilehash: f32146697be234a8a288ff991b1f7adf6e76dc7e
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2019
ms.locfileid: "64724495"
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
Select-AzSubscription -SubscriptionId YourSubscriptionID
$vaultResourceId = (Get-AzKeyVault -VaultName myvault).ResourceId
$vault = Get-AzResource –ResourceId $vaultResourceId -ExpandProperties
$vault.Properties.TenantId = (Get-AzContext).Tenant.TenantId
$vault.Properties.AccessPolicies = @()
Set-AzResource -ResourceId $vaultResourceId -Properties $vault.Properties
</pre>

Dado que este almacén estaba en el inquilino A antes de mover la suscripción original, el valor original de **$vault. Properties.TenantId** es el inquilino A, mientras que **(Get-AzContext).Tenant.TenantId** es el inquilino B.

Ahora que el almacén está asociado al identificador de inquilino correcto y que se han quitado las entradas de la directiva de acceso antiguas, establezca nuevas entradas de la directiva de acceso con [Set-AzKeyVaultAccessPolicy](https://docs.microsoft.com/powershell/module/az.keyvault/Set-azKeyVaultAccessPolicy).

## <a name="next-steps"></a>Pasos siguientes

Si le queda alguna duda al respecto de Azure Key Vault, visite los [foros de Azure Key Vault](https://social.msdn.microsoft.com/forums/azure/home?forum=AzureKeyVault).
