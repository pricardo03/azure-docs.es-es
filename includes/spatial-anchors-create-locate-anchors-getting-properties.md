---
ms.openlocfilehash: c5ca85c0dfe8d601821a78c02b2230c0909c8003
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60232355"
---
No se puede actualizar la ubicación de un delimitador de una vez que se creó en el servicio, debe crear un nuevo anclaje y eliminar la antigua para realizar un seguimiento de una nueva posición.

Si no necesita localizar un delimitador para actualizar sus propiedades, puede usar el `GetAnchorPropertiesAsync()` método, que devuelve un `CloudSpatialAnchor` objeto con propiedades.
