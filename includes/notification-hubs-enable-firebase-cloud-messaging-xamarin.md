---
title: archivo de inclusión
description: archivo de inclusión
services: notification-hubs
author: spelluru
ms.service: notification-hubs
ms.topic: include
ms.date: 03/11/2019
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: 90ffebf5f3375e94545f82a95f5c240ad845bd94
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2019
ms.locfileid: "66162171"
---
1. Inicie sesión en la [consola Firebase](https://firebase.google.com/console/). Si aún no tiene uno, cree un nuevo proyecto de Firebase.
2. Después de crear el proyecto, seleccione **Add Firebase to your Android app** (Añade Firebase a tu aplicación de Android). 

    ![Agregar Firebase a una aplicación de Android](./media/notification-hubs-enable-firebase-cloud-messaging/notification-hubs-add-firebase-to-android-app.png)
3. En la página **Agregar Firebase a una aplicación de Android**, realice los pasos siguientes: 
    1. En **Nombre del paquete de Android**, escriba un nombre para el paquete. Por ejemplo: `tutorials.tutoria1.xamarinfcmapp`. 

        ![Especifique el nombre del paquete.](./media/notification-hubs-enable-firebase-cloud-messaging/specify-package-name-fcm-settings.png)
    2. Seleccione **Registrar aplicación**. 
4. Seleccione **Descargar google-services.json**, guarde el archivo en la carpeta **app** del proyecto y, a continuación, seleccione **Siguiente**. 

    ![Descargue el archivo google-services.json.](./media/notification-hubs-enable-firebase-cloud-messaging/download-google-service-button.png)
6. Seleccione **Siguiente** en la página. 
7. Seleccione **Omitir este paso** en la página. 

    ![Omita el último paso.](./media/notification-hubs-enable-firebase-cloud-messaging/skip-this-step.png)
8. En la consola Firebase, seleccione el icono de la rueda dentada del proyecto. Luego, seleccione **Project Settings** (Configuración del proyecto).

    ![Seleccionar Project Settings (Configuración del proyecto)](./media/notification-hubs-enable-firebase-cloud-messaging/notification-hubs-firebase-console-project-settings.png)
4. Si no ha descargado el archivo **google-services.json**, puede hacerlo en esta página. 
5. Cambie a la pestaña **Cloud Messaging** de la parte superior. 
6. Copie y guarde la **clave del servidor** para su uso posterior. Este valor se utilizará para configurar el centro de notificaciones.
