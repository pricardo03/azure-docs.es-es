---
title: Configuración de Google Firebase Cloud Messaging en Azure Notification Hubs | Microsoft Docs
description: Aprenda a configurar un centro de notificaciones de Azure con la configuración de Google Firebase Cloud Messaging.
services: notification-hubs
author: jwargo
manager: patniko
editor: spelluru
ms.service: notification-hubs
ms.workload: mobile
ms.topic: article
ms.date: 03/25/2019
ms.author: jowargo
ms.openlocfilehash: d200f5746d5f7c62ff3a52873d5d5d6c3e1322dc
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "60239200"
---
# <a name="configure-google-firebase-cloud-messaging-fcm-settings-for-a-notification-hub-in-the-azure-portal"></a>Configuración de Google Firebase Cloud Messaging (FCM) para un centro de notificaciones en Azure Portal
En este artículo se muestra cómo establecer la configuración de Google Firebase Cloud Messaging (FCM) para un centro de notificaciones de Azure mediante Azure Portal.  

## <a name="prerequisites"></a>Requisitos previos
Si aún no ha creado un centro de notificaciones, cree uno ahora. Para más información, consulte [Creación de un centro de notificaciones de Azure en Azure Portal](create-notification-hub-portal.md). 

## <a name="configure-google-firebase-cloud-messaging-fcm"></a>Configuración de Google Firebase Cloud Messaging (FCM)

El siguiente procedimiento le proporciona los pasos para establecer la configuración de Google Firebase Cloud Messaging (FCM) para un centro de notificaciones: 

1. En Azure Portal, en la página **Centro de notificaciones**, seleccione **Google (GCM/FCM)** en el menú izquierdo. 
2. Pegue la **clave de API** para el proyecto FCM que ha guardado anteriormente. 
3. Seleccione **Guardar**. 

   ![Captura de pantalla que muestra cómo configurar Notification Hubs para Google FCM](./media/notification-hubs-android-push-notification-google-fcm-get-started/fcm-server-key.png)

## <a name="next-steps"></a>Pasos siguientes
Para ver un tutorial con instrucciones paso a paso para insertar notificaciones en dispositivos Android mediante Azure Notification Hubs y Google Firebase Cloud Messaging, consulte [Tutorial: Envío de notificaciones push a dispositivos Android con Azure Notification Hubs y Google Firebase Cloud Messaging](notification-hubs-android-push-notification-google-fcm-get-started.md).

