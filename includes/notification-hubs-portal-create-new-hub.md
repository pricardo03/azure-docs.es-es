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
ms.openlocfilehash: a5d29e77f6ba10ed3069cb3f5a3c8089f49c237d
ms.sourcegitcommit: f7be3cff2cca149e57aa967e5310eeb0b51f7c77
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/15/2019
ms.locfileid: "56313779"
---
1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
2. Seleccione **Todos los servicios** en el menú de la izquierda y seleccione **Notification Hubs** en la sección **Móvil**. Seleccione la estrella (`*`) junto al nombre del servicio para agregarlo a la sección **FAVORITOS** en el menú de la izquierda. Cuando **Notification Hubs** se agrega a **FAVORITOS**, selecciónelo en el menú de la izquierda. 

      ![Azure Portal: selección de Notification Hubs](./media/notification-hubs-portal-create-new-hub/all-services-select-notification-hubs.png)
3. En la página **Notification Hubs**, seleccione **Agregar** en la barra de herramientas. 

      ![Notification Hubs: botón Agregar de la barra de herramientas](./media/notification-hubs-portal-create-new-hub/add-toolbar-button.png)
4. En la página **Notification Hubs**, realice los pasos siguientes: 
    1. Especifique un **nombre** para el **centro** de notificaciones.  
    2. Especifique un **nombre** para el **espacio de nombres**. Un espacio de nombres contiene uno o más centros. 
    3. Seleccione la **ubicación** en la que quiere crear el centro de notificaciones. 
    4. Seleccione un grupo de recursos existente o escriba un nombre para el nuevo **grupo**.
    5. Seleccione **Crear**. 

        ![Azure Portal: establecimiento de las propiedades del centro de notificaciones](./media/notification-hubs-portal-create-new-hub/notification-hubs-azure-portal-settings.png)
4. Seleccione **Notificaciones** (icono de campana) y, luego, **Go to resource** (Ir al recurso). También puede actualizar la lista en la página **Notification Hubs** (Centros de notificaciones) y seleccionar el centro de notificaciones. 

      ![Azure Portal: notificaciones -> Go to resource (Ir al recurso)](./media/notification-hubs-portal-create-new-hub/go-to-notification-hub.png)
5. Seleccione **Directivas de acceso** en la lista. Anote las dos cadenas de conexión disponibles. Las necesitará para gestionar las notificaciones push más tarde.

      >[!IMPORTANT]
      >**NO** use la clave DefaultFullSharedAccessSignature en su aplicación. Está pensada para usarse solamente en el back-end.
      >

      ![Azure Portal: cadenas de conexión del centro de notificaciones](./media/notification-hubs-portal-create-new-hub/notification-hubs-connection-strings-portal.png)
