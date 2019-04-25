---
title: Configuración de Apple Push Notification Service en Azure Notification Hubs | Microsoft Docs
description: Obtenga información sobre cómo configurar un centro de notificaciones de Azure con la configuración de Apple Push Notification Service (APNS).
services: notification-hubs
author: jwargo
manager: patniko
editor: spelluru
ms.service: notification-hubs
ms.workload: mobile
ms.topic: article
ms.date: 03/25/2019
ms.author: jowargo
ms.openlocfilehash: 9a9db9f05895569b050e56cdeec1ee2ee25af0df
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60237826"
---
# <a name="configure-apple-push-notification-service-apns-settings-for-a-notification-hub-in-the-azure-portal"></a>Configuración de Apple Push Notification Service (APNS) para un centro de notificaciones en el portal de Azure
Este artículo muestra cómo configurar opciones de Apple Push Notification Service (APNS) para un centro de notificaciones de Azure mediante el portal de Azure. 

## <a name="prerequisites"></a>Requisitos previos
Si aún no ha creado un centro de notificaciones, cree uno ahora. Para más información, consulte [Creación de un centro de notificaciones de Azure en Azure Portal](create-notification-hub-portal.md). 

## <a name="configure-apple-push-notification-service"></a>Configurar el servicio de notificaciones Push de Apple

El siguiente procedimiento proporciona pasos para configurar las opciones de Apple Push Notification Service (APNS) para un centro de notificaciones:

1. En el portal de Azure, en el **centro de notificaciones** página, seleccione **Apple (APN)** en el menú izquierdo.

1. Para **Modo de autenticación**, seleccione **Certificado** o **Token**.

    a. Si ha seleccionado **Certificado**:
   * Seleccione el icono de archivo, y luego el archivo *.p12* que desea cargar.
   * Escriba una contraseña.
   * Seleccione el modo **Espacio aislado**. O para enviar notificaciones push a los usuarios que compraron la aplicación en la tienda, seleccione el modo **Producción**.

     ![Captura de pantalla de una configuración del certificado APNS en Azure Portal](./media/notification-hubs-ios-get-started/notification-hubs-apple-config-cert.png)

   b. Si selecciona **Token**:

   * Especifique los valores **Id. de clave**, **Id. de agrupación**, **Id. de equipo** y **Token**.
   * Seleccione el modo **Espacio aislado**. O para enviar notificaciones push a los usuarios que compraron la aplicación en la tienda, seleccione el modo **Producción**.

     ![Captura de pantalla de una configuración del token APNS en Azure Portal](./media/notification-hubs-ios-get-started/notification-hubs-apple-config-token.png)

## <a name="next-steps"></a>Pasos siguientes
Para ver un tutorial con instrucciones paso a paso para enviar notificaciones Push a dispositivos iOS, consulte el artículo siguiente: [Envío de notificaciones push a aplicaciones iOS mediante Notification Hubs y APNS](notification-hubs-ios-apple-push-notification-apns-get-started.md)
