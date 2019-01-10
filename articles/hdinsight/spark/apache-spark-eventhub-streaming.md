---
title: 'Tutorial: Procesamiento de datos desde Event Hubs con Apache Spark en Azure HDInsight '
description: Conecte Apache Spark en Azure HDInsight a Azure Event Hubs y procese los datos de streaming.
services: hdinsight
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive,mvc
ms.topic: conceptual
ms.date: 12/28/2018
ms.openlocfilehash: 81104c7b206d4fe158df1ae9d329084ad88c3bdd
ms.sourcegitcommit: 803e66de6de4a094c6ae9cde7b76f5f4b622a7bb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/02/2019
ms.locfileid: "53976637"
---
# <a name="tutorial-process-tweets-using-azure-event-hubs-and-apache-spark-in-hdinsight"></a>Tutorial: Procesamiento de tweets con Azure Event Hubs y Apache Spark en HDInsight

En este tutorial, aprenderá a crear una aplicación de streaming de [Apache Spark](https://spark.apache.org/) para enviar tweets a un centro de eventos de Azure y creará otra aplicación para leerlos desde dicho centro. Para obtener una explicación detallada del streaming de Spark consulte la [información general sobre el streaming de Apache Spark](https://spark.apache.org/docs/latest/streaming-programming-guide.html#overview). HDInsight ofrece las mismas funciones de streaming para un clúster de Spark en Azure.

En este tutorial, aprenderá a:
> [!div class="checklist"]
> * Envío de mensajes a un centro de eventos de Azure
> * Lectura de mensajes de un centro de eventos de Azure

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

* **Completar el artículo [Tutorial: Carga de datos y ejecución de consultas en un clúster de Apache Spark en Azure HDInsight](./apache-spark-load-data-run-query.md)**.

## <a name="create-a-twitter-application"></a>Crear una aplicación de Twitter

Para recibir un flujo de tweets, cree una aplicación en Twitter. Siga las instrucciones para crear una aplicación de Twitter y anote los valores que necesita para finalizar este tutorial.

1. Vaya a la [página de administración de aplicaciones de Twitter](https://apps.twitter.com/).

1. Seleccione **Crear nueva aplicación**.

1. Proporcione los valores siguientes:

    - Nombre: proporcione el nombre de la aplicación. El valor utilizado para este tutorial es **HDISparkStreamApp0423**. Este nombre debe ser único.
    - Descripción: proporcione una descripción breve de la aplicación. El valor utilizado para este tutorial es **una aplicación de streaming sencilla de HDInsight Spark**.
    - Sitio web: indique el sitio web de la aplicación. No es necesario que sea un sitio web válido.  El valor utilizado para este tutorial es **http://www.contoso.com**.
    - Dirección URL de devolución de llamada: puede dejarla en blanco.

1. Seleccione **Yes, I have read and agree to the Twitter Developer Agreement** (Sí, he leído y acepto el acuerdo para desarrolladores de Twitter) y luego seleccione **Create your Twitter application** (Crear la aplicación de Twitter).

1. Seleccione la pestaña **Claves y tokens de acceso** .

1. Seleccione **Create my access token** (Crear mi token de acceso) al final de la página.

1. Anote los valores siguientes de la página.  Necesitará estos valores más adelante en el tutorial:

    - **Clave de consumidor (clave de API)**    
    - **Secreto de consumidor (secreto de API)**  
    - **Token de acceso**
    - **Secreto de token de acceso**   

## <a name="create-an-azure-event-hubs-namespace"></a>Creación de un espacio de nombres de Azure Event Hubs

Use este centro de eventos para almacenar tweets.

1. Inicie sesión en el [Azure Portal](https://portal.azure.com). 

1. En el menú izquierdo, seleccione **Todos los servicios**.  

1. En **Internet de las cosas**, seleccione **Event Hubs**. 

    ![Creación de un centro de eventos para un ejemplo de streaming de Spark](./media/apache-spark-eventhub-streaming/hdinsight-create-event-hub-for-spark-streaming.png "Create event hub for Spark streaming example")

4. Seleccione **+Agregar**.
5. Escriba los siguientes valores para el nuevo espacio de nombres de Event Hubs:

    - **Nombre**: especifique un nombre para el centro de eventos.  El valor utilizado para este tutorial es **myeventhubns20180403**.

    - **Plan de tarifa**: Seleccione **Estándar**.

    - **Suscripción**: Seleccione una suscripción adecuada.

    - **Grupo de recursos**: Seleccione un grupo de recursos existente en la lista desplegable o seleccione la opción **Crear nuevo** para crear un nuevo grupo de recursos.

    - **Ubicación**: para reducir la latencia y los costos, debe seleccionar la misma **ubicación** que la del clúster Apache Spark en HDInsight.

    - **Habilitar el inflado automático**: (opcional) el inflado automático permite escalar el número de unidades de procesamiento asignado a su espacio de nombres de Event Hubs cuando el tráfico supera la capacidad asignada de las unidades de procesamiento.  

    - **Aumentar de forma automática el máximo de unidades de procesamiento**: (opcional) este control deslizante solo aparecerá si marca **Habilitar el inflado automático**.  

      ![Aportación de un nombre de centro de evento para el ejemplo de streaming de Spark](./media/apache-spark-eventhub-streaming/hdinsight-provide-event-hub-name-for-spark-streaming.png "Provide an event hub name for Spark streaming example")
6. Seleccione **Crear** para crear el espacio de nombres.  La implementación se completará en unos minutos.

## <a name="create-an-azure-event-hub"></a>Creación de un centro de eventos de Azure
Crear un centro de eventos una vez implementado el espacio de nombres de Event Hubs.  En el portal:

1. En el menú izquierdo, seleccione **Todos los servicios**.  

1. En **Internet de las cosas**, seleccione **Event Hubs**.  

1. Seleccione el espacio de nombres de Event Hubs en la lista.  

1. En la página **Espacio de nombres de Event Hubs**, seleccione **+Centro de eventos**.  
1. Escriba los siguientes valores en la página **Crear centro de eventos**:

    - **Nombre**: asigne un nombre al centro de eventos. 
 
    - **Número de particiones**: 10.  

    - **Retención de mensajes**: 1.   
   
      ![Aportación de detalles del centro de evento para el ejemplo de streaming de Spark](./media/apache-spark-eventhub-streaming/hdinsight-provide-event-hub-details-for-spark-streaming-example.png "Provide event hub details for Spark streaming example")

1. Seleccione **Crear**.  La implementación debería completarse en unos segundos y será reconducido a la página Espacio de nombres de Event Hubs.

1. En **Configuración**, seleccione **Directivas de acceso compartido**.

1. Seleccione **RootManageSharedAccessKey**.
    
     ![Establecimiento de directivas de Event Hub para el ejemplo de streaming de Spark](./media/apache-spark-eventhub-streaming/hdinsight-set-event-hub-policies-for-spark-streaming-example.png "Set Event Hub policies for the Spark streaming example")

1. Guarde los valores **Clave principal** y **Cadena de conexión: clave principal** para usarlos más adelante en este tutorial.

     ![Vista de las claves de directiva de Event Hub para el ejemplo de streaming de Spark](./media/apache-spark-eventhub-streaming/hdinsight-view-event-hub-policy-keys.png "View Event Hub policy keys for the Spark streaming example")


## <a name="send-tweets-to-the-event-hub"></a>Envío de tweets al centro de eventos

Cree un cuaderno de Jupyter Notebook y asignarle el nombre **SendTweetsToEventHub**. 

1. Ejecute el código siguiente para agregar las bibliotecas de Apache Maven externas:

    ```
    %%configure
    {"conf":{"spark.jars.packages":"com.microsoft.azure:azure-eventhubs-spark_2.11:2.2.0,org.twitter4j:twitter4j-core:4.0.6"}}
    ```

2. Ejecute el siguiente código para enviar tweets al centro de eventos:

    ```
    import java.util._
    import scala.collection.JavaConverters._
    import java.util.concurrent._
    
    import org.apache.spark._
    import org.apache.spark.streaming._
    import org.apache.spark.eventhubs.ConnectionStringBuilder

    // Event hub configurations
    // Replace values below with yours        
    val eventHubName = "<Event hub name>"
    val eventHubNSConnStr = "<Event hub namespace connection string>"
    val connStr = ConnectionStringBuilder(eventHubNSConnStr).setEventHubName(eventHubName).build 
    
    import com.microsoft.azure.eventhubs._
    val pool = Executors.newFixedThreadPool(1)
    val eventHubClient = EventHubClient.create(connStr.toString(), pool)
    
    def sendEvent(message: String) = {
          val messageData = EventData.create(message.getBytes("UTF-8"))
          eventHubClient.get().send(messageData)
          println("Sent event: " + message + "\n")
    }
    
    import twitter4j._
    import twitter4j.TwitterFactory
    import twitter4j.Twitter
    import twitter4j.conf.ConfigurationBuilder

    // Twitter application configurations
    // Replace values below with yours   
    val twitterConsumerKey = "<CONSUMER KEY>"
    val twitterConsumerSecret = "<CONSUMER SECRET>"
    val twitterOauthAccessToken = "<ACCESS TOKEN>"
    val twitterOauthTokenSecret = "<TOKEN SECRET>"
    
    val cb = new ConfigurationBuilder()
    cb.setDebugEnabled(true).setOAuthConsumerKey(twitterConsumerKey).setOAuthConsumerSecret(twitterConsumerSecret).setOAuthAccessToken(twitterOauthAccessToken).setOAuthAccessTokenSecret(twitterOauthTokenSecret)
    
    val twitterFactory = new TwitterFactory(cb.build())
    val twitter = twitterFactory.getInstance()

    // Getting tweets with keyword "Azure" and sending them to the Event Hub in realtime!
    
    val query = new Query(" #Azure ")
    query.setCount(100)
    query.lang("en")
    var finished = false
    while (!finished) {
      val result = twitter.search(query)
      val statuses = result.getTweets()
      var lowestStatusId = Long.MaxValue
      for (status <- statuses.asScala) {
        if(!status.isRetweet()){
          sendEvent(status.getText())
        }
        lowestStatusId = Math.min(status.getId(), lowestStatusId)
        Thread.sleep(2000)
      }
      query.setMaxId(lowestStatusId - 1)
    }
    
    // Closing connection to the Event Hub
    eventHubClient.get().close()
    ```

3. Abra el centro de eventos en Azure Portal.  En **Introducción**, verá que algunos gráficos muestran los mensajes enviados al centro de eventos.

## <a name="read-tweets-from-the-event-hub"></a>Lectura de tweets desde el centro de eventos

Cree otro cuaderno de Jupyter Notebook y asignarle el nombre **ReadTweetsFromEventHub**. 

1. Ejecute el código siguiente para agregar una biblioteca de Apache Maven externa:

    ```
    %%configure -f
    {"conf":{"spark.jars.packages":"com.microsoft.azure:azure-eventhubs-spark_2.11:2.2.0"}}
    ```
2. Ejecute el código siguiente para leer tweets del centro de eventos:

    ```
    import org.apache.spark.eventhubs._
    // Event hub configurations
    // Replace values below with yours        
    val eventHubName = "<Event hub name>"
    val eventHubNSConnStr = "<Event hub namespace connection string>"
    val connStr = ConnectionStringBuilder(eventHubNSConnStr).setEventHubName(eventHubName).build 
    
    val customEventhubParameters = EventHubsConf(connStr).setMaxEventsPerTrigger(5)
    val incomingStream = spark.readStream.format("eventhubs").options(customEventhubParameters.toMap).load()
    //incomingStream.printSchema    
    
    import org.apache.spark.sql.types._
    import org.apache.spark.sql.functions._
    
    // Event Hub message format is JSON and contains "body" field
    // Body is binary, so you cast it to string to see the actual content of the message
    val messages = incomingStream.withColumn("Offset", $"offset".cast(LongType)).withColumn("Time (readable)", $"enqueuedTime".cast(TimestampType)).withColumn("Timestamp", $"enqueuedTime".cast(LongType)).withColumn("Body", $"body".cast(StringType)).select("Offset", "Time (readable)", "Timestamp", "Body")
    
    messages.printSchema
    
    messages.writeStream.outputMode("append").format("console").option("truncate", false).start().awaitTermination()
    ```

## <a name="clean-up-resources"></a>Limpieza de recursos

Con HDInsight, los datos se almacenan en Azure Storage o Azure Data Lake Storage, por lo que cualquier clúster se puede eliminar de forma segura cuando no está en uso. También se le cobrará por un clúster de HDInsight aunque no se esté usando. Si tiene previsto pasar inmediatamente al siguiente tutorial, es aconsejable conservar el clúster; de lo contrario, elimínelo.

Abra el clúster en Azure Portal y seleccione **Eliminar**.

![Eliminar clúster de HDInsight](./media/apache-spark-load-data-run-query/hdinsight-azure-portal-delete-cluster.png "Eliminar clúster de HDInsight")

También puede seleccionar el nombre del grupo de recursos para abrir la página del grupo de recursos y, después, hacer clic en **Eliminar grupo de recursos**. Al eliminar el grupo de recursos, se eliminan tanto el clúster de HDInsight Spark como la cuenta de almacenamiento predeterminada.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial aprendió lo siguiente:

* Leer el mensaje de un centro de eventos.
Vaya al siguiente artículo para saber cómo crear una aplicación de Machine Learning. 

> [!div class="nextstepaction"]
> [Creación de una aplicación de Machine Learning](./apache-spark-ipython-notebook-machine-learning.md)


