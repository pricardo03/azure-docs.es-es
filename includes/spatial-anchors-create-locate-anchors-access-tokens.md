---
author: ramonarguelles
ms.service: azure-spatial-anchors
ms.topic: include
ms.date: 02/21/2019
ms.author: rgarcia
ms.openlocfilehash: 0e4c0b6d622fe0d8d20692ec6c0c889779e4601a
ms.sourcegitcommit: e88188bc015525d5bead239ed562067d3fae9822
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/24/2019
ms.locfileid: "56751899"
---
### <a name="access-tokens"></a>Tokens de acceso

Los Tokens de acceso son un método más robusto para autenticarse con Azure espacial delimitadores. Especialmente cuando se prepare la aplicación para una implementación de producción. El resumen de este enfoque consiste en configurar un servicio back-end que la aplicación cliente puede autenticarse de forma segura. Las interfaces de servicio de back-end con AAD en tiempo de ejecución y con el servicio STS de Azure espacial delimitadores para solicitar un Token de acceso. Este token, a continuación, se entrega a la aplicación cliente y usa en el SDK para autenticarse con Azure espacial delimitadores.
