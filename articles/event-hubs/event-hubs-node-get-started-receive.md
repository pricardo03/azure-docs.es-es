---
title: 'Recepción de eventos mediante Node.js: Azure Event Hubs | Microsoft Docs'
description: En este artículo se ofrece un tutorial para crear una aplicación de Node.js que recibe eventos de Azure Event Hubs.
services: event-hubs
author: ShubhaVijayasarathy
manager: kamalb
ms.service: event-hubs
ms.workload: core
ms.topic: article
ms.custom: seodec18
ms.date: 12/06/2018
ms.author: shvija
ms.openlocfilehash: d13f97ff7ec25fd638b92dc1c057afcaeecc2bf9
ms.sourcegitcommit: c2e61b62f218830dd9076d9abc1bbcb42180b3a8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/15/2018
ms.locfileid: "53434076"
---
# <a name="receive-events-from-azure-event-hubs-using-nodejs"></a>Recepción de eventos de Azure Event Hubs mediante Node.js

Azure Event Hubs es un sistema de administración de eventos de alta escalabilidad que puede controlar millones de eventos por segundo, habilitando aplicaciones para procesar y analizar las grandes cantidades de datos generados por los dispositivos conectados y por otros sistemas. Una vez recopilados en un centro de eventos, puede recibir y controlar los eventos mediante controladores en proceso o mediante el reenvío a otros sistemas de análisis. También puede capturar datos de eventos en Azure Storage o Azure Data Lake Storage antes de que se procesen.  

Para más información sobre Event Hubs, consulte la [información general de Event Hubs](event-hubs-about.md).

En este tutorial se muestra cómo recibir eventos de un centro de eventos mediante [EventProcessorHost](event-hubs-event-processor-host.md) de Azure en una aplicación de Node.js. EventProcessorHost (EPH) ayuda a recibir de manera eficaz eventos de un centro de eventos mediante la creación de receptores en todas las particiones del grupo de consumidores de un centro de eventos. Establece un punto de comprobación de los metadatos de los mensajes recibidos a intervalos regulares en una instancia de Azure Storage Blob. Este enfoque le permite seguir recibiendo más adelante mensajes desde donde lo dejó.

