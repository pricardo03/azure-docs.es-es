---
ms.openlocfilehash: c5ca85c0dfe8d601821a78c02b2230c0909c8003
ms.sourcegitcommit: 956749f17569a55bcafba95aef9abcbb345eb929
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2019
ms.locfileid: "58632565"
---
No se puede actualizar la ubicación de un delimitador de una vez que se creó en el servicio, debe crear un nuevo anclaje y eliminar la antigua para realizar un seguimiento de una nueva posición.

Si no necesita localizar un delimitador para actualizar sus propiedades, puede usar el `GetAnchorPropertiesAsync()` método, que devuelve un `CloudSpatialAnchor` objeto con propiedades.
