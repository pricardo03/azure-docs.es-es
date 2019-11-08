---
title: 'Inicio rápido: Envío y recepción de eventos mediante Python en Azure Event Hubs'
description: 'Inicio rápido: En este tutorial se muestra cómo crear y ejecutar scripts de Python que envíen o reciban eventos de Azure Event Hubs.'
services: event-hubs
author: ShubhaVijayasarathy
manager: femila
ms.service: event-hubs
ms.workload: core
ms.topic: quickstart
ms.date: 11/05/2019
ms.author: shvija
ms.openlocfilehash: 9b6c3fb03f696f4142721284a14001eb51153a77
ms.sourcegitcommit: bc7725874a1502aa4c069fc1804f1f249f4fa5f7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/07/2019
ms.locfileid: "73720551"
---
# <a name="quickstart-send-and-receive-events-with-event-hubs-using-python"></a>Inicio rápido: Envío y recepción de eventos con Azure Event Hubs mediante Python

Azure Event Hubs es una plataforma de streaming de macrodatos y servicio de ingesta de eventos capaz de recibir y procesar millones de eventos por segundo. Event Hubs puede procesar y almacenar los eventos, datos o telemetría generados por dispositivos y software distribuido. Los datos enviados a un centro de eventos se pueden transformar y almacenar con cualquier proveedor de análisis en tiempo real o adaptadores de procesamiento por lotes y almacenamiento. Para más información sobre Event Hubs, consulte [Azure Event Hubs](event-hubs-about.md) y [Características y terminología de Azure Event Hubs](event-hubs-features.md).

En este inicio rápido se muestra cómo crear aplicaciones de Python para enviar o recibir eventos en un centro de eventos. 

