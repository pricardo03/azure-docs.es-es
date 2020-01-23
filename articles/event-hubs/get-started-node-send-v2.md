---
title: 'Envío y recepción de eventos mediante Node.js: Azure Event Hubs'
description: En este artículo se ofrece un tutorial para crear una aplicación de Node.js que envía eventos de Azure Event Hubs.
services: event-hubs
author: spelluru
ms.service: event-hubs
ms.workload: core
ms.topic: article
ms.date: 01/09/2020
ms.author: spelluru
ms.openlocfilehash: d4810c325acc42d5aa665002654cb01154cdc6bb
ms.sourcegitcommit: 3dc1a23a7570552f0d1cc2ffdfb915ea871e257c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/15/2020
ms.locfileid: "75981611"
---
# <a name="send-events-to-or-receive-events-from-azure-event-hubs-using-nodejs"></a>Envío o recepción de eventos en Event Hubs mediante Node.js

Azure Event Hubs es una plataforma de streaming de macrodatos y servicio de ingesta de eventos capaz de recibir y procesar millones de eventos por segundo. Event Hubs puede procesar y almacenar eventos, datos o telemetría generados por dispositivos y software distribuido. Los datos enviados a un centro de eventos se pueden transformar y almacenar con cualquier proveedor de análisis en tiempo real o adaptadores de procesamiento por lotes y almacenamiento. Para más información sobre Event Hubs, consulte [Introducción a Event Hubs](event-hubs-about.md) y [Características de Event Hubs](event-hubs-features.md).

En este tutorial se describe cómo crear aplicaciones de Node.js para enviar o recibir eventos en un centro de eventos.

