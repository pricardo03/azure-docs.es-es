---
title: 'Envío o recepción de eventos mediante Python: Azure Event Hubs | Microsoft Docs'
description: En este artículo se ofrece un tutorial para crear una aplicación de Python que envía eventos a Azure Event Hubs.
services: event-hubs
author: ShubhaVijayasarathy
manager: femila
ms.service: event-hubs
ms.workload: core
ms.topic: article
ms.date: 04/15/2019
ms.author: shvija
ms.openlocfilehash: 22726ed8acae69adc09389b8f5f28df594a570a3
ms.sourcegitcommit: 083aa7cc8fc958fc75365462aed542f1b5409623
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/11/2019
ms.locfileid: "70915427"
---
# <a name="send-events-to-or-receive-events-from-event-hubs-using-python"></a>Envío o recepción de eventos en Event Hubs mediante Python

Azure Event Hubs es una plataforma de streaming de macrodatos y servicio de ingesta de eventos de gran escalabilidad capaz de recibir y procesar millones de eventos por segundo. Event Hubs puede procesar y almacenar eventos, datos o telemetría generados por dispositivos y software distribuido. Los datos enviados a un centro de eventos se pueden transformar y almacenar con cualquier proveedor de análisis en tiempo real o adaptadores de procesamiento por lotes y almacenamiento. Para más información sobre Event Hubs, consulte [Introducción a Event Hubs](event-hubs-about.md) y [Características de Event Hubs](event-hubs-features.md).

En este tutorial se describe cómo crear aplicaciones de Python para enviar o recibir eventos en un centro de eventos. 

> [!NOTE]
> Puede descargar esta guía de inicio rápido como un ejemplo desde [GitHub](https://github.com/Azure/azure-event-hubs-python/tree/master/examples), reemplazar las cadenas `EventHubConnectionString` y `EventHubName` por los valores del centro de eventos, y ejecutarlo. También puede seguir los pasos de este tutorial para crear el suyo propio.

## <a name="prerequisites"></a>Requisitos previos

Para completar este tutorial, debe cumplir los siguientes requisitos previos:

- Una suscripción de Azure. Si no tiene una, [cree una cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.
- Python 3.4 o versiones posteriores.
- Use [Azure Portal](https://portal.azure.com) para crear un espacio de nombres de tipo Event Hubs y obtener las credenciales de administración que la aplicación necesita para comunicarse con el centro de eventos. Para crear un espacio de nombres y un centro de eventos, siga el procedimiento que se indica en [este artículo](event-hubs-create.md). Luego, para obtener el valor de la clave de acceso del centro de eventos, siga las instrucciones del artículo: [Obtenga la cadena de conexión](event-hubs-get-connection-string.md#get-connection-string-from-the-portal). Utilice la clave de acceso en el código que escriba más adelante en este tutorial. El nombre de la clave predeterminada es: **RootManageSharedAccessKey**.

## <a name="install-python-package"></a>Instalación del paquete de Python

Para instalar el paquete de Python correspondiente a Event Hubs, abra un símbolo del sistema que tenga Python en su ruta de acceso y, después, ejecute el siguiente comando: 

```bash
pip install azure-eventhub
```

## <a name="send-events"></a>Envío de eventos

> [!NOTE]
> El código de esta sección es para la versión estable actual (1.3.1) del SDK de Event Hubs. Si busca el código de ejemplo que usa la versión preliminar del SDK, consulte [esta página](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/eventhub/azure-eventhubs/examples).

### <a name="create-a-python-script-to-send-events"></a>Creación de un script de Python para enviar eventos

A continuación, cree una aplicación de Python que envíe eventos a un centro de eventos:

1. Abra el editor de Python que prefiera, como [Visual Studio Code](https://code.visualstudio.com/).
2. Cree un script llamado **send.py**. Este script envía 100 eventos a un centro de eventos.
3. Pegue el código siguiente en recv.py, reemplazando los valores ADDRESS, USER y KEY con los que obtuvo de Azure Portal en la sección anterior: 

```python
import sys
import logging
import datetime
import time
import os

from azure.eventhub import EventHubClient, Sender, EventData

logger = logging.getLogger("azure")

# Address can be in either of these formats:
# "amqps://<URL-encoded-SAS-policy>:<URL-encoded-SAS-key>@<mynamespace>.servicebus.windows.net/myeventhub"
# "amqps://<mynamespace>.servicebus.windows.net/myeventhub"
# For example:
ADDRESS = "amqps://mynamespace.servicebus.windows.net/myeventhub"

# SAS policy and key are not required if they are encoded in the URL
USER = "RootManageSharedAccessKey"
KEY = "namespaceSASKey"

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
            sender.send(EventData(str(i)))
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

### <a name="run-application-to-send-events"></a>Ejecución de la aplicación para enviar eventos

Para ejecutar el script, abra un símbolo del sistema que tenga Python en su ruta de acceso y, después, ejecute el siguiente comando:

```bash
start python send.py
```

Felicidades. Ha enviado mensajes a un centro de eventos.

## <a name="receive-events"></a>Recepción de eventos

### <a name="create-a-python-script-to-receive-events"></a>Creación de un script de Python para recibir eventos

A continuación, cree una aplicación de Python que reciba eventos de un centro de eventos:

1. Abra el editor de Python que prefiera, como [Visual Studio Code](https://code.visualstudio.com/).
2. Cree un script llamado **recv.py**.
3. Pegue el código siguiente en recv.py, reemplazando los valores ADDRESS, USER y KEY con los que obtuvo de Azure Portal en la sección anterior: 

```python
import os
import sys
import logging
import time
from azure.eventhub import EventHubClient, Receiver, Offset

logger = logging.getLogger("azure")

# Address can be in either of these formats:
# "amqps://<URL-encoded-SAS-policy>:<URL-encoded-SAS-key>@<mynamespace>.servicebus.windows.net/myeventhub"
# "amqps://<mynamespace>.servicebus.windows.net/myeventhub"
# For example:
ADDRESS = "amqps://mynamespace.servicebus.windows.net/myeventhub"

# SAS policy and key are not required if they are encoded in the URL
USER = "RootManageSharedAccessKey"
KEY = "namespaceSASKey"
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
        last_offset = event_data.offset
        last_sn = event_data.sequence_number
        print("Received: {}, {}".format(last_offset, last_sn))
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

### <a name="receive-events"></a>Recepción de eventos

Para ejecutar el script, abra un símbolo del sistema que tenga Python en su ruta de acceso y, después, ejecute el siguiente comando:

```bash
start python recv.py
```
 
## <a name="next-steps"></a>Pasos siguientes
Lea los siguientes artículos:

- [EventProcessorHost](event-hubs-event-processor-host.md)
- [Características y terminología de Azure Event Hubs](event-hubs-features.md)
- [Preguntas más frecuentes sobre Event Hubs](event-hubs-faq.md)

