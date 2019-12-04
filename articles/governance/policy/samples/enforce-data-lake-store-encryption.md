---
title: 'Ejemplo: Requerir el cifrado de Data Lake Store'
description: Este ejemplo de definición de directiva requiere que se habilite el estado de cifrado en las cuentas de Data Lake Store.
ms.date: 01/23/2019
ms.topic: sample
ms.openlocfilehash: 5b29caa7fa8b9169b8c2164975f9cdb2663c5cef
ms.sourcegitcommit: 95931aa19a9a2f208dedc9733b22c4cdff38addc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/25/2019
ms.locfileid: "74463648"
---
# <a name="sample---require-data-lake-store-encryption"></a>Ejemplo: requerir cifrado de Data Lake Store

Esta directiva integrada deniega las cuentas de Data Lake Store que no tengan el cifrado habilitado.

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-template"></a>Plantilla de ejemplo

```json
{
  "if": {
    "allOf": [
      {
        "field": "type",
        "equals": "Microsoft.DataLakeStore/accounts"
      },
      {
        "field": "Microsoft.DataLakeStore/accounts/encryptionState",
        "equals": "Disabled"
      }
    ]
  },
  "then": {
    "effect": "deny"
  }
}
```

Puede implementar esta plantilla mediante [Azure Portal](#deploy-with-the-portal), con [PowerShell](#deploy-with-powershell) o con la [CLI de Azure](#deploy-with-azure-cli). Para obtener la directiva integrada, use el identificador `a7ff3161-0087-490a-9ad9-ad6217f4f43a`.

## <a name="deploy-with-the-portal"></a>Implementación con el portal

Al asignar una directiva, seleccione **Aplicar cifrado en cuentas de DataLakeStore** en las definiciones integradas disponibles.

## <a name="deploy-with-powershell"></a>Implementación con PowerShell

[!INCLUDE [sample-powershell-install](../../../../includes/sample-powershell-install-no-ssh-az.md)]

```azurepowershell-interactive
$definition = Get-AzPolicyDefinition -Id /providers/Microsoft.Authorization/policyDefinitions/a7ff3161-0087-490a-9ad9-ad6217f4f43a

New-AzPolicyAssignment -name "Data Lake Store encryption" -PolicyDefinition $definition -Scope <scope>
```

### <a name="clean-up-powershell-deployment"></a>Limpieza de la implementación de PowerShell

Ejecute el siguiente comando para quitar la asignación de la directiva.

```azurepowershell-interactive
Remove-AzPolicyAssignment -Name "Data Lake Store encryption" -Scope <scope>
```

## <a name="deploy-with-azure-cli"></a>Implementación con la CLI de Azure

[!INCLUDE [sample-cli-install](../../../../includes/sample-cli-install.md)]

```azurecli-interactive
az policy assignment create --scope <scope> --name "Data Lake Store encryption" --policy a7ff3161-0087-490a-9ad9-ad6217f4f43a
```

### <a name="clean-up-azure-cli-deployment"></a>Limpieza de la implementación de la CLI de Azure

Ejecute el siguiente comando para quitar la asignación de la directiva.

```azurecli-interactive
az policy assignment delete --name "Data Lake Store encryption" --resource-group myResourceGroup
```

## <a name="next-steps"></a>Pasos siguientes

- Consulte más ejemplos en [Ejemplos de Azure Policy](index.md).