---
title: Envío o recepción de eventos desde Azure Event Hubs mediante Python (anteriores)
description: En este tutorial se muestra cómo crear y ejecutar scripts de Python que envíen eventos a Azure Event Hubs, o los reciban de él, mediante el anterior paquete azure-eventhub versión 1.
services: event-hubs
author: spelluru
manager: femila
ms.service: event-hubs
ms.workload: core
ms.topic: quickstart
ms.date: 01/15/2020
ms.author: spelluru
ms.openlocfilehash: 654ccd6352dc0b671cc3becdafd2f1e1102dd39e
ms.sourcegitcommit: 67e9f4cc16f2cc6d8de99239b56cb87f3e9bff41
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/31/2020
ms.locfileid: "76902941"
---
# <a name="quickstart-send-and-receive-events-with-event-hubs-using-python-azure-eventhub-version-1"></a>Inicio rápido: Envío y recepción de eventos con Azure Event Hubs mediante Python (azure-eventhub versión 1)

Azure Event Hubs es una plataforma de streaming de macrodatos y servicio de ingesta de eventos capaz de recibir y procesar millones de eventos por segundo. Event Hubs puede procesar y almacenar los eventos, datos o telemetría generados por dispositivos y software distribuido. Los datos enviados a un centro de eventos se pueden transformar y almacenar con cualquier proveedor de análisis en tiempo real o adaptadores de procesamiento por lotes y almacenamiento. Para más información sobre Event Hubs, consulte [Azure Event Hubs](event-hubs-about.md) y [Características y terminología de Azure Event Hubs](event-hubs-features.md).

En este inicio rápido se muestra cómo crear aplicaciones de Python para enviar o recibir eventos en un centro de eventos. 

> [!WARNING]
> Este inicio rápido es para la versión 1 del SDK de Python para Azure Event Hubs. Se recomienda [migrar](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/eventhub/azure-eventhub/migration_guide.md) el código a la [versión 5 del SDK de Python](get-started-python-send-v2.md).

 
 

## <a name="prerequisites"></a>Prerequisites

Para completar este tutorial de inicio rápido, debe cumplir los siguientes requisitos previos:

- Suscripción a Azure. Si no tiene una, [cree una cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.
- Un espacio de nombres de Event Hubs activo y un centro de eventos, creados siguiendo las instrucciones que se indican en [Inicio rápido: Creación de un centro de eventos mediante Azure Portal](event-hubs-create.md). Tome nota del espacio de nombres y los nombres del centro de eventos para usarlos más adelante en este tutorial. 
- El nombre de la clave de acceso compartido y el valor de clave principal del espacio de nombres de Event Hubs. Obtenga el nombre y el valor de la clave de acceso; para ello, siga las instrucciones de [Obtención de la cadena de conexión del portal](event-hubs-get-connection-string.md#get-connection-string-from-the-portal). El nombre de la clave de acceso predeterminada es **RootManageSharedAccessKey**. Copie el nombre de clave y el valor de clave principal para usarlos más adelante en este tutorial. 
- Python 3.4 o posterior, con `pip` instalado y actualizado.
- El paquete de Python para Event Hubs. Para instalar el paquete, ejecute este comando en un símbolo del sistema que tenga Python en su ruta de acceso: 
  
  ```cmd
  pip install azure-eventhub==1.3.*
  ```


## <a name="send-events"></a>Envío de eventos

Para crear una aplicación de Python que envíe eventos a un centro de eventos:

> [!NOTE]
> En lugar de trabajar con el inicio rápido, puede descargar y ejecutar las [aplicaciones de ejemplo](https://github.com/Azure/azure-event-hubs-python/tree/master/examples) de GitHub. Reemplace las cadenas `EventHubConnectionString` y `EventHubName` por los valores del centro de eventos.

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

