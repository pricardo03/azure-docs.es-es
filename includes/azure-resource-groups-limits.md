---
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: include
ms.date: 04/19/2019
ms.author: tomfitz
ms.openlocfilehash: 8bd16378e9c82a011309c12cf241b59d03405a77
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/22/2019
ms.locfileid: "60012530"
---
| Recurso | Límite predeterminado | Límite máximo |
| --- | --- | --- |
| Recursos por [grupo de recursos](../articles/azure-resource-manager/resource-group-overview.md#resource-groups), por tipo de recurso |800 |Varía por tipo de recurso |
| Implementaciones por grupo de recursos en el historial de implementaciones |800<sup>1</sup> |800 |
| Recursos por implementación |800 |800 |
| Bloqueos de administración por ámbito único |20 |20 |
| Número de etiquetas por recurso o grupo de recursos |15 |15 |
| Longitud de la clave de etiqueta |512 |512 |
| Longitud del valor de la etiqueta |256 |256 |

<sup>1</sup>si se alcanza el límite de 800 implementaciones por grupo de recursos, eliminar las implementaciones del historial que ya no son necesarios. Eliminar una entrada del historial de implementación no afecta a los recursos implementados. Puede eliminar las entradas del historial con [az group deployment delete](/cli/azure/group/deployment) en la CLI de Azure o [Remove-AzResourceGroupDeployment](/powershell/module/az.resources/remove-azresourcegroupdeployment) en PowerShell.  Para un PowerShell eliminar implementaciones en un escenario de entrega continua (CI/CD) y la integración continua consulte secuencia de comandos que automatiza [remove-deployments.ps1](https://gist.github.com/bmoore-msft/ed33fb940dafb09380174b7fca57651f).

#### <a name="template-limits"></a>Límites de plantilla

| Value | Límite predeterminado | Límite máximo |
| --- | --- | --- |
| Parámetros |256 |256 |
| variables |256 |256 |
| Recursos (incluido el recuento de copia) |800 |800 |
| Salidas |64 |64 |
| Expresión de plantilla |24 576 caracteres |24 576 caracteres |
| Recursos de plantillas exportadas |200 |200 | 
| Tamaño de la plantilla |1 MB |1 MB |
| Tamaño del archivo de parámetros |64 KB |64 KB |

Puede superar algunos límites de plantilla utilizando una plantilla anidada. Para obtener más información, consulte [usar plantillas vinculadas al implementar recursos de Azure](../articles/azure-resource-manager/resource-group-linked-templates.md). Para reducir el número de parámetros, variables o salidas, puede combinar varios valores en un objeto. Para más información, consulte [Objetos como parámetros](../articles/azure-resource-manager/resource-manager-objects-as-parameters.md).
