---
author: ramonarguelles
ms.service: azure-spatial-anchors
ms.topic: include
ms.date: 08/23/2019
ms.author: rgarcia
ms.openlocfilehash: 9a02dfbf3f2976489c10ccfeb935915a65d8dc84
ms.sourcegitcommit: 83df2aed7cafb493b36d93b1699d24f36c1daa45
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/22/2019
ms.locfileid: "71180074"
---
Una vez creado el monitor, se desencadenará el evento `AnchorLocated` para cada delimitador solicitado. Este evento se desencadena cuando se encuentra un delimitador o si no puede encontrarse. Si se produce esta situación, el motivo se incluirá en el estado. Una vez procesados todos los delimitadores de un monitor, tanto si se encuentran como si no, se desencadenará el evento `LocateAnchorsCompleted`. Hay un límite de 35 identificadores por monitor. 
