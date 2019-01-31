---
title: 'Recepción de eventos mediante Java: Azure Event Hubs | Microsoft Docs'
description: En este artículo se ofrece un tutorial para crear una aplicación de Java que recibe eventos de Azure Event Hubs.
services: event-hubs
author: ShubhaVijayasarathy
manager: timlt
ms.service: event-hubs
ms.workload: core
ms.topic: article
ms.custom: seodec18
ms.date: 12/06/2018
ms.author: shvija
ms.openlocfilehash: 0fc9b8b6a8bcd62aafda7c04697ab8b9c096b17e
ms.sourcegitcommit: a7331d0cc53805a7d3170c4368862cad0d4f3144
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2019
ms.locfileid: "55296586"
---
# <a name="receive-events-from-azure-event-hubs-using-java"></a>Recepción de eventos desde Azure Event Hubs mediante Java

Event Hubs es un sistema de recopilación de alta escalabilidad que puede recibir millones de eventos por segundo, habilitando una aplicación para procesar y analizar las grandes cantidades de datos generados por las aplicaciones y los dispositivos conectados. Una vez recopilados en Event Hubs, puede transformar y almacenar los datos usando cualquier proveedor de análisis en tiempo real o clúster de almacenamiento.

Para más información, consulte [Información general de Event Hubs][Event Hubs overview].

Este tutorial muestra cómo recibir eventos en un centro de eventos mediante una aplicación de consola escrita en Java.

## <a name="prerequisites"></a>Requisitos previos

Para completar este tutorial, debe cumplir los siguientes requisitos previos:

