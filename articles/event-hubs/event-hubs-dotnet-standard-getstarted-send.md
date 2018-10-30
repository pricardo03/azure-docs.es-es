---
title: Envío de eventos a Azure Event Hubs mediante .NET Standard | Microsoft Docs
description: Introducción al envío de eventos a Event Hubs en .NET Standard
services: event-hubs
documentationcenter: na
author: ShubhaVijayasarathy
manager: timlt
editor: ''
ms.assetid: ''
ms.service: event-hubs
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/18/2018
ms.author: shvija
ms.openlocfilehash: e826dcdbc6d32e6f0ad6ddf72a95869c96af6d69
ms.sourcegitcommit: 668b486f3d07562b614de91451e50296be3c2e1f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/19/2018
ms.locfileid: "49456531"
---
# <a name="get-started-sending-messages-to-azure-event-hubs-in-net-standard"></a>Introducción al envío de mensajes a Azure Event Hubs en .NET Standard
Event Hubs es un servicio que procesa grandes cantidades de datos de eventos (telemetría) desde aplicaciones y dispositivos conectados. Después de recopilar datos en Event Hubs, puede almacenarlos mediante un clúster de almacenamiento o transformarlos por medio de un proveedor de análisis en tiempo real. Esta funcionalidad de recopilación y procesamiento de eventos a gran escala es un componente clave de las modernas arquitecturas de aplicaciones, entre las que se incluye Internet de las cosas (IoT). Información general detallada de Event Hubs, consulte [Introducción a Event Hubs](event-hubs-about.md) y [Características de Event Hubs](event-hubs-features.md).

Este tutorial muestra cómo enviar eventos a un centro de eventos mediante una aplicación de consola escrita en C# con .NET Core. 

> [!NOTE]
> Puede descargar esta guía de inicio rápido como un ejemplo desde [GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleSender), reemplazar las cadenas `EventHubConnectionString` y `EventHubName` con los valores del centro de eventos, y ejecutarlo. También puede seguir los pasos de este tutorial para crear el suyo propio.

