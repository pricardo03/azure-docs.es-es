---
title: Recepción de eventos desde Azure Event Hubs mediante la biblioteca de .NET Standard | Microsoft Docs
description: Introducción a la recepción de mensajes con EventProcessorHost en .NET Standard
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
ms.date: 08/16/2018
ms.author: shvija
ms.openlocfilehash: 03cba90874d0f42e6c404009dc4115fb4f1798ed
ms.sourcegitcommit: 62759a225d8fe1872b60ab0441d1c7ac809f9102
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/19/2018
ms.locfileid: "49468082"
---
# <a name="get-started-receiving-messages-with-the-event-processor-host-in-net-standard"></a>Introducción a la recepción de mensajes con el Host del procesador de eventos en .NET Standard
Event Hubs es un servicio que procesa grandes cantidades de datos de eventos (telemetría) desde aplicaciones y dispositivos conectados. Después de recopilar datos en Event Hubs, puede almacenarlos mediante un clúster de almacenamiento o transformarlos por medio de un proveedor de análisis en tiempo real. Esta funcionalidad de recopilación y procesamiento de eventos a gran escala es un componente clave de las modernas arquitecturas de aplicaciones, entre las que se incluye Internet de las cosas (IoT). Información general detallada de Event Hubs, consulte [Introducción a Event Hubs](event-hubs-about.md) y [Características de Event Hubs](event-hubs-features.md).

En este tutorial se muestra cómo escribir una aplicación de consola de .NET Core que reciba mensajes de un centro de eventos mediante [host de procesador de eventos](event-hubs-event-processor-host.md). El [host de procesador de eventos](event-hubs-event-processor-host.md) es una clase .NET que simplifica la recepción de eventos desde centros de eventos al administrar los puntos de control persistentes y las recepciones paralelas desde tales centros de eventos. Mediante el host de procesador de eventos, puede dividir eventos entre varios receptores, aunque estén hospedados en distintos nodos. En este ejemplo se muestra cómo usar el host de procesador de eventos en un solo receptor. En el ejemplo [Escalado horizontal del procesamiento de eventos][Escalado horizontal del procesamiento de eventos con Event Hubs] se muestra cómo usar el host de procesador de eventos con varios receptores.

> [!NOTE]
> Puede descargar esta guía de inicio rápido como un ejemplo desde [GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleEphReceiver), reemplazar las cadenas `EventHubConnectionString` y `EventHubName`, `StorageAccountName`, `StorageAccountKey` y `StorageContainerName` con los valores del centro de eventos y ejecutarlo. También puede seguir los pasos de este tutorial para crear el suyo propio.

