---
title: Configurar el servicio de notificaciones Push de Windows en Azure Notification Hubs | Microsoft Docs
description: Obtenga información sobre cómo configurar el servicio de notificaciones Push de Windows para un centro de notificaciones de Azure.
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
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60240282"
---
# <a name="configure-windows-push-notification-service-wns-settings-for-a-notification-hub-in-the-azure-portal"></a>Configurar el servicio de notificaciones Push de Windows (WNS) para un centro de notificaciones en el portal de Azure
Este artículo muestra cómo configurar el servicio de notificaciones de Windows (WNS) para un centro de notificaciones de Azure mediante el portal de Azure.  

## <a name="prerequisites"></a>Requisitos previos
Si aún no ha creado un centro de notificaciones, cree uno ahora. Para más información, consulte [Creación de un centro de notificaciones de Azure en Azure Portal](create-notification-hub-portal.md). 

## <a name="configure-windows-push-notification-service-wns"></a>Configurar el servicio de notificaciones Push de Windows (WNS)

El siguiente procedimiento proporciona pasos para configurar el servicio de notificaciones Push de Windows (WNS) para un centro de notificaciones: 

1. En el portal de Azure, en el **centro de notificaciones** página, seleccione **Windows (WNS)** en el menú izquierdo.
2. Especifique los valores de **SID de paquete** y **Clave de seguridad**.
3. Seleccione **Guardar**.

   ![Captura de pantalla que muestra los cuadros de SID del paquete y Clave de seguridad](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-configure-wns.png)

## <a name="next-steps"></a>Pasos siguientes
Para ver un tutorial con instrucciones paso a paso para enviar notificaciones Push a aplicaciones de la plataforma Universal de Windows mediante Azure Notification Hubs y servicio de notificaciones Push de Windows (WNS), consulte [enviar notificaciones a aplicaciones para UWP mediante el uso de Azure Notification Hubs](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md).