## <a name="prerequisites"></a>Requisitos previos
* [Microsoft Visual Studio 2015 o 2017](http://www.visualstudio.com). En los ejemplos de este tutorial se usa Visual Studio 2017, pero también se admite Visual Studio 2015.
* [Herramientas de .NET Core Visual Studio 2015 o 2017](http://www.microsoft.com/net/core). 

## <a name="create-an-event-hubs-namespace-and-an-event-hub"></a>Creación de un espacio de nombres de Event Hubs y un centro de eventos
El primer paso consiste en usar [Azure Portal](https://portal.azure.com) para crear un espacio de nombres de tipo Event Hubs y obtener las credenciales de administración que la aplicación necesita para comunicarse con el centro de eventos. Para crear un espacio de nombres y un centro de eventos, siga el procedimiento de [este artículo](event-hubs-create.md) y después continúe con los pasos siguientes en este tutorial.

## <a name="create-a-console-application"></a>Creación de una aplicación de consola

Inicie Visual Studio. En el menú **Archivo**, haga clic en **Nuevo** y en **Proyecto**. Cree una aplicación de consola de .NET Core.

![Nuevo proyecto][1]

## <a name="add-the-event-hubs-nuget-package"></a>Agregue el paquete NuGet de Event Hubs.

Agregue el paquete NuGet de la biblioteca estándar .NET [`Microsoft.Azure.EventHubs`](https://www.nuget.org/packages/Microsoft.Azure.EventHubs/) a su proyecto siguiendo estos pasos: 

1. Haga clic con el botón derecho en el proyecto recién creado y seleccione **Administrar paquetes NuGet**.
2. Haga clic en la pestaña **Examinar** y, después, busque "Microsoft.Azure.EventHubs" y seleccione el paquete **Microsoft.Azure.EventHubs**. Haga clic en **Instalar** para completar la instalación y, a continuación, cierre este cuadro de diálogo.

## <a name="write-code-to-send-messages-to-the-event-hub"></a>Escritura de código para enviar mensajes al centro de eventos

1. Agregue las siguientes instrucciones `using` al principio del archivo Program.cs:

    ```csharp
    using Microsoft.Azure.EventHubs;
    using System.Text;
    using System.Threading.Tasks;
    ```

2. Agregue constantes a la clase `Program` para la cadena de conexión de Event Hubs y la ruta de la entidad (nombre de centro de eventos individual). Reemplace los marcadores de posición entre llaves por los valores adecuados obtenidos al crear el centro de eventos. Asegúrese de que `{Event Hubs connection string}` es la cadena de conexión en el nivel de espacio de nombres y no la cadena de Event Hub. 

    ```csharp
    private static EventHubClient eventHubClient;
    private const string EventHubConnectionString = "{Event Hubs connection string}";
    private const string EventHubName = "{Event Hub path/name}";
    ```

3. Agregue un nuevo método denominado `MainAsync` a la clase `Program`, de esta manera:

    ```csharp
    private static async Task MainAsync(string[] args)
    {
        // Creates an EventHubsConnectionStringBuilder object from the connection string, and sets the EntityPath.
        // Typically, the connection string should have the entity path in it, but this simple scenario
        // uses the connection string from the namespace.
        var connectionStringBuilder = new EventHubsConnectionStringBuilder(EventHubConnectionString)
        {
            EntityPath = EventHubName
        };

        eventHubClient = EventHubClient.CreateFromConnectionString(connectionStringBuilder.ToString());

        await SendMessagesToEventHub(100);

        await eventHubClient.CloseAsync();

        Console.WriteLine("Press ENTER to exit.");
        Console.ReadLine();
    }
    ```

4. Agregue un nuevo método denominado `SendMessagesToEventHub` a la clase `Program`, de esta manera:

    ```csharp
    // Creates an event hub client and sends 100 messages to the event hub.
    private static async Task SendMessagesToEventHub(int numMessagesToSend)
    {
        for (var i = 0; i < numMessagesToSend; i++)
        {
            try
            {
                var message = $"Message {i}";
                Console.WriteLine($"Sending message: {message}");
                await eventHubClient.SendAsync(new EventData(Encoding.UTF8.GetBytes(message)));
            }
            catch (Exception exception)
            {
                Console.WriteLine($"{DateTime.Now} > Exception: {exception.Message}");
            }

            await Task.Delay(10);
        }

        Console.WriteLine($"{numMessagesToSend} messages sent.");
    }
    ```

5. Agregue el siguiente código al método `Main` de la clase `Program`:

    ```csharp
    MainAsync(args).GetAwaiter().GetResult();
    ```

   Este es el aspecto que debería tener Program.cs.

    ```csharp
    namespace SampleSender
    {
        using System;
        using System.Text;
        using System.Threading.Tasks;
        using Microsoft.Azure.EventHubs;

        public class Program
        {
            private static EventHubClient eventHubClient;
            private const string EventHubConnectionString = "{Event Hubs connection string}";
            private const string EventHubName = "{Event Hub path/name}";

            public static void Main(string[] args)
            {
                MainAsync(args).GetAwaiter().GetResult();
            }

            private static async Task MainAsync(string[] args)
            {
                // Creates an EventHubsConnectionStringBuilder object from the connection string, and sets the EntityPath.
                // Typically, the connection string should have the entity path in it, but for the sake of this simple scenario
                // we are using the connection string from the namespace.
                var connectionStringBuilder = new EventHubsConnectionStringBuilder(EventHubConnectionString)
                {
                    EntityPath = EventHubName
                };

                eventHubClient = EventHubClient.CreateFromConnectionString(connectionStringBuilder.ToString());

                await SendMessagesToEventHub(100);

                await eventHubClient.CloseAsync();

                Console.WriteLine("Press ENTER to exit.");
                Console.ReadLine();
            }

            // Creates an event hub client and sends 100 messages to the event hub.
            private static async Task SendMessagesToEventHub(int numMessagesToSend)
            {
                for (var i = 0; i < numMessagesToSend; i++)
                {
                    try
                    {
                        var message = $"Message {i}";
                        Console.WriteLine($"Sending message: {message}");
                        await eventHubClient.SendAsync(new EventData(Encoding.UTF8.GetBytes(message)));
                    }
                    catch (Exception exception)
                    {
                        Console.WriteLine($"{DateTime.Now} > Exception: {exception.Message}");
                    }

                    await Task.Delay(10);
                }

                Console.WriteLine($"{numMessagesToSend} messages sent.");
            }
        }
    }
    ```

6. Ejecute el programa y asegúrese de que no hay ningún error.

Felicidades. Ha enviado mensajes a un centro de eventos.

## <a name="next-steps"></a>Pasos siguientes
En esta guía de inicio rápido, ha enviado mensajes a un centro de eventos mediante .NET Standard. Para información acerca de cómo recibir eventos a un centro de eventos mediante .NET Standard, consulte [Recepción de eventos desde el centro de eventos: .NET Standard](event-hubs-dotnet-standard-getstarted-receive-eph.md).

[1]: ./media/event-hubs-dotnet-standard-getstarted-send/netcoresnd.png
