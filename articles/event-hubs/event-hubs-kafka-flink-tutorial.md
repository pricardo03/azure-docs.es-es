---
title: Uso de Apache Flink con Azure Event Hubs para Apache Kafka | Microsoft Docs
description: Conexión de Apache Flink a una instancia de Event Hubs habilitada para Kafka
services: event-hubs
documentationcenter: ''
author: basilhariri
manager: timlt
ms.service: event-hubs
ms.topic: article
ms.custom: mvc
ms.date: 08/06/2018
ms.author: bahariri
ms.openlocfilehash: 3642c1b7d3de751ecb8a72edaecfe7a15c0acbdb
ms.sourcegitcommit: 74941e0d60dbfd5ab44395e1867b2171c4944dbe
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2018
ms.locfileid: "49320839"
---
# <a name="use-apache-flink-with-azure-event-hubs-for-apache-kafka"></a>Uso de Apache Flink con Azure Event Hubs para Apache Kafka
En este tutorial se muestra cómo conectar instancias de Event Hubs habilitadas para Apache Flink sin cambiar los clientes del protocolo o ejecutar sus propios clústeres. Azure Event Hubs admite [Apache Kafka versión 1.0.](https://kafka.apache.org/10/documentation.html).

Uno de los beneficios clave de usar Apache Kafka es el ecosistema de plataformas a los que se puede conectar. La instancia de Event Hubs habilitada para Kafka combina la flexibilidad de Kafka con la escalabilidad, consistencia y asistencia del ecosistema Azure.

En este tutorial, aprenderá a:
> [!div class="checklist"]
> * Crear un espacio de nombres de Event Hubs
> * Clonación del proyecto de ejemplo
> * Ejecutar el productor Flink 
> * Ejecutar el consumidor Flink

> [!NOTE]
> Este ejemplo está disponible en [GitHub](https://github.com/Azure/azure-event-hubs-for-kafka/tree/master/tutorials/flink).

## <a name="prerequisites"></a>Requisitos previos

Para completar este tutorial, asegúrese de cumplir estos requisitos previos:

* Lea el artículo [Event Hubs para Apache Kafka](event-hubs-for-kafka-ecosystem-overview.md). 
* Una suscripción de Azure. Si no tiene una, cree una [cuenta gratuita](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) antes de empezar.
* [Kit de desarrollo de Java (JDK) 1.7+](http://www.oracle.com/technetwork/java/javase/downloads/index.html)
    * En Ubuntu, ejecute `apt-get install default-jdk` para instalar el JDK.
    * Asegúrese de establecer la variable de entorno JAVA_HOME para que apunte a la carpeta donde está instalado el JDK.
* [Descargue](http://maven.apache.org/download.cgi) e [instale](http://maven.apache.org/install.html) un archivo binario de Maven
    * En Ubuntu, puede ejecutar `apt-get install maven` para instalar Maven.
* [Git](https://www.git-scm.com/downloads)
    * En Ubuntu, puede ejecutar `sudo apt-get install git` para instalar Git.

## <a name="create-an-event-hubs-namespace"></a>Crear un espacio de nombres de Event Hubs

Se requiere un espacio de nombres de Event Hubs para enviar o recibir de cualquier servicio de Event Hubs. Consulte [Creación de un espacio de nombres de Event Hubs y un centro de eventos con Azure Portal](event-hubs-create-kafka-enabled.md) para obtener información sobre cómo obtener un punto de conexión de Kafka para Event Hubs. Asegúrese de copiar la cadena de conexión de Event Hubs para su uso posterior.

## <a name="clone-the-example-project"></a>Clonación del proyecto de ejemplo

Ahora que tiene una cadena de conexión de Event Hubs habilitada para Kafka, clone el repositorio de Azure Event Hubs para Kafka y vaya a la subcarpeta `flink`:

```shell
git clone https://github.com/Azure/azure-event-hubs-for-kafka.git
cd azure-event-hubs-for-kafka/tutorials/flink
```

## <a name="run-flink-producer"></a>Ejecutar el productor Flink

Con el ejemplo de productor de Flink proporcionado, envíe mensajes al servicio Event Hubs.

### <a name="provide-an-event-hubs-kafka-endpoint"></a>Proporcionar un punto de conexión de Kafka para Event Hubs

#### <a name="producerconfig"></a>Producer.config

Actualice los valores `bootstrap.servers` y `sasl.jaas.config` en `producer/src/main/resources/producer.config` para dirigir el productor al punto de conexión de Kafka para Event Hubs con la autenticación correcta.

```xml
bootstrap.servers={YOUR.EVENTHUBS.FQDN}:9093
client.id=FlinkExampleProducer
sasl.mechanism=PLAIN
security.protocol=SASL_SSL
sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required \
   username="$ConnectionString" \
   password="{YOUR.EVENTHUBS.CONNECTION.STRING}";
```

### <a name="run-producer-from-the-command-line"></a>Ejecución del productor desde la línea de comandos

Para ejecutar el productor desde la línea de comandos, genere los JAR y luego ejecútelo desde Maven (o genere los JAR con Maven, luego ejecútelo en Java al agregar los JAR de Kafka necesarios al parámetro classpath):

```shell
mvn clean package
mvn exec:java -Dexec.mainClass="FlinkTestProducer"
```

El productor ahora comenzará a enviar eventos a la instancia de Event Hubs habilitada para Kafka en el tema `test` e imprimir los eventos a stdout.

## <a name="run-flink-consumer"></a>Ejecutar el consumidor Flink

Con el ejemplo de consumidor proporcionado, reciba mensajes desde la instancia de Event Hubs habilitada para Kafka.

### <a name="provide-an-event-hubs-kafka-endpoint"></a>Proporcionar un punto de conexión de Kafka para Event Hubs

#### <a name="consumerconfig"></a>consumer.config

Actualice los valores `bootstrap.servers` y `sasl.jaas.config` en `consumer/src/main/resources/consumer.config` para dirigir el consumidor al punto de conexión de Kafka para Event Hubs con la autenticación correcta.

```xml
bootstrap.servers={YOUR.EVENTHUBS.FQDN}:9093
group.id=FlinkExampleConsumer
sasl.mechanism=PLAIN
security.protocol=SASL_SSL
sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required \
   username="$ConnectionString" \
   password="{YOUR.EVENTHUBS.CONNECTION.STRING}";
```

### <a name="run-consumer-from-the-command-line"></a>Ejecución del consumidor desde la línea de comandos

Para ejecutar el consumidor desde la línea de comandos, genere los JAR y luego ejecútelo desde Maven (o genere los JAR con Maven, luego ejecútelo en Java al agregar los JAR de Kafka necesarios al parámetro classpath):

```shell
mvn clean package
mvn exec:java -Dexec.mainClass="FlinkTestConsumer"
```

Si la instancia de Event Hubs habilitada para Kafka tiene eventos (por ejemplo, si el productor también está ejecutándose), el consumidor ahora comienza a recibir eventos del tema `test`.

Revise la [guía del conector Kafka de Flink](https://ci.apache.org/projects/flink/flink-docs-stable/dev/connectors/kafka.html) para más información acerca de cómo conectar Flink a Kafka.

## <a name="next-steps"></a>Pasos siguientes
En este tutorial aprendió a conectar Apache Flink a instancias de Event Hubs habilitadas para Kafka sin cambiar los clientes del protocolo o ejecutar sus propios clústeres. Como parte de este tutorial, realizó los pasos siguientes: 

> [!div class="checklist"]
> * Crear un espacio de nombres de Event Hubs
> * Clonación del proyecto de ejemplo
> * Ejecutar el productor Flink 
> * Ejecutar el consumidor Flink

Para obtener más información acerca de Event Hubs y Event Hubs para Kafka, consulte el tema siguiente:  

* [Más información sobre Events Hubs](event-hubs-what-is-event-hubs.md)
* [Información sobre Event Hubs para Kafka](event-hubs-for-kafka-ecosystem-overview.md)
* [Más ejemplos en GitHub de Event Hubs para Kafka](https://github.com/Azure/azure-event-hubs-for-kafka)
* Use [MirrorMaker](https://cwiki.apache.org/confluence/pages/viewpage.action?pageId=27846330) para [transmitir eventos desde Kafka local a una instancia de Event Hubs habilitada para Kafka en la nube](event-hubs-kafka-mirror-maker-tutorial.md).
* Aprenda a transmitir en una instancia de Event Hubs habilitada para Kafka mediante [aplicaciones nativas de Kafka](event-hubs-quickstart-kafka-enabled-event-hubs.md)o [Akka Streams](event-hubs-kafka-akka-streams-tutorial.md).
