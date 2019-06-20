---
author: conceptdev
ms.service: app-service-mobile
ms.topic: include
ms.date: 08/23/2018
ms.author: crdun
ms.openlocfilehash: 79459be30a5a2018dc82486a84895b1a954941bc
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/18/2019
ms.locfileid: "67186814"
---
1. En [Azure Portal](https://portal.azure.com/), seleccione **Examinar todo** > **App Services**. A continuación, seleccione el back-end de Mobile Apps. En **Configuración**, seleccione **App Service Push**. A continuación, seleccione el nombre del centro de notificaciones.
2. Vaya a **Windows (WNS)** . Escriba la **Clave de seguridad** (secreto de cliente) y el **SID del paquete** que ha obtenido en el sitio de Servicios Live. Luego, seleccione **Guardar**.

    ![Establecimiento de la clave de WNS en el portal](./media/app-service-mobile-configure-wns/mobile-push-wns-credentials.png)

El back-end ahora está configurado para usar WNS para enviar notificaciones push.
