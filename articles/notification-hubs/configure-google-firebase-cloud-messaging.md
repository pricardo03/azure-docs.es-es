---
title: Configurar la mensajería de Azure Notification Hubs de en la nube de Google Firebase | Microsoft Docs
description: Obtenga información sobre cómo configurar un centro de notificaciones de Azure con la configuración de Google Firebase Cloud Messaging.
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
ms.sourcegitcommit: 0dd053b447e171bc99f3bad89a75ca12cd748e9c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2019
ms.locfileid: "58488370"
---
# <a name="configure-google-firebase-cloud-messaging-fcm-settings-for-a-notification-hub-in-the-azure-portal"></a>Configuración de Google Firebase Cloud Messaging (FCM) de un centro de notificaciones en el portal de Azure
Este artículo muestra cómo configurar opciones de Google Firebase Cloud Messaging (FCM) de un centro de notificaciones de Azure mediante el portal de Azure.  

## <a name="prerequisites"></a>Requisitos previos
Si aún no ha creado un centro de notificaciones, cree uno ahora. Para obtener más información, consulte [crear un centro de notificaciones de Azure en Azure portal](create-notification-hub-portal.md). 

## <a name="configure-google-firebase-cloud-messaging-fcm"></a>Configurar la nube de Google Firebase (FCM) de mensajería

El siguiente procedimiento proporciona pasos para configurar las opciones de Google Firebase Cloud Messaging (FCM) de un centro de notificaciones: 

1. En el portal de Azure, en el **centro de notificaciones** página, seleccione **Google (GCM o FCM)** en el menú izquierdo. 
2. Pegar la **clave de API** para el proyecto FCM que guardó anteriormente. 
3. Seleccione **Guardar**. 

   ![Captura de pantalla que muestra cómo configurar centros de notificaciones de FCM de Google](./media/notification-hubs-android-push-notification-google-fcm-get-started/fcm-server-key.png)

## <a name="next-steps"></a>Pasos siguientes
Para ver un tutorial con instrucciones paso a paso para enviar notificaciones Push a dispositivos Android mediante Azure Notification Hubs y Google Firebase Cloud Messaging, consulte [notificaciones Push a dispositivos Android mediante el uso de Notification Hubs y FCM de Google ](notification-hubs-android-push-notification-google-fcm-get-started.md).

