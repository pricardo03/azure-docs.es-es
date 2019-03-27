---
title: 'Tutorial: Uso de Producer API y Consumer API de Apache Kafka: Azure HDInsight '
description: Aprenda a usar Producer y Consumer API de Apache Kafka con Kafka en HDInsight. En este tutorial, aprenderá a usar estas API con Kafka en HDInsight desde una aplicación de Java.
services: hdinsight
author: dhgoelmsft
ms.author: dhgoel
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: tutorial
ms.date: 11/06/2018
ms.openlocfilehash: 556fc1f04cc6a1d1b594bdd3787ed43d30f607c6
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2019
ms.locfileid: "58091178"
---
# <a name="tutorial-use-the-apache-kafka-producer-and-consumer-apis"></a>Tutorial: Uso de Producer API y Consumer API de Apache Kafka

Aprenda a usar Producer y Consumer API de Apache Kafka con Kafka en HDInsight.

Producer API de Kafka permite a las aplicaciones enviar flujos de datos al clúster de Kafka. Consumer API de Kafka permite a las aplicaciones leer flujos de datos del clúster.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Configurado su entorno de desarrollo
> * Configuración del entorno de implementación
> * Comprendiendo el código
> * Compilar e implementar la aplicación
> * Ejecutar la aplicación en el clúster

