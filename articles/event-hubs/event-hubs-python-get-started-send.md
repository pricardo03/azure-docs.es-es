---
title: 'Envío de eventos mediante Python: Azure Event Hubs | Microsoft Docs'
description: En este artículo se ofrece un tutorial para crear una aplicación de Node.js que envía eventos a Azure Event Hubs.
services: event-hubs
author: ShubhaVijayasarathy
manager: femila
ms.service: event-hubs
ms.workload: core
ms.topic: article
ms.date: 11/16/2018
ms.author: shvija
ms.openlocfilehash: b7adf3976f5f7e028ffa9ffeb13db22d3d4bba8e
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/08/2018
ms.locfileid: "53102986"
---
# <a name="send-events-to-event-hubs-using-python"></a>Envío de eventos a Event Hubs mediante Python

Azure Event Hubs es una plataforma de streaming de macrodatos y servicio de ingesta de eventos de gran escalabilidad capaz de recibir y procesar millones de eventos por segundo. Event Hubs puede procesar y almacenar eventos, datos o telemetría generados por dispositivos y software distribuido. Los datos enviados a un centro de eventos se pueden transformar y almacenar con cualquier proveedor de análisis en tiempo real o adaptadores de procesamiento por lotes y almacenamiento. Para más información sobre Event Hubs, consulte [Introducción a Event Hubs](event-hubs-about.md) y [Características de Event Hubs](event-hubs-features.md).

Este tutorial describe cómo enviar eventos a un centro de eventos desde una aplicación escrita en Python. 

> [!NOTE]
> Puede descargar esta guía de inicio rápido como un ejemplo desde [GitHub](https://github.com/Azure/azure-event-hubs-python/tree/master/examples), reemplazar las cadenas `EventHubConnectionString` y `EventHubName` por los valores del centro de eventos, y ejecutarlo. También puede seguir los pasos de este tutorial para crear el suyo propio.

## <a name="prerequisites"></a>Requisitos previos

Para completar este tutorial, debe cumplir los siguientes requisitos previos:

- Una suscripción de Azure. Si no tiene una, [cree una cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.
- Python 3.4 o versiones posteriores.


## <a name="create-an-event-hubs-namespace-and-an-event-hub"></a>Creación de un espacio de nombres de Event Hubs y un centro de eventos
El primer paso consiste en usar [Azure Portal](https://portal.azure.com) para crear un espacio de nombres de tipo Event Hubs y obtener las credenciales de administración que la aplicación necesita para comunicarse con el centro de eventos. Para crear un espacio de nombres y un centro de eventos, siga el procedimiento que se indica en [este artículo](event-hubs-create.md).

Obtenga el valor de la clave de acceso para el centro de eventos siguiendo las instrucciones del artículo: [Obtenga la cadena de conexión](event-hubs-get-connection-string.md#get-connection-string-from-the-portal). Utilice la clave de acceso en el código que escriba más adelante en este tutorial. El nombre de la clave predeterminada es: **RootManageSharedAccessKey**.

Ahora, continúe con los pasos siguientes en este tutorial.

## <a name="install-python-package"></a>Instalación del paquete de Python

Para instalar el paquete de Python correspondiente a Event Hubs, abra un símbolo del sistema que tenga Python en su ruta de acceso y, después, ejecute el siguiente comando: 

```bash
pip install azure-eventhub
```

## <a name="create-a-python-script-to-send-events"></a>Creación de un script de Python para enviar eventos

A continuación, cree una aplicación de Python que envíe eventos a un centro de eventos:

1. Abra el editor de Python que prefiera, como [Visual Studio Code][Visual Studio Code].
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

## <a name="run-application-to-send-events"></a>Ejecución de la aplicación para enviar eventos

Para ejecutar el script, abra un símbolo del sistema que tenga Python en su ruta de acceso y, después, ejecute el siguiente comando:

```bash
start python send.py
```

Felicidades. Ha enviado mensajes a un centro de eventos.
 
## <a name="next-steps"></a>Pasos siguientes
En esta guía de inicio rápido, ha enviado mensajes a un centro de eventos mediante Python. Para saber cómo recibir eventos desde un centro de eventos mediante Python, consulte [Recepción de eventos de Event Hubs mediante Python](event-hubs-python-get-started-receive.md).

<!-- Links -->
[Event Hubs overview]: event-hubs-about.md
[Visual Studio Code]: https://code.visualstudio.com/
[free account]: https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio
