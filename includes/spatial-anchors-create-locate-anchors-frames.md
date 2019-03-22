---
ms.openlocfilehash: 10eb16dc1757255c77ed8967dc132f42660e070f
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2019
ms.locfileid: "57907794"
---
## <a name="provide-frames-to-the-session"></a>Proporcionar fotogramas a la sesión

La sesión de anclaje espacial funciona asignando el espacio que rodea el usuario. Esto ayuda a determinar dónde se encuentran los delimitadores. Plataformas móviles (iOS y Android) requieren una llamada a la cámara de fuente para obtener los marcos de la biblioteca de AR de su plataforma nativa. En cambio, HoloLens constantemente está recorriendo el entorno, por lo que no hay ninguna necesidad de una llamada concreta al igual que con Mobile.