---
title: Envío de eventos a Azure Event Hubs mediante Node.js | Microsoft Docs
description: Comience a enviar eventos a Event Hubs mediante Node.js.
services: event-hubs
author: ShubhaVijayasarathy
manager: kamalb
ms.service: event-hubs
ms.workload: core
ms.topic: article
ms.date: 09/18/2018
ms.author: shvija
ms.openlocfilehash: 413f36a12dee135cc1a7dc99a34d8b7b2be6c46f
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2018
ms.locfileid: "48801070"
---
# <a name="send-events-to-azure-event-hubs-using-nodejs"></a>Envío de eventos a Azure Event Hubs mediante Node.js

Azure Event Hubs es un sistema de administración de eventos de alta escalabilidad que puede controlar millones de eventos por segundo, habilitando aplicaciones para procesar y analizar las grandes cantidades de datos generados por los dispositivos conectados y por otros sistemas. Una vez recopilados en un centro de eventos, puede recibir y controlar los eventos mediante controladores en proceso o mediante el reenvío a otros sistemas de análisis.

Para más información sobre Event Hubs, consulte [Información general de Event Hubs](event-hubs-about.md).

En este tutorial se describe cómo enviar eventos a un centro de eventos desde una aplicación escrita en Node.js. Para recibir eventos mediante el paquete del host del procesador de eventos de Node.js, consulte [el artículo de recepción correspondiente](event-hubs-node-get-started-receive.md).

El código de esta guía de inicio rápido está disponible en [GitHub](https://github.com/Azure/azure-event-hubs-node/tree/master/client). 

## <a name="prerequisites"></a>Requisitos previos

Para completar este tutorial, debe cumplir los siguientes requisitos previos:

- Node.js versión 8.x y posteriores. Descargue la versión LTS más reciente en [https://nodejs.org](https://nodejs.org).
- Una cuenta de Azure activa. Si no tiene una suscripción a Azure, cree una [cuenta gratuita][] antes de empezar.
- Visual Studio Code (recomendado) o cualquier otro IDE

## <a name="create-a-namespace-and-event-hub"></a>Creación de un espacio de nombres y un centro de eventos
El primer paso es usar Azure Portal para crear un espacio de nombres de Event Hubs con un centro de eventos. Si no tiene, puede crear estas entidades si sigue las instrucciones que aparecen en [Creación de un espacio de nombres de Event Hubs y un centro de eventos con Azure Portal](event-hubs-create.md).

## <a name="clone-the-sample-git-repository"></a>Clonación del repositorio de Git de ejemplo
Clone el repositorio de Git de ejemplo de [Github](https://github.com/Azure/azure-event-hubs-node) en la máquina. 

## <a name="install-nodejs-package"></a>Instalación del paquete de Node.js
Instale el paquete de Node.js para Azure Event Hubs en la máquina. 

```nodejs
npm install @azure/event-hubs
```

## <a name="clone-the-git-repository"></a>Clonación del repositorio de Git
Descargue o clone el [ejemplo](https://github.com/Azure/azure-event-hubs-node/tree/master/client/examples) desde GitHub. 

## <a name="send-events"></a>Envío de eventos
El SDK que clonó contiene varios ejemplos que muestran cómo enviar eventos a un centro de eventos mediante Node.js. En esta guía de inicio rápido, usará el ejemplo **simpleSender.js**. Para observar los eventos que se reciben, abra otro terminal y reciba los eventos con el [ejemplo de recepción](event-hubs-node-get-started-receive.md).

1. Abra el proyecto en Visual Studio Code. 
2. Cree un archivo denominado **.env** bajo la carpeta **client**. Copie y pegue las variables de entorno de ejemplo desde **sample.env** en la carpeta raíz.
3. Configure la cadena de conexión del centro de eventos, el nombre del centro de eventos y el punto de conexión de almacenamiento. Puede copiar la cadena de conexión para el centro de eventos desde la clave **Connection string-primary** en **RootManageSharedAccessKey** en la página del centro de eventos en Azure Portal. Para ver los pasos detallados, consulte [Obtención de la cadena de conexión](event-hubs-quickstart-portal.md#create-an-event-hubs-namespace).
4. En la CLI de Azure, vaya a la ruta de acceso de la carpeta **client**. Instale los paquetes de nodos y compile el proyecto mediante la ejecución de estos comandos:

    ```nodejs
    npm i
    npm run build
    ```
5. Inicie el envío de eventos mediante la ejecución del siguiente comando: 

    ```nodejs
    node dist/examples/simpleSender.js
    ```


## <a name="review-the-sample-code"></a>Revisión del código de ejemplo 
Este es el código de ejemplo para enviar eventos a un centro de eventos mediante Node.js. Puede crear manualmente un archivo sampleSender.js y ejecutarlo para enviar eventos a un centro de eventos. 


```nodejs
const { EventHubClient, EventPosition } = require('@azure/event-hubs');

const client = EventHubClient.createFromConnectionString(process.env["EVENTHUB_CONNECTION_STRING"], process.env["EVENTHUB_NAME"]);

async function main() {
    // NOTE: For receiving events from Azure Stream Analytics, please send Events to an EventHub where the body is a JSON object/array.
    // const eventData = { body: { "message": "Hello World" } };
    const data = { body: "Hello World 1" };
    const delivery = await client.send(data);
    console.log("message sent successfully.");
}

main().catch((err) => {
    console.log(err);
});

```

Recuerde establecer las variables de entorno antes de ejecutar el script. Puede configurar esto en la línea de comandos tal como se muestra en el ejemplo siguiente, o bien usar el [paquete dotenv](https://www.npmjs.com/package/dotenv#dotenv). 

```
// For windows
set EVENTHUB_CONNECTION_STRING="<your-connection-string>"
set EVENTHUB_NAME="<your-event-hub-name>"

// For linux or macos
export EVENTHUB_CONNECTION_STRING="<your-connection-string>"
export EVENTHUB_NAME="<your-event-hub-name>"
```

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre Event Hubs, consulte los siguientes artículos:

* [Recepción de eventos mediante Node.js](event-hubs-node-get-started-receive.md)
* [Ejemplos en GitHub](https://github.com/Azure/azure-event-hubs-node/tree/master/client/examples/)
* [Preguntas más frecuentes sobre Event Hubs](event-hubs-faq.md)

<!-- Links -->
[cuenta gratuita]: https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio
