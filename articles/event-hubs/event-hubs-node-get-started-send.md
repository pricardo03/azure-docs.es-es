---
title: Enviar y recibir eventos mediante Node.js - Azure Event Hubs | Microsoft Docs
description: En este artículo se ofrece un tutorial para crear una aplicación de Node.js que envía eventos de Azure Event Hubs.
services: event-hubs
author: spelluru
manager: kamalb
ms.service: event-hubs
ms.workload: core
ms.topic: article
ms.custom: seodec18
ms.date: 04/15/2019
ms.author: spelluru
ms.openlocfilehash: f03bfde8f7ea37989756ad47678369e94b831438
ms.sourcegitcommit: c3d1aa5a1d922c172654b50a6a5c8b2a6c71aa91
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59677908"
---
# <a name="send-events-to-or-receive-events-from-azure-event-hubs-using-nodejs"></a>Enviar eventos a o recibir eventos desde Azure Event Hubs con Node.js

Azure Event Hubs es una plataforma de streaming de macrodatos y servicio de ingesta de eventos capaz de recibir y procesar millones de eventos por segundo. Event Hubs puede procesar y almacenar eventos, datos o telemetría generados por dispositivos y software distribuido. Los datos enviados a un centro de eventos se pueden transformar y almacenar con cualquier proveedor de análisis en tiempo real o adaptadores de procesamiento por lotes y almacenamiento. Para más información sobre Event Hubs, consulte [Introducción a Event Hubs](event-hubs-about.md) y [Características de Event Hubs](event-hubs-features.md).

Este tutorial describe cómo crear aplicaciones de Node.js para enviar eventos a o recibir eventos desde un centro de eventos.