> [!NOTE]
> En lugar de trabajar con el inicio rápido, puede descargar y ejecutar las [aplicaciones de ejemplo](https://github.com/Azure/azure-event-hubs-python/tree/master/examples) de GitHub. Reemplace las cadenas `EventHubConnectionString` y `EventHubName` por los valores del centro de eventos. 

## <a name="prerequisites"></a>Requisitos previos

Para completar este tutorial de inicio rápido, debe cumplir los siguientes requisitos previos:

- Una suscripción de Azure. Si no tiene una, [cree una cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.
- Un espacio de nombres de Event Hubs activo y un centro de eventos, creados siguiendo las instrucciones que se indican en [Inicio rápido: Creación de un centro de eventos mediante Azure Portal](event-hubs-create.md). Tome nota del espacio de nombres y los nombres del centro de eventos para usarlos más adelante en este tutorial. 
- El nombre de la clave de acceso compartido y el valor de clave principal del espacio de nombres de Event Hubs. Obtenga el nombre y el valor de la clave de acceso; para ello, siga las instrucciones de [Obtención de la cadena de conexión del portal](event-hubs-get-connection-string.md#get-connection-string-from-the-portal). El nombre de la clave de acceso predeterminada es **RootManageSharedAccessKey**. Copie el nombre de clave y el valor de clave principal para usarlos más adelante en este tutorial. 
- Python 3.4 o posterior, con `pip` instalado y actualizado.
- El paquete de Python para Event Hubs. Para instalar el paquete, ejecute este comando en un símbolo del sistema que tenga Python en su ruta de acceso: 
  
  ```cmd
  pip install azure-eventhub
  ```
  
  > [!NOTE]
  > El código de este inicio rápido usa la versión estable actual 1.3.1 del SDK de Event Hubs. Para el código de muestra que usa la versión preliminar del SDK, consulte [https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/eventhub/azure-eventhubs/examples](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/eventhub/azure-eventhubs/examples).

## <a name="send-events"></a>Envío de eventos

Para crear una aplicación de Python que envíe eventos a un centro de eventos:

1. Abra el editor de Python que prefiera, como [Visual Studio Code](https://code.visualstudio.com/).
2. Cree un nuevo archivo denominado *send.py*. Este script envía 100 eventos a un centro de eventos.
3. Pegue el código siguiente en *send.py*, reemplazando los valores \<namespace>, \<eventhub>, \<AccessKeyName> y \<valor de clave principal> de Event Hubs por sus valores: 
   
   ```python
   import sys
   import logging
   import datetime
   import time
   import os
   
   from azure.eventhub import EventHubClient, Sender, EventData
   
   logger = logging.getLogger("azure")
   
   # Address can be in either of these formats:
   # "amqps://<URL-encoded-SAS-policy>:<URL-encoded-SAS-key>@<namespace>.servicebus.windows.net/eventhub"
   # "amqps://<namespace>.servicebus.windows.net/<eventhub>"
   # SAS policy and key are not required if they are encoded in the URL
   
   ADDRESS = "amqps://<namespace>.servicebus.windows.net/<eventhub>"
   USER = "<AccessKeyName>"
   KEY = "<primary key value>"
   
   try:
       if not ADDRESS:
           raise ValueError("No EventHubs URL supplied.")
   
       # Create Event Hubs client
       client = EventHubClient(ADDRESS, debug=False, username=USER, password=KEY)
       sender = client.add_sender(partition="0")
       client.run()
       try:
           start_time = time.time()
           for i in range(100):
               print("Sending message: {}".format(i))
               message = "Message {}".format(i)
               sender.send(EventData(message))
       except:
           raise
       finally:
           end_time = time.time()
           client.stop()
           run_time = end_time - start_time
           logger.info("Runtime: {} seconds".format(run_time))
   
   except KeyboardInterrupt:
       pass
   ```
   
4. Guarde el archivo. 

Para ejecutar el script, desde el directorio en el que guardó *send.py*, ejecute este comando:

```cmd
start python send.py
```

Felicidades. Ha enviado mensajes a un centro de eventos.

## <a name="receive-events"></a>Recepción de eventos

Para crear una aplicación de Python que reciba eventos de un centro de eventos:

1. En el editor de Python, cree un archivo llamado *recv.py*.
2. Pegue el código siguiente en *recv.py*, reemplazando los valores \<namespace>, \<eventhub>, \<AccessKeyName> y \<valor de clave principal> de Event Hubs por sus valores: 
   
   ```python
   import os
   import sys
   import logging
   import time
   from azure.eventhub import EventHubClient, Receiver, Offset
   
   logger = logging.getLogger("azure")
   
   # Address can be in either of these formats:
   # "amqps://<URL-encoded-SAS-policy>:<URL-encoded-SAS-key>@<mynamespace>.servicebus.windows.net/myeventhub"
   # "amqps://<namespace>.servicebus.windows.net/<eventhub>"
   # SAS policy and key are not required if they are encoded in the URL
   
   ADDRESS = "amqps://<namespace>.servicebus.windows.net/<eventhub>"
   USER = "<AccessKeyName>"
   KEY = "<primary key value>"
   
   
   CONSUMER_GROUP = "$default"
   OFFSET = Offset("-1")
   PARTITION = "0"
   
   total = 0
   last_sn = -1
   last_offset = "-1"
   client = EventHubClient(ADDRESS, debug=False, username=USER, password=KEY)
   try:
       receiver = client.add_receiver(
           CONSUMER_GROUP, PARTITION, prefetch=5000, offset=OFFSET)
       client.run()
       start_time = time.time()
       for event_data in receiver.receive(timeout=100):
           print("Received: {}".format(event_data.body_as_str(encoding='UTF-8')))
           total += 1
   
       end_time = time.time()
       client.stop()
       run_time = end_time - start_time
       print("Received {} messages in {} seconds".format(total, run_time))
   
   except KeyboardInterrupt:
       pass
   finally:
       client.stop()
   ```
   
4. Guarde el archivo.

Para ejecutar el script, desde el directorio en el que guardó *recv.py*, ejecute este comando:

```cmd
start python recv.py
```

## <a name="next-steps"></a>Pasos siguientes
Para más información acerca de Event Hubs, consulte los siguientes artículos:

- [EventProcessorHost](event-hubs-event-processor-host.md)
- [Características y terminología de Azure Event Hubs](event-hubs-features.md)
- [Preguntas más frecuentes sobre Event Hubs](event-hubs-faq.md)

