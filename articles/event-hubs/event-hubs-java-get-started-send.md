---
title: 'Envío de eventos mediante Java: Azure Event Hubs | Microsoft Docs'
description: En este artículo se ofrece un tutorial para crear una aplicación de Java que envía eventos a Azure Event Hubs.
services: event-hubs
author: ShubhaVijayasarathy
manager: timlt
ms.service: event-hubs
ms.workload: core
ms.topic: article
ms.custom: seodec18
ms.date: 12/06/2018
ms.author: shvija
ms.openlocfilehash: 80c413c874ca3e1bf46bfa4e5becb184223c5eeb
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/08/2018
ms.locfileid: "53091302"
---
# <a name="send-events-to-azure-event-hubs-using-java"></a>Envío de eventos a Azure Event Hubs mediante Java

Azure Event Hubs es una plataforma de streaming de macrodatos y servicio de ingesta de eventos de gran escalabilidad capaz de recibir y procesar millones de eventos por segundo. Event Hubs puede procesar y almacenar eventos, datos o telemetría generados por dispositivos y software distribuido. Los datos enviados a un centro de eventos se pueden transformar y almacenar con cualquier proveedor de análisis en tiempo real o adaptadores de procesamiento por lotes y almacenamiento. Para más información sobre Event Hubs, consulte [Introducción a Event Hubs](event-hubs-about.md) y [Características de Event Hubs](event-hubs-features.md).

En este tutorial se muestra cómo enviar eventos a un centro de eventos mediante una aplicación de consola escrita en Java. 

> [!NOTE]
> Puede descargar esta guía de inicio rápido como un ejemplo desde [GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/Java/Basic/SimpleSend), reemplazar las cadenas `EventHubConnectionString` y `EventHubName` por los valores del centro de eventos, y ejecutarlo. También puede seguir los pasos de este tutorial para crear el suyo propio.

## <a name="prerequisites"></a>Requisitos previos

Para completar este tutorial, debe cumplir los siguientes requisitos previos:

