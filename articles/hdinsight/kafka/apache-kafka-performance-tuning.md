---
title: Optimización del rendimiento para los clústeres de Apache Kafka HDInsight
description: Proporciona información general sobre técnicas para optimizar las cargas de trabajo de Apache Kafka en HDInsight de Azure.
services: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 02/21/2019
ms.openlocfilehash: 3f15f45e0543c582d70463fb9ddc7ac569ff57bc
ms.sourcegitcommit: c63fe69fd624752d04661f56d52ad9d8693e9d56
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2019
ms.locfileid: "58576765"
---
# <a name="performance-optimization-for-apache-kafka-hdinsight-clusters"></a>Optimización del rendimiento para los clústeres de Apache Kafka HDInsight

Este artículo proporcionan algunas sugerencias para optimizar el rendimiento de las cargas de trabajo de Apache Kafka en HDInsight. Es el enfoque sobre el ajuste de productor y configuración de Service broker. Hay diferentes maneras de medir el rendimiento y las optimizaciones que aplicar dependerá de sus necesidades empresariales.

## <a name="architecture-overview"></a>Introducción a la arquitectura

Los temas de Kafka se utilizan para organizar los registros. Registros son generados por los productores y consumidores reciben los. Los productores envían registros a los agentes de Kafka, que, a continuación, almacenarán los datos. Cada nodo de trabajo del clúster de HDInsight es un agente de Kafka.

Los temas particionan los registros entres los agentes. Al consumir registros, puede usar hasta un consumidor por partición para lograr el procesamiento en paralelo de los datos.

La replicación se utiliza para duplicar las particiones entre nodos. Esto protege contra las desconexiones de nodos (agente). Una sola partición entre el grupo de réplicas se designa como el líder de la partición. El tráfico del productor se enruta al líder de cada nodo, con el estado administrado por ZooKeeper.

## <a name="identify-your-scenario"></a>Identificación del escenario

Rendimiento de Apache Kafka tiene dos aspectos principales: rendimiento y latencia. Rendimiento es la velocidad máxima a la que se pueden procesar los datos. Un mayor rendimiento es mejor. La latencia es el tiempo necesario para que los datos que se almacena o recupera. Una latencia menor es mejor. Puede resultar complicado encontrar el equilibrio correcto entre el rendimiento, latencia y el costo de infraestructura de la aplicación. Los requisitos de rendimiento es probable que coincidirá con uno de las siguientes tres situaciones comunes, en función de si requiere un alto rendimiento, baja latencia o ambos:

* Alto rendimiento, baja latencia. Este escenario requiere un alto rendimiento y baja latencia (~ 100 milisegundos). Un ejemplo de este tipo de aplicación es servicio de supervisión de disponibilidad.
* Alto rendimiento, alta latencia. Este escenario requiere un rendimiento alto (~1.5 GBps), pero puede tolerar una latencia mayor (< 250 ms). Un ejemplo de este tipo de aplicación es la ingesta de datos de telemetría para cerca de los procesos en tiempo real, como aplicaciones de detección de intrusiones y seguridad.
* Bajo rendimiento, baja latencia. Este escenario requiere una latencia baja (< 10 ms) para el procesamiento en tiempo real, pero puede tolerar un menor rendimiento. Un ejemplo de este tipo de aplicación es comprobaciones de ortografía y gramática en línea.

## <a name="producer-configurations"></a>Configuraciones de productor

