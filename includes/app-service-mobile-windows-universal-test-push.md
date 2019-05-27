---
author: conceptdev
ms.author: crdun
ms.service: app-service-mobile
ms.topic: include
ms.date: 08/23/2018
ms.openlocfilehash: 70e0d766fe3c9669912ce9f13f729fb1c757a26f
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2019
ms.locfileid: "66140475"
---
1. Haga clic con el botón derecho en el proyecto de la Tienda Windows, haga clic en **Establecer como proyecto de inicio**y luego presione la tecla F5 para ejecutar la aplicación de la Tienda Windows.

    Cuando la aplicación se inicia, el dispositivo se registra para recibir notificaciones push.
2. Detenga la aplicación de la Tienda Windows y repita el paso anterior para ejecutar la aplicación de la Tienda de Windows Phone.

    En este momento, ambos dispositivos están registrados para recibir notificaciones push.

3. Vuelva a ejecutar la aplicación de la Tienda Windows, escriba texto en **Insertar un TodoItem** y haga clic en **Guardar**.

    Después de hacer esto, las aplicaciones de la Tienda Windows y de Windows Phone recibirán una notificación push de WNS. La notificación se muestra en Windows Phone aunque la aplicación no se esté ejecutando.

    ![](./media/app-service-mobile-windows-universal-test-push/mobile-quickstart-push5-wp8.png)