* Un entorno de desarrollo de Java. En este tutorial se usa [Eclipse](https://www.eclipse.org/).

## <a name="create-an-event-hubs-namespace-and-an-event-hub"></a>Creación de un espacio de nombres de Event Hubs y un centro de eventos
El primer paso consiste en usar [Azure Portal](https://portal.azure.com) para crear un espacio de nombres de tipo Event Hubs y obtener las credenciales de administración que la aplicación necesita para comunicarse con el centro de eventos. Para crear un espacio de nombres y un centro de eventos, siga el procedimiento que se indica en [este artículo](event-hubs-create.md).

Obtenga el valor de la clave de acceso para el centro de eventos siguiendo las instrucciones del artículo: [Obtenga la cadena de conexión](event-hubs-get-connection-string.md#get-connection-string-from-the-portal). Utilice la clave de acceso en el código que escriba más adelante en este tutorial. El nombre de la clave predeterminada es: **RootManageSharedAccessKey**.

Ahora, continúe con los pasos siguientes en este tutorial.

## <a name="add-reference-to-azure-event-hubs-library"></a>Incorporación de una referencia a la biblioteca de Azure Event Hubs

La biblioteca de cliente de Java para Event Hubs está disponible para su uso en proyectos de Maven desde el [repositorio central de Maven](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22). Puede hacer referencia a esta biblioteca con la siguiente declaración de dependencia en el archivo de proyecto de Maven. La versión actual es 1.0.2:    

```xml
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-eventhubs</artifactId>
    <version>1.0.2</version>
</dependency>
```

Para distintos tipos de entornos de compilación, puede obtener explícitamente los últimos archivos JAR publicados del [repositorio central de Maven](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22).  

Para un editor de eventos simples, importe el paquete *com.microsoft.azure.eventhubs* para las clases de cliente de Event Hubs y el paquete *com.microsoft.azure.servicebus* para las clases de utilidad, como las excepciones comunes que se comparten con el cliente de mensajería de Azure Service Bus. 

## <a name="write-code-to-send-messages-to-the-event-hub"></a>Escritura de código para enviar mensajes al centro de eventos

Para el ejemplo siguiente, primero cree un nuevo proyecto de Maven para una aplicación de consola o shell en su entorno de desarrollo de Java favorito. Asigne `SimpleSend` como nombre de la clase:     

```java
package com.microsoft.azure.eventhubs.samples.send;

import com.google.gson.Gson;
import com.google.gson.GsonBuilder;
import com.microsoft.azure.eventhubs.ConnectionStringBuilder;
import com.microsoft.azure.eventhubs.EventData;
import com.microsoft.azure.eventhubs.EventHubClient;
import com.microsoft.azure.eventhubs.EventHubException;

import java.io.IOException;
import java.nio.charset.Charset;
import java.time.Instant;
import java.util.concurrent.ExecutionException;
import java.util.concurrent.Executors;
import java.util.concurrent.ExecutorService;

public class SimpleSend {

    public static void main(String[] args)
            throws EventHubException, ExecutionException, InterruptedException, IOException {
            
            
    }
 }
```

### <a name="construct-connection-string"></a>Crear cadena de conexión

Use la clase ConnectionStringBuilder para construir un valor de cadena de conexión y pasarlo a la instancia de cliente de Event Hubs. Reemplace los marcadores de posición por los valores que obtuvo al crear el espacio de nombres y el centro de eventos:

```java
final ConnectionStringBuilder connStr = new ConnectionStringBuilder()
        .setNamespaceName("Your Event Hubs namespace name")
        .setEventHubName("Your event hub")
        .setSasKeyName("Your policy name")
        .setSasKey("Your primary SAS key");
```

### <a name="send-events"></a>Envío de eventos

Cree un evento singular transformando una cadena en su codificación de bytes UTF-8. Después cree una instancia de cliente de Event Hubs a partir de la cadena de conexión y envíe el mensaje:   

```java 
String payload = "Message " + Integer.toString(i);
byte[] payloadBytes = gson.toJson(payload).getBytes(Charset.defaultCharset());
EventData sendEvent = EventData.create(payloadBytes);

final EventHubClient ehClient = EventHubClient.createSync(connStr.toString(), executorService);
ehClient.sendSync(sendEvent);
    
// close the client at the end of your program
ehClient.closeSync();

``` 

Compile y ejecute el programa y asegúrese de que no haya errores.

Felicidades. Ha enviado mensajes a un centro de eventos.

### <a name="appendix-how-messages-are-routed-to-eventhub-partitions"></a>Apéndice: Cómo se enrutan los mensajes a las particiones de EventHub

Antes de que los consumidores puedan recuperar los mensajes, los editores tienen que publicarlos primero en las particiones. Cuando los mensajes se publican en el centro de eventos de manera sincrónica con el método sendSync() en el objeto com.microsoft.azure.eventhubs.EventHubClient, se pueden enviar a una partición específica o distribuirse a todas las particiones disponibles en modo round-robin, según si se especifica o no la clave de partición.

Cuando se especifica una cadena que representa la clave de partición, se aplicará un algoritmo hash a la clave para determinar la partición a la que desea enviar el evento.

Cuando no se establece la clave de partición, los mensajes se envían en modo round-robin a todas las particiones disponibles

```java
// Serialize the event into bytes
byte[] payloadBytes = gson.toJson(messagePayload).getBytes(Charset.defaultCharset());

// Use the bytes to construct an {@link EventData} object
EventData sendEvent = EventData.create(payloadBytes);

// Transmits the event to event hub without a partition key
// If a partition key is not set, then we will round-robin to all topic partitions
eventHubClient.sendSync(sendEvent);

//  the partitionKey will be hash'ed to determine the partitionId to send the eventData to.
eventHubClient.sendSync(sendEvent, partitionKey);

// close the client at the end of your program
eventHubClient.closeSync();

```

## <a name="next-steps"></a>Pasos siguientes

En esta guía de inicio rápido, ha enviado mensajes a un centro de eventos mediante Java. Para saber cómo recibir eventos de un centro de eventos mediante Java, consulte [Recepción de eventos desde Azure Event Hubs mediante Java](event-hubs-java-get-started-receive-eph.md).

<!-- Links -->
[Event Hubs overview]: event-hubs-overview.md
[free account]: https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio

