---
ms.openlocfilehash: c4339aa8548ef66c862200ad61b6aaca90332ad0
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/18/2019
ms.locfileid: "67186034"
---
Una vez creado el monitor, se desencadenará el evento `AnchorLocated` para cada delimitador solicitado. Este evento se desencadena cuando se encuentra un delimitador o si no puede encontrarse. Si se produce esta situación, el motivo se incluirá en el estado. Una vez procesados todos los delimitadores de un monitor, tanto si se encuentran como si no, se desencadenará el evento `LocateAnchorsCompleted`.