Para más información acerca de las API, consulte la documentación de Apache relativa a [Producer API](https://kafka.apache.org/documentation/#producerapi) y a [Consumer API](https://kafka.apache.org/documentation/#consumerapi).

## <a name="set-up-your-development-environment"></a>Configurado su entorno de desarrollo

Debe tener los siguientes componentes instalados en el entorno de desarrollo:

* [Java JDK 8](https://aka.ms/azure-jdks) o equivalente, como OpenJDK.

* [Apache Maven](https://maven.apache.org/)

* Un cliente de SSH y el comando `scp`. Para más información, vea el documento [Uso de SSH con HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md).

* Un editor de texto o el entorno de desarrollo integrado de Java.

Pueden establecer las siguientes variables de entorno al instalar Java y el JDK en la estación de trabajo de desarrollo. Sin embargo, debe comprobar que existen y que contienen los valores correctos para su sistema.

* `JAVA_HOME`: debe apuntar al directorio en el que esté instalado JDK.
* `PATH`: debe contener las rutas de acceso siguientes:

    * `JAVA_HOME` (o la ruta de acceso equivalente).
    * `JAVA_HOME\bin` (o la ruta de acceso equivalente).
    * Directorio en el que esté instalado Maven.

## <a name="set-up-your-deployment-environment"></a>Configuración del entorno de implementación

Para este tutorial se necesita Apache Kafka en HDInsight 3.6. Para aprender a crear un clúster de Kafka en HDInsight, consulte el documento [Inicio de Apache Kafka en HDInsight](apache-kafka-get-started.md).

## <a name="understand-the-code"></a>Comprendiendo el código

La aplicación de ejemplo se encuentra en [https://github.com/Azure-Samples/hdinsight-kafka-java-get-started](https://github.com/Azure-Samples/hdinsight-kafka-java-get-started), en el subdirectorio `Producer-Consumer`. Básicamente, la aplicación consta de cuatro archivos:

* `pom.xml`: este archivo define las dependencias del proyecto, la versión de Java y los métodos de empaquetado.
* `Producer.java`: este archivo envía oraciones aleatorias a Kafka mediante Producer API.
* `Consumer.java`: este archivo usa Consumer API para leer datos de Kafka y emitirlos en STDOUT.
* `Run.java`: la interfaz de línea de comandos que se usa para ejecutar el código de Producer API y Consumer API.

### <a name="pomxml"></a>Pom.xml

Esto es lo más importante que hay que saber del archivo `pom.xml`:

* Dependencias: este proyecto utiliza Producer API y Consumer API de Kafka, ambas proporcionadas por el paquete `kafka-clients`. El siguiente código XML define esta dependencia:

    ```xml
    <!-- Kafka client for producer/consumer operations -->
    <dependency>
      <groupId>org.apache.kafka</groupId>
      <artifactId>kafka-clients</artifactId>
      <version>${kafka.version}</version>
    </dependency>
    ```

    > [!NOTE]  
    > La entrada `${kafka.version}` se declara en la sección `<properties>..</properties>` de `pom.xml` y está configurada para la versión Kafka del clúster de HDInsight.

* Complementos: los complementos de Maven proporcionan varias funcionalidades. En este proyecto, se utilizan los siguientes complementos:

    * `maven-compiler-plugin`: se utiliza para establecer que el proyecto utiliza la versión 8 de Java. Esta es la versión de Java que utiliza HDInsight 3.6.
    * `maven-shade-plugin`: se utiliza para generar un archivo uber-jar que contiene esta aplicación, así como todas las dependencias. También se usa para establecer el punto de entrada de la aplicación, con el fin de que pueda ejecutar directamente el archivo Jar sin tener que especificar la clase principal.

### <a name="producerjava"></a>Producer.java

El productor se comunica con los hosts del agente de Kafka (nodos de trabajo) y envía datos a un tema de Kafka. El siguiente fragmento de código es del archivo [Producer.java](https://github.com/Azure-Samples/hdinsight-kafka-java-get-started/blob/master/Producer-Consumer/src/main/java/com/microsoft/example/Producer.java) del [repositorio de GitHub](https://github.com/Azure-Samples/hdinsight-kafka-java-get-started) y muestra cómo establecer las propiedades del productor:

```java
Properties properties = new Properties();
// Set the brokers (bootstrap servers)
properties.setProperty("bootstrap.servers", brokers);
// Set how to serialize key/value pairs
properties.setProperty("key.serializer","org.apache.kafka.common.serialization.StringSerializer");
properties.setProperty("value.serializer","org.apache.kafka.common.serialization.StringSerializer");
KafkaProducer<String, String> producer = new KafkaProducer<>(properties);
```

### <a name="consumerjava"></a>Consumer.java

El consumidor se comunica con los hosts del agente de Kafka (nodos de trabajo) y lee los registros en un bucle. El siguiente fragmento de código del archivo [Consumer.java](https://github.com/Azure-Samples/hdinsight-kafka-java-get-started/blob/master/Producer-Consumer/src/main/java/com/microsoft/example/Consumer.java) establece las propiedades del consumidor:

```java
KafkaConsumer<String, String> consumer;
// Configure the consumer
Properties properties = new Properties();
// Point it to the brokers
properties.setProperty("bootstrap.servers", brokers);
// Set the consumer group (all consumers must belong to a group).
properties.setProperty("group.id", groupId);
// Set how to serialize key/value pairs
properties.setProperty("key.deserializer","org.apache.kafka.common.serialization.StringDeserializer");
properties.setProperty("value.deserializer","org.apache.kafka.common.serialization.StringDeserializer");
// When a group is first created, it has no offset stored to start reading from. This tells it to start
// with the earliest record in the stream.
properties.setProperty("auto.offset.reset","earliest");

consumer = new KafkaConsumer<>(properties);
```

En este código, el consumidor está configurado para leer desde el principio del tema (`auto.offset.reset` está establecido en `earliest`).

### <a name="runjava"></a>Run.java

El archivo [Run.java](https://github.com/Azure-Samples/hdinsight-kafka-java-get-started/blob/master/Producer-Consumer/src/main/java/com/microsoft/example/Run.java) proporciona una interfaz de línea de comandos que ejecuta el código del productor o del consumidor. Debe proporcionar la información del host del agente de Kafka como parámetro. También puede incluir un valor de identificador de grupo. Dicho calor lo utiliza el proceso de consumidor. Si crea varias instancias de consumidor con el mismo identificador de grupo, equilibrarán la carga de la lectura del tema.

## <a name="build-and-deploy-the-example"></a>Compilación e implementación del ejemplo

Puede omitir los pasos 1 y 2 de la compilación y descargar los archivos JAR(kafka-producer-consumer.jar) pregenerados desde [ https://github.com/Azure-Samples/hdinsight-kafka-java-get-started/tree/master/Prebuilt-Jars ](https://github.com/Azure-Samples/hdinsight-kafka-java-get-started/tree/master/Prebuilt-Jars). Luego puede copiar este archivo jar en su clúster de HDInsight.

1. Descargue los ejemplos de [https://github.com/Azure-Samples/hdinsight-kafka-java-get-started](https://github.com/Azure-Samples/hdinsight-kafka-java-get-started).

2. Cambie los directorios a la ubicación del directorio `Producer-Consumer` y use el siguiente comando:

    ```
    mvn clean package
    ```

    Este comando crea un directorio denominado `target`, que contiene un archivo denominado `kafka-producer-consumer-1.0-SNAPSHOT.jar`.

3. Use los siguientes comandos para copiar el archivo `kafka-producer-consumer-1.0-SNAPSHOT.jar` en el clúster de HDInsight:

    ```bash
    scp ./target/kafka-producer-consumer-1.0-SNAPSHOT.jar SSHUSER@CLUSTERNAME-ssh.azurehdinsight.net:kafka-producer-consumer.jar
    ```

    Reemplace **SSHUSER** por el usuario SSH de su clúster y **CLUSTERNAME** por el nombre de su clúster. Cuando se le solicite, escriba la contraseña del usuario SSH.

## <a id="run"></a> Ejecutar el ejemplo

1. Para abrir una conexión de SSH con el clúster, use el siguiente comando:

    ```bash
    ssh SSHUSER@CLUSTERNAME-ssh.azurehdinsight.net
    ```

    Reemplace **SSHUSER** por el usuario SSH de su clúster y **CLUSTERNAME** por el nombre de su clúster. Si se le solicita, escriba la contraseña de la cuenta de usuario de SSH. Para obtener más información sobre cómo usar `scp` con HDInsight, consulte [Conexión a través de SSH con HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md).

2. Para crear los temas de Kafka que se usan en este ejemplo, siga estos pasos:

    1. Para guardar el nombre del clúster en una variable e instalar una utilidad de análisis de JSON (`jq`), use los siguientes comandos. Cuando se le solicite, escriba el nombre del clúster de Kafka:
    
        ```bash
        sudo apt -y install jq
        read -p 'Enter your Kafka cluster name:' CLUSTERNAME
        ```
    
    2. Para obtener tanto los hosts del agente de Kafka como los hosts de Apache Zookeeper, use los siguientes comandos. Cuando se le solicite, escriba la contraseña de administrador del clúster.
    
        ```bash
        export KAFKABROKERS=`curl -sS -u admin -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/KAFKA/components/KAFKA_BROKER | jq -r '["\(.host_components[].HostRoles.host_name):9092"] | join(",")' | cut -d',' -f1,2`; \
        ```

    3. Para crear el tema `test`, use el siguiente comando:

        ```bash
        java -jar kafka-producer-consumer.jar create test $KAFKABROKERS
        ```

3. Para ejecutar el productor y escribir datos en el tema, utilice el siguiente comando:

    ```bash
    java -jar kafka-producer-consumer.jar producer test $KAFKABROKERS
    ```

4. Cuando finalice el productor, use el comando siguiente para leer desde el tema:

    ```bash
    java -jar kafka-producer-consumer.jar consumer test $KAFKABROKERS
    ```

    Se muestran los registros leídos, junto con un número de registros.

5. Use __Ctrl + C__ para salir del consumidor.

### <a name="multiple-consumers"></a>Varios consumidores

Los consumidores de Kafka usan un grupo de consumidores al leer los registros. Usar el mismo grupo con varios consumidores deriva en lecturas de un tema con equilibrio de carga. Cada consumidor del grupo recibe una parte de los registros.

La aplicación del consumidor acepta un parámetro que se usa como identificador del grupo. Por ejemplo, el comando siguiente inicia un consumidor mediante un identificador de grupo `mygroup`:

```bash
java -jar kafka-producer-consumer.jar consumer test $KAFKABROKERS mygroup
```

Para ver este proceso en acción, use el siguiente comando:

```bash
tmux new-session 'java -jar kafka-producer-consumer.jar consumer test $KAFKABROKERS mygroup' \; split-w
indow -h 'java -jar kafka-producer-consumer.jar consumer test $KAFKABROKERS mygroup' \; attach
```

Este comando usa `tmux` para dividir el terminal en dos columnas. Se inicia un consumidor en cada una de ellas, con el mismo valor de identificador de grupo. Una vez que los consumidores terminan de leer, fíjese en que cada uno de ellos lee solo una parte de los registros. Use __Ctrl + C__ dos veces para salir de `tmux`.

El consumo por clientes del mismo grupo se controla mediante las particiones del tema. En este ejemplo de código, el tema `test` creado anteriormente tiene ocho particiones. Si inicia ocho consumidores, cada uno de ellos lee los registros de una única partición del tema.

> [!IMPORTANT]  
> No puede haber más instancias de consumidor en un grupo de consumidores que particiones. En este ejemplo, un grupo de consumidores puede contener hasta ocho, ya que es el número de particiones del tema. O bien, puede tener varios grupos de consumidores, los cuales no tengan más de ocho consumidores cada uno.

Los registros almacenados en Kafka se almacenan en el orden en que se reciben dentro de una partición. Para lograr la entrega ordenada de registros *dentro de una partición*, cree un grupo de consumidores donde el número de instancias de consumidor coincida con el número de particiones. Para lograr la entrega ordenada de registros *dentro del tema*, cree un grupo de consumidores con solo una instancia de consumidor.

## <a name="next-steps"></a>Pasos siguientes

En este documento, aprendió a usar Producer y Consumer API de Apache Kafka con Kafka en HDInsight. Para más información sobre cómo trabajar con Kafka:

* [Análisis de registros de Apache Kafka](apache-kafka-log-analytics-operations-management.md)
* [Réplica de datos entre clústeres de Apache Kafka](apache-kafka-mirroring.md)
* [Streams API de Apache Kafka con HDInsight](apache-kafka-streams-api.md)
