---
author: ramonarguelles
ms.service: azure-spatial-anchors
ms.topic: include
ms.date: 02/21/2019
ms.author: rgarcia
ms.openlocfilehash: 63725d55e2b2935ec6a899789249259b096865c3
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2019
ms.locfileid: "66110495"
---
### <a name="access-tokens"></a>Tokens de acceso

Los Tokens de acceso son un método más robusto para autenticarse con Azure espacial delimitadores. Especialmente cuando se prepare la aplicación para una implementación de producción. El resumen de este enfoque consiste en configurar un servicio back-end que la aplicación cliente puede autenticarse de forma segura. Las interfaces de servicio de back-end con AAD en tiempo de ejecución y con el Azure espacial delimitadores de servicio de Token seguro para solicitar un Token de acceso. Este token, a continuación, se entrega a la aplicación cliente y usa en el SDK para autenticarse con Azure espacial delimitadores.