> [!NOTE]
> Puede descargar esta guía de inicio rápido como un ejemplo desde [GitHub](https://github.com/Azure/azure-event-hubs-node/tree/master/client), reemplazar las cadenas `EventHubConnectionString` y `EventHubName` por los valores del centro de eventos, y ejecutarlo. También puede seguir los pasos de este tutorial para crear el suyo propio.

## <a name="prerequisites"></a>Requisitos previos

Para completar este tutorial, debe cumplir los siguientes requisitos previos:

- Una cuenta de Azure activa. Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) antes de empezar.
- Node.js versión 8.x y posteriores. Descargue la versión LTS más reciente en [https://nodejs.org](https://nodejs.org).
- Visual Studio Code (recomendado) o cualquier otro IDE
- **Crear un espacio de nombres de Event Hubs y un centro de eventos**. El primer paso consiste en usar [Azure Portal](https://portal.azure.com) para crear un espacio de nombres de tipo Event Hubs y obtener las credenciales de administración que la aplicación necesita para comunicarse con el centro de eventos. Para crear un espacio de nombres y un centro de eventos, siga el procedimiento de [este artículo](event-hubs-create.md) y después continúe con los pasos siguientes de este tutorial. A continuación, obtenga la cadena de conexión para el espacio de nombres del centro de eventos siguiendo las instrucciones del artículo: [Obtenga la cadena de conexión](event-hubs-get-connection-string.md#get-connection-string-from-the-portal). Utilizará la cadena de conexión más adelante en el tutorial.
- Clone el [repositorio de GitHub de ejemplo](https://github.com/Azure/azure-event-hubs-node) en su equipo. 


## <a name="send-events"></a>Envío de eventos
En esta sección se muestra cómo crear una aplicación de Node.js que envía eventos a un centro de eventos. 

### <a name="install-nodejs-package"></a>Instalación del paquete de Node.js
Instale el paquete de Node.js para Azure Event Hubs en la máquina. 

```shell
npm install @azure/event-hubs
```

Si aún no ha clonado el repositorio de Git como se mencionó en los requisitos previos, descargue el [ejemplo](https://github.com/Azure/azure-event-hubs-node/tree/master/client/examples) desde GitHub. 

El SDK que clonó contiene varios ejemplos que muestran cómo enviar eventos a un centro de eventos mediante Node.js. En esta guía de inicio rápido, usará el ejemplo **simpleSender.js**. Para observar los eventos que se reciben, abra otro terminal y reciba los eventos con el [ejemplo de recepción](event-hubs-node-get-started-receive.md).

1. Abra el proyecto en Visual Studio Code. 
2. Cree un archivo denominado **.env** bajo la carpeta **client**. Copie y pegue las variables de entorno de ejemplo desde **sample.env** en la carpeta raíz.
3. Configure la cadena de conexión del centro de eventos, el nombre del centro de eventos y el punto de conexión de almacenamiento. Para obtener instrucciones sobre cómo obtener una cadena de conexión para un centro de eventos, consulte [cómo obtener la cadena de conexión](event-hubs-create.md#create-an-event-hubs-namespace).
4. En la CLI de Azure, vaya a la ruta de acceso de la carpeta **client**. Instale los paquetes de nodos y compile el proyecto mediante la ejecución de estos comandos:

    ```shell
    npm i
    npm run build
    ```
5. Inicie el envío de eventos mediante la ejecución del siguiente comando: 

    ```shell
    node dist/examples/simpleSender.js
    ```


### <a name="review-the-sample-code"></a>Revisión del código de ejemplo 
Revise el código de ejemplo en el archivo simpleSender.js para enviar eventos a un centro de eventos.

```javascript
"use strict";
Object.defineProperty(exports, "__esModule", { value: true });
const lib_1 = require("../lib");
const dotenv = require("dotenv");
dotenv.config();
const connectionString = "EVENTHUB_CONNECTION_STRING";
const entityPath = "EVENTHUB_NAME";
const str = process.env[connectionString] || "";
const path = process.env[entityPath] || "";

async function main() {
    const client = lib_1.EventHubClient.createFromConnectionString(str, path);
    const data = {
        body: "Hello World!!"
    };
    const delivery = await client.send(data);
    console.log(">>> Sent the message successfully: ", delivery.tag.toString());
    console.log(delivery);
    console.log("Calling rhea-promise sender close directly. This should result in sender getting reconnected.");
    await Object.values(client._context.senders)[0]._sender.close();
    // await client.close();
}

main().catch((err) => {
    console.log("error: ", err);
});

```

Recuerde establecer las variables de entorno antes de ejecutar el script. Puede configurarlas en la línea de comandos, tal como se muestra en el ejemplo siguiente, o bien usar el [paquete dotenv](https://www.npmjs.com/package/dotenv#dotenv). 

```shell
// For windows
set EVENTHUB_CONNECTION_STRING="<your-connection-string>"
set EVENTHUB_NAME="<your-event-hub-name>"

// For linux or macos
export EVENTHUB_CONNECTION_STRING="<your-connection-string>"
export EVENTHUB_NAME="<your-event-hub-name>"
```

## <a name="receive-events"></a>Recepción de eventos
En este tutorial se muestra cómo recibir eventos de un centro de eventos mediante [EventProcessorHost](event-hubs-event-processor-host.md) de Azure en una aplicación de Node.js. EventProcessorHost (EPH) ayuda a recibir de manera eficaz eventos de un centro de eventos mediante la creación de receptores en todas las particiones del grupo de consumidores de un centro de eventos. Establece un punto de comprobación de los metadatos de los mensajes recibidos a intervalos regulares en una instancia de Azure Storage Blob. Este enfoque le permite seguir recibiendo más adelante mensajes desde donde lo dejó.

El código de esta guía de inicio rápido está disponible en [GitHub](https://github.com/Azure/azure-event-hubs-node/tree/master/processor).

### <a name="clone-the-git-repository"></a>Clonación del repositorio de Git
Descargue o clone el [ejemplo](https://github.com/Azure/azure-event-hubs-node/tree/master/processor/examples/) desde GitHub. 

### <a name="install-the-eventprocessorhost"></a>Instalación de EventProcessorHost
Instale EventProcessorHost para el módulo Event Hubs. 

```shell
npm install @azure/event-processor-host
```

### <a name="receive-events-using-eventprocessorhost"></a>Recepción de eventos mediante EventProcessorHost
El SDK que clonó contiene varios ejemplos que muestran cómo recibir eventos desde un centro de eventos mediante Node.js. En esta guía de inicio rápido, usará el ejemplo **singleEPH.js**. Para observar los eventos que se reciben, abra otro terminal y envíe eventos con el [ejemplo de envío](event-hubs-node-get-started-send.md).

1. Abra el proyecto en Visual Studio Code. 
2. Cree un archivo llamado **.env** en la carpeta **processor**. Copie y pegue las variables de entorno de ejemplo desde **sample.env** en la carpeta raíz.
3. Configure la cadena de conexión del centro de eventos, el nombre del centro de eventos y el punto de conexión de almacenamiento. Puede copiar la cadena de conexión para el centro de eventos desde la clave **Connection string-primary** en **RootManageSharedAccessKey** en la página del centro de eventos en Azure Portal. Para ver los pasos detallados, consulte [Obtención de la cadena de conexión](event-hubs-create.md#create-an-event-hubs-namespace).
4. En la CLI de Azure, vaya a la ruta de acceso a la carpeta **processor**. Instale los paquetes de nodos y compile el proyecto mediante la ejecución de estos comandos:

    ```shell
    npm i
    npm run build
    ```
5. Reciba eventos con el host del procesador de eventos mediante la ejecución de este comando:

    ```shell
    node dist/examples/singleEph.js
    ```

### <a name="review-the-sample-code"></a>Revisión del código de ejemplo 
Este es el código de ejemplo para recibir eventos desde un centro de eventos mediante Node.js. Puede crear manualmente un archivo sampleEph.js y ejecutarlo para recibir eventos de un centro de eventos. 

  ```javascript
  const { EventProcessorHost, delay } = require("@azure/event-processor-host");

  const path = process.env.EVENTHUB_NAME;
  const storageCS = process.env.STORAGE_CONNECTION_STRING;
  const ehCS = process.env.EVENTHUB_CONNECTION_STRING;
  const storageContainerName = "test-container";
  
  async function main() {
    // Create the Event Processor Host
    const eph = EventProcessorHost.createFromConnectionString(
      EventProcessorHost.createHostName("my-host"),
      storageCS,
      storageContainerName,
      ehCS,
      {
        eventHubPath: path,
        onEphError: (error) => {
          console.log("This handler will notify you of any internal errors that happen " +
          "during partition and lease management: %O", error);
        }
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

```shell
// For windows
set EVENTHUB_CONNECTION_STRING="<your-connection-string>"
set EVENTHUB_NAME="<your-event-hub-name>"

// For linux or macos
export EVENTHUB_CONNECTION_STRING="<your-connection-string>"
export EVENTHUB_NAME="<your-event-hub-name>"
```

Puede encontrar más ejemplos [aquí](https://github.com/Azure/azure-event-hubs-node/tree/master/processor/examples).


## <a name="next-steps"></a>Pasos siguientes
Lea los siguientes artículos:

- [EventProcessorHost](event-hubs-event-processor-host.md)
- [Características y la terminología de Azure Event Hubs](event-hubs-features.md)
- [Preguntas más frecuentes sobre Event Hubs](event-hubs-faq.md)
- Consulte otros ejemplos de Node.js para Event Hubs en [GitHub](https://github.com/Azure/azure-event-hubs-node/tree/master/client/examples/).
