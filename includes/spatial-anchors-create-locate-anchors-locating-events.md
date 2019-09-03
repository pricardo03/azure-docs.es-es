---
ms.openlocfilehash: f478956c3a220ea4435c4f6e8b1096a1e9fdcadf
ms.sourcegitcommit: dcf3e03ef228fcbdaf0c83ae1ec2ba996a4b1892
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/23/2019
ms.locfileid: "70014815"
---
Una vez creado el monitor, se desencadenará el evento `AnchorLocated` para cada delimitador solicitado. Este evento se desencadena cuando se encuentra un delimitador o si no puede encontrarse. Si se produce esta situación, el motivo se incluirá en el estado. Una vez procesados todos los delimitadores de un monitor, tanto si se encuentran como si no, se desencadenará el evento `LocateAnchorsCompleted`. Hay un límite de 35 identificadores por monitor. 