El código de esta guía de inicio rápido está disponible en [GitHub](https://github.com/Azure/azure-event-hubs-node/tree/master/processor).

> [!NOTE]
>  Para enviar eventos a Event Hubs con Node.js, vea este artículo: [Envío de eventos a Azure Event Hubs mediante Node.js](event-hubs-node-get-started-send.md). 

## <a name="prerequisites"></a>Requisitos previos

Para completar este tutorial, debe cumplir los siguientes requisitos previos:

- Node.js versión 8.x y posteriores. Descargue la versión LTS más reciente de [https://nodejs.org](https://nodejs.org). No use la versión LTS anterior de node.js. 
- Una cuenta de Azure activa. Si no tiene una suscripción a Azure, cree una [cuenta gratuita][] antes de empezar.

## <a name="create-a-namespace-and-event-hub"></a>Creación de un espacio de nombres y un centro de eventos
El primer paso es usar Azure Portal para crear un espacio de nombres de Event Hubs con un centro de eventos. Si no tiene, puede crear estas entidades si sigue las instrucciones que aparecen en [Creación de un espacio de nombres de Event Hubs y un centro de eventos con Azure Portal](event-hubs-create.md).

## <a name="create-a-storage-account-and-container"></a>Creación de una cuenta de almacenamiento y un contenedor
Para usar EventProcessorHost, debe tener una cuenta de Azure Storage. La información de estado, como las concesiones sobre particiones y puntos de comprobación del flujo de eventos se comparten entre receptores mediante un contenedor de Azure Storage. Puede crear una cuenta de Azure Storage con las instrucciones que aparecen en [este artículo](../storage/common/storage-quickstart-create-account.md).

## <a name="clone-the-git-repository"></a>Clonación del repositorio de Git
Descargue o clone el [ejemplo](https://github.com/Azure/azure-event-hubs-node/tree/master/processor/examples/) desde GitHub. 

## <a name="install-the-eventprocessorhost"></a>Instalación de EventProcessorHost
Instale EventProcessorHost para el módulo Event Hubs. 

```nodejs
npm install @azure/event-processor-host
```

## <a name="receive-events-using-eventprocessorhost"></a>Recepción de eventos mediante EventProcessorHost
El SDK que clonó contiene varios ejemplos que muestran cómo recibir eventos desde un centro de eventos mediante Node.js. En esta guía de inicio rápido, usará el ejemplo **singleEPH.js**. Para observar los eventos que se reciben, abra otro terminal y envíe eventos con el [ejemplo de envío](event-hubs-node-get-started-send.md).

1. Abra el proyecto en Visual Studio Code. 
2. Cree un archivo llamado **.env** en la carpeta **processor**. Copie y pegue las variables de entorno de ejemplo desde **sample.env** en la carpeta raíz.
3. Configure la cadena de conexión del centro de eventos, el nombre del centro de eventos y el punto de conexión de almacenamiento. Puede copiar la cadena de conexión para el centro de eventos desde la clave **Connection string-primary** en **RootManageSharedAccessKey** en la página del centro de eventos en Azure Portal. Para ver los pasos detallados, consulte [Obtención de la cadena de conexión](event-hubs-create.md#create-an-event-hubs-namespace).
4. En la CLI de Azure, vaya a la ruta de acceso a la carpeta **processor**. Instale los paquetes de nodos y compile el proyecto mediante la ejecución de estos comandos:

    ```nodejs
    npm i
    npm run build
    ```
5. Reciba eventos con el host del procesador de eventos mediante la ejecución de este comando:

    ```nodejs
    node dist/examples/singleEph.js
    ```

## <a name="review-the-sample-code"></a>Revisión del código de ejemplo 
Este es el código de ejemplo para recibir eventos desde un centro de eventos mediante Node.js. Puede crear manualmente un archivo sampleEph.js y ejecutarlo para recibir eventos de un centro de eventos. 

  ```nodejs
  const { EventProcessorHost, delay } = require("@azure/event-processor-host");

  const path = process.env.EVENTHUB_NAME;
  const storageCS = process.env.STORAGE_CONNECTION_STRING;
  const ehCS = process.env.EVENTHUB_CONNECTION_STRING;
  const storageContainerName = "test-container";
  
  async function main() {
    // Create the Event Processo Host
    const eph = EventProcessorHost.createFromConnectionString(
      EventProcessorHost.createHostName("my-host"),
      storageCS,
      storageContainerName,
      ehCS,
      {
        eventHubPath: path
      },
      onEphError: (error) => {
        console.log("This handler will notify you of any internal errors that happen " +
        "during partition and lease management: %O", error);
      }
    );
    let count = 0;
    // Message event handler
    const onMessage = async (context/*PartitionContext*/, data /*EventData*/) => {
      console.log(">>>>> Rx message from '%s': '%s'", context.partitionId, data.body);
      count++;
      // let us checkpoint every 100th message that is received across all the partitions.
      if (count % 100 === 0) {
        return await context.checkpoint();
      }
    };
    // Error event handler
    const onError = (error) => {
      console.log(">>>>> Received Error: %O", error);
    };
    // start the EPH
    await eph.start(onMessage, onError);
    // After some time let' say 2 minutes
    await delay(120000);
    // This will stop the EPH.
    await eph.stop();
  }
  
  main().catch((err) => {
    console.log(err);
  });
      
  ```

Recuerde establecer las variables de entorno antes de ejecutar el script. Puede configurar esto en la línea de comandos tal como se muestra en el ejemplo siguiente, o bien usar el [paquete de dotenv](https://www.npmjs.com/package/dotenv#dotenv). 

```
// For windows
set EVENTHUB_CONNECTION_STRING="<your-connection-string>"
set EVENTHUB_NAME="<your-event-hub-name>"

// For linux or macos
export EVENTHUB_CONNECTION_STRING="<your-connection-string>"
export EVENTHUB_NAME="<your-event-hub-name>"
```

Puede encontrar más ejemplos [aquí](https://github.com/Azure/azure-event-hubs-node/tree/master/processor/examples).


## <a name="next-steps"></a>Pasos siguientes

En esta guía de inicio rápido, ha creado la aplicación de Node.js que recibe mensajes desde un centro de eventos. Para aprender a enviar eventos a un centro de eventos mediante Node.js, consulte [Envío de eventos desde el centro de eventos con Node.js](event-hubs-node-get-started-send.md).

<!-- Links -->
[cuenta gratuita]: https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio
