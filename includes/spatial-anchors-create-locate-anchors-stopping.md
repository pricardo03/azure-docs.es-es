---
ms.openlocfilehash: 6d6886996d926ce778600dfa1d69caaa1c7ac004
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2019
ms.locfileid: "66110509"
---
## <a name="pause-reset-or-stop-the-session"></a>Pausar, restablecer o detener la sesión

Para detener la sesión temporalmente, puede invocar `Stop()`. Si lo hace, se detendrán los monitores y procesamiento de entorno, incluso si se invoca ProcessFrame(). A continuación, puede invocar `Start()` para reanudar el procesamiento. Cuando se reanuda, se mantienen los datos de entorno ya está capturados en la sesión.
