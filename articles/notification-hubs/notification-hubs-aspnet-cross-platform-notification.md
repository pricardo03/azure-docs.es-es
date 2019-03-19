---
title: Envío de notificaciones entre plataformas a usuarios con Azure Notification Hubs (ASP.NET)
description: Obtenga información acerca de cómo utilizar las plantillas de Notification Hubs para enviar, en una sola solicitud, una notificación independiente de plataforma que tenga como destino todas las plataformas.
services: notification-hubs
documentationcenter: ''
author: jwargo
manager: patniko
editor: spelluru
ms.assetid: 11d2131b-f683-47fd-a691-4cdfc696f62b
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows
ms.devlang: multiple
ms.topic: article
ms.date: 01/04/2019
ms.author: jowargo
ms.openlocfilehash: 0f92b49c9d77029a9624782b49eb23f7083c49aa
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2019
ms.locfileid: "57863107"
---
# <a name="send-cross-platform-notifications-to-users-with-notification-hubs"></a>Envío de notificaciones entre plataformas a los usuarios con Notification Hubs

En un tutorial anterior, [Notificación a los usuarios con Notification Hubs], aprendió a insertar notificaciones en todos los dispositivos que están registrados en un usuario autenticado específico. En ese tutorial, se necesitaban varias solicitudes para enviar una notificación a cada plataforma de cliente compatible. Azure Notification Hubs admite plantillas, que le permiten especificar cómo un dispositivo específico desea recibir notificaciones. Este método simplifica el envío de notificaciones entre plataformas.

En este artículo se muestra cómo aprovechar las plantillas para enviar, en una sola solicitud, una notificación independiente de la plataforma que se dirige a todas las plataformas. Para más información sobre las plantillas, consulte [Introducción a Azure Notification Hubs][Templates].

> [!IMPORTANT]
> Windows Phone 8.1 y versiones anteriores no se admiten en Visual Studio 2017. Para más información, consulte [Compatibilidad y destinatarios de la plataforma de Visual Studio 2017](https://www.visualstudio.com/en-us/productinfo/vs2017-compatibility-vs).

> [!NOTE]
> Con Notification Hubs, un dispositivo puede registrar varias plantillas con la misma etiqueta. En este caso, un mensaje entrante dirigido a esa etiqueta da lugar a que se entreguen varias notificaciones al dispositivo, una por cada plantilla. Este proceso le permite mostrar el mismo mensaje en varias notificaciones visuales, como distintivo y como notificación del sistema en una aplicación de la Tienda Windows.

## <a name="send-cross-platform-notifications-using-templates"></a>Envío de notificaciones entre plataformas mediante plantillas

Para enviar notificaciones entre plataformas mediante plantillas, haga lo siguiente:

1. En el Explorador de soluciones de Visual Studio, expanda la carpeta **Controladores** y, a continuación, abra el archivo RegisterController.cs.

2. Encuentre el bloque de código en el método `Put` que crea un nuevo registro y reemplace el contenido de `switch` por el código siguiente:

    ```csharp
    switch (deviceUpdate.Platform)
    {
        case "mpns":
            var toastTemplate = "<?xml version=\"1.0\" encoding=\"utf-8\"?>" +
                "<wp:Notification xmlns:wp=\"WPNotification\">" +
                    "<wp:Toast>" +
                        "<wp:Text1>$(message)</wp:Text1>" +
                    "</wp:Toast> " +
                "</wp:Notification>";
            registration = new MpnsTemplateRegistrationDescription(deviceUpdate.Handle, toastTemplate);
            break;
        case "wns":
            toastTemplate = @"<toast><visual><binding template=""ToastText01""><text id=""1"">$(message)</text></binding></visual></toast>";
            registration = new WindowsTemplateRegistrationDescription(deviceUpdate.Handle, toastTemplate);
            break;
        case "apns":
            var alertTemplate = "{\"aps\":{\"alert\":\"$(message)\"}}";
            registration = new AppleTemplateRegistrationDescription(deviceUpdate.Handle, alertTemplate);
            break;
        case "fcm":
            var messageTemplate = "{\"data\":{\"message\":\"$(message)\"}}";
            registration = new FcmTemplateRegistrationDescription(deviceUpdate.Handle, messageTemplate);
            break;
        default:
            throw new HttpResponseException(HttpStatusCode.BadRequest);
    }
    ```

    Este código llama al método específico de la plataforma para crear un registro de plantilla en lugar de un registro nativo. Como los registros de plantilla se obtienen de registros nativos, no es necesario modificar los registros existentes.

3. En el controlador `Notifications`, reemplace el método `sendNotification` por el código siguiente:

    ```csharp
    public async Task<HttpResponseMessage> Post()
    {
        var user = HttpContext.Current.User.Identity.Name;
        var userTag = "username:" + user;

        var notification = new Dictionary<string, string> { { "message", "Hello, " + user } };
        await Notifications.Instance.Hub.SendTemplateNotificationAsync(notification, userTag);

        return Request.CreateResponse(HttpStatusCode.OK);
    }
    ```

    Este código envía una notificación a todas las plataformas al mismo tiempo, sin que sea necesario especificar una carga nativa. Notification Hubs crea y entrega la carga correcta a cada dispositivo con el valor *tag* proporcionado, tal como se especifica en las plantillas registradas.

4. Publique de nuevo su proyecto de back-end de WebApi.

5. Vuelva a ejecutar la aplicación cliente y, a continuación, compruebe que el registro se ha realizado correctamente.

6. (Opcional) Implemente la aplicación cliente en un segundo dispositivo y, a continuación, ejecute la aplicación.
    Aparece una notificación en cada dispositivo.

## <a name="next-steps"></a>Pasos siguientes

Ahora que completó este tutorial, obtenga más información acerca de Notification Hubs y las plantillas en estos temas:

* [Use Notification Hubs to send breaking news]: Demonstrates another scenario for using templates.
* [Introducción a Azure Notification Hubs][Templates]: contiene información más detallada sobre las plantillas.

<!-- Anchors. -->

<!-- Images. -->

<!-- URLs. -->
[Push to users ASP.NET]: notification-hubs-aspnet-backend-ios-apple-apns-notification.md
[Push to users Mobile Services]: notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md
[Visual Studio 2012 Express for Windows 8]: https://go.microsoft.com/fwlink/?LinkId=257546

[Use Notification Hubs to send breaking news]: notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md
[Azure Notification Hubs]: https://go.microsoft.com/fwlink/p/?LinkId=314257
[Notificación a los usuarios con Notification Hubs]: notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md
[Templates]: https://go.microsoft.com/fwlink/p/?LinkId=317339
[Notification Hub How to for Windows Store]: https://msdn.microsoft.com/library/windowsazure/jj927172.aspx
