---
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: include
ms.date: 07/12/2019
ms.author: tomfitz
ms.openlocfilehash: 5e251402a89f84a0aa67eee676c1794207dd24fb
ms.sourcegitcommit: 10251d2a134c37c00f0ec10e0da4a3dffa436fb3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/13/2019
ms.locfileid: "67869883"
---
| Recurso | Límite predeterminado | Límite máximo |
| --- | --- | --- |
| Recursos por [grupo de recursos](../articles/azure-resource-manager/resource-group-overview.md#resource-groups) (por tipo de recurso) |800 |Varía por tipo de recurso |
| Implementaciones por grupo de recursos en el historial de implementaciones |800<sup>1</sup> |800 |
| Recursos por implementación |800 |800 |
| Bloqueos de administración por ámbito único |20 |20 |
| Número de etiquetas por recurso o grupo de recursos |15 |15 |
| Longitud de la clave de etiqueta |512 |512 |
| Longitud del valor de la etiqueta |256 |256 |

<sup>1</sup>Si se alcanza el límite de 800 implementaciones por grupo de recursos, elimine las implementaciones que ya no necesite del historial. Eliminar una entrada del historial de implementaciones no afecta a los recursos implementados. Puede eliminar las entradas del historial con [az group deployment delete](/cli/azure/group/deployment) en la CLI de Azure o [Remove-AzResourceGroupDeployment](/powershell/module/az.resources/remove-azresourcegroupdeployment) en PowerShell.  Para información sobre un script de PowerShell que automatiza la eliminación de implementaciones en un escenario de integración continua y entrega continua (CI/CD), consulte [remove-deployments.ps1](https://gist.github.com/bmoore-msft/ed33fb940dafb09380174b7fca57651f).

#### <a name="template-limits"></a>Límites de plantilla

| Valor | Límite predeterminado | Límite máximo |
| --- | --- | --- |
| Parámetros |256 |256 |
| variables |256 |256 |
| Recursos (incluido el recuento de copia) |800 |800 |
| Salidas |64 |64 |
| Expresión de plantilla |24 576 caracteres |24 576 caracteres |
| Recursos de plantillas exportadas |200 |200 | 
| Tamaño de la plantilla |4 MB |4 MB |
| Tamaño del archivo de parámetros |64 KB |64 KB |

Puede superar algunos límites de plantilla utilizando una plantilla anidada. Para más información, consulte [Uso de plantillas vinculadas al implementar recursos de Azure](../articles/azure-resource-manager/resource-group-linked-templates.md). Para reducir el número de parámetros, variables o salidas, puede combinar varios valores en un objeto. Para más información, consulte [Objetos como parámetros](../articles/azure-resource-manager/resource-manager-objects-as-parameters.md).
