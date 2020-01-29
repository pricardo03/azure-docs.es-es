---
title: Lectura de datos capturados en la aplicación de Python en Azure Event Hubs | Microsoft Docs
description: En este artículo se muestra cómo escribir código de Python para capturar datos que se envían a un centro de eventos y leer los datos de eventos capturados desde Azure Storage.
services: event-hubs
documentationcenter: ''
author: spelluru
editor: ''
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/05/2019
ms.author: spelluru
ms.openlocfilehash: 43223f7cb9ed254340c99d235d494d1e93583c7f
ms.sourcegitcommit: 7221918fbe5385ceccf39dff9dd5a3817a0bd807
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/21/2020
ms.locfileid: "76293545"
---
# <a name="capture-event-hubs-data-in-azure-storage-and-read-it-using-python"></a>Capture datos de centros de eventos en Azure Storage y léalos mediante Python 
Puede usar Configurar un centro de eventos para que los datos enviados a un centro de eventos se capturen en Azure Storage o Azure Data Lake Storage. En este artículo se muestra cómo escribir código de Python para enviar eventos a un centro de eventos y leer los datos capturados en Azure Blob Storage. Para obtener más información acerca de esta característica, consulte la [Introducción a la característica de Captura de centros de eventos](event-hubs-capture-overview.md).