## <a name="prerequisites"></a>Requisitos previos
* [Microsoft Visual Studio 2015 o 2017](http://www.visualstudio.com). En los ejemplos de este tutorial se usa Visual Studio 2017, pero también se admite Visual Studio 2015.
* [Herramientas de .NET Core Visual Studio 2015 o 2017](http://www.microsoft.com/net/core).

## <a name="create-an-event-hubs-namespace-and-an-event-hub"></a>Creación de un espacio de nombres de Event Hubs y un centro de eventos
El primer paso consiste en usar [Azure Portal](https://portal.azure.com) para crear un espacio de nombres de tipo Event Hubs y obtener las credenciales de administración que la aplicación necesita para comunicarse con el centro de eventos. Para crear un espacio de nombres y un centro de eventos, siga el procedimiento de [este artículo](event-hubs-create.md) y después continúe con los pasos siguientes en este tutorial.

[!INCLUDE [event-hubs-create-storage](../../includes/event-hubs-create-storage.md)]

## <a name="create-a-console-application"></a>Creación de una aplicación de consola

Inicie Visual Studio. En el menú **Archivo**, haga clic en **Nuevo** y en **Proyecto**. Cree una aplicación de consola de .NET Core.

![Nuevo proyecto][2]

## <a name="add-the-event-hubs-nuget-package"></a>Incorporación del paquete NuGet de Event Hubs

Agregue los paquetes NuGet de la biblioteca de .NET Standard [ **Microsoft.Azure.EventHubs**](https://www.nuget.org/packages/Microsoft.Azure.EventHubs/) y [**Microsoft.Azure.EventHubs.Processor**](https://www.nuget.org/packages/Microsoft.Azure.EventHubs.Processor/) al proyecto mediante estos pasos: 

1. Haga clic con el botón derecho en el proyecto recién creado y seleccione **Administrar paquetes NuGet**.
2. Haga clic en la pestaña **Examinar**, busque **Microsoft.Azure.EventHubs** y seleccione el paquete **Microsoft.Azure.EventHubs**. Haga clic en **Instalar** para completar la instalación y, a continuación, cierre este cuadro de diálogo.
3. Repita los pasos 1 y 2 e instale el paquete **Microsoft.Azure.EventHubs.Processor**.

## <a name="implement-the-ieventprocessor-interface"></a>Implementación de la interfaz de IEventProcessor

1. En el Explorador de soluciones, haga clic con el botón derecho en el proyecto, haga clic en **Agregar** y después en **Clase**. Asigne el nombre **SimpleEventProcessor** a la nueva clase.

2. Abra el archivo SimpleEventProcessor.cs y agregue las siguientes instrucciones `using` en la parte superior del archivo.

    ```csharp
    using Microsoft.Azure.EventHubs;
    using Microsoft.Azure.EventHubs.Processor;
    using System.Threading.Tasks;
    ```

3. Implemente la interfaz `IEventProcessor`. Reemplace todo el contenido de la clase `SimpleEventProcessor` con el código siguiente:

    ```csharp
    public class SimpleEventProcessor : IEventProcessor
    {
        public Task CloseAsync(PartitionContext context, CloseReason reason)
        {
            Console.WriteLine($"Processor Shutting Down. Partition '{context.PartitionId}', Reason: '{reason}'.");
            return Task.CompletedTask;
        }

        public Task OpenAsync(PartitionContext context)
        {
            Console.WriteLine($"SimpleEventProcessor initialized. Partition: '{context.PartitionId}'");
            return Task.CompletedTask;
        }

        public Task ProcessErrorAsync(PartitionContext context, Exception error)
        {
            Console.WriteLine($"Error on Partition: {context.PartitionId}, Error: {error.Message}");
            return Task.CompletedTask;
        }

        public Task ProcessEventsAsync(PartitionContext context, IEnumerable<EventData> messages)
        {
            foreach (var eventData in messages)
            {
                var data = Encoding.UTF8.GetString(eventData.Body.Array, eventData.Body.Offset, eventData.Body.Count);
                Console.WriteLine($"Message received. Partition: '{context.PartitionId}', Data: '{data}'");
            }

            return context.CheckpointAsync();
        }
    }
    ```

## <a name="update-the-main-method-to-use-simpleeventprocessor"></a>Actualización del método Main para usar SimpleEventProcessor

1. Agregue las siguientes instrucciones `using` al principio del archivo Program.cs.

    ```csharp
    using Microsoft.Azure.EventHubs;
    using Microsoft.Azure.EventHubs.Processor;
    using System.Threading.Tasks;
    ```

2. Agregue constantes a la clase `Program` para la cadena de conexión del centro de eventos, el nombre del centro de eventos, el nombre del contenedor de la cuenta de almacenamiento, y el nombre de la cuenta de almacenamiento y la clave de la cuenta de almacenamiento. Agregue el código siguiente, reemplazando los marcadores de posición con sus valores correspondientes:

    ```csharp
    private const string EventHubConnectionString = "{Event Hubs connection string}";
    private const string EventHubName = "{Event Hub path/name}";
    private const string StorageContainerName = "{Storage account container name}";
    private const string StorageAccountName = "{Storage account name}";
    private const string StorageAccountKey = "{Storage account key}";

    private static readonly string StorageConnectionString = string.Format("DefaultEndpointsProtocol=https;AccountName={0};AccountKey={1}", StorageAccountName, StorageAccountKey);
    ```   

3. Agregue un nuevo método denominado `MainAsync` a la clase `Program`, de esta manera:

    ```csharp
    private static async Task MainAsync(string[] args)
    {
        Console.WriteLine("Registering EventProcessor...");

        var eventProcessorHost = new EventProcessorHost(
            EventHubName,
            PartitionReceiver.DefaultConsumerGroupName,
            EventHubConnectionString,
            StorageConnectionString,
            StorageContainerName);

        // Registers the Event Processor Host and starts receiving messages
        await eventProcessorHost.RegisterEventProcessorAsync<SimpleEventProcessor>();

        Console.WriteLine("Receiving. Press ENTER to stop worker.");
        Console.ReadLine();

        // Disposes of the Event Processor Host
        await eventProcessorHost.UnregisterEventProcessorAsync();
    }
    ```

3. Agregue la siguiente línea de código al método `Main`:

    ```csharp
    MainAsync(args).GetAwaiter().GetResult();
    ```

    Este es el aspecto que debería tener el archivo Program.cs:

    ```csharp
    namespace SampleEphReceiver
    {

        public class Program
        {
            private const string EventHubConnectionString = "{Event Hubs connection string}";
            private const string EventHubName = "{Event Hub path/name}";
            private const string StorageContainerName = "{Storage account container name}";
            private const string StorageAccountName = "{Storage account name}";
            private const string StorageAccountKey = "{Storage account key}";

            private static readonly string StorageConnectionString = string.Format("DefaultEndpointsProtocol=https;AccountName={0};AccountKey={1}", StorageAccountName, StorageAccountKey);

            public static void Main(string[] args)
            {
                MainAsync(args).GetAwaiter().GetResult();
            }

            private static async Task MainAsync(string[] args)
            {
                Console.WriteLine("Registering EventProcessor...");

                var eventProcessorHost = new EventProcessorHost(
                    EventHubName,
                    PartitionReceiver.DefaultConsumerGroupName,
                    EventHubConnectionString,
                    StorageConnectionString,
                    StorageContainerName);

                // Registers the Event Processor Host and starts receiving messages
                await eventProcessorHost.RegisterEventProcessorAsync<SimpleEventProcessor>();

                Console.WriteLine("Receiving. Press ENTER to stop worker.");
                Console.ReadLine();

                // Disposes of the Event Processor Host
                await eventProcessorHost.UnregisterEventProcessorAsync();
            }
        }
    }
    ```

4. Ejecute el programa y asegúrese de que no hay ningún error.

Felicidades. Recibió mensajes de un centro de eventos mediante el host de procesador de eventos.

> [!NOTE]
> Este tutorial usa una sola instancia de [EventProcessorHost](event-hubs-event-processor-host.md). Para aumentar el rendimiento, se recomienda ejecutar varias instancias de [EventProcessorHost](event-hubs-event-processor-host.md) como se muestra en el ejemplo [Escalado horizontal del procesamiento de eventos](https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3). En esos casos, las diferentes instancias se coordinan automáticamente entre sí con el fin de equilibrar la carga de los eventos recibidos. 

## <a name="next-steps"></a>Pasos siguientes
En esta guía de inicio rápido, ha creado la aplicación de .NET Standard que recibe mensajes desde un centro de eventos. Para obtener información acerca de cómo enviar eventos a un centro de eventos mediante .NET Standard, consulte [Envío de eventos desde el centro de eventos con .NET Standard](event-hubs-dotnet-standard-getstarted-send.md).

[1]: ./media/event-hubs-dotnet-standard-getstarted-receive-eph/event-hubs-python1.png
[2]: ./media/event-hubs-dotnet-standard-getstarted-receive-eph/netcorercv.png
