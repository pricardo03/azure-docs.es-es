---
title: Streaming en Azure Event Hubs para Apache Kafka | Microsoft Docs
description: Streaming en Event Hubs mediante el protocolo y las API de Kafka.
services: event-hubs
documentationcenter: ''
author: basilhariri
manager: timlt
ms.service: event-hubs
ms.devlang: na
ms.topic: quickstart
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/01/2018
ms.author: bahariri
ms.openlocfilehash: 90d9f3620f954da42add08a0aebf779a95c7e7a3
ms.sourcegitcommit: 974c478174f14f8e4361a1af6656e9362a30f515
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/20/2018
ms.locfileid: "42023031"
---
# <a name="stream-into-event-hubs-for-the-apache-kafka"></a>Streaming en Event Hubs para Apache Kafka
En esta guía de inicio rápido se muestra cómo transmitir a instancias de Event Hubs habilitadas para Kafka sin cambiar los clientes del protocolo o ejecutar sus propios clústeres. Va a aprender a usar sus productores y consumidores para hablar con instancias de Event Hubs habilitadas para Kafka con un simple cambio de configuración en sus aplicaciones. Azure Event Hubs admite [Apache Kafka versión 1.0.](https://kafka.apache.org/10/documentation.html)

> [!NOTE]
> Este ejemplo está disponible en [GitHub](https://github.com/Azure/azure-event-hubs).

## <a name="prerequisites"></a>Requisitos previos

Para completar esta guía de inicio rápido, asegúrese de cumplir los siguientes requisitos previos:

* Lea el artículo [Event Hubs para Apache Kafka](event-hubs-for-kafka-ecosystem-overview.md).
* Una suscripción de Azure. Si no tiene una, cree una [cuenta gratuita](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) antes de empezar.
* [Kit de desarrollo de Java (JDK) 1.7+](http://www.oracle.com/technetwork/java/javase/downloads/index.html).
* [Descargue](http://maven.apache.org/download.cgi) e [instale](http://maven.apache.org/install.html) un archivo binario de Maven.
* [Git](https://www.git-scm.com/)
* [Un espacio de nombres de Event Hubs habilitado para Kafka](event-hubs-create.md)

## <a name="create-a-kafka-enabled-event-hubs-namespace"></a>Creación de un espacio de nombres de Event Hubs habilitado para Kafka

1. Inicie sesión en [Azure Portal][Azure Portal] y haga clic en **Crear un recurso** en la parte superior izquierda de la pantalla.

2. Busque Event Hubs y seleccione las opciones que se muestran aquí:
    
    ![Búsqueda de Event Hubs en el portal](./media/event-hubs-create-kafka-enabled/event-hubs-create-event-hubs.png)
 
3. Proporcione un nombre único y habilite a Kafka en el espacio de nombres. Haga clic en **Create**(Crear).
    
    ![Creación de un espacio de nombres](./media/event-hubs-create-kafka-enabled/create-kafka-namespace.png)
 
4. Una vez que se crea el espacio de nombres, en la pestaña **Configuración**, haga clic en **Directivas de acceso compartido** para obtener la cadena de conexión.

    ![Haga clic en Directivas de acceso compartido](./media/event-hubs-create/create-event-hub7.png)

5. Puede elegir la directiva **RootManageSharedAccessKey** predeterminada o agregar una nueva directiva. Haga clic en el nombre de la directiva y copie la cadena de conexión. 
    
    ![Selección de una directiva](./media/event-hubs-create/create-event-hub8.png)
 
6. Agregue esta cadena de conexión a la configuración de la aplicación de Kafka.

Ahora puede transmitir eventos desde las aplicaciones que usan el protocolo de Kafka en Event Hubs.

## <a name="send-and-receive-messages-with-kafka-in-event-hubs"></a>Envío y recepción de mensajes con Kafka en Event Hubs

1. Clone el [repositorio de Azure Event Hubs](https://github.com/Azure/azure-event-hubs).

2. Vaya a `azure-event-hubs/samples/kafka/quickstart/producer`.

3. Actualice los detalles de configuración para el productor en `src/main/resources/producer.config` de la siguiente manera:

    ```xml
    bootstrap.servers={YOUR.EVENTHUBS.FQDN}:9093
    security.protocol=SASL_SSL
    sasl.mechanism=PLAIN
    sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="$ConnectionString" password="{YOUR.EVENTHUBS.CONNECTION.STRING}";
    ```
4. Ejecute el código de productor y transmítalo a Event Hubs habilitado para Kafka:
   
    ```shell
    mvn clean package
    mvn exec:java -Dexec.mainClass="TestProducer"                                    
    ```
5. Vaya a `azure-event-hubs/samples/kafka/quickstart/consumer`.

6. Actualice los detalles de configuración para el consumidor en `src/main/resources/consumer.config` de la siguiente manera:
   
    ```xml
    bootstrap.servers={YOUR.EVENTHUBS.FQDN}:9093
    security.protocol=SASL_SSL
    sasl.mechanism=PLAIN
    sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="$ConnectionString" password="{YOUR.EVENTHUBS.CONNECTION.STRING}";
    ```

7. Ejecute el código de consumidor y procéselo desde Event Hubs habilitado para Kafka mediante los clientes de Kafka:

    ```java
    mvn clean package
    mvn exec:java -Dexec.mainClass="TestConsumer"                                    
    ```

Si el clúster de Kafka de Event Hubs tiene eventos, ahora comenzará a recibirlos desde el consumidor.

## <a name="next-steps"></a>Pasos siguientes
En este artículo se muestra cómo hacer streaming a instancias de Event Hubs habilitadas para Kafka sin cambiar los clientes del protocolo o ejecutar sus propios clústeres. Para más información, continúe con el siguiente tutorial:

> [!div class="nextstepaction"]
> [Uso de Kafka MirrorMaker con Event Hubs](event-hubs-kafka-mirror-maker-tutorial.md)
