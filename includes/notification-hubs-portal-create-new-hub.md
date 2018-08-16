---
title: archivo de inclusión
description: archivo de inclusión
services: notification-hubs
author: spelluru
ms.service: notification-hubs
ms.topic: include
ms.date: 03/28/2018
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: 19ea9c749b58f6f81dc2087caa77573062d883b5
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/02/2018
ms.locfileid: "39485761"
---
1. Inicie sesión en el [Azure Portal](https://portal.azure.com).

1. Seleccione **Crear un recurso** > **Móvil** > **Centro de notificaciones**.
   
      ![Azure Portal: creación de centros de notificaciones](./media/notification-hubs-portal-create-new-hub/notification-hubs-azure-portal-create.png)
      
1. En el cuadro **Centro de notificaciones**, escriba un nombre único. Seleccione el elemento **Región**, **Suscripción** y **Grupo de recursos** (si dispone ya de uno). 
   
      Si aún no tiene un espacio de nombres de Service Bus, puede usar el predeterminado, que se crea en función del nombre del centro (si está disponible el espacio de nombres).
    
      Si ya tiene un espacio de nombres de Service Bus donde crear el centro, siga estos pasos:

    a. En el área **Espacio de nombres**, seleccione el vínculo **Seleccionar uno existente**. 
   
    b. Seleccione **Crear**.
   
      ![Azure Portal: establecimiento de las propiedades del centro de notificaciones](./media/notification-hubs-portal-create-new-hub/notification-hubs-azure-portal-settings.png)

1. Seleccione **Notificaciones** (icono de campana) y, luego, **Go to resource** (Ir al recurso). 

      ![Azure Portal: notificaciones -> Go to resource (Ir al recurso)](./media/notification-hubs-portal-create-new-hub/notification-go-to-resource.png)    
1. Seleccione **Directivas de acceso** en la lista. Anote las dos cadenas de conexión disponibles. Las necesitará para gestionar las notificaciones push más tarde.

      >[!IMPORTANT]
      >**NO** use la clave DefaultFullSharedAccessSignature en su aplicación. Está pensada para usarse solamente en el back-end.
      >
   
      ![Azure Portal: cadenas de conexión del centro de notificaciones](./media/notification-hubs-portal-create-new-hub/notification-hubs-connection-strings-portal.png)

