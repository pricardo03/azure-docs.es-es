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
ms.openlocfilehash: f6671e05c53b8be2e06eb969adc8a4d5736aff37
ms.sourcegitcommit: 068fc623c1bb7fb767919c4882280cad8bc33e3a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/27/2018
ms.locfileid: "39284469"
---
# <a name="using-akka-streams-with-event-hubs-for-apache-kafka"></a>Uso de Akka Streams con Event Hubs para Apache Kafka

Uno de los beneficios clave de usar Apache Kafka es el ecosistema de plataformas a los que se puede conectar. La instancia de Event Hubs habilitada para Kafka combina la flexibilidad de Kafka con la escalabilidad, consistencia y asistencia del ecosistema Azure.

En este tutorial se muestra cómo conectar Akka Streams a instancias de Event Hubs habilitadas para Kafka sin cambiar los clientes del protocolo o ejecutar sus propios clústeres. Azure Event Hubs para Kafka admite [Apache Kafka versión 1.0.](https://kafka.apache.org/10/documentation.html)

## <a name="prerequisites"></a>Requisitos previos

Para completar este tutorial, asegúrese de cumplir estos requisitos previos:

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

## <a name="akka-streams-producer"></a>Productor de Akka Streams

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

## <a name="akka-streams-consumer"></a>Consumidor de Akka Streams

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

* [Más información sobre Events Hubs](event-hubs-what-is-event-hubs.md)
* [Información sobre Event Hubs para Kafka](event-hubs-for-kafka-ecosystem-overview.md)
* Use [MirrorMaker](https://cwiki.apache.org/confluence/pages/viewpage.action?pageId=27846330) para [transmitir eventos desde Kafka local a una instancia de Event Hubs habilitada para Kafka en la nube](event-hubs-kafka-mirror-maker-tutorial.md).
* Aprenda a transmitir en una instancia de Event Hubs habilitada para Kafka mediante [aplicaciones nativas de Kafka](event-hubs-quickstart-kafka-enabled-event-hubs.md) o [Apache Flink](event-hubs-kafka-flink-tutorial.md).
