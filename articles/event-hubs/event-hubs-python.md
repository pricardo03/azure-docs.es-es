---
title: 'Uso de Python para enviar y recibir mensajes: Azure Event Hubs | Microsoft Docs'
description: Obtenga información sobre cómo enviar eventos, recibir eventos y capturar la transmisión de eventos mediante Event Hubs con Python.
keywords: ''
documentationcenter: ''
services: event-hubs
author: ShubhaVijayasarathy
manager: ''
editor: ''
ms.assetid: ''
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/26/2018
ms.author: shvija
ms.openlocfilehash: 88fdaec9e19c082a6fe981dc4d9a0e015335f1e2
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60202980"
---
# <a name="how-to-use-azure-event-hubs-from-a-python-application"></a>Cómo usar Azure Event Hubs desde una aplicación de Python
Azure Event Hubs es una plataforma de streaming de macrodatos y servicio de ingesta de eventos de gran escalabilidad capaz de recibir y procesar millones de eventos por segundo. Event Hubs puede procesar y almacenar eventos, datos o telemetría generados por dispositivos y software distribuido. Los datos enviados a un centro de eventos se pueden transformar y almacenar con cualquier proveedor de análisis en tiempo real o adaptadores de procesamiento por lotes y almacenamiento. Para más información, consulte [Introducción a Event Hubs](event-hubs-what-is-event-hubs.md). 

Este artículo contiene vínculos a artículos que muestran cómo realizar las tareas siguientes desde una aplicación escrita en **Python**:

