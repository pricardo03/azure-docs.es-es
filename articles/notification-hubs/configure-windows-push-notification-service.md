---
title: Configuración de Servicios de notificaciones de inserción de Windows en Azure Notification Hubs | Microsoft Docs
description: Aprenda a configurar Servicios de notificaciones de inserción de Windows para un centro de notificaciones de Azure.
services: notification-hubs
author: sethmanheim
manager: femila
editor: jwargo
ms.service: notification-hubs
ms.workload: mobile
ms.topic: article
ms.date: 03/25/2019
ms.author: sethm
ms.reviewer: jowargo
ms.lastreviewed: 03/25/2019
ms.openlocfilehash: a7f7734d97cd67c133ff0cedc3ef2376967bcdf4
ms.sourcegitcommit: 7df70220062f1f09738f113f860fad7ab5736e88
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2019
ms.locfileid: "71212418"
---
# <a name="configure-windows-push-notification-service-wns-settings-for-a-notification-hub-in-the-azure-portal"></a>Configuración de Servicios de notificaciones de inserción de Windows (WNS) para un centro de notificaciones en Azure Portal
En este artículo podrá ver cómo establecer la configuración del Servicio de notificaciones de Windows (WNS) para un centro de notificaciones de Azure mediante Azure Portal.  

## <a name="prerequisites"></a>Requisitos previos
Si aún no ha creado un centro de notificaciones, cree uno ahora. Para más información, consulte [Creación de un centro de notificaciones de Azure en Azure Portal](create-notification-hub-portal.md). 

## <a name="configure-windows-push-notification-service-wns"></a>Configuración de Servicios de notificaciones de inserción de Windows (WNS)

El siguiente procedimiento le proporciona los pasos para establecer la configuración de Servicios de notificaciones de inserción de Windows (WNS) para un centro de notificaciones: 

1. En Azure Portal, en la página **Centro de notificaciones**, seleccione **Windows (WNS)** en el menú izquierdo.
2. Especifique los valores de **SID de paquete** y **Clave de seguridad**.
3. Seleccione **Guardar**.

   ![Captura de pantalla que muestra los cuadros de SID del paquete y Clave de seguridad](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-configure-wns.png)

## <a name="next-steps"></a>Pasos siguientes
Para ver un tutorial con instrucciones paso a paso sobre cómo insertar notificaciones en aplicaciones de Plataforma universal de Windows mediante Azure Notification Hubs y Servicios de notificaciones de inserción de Windows (WNS), consulte [Envío de notificaciones a aplicaciones de Plataforma universal de Windows mediante Azure Notification Hubs](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md).