* Un entorno de desarrollo de Java. En este tutorial, se da por hecho que se va a trabajar con [Eclipse](https://www.eclipse.org/).
* Una cuenta de Azure activa. Si no tiene una suscripción a Azure, cree una [cuenta gratuita][] antes de empezar.

El código de este tutorial se basa en el [código EventProcessorSample en GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/Java/Basic/EventProcessorSample), que se puede examinar para ver toda la aplicación en funcionamiento.

## <a name="receive-messages-with-eventprocessorhost-in-java"></a>Recepción de mensajes con EventProcessorHost en Java

**EventProcessorHost** es una clase de Java que simplifica la recepción de eventos desde Event Hubs mediante la administración de puntos de control persistentes y recepciones paralelas desde tales Centros de eventos. Con EventProcessorHost, puede dividir eventos entre varios destinatarios, incluso cuando están hospedados en distintos nodos. Este ejemplo muestra cómo usar EventProcessorHost para un solo destinatario.

### <a name="create-a-storage-account"></a>Crear una cuenta de almacenamiento

Para usar EventProcessorHost, debe tener una [cuenta de Azure Storage][Azure Storage account]:

1. Inicie sesión en [Azure Portal][Azure portal] y haga clic en **+ Crear un recurso** en la parte izquierda de la pantalla.
2. Haga clic en **Storage** y luego en **Cuenta de Storage**. En la ventana **Crear cuenta de almacenamiento**, escriba un nombre para la cuenta de almacenamiento. Complete el resto de los campos, seleccione la región que desee y, finalmente, haga clic en **Crear**.
   
    ![Crear cuenta de almacenamiento](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-storage2.png)

3. Haga clic en la cuenta de almacenamiento recién creada y, a continuación, en **Claves de acceso**:
   
    ![Obtención de las claves de acceso](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-storage3.png)

    Copie el valor key1 en una ubicación temporal. Lo usará más adelante en este tutorial.

### <a name="create-a-java-project-using-the-eventprocessor-host"></a>Creación de un proyecto de Java mediante EventProcessorHost

La biblioteca de cliente de Java para Event Hubs está disponible para su uso en proyectos de Maven en el [repositorio central de Maven][Maven Package], y se puede hacer referencia a ella mediante la siguiente declaración de dependencia en el archivo de proyecto de Maven: 

```xml
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-eventhubs</artifactId>
    <version>2.2.0</version>
</dependency>
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-eventhubs-eph</artifactId>
    <version>2.4.0</version>
</dependency>
```

Para distintos tipos de entornos de compilación, puede obtener explícitamente los últimos archivos JAR publicados del [repositorio central de Maven][Maven Package].  

1. Para el ejemplo siguiente, primero cree un nuevo proyecto de Maven para una aplicación de consola o shell en su entorno de desarrollo de Java favorito. La clase se denomina `ErrorNotificationHandler`.     
   
    ```java
    import java.util.function.Consumer;
    import com.microsoft.azure.eventprocessorhost.ExceptionReceivedEventArgs;
   
    public class ErrorNotificationHandler implements Consumer<ExceptionReceivedEventArgs>
    {
        @Override
        public void accept(ExceptionReceivedEventArgs t)
        {
            System.out.println("SAMPLE: Host " + t.getHostname() + " received general error notification during " + t.getAction() + ": " + t.getException().toString());
        }
    }
    ```
2. Cree una clase nueva denominada `EventProcessorSample`con el código siguiente. Reemplace los siguientes marcadores de posición por los valores que usó al crear el centro de eventos y la cuenta de almacenamiento:
   
   ```java
   package com.microsoft.azure.eventhubs.samples.eventprocessorsample;

   import com.microsoft.azure.eventhubs.ConnectionStringBuilder;
   import com.microsoft.azure.eventhubs.EventData;
   import com.microsoft.azure.eventprocessorhost.CloseReason;
   import com.microsoft.azure.eventprocessorhost.EventProcessorHost;
   import com.microsoft.azure.eventprocessorhost.EventProcessorOptions;
   import com.microsoft.azure.eventprocessorhost.ExceptionReceivedEventArgs;
   import com.microsoft.azure.eventprocessorhost.IEventProcessor;
   import com.microsoft.azure.eventprocessorhost.PartitionContext;

   import java.util.concurrent.ExecutionException;
   import java.util.function.Consumer;

   public class EventProcessorSample
   {
       public static void main(String args[]) throws InterruptedException, ExecutionException
       {
           String consumerGroupName = "$Default";
           String namespaceName = "----NamespaceName----";
           String eventHubName = "----EventHubName----";
           String sasKeyName = "----SharedAccessSignatureKeyName----";
           String sasKey = "----SharedAccessSignatureKey----";
           String storageConnectionString = "----AzureStorageConnectionString----";
           String storageContainerName = "----StorageContainerName----";
           String hostNamePrefix = "----HostNamePrefix----";
        
           ConnectionStringBuilder eventHubConnectionString = new ConnectionStringBuilder()
                .setNamespaceName(namespaceName)
                .setEventHubName(eventHubName)
                .setSasKeyName(sasKeyName)
                .setSasKey(sasKey);
        
           EventProcessorHost host = new EventProcessorHost(
                EventProcessorHost.createHostName(hostNamePrefix),
                eventHubName,
                consumerGroupName,
                eventHubConnectionString.toString(),
                storageConnectionString,
                storageContainerName);
        
           System.out.println("Registering host named " + host.getHostName());
           EventProcessorOptions options = new EventProcessorOptions();
           options.setExceptionNotification(new ErrorNotificationHandler());

           host.registerEventProcessor(EventProcessor.class, options)
           .whenComplete((unused, e) ->
           {
               if (e != null)
               {
                   System.out.println("Failure while registering: " + e.toString());
                   if (e.getCause() != null)
                   {
                       System.out.println("Inner exception: " + e.getCause().toString());
                   }
               }
           })
           .thenAccept((unused) ->
           {
               System.out.println("Press enter to stop.");
               try 
               {
                   System.in.read();
               }
               catch (Exception e)
               {
                   System.out.println("Keyboard read failed: " + e.toString());
               }
           })
           .thenCompose((unused) ->
           {
               return host.unregisterEventProcessor();
           })
           .exceptionally((e) ->
           {
               System.out.println("Failure while unregistering: " + e.toString());
               if (e.getCause() != null)
               {
                   System.out.println("Inner exception: " + e.getCause().toString());
               }
               return null;
           })
           .get(); // Wait for everything to finish before exiting main!
        
           System.out.println("End of sample");
       }
    ```
3. Cree una clase final más llamada `EventProcessor`, con el código siguiente:
   
    ```java
    public static class EventProcessor implements IEventProcessor
    {
        private int checkpointBatchingCount = 0;

        // OnOpen is called when a new event processor instance is created by the host. 
        @Override
        public void onOpen(PartitionContext context) throws Exception
        {
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " is opening");
        }

        // OnClose is called when an event processor instance is being shut down. 
        @Override
        public void onClose(PartitionContext context, CloseReason reason) throws Exception
        {
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " is closing for reason " + reason.toString());
        }
        
        // onError is called when an error occurs in EventProcessorHost code that is tied to this partition, such as a receiver failure.
        @Override
        public void onError(PartitionContext context, Throwable error)
        {
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " onError: " + error.toString());
        }

        // onEvents is called when events are received on this partition of the Event Hub. 
        @Override
        public void onEvents(PartitionContext context, Iterable<EventData> events) throws Exception
        {
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " got event batch");
            int eventCount = 0;
            for (EventData data : events)
            {
                try
                {
                    System.out.println("SAMPLE (" + context.getPartitionId() + "," + data.getSystemProperties().getOffset() + "," +
                            data.getSystemProperties().getSequenceNumber() + "): " + new String(data.getBytes(), "UTF8"));
                    eventCount++;
                    
                    // Checkpointing persists the current position in the event stream for this partition and means that the next
                    // time any host opens an event processor on this event hub+consumer group+partition combination, it will start
                    // receiving at the event after this one. 
                    this.checkpointBatchingCount++;
                    if ((checkpointBatchingCount % 5) == 0)
                    {
                        System.out.println("SAMPLE: Partition " + context.getPartitionId() + " checkpointing at " +
                            data.getSystemProperties().getOffset() + "," + data.getSystemProperties().getSequenceNumber());
                        // Checkpoints are created asynchronously. It is important to wait for the result of checkpointing
                        // before exiting onEvents or before creating the next checkpoint, to detect errors and to ensure proper ordering.
                        context.checkpoint(data).get();
                    }
                }
                catch (Exception e)
                {
                    System.out.println("Processing failed for an event: " + e.toString());
                }
            }
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " batch size was " + eventCount + " for host " + context.getOwner());
        }
    }
    ```

Este tutorial usa una sola instancia de EventProcessorHost. Para aumentar la capacidad de procesamiento, se recomienda ejecutar varias instancias de EventProcessorHost, preferiblemente en máquinas independientes.  Esto proporciona redundancia también. En esos casos, las diferentes instancias se coordinan automáticamente entre sí con el fin de equilibrar la carga de los eventos recibidos. Si desea que varios destinatarios procesen *todos* los eventos, debe usar el concepto **ConsumerGroup** . Cuando se reciben eventos de distintos equipos, puede ser útil especificar nombres para las instancias de EventProcessorHost según los equipos (o roles) en que se implementan.

## <a name="publishing-messages-to-eventhub"></a>Publicación de mensajes en EventHub

Antes de que los consumidores puedan recuperar los mensajes, los editores tienen que publicarlos primero en las particiones. Merece la pena destacar que cuando los mensajes se publican en el centro de eventos de manera sincrónica con el método sendSync() en el objeto com.microsoft.azure.eventhubs.EventHubClient, se podrían enviar a una partición específica o distribuirse a todas las particiones disponibles en modo round-robin, según si se especifica o no la clave de partición.

Cuando se especifica una cadena que representa la clave de partición, se aplicará un algoritmo hash a la clave para determinar la partición a la que desea enviar el evento.

Cuando no se establece la clave de partición, los mensajes se envían en modo round-robin a todas las particiones disponibles

```java
// Serialize the event into bytes
byte[] payloadBytes = gson.toJson(messagePayload).getBytes(Charset.defaultCharset());

// Use the bytes to construct an {@link EventData} object
EventData sendEvent = EventData.create(payloadBytes);

// Transmits the event to event hub without a partition key
// If a partition key is not set, then we will round-robin to all topic partitions
eventHubClient.sendSync(sendEvent);

//  the partitionKey will be hash'ed to determine the partitionId to send the eventData to.
eventHubClient.sendSync(sendEvent, partitionKey);

```

## <a name="implementing-a-custom-checkpointmanager-for-eventprocessorhost-eph"></a>Implementación del administrador de puntos de control personalizado para EventProcessorHost (EPH)

La API proporciona un mecanismo para implementar el administrador de puntos de control personalizado en escenarios donde la implementación predeterminada no es compatible con su caso de uso.

El administrador de puntos de control predeterminado usa el almacenamiento de blobs, pero si reemplaza el administrador de puntos de control que usa EPH por su propia implementación, puede usar cualquier almacén que quiera para respaldar la implementación del administrador de puntos de control.

Cree una clase que implemente la interfaz com.microsoft.azure.eventprocessorhost.ICheckpointManager.

Use la implementación personalizada del administrador de puntos de control (com.microsoft.azure.eventprocessorhost.ICheckpointManager).

Dentro de su implementación, puede reemplazar el mecanismo de puntos de control predeterminado e implementar nuestros propios puntos de control en función de su propio almacén de datos (SQL Server, CosmosDB, Azure Cache for Redis, etc.). Se recomienda que el almacén usado para respaldar la implementación del administrador de puntos de control sea accesible para todas las instancias de EPH que van a procesar eventos para el grupo de consumidores.

Puede usar cualquier almacén de datos que esté disponible en su entorno.

La clase com.microsoft.azure.eventprocessorhost.EventProcessorHost proporciona dos constructores que le permiten reemplazar el administrador de puntos de control por EventProcessorHost.

## <a name="next-steps"></a>Pasos siguientes
En esta guía de inicio rápido, ha creado la aplicación de Java que recibe mensajes desde un centro de eventos. Para obtener información acerca de cómo enviar eventos a un centro de eventos mediante Java, consulte [Envío de eventos desde el centro de eventos con Java](event-hubs-java-get-started-send.md).

<!-- Links -->
[Event Hubs overview]: event-hubs-what-is-event-hubs.md
[Azure Storage account]: ../storage/common/storage-create-storage-account.md
[Azure portal]: https://portal.azure.com
[Maven Package]: https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs-eph%22

<!-- Images -->
[11]: ./media/service-bus-event-hubs-get-started-receive-ephjava/create-eph-csharp2.png
[12]: ./media/service-bus-event-hubs-get-started-receive-ephjava/create-eph-csharp3.png
[cuenta gratuita]: https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio
