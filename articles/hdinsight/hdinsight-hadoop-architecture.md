---
title: Arquitectura de Hadoop en Azure HDInsight
description: Describe el procesamiento y almacenamiento Hadoop en clústeres HDInsight.
services: hdinsight
author: ashishthaps
ms.author: ashishth
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 01/19/2018
ms.openlocfilehash: f22cb6a56e0ef81e3d7799b38e33113f8b175457
ms.sourcegitcommit: cb61439cf0ae2a3f4b07a98da4df258bfb479845
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/05/2018
ms.locfileid: "43699437"
---
# <a name="hadoop-architecture-in-hdinsight"></a>Arquitectura de Hadoop en HDInsight

Hadoop incluye dos componentes principales: el Sistema de archivos distribuido de Hadoop (HDFS), que proporciona almacenamiento, y Yet Another Resource Negotiator (YARN), que proporciona procesamiento. Con las capacidades de almacenamiento y procesamiento, un clúster puede ejecutar programas MapReduce para realizar el procesamiento de datos deseado.

> [!NOTE]
> Normalmente, HDFS no se implementa dentro del clúster HDInsight para proporcionar almacenamiento. En su lugar, los componentes de Hadoop utilizan una capa de interfaz compatible con HDFS. La capacidad de almacenamiento real la proporcionan Azure Storage o Azure Data Lake Store. Para Hadoop, los trabajos MapReduce que se ejecutan en el clúster HDInsight, lo hacen como si HDFS estuviese presente y no requieren cambios para satisfacer sus necesidades de almacenamiento. En Hadoop en HDInsight, el almacenamiento es externo, pero el procesamiento YARN sigue siendo un componente principal. Para más información, consulte [Introducción a Azure HDInsight](hadoop/apache-hadoop-introduction.md).

En este artículo se introduce YARN y cómo coordina la ejecución de aplicaciones en HDInsight.

## <a name="yarn-basics"></a>Conceptos básicos de YARN 

YARN rige y organiza el procesamiento de datos en Hadoop. YARN tiene dos servicios principales que se ejecutan como procesos en nodos del clúster: 

* ResourceManager 
* NodeManager

ResourceManager concede recursos de proceso de clúster a aplicaciones, como trabajos MapReduce. ResourceManager concede estos recursos como contenedores, y cada contenedor se compone de una asignación de núcleos de CPU y memoria RAM. Si se combinan todos los recursos disponibles en un clúster y, después, los núcleos y la memoria se distribuyen en bloques, cada bloque de recursos es un contenedor. Cada nodo del clúster tiene capacidad para un determinado número de contenedores, por lo tanto, el clúster tiene un límite fijo de número de contenedores disponibles. La asignación de recursos en un contenedor es configurable. 

Cuando se ejecuta una aplicación MapReduce en un clúster, ResourceManager proporciona a la aplicación los contenedores en los que se puede ejecutar. ResourceManager realiza un seguimiento del estado de las aplicaciones en ejecución y la capacidad de clúster disponible, y realiza un seguimiento de cuándo se completan las aplicaciones y liberan sus recursos. 

ResourceManager también ejecuta un proceso de servidor web que proporciona una interfaz de usuario web para supervisar el estado de las aplicaciones.

Cuando un usuario envía una aplicación MapReduce para que se ejecute en el clúster, la aplicación se envía a ResourceManager. A su vez, ResourceManager asigna un contenedor en nodos de NodeManager disponibles. Los nodos de NodeManager son la ubicación en que realmente se ejecuta la aplicación. El primer contenedor asignado ejecuta una aplicación especial denominada ApplicationMaster. ApplicationMaster es responsable de adquirir recursos, en forma de contenedores subsiguientes, necesarios para ejecutar la aplicación enviada. ApplicationMaster examina las fases de la aplicación, como la fase de asignación y la de reducción, y considera la cantidad de datos que deben procesarse. A continuación, ApplicationMaster solicita (*negocia*) los recursos de ResourceManager en nombre de la aplicación. ResourceManager concede, a su vez, recursos de NodeManagers del clúster a ApplicationMaster para que los use al ejecutar la aplicación. 

Los servicios NodeManager ejecutan las tareas que componen la aplicación y, a continuación, informan de su progreso y estado a ApplicationMaster. ApplicationMaster, a su vez, informa del estado de la aplicación a ResourceManager. ResourceManager devuelve todos los resultados al cliente.

## <a name="yarn-on-hdinsight"></a>YARN en HDInsight

Todos los tipos de clúster HDInsight implementan YARN. ResourceManager se implementa para lograr alta disponibilidad con una instancia principal y secundaria, que se ejecutan en el primer y segundo nodo principal dentro del clúster, respectivamente. Solo la única instancia de ResourceManager está activa a la vez. Las instancias de NodeManager se ejecutan en todos los nodos de trabajo disponibles en el clúster.

![YARN en HDInsight](./media/hdinsight-hadoop-architecture/yarn-on-hdinsight.png)

## <a name="next-steps"></a>Pasos siguientes

* [Uso de MapReduce en Hadoop en HDInsight](hadoop/hdinsight-use-mapreduce.md)
* [Introducción a Azure HDInsight](hadoop/apache-hadoop-introduction.md)
