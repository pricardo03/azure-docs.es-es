---
title: ¿Qué es Azure Event Hubs? | Microsoft Docs
description: Obtenga más información sobre Azure Event Hubs, un servicio de streaming de macrodatos que ingiere millones de eventos por segundo.
services: event-hubs
documentationcenter: na
author: ShubhaVijayasarathy
manager: timlt
ms.service: event-hubs
ms.topic: overview
ms.custom: mvc
ms.date: 08/01/2018
ms.author: shvija
ms.openlocfilehash: 8437b1c10facc28c5fd71b70dd7acf01b7d39e8e
ms.sourcegitcommit: 387d7edd387a478db181ca639db8a8e43d0d75f7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/10/2018
ms.locfileid: "42023752"
---
# <a name="what-is-azure-event-hubs"></a>¿Qué es Azure Event Hubs?

Azure Event Hubs es una plataforma de streaming de macrodatos y servicio de ingesta de eventos de gran escalabilidad capaz de recibir y procesar millones de eventos por segundo. Event Hubs puede procesar y almacenar eventos, datos o telemetría generados por dispositivos y software distribuido. Los datos enviados a un centro de eventos se pueden transformar y almacenar con cualquier proveedor de análisis en tiempo real o adaptadores de procesamiento por lotes y almacenamiento. 

Event Hubs se usa en algunos de los siguientes escenarios comunes:

- Detección de anomalías (fraude/valores atípicos)
- Registro de aplicaciones
- Canalizaciones de análisis, como secuencias de clics
- Paneles en vivo
- Archivado de datos
- Procesamiento de transacciones
- Procesamiento de telemetría de usuario
- Streaming de telemetría de dispositivo 

## <a name="why-use-event-hubs"></a>¿Por qué usar Event Hubs?

Los datos solo son valiosos cuando existe una forma sencilla de procesarlos y obtener información oportuna a partir de los orígenes de datos. Event Hubs ofrece una plataforma de procesamiento de secuencias distribuida con baja latencia y una integración perfecta, con servicios de datos y análisis dentro y fuera de Azure para crear una canalización de macrodatos completa.

Event Hubs representa la "puerta principal" de una canalización de eventos, conocida a menudo como un *agente de ingesta de eventos* en las arquitecturas de la solución. Un agente de ingesta de eventos es un componente o servicio que se encuentra entre los publicadores de eventos y los consumidores de eventos para desacoplar la producción de un flujo de eventos del consumo de esos eventos. Event Hubs ofrece una plataforma de streaming unificada con búfer de retención de tiempo, lo que desacopla los productores de eventos de los consumidores de eventos. 

En las secciones siguientes se describen las características clave del servicio Azure Event Hubs: 

## <a name="fully-managed-paas"></a>PaaS completamente administrada 

Event Hubs es un servicio administrado con poca sobrecarga de administración o configuración, para que pueda centrarse en las soluciones empresariales. [Event Hubs para ecosistemas de Apache Kafka](event-hubs-for-kafka-ecosystem-overview.md) le ofrece la experiencia de PaaS de Kafka sin tener que administrar, configurar ni ejecutar los clústeres.

## <a name="support-for-real-time-and-batch-processing"></a>Compatibilidad con procesamiento por lotes y en tiempo real

Ingiera, almacene en búfer y procese la secuencia en tiempo real para obtener información práctica y útil. Event Hubs usa un [modelo de consumidor con particiones](event-hubs-features.md#partitions), que permite que varias aplicaciones procesen la secuencia de manera simultánea y permite controlar la velocidad del procesamiento.

[Capture](event-hubs-capture-overview.md) los datos casi en tiempo real en una instancia de [Azure Blob Storage](https://azure.microsoft.com/services/storage/blobs/) o de [Azure Data Lake Store](https://azure.microsoft.com/services/data-lake-store/) para realizar la retención a largo plazo o el procesamiento por microlotes. Puede lograr esto en la misma secuencia que usa para derivar un análisis en tiempo real. La configuración de Capture es rápida, su ejecución no tiene costes administrativos y se escala automáticamente con las  [unidades de procesamiento](event-hubs-features.md#throughput-units) de Event Hubs. Event Hubs Capture permite centrarse en el procesamiento de datos en lugar de hacerlo en la captura de datos.

Azure Event Hubs también se integra con [Azure Functions](/azure/azure-functions/) para una arquitectura sin servidor.

## <a name="scalable"></a>Escalable 

Con Event Hubs, puede comenzar con los flujos de datos en megabytes y aumentar a gigabytes o terabytes. La característica de [inflado automático](event-hubs-auto-inflate.md) es una de las muchas opciones disponibles para escalar el número de unidades de procesamiento con el fin de satisfacer las necesidades de uso. 

## <a name="rich-ecosystem"></a>Ecosistema enriquecido

[Event Hubs para ecosistemas de Apache Kafka](event-hubs-for-kafka-ecosystem-overview.md) permite que aplicaciones y clientes de [Apache Kafka (1.0 y posteriores)](https://kafka.apache.org/) se comunique con Event Hubs sin que tenga que administrar ningún clúster.
 
Con un ecosistema amplio disponible en diversos [lenguajes (.NET, Java, Python, Go, Node.js)](https://github.com/Azure/azure-event-hubs), puede empezar a procesar fácilmente los flujos desde Event Hubs. Todos los lenguajes de cliente compatibles proporcionan integración de nivel bajo.

## <a name="key-architecture-components"></a>Componentes clave de la arquitectura

Event Hubs proporciona una funcionalidad de control del flujo de mensajes pero tiene características diferentes de la mensajería empresarial tradicional. Las funcionalidades de Event Hubs se basan en un alto rendimiento y en escenarios de procesamiento de eventos. Event Hubs contiene los siguientes [componentes clave](event-hubs-features.md):

- **Productores de eventos**: una entidad que envía datos a un centro de eventos. Los publicadores de eventos pueden publicar eventos mediante HTTPS, AMQP 1.0 o Apache Kafka (1.0 y posterior)
- **Particiones**: cada consumidor solo lee un subconjunto específico, o partición, de la secuencia de mensajes.
- **Grupos de consumidores**: vista (estado, posición o desplazamiento) de todo un centro de eventos. Los grupos de consumidores habilitan varias aplicaciones consumidoras para que cada una tenga una vista separada del flujo de eventos y para que lean el flujo de forma independiente a su propio ritmo y con sus propios desplazamientos.
- **Unidades de procesamiento**: unidades de capacidad compradas previamente que controlan la capacidad de rendimiento de Event Hubs.
- **Receptores de eventos**: cualquier entidad que lea datos de evento de un centro de eventos. Todos los consumidores de Event Hubs se conectan a través de la sesión de AMQP 1.0, y los eventos se entregan a través de la sesión a medida que están disponibles.

La siguiente ilustración muestra la arquitectura de procesamiento del flujo de Event Hubs:

![Event Hubs](./media/event-hubs-about/event_hubs_architecture.png)


## <a name="next-steps"></a>Pasos siguientes

Para empezar a usar Event Hubs, consulte los artículos siguientes:

* [Ingest into Event Hubs](event-hubs-quickstart-portal.md) (Ingesta en Event Hubs)
* [Información general de las características de Event Hubs](event-hubs-features.md)
* [Preguntas más frecuentes](event-hubs-faq.md)


