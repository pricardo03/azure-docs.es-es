---
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: include
ms.date: 02/24/2020
ms.author: tomfitz
ms.openlocfilehash: c1a52e1bc2dde74289cb270fcae832be24de5a06
ms.sourcegitcommit: 99ac4a0150898ce9d3c6905cbd8b3a5537dd097e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/25/2020
ms.locfileid: "77592444"
---
| Resource | Límite predeterminado | Límite máximo |
| --- | --- | --- |
| Recursos por [grupo de recursos](../articles/azure-resource-manager/management/overview.md#resource-groups) | N/D | Los recursos no están limitados por el grupo de recursos. En su lugar, están limitados por el tipo de recurso de un grupo de recursos. Consulte la fila siguiente. | 
| Recursos por grupo de recursos, por tipo de recurso |800 |Algunos tipos de recursos pueden superar el límite de 800. Consulte [Resources not limited to 800 instances per resource group](../articles/azure-resource-manager/management/resources-without-resource-group-limit.md) (Recursos no limitados a 800 instancias por grupo de recursos). |
| Implementaciones por grupo de recursos en el historial de implementaciones |800<sup>1</sup> |800 |
| Recursos por implementación |800 |800 |
| Bloqueos de administración por ámbito único |20 |20 |
| Número de etiquetas por recurso o grupo de recursos |50 |50 |
| Longitud de la clave de etiqueta |512 |512 |
| Longitud del valor de la etiqueta |256 |256 |

<sup>1</sup>Si se alcanza el límite de 800 implementaciones por grupo de recursos, elimine las implementaciones que ya no necesite del historial. Eliminar una entrada del historial de implementaciones no afecta a los recursos implementados. Para más información, consulte [Resolución de error cuando el recuento de implementaciones es superior a 800](../articles/azure-resource-manager/templates/deployment-quota-exceeded.md).

#### <a name="template-limits"></a>Límites de plantilla

| Value | Límite predeterminado | Límite máximo |
| --- | --- | --- |
| Parámetros |256 |256 |
| variables |256 |256 |
| Recursos (incluido el recuento de copia) |800 |800 |
| Salidas |64 |64 |
| Expresión de plantilla |24 576 caracteres |24 576 caracteres |
| Recursos de plantillas exportadas |200 |200 | 
| Tamaño de la plantilla |4 MB |4 MB |
| Tamaño del archivo de parámetros |64 KB |64 KB |

Puede superar algunos límites de plantilla utilizando una plantilla anidada. Para más información, consulte [Uso de plantillas vinculadas al implementar recursos de Azure](../articles/azure-resource-manager/templates/linked-templates.md). Para reducir el número de parámetros, variables o salidas, puede combinar varios valores en un objeto. Para más información, consulte [Objetos como parámetros](../articles/azure-resource-manager/resource-manager-objects-as-parameters.md).
