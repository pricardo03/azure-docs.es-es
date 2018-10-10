---
title: 'Ejemplo de Azure Policy: auditar si no hay administrador de Azure AD'
description: Esta directiva de ejemplo audita cuándo no hay administrador de Azure Active Directory asignado al servidor SQL.
services: azure-policy
author: DCtheGeek
manager: carmonm
ms.service: azure-policy
ms.topic: sample
ms.date: 09/18/2018
ms.author: dacoulte
ms.custom: mvc
ms.openlocfilehash: f85653f57d5052c60ca2a6170370513c88c3b8cf
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46965303"
---
# <a name="audit-no-azure-active-directory-administrator"></a>Auditar cuándo no hay administrador de Azure Active Directory

Audite cuándo no hay administrador de Azure Active Directory asignado al servidor SQL.

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-template"></a>Plantilla de ejemplo

[!code-json[main](../../../../policy-templates/samples/SQL/audit-if-no-sql-active-directory-admin/azurepolicy.json "Audit SQL DB Level Audit Setting")]

Puede implementar esta plantilla mediante [Azure Portal](#deploy-with-the-portal), con [PowerShell](#deploy-with-powershell) o con la [CLI de Azure](#deploy-with-azure-cli).

## <a name="deploy-with-the-portal"></a>Implementación con el portal

[![Implementación en Azure](http://azuredeploy.net/deploybutton.png)](https://portal.azure.com/?feature.customportal=false&microsoft_azure_policy=true&microsoft_azure_policy_policyinsights=true&feature.microsoft_azure_security_policy=true&microsoft_azure_marketplace_policy=true#blade/Microsoft_Azure_Policy/CreatePolicyDefinitionBlade/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-policy%2Fmaster%2Fsamples%2FSQL%2Faudit-if-no-sql-active-directory-admin%2Fazurepolicy.json)

## <a name="deploy-with-powershell"></a>Implementación con PowerShell

[!INCLUDE [sample-powershell-install](../../../../includes/sample-powershell-install-no-ssh.md)]

```azurepowershell-interactive
$definition = New-AzureRmPolicyDefinition -Name "audit-if-no-sql-active-directory-admin" -DisplayName "Audit If no AAD Admin" -description "Aduit If there is no AAD Admin assigned to this server" -Policy 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/SQL/audit-if-no-sql-active-directory-admin/azurepolicy.rules.json' -Parameter 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/SQL/audit-if-no-sql-active-directory-admin/azurepolicy.parameters.json' -Mode All
$definition
$assignment = New-AzureRMPolicyAssignment -Name <assignmentname> -Scope <scope>  -PolicyDefinition $definition
$assignment
```

### <a name="clean-up-powershell-deployment"></a>Limpieza de la implementación de PowerShell

Ejecute el siguiente comando para quitar el grupo de recursos, la máquina virtual y todos los recursos relacionados.

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="deploy-with-azure-cli"></a>Implementación con la CLI de Azure

[!INCLUDE [sample-cli-install](../../../../includes/sample-cli-install.md)]

```azurecli-interactive
az policy definition create --name 'audit-if-no-sql-active-directory-admin' --display-name 'Audit If no AAD Admin' --description 'Aduit If there is no AAD Admin assigned to this server' --rules 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/SQL/audit-if-no-sql-active-directory-admin/azurepolicy.rules.json' --params 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/SQL/audit-if-no-sql-active-directory-admin/azurepolicy.parameters.json' --mode All

az policy assignment create --name <assignmentname> --scope <scope> --policy "audit-if-no-sql-active-directory-admin" 
```

### <a name="clean-up-azure-cli-deployment"></a>Limpieza de la implementación de la CLI de Azure

Ejecute el siguiente comando para quitar el grupo de recursos, la máquina virtual y todos los recursos relacionados.

```azurecli-interactive
az group delete --name myResourceGroup --yes
```

## <a name="next-steps"></a>Pasos siguientes

- Consulte más ejemplos en [Ejemplos de Azure Policy](index.md).