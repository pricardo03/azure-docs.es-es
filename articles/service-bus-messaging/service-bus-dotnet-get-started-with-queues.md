---
title: Introducción a las colas de Azure Service Bus | Microsoft Docs
description: Escriba una aplicación de consola en C# que use las colas de mensajería de Service Bus.
services: service-bus-messaging
documentationcenter: .net
author: axisc
manager: timlt
editor: spelluru
ms.assetid: 68a34c00-5600-43f6-bbcc-fea599d500da
ms.service: service-bus-messaging
ms.devlang: tbd
ms.topic: conceptual
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 04/10/2019
ms.author: aschhab
ms.openlocfilehash: 22996b277aba96cbbfedbb3e9cc67644d1a535ca
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/18/2019
ms.locfileid: "59799021"
---
# <a name="get-started-with-service-bus-queues"></a>Introducción a las colas de Service Bus
[!INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]
En este tutorial, cree aplicaciones de consola .NET Core para enviar y recibir mensajes de una cola de Service Bus. 

## <a name="prerequisites"></a>Requisitos previos

1. [Visual Studio 2017 Update 3 (versión 15.3, 26730.01)](https://www.visualstudio.com/vs) o posterior.
2. [NET Core SDK](https://www.microsoft.com/net/download/windows), versión 2.0 o posterior.
2. Una suscripción de Azure. Para completar este tutorial, deberá tener una cuenta de Azure. Puede activar su [ventajas de suscriptor MSDN](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/?WT.mc_id=A85619ABF) o registrarse para obtener un [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A85619ABF).
3. Si no tiene una cola para que funcione con, siga los pasos de la [uso de Azure portal para crear una cola de Service Bus](service-bus-quickstart-portal.md) artículo para crear una cola.
    1. Leer el breve **Introducción** de Service Bus **colas**. 
    2. Creación de un Bus de servicio **espacio de nombres**. 
    3. Obtener el **cadena de conexión**. 
    4. Creación de un Bus de servicio **cola**. 

## <a name="send-messages-to-the-queue"></a>Envío de mensajes a la cola

Para enviar mensajes a la cola, escriba una aplicación de consola en C# mediante Visual Studio.

### <a name="create-a-console-application"></a>Creación de una aplicación de consola

Inicie Visual Studio y cree un nuevo proyecto **Aplicación de consola (.NET Core)**.

### <a name="add-the-service-bus-nuget-package"></a>Agregar el paquete NuGet de Service Bus

1. Haga clic con el botón derecho en el proyecto recién creado y seleccione **Administrar paquetes NuGet**.
2. Haga clic en la pestaña **Examinar**, busque **[Microsoft.Azure.ServiceBus](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus/)** y seleccione el elemento **WindowsAzure.ServiceBus**. Haga clic en **Instalar** para completar la instalación y, a continuación, cierre este cuadro de diálogo.
   
    ![Seleccionar un paquete NuGet][nuget-pkg]

### <a name="write-code-to-send-messages-to-the-queue"></a>Escritura del código para enviar mensajes a la cola

1. En Program.cs, agregue las siguientes instrucciones `using` en la parte superior de la definición del espacio de nombres, antes de la declaración de clase:
   
    ```csharp
    using System.Text;
    using System.Threading;
    using System.Threading.Tasks;
    using Microsoft.Azure.ServiceBus;
    ```

2. En la clase `Program`, declare las variables siguientes. Establezca la variable `ServiceBusConnectionString` en la cadena de conexión que obtuvo al crear el espacio de nombres y establezca `QueueName` en el nombre que usó cuando creó la cola:
   
    ```csharp
    const string ServiceBusConnectionString = "<your_connection_string>";
    const string QueueName = "<your_queue_name>";
    static IQueueClient queueClient;
    ``` 

3. Reemplace el contenido predeterminado de `Main()` con la siguiente línea de código:

    ```csharp
    MainAsync().GetAwaiter().GetResult();
    ```

4. Directamente después de `Main()`, agregue el siguiente método `MainAsync()` asincrónico que llama al método para enviar mensajes:

    ```csharp
    static async Task MainAsync()
    {
        const int numberOfMessages = 10;
        queueClient = new QueueClient(ServiceBusConnectionString, QueueName);

        Console.WriteLine("======================================================");
        Console.WriteLine("Press ENTER key to exit after sending all the messages.");
        Console.WriteLine("======================================================");

        // Send messages.
        await SendMessagesAsync(numberOfMessages);

        Console.ReadKey();

        await queueClient.CloseAsync();
    }
    ```

5. Directamente después del método `MainAsync()`, agregue el siguiente método `SendMessagesAsync()` que realiza el trabajo de enviar el número de mensajes especificado por `numberOfMessagesToSend` (actualmente se establece en 10):

    ```csharp
    static async Task SendMessagesAsync(int numberOfMessagesToSend)
    {
        try
        {
            for (var i = 0; i < numberOfMessagesToSend; i++)
            {
                // Create a new message to send to the queue.
                string messageBody = $"Message {i}";
                var message = new Message(Encoding.UTF8.GetBytes(messageBody));

                // Write the body of the message to the console.
                Console.WriteLine($"Sending message: {messageBody}");

                // Send the message to the queue.
                await queueClient.SendAsync(message);
            }
        }
        catch (Exception exception)
        {
            Console.WriteLine($"{DateTime.Now} :: Exception: {exception.Message}");
        }
    }
    ```
   
6. Así debería ser el archivo Program.cs.
   
    ```csharp
    namespace CoreSenderApp
    {
        using System;
        using System.Text;
        using System.Threading;
        using System.Threading.Tasks;
        using Microsoft.Azure.ServiceBus;

        class Program
        {
            // Connection String for the namespace can be obtained from the Azure portal under the 
            // 'Shared Access policies' section.
            const string ServiceBusConnectionString = "<your_connection_string>";
            const string QueueName = "<your_queue_name>";
            static IQueueClient queueClient;

            static void Main(string[] args)
            {
                MainAsync().GetAwaiter().GetResult();
            }

            static async Task MainAsync()
            {
                const int numberOfMessages = 10;
                queueClient = new QueueClient(ServiceBusConnectionString, QueueName);

                Console.WriteLine("======================================================");
                Console.WriteLine("Press ENTER key to exit after sending all the messages.");
                Console.WriteLine("======================================================");

                // Send Messages
                await SendMessagesAsync(numberOfMessages);
                        
                Console.ReadKey();

                await queueClient.CloseAsync();
            }

            static async Task SendMessagesAsync(int numberOfMessagesToSend)
            {
                try
                {
                    for (var i = 0; i < numberOfMessagesToSend; i++)
                    {
                        // Create a new message to send to the queue
                        string messageBody = $"Message {i}";
                        var message = new Message(Encoding.UTF8.GetBytes(messageBody));

                        // Write the body of the message to the console
                        Console.WriteLine($"Sending message: {messageBody}");

                        // Send the message to the queue
                        await queueClient.SendAsync(message);
                    }
                }
                catch (Exception exception)
                {
                    Console.WriteLine($"{DateTime.Now} :: Exception: {exception.Message}");
                }
            }
        }
    }
    ```

7. Ejecute el programa y compruebe Azure Portal: haga clic en el nombre de la cola en la hoja **Información general** del espacio de nombres. Se muestra la pantalla **Essentials** de la cola. Tenga en cuenta que el valor de **Recuento de mensajes activos** es ahora **10**. Cada vez que se ejecuta la aplicación de remitente sin recuperar los mensajes (como se describe en la siguiente sección), este valor aumenta en 10. Tenga en cuenta también que el tamaño actual de la cola aumenta el valor de **Actual** en la ventana **Essentials** cada vez que la aplicación agrega mensajes a la cola.
   
      ![Tamaño del mensaje][queue-message]

## <a name="receive-messages-from-the-queue"></a>Recepción de mensajes de la cola

Para recibir los mensajes que enviar, cree otra aplicación de consola .NET Core e instale el **Microsoft.Azure.ServiceBus** paquete de NuGet, similar a la aplicación de remitente anterior.

### <a name="write-code-to-receive-messages-from-the-queue"></a>Escritura de código para recibir mensajes de la cola

1. En Program.cs, agregue las siguientes instrucciones `using` en la parte superior de la definición del espacio de nombres, antes de la declaración de clase:
   
    ```csharp
    using System.Text;
    using System.Threading;
    using System.Threading.Tasks;
    using Microsoft.Azure.ServiceBus;
    ```

2. En la clase `Program`, declare las variables siguientes. Establezca la variable `ServiceBusConnectionString` en la cadena de conexión que obtuvo al crear el espacio de nombres y establezca `QueueName` en el nombre que usó cuando creó la cola:
   
    ```csharp
    const string ServiceBusConnectionString = "<your_connection_string>";
    const string QueueName = "<your_queue_name>";
    static IQueueClient queueClient;
    ```

3. Reemplace el contenido predeterminado de `Main()` con la siguiente línea de código:

    ```csharp
    MainAsync().GetAwaiter().GetResult();
    ```

4. Directamente después de `Main()`, agregue el siguiente método `MainAsync()` asincrónico que llama al método `RegisterOnMessageHandlerAndReceiveMessages()`:

    ```csharp
    static async Task MainAsync()
    {
        queueClient = new QueueClient(ServiceBusConnectionString, QueueName);

        Console.WriteLine("======================================================");
        Console.WriteLine("Press ENTER key to exit after receiving all the messages.");
        Console.WriteLine("======================================================");

        // Register the queue message handler and receive messages in a loop
        RegisterOnMessageHandlerAndReceiveMessages();

        Console.ReadKey();

        await queueClient.CloseAsync();
    }
    ```

5. Directamente después del método `MainAsync()`, agregue el método siguiente que registra el controlador de mensajes y recibe los mensajes enviados por la aplicación remitente:

    ```csharp
    static void RegisterOnMessageHandlerAndReceiveMessages()
    {
        // Configure the message handler options in terms of exception handling, number of concurrent messages to deliver, etc.
        var messageHandlerOptions = new MessageHandlerOptions(ExceptionReceivedHandler)
        {
            // Maximum number of concurrent calls to the callback ProcessMessagesAsync(), set to 1 for simplicity.
            // Set it according to how many messages the application wants to process in parallel.
            MaxConcurrentCalls = 1,

            // Indicates whether the message pump should automatically complete the messages after returning from user callback.
            // False below indicates the complete operation is handled by the user callback as in ProcessMessagesAsync().
            AutoComplete = false
        };

        // Register the function that processes messages.
        queueClient.RegisterMessageHandler(ProcessMessagesAsync, messageHandlerOptions);
    }
    ```

6. Directamente después del método anterior, agregue el siguiente método `ProcessMessagesAsync()` para procesar los mensajes recibidos:
 
    ```csharp
    static async Task ProcessMessagesAsync(Message message, CancellationToken token)
    {
        // Process the message.
        Console.WriteLine($"Received message: SequenceNumber:{message.SystemProperties.SequenceNumber} Body:{Encoding.UTF8.GetString(message.Body)}");

        // Complete the message so that it is not received again.
        // This can be done only if the queue Client is created in ReceiveMode.PeekLock mode (which is the default).
        await queueClient.CompleteAsync(message.SystemProperties.LockToken);

        // Note: Use the cancellationToken passed as necessary to determine if the queueClient has already been closed.
        // If queueClient has already been closed, you can choose to not call CompleteAsync() or AbandonAsync() etc.
        // to avoid unnecessary exceptions.
    }
    ```

7. Por último, agregue el siguiente método para controlar las excepciones que pueden producirse:
 
    ```csharp
    // Use this handler to examine the exceptions received on the message pump.
    static Task ExceptionReceivedHandler(ExceptionReceivedEventArgs exceptionReceivedEventArgs)
    {
        Console.WriteLine($"Message handler encountered an exception {exceptionReceivedEventArgs.Exception}.");
        var context = exceptionReceivedEventArgs.ExceptionReceivedContext;
        Console.WriteLine("Exception context for troubleshooting:");
        Console.WriteLine($"- Endpoint: {context.Endpoint}");
        Console.WriteLine($"- Entity Path: {context.EntityPath}");
        Console.WriteLine($"- Executing Action: {context.Action}");
        return Task.CompletedTask;
    }    
    ```    
   
8. Este es el aspecto que debería tener el archivo Program.cs:
   
    ```csharp
    namespace CoreReceiverApp
    {
        using System;
        using System.Text;
        using System.Threading;
        using System.Threading.Tasks;
        using Microsoft.Azure.ServiceBus;

        class Program
        {
            // Connection String for the namespace can be obtained from the Azure portal under the 
            // 'Shared Access policies' section.
            const string ServiceBusConnectionString = "<your_connection_string>";
            const string QueueName = "<your_queue_name>";
            static IQueueClient queueClient;

            static void Main(string[] args)
            {
                MainAsync().GetAwaiter().GetResult();
            }

            static async Task MainAsync()
            {
                queueClient = new QueueClient(ServiceBusConnectionString, QueueName);

                Console.WriteLine("======================================================");
                Console.WriteLine("Press ENTER key to exit after receiving all the messages.");
                Console.WriteLine("======================================================");

                // Register QueueClient's MessageHandler and receive messages in a loop
                RegisterOnMessageHandlerAndReceiveMessages();
                    
                Console.ReadKey();

                await queueClient.CloseAsync();
            }

            static void RegisterOnMessageHandlerAndReceiveMessages()
            {
                // Configure the MessageHandler Options in terms of exception handling, number of concurrent messages to deliver etc.
                var messageHandlerOptions = new MessageHandlerOptions(ExceptionReceivedHandler)
                {
                    // Maximum number of Concurrent calls to the callback `ProcessMessagesAsync`, set to 1 for simplicity.
                    // Set it according to how many messages the application wants to process in parallel.
                    MaxConcurrentCalls = 1,

                    // Indicates whether MessagePump should automatically complete the messages after returning from User Callback.
                    // False below indicates the Complete will be handled by the User Callback as in `ProcessMessagesAsync` below.
                    AutoComplete = false
                };

                // Register the function that will process messages
                queueClient.RegisterMessageHandler(ProcessMessagesAsync, messageHandlerOptions);
            }

            static async Task ProcessMessagesAsync(Message message, CancellationToken token)
            {
                // Process the message
                Console.WriteLine($"Received message: SequenceNumber:{message.SystemProperties.SequenceNumber} Body:{Encoding.UTF8.GetString(message.Body)}");

                // Complete the message so that it is not received again.
                // This can be done only if the queueClient is created in ReceiveMode.PeekLock mode (which is default).
                await queueClient.CompleteAsync(message.SystemProperties.LockToken);

                // Note: Use the cancellationToken passed as necessary to determine if the queueClient has already been closed.
                // If queueClient has already been Closed, you may chose to not call CompleteAsync() or AbandonAsync() etc. calls 
               // to avoid unnecessary exceptions.
            }

            static Task ExceptionReceivedHandler(ExceptionReceivedEventArgs exceptionReceivedEventArgs)
            {
                Console.WriteLine($"Message handler encountered an exception {exceptionReceivedEventArgs.Exception}.");
                var context = exceptionReceivedEventArgs.ExceptionReceivedContext;
                Console.WriteLine("Exception context for troubleshooting:");
                Console.WriteLine($"- Endpoint: {context.Endpoint}");
                Console.WriteLine($"- Entity Path: {context.EntityPath}");
                Console.WriteLine($"- Executing Action: {context.Action}");
                return Task.CompletedTask;
            }
        }
    }
    ```
4. Ejecute el programa y vuelva a comprobar el portal. Tenga en cuenta que los valores de **Recuento de mensajes activos** y **Actual** ahora son **0**.
   
    ![Longitud de la cola][queue-message-receive]

Felicidades. Ahora ha creado una cola, ha enviado un mensaje a dicha cola y ha recibido esos mensajes de la misma cola.

## <a name="next-steps"></a>Pasos siguientes

Consulte nuestro [repositorio de GitHub con ejemplos](https://github.com/Azure/azure-service-bus/tree/master/samples), donde se muestran algunas de las características más avanzadas de la mensajería de Service Bus.

<!--Image references-->

[nuget-pkg]: ./media/service-bus-dotnet-get-started-with-queues/nuget-package.png
[queue-message]: ./media/service-bus-dotnet-get-started-with-queues/queue-message.png
[queue-message-receive]: ./media/service-bus-dotnet-get-started-with-queues/queue-message-receive.png

