---
author: conceptdev
ms.author: crdun
ms.service: app-service-mobile
ms.topic: include
ms.date: 08/23/2018
ms.openlocfilehash: c86210208b9f747cbef1d9231528fa6cedbdb5d2
ms.sourcegitcommit: 58c5cd866ade5aac4354ea1fe8705cee2b50ba9f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/24/2018
ms.locfileid: "42815013"
---
1. Haga clic con el botón derecho en el proyecto de la Tienda Windows, haga clic en **Establecer como proyecto de inicio**y luego presione la tecla F5 para ejecutar la aplicación de la Tienda Windows.

    Cuando la aplicación se inicia, el dispositivo se registra para recibir notificaciones push.
2. Detenga la aplicación de la Tienda Windows y repita el paso anterior para ejecutar la aplicación de la Tienda de Windows Phone.

    En este momento, ambos dispositivos están registrados para recibir notificaciones push.

3. Vuelva a ejecutar la aplicación de la Tienda Windows, escriba texto en **Insertar un TodoItem** y haga clic en **Guardar**.

    Después de hacer esto, las aplicaciones de la Tienda Windows y de Windows Phone recibirán una notificación push de WNS. La notificación se muestra en Windows Phone aunque la aplicación no se esté ejecutando.

    ![](./media/app-service-mobile-windows-universal-test-push/mobile-quickstart-push5-wp8.png)