- [Envío de eventos a un centro de eventos](#send-events-to-event-hubs)
- [Recepción de eventos desde un centro de eventos](#receive-events-from-event-hubs)
- Lectura de datos de eventos capturados desde Azure Storage. 

## <a name="prerequisites"></a>Requisitos previos
- Cree un centro de eventos siguiendo uno de estos inicios rápidos: [Azure Portal](event-hubs-create.md), [CLI de Azure](event-hubs-quickstart-cli.md), [Azure PowerShell](event-hubs-quickstart-powershell.md), [plantilla de Azure Resource Manager](event-hubs-resource-manager-namespace-event-hub.md). 
- Python 3.4 o posterior instalado en el equipo.

## <a name="install-python-package"></a>Instalación del paquete de Python

Para instalar el paquete de Python correspondiente a Event Hubs, abra un símbolo del sistema que tenga Python en su ruta de acceso y, después, ejecute el siguiente comando: 

```bash
pip install azure-eventhub
```

## <a name="send-events-to-event-hubs"></a>Envío de eventos a Event Hubs
El código siguiente muestra cómo enviar eventos a un centro de eventos desde una aplicación de Python: 

1. Cree variables para contener la dirección URL del centro de eventos, el nombre de clave y el valor de clave. 

    ```python
    # Import classes from Event Hubs python package
    from azure.eventhub import EventHubClient, Receiver, Offset
    
    # Address can be in either of these formats:
    # "amqps://<URL-encoded-SAS-policy>:<URL-encoded-SAS-key>@<mynamespace>.servicebus.windows.net/myeventhub"
    # "amqps://<mynamespace>.servicebus.windows.net/myeventhub"
    # For example:
    ADDRESS = "amqps://<MyEventHubNamspaceName>.servicebus.windows.net/<MyEventHubName>"
    
    # SAS policy and key are not required if they are encoded in the URL
    USER = "<Name of the access key. Default name: RootManageSharedAccessKey>"
    KEY = "<The access key>"
    ```

2. Cree un cliente de Event Hubs, agregue un remitente, ejecute el cliente, envíe el evento con el remitente y, a continuación, detenga el cliente cuando haya terminado. 

    ```python
    # Create an Event Hubs client
    client = EventHubClient(ADDRESS, debug=False, username=USER, password=KEY)
    
    # Add a sender to the client
    sender = client.add_sender(partition="0")
    
    # Run the Event Hub client
    client.run()
    
    # Send event to the event hub
    sender.send(EventData("<MyEventData>"))
    
    # Stop the Event Hubs client
    client.stop()
    
    ```

Para obtener un tutorial completo sobre cómo enviar eventos a un centro de eventos desde una aplicación escrita en Python, consulte [este artículo](event-hubs-python-get-started-send.md).

## <a name="receive-events-from-event-hubs"></a>Recepción de eventos de Event Hubs
El código siguiente muestra cómo recibir eventos de un centro de eventos desde una aplicación de Python: 

```python

# Create an Event Hubs client
client = EventHubClient(ADDRESS, debug=False, username=USER, password=KEY)

# Add a receiver to the client
receiver = client.add_receiver(CONSUMER_GROUP, PARTITION, prefetch=5000, offset=OFFSET)

# Run the Event Hubs client
client.run()

# Receive event data from the event hub
for event_data in receiver.receive(timeout=100):
    last_offset = event_data.offset
    last_sn = event_data.sequence_number
    print("Received: {}, {}".format(last_offset, last_sn))

# Stop the Event Hubs client
client.stop()
```

Para obtener un tutorial completo sobre cómo recibir eventos de un centro de eventos desde una aplicación escrita en Python, consulte [este artículo](event-hubs-python-get-started-receive.md)

## <a name="read-capture-event-data-from-azure-storage"></a>Lectura de datos de eventos capturados desde Azure Storage
El código siguiente muestra cómo leer datos de eventos capturados almacenados en **Azure Blob Storage** desde una aplicación de Python: Habilite la característica **Capture** en el centro de eventos siguiendo las instrucciones de: [Habilitación de Event Hubs Capture mediante Azure Portal](event-hubs-capture-enable-through-portal.md). A continuación, envíe algunos eventos al centro de eventos antes de probar el código. 

```python
import os
import string
import json
import avro.schema
from avro.datafile import DataFileReader, DataFileWriter
from avro.io import DatumReader, DatumWriter
from azure.storage.blob import BlockBlobService

def processBlob(filename):
    reader = DataFileReader(open(filename, 'rb'), DatumReader())
    dict = {}
    for reading in reader:
        parsed_json = json.loads(reading["Body"])
        if not 'id' in parsed_json:
            return
        if not dict.has_key(parsed_json['id']):
            list = []
            dict[parsed_json['id']] = list
        else:
            list = dict[parsed_json['id']]
            list.append(parsed_json)
    reader.close()
    for device in dict.keys():
        deviceFile = open(device + '.csv', "a")
        for r in dict[device]:
            deviceFile.write(", ".join([str(r[x]) for x in r.keys()])+'\n')

def startProcessing(accountName, key, container):
    print 'Processor started using path: ' + os.getcwd()
    block_blob_service = BlockBlobService(account_name=accountName, account_key=key)
    generator = block_blob_service.list_blobs(container)
    for blob in generator:
        #content_length == 508 is an empty file, so only process content_length > 508 (skip empty files)
        if blob.properties.content_length > 508:
            print('Downloaded a non empty blob: ' + blob.name)
            cleanName = string.replace(blob.name, '/', '_')
            block_blob_service.get_blob_to_path(container, blob.name, cleanName)
            processBlob(cleanName)
            os.remove(cleanName)
        block_blob_service.delete_blob(container, blob.name)
startProcessing('YOUR STORAGE ACCOUNT NAME', 'YOUR KEY', 'capture')

```

Para obtener un tutorial completo sobre cómo leer datos capturados de Event Hubs en un almacenamiento de blobs de Azure desde una aplicación escrita en Python, consulte [este artículo](event-hubs-capture-python.md)

## <a name="github-samples"></a>Ejemplos de GitHub
Puede encontrar más ejemplos de Python en el [repositorio de Git azure-event-hubs-python](https://github.com/Azure/azure-event-hubs-python/).

## <a name="next-steps"></a>Pasos siguientes
Consulte los artículos de la sección de conceptos a partir de [Introducción a las características de Event Hubs](event-hubs-features.md).
