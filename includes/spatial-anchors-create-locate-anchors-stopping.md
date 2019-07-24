---
ms.openlocfilehash: 6d6886996d926ce778600dfa1d69caaa1c7ac004
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/18/2019
ms.locfileid: "67186036"
---
## <a name="pause-reset-or-stop-the-session"></a>Pausa, restablecimiento o detención de la sesión

Para detener la sesión temporalmente, puede invocar `Stop()`. Si lo hace, se detendrán los monitores y el procesamiento de entorno, incluso si invoca ProcessFrame(). Puede invocar `Start()` para reanudar el procesamiento. Cuando se reanude, se conservarán los datos del entorno que ya se hayan capturado en la sesión.
