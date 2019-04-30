---
title: Ejemplo de topología de Apache Storm en Java - Azure HDInsight
description: Aprenda a crear topologías de Apache Storm en Java al crear una topología de recuento de palabras de ejemplo.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
keywords: apache storm,ejemplo de apache storm,storm java,ejemplo de topología de storm
ms.service: hdinsight
ms.topic: conceptual
origin.date: 03/14/2019
ms.date: 04/29/2019
ms.author: v-yiso
ms.custom: H1Hack27Feb2017,hdinsightactive,hdiseo17may2017
ms.openlocfilehash: 43f68908c8549c2f1d8322b5c4ad3985618cfe6e
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "62125177"
---
# <a name="create-an-apache-storm-topology-in-java"></a>Crear una topología de Apache Storm en Java

Aprenda a crear una topología de [Apache Storm](https://storm.apache.org/) basada en Java. En este caso, cree una topología de Storm que implemente una aplicación de recuento de palabras. Para compilar y empaquetar el proyecto, puede usar [Apache Maven](https://maven.apache.org/). A continuación, obtendrá información sobre cómo definir la topología mediante la [Apache Storm Flux](https://storm.apache.org/releases/2.0.0-SNAPSHOT/flux.html) framework.

Después de completar los pasos descritos en este documento, puede implementar la topología en Apache Storm en HDInsight.

> [!NOTE]  
> Hay una versión completa de los ejemplos de topología de Storm creada en este documento disponible en [https://github.com/Azure-Samples/hdinsight-java-storm-wordcount](https://github.com/Azure-Samples/hdinsight-java-storm-wordcount).

## <a name="prerequisites"></a>Requisitos previos

* [Kit de desarrolladores de Java (JDK), versión 8](https://aka.ms/azure-jdks)

* [Apache Maven](https://maven.apache.org/download.cgi) correctamente [instalado](https://maven.apache.org/install.html) según Apache.  Maven es un sistema de compilación de proyectos de Java.

## <a name="test-environment"></a>Entorno de prueba
El entorno usado en este artículo era un equipo que ejecuta Windows 10.  Los comandos se ejecutaron en un símbolo del sistema y se editaron los distintos archivos con el Bloc de notas.

Desde un símbolo del sistema, escriba los siguientes comandos para crear un entorno de trabajo:

```cmd
mkdir C:\HDI
cd C:\HDI
```

## <a name="create-a-maven-project"></a>Creación de un proyecto de Maven

Escriba el siguiente comando para crear un proyecto de Maven denominado **WordCount**:

```cmd
mvn archetype:generate -DarchetypeArtifactId=maven-archetype-quickstart -DgroupId=com.microsoft.example -DartifactId=WordCount -DinteractiveMode=false

cd WordCount
mkdir resources
```

Este comando crea un directorio denominado `WordCount` en la ubicación actual, que contiene un proyecto de Maven básico. El segundo comando cambia el directorio de trabajo actual a `WordCount`. El tercer comando crea un nuevo directorio, `resources`, que se usará más adelante.  El directorio `WordCount` contiene los siguientes elementos:

* `pom.xml`: Contiene la configuración del proyecto de Maven.
* `src\main\java\com\microsoft\example`: Contiene el código de la aplicación.
* `src\test\java\com\microsoft\example`: Contiene pruebas para la aplicación.  

### <a name="remove-the-generated-example-code"></a>Eliminar el código de ejemplo generado

Eliminar los archivos de aplicación y la prueba generada `AppTest.java`, y `App.java` escribiendo los siguientes comandos:

```cmd
DEL src\main\java\com\microsoft\example\App.java
DEL src\test\java\com\microsoft\example\AppTest.java
```

## <a name="add-maven-repositories"></a>Agregar repositorios de Maven

HDInsight se basa en Hortonworks Data Platform (HDP), por lo que se recomienda usar el repositorio de Hortonworks para descargar las dependencias correspondientes a los proyectos de Apache Storm.  

Abra `pom.xml` escribiendo el comando siguiente:

```cmd
notepad pom.xml
```

A continuación, agregue el siguiente código XML después de la `<url> https://maven.apache.org</url>` línea:

```xml
<repositories>
    <repository>
        <releases>
            <enabled>true</enabled>
            <updatePolicy>always</updatePolicy>
            <checksumPolicy>warn</checksumPolicy>
        </releases>
        <snapshots>
            <enabled>false</enabled>
            <updatePolicy>never</updatePolicy>
            <checksumPolicy>fail</checksumPolicy>
        </snapshots>
        <id>HDPReleases</id>
        <name>HDP Releases</name>
        <url>https://repo.hortonworks.com/content/repositories/releases/</url>
        <layout>default</layout>
    </repository>
    <repository>
        <releases>
            <enabled>true</enabled>
            <updatePolicy>always</updatePolicy>
            <checksumPolicy>warn</checksumPolicy>
        </releases>
        <snapshots>
            <enabled>false</enabled>
            <updatePolicy>never</updatePolicy>
            <checksumPolicy>fail</checksumPolicy>
        </snapshots>
        <id>HDPJetty</id>
        <name>Hadoop Jetty</name>
        <url>https://repo.hortonworks.com/content/repositories/jetty-hadoop/</url>
        <layout>default</layout>
    </repository>
</repositories>
```

## <a name="add-properties"></a>Agregar propiedades

Maven permite definir los valores de nivel de proyecto llamados propiedades. En `pom.xml`, agregue el siguiente texto después de la `</repositories>` línea:

```xml
<properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <!--
    This is a version of Storm from the Hortonworks repository that is compatible with HDInsight 3.6.
    -->
    <storm.version>1.1.0.2.6.1.9-1</storm.version>
</properties>
```

Ahora puede usar este valor en otras secciones de `pom.xml`. Por ejemplo, al especificar la versión de los componentes de Storm, puede usar `${storm.version}` en lugar de codificar un valor de forma rígida.

## <a name="add-dependencies"></a>Adición de dependencias

Agregue una dependencia para componentes de Storm. En `pom.xml`, agregue el siguiente texto en el `<dependencies>` sección:

```xml
<dependency>
    <groupId>org.apache.storm</groupId>
    <artifactId>storm-core</artifactId>
    <version>${storm.version}</version>
    <!-- keep storm out of the jar-with-dependencies -->
    <scope>provided</scope>
</dependency>
```

En tiempo de compilación, Maven usa esta información para buscar `storm-core` en el repositorio de Maven. Busca primero en el repositorio del equipo local. Si los archivos no están allí, Maven los descarga desde el repositorio de Maven público y los almacena en el repositorio local.

> [!NOTE]  
> Observe la línea `<scope>provided</scope>` en esta sección. Esta opción indica a Maven que excluya **storm-core** de los archivos JAR creados, ya que el sistema lo proporciona.

## <a name="build-configuration"></a>Configuración de compilación

Los complementos de Maven permiten personalizar las fases de compilación del proyecto, por ejemplo, cómo se compila el proyecto o cómo este se empaqueta en un archivo JAR. En `pom.xml`, agregue el texto siguiente justo encima el `</project>` línea.

```xml
<build>
    <plugins>
    </plugins>
    <resources>
    </resources>
</build>
```

Esta sección se usa para agregar complementos, recursos y otras opciones de configuración de compilación. Para obtener una referencia completa de la `pom.xml` de archivos, consulte [ https://maven.apache.org/pom.html ](https://maven.apache.org/pom.html).

### <a name="add-plug-ins"></a>Agregar complementos

* **Complemento Exec Maven**

    En el caso de las topologías de Apache Storm implementadas en Java, el [Complemento Exec Maven](https://www.mojohaus.org/exec-maven-plugin/) es útil porque le permite ejecutar fácilmente la topología de manera local en su entorno de desarrollo. Agregue lo siguiente a la sección `<plugins>` del archivo `pom.xml` para incluir el complemento Exec Maven:
    
    ```xml
    <plugin>
        <groupId>org.codehaus.mojo</groupId>
        <artifactId>exec-maven-plugin</artifactId>
        <version>1.6.0</version>
        <executions>
            <execution>
            <goals>
                <goal>exec</goal>
            </goals>
            </execution>
        </executions>
        <configuration>
            <executable>java</executable>
            <includeProjectDependencies>true</includeProjectDependencies>
            <includePluginDependencies>false</includePluginDependencies>
            <classpathScope>compile</classpathScope>
            <mainClass>${storm.topology}</mainClass>
            <cleanupDaemonThreads>false</cleanupDaemonThreads> 
        </configuration>
    </plugin>
    ```

* **Apache Maven Compiler Plugin**

    Otro complemento útil es el [Complemento Apache Maven Compiler](https://maven.apache.org/plugins/maven-compiler-plugin/), que se usa para cambiar las opciones de compilación. Cambiar la versión de Java que usa Maven para el origen y destino de la aplicación.
    
  * Para HDInsight __3.4 o una versión anterior__, establezca la versión de origen y destino de Java en __1.7__.
    
  * Para HDInsight __3.5__, establezca la versión de origen y destino de Java en __1.8__.
    
    Agregue el siguiente texto en la sección `<plugins>` del archivo `pom.xml` para incluir el complemento Apache Maven Compiler. Este ejemplo especifica 1.8, por lo que la versión de HDInsight de destino es 3.5.
    
    ```xml
    <plugin>
      <groupId>org.apache.maven.plugins</groupId>
      <artifactId>maven-compiler-plugin</artifactId>
      <version>3.3</version>
      <configuration>
      <source>1.8</source>
      <target>1.8</target>
      </configuration>
    </plugin>
    ```

### <a name="configure-resources"></a>Configure resources

Gracias a la sección de recursos, podrá incluir recursos no basados en códigos, como los archivos de configuración que necesitan los componentes de la topología. En este ejemplo, agregue el siguiente texto en el `<resources>` sección de la `pom.xml` archivo.

```xml
<resource>
    <directory>${basedir}/resources</directory>
    <filtering>false</filtering>
    <includes>
        <include>log4j2.xml</include>
    </includes>
</resource>
```

Este ejemplo agrega el directorio de recursos en la raíz del proyecto (`${basedir}`) como una ubicación que contiene recursos e incluye el archivo denominado `log4j2.xml`. Este archivo se utiliza para configurar la información que registra la topología.

## <a name="create-the-topology"></a>Creación de la topología

Una topología de Apache Storm basada en Java consta de tres componentes que debe crear (o hacer referencia) como una dependencia.

* **Spouts**: lee datos de orígenes externos y emite flujos de datos a la topología.

* **Bolts**: realiza el procesamiento en flujos que emite spouts u otros bolts, y emite uno o varios flujos.

* **Topología**: define cómo se organizan los spouts y bolts, y proporciona el punto de entrada de la topología.

### <a name="create-the-spout"></a>Creación del spout

Para reducir los requisitos de la configuración de los orígenes de datos externos, el spout siguiente simplemente emite frases aleatorias. Se trata de una versión modificada de un spout que se proporciona con los [ejemplos de Storm-Starter](https://github.com/apache/storm/blob/0.10.x-branch/examples/storm-starter/src/jvm/storm/starter).  Aunque esta topología solo usa un spout, otras pueden tener varios que alimenten datos desde orígenes distintos en la topología.

Escriba el comando siguiente para crear y abrir un nuevo archivo `RandomSentenceSpout.java`:

```cmd
notepad src\main\java\com\microsoft\example\RandomSentenceSpout.java
```

A continuación, copie y pegue el código java siguiente en el nuevo archivo.  A continuación, cierre el archivo.

```java
package com.microsoft.example;

import org.apache.storm.spout.SpoutOutputCollector;
import org.apache.storm.task.TopologyContext;
import org.apache.storm.topology.OutputFieldsDeclarer;
import org.apache.storm.topology.base.BaseRichSpout;
import org.apache.storm.tuple.Fields;
import org.apache.storm.tuple.Values;
import org.apache.storm.utils.Utils;

import java.util.Map;
import java.util.Random;

//This spout randomly emits sentences
public class RandomSentenceSpout extends BaseRichSpout {
  //Collector used to emit output
  SpoutOutputCollector _collector;
  //Used to generate a random number
  Random _rand;

  //Open is called when an instance of the class is created
  @Override
  public void open(Map conf, TopologyContext context, SpoutOutputCollector collector) {
  //Set the instance collector to the one passed in
    _collector = collector;
    //For randomness
    _rand = new Random();
  }

  //Emit data to the stream
  @Override
  public void nextTuple() {
  //Sleep for a bit
    Utils.sleep(100);
    //The sentences that are randomly emitted
    String[] sentences = new String[]{ "the cow jumped over the moon", "an apple a day keeps the doctor away",
        "four score and seven years ago", "snow white and the seven dwarfs", "i am at two with nature" };
    //Randomly pick a sentence
    String sentence = sentences[_rand.nextInt(sentences.length)];
    //Emit the sentence
    _collector.emit(new Values(sentence));
  }

  //Ack is not implemented since this is a basic example
  @Override
  public void ack(Object id) {
  }

  //Fail is not implemented since this is a basic example
  @Override
  public void fail(Object id) {
  }

  //Declare the output fields. In this case, an sentence
  @Override
  public void declareOutputFields(OutputFieldsDeclarer declarer) {
    declarer.declare(new Fields("sentence"));
  }
}
```

> [!NOTE]  
> Para ver un ejemplo de un spout que lee desde un origen de datos externos, consulte uno de los siguientes ejemplos:
>
> * [TwitterSampleSPout](https://github.com/apache/storm/blob/0.10.x-branch/examples/storm-starter/src/jvm/storm/starter/spout/TwitterSampleSpout.java): un spout de ejemplo que lee desde Twitter.
> * [Storm-Kafka](https://github.com/apache/storm/tree/0.10.x-branch/external/storm-kafka): un spout que lee desde Kafka.


### <a name="create-the-bolts"></a>Creación de los bolts

Los bolts controlan el procesamiento de datos. Los bolts pueden hacer cualquier cosa; por ejemplo, cálculo, persistencia o hablar con componentes externos. Esta topología utiliza dos bolts:

* **SplitSentence**: divide las frases que emite **RandomSentenceSpout** en palabras individuales.

* **WordCount**: cuenta cuántas veces se ha repetido cada palabra.


#### <a name="splitsentence"></a>SplitSentence

Escriba el comando siguiente para crear y abrir un nuevo archivo `SplitSentence.java`:

```cmd
notepad src\main\java\com\microsoft\example\SplitSentence.java
```

A continuación, copie y pegue el código java siguiente en el nuevo archivo.  A continuación, cierre el archivo.

```java
package com.microsoft.example;

import java.text.BreakIterator;

import org.apache.storm.topology.BasicOutputCollector;
import org.apache.storm.topology.OutputFieldsDeclarer;
import org.apache.storm.topology.base.BaseBasicBolt;
import org.apache.storm.tuple.Fields;
import org.apache.storm.tuple.Tuple;
import org.apache.storm.tuple.Values;

//There are a variety of bolt types. In this case, use BaseBasicBolt
public class SplitSentence extends BaseBasicBolt {

  //Execute is called to process tuples
  @Override
  public void execute(Tuple tuple, BasicOutputCollector collector) {
    //Get the sentence content from the tuple
    String sentence = tuple.getString(0);
    //An iterator to get each word
    BreakIterator boundary=BreakIterator.getWordInstance();
    //Give the iterator the sentence
    boundary.setText(sentence);
    //Find the beginning first word
    int start=boundary.first();
    //Iterate over each word and emit it to the output stream
    for (int end=boundary.next(); end != BreakIterator.DONE; start=end, end=boundary.next()) {
      //get the word
      String word=sentence.substring(start,end);
      //If a word is whitespace characters, replace it with empty
      word=word.replaceAll("\\s+","");
      //if it's an actual word, emit it
      if (!word.equals("")) {
        collector.emit(new Values(word));
      }
    }
  }

  //Declare that emitted tuples contain a word field
  @Override
  public void declareOutputFields(OutputFieldsDeclarer declarer) {
    declarer.declare(new Fields("word"));
  }
}
```

#### <a name="wordcount"></a>WordCount

Escriba el comando siguiente para crear y abrir un nuevo archivo `WordCount.java`:

```cmd
notepad src\main\java\com\microsoft\example\WordCount.java
```

A continuación, copie y pegue el código java siguiente en el nuevo archivo.  A continuación, cierre el archivo.

```java
package com.microsoft.example;

import java.util.HashMap;
import java.util.Map;
import java.util.Iterator;

import org.apache.storm.Constants;
import org.apache.storm.topology.BasicOutputCollector;
import org.apache.storm.topology.OutputFieldsDeclarer;
import org.apache.storm.topology.base.BaseBasicBolt;
import org.apache.storm.tuple.Fields;
import org.apache.storm.tuple.Tuple;
import org.apache.storm.tuple.Values;
import org.apache.storm.Config;

// For logging
import org.apache.logging.log4j.Logger;
import org.apache.logging.log4j.LogManager;

//There are a variety of bolt types. In this case, use BaseBasicBolt
public class WordCount extends BaseBasicBolt {
  //Create logger for this class
  private static final Logger logger = LogManager.getLogger(WordCount.class);
  //For holding words and counts
  Map<String, Integer> counts = new HashMap<String, Integer>();
  //How often to emit a count of words
  private Integer emitFrequency;

  // Default constructor
  public WordCount() {
      emitFrequency=5; // Default to 60 seconds
  }

  // Constructor that sets emit frequency
  public WordCount(Integer frequency) {
      emitFrequency=frequency;
  }

  //Configure frequency of tick tuples for this bolt
  //This delivers a 'tick' tuple on a specific interval,
  //which is used to trigger certain actions
  @Override
  public Map<String, Object> getComponentConfiguration() {
      Config conf = new Config();
      conf.put(Config.TOPOLOGY_TICK_TUPLE_FREQ_SECS, emitFrequency);
      return conf;
  }

  //execute is called to process tuples
  @Override
  public void execute(Tuple tuple, BasicOutputCollector collector) {
    //If it's a tick tuple, emit all words and counts
    if(tuple.getSourceComponent().equals(Constants.SYSTEM_COMPONENT_ID)
            && tuple.getSourceStreamId().equals(Constants.SYSTEM_TICK_STREAM_ID)) {
      for(String word : counts.keySet()) {
        Integer count = counts.get(word);
        collector.emit(new Values(word, count));
        logger.info("Emitting a count of " + count + " for word " + word);
      }
    } else {
      //Get the word contents from the tuple
      String word = tuple.getString(0);
      //Have we counted any already?
      Integer count = counts.get(word);
      if (count == null)
        count = 0;
      //Increment the count and store it
      count++;
      counts.put(word, count);
    }
  }

  //Declare that this emits a tuple containing two fields; word and count
  @Override
  public void declareOutputFields(OutputFieldsDeclarer declarer) {
    declarer.declare(new Fields("word", "count"));
  }
}
```

### <a name="define-the-topology"></a>Definición de la topología

La topología vincula los spouts y los bolts juntos en un gráfico, que define cómo fluyen los datos entre los componentes. También proporciona sugerencias de paralelismos que utiliza Storm al crear instancias de los componentes dentro del clúster.

La siguiente imagen es un diagrama básico del gráfico de componentes para esta topología.

![diagrama que muestra la disposición de los spouts y bolts](./media/apache-storm-develop-java-topology/wordcount-topology.png)

Para implementar la topología, escriba el comando siguiente para crear y abrir un nuevo archivo `WordCountTopology.java`:

```cmd
notepad src\main\java\com\microsoft\example\WordCountTopology.java
```

A continuación, copie y pegue el código java siguiente en el nuevo archivo.  A continuación, cierre el archivo.

```java
package com.microsoft.example;

import org.apache.storm.Config;
import org.apache.storm.LocalCluster;
import org.apache.storm.StormSubmitter;
import org.apache.storm.topology.TopologyBuilder;
import org.apache.storm.tuple.Fields;

import com.microsoft.example.RandomSentenceSpout;

public class WordCountTopology {

  //Entry point for the topology
  public static void main(String[] args) throws Exception {
  //Used to build the topology
    TopologyBuilder builder = new TopologyBuilder();
    //Add the spout, with a name of 'spout'
    //and parallelism hint of 5 executors
    builder.setSpout("spout", new RandomSentenceSpout(), 5);
    //Add the SplitSentence bolt, with a name of 'split'
    //and parallelism hint of 8 executors
    //shufflegrouping subscribes to the spout, and equally distributes
    //tuples (sentences) across instances of the SplitSentence bolt
    builder.setBolt("split", new SplitSentence(), 8).shuffleGrouping("spout");
    //Add the counter, with a name of 'count'
    //and parallelism hint of 12 executors
    //fieldsgrouping subscribes to the split bolt, and
    //ensures that the same word is sent to the same instance (group by field 'word')
    builder.setBolt("count", new WordCount(), 12).fieldsGrouping("split", new Fields("word"));

    //new configuration
    Config conf = new Config();
    //Set to false to disable debug information when
    // running in production on a cluster
    conf.setDebug(false);

    //If there are arguments, we are running on a cluster
    if (args != null && args.length > 0) {
      //parallelism hint to set the number of workers
      conf.setNumWorkers(3);
      //submit the topology
      StormSubmitter.submitTopology(args[0], conf, builder.createTopology());
    }
    //Otherwise, we are running locally
    else {
      //Cap the maximum number of executors that can be spawned
      //for a component to 3
      conf.setMaxTaskParallelism(3);
      //LocalCluster is used to run locally
      LocalCluster cluster = new LocalCluster();
      //submit the topology
      cluster.submitTopology("word-count", conf, builder.createTopology());
      //sleep
      Thread.sleep(10000);
      //shut down the cluster
      cluster.shutdown();
    }
  }
}
```

### <a name="configure-logging"></a>registro

Storm utiliza [Apache Log4j 2](https://logging.apache.org/log4j/2.x/) para registrar información. Si no configura el registro, la topología emite información de diagnóstico. Para controlar lo que se registra, cree un archivo denominado `log4j2.xml` en el `resources` directorio escribiendo el comando siguiente:

```cmd
notepad resources\log4j2.xml
```

A continuación, copie y pegue el texto XML siguiente en el nuevo archivo.  A continuación, cierre el archivo.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<Configuration>
<Appenders>
    <Console name="STDOUT" target="SYSTEM_OUT">
        <PatternLayout pattern="%d{HH:mm:ss} [%t] %-5level %logger{36} - %msg%n"/>
    </Console>
</Appenders>
<Loggers>
    <Logger name="com.microsoft.example" level="trace" additivity="false">
        <AppenderRef ref="STDOUT"/>
    </Logger>
    <Root level="error">
        <Appender-Ref ref="STDOUT"/>
    </Root>
</Loggers>
</Configuration>
```

Este XML configura un nuevo registrador para la clase `com.microsoft.example`, que incluye los componentes de esta topología de ejemplo. El nivel está establecido para realizar un seguimiento de este registrador, que captura cualquier información de registro que generen los componentes de esta topología.

La sección `<Root level="error">` configura el nivel de registro raíz (todo lo que no esté en `com.microsoft.example`) para registrar solo la información de los errores.

Para más información sobre la configuración del registro para Log4j 2, consulte [https://logging.apache.org/log4j/2.x/manual/configuration.html](https://logging.apache.org/log4j/2.x/manual/configuration.html).

> [!NOTE]  
> La versión 0.10.0 y superiores de Storm utilizan Log4j 2.x. Las versiones anteriores de Storm usaban Log4j 1.x, que empleaba otro formato de configuración del registro. Para información sobre la configuración anterior, consulte [https://wiki.apache.org/logging-log4j/Log4jXmlFormat](https://wiki.apache.org/logging-log4j/Log4jXmlFormat).

## <a name="test-the-topology-locally"></a>Prueba de la topología de manera local

Después de guardar los archivos, use el comando siguiente para probar localmente la topología.

```cmd
mvn compile exec:java -Dstorm.topology=com.microsoft.example.WordCountTopology
```

Mientras se ejecuta, la topología muestra información de inicio. El siguiente texto es un ejemplo de la salida del recuento de palabras:

    17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 56 for word snow
    17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 56 for word white
    17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 112 for word seven
    17:33:27 [Thread-16-count] INFO  com.microsoft.example.WordCount - Emitting a count of 195 for word the
    17:33:27 [Thread-30-count] INFO  com.microsoft.example.WordCount - Emitting a count of 113 for word and
    17:33:27 [Thread-30-count] INFO  com.microsoft.example.WordCount - Emitting a count of 57 for word dwarfs
    17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 57 for word snow

Este registro de ejemplo indica que la palabra «and» se ha emitido 113 veces. El recuento continúa aumentando mientras se ejecute la topología porque el spout emite continuamente las mismas frases.

Hay un intervalo de 5 segundos entre la emisión de las palabras y los recuentos. El componente **WordCount** está configurado para emitir la información solo cuando llegue una tupla de marca. Solicita que esas tuplas se entreguen solo cada cinco segundos.

## <a name="convert-the-topology-to-flux"></a>Conversión de la topología a Flux

[Flux](https://storm.apache.org/releases/2.0.0-SNAPSHOT/flux.html) es un nuevo marco de trabajo disponible con Storm 0.10.0 y versiones superiores que permite separar la configuración de la implementación. Los componentes se siguen definiendo en Java, pero la topología se define con un archivo YAML. Puede empaquetar una definición de topología predeterminada con el proyecto, o bien usar un archivo independiente al enviar la topología. Al enviar la topología a Storm, puede usar variables de entorno o archivos de configuración para rellenar los valores de la definición de la topología de YAML.

El archivo YAML define los componentes que se usarán para la topología y el flujo de datos entre ellos. Puede incluir un archivo YAML como parte del archivo jar o puede usar un archivo YAML externo.

Para más información sobre la plataforma Flux, consulte [Plataforma Flux (https://storm.apache.org/releases/1.0.6/flux.html)](https://storm.apache.org/releases/1.0.6/flux.html).

> [!WARNING]  
> Debido a un [error (https://issues.apache.org/jira/browse/STORM-2055)](https://issues.apache.org/jira/browse/STORM-2055) con Storm 1.0.1, es posible que deba instalar un [entorno de desarrollo de Storm](https://storm.apache.org/releases/current/Setting-up-development-environment.html) para ejecutar topologías de Flux de forma local.

1. Anteriormente, `WordCountTopology.java` definía la topología, pero no es necesario con Flux. Elimine el archivo con el siguiente comando:

    ```cmd
    DEL src\main\java\com\microsoft\example\WordCountTopology.java
    ```

2. Escriba el comando siguiente para crear y abrir un nuevo archivo `topology.yaml`:

    ```cmd
    notepad resources\topology.yaml
    ```

    A continuación, copie y pegue el texto siguiente en el nuevo archivo.  A continuación, cierre el archivo.

    ```yaml
    name: "wordcount"       # friendly name for the topology

    config:                 # Topology configuration
      topology.workers: 1     # Hint for the number of workers to create

    spouts:                 # Spout definitions
    - id: "sentence-spout"
      className: "com.microsoft.example.RandomSentenceSpout"
      parallelism: 1      # parallelism hint

    bolts:                  # Bolt definitions
    - id: "splitter-bolt"
      className: "com.microsoft.example.SplitSentence"
      parallelism: 1
        
    - id: "counter-bolt"
      className: "com.microsoft.example.WordCount"
      constructorArgs:
        - 10
      parallelism: 1

    streams:                # Stream definitions
    - name: "Spout --> Splitter" # name isn't used (placeholder for logging, UI, etc.)
      from: "sentence-spout"       # The stream emitter
      to: "splitter-bolt"          # The stream consumer
      grouping:                    # Grouping type
        type: SHUFFLE
    
    - name: "Splitter -> Counter"
      from: "splitter-bolt"
      to: "counter-bolt"
      grouping:
        type: FIELDS
        args: ["word"]           # field(s) to group on
    ```

3. Escriba el siguiente comando para abrir `pom.xml` para que las revisiones se describe a continuación:

    ```cmd
    notepad pom.xml
    ```

   * Agregue la siguiente dependencia nueva en la sección `<dependencies>` :

        ```xml
        <!-- Add a dependency on the Flux framework -->
        <dependency>
            <groupId>org.apache.storm</groupId>
            <artifactId>flux-core</artifactId>
            <version>${storm.version}</version>
        </dependency>
        ```

   * Agregue el complemento siguiente a la sección `<plugins>` . Este complemento administra la creación de un paquete (archivo jar) para el proyecto y aplica algunas transformaciones específicas a Flux cuando se crea el paquete.

        ```xml
        <!-- build an uber jar -->
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-shade-plugin</artifactId>
            <version>3.2.1</version>
            <configuration>
                <transformers>
                    <!-- Keep us from getting a "can't overwrite file error" -->
                    <transformer implementation="org.apache.maven.plugins.shade.resource.ApacheLicenseResourceTransformer" />
                    <transformer implementation="org.apache.maven.plugins.shade.resource.ServicesResourceTransformer" />
                    <!-- We're using Flux, so refer to it as main -->
                    <transformer implementation="org.apache.maven.plugins.shade.resource.ManifestResourceTransformer">
                        <mainClass>org.apache.storm.flux.Flux</mainClass>
                    </transformer>
                </transformers>
                <!-- Keep us from getting a bad signature error -->
                <filters>
                    <filter>
                        <artifact>*:*</artifact>
                        <excludes>
                            <exclude>META-INF/*.SF</exclude>
                            <exclude>META-INF/*.DSA</exclude>
                            <exclude>META-INF/*.RSA</exclude>
                        </excludes>
                    </filter>
                </filters>
            </configuration>
            <executions>
                <execution>
                    <phase>package</phase>
                    <goals>
                        <goal>shade</goal>
                    </goals>
                </execution>
            </executions>
        </plugin>
        ```

   * En el **exec-maven-plugin** `<configuration>` sección, cambie el valor de `<mainClass>` desde `${storm.topology}` a `org.apache.storm.flux.Flux`. Esta opción permite a Flux controlar la ejecución de la topología localmente en el entorno de desarrollo.

   * En el `<resources>` sección, agregue lo siguiente a `<includes>`. Este XML incluye el archivo YAML que define la topología como parte del proyecto.

        ```xml
        <include>topology.yaml</include>
        ```

## <a name="test-the-flux-topology-locally"></a>Prueba local de la topología

1. Escriba el siguiente comando para compilar y ejecutar la topología de Flux con Maven:

    ```cmd
    mvn compile exec:java -Dexec.args="--local -R /topology.yaml"
    ```

    > [!WARNING]  
    > Este comando produce un error si la topología usa bits de Storm 1.0.1. Este error lo causa [https://issues.apache.org/jira/browse/STORM-2055](https://issues.apache.org/jira/browse/STORM-2055). En su lugar, [instale Storm en su entorno de desarrollo](https://storm.apache.org/releases/current/Setting-up-development-environment.html) y siga los siguientes pasos:
    >
    > Si tiene [instalado Storm en el entorno de desarrollo](https://storm.apache.org/releases/current/Setting-up-development-environment.html), puede usar en su lugar los siguientes comandos:
    >
    > ```cmd
    > mvn compile package
    > storm jar target/WordCount-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --local -R /topology.yaml
    > ```

    El parámetro `--local` ejecuta la topología en modo local en el entorno de desarrollo. El parámetro `-R /topology.yaml` usa el recurso de archivo `topology.yaml` del archivo jar para definir la topología.

    Mientras se ejecuta, la topología muestra información de inicio. El siguiente texto es un ejemplo de la salida:

        17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 56 for word snow
        17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 56 for word white
        17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 112 for word seven
        17:33:27 [Thread-16-count] INFO  com.microsoft.example.WordCount - Emitting a count of 195 for word the
        17:33:27 [Thread-30-count] INFO  com.microsoft.example.WordCount - Emitting a count of 113 for word and
        17:33:27 [Thread-30-count] INFO  com.microsoft.example.WordCount - Emitting a count of 57 for word dwarfs

    Hay un retraso de 10 segundos entre los distintos lotes de la información registrada.

2. Crear un nuevo yaml topología desde el proyecto.
 
     a. Escriba el siguiente comando para abrir `topology.xml`:

    ```cmd
    notepad resources\topology.yaml
    ```

    b. Busque la siguiente sección y cambie el valor de `10` a `5`. Esta modificación altera el intervalo entre la emisión de lotes de recuentos de palabras de 10 segundos a 5.  

    ```yaml
    - id: "counter-bolt"
      className: "com.microsoft.example.WordCount"
      constructorArgs:
        - 5
      parallelism: 1  
    ```  

    c. Guardar archivo como `newtopology.yaml`.

3. Para ejecutar la topología, escriba el siguiente comando:

    ```cmd
    mvn exec:java -Dexec.args="--local resources/newtopology.yaml"
    ```

    También puede hacer lo siguiente si tiene Storm en el entorno de desarrollo:

    ```cmd
    storm jar target/WordCount-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --local resources/newtopology.yaml
    ```

     Este comando usa el `newtopology.yaml` como la definición de topología. Puesto que no se incluyó el parámetro `compile`, Maven usa la versión del proyecto compilada en los pasos anteriores.

    Una vez que se inicia la topología, observará que el tiempo entre los lotes emitidos ha cambiado para reflejar el valor en `newtopology.yaml`. Esto demuestra que puede cambiar la configuración a través de un archivo YAML sin tener que volver a compilar la topología.

Para más información sobre estas y otras características del marco de trabajo Flux, consulte [Flux (https://storm.apache.org/releases/current/flux.html)](https://storm.apache.org/releases/current/flux.html).

## <a name="trident"></a>Trident

[Trident](https://storm.apache.org/releases/current/Trident-API-Overview.html) es una abstracción de alto nivel que ofrece Storm. Admite el procesamiento con estado. La principal ventaja de Trident es que puede garantizar que todos los mensajes que entran en la topología se procesan una sola vez. Si no se usa Trident, la topología solo puede garantizar que los mensajes se procesan al menos una vez. También hay otras diferencias, como los componentes integrados que se pueden usar en lugar de crear bolts. De hecho, los bolts se reemplazan por componentes menos genéricos, como filtros, proyecciones y funciones.

Las aplicaciones de Trident se pueden crear mediante proyectos de Maven. Siga los mismos pasos básicos que se mostraron anteriormente en este artículo; lo único diferente es el código. Actualmente, tampoco puede usarse Trident con el marco de trabajo Flux.

Para obtener más información sobre Trident, consulte [Información general sobre la API de Trident](https://storm.apache.org/releases/current/Trident-API-Overview.html).

## <a name="next-steps"></a>Pasos siguientes

Ha aprendido a crear una topología de Apache Storm con Java. Ahora obtenga información sobre:

* [Implementación y administración de topologías de Apache Storm en HDInsight](apache-storm-deploy-monitor-topology-linux.md)

* [Desarrollo de topologías de C# para Apache Storm en HDInsight con Visual Studio](apache-storm-develop-csharp-visual-studio-topology.md)

Puede encontrar más topologías de ejemplo de Apache Storm en [Topologías de ejemplo para Apache Storm en HDInsight](apache-storm-example-topology.md).
