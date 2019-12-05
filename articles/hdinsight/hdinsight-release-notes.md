---
title: Notas de la versión de Azure HDInsight.
description: Notas más recientes de la versión de Azure HDInsight. Obtenga sugerencias de desarrollo y detalles sobre Hadoop, Spark, R Server, Hive, etc.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.service: hdinsight
ms.topic: conceptual
ms.date: 08/08/2019
ms.openlocfilehash: 54e28a9d434500915aa8cc8e07ade3592e5aa96c
ms.sourcegitcommit: dbde4aed5a3188d6b4244ff7220f2f75fce65ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/19/2019
ms.locfileid: "74185601"
---
# <a name="release-notes"></a>Notas de la versión

Este artículo proporciona información sobre las **últimas** actualizaciones de versión de Azure HDInsight. Para más información sobre versiones anteriores, vea [Notas de la versión (en archivo) de los componentes de Hadoop en Azure HDInsight](hdinsight-release-notes-archive.md).

## <a name="summary"></a>Resumen

Azure HDInsight es uno de los servicios más populares entre los clientes de empresa para el análisis de código abierto de Apache Hadoop y Apache Spark en Azure.

## <a name="release-date-11072019"></a>Fecha de lanzamiento: 07/11/2019

Esta versión se aplica a HDInsight 3.6 y 4.0.

> [!IMPORTANT]  
> Linux es el único sistema operativo que se usa en la versión 3.4 de HDInsight, o en las superiores. Para obtener más información, consulte el [artículo de control de versiones de HDInsight](hdinsight-component-versioning.md).


## <a name="new-features"></a>Nuevas características

### <a name="hdinsight-identity-broker-hib-preview"></a>HDInsight Identity Broker (HIB) (versión preliminar)

HDInsight Identity Broker (HIB) permite a los usuarios iniciar sesión en Apache Ambari mediante Multi-Factor Authentication (MFA) y obtener los vales de Kerberos necesarios sin necesidad de hash de contraseña en Azure Active Directory Domain Services (AAD-DS). Actualmente, HIB solo está disponible para los clústeres implementados mediante la plantilla ARM.

### <a name="kafka-rest-api-proxy-preview"></a>Proxy de API de REST de Kafka (versión preliminar)

El proxy de API de REST de Kafka proporciona una implementación de un solo clic del proxy de REST de alta disponibilidad con un clúster de Kafka mediante la autorización segura de AAD y el protocolo OAuth. 

### <a name="auto-scale"></a>Escalado automático

La escalabilidad automática en Azure HDInsight ya está disponible con carácter general en todas las regiones para los tipos de clúster de Apache Spark y Hadoop. Esta característica permite administrar las cargas de trabajo de análisis de macrodatos de un modo más rentable y productivo. Ahora puede optimizar el uso de los clústeres de HDInsight y pagar solo por lo que necesita.

En función de sus requisitos, puede elegir escalabilidad automática basada en la carga o basada en una programación. La escalabilidad automática basada en la carga puede escalar o reducir verticalmente el tamaño del clúster según las necesidades de recursos en ese momento, mientras que la escalabilidad automática basada en una programación cambia el tamaño del clúster según la programación que se haya definido previamente. 

