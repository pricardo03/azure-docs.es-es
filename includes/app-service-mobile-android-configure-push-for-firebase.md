---
author: conceptdev
ms.author: crdun
ms.service: app-service-mobile
ms.topic: include
ms.date: 08/23/2018
ms.openlocfilehash: bb03e4b5b04a0272d8fa9b032da5adb50878b620
ms.sourcegitcommit: 58c5cd866ade5aac4354ea1fe8705cee2b50ba9f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/24/2018
ms.locfileid: "42809649"
---
1. En [Azure Portal](https://portal.azure.com/), haga clic en **Examinar todo** > **App Services** y finalmente en el back-end de Mobile Apps. En **Configuración**, haga clic en **App Service Push** y, después, en el nombre del centro de notificaciones.
2. Vaya a **Google (GCM)**, escriba el valor de **Clave de servidor** que obtuvo de Firebase en el procedimiento anterior y, después, haga clic en **Guardar**.

    ![Establezca la clave de API en el portal](./media/app-service-mobile-android-configure-push/mobile-push-api-key.png)

El back-end de Mobile Apps ahora está configurado para usar Firebase Cloud Messaging. Esto permite enviar notificaciones push a la aplicación que se ejecuta en un dispositivo Android mediante el centro de notificaciones.
