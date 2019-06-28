---
title: Configuración de Servicios de notificaciones de inserción de Windows en Azure Notification Hubs | Microsoft Docs
description: Aprenda a configurar Servicios de notificaciones de inserción de Windows para un centro de notificaciones de Azure.
services: notification-hubs
author: jwargo
manager: patniko
editor: spelluru
ms.service: notification-hubs
ms.workload: mobile
ms.topic: article
ms.date: 03/25/2019
ms.author: jowargo
ms.openlocfilehash: c3e3f1e7df5c90c690756375ff1e1b0350c72714
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "60240282"
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


