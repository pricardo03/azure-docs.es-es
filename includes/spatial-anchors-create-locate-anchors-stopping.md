---
ms.openlocfilehash: 8e02067b32d9404a5bbdf7362b2cc32712b96250
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/18/2019
ms.locfileid: "57907778"
---
## <a name="pause-reset-or-stop-the-session"></a>Pausar, restablecer o detener la sesión

Para detener la sesión de forma temporal, puede invocar Stop(). Si lo hace, se detendrán los monitores y procesamiento de entorno, incluso si se invoca ProcessFrame(). A continuación, puede invocar Start() para reanudar el procesamiento. Cuando se reanuda, se mantienen los datos de entorno ya está capturados en la sesión.
