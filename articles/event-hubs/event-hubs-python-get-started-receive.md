---
title: Recepción de eventos de Azure Event Hubs mediante Python | Microsoft Docs
description: Introducción a la recepción de eventos de Event Hubs mediante Python
services: event-hubs
author: sethmanheim
manager: femila
ms.service: event-hubs
ms.workload: core
ms.topic: article
ms.date: 07/26/2018
ms.author: sethm
ms.openlocfilehash: 9707f0cf5a76c0ac4006b53ca8166d3b42fc9af4
ms.sourcegitcommit: dbfd977100b22699823ad8bf03e0b75e9796615f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2018
ms.locfileid: "50240569"
---
# <a name="receive-events-from-event-hubs-using-python"></a>Recepción de eventos de Event Hubs mediante Python

Azure Event Hubs es un sistema de administración de eventos de alta escalabilidad que puede controlar millones de eventos por segundo, habilitando aplicaciones para procesar y analizar las grandes cantidades de datos generados por los dispositivos conectados y por otros sistemas. Una vez recopilados en un centro de eventos, puede recibir y controlar los eventos mediante controladores en proceso o mediante el reenvío a otros sistemas de análisis.

Para más información sobre Event Hubs, consulte [Información general de Event Hubs][Event Hubs overview].

Este tutorial describe cómo recibir eventos de un centro de eventos desde una aplicación escrita en Python. Para enviar eventos, consulte [el artículo de envío correspondiente](event-hubs-python-get-started-send.md).

El código de este tutorial se ha tomado de [estos ejemplos de GitHub](https://github.com/Azure/azure-event-hubs-python/tree/master/examples), que puede examinar para ver toda la aplicación en funcionamiento incluidas las instrucciones de importación y las declaraciones de variables. Hay otros ejemplos disponibles en la misma carpeta de GitHub.

## <a name="prerequisites"></a>Requisitos previos

Para completar este tutorial, debe cumplir los siguientes requisitos previos:

- Python 3.4 o versiones posteriores.
- Un centro de eventos y un espacio de nombres de Event Hubs existentes. Puede crear estas entidades siguiendo las instrucciones de [este artículo](event-hubs-create.md). 

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]


## <a name="install-python-package"></a>Instalación del paquete de Python

Para instalar el paquete de Python correspondiente a Event Hubs, abra un símbolo del sistema que tenga Python en su ruta de acceso y, después, ejecute el siguiente comando: 

```bash
pip install azure-eventhub
```

## <a name="create-a-python-script-to-receive-events"></a>Creación de un script de Python para recibir eventos

A continuación, cree una aplicación de Python que reciba eventos de un centro de eventos:

1. Abra el editor de Python que prefiera, como [Visual Studio Code][Visual Studio Code].
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
    receiver = client.add_receiver(CONSUMER_GROUP, PARTITION, prefetch=5000, offset=OFFSET)
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

## <a name="receive-events"></a>Recepción de eventos

Para ejecutar el script, abra un símbolo del sistema que tenga Python en su ruta de acceso y, después, ejecute el siguiente comando:

```bash
start python recv.py
```
 
## <a name="next-steps"></a>Pasos siguientes
En esta guía de inicio rápido, ha creado la aplicación de Python que recibe mensajes desde un centro de eventos. Para obtener información acerca de cómo enviar eventos a un centro de eventos mediante Python, consulte [Envío de eventos desde el centro de eventos con Python](event-hubs-python-get-started-send.md).

<!-- Links -->
[Event Hubs overview]: event-hubs-about.md
[Visual Studio Code]: https://code.visualstudio.com/
[free account]: https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio
