---
title: Configurar el servicio de notificaciones Push de Microsoft en Azure Notification Hubs | Microsoft Docs
description: Obtenga información sobre cómo configurar el servicio de notificación de inserción de Microsoft para un centro de notificaciones de Azure.
services: notification-hubs
author: jwargo
manager: patniko
editor: spelluru
ms.service: notification-hubs
ms.workload: mobile
ms.topic: article
ms.date: 03/25/2019
ms.author: jowargo
ms.openlocfilehash: 1c76b44438e6527439d0a370c92f4120424b8da5
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60240314"
---
# <a name="configure-microsoft-push-notification-service-mpns-settings-for-a-notification-hub-in-the-azure-portal"></a>Configurar el servicio de notificaciones Push de Microsoft (MPNS) para un centro de notificaciones en el portal de Azure
Este artículo muestra cómo configurar el servicio de notificaciones Push de Microsoft (MPNS) para un centro de notificaciones de Azure mediante el portal de Azure. 

## <a name="prerequisites"></a>Requisitos previos
Si aún no ha creado un centro de notificaciones, cree uno ahora. Para más información, consulte [Creación de un centro de notificaciones de Azure en Azure Portal](create-notification-hub-portal.md). 

## <a name="configure-microsoft-push-notification-service-mpns"></a>Configurar el servicio de notificaciones Push de Microsoft (MPNS)

El siguiente procedimiento proporciona pasos para configurar el servicio de notificaciones Push de Microsoft (MPNS) para un centro de notificaciones: 

1. En el portal de Azure, en el **centro de notificaciones** página, seleccione **Windows Phone (MPNS)** en el menú izquierdo.
1. Habilite las notificaciones push sin autenticar o autenticadas:

    a. Para habilitar las notificaciones push no autenticadas, seleccione **Habilitar notificaciones de inserción sin autenticar** > **Guardar**.

      ![Captura de pantalla que muestra cómo habilitar las notificaciones push no autenticadas](./media/notification-hubs-windows-phone-get-started/azure-portal-unauth.png)

   b. Para habilitar las notificaciones push no autenticadas:
      * Seleccione **Cargar certificado** en la barra de herramientas.
      * Seleccione el icono de archivo y el archivo de certificado.
      * Escriba la contraseña del certificado.
      * Seleccione **Aceptar**.
      * En la página **Windows Phone (MPNS)**, seleccione **Guardar**.

## <a name="next-steps"></a>Pasos siguientes
Para ver un tutorial con instrucciones paso a paso para enviar notificaciones Push a dispositivos Windows Phone con Azure Notification Hubs y servicio de notificaciones Push de Microsoft (MPNS), consulte [notificaciones de inserción a aplicaciones de Windows Phone mediante el uso de notificación Concentradores](notification-hubs-windows-mobile-push-notifications-mpns.md).

