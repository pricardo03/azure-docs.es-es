---
ms.openlocfilehash: eafe71902ee849c310847f4537214722e129ad42
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/18/2019
ms.locfileid: "57907758"
---
## <a name="update-properties"></a>Actualizar propiedades

Para actualizar las propiedades en un delimitador, utilice el método UpdateAnchorProperties. Si dos o más dispositivos intentan actualizar las propiedades para el mismo delimitador al mismo tiempo, usamos un modelo de simultaneidad optimista. Lo que significa que la primera escritura tendrán prioridad.  Todas las otras escrituras producirá un error de "Concurrency": se necesitaría una actualización de las propiedades antes de intentarlo de nuevo.
