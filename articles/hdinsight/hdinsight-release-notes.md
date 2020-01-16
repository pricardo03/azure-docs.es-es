---
title: Notas de la versión de Azure HDInsight.
description: Notas más recientes de la versión de Azure HDInsight. Obtenga sugerencias de desarrollo y detalles sobre Hadoop, Spark, R Server, Hive, etc.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.service: hdinsight
ms.topic: conceptual
ms.date: 12/17/2019
ms.openlocfilehash: 2abdae95e14ecc9dab673216a2c6aef652915988
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/25/2019
ms.locfileid: "75435455"
---
# <a name="release-notes"></a>Notas de la versión

Este artículo proporciona información sobre las **últimas** actualizaciones de versión de Azure HDInsight. Para más información sobre versiones anteriores, vea [Notas de la versión (en archivo) de los componentes de Hadoop en Azure HDInsight](hdinsight-release-notes-archive.md).

## <a name="summary"></a>Resumen

Azure HDInsight es uno de los servicios más populares entre los clientes de empresa para el análisis de código abierto de Apache Hadoop y Apache Spark en Azure.

## <a name="release-date-12172019"></a>Fecha de lanzamiento: 17/12/2019

Esta versión se aplica a HDInsight 3.6 y 4.0.

> [!IMPORTANT]  
> Linux es el único sistema operativo que se usa en la versión 3.4 de HDInsight, o en las superiores. Para obtener más información, consulte el [artículo de control de versiones de HDInsight](hdinsight-component-versioning.md).


## <a name="new-features"></a>Nuevas características

### <a name="service-tags"></a>Etiquetas de servicio
Las etiquetas de servicio simplifican la seguridad de las máquinas virtuales y las redes virtuales de Azure, permitiéndole restringir fácilmente el acceso de red a los servicios de Azure. Puede usar etiquetas de servicio en las reglas del grupo de seguridad de red (NSG) para permitir o denegar el tráfico a un servicio específico de Azure, globalmente o por región de Azure. Azure proporciona el mantenimiento de las direcciones IP subyacentes a cada etiqueta. Las etiquetas de servicio de HDInsight de los grupos de seguridad de red (NSG) son grupos de direcciones IP para los servicios de mantenimiento y administración. Estos grupos ayudan a minimizar la complejidad de la creación de reglas de seguridad. Los clientes de HDInsight pueden habilitar la etiqueta de servicio mediante Azure Portal, PowerShell y la API REST. Para más información, consulte [Etiquetas de servicio del grupo de seguridad de red (NSG) para Azure HDInsight](https://docs.microsoft.com/azure/hdinsight/hdinsight-service-tags).

### <a name="custom-ambari-db"></a>Base de datos de Ambari personalizada
HDInsight ahora permite usar su propia base de datos SQL para Apache Ambari. Puede configurar esta base de datos de Ambari personalizada en Azure Portal o mediante una plantilla de Resource Manager.  Esta característica le permite elegir la base de datos SQL adecuada para sus necesidades de procesamiento y capacidad. También se puede actualizar fácilmente para cumplir los requisitos de crecimiento del negocio. Para más información, consulte [Configuración de clústeres de HDInsight con una base de datos de Ambari personalizada](hdinsight-custom-ambari-db.md).

![Base de datos de Ambari personalizada](./media/hdinsight-release-notes/custom-ambari-db.png)

## <a name="deprecation"></a>Desuso
No hay elementos en desuso en esta versión. Para prepararse para las próximas entradas en desuso, consulte [Próximos cambios](#upcoming-changes).

## <a name="behavior-changes"></a>Cambios de comportamiento
No hay cambios de comportamiento en esta versión. Para prepararse para los próximos cambios de comportamiento, consulte [Próximos cambios](#upcoming-changes).

## <a name="upcoming-changes"></a>Próximos cambios
En las próximas versiones, se realizarán los siguientes cambios. 

### <a name="transport-layer-security-tls-12-enforcement"></a>Cumplimiento de Seguridad de la capa de transporte (TLS) 1.2
Seguridad de la capa de transporte (TLS) y Capa de sockets seguros (SSL) son protocolos criptográficos que proporcionan la seguridad de las comunicaciones a través de una red de equipos. Para más información, consulte [Seguridad de la capa de transporte](https://en.wikipedia.org/wiki/Transport_Layer_Security#SSL_1.0.2C_2.0_and_3.0). Aunque los clústeres de Azure HDInsight aceptan conexiones TLS 1.2 en puntos de conexión HTTPS públicos, todavía se admite TLS 1.1 para la compatibilidad con versiones anteriores de clientes más antiguos.

A partir de la siguiente versión, podrá elegir y configurar los nuevos clústeres de HDInsight para que solo acepten conexiones TLS 1.2. 

Más adelante en el año, a partir del 30/6/2020, Azure HDInsight aplicará TLS 1.2 o versiones posteriores para todas las conexiones HTTPS. Se recomienda asegurarse de que todos los clientes están listos para trabajar con TLS 1.2 o versiones posteriores.

### <a name="moving-to-azure-virtual-machine-scale-sets"></a>Movimiento a conjuntos de escalado de máquinas virtuales
Ahora HDInsight usa máquinas virtuales de Azure para aprovisionar el clúster. A partir de febrero de 2020 (la fecha exacta se comunicará más adelante), HDInsight usará conjuntos de escalado de máquinas virtuales de Azure en su lugar. Consulte más información sobre los [conjuntos de escalado de máquinas virtuales de Azure](https://docs.microsoft.com/azure/virtual-machine-scale-sets/overview).

### <a name="esp-spark-cluster-node-size-change"></a>Cambio de tamaño del nodo del clúster de Spark de ESP 
En la próxima versión:
- El tamaño de nodo mínimo permitido para un clúster de Spark de ESP se cambiará a Standard_D13_V2. 
- Las máquinas virtuales de la serie A dejarán de usarse para crear nuevos clústeres de ESP, ya que las máquinas virtuales de la serie A podrían causar problemas en el clúster de ESP debido a una capacidad de memoria y CPU relativamente bajas.

### <a name="hbase-20-to-21"></a>HBase 2.0 a 2.1
En la próxima versión de HDInsight 4.0, la versión de HBase se actualizará de 2.0 a 2.1.

## <a name="bug-fixes"></a>Corrección de errores
HDInsight continúa realizando mejoras en la confiabilidad y el rendimiento del clúster. 

## <a name="component-version-change"></a>Cambio de versión de componentes
Hemos ampliado la compatibilidad con HDInsight 3.6 al 31 de diciembre de 2020. Puede encontrar más detalles en [Versiones admitidas de HDInsight](hdinsight-component-versioning.md#supported-hdinsight-versions).

No hay cambio de versión de componentes para HDInsight 4.0.

Apache Zeppelin en HDInsight 3.6: 0.7.0-->0.7.3. 

Puede encontrar las versiones de componentes más actualizadas en [este documento](https://docs.microsoft.com/azure/hdinsight/hdinsight-component-versioning#apache-hadoop-components-available-with-different-hdinsight-versions).

## <a name="new-regions"></a>Nuevas regiones

### <a name="uae-north"></a>Norte de Emiratos Árabes Unidos
Las direcciones IP de administración de Norte de Emiratos Árabes Unidos son: `65.52.252.96` y `65.52.252.97`.