Las secciones siguientes resaltan algunas de las propiedades de configuración más importantes para optimizar el rendimiento de los productores de Kafka. Para obtener una explicación detallada de todas las propiedades de configuración, consulte [documentación de Apache Kafka en configuraciones de productor](https://kafka.apache.org/documentation/#producerconfigs).

### <a name="batch-size"></a>Tamaño de lote

Los productores de Apache Kafka reunir grupos de mensajes (denominados lotes) que se envían como una unidad que se almacenará en una partición de almacenamiento único. Tamaño del lote, significa que el número de bytes que deben estar presentes antes de que se transmite a ese grupo. Aumentar el `batch.size` parámetro puede aumentar el rendimiento, ya que reduce el procesamiento de sobrecarga de red y las solicitudes de E/S. Bajo una carga ligera, tamaño de lote mayor puede aumentar la latencia de envío de Kafka como el productor espera a que un lote estar preparado. Con mucha carga, se recomienda para aumentar el tamaño del lote para mejorar el rendimiento y latencia.

### <a name="producer-required-acknowledgements"></a>Confirmaciones de productor necesario

El productor necesario `acks` configuración determina el número de confirmaciones requeridas por el líder de la partición antes de que una solicitud de escritura se considera completado. Esta configuración afecta a la confiabilidad de los datos y toma los valores de `0`, `1`, o `-1`. El valor de `-1` significa que se debe recibir una confirmación desde todas las réplicas antes de que finalice la operación de escritura. Establecer `acks = -1` proporciona mayores garantías contra la pérdida de datos, pero también los resultados en una latencia mayor y menor rendimiento. Si los requisitos de su aplicación exigen un mayor rendimiento, pruebe a establecer `acks = 0` o `acks = 1`. Tenga en cuenta, que puede reducir la confiabilidad de los datos no reconocer todas las réplicas.

### <a name="compression"></a>Compresión

Un productor de Kafka puede configurarse para comprimir los mensajes antes de enviarlos a los agentes. El `compression.type` configuración especifica el códec de compresión que se usará. Códecs de compresión admitidos son "gzip", "snappy" y "lz4." La compresión es beneficiosa y se debe considerar si hay una limitación en la capacidad de disco.

Entre los dos códecs de compresión frecuente, `gzip` y `snappy`, `gzip` tiene una mayor proporción de compresión, que da como resultado una menor uso de disco a costa de una carga más elevada de CPU. El `snappy` códec proporciona una compresión menos con menos sobrecarga de CPU. Puede decidir qué códec para usar en función de limitaciones de CPU de disco o el productor de broker. `gzip` puede comprimir los datos a una velocidad cinco veces mayor que `snappy`.

Usar la compresión de datos, aumentará el número de registros que se pueden almacenar en un disco. También puede aumentar CPU sobrecarga en casos donde hay una discrepancia entre los formatos de compresión, el productor y el agente usando. como los datos se deben comprimir antes de enviar y luego descomprimirlos antes del procesamiento.

## <a name="broker-settings"></a>Configuración del agente

Las secciones siguientes resaltan algunas de las opciones más importantes para optimizar el rendimiento de los agentes de Kafka. Para obtener una explicación detallada de todos los valores de broker, vea [documentación de Apache Kafka en configuraciones de productor](https://kafka.apache.org/documentation/#producerconfigs).


### <a name="number-of-disks"></a>Número de discos

Discos de almacenamiento limitada (entrada/salida operaciones por segundo) de e/s por segundo y bytes por segundo de lectura/escritura. Al crear nuevas particiones, Kafka almacena cada nueva partición en el disco con el menor número de las particiones existentes para el equilibrio entre ellos a través de los discos disponibles. A pesar de la estrategia de almacenamiento, al procesar cientos de réplicas de particiones en cada disco, Kafka puede saturar fácilmente el rendimiento de disco disponible. Es aquí el equilibrio entre rendimiento y costo. Si la aplicación requiere un rendimiento mayor, crear un clúster con más administrada discos por agente. HDInsight no admite actualmente la adición de discos administrados a un clúster en ejecución. Para obtener más información sobre cómo configurar el número de discos administrados, consulte [configurar el almacenamiento y la escalabilidad de Apache Kafka en HDInsight](apache-kafka-scalability.md). Entender las implicaciones de coste de creciente espacio de almacenamiento para los nodos del clúster.

### <a name="number-of-topics-and-partitions"></a>Número de temas y particiones

Los productores Kafka escriben en los temas. Los consumidores de Kafka se leen de temas. Un tema está asociado con un registro, que es una estructura de datos en el disco. Kafka anexa al final del registro de un tema de los registros desde un productor, productores. Un registro de tema consta del número de particiones que se distribuyen en varios archivos. Estos archivos a su vez, se, distribuyen entre varios nodos de clúster de Kafka. Los consumidores leen los temas de Kafka a su ritmo y pueden elegir su posición (desplazamiento) en el registro de tema.

Cada partición de Kafka es un archivo de registro en el sistema y subprocesos de productor pueden escribir simultáneamente en varios registros. De forma similar, ya que cada subproceso de consumidor lee mensajes de una partición, consumo de varias particiones se controla en paralelo también.

Aumentar la densidad de la partición (el número de particiones por broker) agrega una sobrecarga relacionada con las operaciones de metadatos y por partición solicitud/respuesta entre el líder de la partición y sus seguidores. Incluso en ausencia de datos que fluyen a través, réplicas de la partición aún capturar datos de los líderes, lo que da como resultado un procesamiento adicional para enviar y recibir solicitudes a través de la red.

Clústeres de Apache Kafka de 1.1 y anteriormente en HDInsight, se recomienda tener un máximo de 1000 particiones por agente, incluidas las réplicas. Aumentar el número de particiones por broker reduce el rendimiento y también puede provocar la falta de disponibilidad de tema. Para obtener más información sobre la compatibilidad con particiones de Kafka, consulte [la entrada de blog oficial de Apache Kafka en el aumento del número de particiones compatibles en la versión 1.1.0](https://blogs.apache.org/kafka/entry/apache-kafka-supports-more-partitions). Para obtener más información acerca de cómo modificar temas, consulte [Apache Kafka: modificar temas](https://kafka.apache.org/documentation/#basic_ops_modify_topic).

### <a name="number-of-replicas"></a>Número de réplicas

Factor de replicación mayor da como resultado solicitudes adicionales entre el líder de la partición y seguidores. Por lo tanto, un factor de replicación más alto consume más disco y CPU para controlar las solicitudes adicionales, latencia y reduce el rendimiento de escritura de aumentar.

Se recomienda utilizar la replicación de al menos 3 x para Kafka en HDInsight de Azure. Regiones de Azure más tienen tres dominios de error, pero en regiones con solo dos dominios de error, los usuarios deben utilizar la replicación de 4 x.

Para obtener más información sobre la replicación, vea [Apache Kafka: replicación](https://kafka.apache.org/documentation/#replication) y [Apache Kafka: aumentar el factor de replicación](https://kafka.apache.org/documentation/#basic_ops_increase_replication_factor).

## <a name="next-steps"></a>Pasos siguientes

* [Procesamiento de billones de eventos al día con Apache Kafka en Azure](https://azure.microsoft.com/blog/processing-trillions-of-events-per-day-with-apache-kafka-on-azure/)
* [Qué es Apache Kafka en HDInsight](apache-kafka-introduction.md)
