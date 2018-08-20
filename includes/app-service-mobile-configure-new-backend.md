---
title: archivo de inclusión
description: archivo de inclusión
services: app-service\mobile
author: conceptdev
ms.service: app-service-mobile
ms.topic: include
ms.date: 05/25/2018
ms.author: crdun
ms.custom: include file
ms.openlocfilehash: 1d3bfb7bc8a5432392dba3b0c5019902b3e59773
ms.sourcegitcommit: eaad191ede3510f07505b11e2d1bbfbaa7585dbd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/03/2018
ms.locfileid: "39513855"
---
1. Haga clic en el botón **App Services**, seleccione sucesivamente el back-end de Mobile Apps, **Inicio rápido** y la plataforma de cliente (iOS, Android, Xamarin, Cordova).

    ![Azure Portal con inicio rápido de Mobile Apps resaltado][quickstart]

1. Si no hay configurada ninguna conexión de base de datos, cree una haciendo lo siguiente:

    ![Azure Portal con Mobile Apps: conexión a la base de datos][connect]

    a. Cree una nueva instancia y un nuevo servidor de SQL Database. Puede que tenga que dejar el campo del nombre de la cadena de conexión en el valor predeterminado MS_TableConnectionString para completar el paso 3 siguiente.

    ![Azure Portal con Mobile Apps: creación de una nueva base de datos y un nuevo servidor][server]

    b. Espere hasta que se haya creado correctamente la conexión de datos.

    ![Notificación de Azure Portal de la creación correcta de conexión de datos][notification]

    c. La conexión de datos debe haberse creado correctamente.

    ![Notificación de Azure Portal: "Ya tiene una conexión de datos"][already-connection]

1. En **2. Crear una API de tabla**, seleccione Node.js para **Lenguaje de back-end**.

1. Acepte la confirmación y seleccione **Crear tabla TodoItem**.
    Esta acción crea una nueva tabla de elementos pendientes en la base de datos.

    >[!IMPORTANT]
    > El cambio de un back-end existente a Node.js sobrescribe todo el contenido. Para crear un back-end de .NET en su lugar, consulte [Trabajar con el SDK del servidor back-end de .NET para Mobile Apps][instructions].

<!-- Images. -->
[quickstart]: ./media/app-service-mobile-configure-new-backend/quickstart.png
[connect]: ./media/app-service-mobile-configure-new-backend/connect-to-bd.png
[notification]: ./media/app-service-mobile-configure-new-backend/notification-data-connection-create.png
[server]: ./media/app-service-mobile-configure-new-backend/create-new-server.png
[already-connection]: ./media/app-service-mobile-configure-new-backend/already-connection.png

<!-- URLs -->
[instructions]: ../articles/app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#create-app