La escalabilidad automática para cargas de trabajo de HBase y LLAP también está en versión preliminar pública. Para más información, consulte [Escalado automático de clústeres de Azure HDInsight](https://docs.microsoft.com/azure/hdinsight/hdinsight-autoscale-clusters).

### <a name="hdinsight-accelerated-writes-for-apache-hbase"></a>Escrituras aceleradas de HDInsight para Apache HBase 

Escrituras aceleradas usa discos administrados SSD premium de Azure para mejorar el rendimiento del registro de escritura previa (WAL) de Apache HBase. Para más información, consulte [Escrituras aceleradas de Azure HDInsight para Apache HBase](https://docs.microsoft.com/azure/hdinsight/hbase/apache-hbase-accelerated-writes).

### <a name="custom-ambari-db"></a>Base de datos de Ambari personalizada

HDInsight ofrece ahora una nueva capacidad para permitir que los usuarios usen su propia instancia de SQL DB para Ambari. Ahora los clientes pueden elegir la instancia correcta de SQL DB para Ambari y actualizarla con facilidad en función de su propio requisito de crecimiento empresarial. La implementación se realiza mediante una plantilla de Azure Resource Manager. Para más información, consulte [Configuración de clústeres de HDInsight con una base de datos de Ambari personalizada](https://docs.microsoft.com/azure/hdinsight/hdinsight-custom-ambari-db).

### <a name="f-series-virtual-machines-are-now-available-with-hdinsight"></a>Las máquinas virtuales de la serie F están ahora disponibles con HDInsight

Las máquinas virtuales (VM) de la serie F son una buena opción para empezar a trabajar con HDInsight con requisitos de procesamiento ligeros. Con un precio en lista por hora inferior, la serie F tiene la mejor relación precio/rendimiento en la cartera de Azure en base a la unidad de Azure Compute (ACU) por vCPU. Para más información, consulte [Selección del tamaño de VM correcto para el clúster de Azure HDInsight](https://docs.microsoft.com/azure/hdinsight/hdinsight-selecting-vm-size).

## <a name="deprecation"></a>Desuso

### <a name="g-series-virtual-machine-deprecation"></a>Desuso de las máquinas virtuales de la serie G
A partir de esta versión, ya no están disponibles las máquinas virtuales de la serie G en HDInsight.

### <a name="dv1-virtual-machine-deprecation"></a>Desuso de la máquina virtual Dv1
A partir de esta versión, han quedado en desuso las máquinas virtuales Dv1 con HDInsight. Cuando los clientes soliciten máquinas virtuales Dv1, se servirán automáticamente máquinas virtuales Dv2. No hay diferencia de precio entre las máquinas virtuales Dv1 y Dv2.

## <a name="behavior-changes"></a>Cambios de comportamiento

### <a name="cluster-managed-disk-size-change"></a>Cambio de tamaño del disco administrado del clúster
HDInsight proporciona espacio en disco administrado con el clúster. A partir de esta versión, el tamaño de disco administrado de cada nodo del nuevo clúster creado se cambia a 128 GB.

## <a name="upcoming-changes"></a>Próximos cambios
En las próximas versiones, se realizarán los siguientes cambios. 

### <a name="moving-to-azure-virtual-machine-scale-sets"></a>Movimiento a conjuntos de escalado de máquinas virtuales
Ahora HDInsight usa máquinas virtuales de Azure para aprovisionar el clúster. A partir de diciembre, HDInsight usará los conjuntos de escalado de máquinas virtuales de Azure en su lugar. Consulte más información sobre los [conjuntos de escalado de máquinas virtuales de Azure](https://docs.microsoft.com/azure/virtual-machine-scale-sets/overview).

### <a name="hbase-20-to-21"></a>HBase 2.0 a 2.1
En la próxima versión de HDInsight 4.0, la versión de HBase se actualizará de 2.0 a 2.1.

### <a name="a-series-virtual-machine-deprecation-for-esp-cluster"></a>Desuso de las máquinas virtuales de la serie A para el clúster de ESP
Las máquinas virtuales de la serie A pueden provocar problemas en el clúster de ESP debido a su relativa poca capacidad de CPU y memoria. En la próxima versión, las máquinas virtuales de la serie A quedarán en desuso para crear nuevos clústeres de ESP.

## <a name="bug-fixes"></a>Corrección de errores
HDInsight continúa realizando mejoras en la confiabilidad y el rendimiento del clúster. 

## <a name="component-version-change"></a>Cambio de versión de componentes
No hay cambio de versión de componentes para esta versión. Puede encontrar las versiones actuales de los componentes para HDInsight 4.0 y HDInsight 3.6 [aquí](https://docs.microsoft.com/azure/hdinsight/hdinsight-component-versioning).
