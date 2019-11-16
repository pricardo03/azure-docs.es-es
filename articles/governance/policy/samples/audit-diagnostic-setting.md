---
title: 'Ejemplo: Auditar la configuración de diagnóstico'
description: Esta definición de directiva de ejemplo audita si la configuración de diagnóstico no está habilitada para los tipos de recursos especificados.
ms.date: 01/23/2019
ms.topic: sample
ms.openlocfilehash: 5940fcc86221d4089609ac2d0828b0c710de7fbc
ms.sourcegitcommit: a107430549622028fcd7730db84f61b0064bf52f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/14/2019
ms.locfileid: "74076415"
---
# <a name="sample---audit-diagnostic-setting"></a>Ejemplo: Auditar la configuración de diagnóstico

Esta directiva integrada audita si la configuración del diagnóstico no está habilitada para los tipos de recursos especificados. Se especifica una matriz de tipos de recursos para comprobar si la configuración de diagnóstico está habilitada.

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-template"></a>Plantilla de ejemplo

[!code-json[main](../../../../policy-templates/samples/Monitoring/audit-diagnostic-setting/azurepolicy.json "Audit diagnostic setting")]

Puede implementar esta plantilla mediante [Azure Portal](#deploy-with-the-portal), con [PowerShell](#deploy-with-powershell) o con la [CLI de Azure](#deploy-with-azure-cli). Para obtener la directiva integrada, use el identificador `7f89b1eb-583c-429a-8828-af049802c1d9`.

## <a name="parameters"></a>Parámetros

Para pasar el valor del parámetro, use el siguiente contacto:

```json
{"listOfResourceTypes":{"value":["Microsoft.Cache/Redis","Microsoft.Compute/virtualmachines"]}}
```

## <a name="deploy-with-the-portal"></a>Implementación con el portal

Al asignar una directiva, seleccione **Configuración de diagnóstico de auditoría** en las definiciones integradas disponibles.

## <a name="deploy-with-powershell"></a>Implementación con PowerShell

[!INCLUDE [sample-powershell-install](../../../../includes/sample-powershell-install-no-ssh-az.md)]

```azurepowershell-interactive
$definition = Get-AzPolicyDefinition -Id /providers/Microsoft.Authorization/policyDefinitions/7f89b1eb-583c-429a-8828-af049802c1d9

New-AzPolicyAssignment -name "Audit diagnostics" -PolicyDefinition $definition -PolicyParameter '{"listOfResourceTypes":{"value":["Microsoft.Cache/Redis","Microsoft.Compute/virtualmachines"]}}' -Scope <scope>
```

### <a name="clean-up-powershell-deployment"></a>Limpieza de la implementación de PowerShell

Ejecute el siguiente comando para quitar el grupo de recursos, la máquina virtual y todos los recursos relacionados.

```azurepowershell-interactive
Remove-AzPolicyAssignment -Name "Audit diagnostics" -Scope <scope>
```

## <a name="deploy-with-azure-cli"></a>Implementación con la CLI de Azure

[!INCLUDE [sample-cli-install](../../../../includes/sample-cli-install.md)]

```azurecli-interactive
az policy assignment create --scope <scope> --name "Audit diagnostics" --policy 7f89b1eb-583c-429a-8828-af049802c1d9 --params '{"listOfResourceTypes":{"value":["Microsoft.Cache/Redis","Microsoft.Compute/virtualmachines"]}}'
```

### <a name="clean-up-azure-cli-deployment"></a>Limpieza de la implementación de la CLI de Azure

Ejecute el siguiente comando para quitar el grupo de recursos, la máquina virtual y todos los recursos relacionados.

```azurecli-interactive
az policy assignment delete --name "Audit diagnostics" --resource-group myResourceGroup
```

## <a name="next-steps"></a>Pasos siguientes

- Consulte más ejemplos en [Ejemplos de Azure Policy](index.md).