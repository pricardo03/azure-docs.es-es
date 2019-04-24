---
title: Arquitectura de red virtual de Azure HDInsight
description: Obtenga información sobre los recursos disponibles al crear un clúster de HDInsight en una red Virtual de Azure.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
origin.date: 03/26/2019
ms.date: 04/29/2019
ms.author: v-yiso
ms.openlocfilehash: 6d92273298c0448d7377acab6f3b8ea1cc1ed908
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60484890"
---
# <a name="azure-hdinsight-virtual-network-architecture"></a>Arquitectura de red virtual de Azure HDInsight

Este artículo explica a los recursos que están presentes cuando se implementa un clúster de HDInsight en una red Virtual de Azure personalizada. Esta información le ayudará a conectarse a recursos locales para el clúster de HDInsight en Azure. Para obtener más información sobre las redes virtuales de Azure, consulte [¿qué es Azure Virtual Network?](../virtual-network/virtual-networks-overview.md).

## <a name="resource-types-in-azure-hdinsight-clusters"></a>Tipos de recursos de clústeres de HDInsight de Azure

Clústeres de HDInsight de Azure tienen distintos tipos de máquinas virtuales o nodos. Cada tipo de nodo desempeña un papel en la operación del sistema. La tabla siguiente resume estos tipos de nodo y sus roles en el clúster.

| Type | DESCRIPCIÓN |
| --- | --- |
| Nodo principal |  Para todos los tipos de clúster excepto Apache Storm, los nodos principales hospedan los procesos que administración la ejecución de la aplicación distribuida. El nodo principal es también el nodo que se puede acceder mediante SSH en y ejecutar aplicaciones que, a continuación, se coordinan para ejecutar en los recursos del clúster. El número de nodos principales se fija en dos para los tipos de clúster. |
| Nodo zooKeeper | Zookeeper coordina las tareas entre los nodos que realizan procesamiento de datos. También elección de líder del nodo principal y realiza un seguimiento de qué nodo principal se está ejecutando un servicio principal específico. El número de nodos de ZooKeeper se fija en dos. |
| Nodo de trabajo | Representa los nodos que admiten la funcionalidad de procesamiento de datos. Pueden agregar o quitar del clúster para escalar la funcionalidad de computación y administrar los costos de los nodos de trabajo. |
| Nodo perimetral de R Server | El nodo perimetral de R Server representa el nodo se puede acceder mediante SSH en y ejecutar aplicaciones que, a continuación, se coordinan para ejecutar en los recursos del clúster. Un nodo perimetral no participa en el análisis de datos dentro del clúster. Este nodo también hospeda R Studio Server, lo que permite ejecutar la aplicación de R con un explorador. |
| Nodo de región | Para el tipo de clúster de HBase, el nodo de la región (también denominado un nodo de datos) ejecuta en el servidor de regiones. Los servidores de regiones atender y administración una parte de los datos administrados por HBase. Pueden agregar o quitar del clúster para escalar la funcionalidad de computación y administrar los costos de los nodos de la región.|
| Nodo Nimbus | Para el tipo de clúster Storm, el nodo de Nimbus proporciona una funcionalidad similar para el nodo principal. El nodo de Nimbus asigna tareas a otros nodos en un clúster mediante Zookeeper, que coordina la ejecución de topologías de Storm. |
| Nodo de supervisor | Para el tipo de clúster Storm, el nodo de supervisor ejecuta las instrucciones proporcionadas por el nodo de Nimbus para realizar el procesamiento deseado. |

## <a name="basic-virtual-network-resources"></a>Recursos de red virtual básica

El siguiente diagrama muestra la colocación de los nodos de HDInsight y los recursos de red en Azure.

![Diagrama de entidades de HDInsight que creó en la red virtual de Azure personalizado](./media/hdinsight-virtual-network-architecture/vnet-diagram.png)

Los recursos predeterminados presentes cuando se implementa HDInsight en una red Virtual de Azure incluyen los tipos de nodo de clúster se ha mencionado en la tabla anterior, así como los dispositivos de red que admiten la comunicación entre la red virtual y redes externas.

En la tabla siguiente se resume los nodos del clúster de nueve que se crean cuando se implementa HDInsight en una red Virtual de Azure personalizada.

| Tipo de recurso | Número presente | Detalles |
| --- | --- | --- |
|Nodo principal | two |    |
|Nodo de Zookeeper | three | |
|Nodo de trabajo | two | Este número puede variar en función de la configuración del clúster y ajuste de escala. Se necesita un mínimo de tres nodos de trabajo de Apache Kafka.  |
|Nodo de puerta de enlace | two | Nodos de puerta de enlace son máquinas virtuales que se crean en Azure, pero no están visibles en su suscripción. Póngase en contacto con soporte técnico si necesita reiniciar estos nodos. |

Se crean automáticamente los siguientes recursos de red presentes dentro de la red virtual usada con HDInsight:

| Recursos de redes | Número presente | Detalles |
| --- | --- | --- |
|Equilibrador de carga | three | |
|Interfaces de red | nueve | Este valor se basa en un clúster normal, donde cada nodo tiene su propia interfaz de red. Las interfaces de nueve son para los dos nodos principales, tres nodos zookeeper, dos nodos de trabajo y dos nodos de puerta de enlace que se mencionan en la tabla anterior. |
|Direcciones IP públicas | two |    |

## <a name="endpoints-for-connecting-to-hdinsight"></a>Puntos de conexión para conectarse a HDInsight

Puede tener acceso a su clúster de HDInsight en tres formas:

- Un extremo HTTPS fuera de la red virtual en `CLUSTERNAME.azurehdinsight.net`.
- Un punto de conexión SSH para conectarse directamente al nodo principal en `CLUSTERNAME-ssh.azurehdinsight.net`.
- Un extremo HTTPS en la red virtual `CLUSTERNAME-int.azurehdinsight.net`. Tenga en cuenta el "-int" en esta dirección URL. Este punto de conexión se resolverá en una dirección IP privada en la red virtual y no es accesible desde internet público.

Cada uno de estos tres puntos de conexión se asignan un equilibrador de carga.

Las direcciones IP públicas también se proporcionan a los dos extremos que permiten la conexión desde fuera de la red virtual.

1. Se asigna una dirección IP pública al equilibrador de carga para el nombre de dominio completo (FQDN) que se usará al conectarse al clúster desde internet `CLUSTERNAME.azurehdinsight.net`.
1. La segunda dirección IP pública se utiliza para el nombre de dominio solo SSH `CLUSTERNAME-ssh.azurehdinsight.net`.

## <a name="next-steps"></a>Pasos siguientes

* [Proteger el tráfico entrante a los clústeres de HDInsight en una red virtual con el punto de conexión privado](https://azure.microsoft.com/blog/secure-incoming-traffic-to-hdinsight-clusters-in-a-vnet-with-private-endpoint/)