En este ejemplo se usa el [SDK de Azure para Python](https://azure.microsoft.com/develop/python/) a fin de demostrar la característica Capture. El programa sender.py envía datos telemétricos del entorno simulados a Event Hubs en formato JSON. El centro de eventos está configurado para usar la característica Capture para escribir estos datos en Blob Storage en lotes. La aplicación capturereader.py lee estos blobs y crea un archivo de anexos por dispositivo. Luego, la aplicación escribe los datos en archivos .csv.

> [!IMPORTANT]
> Este inicio rápido usa la versión 5 del SDK de Python para Azure Event Hubs. Para ver un inicio rápido que usa la versión 1 antigua del SDK de Python, consulte [este artículo](event-hubs-capture-python.md). Si usa la versión 1 del SDK, se recomienda migrar el código a la versión más reciente. Para obtener más información, consulte la [Guía de migración](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/eventhub/azure-eventhub/migration_guide.md).

En esta guía de inicio rápido: 

> [!div class="checklist"]
> * Crear una cuenta de Azure Blob Storage y un contenedor en Azure Portal.
> * Crear un espacio de nombres de Event Hubs mediante Azure Portal.
> * Cree un centro de eventos con la característica de Captura habilitada y conéctelo a su cuenta de almacenamiento.
> * Enviar datos al centro de eventos con un script de Python.
> * Leer y procesar los archivos de Capture de Event Hubs con otro script de Python.

## <a name="prerequisites"></a>Prerequisites

- Python 2.7 y 3.5 o posterior, con `pip` instalado y actualizado.
- Suscripción a Azure. Si no tiene una, [cree una cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.
- [Crear un espacio de nombres de Event Hubs y un centro de eventos en el espacio de nombres](event-hubs-create.md). Anote el nombre del espacio de nombres de Event Hubs, el nombre del centro de eventos y la clave de acceso principal del espacio de nombres. Obtenga la clave de acceso siguiendo las instrucciones del artículo: [Obtenga la cadena de conexión](event-hubs-get-connection-string.md#get-connection-string-from-the-portal). El nombre de la clave predeterminada es: **RootManageSharedAccessKey**. No necesita la cadena de conexión en el tutorial. Solo necesita la clave principal. 
- Siga estos pasos para crear una **cuenta de Azure Storage** y un **contenedor de blobs**:
    1. [Cree una cuenta de Azure Storage](../storage/common/storage-quickstart-create-account.md?tabs=azure-portal).
    2. [Cree un contenedor de blobs en el almacenamiento](../storage/blobs/storage-quickstart-blobs-portal.md#create-a-container). 
    3. [Obtenga la cadena de conexión con la cuenta de almacenamiento](../storage/common/storage-configure-connection-string.md#view-and-copy-a-connection-string).

        Anote la **cadena de conexión** y el **nombre del contenedor**. Los usará más adelante en el código. 
- Habilitar la característica **Capturar** en el centro de eventos siguiendo las instrucciones de: [Habilitación de Event Hubs Capture mediante Azure Portal](event-hubs-capture-enable-through-portal.md). Seleccione la cuenta de almacenamiento y el contenedor de blobs que creó en el paso anterior. También puede habilitar la característica al crear un centro de eventos. 

## <a name="create-a-python-script-to-send-events-to-your-event-hub"></a>Creación de un script de Python para enviar eventos a un centro de eventos
En esta sección, creará un script de Python que envía 200 eventos (10 dispositivos x 20 eventos) a un centro de eventos. Los eventos son lecturas del entorno de ejemplo que se envían en formato JSON. 

1. Abra el editor de Python que prefiera, como [Visual Studio Code][Visual Studio Code].
2. Crear un script denominado **sender.py**. 
3. Pegue el código siguiente en sender.py. Vea los comentarios de código para obtener más detalles. 
   
    ```python
    import time
    import os
    import uuid
    import datetime
    import random
    import json
    
    from azure.eventhub import EventHubProducerClient, EventData
    
    # this scripts simulates production of events for 10 devices
    devices = []
    for x in range(0, 10):
        devices.append(str(uuid.uuid4()))
    
    # create a producer client to produce/publish events to the event hub
    producer = EventHubProducerClient.from_connection_string(conn_str="EVENT HUBS NAMESAPCE CONNECTION STRING", eventhub_name="EVENT HUB NAME")
    
    for y in range(0,20):    # for each device, produce 20 events 
        event_data_batch = producer.create_batch() # create a batch. you will add events to the batch later. 
        for dev in devices:
            # create a dummy reading
            reading = {'id': dev, 'timestamp': str(datetime.datetime.utcnow()), 'uv': random.random(), 'temperature': random.randint(70, 100), 'humidity': random.randint(70, 100)}
            s = json.dumps(reading) # convert reading into a JSON string
            event_data_batch.add(EventData(s)) # add event data to the batch
        producer.send_batch(event_data_batch) # send the batch of events to the event hub
    
    # close the producer    
    producer.close()
    ```
4. Reemplace los valores siguientes en los scripts:
    1. Reemplace `EVENT HUBS NAMESPACE CONNECTION STRING` por la cadena de conexión para el espacio de nombres de Event Hubs.
    2. Reemplace `EVENT HUB NAME` por el nombre del centro de eventos. 
5. Ejecute el script para enviar eventos al centro de eventos. 
6. En Azure Portal, puede comprobar que el centro de eventos ha recibido los mensajes. Cambie a la vista **Mensajes** en la sección **Métricas**. Actualice la página para actualizar el gráfico. Puede tardar unos segundos en mostrar que los mensajes se han recibido. 

    [![Comprobación de que el centro de eventos ha recibido los mensajes](./media/get-started-capture-python-v2/messages-portal.png)](./media/get-started-capture-python-v2/messages-portal.png#lightbox)

## <a name="create-a-python-script-to-read-your-capture-files"></a>Creación de un script de Python que lea archivos de Capture
En este ejemplo, los datos capturados se almacenan en Azure Blob Storage. El script de esta sección lee los archivos de datos de captura de Azure Storage y genera archivos CSV que podrá abrir fácilmente para ver sus contenidos. Verá 10 archivos en el directorio de trabajo actual de la aplicación. Estos archivos contendrán las lecturas de entorno para los 10 dispositivos. 

1. En el editor de Python, cree un script denominado **capturereader.py**. Este script lee los archivos capturados y crea un archivo por dispositivo para escribir los datos solo para dicho dispositivo.
2. Pegue el código siguiente en capturereader.py. Vea los comentarios de código para obtener más detalles. 
   
    ```python
    import os
    import string
    import json
    import uuid
    import avro.schema
    
    from azure.storage.blob import ContainerClient, BlobClient
    from avro.datafile import DataFileReader, DataFileWriter
    from avro.io import DatumReader, DatumWriter
    
    
    def processBlob2(filename):
        reader = DataFileReader(open(filename, 'rb'), DatumReader())
        dict = {}
        for reading in reader:
            parsed_json = json.loads(reading["Body"])
            if not 'id' in parsed_json:
                return
            if not parsed_json['id'] in dict:
                list = []
                dict[parsed_json['id']] = list
            else:
                list = dict[parsed_json['id']]
                list.append(parsed_json)
        reader.close()
        for device in dict.keys():
            filename = os.getcwd() + '\\' + str(device) + '.csv'
            deviceFile = open(filename, "a")
            for r in dict[device]:
                deviceFile.write(", ".join([str(r[x]) for x in r.keys()])+'\n')
    
    def startProcessing():
        print('Processor started using path: ' + os.getcwd())
        # create a blob container client
        container = ContainerClient.from_connection_string("AZURE STORAGE CONNECTION STRING", container_name="BLOB CONTAINER NAME")
        blob_list = container.list_blobs() # list all the blobs in the container
        for blob in blob_list:
            #content_length == 508 is an empty file, so only process content_length > 508 (skip empty files)        
            if blob.size > 508:
                print('Downloaded a non empty blob: ' + blob.name)
                # create a blob client for the blob
                blob_client = ContainerClient.get_blob_client(container, blob=blob.name)
                # construct a file name based on the blob name
                cleanName = str.replace(blob.name, '/', '_')
                cleanName = os.getcwd() + '\\' + cleanName 
                with open(cleanName, "wb+") as my_file: # open the file to write. create if it doesn't exist. 
                    my_file.write(blob_client.download_blob().readall()) # write blob contents into the file
                processBlob2(cleanName) # convert the file into a CSV file
                os.remove(cleanName) # remove the original downloaded file
                # delete the blob from the container after it's read
                container.delete_blob(blob.name)
    
    startProcessing()    
    ```
4. Reemplace `<AZURE STORAGE CONNECTION STRING>` por la cadena de conexión para la cuenta de Azure Storage. El nombre del contenedor que creó en este tutorial es: **capture**. Si ha usado un nombre diferente para el contenedor, reemplace `capture` por el nombre del contenedor en la cuenta de almacenamiento. 

## <a name="run-the-scripts"></a>Ejecución de los scripts
1. Abra un símbolo del sistema que tiene Python en su ruta de acceso y, después, ejecute dichos comandos para instalar los paquetes de requisitos previos de Python:
   
   ```
   pip install azure-storage-blob
   pip install azure-eventhub
   pip install avro-python3
   ```
2. Cambie el directorio a la ubicación en que guardó sender.py y capturereader.py, y ejecute este comando:
   
   ```
   python sender.py
   ```
   
   Este comando inicia un nuevo proceso de Python para ejecutar el remitente.
3. Espere unos minutos para que se ejecute la captura. Después, escriba el siguiente comando en la ventana de comandos original:
   
   ```
   python capturereader.py
   ```

   Este procesador de captura usa el directorio local para descargar todos los blobs del contenedor o de la cuenta de captura. Procesa los que no estén vacíos y escribe los resultados en forma de archivos .csv en el directorio local.

## <a name="next-steps"></a>Pasos siguientes
Consulte los ejemplos de Python en GitHub [aquí](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/eventhub/azure-eventhub/samples). 


[Azure portal]: https://portal.azure.com/
[Overview of Event Hubs Capture]: event-hubs-capture-overview.md
[1]: ./media/event-hubs-archive-python/event-hubs-python1.png
[About Azure storage accounts]:../storage/common/storage-create-storage-account.md
[Visual Studio Code]: https://code.visualstudio.com/
[Event Hubs overview]: event-hubs-what-is-event-hubs.md
