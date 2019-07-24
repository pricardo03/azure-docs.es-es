---
ms.openlocfilehash: 8ebb10f955be8f3004fdbdc595ea0fefc0d2b7ea
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/18/2019
ms.locfileid: "67186028"
---
## <a name="update-properties"></a>Actualización de propiedades

Para actualizar las propiedades de un delimitador, se usa el método `UpdateAnchorProperties()`. Si dos o más dispositivos intentan actualizar las propiedades del mismo delimitador al mismo tiempo, se usa un modelo de simultaneidad optimista, lo que significa que la primera escritura tendrá prioridad.  En todas las demás se producirá un error de "simultaneidad", es decir, será necesario actualizar las propiedades antes de intentarlo de nuevo.
