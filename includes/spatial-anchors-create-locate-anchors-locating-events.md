---
ms.openlocfilehash: c4339aa8548ef66c862200ad61b6aaca90332ad0
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2019
ms.locfileid: "66110578"
---
Una vez creado el monitor, el `AnchorLocated` evento se desencadenará para cada delimitador solicitado. Este evento se desencadena cuando se encuentra un delimitador, o si el delimitador no se encuentra. Si se produce esta situación, el motivo se incluirán en el estado. Una vez procesados todos los delimitadores para un monitor, se encuentra o no se encontró el `LocateAnchorsCompleted` se desencadenará el evento.