> [!IMPORTANT]
> En este inicio rápido se usa la versión 5 del SDK del script de Java para Azure Event Hubs. Para ver un inicio rápido que usa la anterior versión 2 del SDK del script de Java, consulte [este artículo](event-hubs-node-get-started-send.md). Si usa la versión 2 del SDK, se recomienda migrar el código a la versión más reciente. Para más información, consulte la [guía de migración](https://github.com/Azure/azure-sdk-for-js/blob/master/sdk/eventhub/event-hubs/migrationguide.md).

## <a name="prerequisites"></a>Prerequisites

Para completar este tutorial, debe cumplir los siguientes requisitos previos:

- Una cuenta de Azure activa. Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) antes de empezar.
- Node.js versión 8.x y posteriores. Descargue la versión LTS más reciente en [https://nodejs.org](https://nodejs.org).
- Visual Studio Code (recomendado) o cualquier otro IDE
- **Creación de un espacio de nombres de Event Hubs y un centro de eventos**. El primer paso consiste en usar [Azure Portal](https://portal.azure.com) para crear un espacio de nombres de tipo Event Hubs y obtener las credenciales de administración que la aplicación necesita para comunicarse con el centro de eventos. Para crear un espacio de nombres y un centro de eventos, siga el procedimiento de [este artículo](event-hubs-create.md) y después continúe con los pasos siguientes de este tutorial. Después, obtenga la cadena de conexión para el espacio de nombres del centro de eventos. Para ello, siga las instrucciones del artículo [Obtenga la cadena de conexión](event-hubs-get-connection-string.md#get-connection-string-from-the-portal). Utilizará la cadena de conexión más adelante en el tutorial.


### <a name="install-npm-packages"></a>Instalación de paquetes de npm
Para instalar el [paquete de npm para Event Hubs](https://www.npmjs.com/package/@azure/event-hubs), abra un símbolo del sistema que tenga `npm` en la ruta de acceso, cambie el directorio a la carpeta en la que quiere tener los ejemplos y ejecute este comando.

```shell
npm install @azure/event-hubs
```

En el lado del receptor, tiene que instalar dos paquetes más. En este inicio rápido, va a usar Azure Blob Storage para conservar los puntos de control de forma que el programa no lea los eventos que ya ha leído. Establece un punto de comprobación de los metadatos de los mensajes recibidos a intervalos regulares en un blob. Este enfoque le permite seguir recibiendo más adelante mensajes desde donde lo dejó.

```shell
npm install @azure/storage-blob
```

```shell
npm install @azure/eventhubs-checkpointstore-blob
```

## <a name="send-events"></a>Envío de eventos

En esta sección se muestra cómo crear una aplicación de Node.js que envíe eventos a un centro de eventos.

1. Abra el editor que prefiera, como [Visual Studio Code](https://code.visualstudio.com).
2. Cree un archivo denominado `send.js` y pegue en él el código siguiente.

    ```javascript
    const { EventHubProducerClient } = require("@azure/event-hubs");

    const connectionString = "EVENT HUBS NAMESPACE CONNECTION STRING";
    const eventHubName = "EVENT HUB NAME";

    async function main() {

      // create a producer client to send messages to the event hub
      const producer = new EventHubProducerClient(connectionString, eventHubName);

      // prepare a batch of three events
      const batch = await producer.createBatch();
      batch.tryAdd({ body: "First event" });
      batch.tryAdd({ body: "Second event" });
      batch.tryAdd({ body: "Third event" });    

      // send the batch to the event hub
      await producer.sendBatch(batch);

      // close the producer client
      await producer.close();

      console.log("A batch of three events have been sent to the event hub");
    }

    main().catch((err) => {
      console.log("Error occurred: ", err);
    });
    ```
3. No olvide reemplazar los valores de **cadena de conexión** y **nombre del centro de eventos** en el código.
5. Ejecute el comando `node send.js` para ejecutar este archivo. Esto enviará un lote de tres eventos al centro de eventos.
6. En Azure Portal, puede comprobar que el centro de eventos ha recibido los mensajes. Cambie a la vista **Mensajes** en la sección **Métricas**. Actualice la página para actualizar el gráfico. Puede tardar unos segundos en mostrar que los mensajes se han recibido.

    [![Comprobación de que el centro de eventos ha recibido los mensajes](./media/getstarted-dotnet-standard-send-v2/verify-messages-portal.png)](./media/getstarted-dotnet-standard-send-v2/verify-messages-portal.png#lightbox)

    > [!NOTE]
    > Para ver el código fuente completo con comentarios muy útiles, consulte [este archivo en GitHub](https://github.com/Azure/azure-sdk-for-js/blob/master/sdk/eventhub/event-hubs/samples/javascript/sendEvents.js).

Felicidades. Ha enviado eventos a un centro de eventos.


## <a name="receive-events"></a>Recepción de eventos
En esta sección se muestra cómo recibir eventos de un centro de eventos mediante un almacén de puntos de comprobación de blob de Azure en una aplicación de Node.js. Establece un punto de comprobación de los metadatos de los mensajes recibidos a intervalos regulares en una instancia de Azure Storage Blob. Este enfoque le permite seguir recibiendo más adelante mensajes desde donde lo dejó.

### <a name="create-an-azure-storage-and-a-blob-container"></a>Creación de una instancia de Azure Storage y un contenedor de blobs de Azure Storage
Siga estos pasos para crear una cuenta de Azure Storage que incluya un contenedor de blobs.

1. [Creación de una cuenta de Azure Storage](../storage/common/storage-account-create.md?tabs=azure-portal)
2. [Creación de un contenedor de blobs](../storage/blobs/storage-quickstart-blobs-portal.md#create-a-container)
3. [Obtención de la cadena de conexión para una cuenta de almacenamiento](../storage/common/storage-configure-connection-string.md?#view-and-copy-a-connection-string)

    Anote la cadena de conexión y el nombre del contenedor. Los usará en el código de recepción.

### <a name="write-code-to-receive-events"></a>Escritura de código para recibir eventos

1. Abra el editor que prefiera, como [Visual Studio Code](https://code.visualstudio.com).
2. Cree un archivo denominado `receive.js` y pegue en él el código siguiente. Vea los comentarios de código para obtener más detalles.
    ```javascript
    const { EventHubConsumerClient } = require("@azure/event-hubs");
    const { ContainerClient } = require("@azure/storage-blob");    
    const { BlobCheckpointStore } = require("@azure/eventhubs-checkpointstore-blob");

    const connectionString = "EVENT HUBS NAMESPACE CONNECTION STRING";    
    const eventHubName = "EVENT HUB NAME";
    const consumerGroup = "$Default"; // name of the default consumer group
    const storageConnectionString = "AZURE STORAGE CONNECTION STRING";
    const containerName = "BLOB CONTAINER NAME";

    async function main() {
      // create a blob container client and a blob checkpoint store using the client
      const containerClient = new ContainerClient(storageConnectionString, containerName);
      const checkpointStore = new BlobCheckpointStore(containerClient);

      // create a consumer client for the event hub by specifying the checkpoint store
      const consumerClient = new EventHubConsumerClient(consumerGroup, connectionString, eventHubName, checkpointStore);

      // subscribe for the events and specify handlers for processing the events and errors.
      const subscription = consumerClient.subscribe({
          processEvents: async (events, context) => {
            for (const event of events) {
              console.log(`Received event: '${event.body}' from partition: '${context.partitionId}' and consumer group: '${context.consumerGroup}'`);
            }
            // update the checkpoint
            await context.updateCheckpoint(events[events.length - 1]);
          },

          processError: async (err, context) => {
            console.log(`Error : ${err}`);
          }
        }
      );

      // after 30 seconds, stop processing
      await new Promise((resolve) => {
        setTimeout(async () => {
          await subscription.close();
          await consumerClient.close();
          resolve();
        }, 30000);
      });
    }

    main().catch((err) => {
      console.log("Error occurred: ", err);
    });    
    ```
3. No olvide especificar los **valores siguientes** en el código:
    - Cadena de conexión al espacio de nombres de Event Hubs
    - Nombre del centro de eventos
    - Cadena de conexión a la cuenta de Azure Storage
    - Nombre del contenedor de blobs
5. Después, ejecute el comando `node receive.js` en un símbolo del sistema para ejecutar este archivo. Debería ver los mensajes sobre los eventos recibidos en la ventana.

    > [!NOTE]
    > Para ver el código fuente completo con comentarios muy útiles, consulte [este archivo en GitHub](https://github.com/Azure/azure-sdk-for-js/blob/master/sdk/eventhub/eventhubs-checkpointstore-blob/samples/receiveEventsUsingCheckpointStore.js).

Felicidades. Ha recibido eventos procedentes del centro de eventos. El programa receptor recibirá los eventos de todas las particiones del grupo de consumidores predeterminado en su centro de eventos.

## <a name="next-steps"></a>Pasos siguientes
Consulte estos ejemplos de código en GitHub:

- [Ejemplos de JavaScript](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/eventhub/event-hubs/samples/javascript)
- [Ejemplos de TypeScript](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/eventhub/event-hubs/samples/typescript)
