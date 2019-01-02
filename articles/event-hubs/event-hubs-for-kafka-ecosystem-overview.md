---
title: 'Uso del centro de eventos desde una aplicación de Apache Kafka: Azure Event Hubs | Microsoft Docs'
description: En este artículo se proporciona información sobre la compatibilidad de Apache Kafka con Azure Event Hubs.
services: event-hubs
documentationcenter: .net
author: basilhariri
manager: timlt
ms.service: event-hubs
ms.topic: article
ms.custom: seodec18
ms.date: 12/06/2018
ms.author: bahariri
ms.openlocfilehash: 1c484b6418fd5a453713bc464fff690ef444f109
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/08/2018
ms.locfileid: "53091193"
---
# <a name="use-azure-event-hubs-from-apache-kafka-applications"></a>Uso de Azure Event Hubs desde aplicaciones de Apache Kafka
Event Hubs proporciona un punto de conexión de Kafka que las aplicaciones basadas en Kafka existentes pueden usar como alternativa a la ejecución de su propio clúster de Kafka. Event Hubs admite el [protocolo de Apache Kafka 1.0 y versiones más recientes](https://kafka.apache.org/documentation/) y funciona con las aplicaciones existentes de Kafka, incluida MirrorMaker.  

## <a name="what-does-event-hubs-for-kafka-provide"></a>¿Qué proporciona Event Hubs para Kafka?

La característica Event Hubs para Kafka proporciona un protocolo principal basado en Azure Event Hubs que es compatible a nivel binario con la versión 1.0 de Kafka y posteriores, tanto para leer desde temas de Kafka como para escribir en ellos. Puede empezar a utilizar el punto de conexión de Kafka desde sus aplicaciones sin tener que realizar ningún cambio en el código, pero será preciso realizar un cambio mínimo en la configuración. Actualice la cadena de conexión en las configuraciones para que apunte al punto de conexión de Kafka que se expone en el evento, en lugar de apuntar al clúster de Kafka. A partir de ahí, puede empezar a hacer streaming de los eventos desde las aplicaciones que usen el protocolo Kafka en Event Hubs. Esta integración también admite marcos como [Kafka Connect](https://github.com/Azure/azure-event-hubs-for-kafka/tree/master/tutorials/connect), que está en versión preliminar actualmente. 

Desde un punto de vista conceptual, Kafka y Event Hubs son casi idénticos: ambos son registros con particiones creados para transmitir datos. En la tabla siguiente, se asignan conceptos entre Kafka y Event Hubs.

### <a name="kafka-and-event-hub-conceptual-mapping"></a>Asignación conceptual de Kafka y Event Hubs

| Concepto de Kafka | Concepto de Event Hubs|
| --- | --- |
| Clúster | Espacio de nombres |
| Tema | Event Hubs |
| Partition | Partition|
| Grupo de consumidores | Grupo de consumidores |
| Offset | Offset|

### <a name="key-differences-between-kafka-and-event-hubs"></a>Diferencias principales entre Kafka y Event Hubs

Mientras que [Kafka Apache](https://kafka.apache.org/) es un software que se puede ejecutar siempre que elija, Event Hubs es un servicio en la nube similar a Azure Blob Storage. No hay ningún servidor o red que administrar ni ningún agente que configurar. Se crea un espacio de nombres, que es un FQDN en el que están los temas, y después se crean Event Hubs o temas dentro de ese espacio de nombres. Para más información acerca de Event Hubs y de los espacios de nombres, consulte las [características de Event Hubs](event-hubs-features.md#namespace). Como un servicio en la nube, Event Hubs usa una única dirección IP virtual estable como punto de conexión, de forma que los clientes no necesitan conocer los agentes o equipos de un clúster. 

La escala en Event Hubs se controla por el número de unidades de rendimiento que adquiere; cada unidad de rendimiento le otorga 1 MB por segundo o 1000 eventos por segundo de entrada. De forma predeterminada, Event Hubs escala verticalmente las unidades de procesamiento cuando se alcanza el límite con la característica [Inflado automático](event-hubs-auto-inflate.md); esta característica también funciona con la característica Event Hubs para Kafka. 

### <a name="security-and-authentication"></a>Seguridad y autenticación

Azure Event Hubs requiere SSL o TLS para todas las comunicaciones y usa firmas de acceso compartido (SAS) para la autenticación. Este requisito también es válido para un punto de conexión de Kafka de Event Hubs. Por compatibilidad con Kafka, Event Hubs usa SASL PLAIN para la autenticación y SASL SSL para la seguridad de transporte. Para obtener más información sobre la seguridad en Event Hubs, vea [Autenticación y seguridad de Event Hubs](event-hubs-authentication-and-security-model-overview.md).

## <a name="other-event-hubs-features-available-for-kafka"></a>Otras características de Event Hubs disponibles para Kafka

La característica Event Hubs para Kafka le permite escribir con un protocolo y leer con otro, para que los productores de Kafka actuales puedan seguir publicando a través de Kafka y usted pueda agregar lectores con Event Hubs, como Azure Stream Analytics o Azure Functions. Además, las características de Event Hubs como [Capture](event-hubs-capture-overview.md) y [Recuperación ante desastres geográfica](event-hubs-geo-dr.md) también funcionan con la característica Event Hubs para Kafka.

## <a name="features-that-are-not-yet-supported"></a>Características que aún no se admiten 

A continuación, se incluye la lista de características de Kafka que aún no se admiten:

*   Productor idempotente
*   Transacción
*   Compresión
*   Retención basada en el tamaño
*   Compactación del registro
*   Adición de particiones a un tema existente
*   Compatibilidad con HTTP Kafka API
*   Kafka Streams

## <a name="next-steps"></a>Pasos siguientes

En este artículo se proporciona una introducción a Event Hubs para Kafka. Para obtener más información, consulte los siguientes vínculos:

- [How to create Kafka enabled Event Hubs](event-hubs-create-kafka-enabled.md) (Cómo crear Event Hubs habilitados para Kafka)
- [Stream into Event Hubs from your Kafka applications](event-hubs-quickstart-kafka-enabled-event-hubs.md) (Transmitir en Event Hubs desde sus aplicaciones de Kafka)
- [Creación de un reflejo de un agente de Kafka en un centro de eventos habilitado para Kafka](event-hubs-kafka-mirror-maker-tutorial.md)
- [Conexión de Apache Spark a un centro de eventos habilitado para Kafka](event-hubs-kafka-spark-tutorial.md)
- [Conexión de Apache Flink a un centro de eventos habilitado para Kafka](event-hubs-kafka-flink-tutorial.md)
- [Integración de Kafka Connect con un centro de eventos habilitado para Kafka](event-hubs-kafka-connect-tutorial.md)
- [Conexión de Akka Streams a un centro de eventos habilitado para Kafka](event-hubs-kafka-akka-streams-tutorial.md)
- [Exploración de ejemplos en nuestro GitHub](https://github.com/Azure/azure-event-hubs-for-kafka)

 
 

