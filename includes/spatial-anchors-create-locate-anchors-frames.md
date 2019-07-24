---
ms.openlocfilehash: 10eb16dc1757255c77ed8967dc132f42660e070f
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/18/2019
ms.locfileid: "67186018"
---
## <a name="provide-frames-to-the-session"></a>Suministro de fotogramas a la sesión

La sesión de anclaje espacial funciona mediante la asignación del espacio en torno al usuario. Esto ayuda a determinar dónde se encuentran los delimitadores. Las plataformas móviles (iOS y Android) requieren una llamada a la fuente de la cámara para obtener fotogramas de la biblioteca de AR de la plataforma nativa. En cambio, HoloLens examina constantemente el entorno, por lo que no hay ninguna necesidad de realizar una llamada concreta, como con las aplicaciones móviles.