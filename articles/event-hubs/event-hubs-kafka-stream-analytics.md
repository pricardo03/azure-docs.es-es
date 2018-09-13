---
title: Procesamiento de eventos de Apache Kafka para Event Hubs mediante Azure Stream Analytics | Microsoft Docs
description: En este artículo se muestra cómo procesar los eventos de Kafka que se introducen a través de Event Hubs mediante Azure Stream Analytics
services: event-hubs
documentationcenter: ''
author: spelluru
manager: ''
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/29/2018
ms.author: spelluru
ms.openlocfilehash: a066d2a55f6949eea316eaf0a2956500667a996f
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2018
ms.locfileid: "43340278"
---
# <a name="process-apache-kafka-for-event-hubs-events-using-stream-analytics"></a>Procesamiento de eventos de Apache Kafka para Event Hubs mediante Stream Analytics 
En este artículo se muestra cómo transmitir datos a Event Hubs habilitado para Kafka y cómo procesarlos con Azure Stream Analytics. Estos pasos son los siguientes: 

1. Cree un espacio de nombres de Event Hubs habilitado para Kafka.
2. Cree un cliente de Kafka que envíe mensajes al centro de eventos.
3. Cree un trabajo de Stream Analytics que copie datos del centro de eventos en Azure Blob Storage. 

