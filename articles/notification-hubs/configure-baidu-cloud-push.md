---
title: Configurar la inserción de Baidu en la nube en Azure Notification Hubs | Microsoft Docs
description: Obtenga información sobre cómo configurar opciones de Baidu para un centro de notificaciones de Azure.
services: notification-hubs
author: jwargo
manager: patniko
editor: spelluru
ms.service: notification-hubs
ms.workload: mobile
ms.topic: article
ms.date: 03/25/2019
ms.author: jowargo
ms.openlocfilehash: 9942b1d4d8b5d538f5150a36e596753282039be7
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60234253"
---
# <a name="configure-baidu-cloud-push-settings-for-a-notification-hub-in-the-azure-portal"></a>Configuración de Baidu Cloud Push para un centro de notificaciones en el portal de Azure
Este artículo muestra cómo configurar opciones de Baidu Cloud Push para un centro de notificaciones de Azure mediante el portal de Azure. 

## <a name="prerequisites"></a>Requisitos previos
Si aún no ha creado un centro de notificaciones, cree uno ahora. Para más información, consulte [Creación de un centro de notificaciones de Azure en Azure Portal](create-notification-hub-portal.md). 

## <a name="configure-baidu-cloud-push"></a>Configuración de Baidu Cloud Push
El siguiente procedimiento proporciona pasos para configurar las opciones de inserción de Baidu Cloud para un centro de notificaciones:

1. En el portal de Azure, en el **centro de notificaciones** página, seleccione **Baidu (Android China)** en el menú izquierdo. 
2. Escriba la **clave de API** que ha obtenido en la consola de Baidu, en el proyecto Baidu Cloud Push. 
3. Escriba la **clave secreta** que ha obtenido en la consola de Baidu, en el proyecto Baidu Cloud Push. 
4. Seleccione **Guardar**. 

    ![Captura de pantalla de Notification Hubs que muestra la configuración de Baidu (Android China) para las notificaciones push](./media/notification-hubs-baidu-get-started/AzureNotificationServicesBaidu.png)

## <a name="next-steps"></a>Pasos siguientes
Para ver un tutorial con instrucciones paso a paso para enviar notificaciones Push a Baidu mediante Azure Notification Hubs y Baidu Cloud Push, consulte [empezar a trabajar con Notification Hubs mediante Baidu](notification-hubs-baidu-china-android-notifications-get-started.md).
