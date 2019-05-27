---
ms.openlocfilehash: 8ebb10f955be8f3004fdbdc595ea0fefc0d2b7ea
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2019
ms.locfileid: "66110766"
---
## <a name="update-properties"></a>Actualizar propiedades

Para actualizar las propiedades en un delimitador, usa el `UpdateAnchorProperties()` método. Si dos o más dispositivos intentan actualizar las propiedades para el mismo delimitador al mismo tiempo, usamos un modelo de simultaneidad optimista. Lo que significa que la primera escritura tendrán prioridad.  Todas las otras escrituras producirá un error de "Concurrency": se necesitaría una actualización de las propiedades antes de intentarlo de nuevo.
