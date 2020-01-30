---
title: 'Envío o recepción de eventos mediante Python: Azure Event Hubs | Microsoft Docs'
description: En este artículo se ofrece un tutorial para crear una aplicación de Python que envía eventos a Azure Event Hubs.
services: event-hubs
author: spelluru
ms.service: event-hubs
ms.workload: core
ms.topic: article
ms.date: 01/08/2020
ms.author: spelluru
ms.openlocfilehash: d7ab79d49aade7dd6e98cf33ce538174d176c784
ms.sourcegitcommit: af6847f555841e838f245ff92c38ae512261426a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/23/2020
ms.locfileid: "76705350"
---
# <a name="send-events-to-or-receive-events-from-event-hubs-using-python"></a>Envío o recepción de eventos en Event Hubs mediante Python

Azure Event Hubs es una plataforma de streaming de macrodatos y servicio de ingesta de eventos de gran escalabilidad capaz de recibir y procesar millones de eventos por segundo. Event Hubs puede procesar y almacenar eventos, datos o telemetría generados por dispositivos y software distribuido. Los datos enviados a un centro de eventos se pueden transformar y almacenar con cualquier proveedor de análisis en tiempo real o adaptadores de procesamiento por lotes y almacenamiento. Para más información sobre Event Hubs, consulte [Introducción a Event Hubs](event-hubs-about.md) y [Características de Event Hubs](event-hubs-features.md).

En este tutorial se describe cómo crear aplicaciones de Python para enviar o recibir eventos en un centro de eventos.

> [!IMPORTANT]
> En este inicio rápido se usa la versión 5 del SDK de Python para Azure Event Hubs. Para ver un inicio rápido que usa la versión 1 antigua del SDK de Python, consulte [este artículo](event-hubs-python-get-started-send.md). Si usa la versión 1 del SDK, se recomienda migrar el código a la versión más reciente. Para más información, consulte la [guía de migración](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/eventhub/azure-eventhub/migration_guide.md).


## <a name="prerequisites"></a>Prerequisites

Para completar este tutorial, debe cumplir los siguientes requisitos previos:

