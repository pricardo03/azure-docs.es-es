---
title: archivo de inclusión
description: archivo de inclusión
services: notification-hubs
author: jwargo
ms.service: notification-hubs
ms.topic: include
ms.date: 01/17/2019
ms.author: jowargo
ms.custom: include file
ms.openlocfilehash: b68fa345d4772134c30ce8b8b559f98113a0496f
ms.sourcegitcommit: 9b6492fdcac18aa872ed771192a420d1d9551a33
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/22/2019
ms.locfileid: "54453109"
---
1. Inicie sesión en el [Azure Portal](https://portal.azure.com).

2. Seleccione **Crear un recurso** > **Móvil** > **Centro de notificaciones**.

      ![Azure Portal: creación de centros de notificaciones](./media/notification-hubs-portal-create-new-hub/notification-hubs-azure-portal-create.png)

3. En el cuadro **Centro de notificaciones**, escriba un nombre único. Seleccione el elemento **Región**, **Suscripción** y **Grupo de recursos** (si dispone ya de uno).

      Si aún no tiene un espacio de nombres de Service Bus, puede usar el predeterminado, que se crea en función del nombre del centro (si está disponible el espacio de nombres).

      Si ya tiene un espacio de nombres de Service Bus donde crear el centro, siga estos pasos:

     a. En el área **Espacio de nombres**, seleccione el vínculo **Seleccionar uno existente**.

    b. Seleccione **Crear**.

    ![Azure Portal: establecimiento de las propiedades del centro de notificaciones](./media/notification-hubs-portal-create-new-hub/notification-hubs-azure-portal-settings.png)

4. Seleccione **Notificaciones** (icono de campana) y, luego, **Go to resource** (Ir al recurso).

      ![Azure Portal: notificaciones -> Go to resource (Ir al recurso)](./media/notification-hubs-portal-create-new-hub/notification-go-to-resource.png)
5. Seleccione **Directivas de acceso** en la lista. Anote las dos cadenas de conexión disponibles. Las necesitará para gestionar las notificaciones push más tarde.

      >[!IMPORTANT]
      >**NO** use la clave DefaultFullSharedAccessSignature en su aplicación. Está pensada para usarse solamente en el back-end.
      >

      ![Azure Portal: cadenas de conexión del centro de notificaciones](./media/notification-hubs-portal-create-new-hub/notification-hubs-connection-strings-portal.png)
