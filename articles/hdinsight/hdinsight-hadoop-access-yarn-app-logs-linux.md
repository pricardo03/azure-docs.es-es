---
title: 'Acceso a los registros de aplicaciones de Apache Hadoop YARN: Azure HDInsight'
description: Obtenga información acerca de cómo tener acceso a los registros de aplicaciones de YARN en un clúster de HDInsight (Apache Hadoop) basado en Linux mediante la línea de comandos y un explorador web.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 03/22/2018
ms.openlocfilehash: 263456769ab391cbc0588eed1a714a1ea5788154
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2019
ms.locfileid: "73494886"
---
# <a name="access-apache-hadoop-yarn-application-logs-on-linux-based-hdinsight"></a>Acceso a registros de aplicación de YARN de Apache Hadoop en HDInsight basado en Linux

Aprenda a acceder a los registros de aplicaciones [Apache Hadoop YARN](https://hadoop.apache.org/docs/current/hadoop-yarn/hadoop-yarn-site/YARN.html) (del inglés Yet Another Resource Negotiator) en un clúster [Apache Hadoop](https://hadoop.apache.org/) en Azure HDInsight.

## <a name="YARNTimelineServer"></a>Servidor de escala de tiempo de YARN

El [servidor de escala de tiempo de Apache Hadoop YARN](https://hadoop.apache.org/docs/r2.7.3/hadoop-yarn/hadoop-yarn-site/TimelineServer.html) proporciona información genérica sobre aplicaciones completadas.

El servidor de escala de tiempo de YARN incluye los siguientes tipos de datos:

* El ID de la aplicación, que es el identificador único de una aplicación.
* El usuario que ha iniciado la aplicación.
* La información acerca de los intentos realizados para completar la aplicación.
* Los contenedores usados por cualquier intento de aplicación concreto.

## <a name="YARNAppsAndLogs"></a>Registros y aplicaciones de YARN

YARN admite varios modelos de programación ([MapReduce de Apache Hadoop](https://hadoop.apache.org/docs/r1.2.1/mapred_tutorial.html) es uno de ellos) al desacoplar la administración de recursos de la programación/supervisión de aplicaciones. Así, usa una instancia de *Resource Manager* (RM) global por nodo de trabajo *NodeManagers* (NM) y por aplicación *ApplicationMasters* (AM). El AM por aplicación negocia recursos (CPU, memoria, disco, red) para ejecutar la aplicación con el RM. El RM funciona con NM para conceder estos recursos como *contenedores*. El AM es responsable del seguimiento del progreso de los contenedores asignados a él por el RM. Una aplicación puede requerir muchos contenedores según la naturaleza de la aplicación.

Cada aplicación puede constar de varios *intentos de aplicación*. Si se produce un error en una aplicación, se puede reintentar como un nuevo intento. Cada intento se ejecuta en un contenedor. En cierto sentido, un contenedor proporciona el contexto de la unidad básica de trabajo realizado por una aplicación de YARN. Todo el trabajo que se realiza en el contexto de un contenedor se realiza solo en el nodo de trabajo en el que se asignó el contenedor. Consulte los [conceptos de Apache Hadoop YARN](https://hadoop.apache.org/docs/r2.7.4/hadoop-yarn/hadoop-yarn-site/WritingYarnApplications.html) para más información.

Los registros de aplicación (y los registros de contenedor asociados) son fundamentales en la depuración de las aplicaciones de Hadoop problemáticas. YARN proporciona un buen marco para recopilar, agregar y almacenar registros de aplicaciones con la característica de [agregación de registros](https://hortonworks.com/blog/simplifying-user-logs-management-and-access-in-yarn/). La característica Agregación de registro hace que el acceso a los registros de las aplicaciones sea más determinista. Esta característica agrega los registros en todos los contenedores de un nodo de trabajo y los almacena como un archivo de registros agregados por cada nodo de trabajo. El registro se almacena en el sistema de archivos de forma predeterminada una vez finalizada una aplicación. Su aplicación puede usar cientos o miles de contenedores, pero los registros para todos los contenedores que se ejecutan en un único nodo de trabajo se agregarán siempre a un único archivo. Por tanto, solo hay un registro por nodo de trabajo usado por la aplicación. La agregación de registros está habilitada de forma predeterminada en los clústeres de HDInsight de la versión 3.0 y superior. Los registros agregados se encuentran en el almacenamiento predeterminado del clúster. La siguiente ruta de acceso es la ruta de acceso de HDFS a los registros:

    /app-logs/<user>/logs/<applicationId>

En la ruta de acceso, `user` es el nombre del usuario que inició la aplicación. `applicationId` es el identificador único asignado a una aplicación mediante el RM de YARN.

Los registros agregados no son legibles directamente, ya que se escriben en [TFile][T-file], un [formato binario ][binary-format] que indexa el contenedor. Use los registros de ResourceManager de YARN o las herramientas de la CLI para ver estos registros como texto sin formato para aplicaciones o contenedores de interés.

## <a name="yarn-cli-tools"></a>Herramientas de la CLI de YARN

Para poder usar las herramientas de la CLI de YARN, tiene que conectarse primero al clúster de HDInsight mediante SSH. Para más información, consulte [Uso de SSH con HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

Puede ver estos registros como texto sin formato ejecutando uno de los siguientes comandos:

    yarn logs -applicationId <applicationId> -appOwner <user-who-started-the-application>
    yarn logs -applicationId <applicationId> -appOwner <user-who-started-the-application> -containerId <containerId> -nodeAddress <worker-node-address>

Al ejecutar estos comandos, especifique la siguiente información: &lt;applicationId>, &lt;user-who-started-the-application>, &lt;containerId> y &lt;worker-node-address>.

## <a name="yarn-resourcemanager-ui"></a>Interfaz de usuario de ResourceManager de YARN

La interfaz de usuario de ResourceManager de YARN se ejecuta en el nodo principal del clúster. Se accede a ella a través de la interfaz de usuario web de Ambari. Para ver los registros de YARN, use los siguientes pasos:

1. Abra el explorador y vaya a https://CLUSTERNAME.azurehdinsight.net. Reemplace CLUSTERNAME por el nombre del clúster de HDInsight.
2. En la lista de servicios de la izquierda de la página, seleccione **YARN**.

    ![Servicio de Apache Ambari Yarn seleccionado](./media/hdinsight-hadoop-access-yarn-app-logs-linux/yarn-service-selected.png)

3. En la lista desplegable **Vínculos rápidos**, seleccione uno de los nodos del clúster principal y elija **Registro de ResourceManager**.

    ![Vínculos rápidos de Apache Ambari Yarn](./media/hdinsight-hadoop-access-yarn-app-logs-linux/hdi-yarn-quick-links.png)

    Aparece una lista de vínculos a los registros de YARN.

[YARN-timeline-server]:https://hadoop.apache.org/docs/r2.4.0/hadoop-yarn/hadoop-yarn-site/TimelineServer.html
[T-file]:https://issues.apache.org/jira/secure/attachment/12396286/TFile%20Specification%2020081217.pdf
[binary-format]:https://issues.apache.org/jira/browse/HADOOP-3315