- Suscripción a Azure. Si no tiene una, [cree una cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.
- Un espacio de nombres de Event Hubs activo y un centro de eventos, creados siguiendo las instrucciones que se indican en [Inicio rápido: Creación de un centro de eventos mediante Azure Portal](event-hubs-create.md). Tome nota del espacio de nombres y los nombres del centro de eventos para usarlos más adelante en este tutorial.
- El nombre de la clave de acceso compartido y el valor de clave principal del espacio de nombres de Event Hubs. Obtenga el nombre y el valor de la clave de acceso; para ello, siga las instrucciones de [Obtención de la cadena de conexión del portal](event-hubs-get-connection-string.md#get-connection-string-from-the-portal). El nombre de la clave de acceso predeterminada es **RootManageSharedAccessKey**. Copie el nombre de clave y el valor de clave principal para usarlos más adelante en este tutorial.
- Python 2.7 y 3.5 o posterior, con `pip` instalado y actualizado.
- El paquete de Python para Event Hubs. Para instalar el paquete, ejecute este comando en un símbolo del sistema que tenga Python en su ruta de acceso:

    ```cmd
    pip install azure-eventhub
    ```

    Instale este paquete para recibir los eventos con Azure Blob Storage como almacén de puntos de control.

    ```cmd
    pip install azure-eventhub-checkpointstoreblob-aio
    ```

## <a name="send-events"></a>Envío de eventos
En esta sección, creará un script de Python para enviar eventos al centro de eventos que creó anteriormente.

1. Abra el editor de Python que prefiera, como [Visual Studio Code](https://code.visualstudio.com/).
2. Cree un script llamado **send.py**. Este script envía un lote de eventos al centro de eventos que creó anteriormente.
3. Pegue el código siguiente en sender.py. Vea los comentarios de código para obtener más detalles.

    ```python
    import asyncio
    from azure.eventhub.aio import EventHubProducerClient
    from azure.eventhub import EventData

    async def run():
        # create a producer client to send messages to the event hub
        # specify connection string to your event hubs namespace and
            # the event hub name
        producer = EventHubProducerClient.from_connection_string(conn_str="EVENT HUBS NAMESPACE - CONNECTION STRING", eventhub_name="EVENT HUB NAME")
        async with producer:
            # create a batch
            event_data_batch = await producer.create_batch()

            # add events to the batch
            event_data_batch.add(EventData('First event '))
            event_data_batch.add(EventData('Second event'))
            event_data_batch.add(EventData('Third event'))

            # send the batch of events to the event hub
            await producer.send_batch(event_data_batch)

    loop = asyncio.get_event_loop()
    loop.run_until_complete(run())

    ```

    > [!NOTE]
    > Para ver el código fuente completo con comentarios muy útiles, consulte [este archivo en GitHub](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/eventhub/azure-eventhub/samples/async_samples/send_async.py).

## <a name="receive-events"></a>Recepción de eventos
En este inicio rápido se usa un Azure Blob Storage como almacén de puntos de control. El almacén de puntos de control se usa para conservar los puntos de control (última posición de lectura).  

### <a name="create-an-azure-storage-and-a-blob-container"></a>Creación de una instancia de Azure Storage y un contenedor de blobs de Azure Storage
Siga estos pasos para crear una cuenta de Azure Storage que incluya un contenedor de blobs.

1. [Creación de una cuenta de Azure Storage](../storage/common/storage-account-create.md?tabs=azure-portal)
2. [Creación de un contenedor de blobs](../storage/blobs/storage-quickstart-blobs-portal.md#create-a-container)
3. [Obtención de la cadena de conexión para una cuenta de almacenamiento](../storage/common/storage-configure-connection-string.md?#view-and-copy-a-connection-string)

    Anote la cadena de conexión y el nombre del contenedor. Los usará en el código de recepción.


### <a name="create-python-script-to-receive-events"></a>Creación de un script de Python para recibir eventos

En esta sección, creará un script de Python para recibir eventos del centro de eventos:

1. Abra el editor de Python que prefiera, como [Visual Studio Code](https://code.visualstudio.com/).
2. Cree un script llamado **recv.py**.
3. Pegue el siguiente código en recv.py. Vea los comentarios de código para obtener más detalles.

    ```python
    import asyncio
    from azure.eventhub.aio import EventHubConsumerClient
    from azure.eventhub.extensions.checkpointstoreblobaio import BlobCheckpointStore


    async def on_event(partition_context, event):
        # print the event data
        print("Received the event: \"{}\" from the partition with ID: \"{}\"".format(event.body_as_str(encoding='UTF-8'), partition_context.partition_id))

        # update the checkpoint so that the program doesn't read the events
        # that it has already read when you run it next time
        await partition_context.update_checkpoint(event)

    async def main():
        # create an Azure blob checkpoint store to store the checkpoints
        checkpoint_store = BlobCheckpointStore.from_connection_string("AZURE STORAGE CONNECTION STRING", "BLOB CONTAINER NAME")

        # create a consumer client for the event hub
        client = EventHubConsumerClient.from_connection_string("EVENT HUBS NAMESPACE CONNECTION STRING", consumer_group="$Default", eventhub_name="EVENT HUB NAME", checkpoint_store=checkpoint_store)
        async with client:
            # call the receive method
            await client.receive(on_event=on_event)

    if __name__ == '__main__':
        loop = asyncio.get_event_loop()
        # run the main method
        loop.run_until_complete(main())    
    ```

    > [!NOTE]
    > Para ver el código fuente completo con comentarios muy útiles, consulte [este archivo en GitHub](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/eventhub/azure-eventhub/samples/async_samples/recv_with_checkpoint_store_async.py).


### <a name="run-the-receiver-app"></a>Ejecución de la aplicación del receptor

Para ejecutar el script, abra un símbolo del sistema que tenga Python en su ruta de acceso y, después, ejecute el siguiente comando:

```bash
python recv.py
```

### <a name="run-the-sender-app"></a>Ejecución de la aplicación del remitente

Para ejecutar el script, abra un símbolo del sistema que tenga Python en su ruta de acceso y, después, ejecute el siguiente comando:

```bash
python send.py
```

Debería ver los mensajes que se enviaron al centro de eventos en la ventana del receptor.


## <a name="next-steps"></a>Pasos siguientes
En este inicio rápido, ha enviado y recibido eventos de forma asincrónica. Para obtener información sobre cómo enviar y recibir eventos sincrónicamente, consulte los ejemplos de [esta ubicación](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/eventhub/azure-eventhub/samples/sync_samples).

Puede encontrar todos los ejemplos (sincrónicos y asincrónicos) en GitHub [aquí](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/eventhub/azure-eventhub/samples).
