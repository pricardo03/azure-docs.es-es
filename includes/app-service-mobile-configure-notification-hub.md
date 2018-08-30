---
author: conceptdev
ms.author: crdun
ms.service: app-service-mobile
ms.topic: include
ms.date: 08/23/2018
ms.openlocfilehash: 0a74079677a084b2d4e8221cf5a74b356126811d
ms.sourcegitcommit: 58c5cd866ade5aac4354ea1fe8705cee2b50ba9f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/24/2018
ms.locfileid: "42816207"
---
La característica Mobile Apps de Azure App Service usa [Azure Notification Hubs] para enviar notificaciones push; por lo tanto, necesita un centro de notificaciones para su aplicación móvil.

1. En [Azure Portal], vaya a **App Services** y seleccione el back-end de la aplicación. En **Configuración**, seleccione **Insertar**.
2. Seleccione **Conectar** para agregar un recurso de Centro de notificaciones a la aplicación. Puede crear un centro o conectarse a uno existente.

    ![Configurar un centro](./media/app-service-mobile-create-notification-hub/configure-hub-flow.png)

Ya ha conectado un centro de notificaciones al proyecto de back-end de Mobile Apps. Más adelante configure este centro de notificaciones para que se conecte a un sistema de notificación de plataforma (PNS) que envíe notificaciones push a los dispositivos.

[Azure Portal]: https://portal.azure.com/
[Azure Notification Hubs]: https://azure.microsoft.com/documentation/articles/notification-hubs-push-notification-overview/
