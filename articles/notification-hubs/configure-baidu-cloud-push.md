---
title: Configuración de Baidu Cloud Push en Azure Notification Hubs | Microsoft Docs
description: Aprenda a establecer la configuración de Baidu para un centro de notificaciones de Azure.
services: notification-hubs
author: jwargo
manager: patniko
ms.service: notification-hubs
ms.workload: mobile
ms.topic: article
ms.date: 03/25/2019
ms.author: jowargo
ms.openlocfilehash: 9feb5f66c5ae979a239c3349b2b1347ad307ce49
ms.sourcegitcommit: 7c5a2a3068e5330b77f3c6738d6de1e03d3c3b7d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/11/2019
ms.locfileid: "70884648"
---
# <a name="deprecated-configure-baidu-cloud-push-settings-for-a-notification-hub-in-the-azure-portal"></a>En desuso: Establecimiento de la configuración de Baidu Cloud Push para un centro de notificaciones en Azure Portal

En este artículo podrá ver cómo establecer la configuración de Baidu Cloud Push para un centro de notificaciones de Azure mediante Azure Portal.

> [!IMPORTANT]
> Este tutorial está en desuso. 

## <a name="prerequisites"></a>Requisitos previos
Si aún no ha creado un centro de notificaciones, cree uno ahora. Para más información, consulte [Creación de un centro de notificaciones de Azure en Azure Portal](create-notification-hub-portal.md). 

## <a name="configure-baidu-cloud-push"></a>Configuración de Baidu Cloud Push
El siguiente procedimiento le proporciona los pasos para establecer la configuración de Baidu Cloud Push para un centro de notificaciones:

1. En Azure Portal, en la página **Centro de notificaciones**, seleccione **Baidu (Android China)** en el menú izquierdo. 
2. Escriba la **clave de API** que ha obtenido en la consola de Baidu, en el proyecto Baidu Cloud Push. 
3. Escriba la **clave secreta** que ha obtenido en la consola de Baidu, en el proyecto Baidu Cloud Push. 
4. Seleccione **Guardar**. 

    ![Captura de pantalla de Notification Hubs que muestra la configuración de Baidu (Android China) para las notificaciones push](./media/notification-hubs-baidu-get-started/AzureNotificationServicesBaidu.png)

## <a name="next-steps"></a>Pasos siguientes
Para ver un tutorial con instrucciones paso a paso para insertar notificaciones en Baidu mediante Azure Notification Hubs y Baidu Cloud Push, consulte [Introducción a Notification Hubs con Baidu](notification-hubs-baidu-china-android-notifications-get-started.md).
