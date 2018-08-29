---
title: Uso de Akka Streams con Azure Event Hubs para Apache Kafka | Microsoft Docs
description: Conexión de Akka Streams a una instancia de Event Hubs habilitada para Kafka
services: event-hubs
documentationcenter: ''
author: basilhariri
manager: timlt
editor: ''
ms.assetid: ''
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.custom: mvc
ms.date: 06/06/2018
ms.author: bahariri
ms.openlocfilehash: 674c3b5cbd3b4253833a785b32e2bc4b0f1928da
ms.sourcegitcommit: 974c478174f14f8e4361a1af6656e9362a30f515
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/20/2018
ms.locfileid: "42146052"
---
# <a name="using-akka-streams-with-event-hubs-for-apache-kafka"></a>Uso de Akka Streams con Event Hubs para Apache Kafka
En este tutorial se muestra cómo conectar Akka Streams a instancias de Event Hubs habilitadas para Kafka sin cambiar los clientes del protocolo o ejecutar sus propios clústeres. Azure Event Hubs para Kafka admite [Apache Kafka versión 1.0.](https://kafka.apache.org/10/documentation.html)

En este tutorial, aprenderá a:
> [!div class="checklist"]
> * Crear un espacio de nombres de Event Hubs
> * Clonación del proyecto de ejemplo
> * Ejecutar el productor Flink 
> * Ejecutar el consumidor Flink

## <a name="prerequisites"></a>Requisitos previos

Para completar este tutorial, asegúrese de cumplir estos requisitos previos:

* Lea el artículo [Event Hubs para Apache Kafka](event-hubs-for-kafka-ecosystem-overview.md). 
* Una suscripción de Azure. Si no tiene una, cree una [cuenta gratuita](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) antes de empezar.
* [Kit de desarrollo de Java (JDK) 1.8+](http://www.oracle.com/technetwork/java/javase/downloads/index.html)
    * En Ubuntu, ejecute `apt-get install default-jdk` para instalar el JDK.
    * Asegúrese de establecer la variable de entorno JAVA_HOME para que apunte a la carpeta donde está instalado el JDK.
* [Descargue](http://maven.apache.org/download.cgi) e [instale](http://maven.apache.org/install.html) un archivo binario de Maven
    * En Ubuntu, puede ejecutar `apt-get install maven` para instalar Maven.
* [Git](https://www.git-scm.com/downloads)
    * En Ubuntu, puede ejecutar `sudo apt-get install git` para instalar Git.

## <a name="create-an-event-hubs-namespace"></a>Crear un espacio de nombres de Event Hubs

Se requiere un espacio de nombres de Event Hubs para enviar o recibir de cualquier servicio de Event Hubs. Consulte [Creación de un espacio de nombres de Event Hubs y un centro de eventos con Azure Portal](event-hubs-create-kafka-enabled.md) para obtener información sobre cómo obtener un punto de conexión de Kafka para Event Hubs. Asegúrese de copiar la cadena de conexión de Event Hubs para su uso posterior.

## <a name="clone-the-example-project"></a>Clonación del proyecto de ejemplo

Ahora que tiene una cadena de conexión de Event Hubs habilitada para Kafka, clone el repositorio de Azure Event Hubs y vaya a la subcarpeta `akka`:

```shell
git clone https://github.com/Azure/azure-event-hubs.git
cd azure-event-hubs/samples/kafka/akka
```

## <a name="run-akka-streams-producer"></a>Ejecutar el productor de Akka Streams

Con el ejemplo del productor de Akka Streams proporcionado, envíe mensajes al servicio Event Hubs.

### <a name="provide-an-event-hubs-kafka-endpoint"></a>Proporcionar un punto de conexión de Kafka para Event Hubs

#### <a name="producer-applicationconf"></a>application.conf del productor

Actualice los valores `bootstrap.servers` y `sasl.jaas.config` en `producer/src/main/resources/application.conf` para dirigir el productor al punto de conexión de Kafka para Event Hubs con la autenticación correcta.

```xml
akka.kafka.producer {
    #Akka Kafka producer properties can be defined here


    # Properties defined by org.apache.kafka.clients.producer.ProducerConfig
    # can be defined in this configuration section.
    kafka-clients {
        bootstrap.servers="{YOUR.EVENTHUBS.FQDN}:9093"
        sasl.mechanism=PLAIN
        security.protocol=SASL_SSL
        sasl.jaas.config="org.apache.kafka.common.security.plain.PlainLoginModule required username=\"$ConnectionString\" password=\"{YOUR.EVENTHUBS.CONNECTION.STRING}\";"
    }
}
```

### <a name="run-producer-from-the-command-line"></a>Ejecución del productor desde la línea de comandos

Para ejecutar el productor desde la línea de comandos, genere los JAR y luego ejecútelo desde Maven (o genere los JAR con Maven, luego ejecútelo en Java al agregar los JAR de Kafka necesarios al parámetro classpath):

```shell
mvn clean package
mvn exec:java -Dexec.mainClass="AkkaTestProducer"
```

El productor ahora comienza a enviar eventos a la instancia de Event Hubs habilitada para Kafka en el tema `test` e imprimir los eventos a stdout.

## <a name="run-akka-streams-consumer"></a>Ejecutar el consumidor de Akka Streams

Con el ejemplo de consumidor proporcionado, reciba mensajes desde la instancia de Event Hubs habilitada para Kafka.

### <a name="provide-an-event-hubs-kafka-endpoint"></a>Proporcionar un punto de conexión de Kafka para Event Hubs

#### <a name="consumer-applicationconf"></a>application.conf del consumidor

Actualice los valores `bootstrap.servers` y `sasl.jaas.config` en `consumer/src/main/resources/application.conf` para dirigir el consumidor al punto de conexión de Kafka para Event Hubs con la autenticación correcta.

```xml
akka.kafka.consumer {
    #Akka Kafka consumer properties defined here
    wakeup-timeout=60s

    # Properties defined by org.apache.kafka.clients.consumer.ConsumerConfig
    # defined in this configuration section.
    kafka-clients {
       request.timeout.ms=60000
       group.id=akka-example-consumer

       bootstrap.servers="{YOUR.EVENTHUBS.FQDN}:9093"
       sasl.mechanism=PLAIN
       security.protocol=SASL_SSL
       sasl.jaas.config="org.apache.kafka.common.security.plain.PlainLoginModule required username=\"$ConnectionString\" password=\"{YOUR.EVENTHUBS.CONNECTION.STRING}\";"
    }
}
```

### <a name="run-consumer-from-the-command-line"></a>Ejecución del consumidor desde la línea de comandos

Para ejecutar el consumidor desde la línea de comandos, genere los JAR y luego ejecútelo desde Maven (o genere los JAR con Maven, luego ejecútelo en Java al agregar los JAR de Kafka necesarios al parámetro classpath):

```shell
mvn clean package
mvn exec:java -Dexec.mainClass="AkkaTestConsumer"
```

Si la instancia de Event Hubs habilitada para Kafka tiene eventos (por ejemplo, si el productor también está ejecutándose), el consumidor comienza a recibir eventos del tema `test`. 

Consulte la [Guía de Kafka de Akka Streams](https://doc.akka.io/docs/akka-stream-kafka/current/home.html) para más información sobre Akka Streams.

## <a name="next-steps"></a>Pasos siguientes
En este tutorial se muestra cómo conectar Akka Streams a instancias de Event Hubs habilitadas para Kafka sin cambiar los clientes del protocolo o ejecutar sus propios clústeres. Azure Event Hubs para Kafka admite [Apache Kafka versión 1.0.](https://kafka.apache.org/10/documentation.html) Como parte de este tutorial, realizó los pasos siguientes: 

> [!div class="checklist"]
> * Crear un espacio de nombres de Event Hubs
> * Clonación del proyecto de ejemplo
> * Ejecutar el productor Flink 
> * Ejecutar el consumidor Flink

Para obtener más información acerca de Event Hubs y Event Hubs para Kafka, consulte el tema siguiente:  

* [Más información sobre Events Hubs](event-hubs-what-is-event-hubs.md)
* [Información sobre Event Hubs para Kafka](event-hubs-for-kafka-ecosystem-overview.md)
* Use [MirrorMaker](https://cwiki.apache.org/confluence/pages/viewpage.action?pageId=27846330) para [transmitir eventos desde Kafka local a una instancia de Event Hubs habilitada para Kafka en la nube](event-hubs-kafka-mirror-maker-tutorial.md).
* Aprenda a transmitir en una instancia de Event Hubs habilitada para Kafka mediante [aplicaciones nativas de Kafka](event-hubs-quickstart-kafka-enabled-event-hubs.md) o [Apache Flink](event-hubs-kafka-flink-tutorial.md).
