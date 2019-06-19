---
title: archivo de inclusión
description: archivo de inclusión
services: notification-hubs
author: spelluru
ms.service: notification-hubs
ms.topic: include
ms.date: 03/30/2018
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: f0ff729084d194ff2e05e89eadc45782f775b1c5
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/18/2019
ms.locfileid: "67186560"
---
En esta sección se envían noticias de última hora como notificaciones de plantilla con etiquetas desde una aplicación de consola .NET. 

1. En Visual Studio, cree una nueva aplicación de consola en Visual C#: a. En el menú, seleccione **Archivo** > **Nuevo** > **Proyecto**.
    b. Expanda **Visual C#** y seleccione **Escritorio de Windows**. 
    c. Seleccione **Aplicación de consola (.NET Framework)** en la lista de plantillas. 
    d. Escriba un **nombre** para la aplicación. 
    e. Seleccione una **carpeta** para la aplicación.
    f. Haga clic en **Aceptar** para crear el proyecto. 
2. En el menú principal de Visual Studio, seleccione **Herramientas** > **Administrador de paquetes NuGet** > **Consola del administrador de paquetes** y, en la ventana de la consola, escriba la cadena siguiente:
   
    ```
    Install-Package Microsoft.Azure.NotificationHubs
    ```
   
3. Seleccione **Entrar**.  
    Esta acción agrega una referencia al SDK de Azure Notification Hubs mediante el [paquete NuGet Microsoft.Azure.NotificationHubs].

4. Abra el archivo Program.cs y agregue la siguiente instrucción `using`:
   
    ```csharp
    using Microsoft.Azure.NotificationHubs;
    ```

5. En la clase `Program` , agregue el siguiente método o reemplácelo si ya existe:
   
    ```csharp
    private static async void SendTemplateNotificationAsync()
    {
        // Define the notification hub.
        NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("<connection string with full access>", "<hub name>");

        // Create an array of breaking news categories.
        var categories = new string[] { "World", "Politics", "Business", "Technology", "Science", "Sports"};

        // Send the notification as a template notification. All template registrations that contain
        // "messageParam" and the proper tags will receive the notifications.
        // This includes APNS, GCM, WNS, and MPNS template registrations.

        Dictionary<string, string> templateParams = new Dictionary<string, string>();

        foreach (var category in categories)
        {
            templateParams["messageParam"] = "Breaking " + category + " News!";
            await hub.SendTemplateNotificationAsync(templateParams, category);
        }
    }
    ```   
   
    Este código envía una notificación de plantilla para cada una de las seis etiquetas en la matriz de cadenas. El uso de etiquetas garantiza que los dispositivos reciben notificaciones solo de las categorías registradas.

5. En el código anterior, reemplace los marcadores de posición `<hub name>` y `<connection string with full access>` por el nombre del centro de notificaciones y la cadena de conexión de *DefaultFullSharedAccessSignature* del panel del centro de notificaciones.

6. Agregue las siguientes líneas al método **Main**:
   
    ```csharp
    SendTemplateNotificationAsync();
    Console.ReadLine();
    ```

7. Compile la aplicación de consola.

<!-- Images. -->
[13]: ./media/notification-hubs-back-end/notification-hub-create-console-app.png

<!-- URLs. -->
[Get started with Notification Hubs]: ../articles/notification-hubs/notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md
[Notification Hubs REST interface]: https://msdn.microsoft.com/library/windowsazure/dn223264.aspx
[Add push notifications for Mobile Apps]: ../articles/app-service-mobile/app-service-mobile-windows-store-dotnet-get-started-push.md
[How to use Notification Hubs from Java or PHP]: ../articles/notification-hubs/notification-hubs-java-push-notification-tutorial.md
[paquete NuGet Microsoft.Azure.NotificationHubs]: http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/
