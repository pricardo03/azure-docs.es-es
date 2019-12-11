---
title: Agregar notificaciones push a la aplicación de Xamarin.Android
description: Aprenda a usar Azure App Service y Azure Notification Hubs para enviar notificaciones push a la aplicación de Xamarin Android.
ms.assetid: 6f7e8517-e532-4559-9b07-874115f4c65b
ms.tgt_pltfrm: mobile-xamarin-android
ms.devlang: dotnet
ms.topic: article
ms.date: 06/25/2019
ms.openlocfilehash: c9dc4c825d65287f152522868a2b9e6a38ea70bb
ms.sourcegitcommit: 3d4917ed58603ab59d1902c5d8388b954147fe50
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/02/2019
ms.locfileid: "74668741"
---
# <a name="add-push-notifications-to-your-xamarinandroid-app"></a>Agregar notificaciones push a la aplicación de Xamarin.Android

[!INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

> [!NOTE]
> Visual Studio App Center admite servicios integrados de un extremo a otro fundamentales para el desarrollo de aplicaciones móviles. Los desarrolladores pueden usar los servicios de **compilación**, **prueba** y **distribución** para configurar la canalización de integración y entrega continuas. Una vez que se ha implementado la aplicación, los desarrolladores pueden supervisar el estado y el uso de su aplicación con los servicios de **análisis** y **diagnóstico**, e interactuar con los usuarios que utilizan el servicio de **Push** (inserción). Además, los desarrolladores pueden aprovechar **Auth** para autenticar a los usuarios y el servicio de **datos** para almacenar y sincronizar los datos de la aplicación en la nube.
>
> Si está pensando en integrar servicios en la nube en su aplicación para dispositivos móviles, regístrese en [App Center](https://appcenter.ms/?utm_source=zumo&utm_medium=Azure&utm_campaign=zumo%20doc) hoy mismo.

## <a name="overview"></a>Información general

En este tutorial, agregará notificaciones push al proyecto de [inicio rápido de Xamarin.Android](app-service-mobile-windows-store-dotnet-get-started.md) para que se inserte un registro cada vez que se envíe una notificación push al dispositivo.

Si no usa el proyecto de servidor de inicio rápido descargado, necesitará el paquete de extensión de notificaciones push. Para más información, consulte [Trabajar con el SDK del servidor back-end de .NET para Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

## <a name="prerequisites"></a>Requisitos previos

En este tutorial, es necesario configurar:

* Una cuenta de Google activa. Puede registrarse para obtener una cuenta de Google en [accounts.google.com](https://go.microsoft.com/fwlink/p/?LinkId=268302).
* [Componente del cliente de Google Cloud Messaging](https://components.xamarin.com/view/GCMClient/).

## <a name="configure-hub"></a>Configurar un Centro de notificaciones

[!INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

## <a id="register"></a>Habilitar la mensajería en la nube Firebase

[!INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

## <a name="configure-azure-to-send-push-requests"></a>Configuración de Azure para enviar notificaciones de inserción

[!INCLUDE [app-service-mobile-android-configure-push](../../includes/app-service-mobile-android-configure-push-for-firebase.md)]

## <a id="update-server"></a>Actualizar el proyecto de servidor para que envíe notificaciones push

[!INCLUDE [app-service-mobile-update-server-project-for-push-template](../../includes/app-service-mobile-update-server-project-for-push-template.md)]

## <a id="configure-app"></a>Configurar el proyecto cliente para las notificaciones push

[!INCLUDE [mobile-services-xamarin-android-push-configure-project](../../includes/mobile-services-xamarin-android-push-configure-project.md)]

## <a id="add-push"></a>Incorporación de código de notificaciones de inserción a la aplicación

[!INCLUDE [app-service-mobile-xamarin-android-push-add-to-app](../../includes/app-service-mobile-xamarin-android-push-add-to-app.md)]

## <a name="test"></a>Prueba de las notificaciones de inserción en su aplicación

Puede probar la aplicación con un dispositivo virtual en el emulador. Hay pasos de configuración adicionales necesarios cuando se ejecuta en un emulador.

1. El dispositivo virtual debe tener las API de Google configuradas como destino en el administrador de dispositivos virtuales Android (AVD).

    ![](./media/app-service-mobile-xamarin-android-get-started-push/google-apis-avd-settings.png)

2. Agregue una cuenta de Google al dispositivo Android haciendo clic en **Aplicaciones** > **Configuración** > **Agregar cuenta** y luego siga las indicaciones.

    ![](./media/app-service-mobile-xamarin-android-get-started-push/add-google-account.png)

3. Ejecute la aplicación todolist como antes e inserte un nuevo elemento todo. Esta vez, se muestra un icono de notificación en el área de notificación. Puede abrir el cajón de notificaciones para ver el texto completo de la notificación.

    ![](./media/app-service-mobile-xamarin-android-get-started-push/android-notifications.png)

<!-- URLs. -->
[Xamarin.Android quick start]: app-service-mobile-xamarin-android-get-started.md
[Google Cloud Messaging Client Component]: https://components.xamarin.com/view/GCMClient/
[Azure Mobile Services Component]: https://components.xamarin.com/view/azure-mobile-services/
