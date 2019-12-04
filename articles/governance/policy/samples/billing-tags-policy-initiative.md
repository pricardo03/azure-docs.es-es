---
title: 'Ejemplo: Iniciativa de directiva de etiquetas de facturación'
description: Este ejemplo de iniciativa de directiva (conjunto de directivas) requiere valores de etiqueta específicos definidos en parámetros para el centro de costo y el nombre del producto.
ms.date: 01/23/2019
ms.topic: sample
ms.openlocfilehash: 449909b160c2b811c62cd8c6592e74dd0f714e41
ms.sourcegitcommit: 95931aa19a9a2f208dedc9733b22c4cdff38addc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/25/2019
ms.locfileid: "74463392"
---
# <a name="sample---billing-tags-policy-initiative"></a>Ejemplo: Iniciativa de directiva de etiquetas de facturación

Este conjunto de directivas requiere valores de etiqueta especificados para el nombre de producto y el centro de costos. Usa directivas integradas para aplicar las etiquetas requeridas. Se especifican los valores requeridos para las etiquetas.

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-template"></a>Plantilla de ejemplo

[!code-json[main](../../../../policy-templates/samples/PolicyInitiatives/multiple-billing-tags/azurepolicyset.json "Billing Tags Policy Initiative")]

Puede implementar esta plantilla con [PowerShell](#deploy-with-powershell).

## <a name="deploy-with-powershell"></a>Implementación con PowerShell

[!INCLUDE [sample-powershell-install](../../../../includes/sample-powershell-install-no-ssh-az.md)]

```azurepowershell-interactive
$policydefinitions = "https://raw.githubusercontent.com/Azure/azure-policy/master/samples/PolicyInitiatives/multiple-billing-tags/azurepolicyset.definitions.json"
$policysetparameters = "https://raw.githubusercontent.com/Azure/azure-policy/master/samples/PolicyInitiatives/multiple-billing-tags/azurepolicyset.parameters.json"

$policyset= New-AzPolicySetDefinition -Name "multiple-billing-tags" -DisplayName "Billing Tags Policy Initiative" -Description "Specify cost Center tag and product name tag" -PolicyDefinition $policydefinitions -Parameter $policysetparameters

New-AzPolicyAssignment -PolicySetDefinition $policyset -Name <assignmentname> -Scope <scope>  -costCenterValue <required value for Cost Center tag> -productNameValue <required value for product Name tag>
```

### <a name="clean-up-powershell-deployment"></a>Limpieza de la implementación de PowerShell

Ejecute el siguiente comando para quitar el grupo de recursos, la máquina virtual y todos los recursos relacionados.

```azurepowershell-interactive
Remove-AzResourceGroup -Name myResourceGroup
```

## <a name="apply-tags-to-existing-resources"></a>Aplicación de etiquetas a los recursos existentes

Después de asignar las directivas, puede desencadenar una actualización a todos los recursos existentes para aplicar las directivas de etiqueta que ha agregado. En el siguiente script se conservan las demás etiquetas que existían en los recursos:

```azurepowershell-interactive
$resources = Get-AzResource -ResourceGroupName 'ExampleGroup'

foreach ($r in $resources) {
    try {
        Set-AzResource -Tags ($a = if ($r.Tags -eq $NULL) { @{} } else {$r.Tags}) -ResourceId $r.ResourceId -Force -UsePatchSemantics
    }
    catch {
        Write-Host $r.ResourceId + "can't be updated"
    }
}
```

## <a name="next-steps"></a>Pasos siguientes

- Consulte más ejemplos en [Ejemplos de Azure Policy](index.md).