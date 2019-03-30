---
ms.openlocfilehash: c4339aa8548ef66c862200ad61b6aaca90332ad0
ms.sourcegitcommit: 956749f17569a55bcafba95aef9abcbb345eb929
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2019
ms.locfileid: "58632615"
---
Una vez creado el monitor, el `AnchorLocated` evento se desencadenará para cada delimitador solicitado. Este evento se desencadena cuando se encuentra un delimitador, o si el delimitador no se encuentra. Si se produce esta situación, el motivo se incluirán en el estado. Una vez procesados todos los delimitadores para un monitor, se encuentra o no se encontró el `LocateAnchorsCompleted` se desencadenará el evento.
