---
ms.openlocfilehash: 10eb16dc1757255c77ed8967dc132f42660e070f
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60232560"
---
## <a name="provide-frames-to-the-session"></a>Proporcionar fotogramas a la sesión

La sesión de anclaje espacial funciona asignando el espacio que rodea el usuario. Esto ayuda a determinar dónde se encuentran los delimitadores. Plataformas móviles (iOS y Android) requieren una llamada a la cámara de fuente para obtener los marcos de la biblioteca de AR de su plataforma nativa. En cambio, HoloLens constantemente está recorriendo el entorno, por lo que no hay ninguna necesidad de una llamada concreta al igual que con Mobile.