No es necesario cambiar a los clientes de protocolo ni ejecutar sus propios clústeres al usar el punto de conexión de Kafka expuesto por un centro de eventos. Azure Event Hubs admite [Apache Kafka versión 1.0.](https://kafka.apache.org/10/documentation.html) y posteriores. 


## <a name="prerequisites"></a>Requisitos previos

Para completar esta guía de inicio rápido, asegúrese de cumplir los siguientes requisitos previos:

* Una suscripción de Azure. Si no tiene una, cree una [cuenta gratuita](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) antes de empezar.
* [Kit de desarrollo de Java (JDK) 1.7+](http://www.oracle.com/technetwork/java/javase/downloads/index.html).
* [Descargue](http://maven.apache.org/download.cgi) e [instale](http://maven.apache.org/install.html) un archivo binario de Maven.
* [Git](https://www.git-scm.com/)
* Una **cuenta de Azure Storage**. Si no tiene, [cree una](../storage/common/storage-create-storage-account.md#create-a-storage-account) antes de continuar. El trabajo de Stream Analytics en este tutorial almacena los datos de salida en Azure Blob Storage. 


## <a name="create-a-kafka-enabled-event-hubs-namespace"></a>Creación de un espacio de nombres de Event Hubs habilitado para Kafka

1. Inicie sesión en [Azure Portal](https://portal.azure.com) y haga clic en **Crear un recurso** en la parte superior izquierda de la pantalla.
2. Busque **Event Hubs** y seleccione las opciones que se muestran aquí:
    
    ![Búsqueda de Event Hubs en el portal](./media/event-hubs-kafka-stream-analytics/select-event-hubs.png) 
3. En la página **Event Hubs**, seleccione **Crear**.
4. En la página **Crear espacio de nombres**, realice las acciones siguientes: 
    1. Proporcione un **nombre** único para el espacio de nombres. 
    2. Seleccione un **plan de tarifa**. 
    3. Seleccione **Habilitar Kafka**. Este es un paso **importante**. 
    4. Seleccione la **suscripción** en la que quiere que se cree el espacio de nombres del centro de eventos. 
    5. Cree un **grupo de recursos** nuevo o seleccione uno existente. 
    6. Seleccione **una ubicación**. 
    7. Haga clic en **Create**(Crear).
    
        ![Creación de un espacio de nombres](./media/event-hubs-kafka-stream-analytics/create-event-hub-namespace-page.png) 
4. En el **mensaje de notificación**, seleccione el **nombre del grupo de recursos**. 

    ![Creación de un espacio de nombres](./media/event-hubs-kafka-stream-analytics/creation-station-message.png)
1. Seleccione el **espacio de nombres del centro de eventos** en el grupo de recursos. 
2. Una vez creado el espacio de nombres, seleccione **Directivas de acceso compartido** en **CONFIGURACIÓN**.

    ![Haga clic en Directivas de acceso compartido](./media/event-hubs-kafka-stream-analytics/shared-access-policies.png)
5. Puede elegir la directiva **RootManageSharedAccessKey** predeterminada o agregar una nueva directiva. Haga clic en el nombre de la directiva y copie la **cadena de conexión**. Utilice la cadena de conexión para configurar al cliente de Kafka. 
    
    ![Selección de una directiva](./media/event-hubs-kafka-stream-analytics/connection-string.png)  

Ahora puede transmitir eventos desde las aplicaciones que usan el protocolo de Kafka en Event Hubs.

## <a name="send-messages-with-kafka-in-event-hubs"></a>Envío de mensajes con Kafka en Event Hubs

1. Clone el [repositorio de Azure Event Hubs](https://github.com/Azure/azure-event-hubs) en la máquina.
2. Vaya a la carpeta `azure-event-hubs/samples/kafka/quickstart/producer`. 
4. Actualice los detalles de configuración para el productor en `src/main/resources/producer.config`. Especifique el **nombre** y **cadena de conexión** para el **espacio de nombres del centro de eventos**. 

    ```xml
    bootstrap.servers={EVENT HUB NAMESPACE}.servicebus.windows.net:9093
    security.protocol=SASL_SSL
    sasl.mechanism=PLAIN
    sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="$ConnectionString" password="{CONNECTION STRING for EVENT HUB NAMESPACE}";
    ```

5. Vaya a `azure-event-hubs/samples/kafka/quickstart/producer/src/main/java/com/example/app` y abra el archivo **TestDataReporter.java** en el editor que prefiera. 
6. Comente las siguientes líneas de código:

    ```java
                //final ProducerRecord<Long, String> record = new ProducerRecord<Long, String>(TOPIC, time, "Test Data " + i);
    ```
3. Agregue la siguiente línea de código en lugar del código con comentarios: 

    ```java
                final ProducerRecord<Long, String> record = new ProducerRecord<Long, String>(TOPIC, time, "{ \"eventData\": \"Test Data " + i + "\" }");            
    ```

    Este código envía los datos del evento en formato **JSON**. Al configurar la entrada para un trabajo de Stream Analytics, especifique JSON como formato de los datos de entrada. 
7. **Ejecute el productor** y transmítalo a Event Hubs habilitado para Kafka. En un equipo Windows, al usar un **símbolo del sistema Node.js**, cambie a la carpeta `azure-event-hubs/samples/kafka/quickstart/producer` antes de ejecutar estos comandos. 
   
    ```shell
    mvn clean package
    mvn exec:java -Dexec.mainClass="TestProducer"                                    
    ```

## <a name="verify-that-event-hub-receives-the-data"></a>Compruebe que ese centro de eventos recibe los datos.

1. Seleccione **Event Hubs** en **ENTIDADES**. Confirme que ve un centro de eventos denominado **prueba**. 

    ![Centro de eventos: prueba](./media/event-hubs-kafka-stream-analytics/test-event-hub.png)
2. Confirme que ve los mensajes que entran en el centro de eventos. 

    ![Centro de eventos: mensajes](./media/event-hubs-kafka-stream-analytics/confirm-event-hub-messages.png)

## <a name="process-event-data-using-a-stream-analytics-job"></a>Procesamiento de los datos de eventos mediante un trabajo de Stream Analytics
En esta sección, vamos a crear un trabajo de Azure Stream Analytics. El cliente de Kafka envía eventos al centro de eventos. Cree un trabajo de Stream Analytics que tome datos de eventos como entrada y los envíe a Azure Blob Storage. Si no tiene una **cuenta de Azure Storage**, debe [crear una](../storage/common/storage-create-storage-account.md#create-a-storage-account).

La consulta en el trabajo de Stream Analytics se pasa a través de los datos sin realizar ningún análisis. Puede crear una consulta que transforme los datos de entrada para generar datos de salida en un formato diferente o con información obtenida.  

### <a name="create-a-stream-analytics-job"></a>Creación de un trabajo de Stream Analytics 

1. Seleccione **+ Crear un recurso** en [Azure Portal](https://portal.azure.com).
2. Seleccione **Analytics** en el menú **Azure Marketplace** y, luego, seleccione **Stream Analytics job** (Trabajo de Stream Analytics). 
3. En la página **Nuevo Stream Analytics**, haga lo siguiente: 
    1. Escriba un **nombre** para el trabajo. 
    2. Seleccione su **suscripción**.
    3. Seleccione **Crear nuevo** para el **grupo de recursos** y escriba el nombre. También puede **usar un grupo de recursos existente**. 
    4. Seleccione una **ubicación** para el trabajo.
    5. Seleccione **Crear** para crear el trabajo. 

        ![Nuevo trabajo de Stream Analytics](./media/event-hubs-kafka-stream-analytics/new-stream-analytics-job.png)

### <a name="configure-job-input"></a>Configuración de la entrada del trabajo

1. En el mensaje de notificación, seleccione ** Ir al recurso** para ver la página **Stream Analytics job** (Trabajo de Stream Analytics). 
2. Seleccione **Entradas** en la sección **TOPOLOGÍA DE TRABAJO** en el menú izquierdo.
3. Seleccione **Add stream input** (Agregar entrada de flujo) y, luego, seleccione **Event Hub** (Centro de eventos). 

    ![Adición del centro de eventos como entrada](./media/event-hubs-kafka-stream-analytics/select-event-hub-input.png)
4. En la página de configuración de la **entrada del centro de eventos**, haga lo siguiente: 

    1. Especifique un **alias** para la entrada. 
    2. Seleccione la **suscripción de Azure**.
    3. Seleccione el **espacio de nombres del centro de eventos** que ha creado anteriormente. 
    4. Seleccione **probar** para el **centro de eventos**. 
    5. Seleccione **Guardar**. 

        ![Configuración de entrada del centro de eventos](./media/event-hubs-kafka-stream-analytics/event-hub-input-configuration.png)

### <a name="configure-job-output"></a>Configuración de la salida del trabajo 

1. Seleccione **Salidas** en la sección **TOPOLOGÍA DE TRABAJO** en el menú. 
2. Seleccione **+ Agregar** en la barra de herramientas y seleccione **Blob Storage**
3. Haga lo siguiente en la página de configuración de salida de Blob Storage: 
    1. Especifique un **alias** para la salida. 
    2. Selección la **suscripción**de Azure. 
    3. Seleccione su **cuenta de Azure Storage**. 
    4. Escriba un **nombre para el contenedor** que almacena los datos de salida de la consulta de Stream Analytics.
    5. Seleccione **Guardar**.

        ![Configuración de salida de Blob Storage](./media/event-hubs-kafka-stream-analytics/output-blob-settings.png)
 

### <a name="define-a-query"></a>Definición de una consulta
Ya tiene un trabajo de Stream Analytics configurado para leer un flujo de datos de entrada. El siguiente paso consiste en crear una transformación que analice los datos en tiempo real. Definirá la consulta de transformación mediante el [lenguaje de consulta de Stream Analytics](https://msdn.microsoft.com/library/dn834998.aspx). En este tutorial, definirá una consulta que se pasa a través de los datos sin realizar ninguna transformación.

1. Seleccione **Consulta**.
2. En la ventana de consulta, reemplace `[YourOutputAlias]` por el alias de salida que creó anteriormente.
3. Reemplace `[YourInputAlias]` por el alias de entrada que creó anteriormente. 
4. Seleccione **Guardar** en la barra de herramientas. 

    ![Consultar](./media/event-hubs-kafka-stream-analytics/query.png)


### <a name="run-the-stream-analytics-job"></a>Ejecución del trabajo de Stream Analytics

1. Seleccione **Información general** en el menú de la izquierda. 
2. Seleccione **Iniciar**. 

    ![Menú Inicio](./media/event-hubs-kafka-stream-analytics/start-menu.png)
1. En la página **Iniciar trabajo**, seleccione **Iniciar**. 

    ![Página Iniciar trabajo](./media/event-hubs-kafka-stream-analytics/start-job-page.png)
1. Espere hasta que el estado del trabajo cambie de **Iniciando** a **En ejecución**. 

    ![Estado del trabajo: en ejecución](./media/event-hubs-kafka-stream-analytics/running.png)

## <a name="test-the-scenario"></a>Probar el escenario
1. Vuelva a ejecutar el **productor de Kafka** para enviar eventos al centro de eventos. 

    ```shell
    mvn exec:java -Dexec.mainClass="TestProducer"                                    
    ```
1. Confirme que ve que **los datos de salida** se generan en **Azure Blob Storage**. Consulte un archivo JSON en el contenedor con 100 filas con un aspecto similar a las siguientes filas de ejemplo: 

    ```
    {"eventData":"Test Data 0","EventProcessedUtcTime":"2018-08-30T03:27:23.1592910Z","PartitionId":0,"EventEnqueuedUtcTime":"2018-08-30T03:27:22.9220000Z"}
    {"eventData":"Test Data 1","EventProcessedUtcTime":"2018-08-30T03:27:23.3936511Z","PartitionId":0,"EventEnqueuedUtcTime":"2018-08-30T03:27:22.9220000Z"}
    {"eventData":"Test Data 2","EventProcessedUtcTime":"2018-08-30T03:27:23.3936511Z","PartitionId":0,"EventEnqueuedUtcTime":"2018-08-30T03:27:22.9220000Z"}
    ```

    El trabajo de Azure Stream Analytics recibió los datos de entrada del centro de eventos y los almacenó en Azure Blob Storage en este escenario. 



## <a name="next-steps"></a>Pasos siguientes
En este artículo se muestra cómo hacer streaming a instancias de Event Hubs habilitadas para Kafka sin cambiar los clientes del protocolo o ejecutar sus propios clústeres. Para más información, continúe con el siguiente tutorial:

> [!div class="nextstepaction"]
> [Uso de Kafka MirrorMaker con Event Hubs](event-hubs-kafka-mirror-maker-tutorial.md)
