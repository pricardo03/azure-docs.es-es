---
author: conceptdev
ms.author: crdun
ms.service: app-service-mobile
ms.topic: include
ms.date: 08/23/2018
ms.openlocfilehash: 42c961b81a254adef5e42c3c8916c9c081f548c8
ms.sourcegitcommit: 58c5cd866ade5aac4354ea1fe8705cee2b50ba9f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/24/2018
ms.locfileid: "42809830"
---
1. En la vista Solución (o **Explorador de soluciones** en Visual Studio), haga clic en la carpeta **Componentes**, en **Obtener más componentes...**, busque el componente **Google Cloud Messaging Client** y agréguelo al proyecto.
2. Abra el archivo de proyecto ToDoActivity.cs y agregue la siguiente instrucción using a la clase:

    ```csharp
    using Gcm.Client;
    ```

3. En la clase **ToDoActivity** , agregue el siguiente código nuevo: 

    ```csharp
    // Create a new instance field for this activity.
    static ToDoActivity instance = new ToDoActivity();

    // Return the current activity instance.
    public static ToDoActivity CurrentActivity
    {
        get
        {
            return instance;
        }
    }
    // Return the Mobile Services client.
    public MobileServiceClient CurrentClient
    {
        get
        {
            return client;
        }
    }
    ```

    Esto le permite tener acceso a la instancia de cliente de Servicios móviles desde el proceso del servicio del controlador de inserciones.
4. Agregue el código siguiente al método **OnCreate** después de crear **MobileServiceClient**:

    ```csharp
    // Set the current instance of TodoActivity.
    instance = this;

    // Make sure the GCM client is set up correctly.
    GcmClient.CheckDevice(this);
    GcmClient.CheckManifest(this);

    // Register the app for push notifications.
    GcmClient.Register(this, ToDoBroadcastReceiver.senderIDs);
    ```

Ahora la **ToDoActivity** estará preparada para agregar notificaciones de inserción.
