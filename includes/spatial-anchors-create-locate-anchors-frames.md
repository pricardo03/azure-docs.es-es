---
ms.openlocfilehash: 505670e719e86086dbf7721b4298ec913220f928
ms.sourcegitcommit: 87781a4207c25c4831421c7309c03fce5fb5793f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/23/2020
ms.locfileid: "76694335"
---
## <a name="provide-frames-to-the-session"></a>Suministro de fotogramas a la sesión

La sesión de anclaje espacial funciona mediante la asignación del espacio en torno al usuario. Esto ayuda a determinar dónde se encuentran los delimitadores. Las plataformas móviles (iOS y Android) requieren una llamada a la fuente de la cámara para obtener fotogramas de la biblioteca de AR de la plataforma nativa. En cambio, HoloLens examina constantemente el entorno, por lo que no hay ninguna necesidad de realizar una llamada concreta, como en las aplicaciones móviles.