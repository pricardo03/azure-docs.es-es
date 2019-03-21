---
title: Instalación de Presto en clústeres de Azure HDInsight basados en Linux
description: Aprenda a instalar Presto y Airpal en clústeres de Hadoop para HDInsight basados en Linux mediante acciones de script.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 01/01/2019
ms.author: hrasheed
ms.openlocfilehash: 435c041bb5fb0a398f92914f943166108cc20080
ms.sourcegitcommit: aa3be9ed0b92a0ac5a29c83095a7b20dd0693463
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2019
ms.locfileid: "58258350"
---
# <a name="install-and-use-presto-on-hadoop-based-hdinsight-clusters"></a>Instalación y uso de Presto en los clústeres de Hadoop para HDInsight

En este artículo se explica cómo instalar Presto en clústeres de HDInsight basado en Hadoop de Azure mediante el uso de las acciones de script. También aprenderá a instalar Airpal en un clúster de HDInsight Presto existente.

HDInsight también ofrece la aplicación Starburst Presto para clústeres de Apache Hadoop. Para más información, consulte [Instalación de aplicaciones de Apache Hadoop de terceros en Azure HDInsight](https://docs.microsoft.com/azure/hdinsight/hdinsight-apps-install-applications).

> [!IMPORTANT]  
> Los pasos descritos en este artículo requieren un clúster de Hadoop para HDInsight 3.5 que use Linux. Linux es el único sistema operativo que se usa en la versión 3.4 de HDInsight, o en las versiones posteriores. Para más información, consulte las [versiones de HDInsight](hdinsight-component-versioning.md).

## <a name="what-is-presto"></a>¿Qué es Presto?
[Presto](https://prestodb.io/overview.html) es un motor de consultas SQL de distribución rápida para macrodatos. Presto es adecuado para las consultas interactivas de petabytes de datos. Para más información sobre los componentes de Presto y cómo funcionan en conjunto, consulte [Presto concepts](https://github.com/prestodb/presto/blob/master/presto-docs/src/main/sphinx/overview/concepts.rst) (Conceptos de Presto).

> [!WARNING]  
> Los componentes proporcionados con el clúster de HDInsight son totalmente compatibles. El soporte técnico de Microsoft ayuda a aislar y a solucionar problemas relacionados con estos componentes.
> 
> Los componentes personalizados como Presto reciben un soporte técnico comercialmente razonable para ayudarle a solucionar el problema. Esto podría bastar para resolver el problema. O quizá se le solicite que consulte los canales disponibles para las tecnologías de código abierto donde se concentran los conocimientos más amplios sobre cada tecnología. Hay diversos sitios de la comunidad que se pueden usar, por ejemplo, el [foro de MSDN para HDInsight](https://social.msdn.microsoft.com/Forums/azure/home?forum=hdinsight) y [Stack Overflow](https://stackoverflow.com). 
>
> Los proyectos de Apache también tienen sitios de proyecto en el [sitio web de Apache](https://apache.org). Un ejemplo es [Hadoop](https://hadoop.apache.org/).


## <a name="install-presto-by-using-script-actions"></a>Instalación de Presto con acciones de script

En esta sección se explica cómo usar el script de ejemplo al crear un clúster mediante Azure Portal: 

1. Siga los pasos de [Crear clústeres basados en Linux en HDInsight con Azure Portal](hdinsight-hadoop-create-linux-clusters-portal.md) para empezar a aprovisionar un clúster. Asegúrese de crear el clúster con el flujo de creación de clústeres **personalizado**. El clúster debe cumplir los siguientes requisitos:

   * Tiene que ser un clúster de Hadoop con HDInsight versión 3.6.

   * Tiene que usar Azure Storage como almacén de datos. El uso de Presto en un clúster que utiliza Azure Data Lake Storage como opción de almacenamiento aún no se ofrece como opción.

     ![HDInsight, Personalizado (tamaño, configuración, aplicaciones)](./media/hdinsight-hadoop-install-presto/hdinsight-install-custom.png)

2. En la zona **Configuración avanzada**, seleccione **Acciones de script**. Proporcione la siguiente información. También puede elegir la opción **Instalar Presto** para el tipo de script:
   
   * **NAME**. escriba un nombre descriptivo para la acción de script.
   * **URI de script de Bash**. `https://raw.githubusercontent.com/hdinsight/presto-hdinsight/master/installpresto.sh`.
   * **HEAD**. Seleccione esta opción.
   * **WORKER**. Seleccione esta opción.
   * **ZOOKEEPER**. Deje en blanco esta casilla de verificación.
   * **PARAMETERS**. deje este campo en blanco.


3. En la parte inferior del área **Acciones de script**, elija el botón **Seleccionar** para guardar la configuración. Por último, elija el botón **Seleccionar** situado en la parte inferior del área **Configuración avanzada** para guardar la información de configuración.

4. Continúe el aprovisionamiento del clúster como se describe en [Crear clústeres basados en Linux en HDInsight con Azure Portal](hdinsight-hadoop-create-linux-clusters-portal.md).

    > [!NOTE]  
    > También se puede utilizar Azure PowerShell, la CLI de Azure clásica, el SDK de HDInsight para .NET o las plantillas de Azure Resource Manager para aplicar las acciones de script. También puede aplicar acciones de script a clústeres que ya se estén ejecutando. Para más información, consulte [Personalización de clústeres de HDInsight basados en Linux mediante acciones de script](hdinsight-hadoop-customize-cluster-linux.md).
    > 
    > 

## <a name="use-presto-with-hdinsight"></a>Uso de Presto con HDInsight

Para trabajar con Presto en un clúster de HDInsight, siga estos pasos:

1. Conéctese al clúster de HDInsight con SSH:
   
    `ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net`
   
    Para más información, consulte [Connect to HDInsight (Apache Hadoop) by using SSH](hdinsight-hadoop-linux-use-ssh-unix.md) (Conexión a través de SSH con HDInsight [Apache Hadoop]).
     

2. Para iniciar el shell de Presto, ejecute el siguiente comando:
   
    `presto --schema default`

3. Ejecute una consulta en una tabla de ejemplo, **hivesampletable**, disponible en todos los clústeres de HDInsight de forma predeterminada:
   
    `select count (*) from hivesampletable;`
   
    De forma predeterminada, los conectores [Apache Hive](https://prestodb.io/docs/current/connector/hive.html) y [TPCH](https://prestodb.io/docs/current/connector/tpch.html) para Presto ya están configurados. El conector de Hive está configurado para usar la instalación predeterminada de Hive. Por lo tanto, todas las tablas de Hive son visibles automáticamente en Presto.

    Consulte la [documentación de Presto](https://prestodb.io/docs/current/index.html) para más información.

## <a name="use-airpal-with-presto"></a>Uso de Airpal con Presto

[Airpal](https://github.com/airbnb/airpal#airpal) es una interfaz de consulta de código abierto basada en web para Presto. Para más información sobre Airpal, consulte la [documentación de Airpal](https://github.com/airbnb/airpal#airpal).

Siga estos pasos para instalar Airpal en el nodo perimetral:

1. Mediante SSH, conéctese al nodo principal del clúster de HDInsight en el que instaló Presto:
   
    `ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net`
   
    Para más información, consulte [Connect to HDInsight (Apache Hadoop) by using SSH](hdinsight-hadoop-linux-use-ssh-unix.md) (Conexión a través de SSH con HDInsight [Apache Hadoop]).

2. Una vez conectado, ejecute el siguiente comando:

    `sudo slider registry  --name presto1 --getexp presto` 
   
    Verá un resultado similar al siguiente JSON:

        {
            "coordinator_address" : [ {
                "value" : "10.0.0.12:9090",
                "level" : "application",
                "updatedTime" : "Mon Apr 03 20:13:41 UTC 2017"
        } ]

3. De esta salida, anote el valor de la propiedad **value**. Va a necesitar este valor durante la instalación de Airpal en el nodo perimetral del clúster. De la salida anterior, el valor que necesita es **10.0.0.12:9090**.

4. Use [esta plantilla](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fhdinsight%2Fpresto-hdinsight%2Fmaster%2Fairpal-deploy.json) para crear un nodo perimetral del clúster de HDInsight. Proporcione los valores de la siguiente captura de pantalla.

    ![Implementación personalizada](./media/hdinsight-hadoop-install-presto/hdinsight-install-airpal.png)

5. Seleccione **Comprar**.

6. Una vez que los cambios se hayan aplicado a la configuración del clúster, acceda desde [Azure Portal](https://portal.azure.com) a la interfaz de web de Airpal siguiendo estos pasos:

    1. En el menú izquierdo, seleccione **Todos los servicios**.

    1. Seleccione **Clústeres de HDInsight** en **ANALYTICS**.

    1. Seleccione el clúster en la lista, lo que abrirá la vista predeterminada.

    1. En la vista predeterminada, en **Configuración**, seleccione **Aplicaciones**.

        ![HDInsight, aplicaciones](./media/hdinsight-hadoop-install-presto/hdinsight-presto-launch-airpal.png)

    1. En la página **Aplicaciones instaladas**, localice la entrada de la tabla de **airpal**. Seleccione **Portal**.

        ![Aplicaciones instaladas](./media/hdinsight-hadoop-install-presto/hdinsight-presto-launch-airpal-1.png)

    1. Cuando se le solicite, escriba las credenciales de administrador que especificó al crear el clúster de HDInsight basado en Hadoop.

## <a name="customize-a-presto-installation-on-hdinsight-cluster"></a>Personalización de una instalación de Presto en un clúster de HDInsight

Para personalizar la instalación, siga estos pasos:

1. Mediante SSH, conéctese al nodo principal del clúster de HDInsight en el que instaló Presto:
   
    `ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net`
   
    Para más información, consulte [Connect to HDInsight (Apache Hadoop) by using SSH](hdinsight-hadoop-linux-use-ssh-unix.md) (Conexión a través de SSH con HDInsight [Apache Hadoop]).

2. Realice los cambios de configuración en el archivo `/var/lib/presto/presto-hdinsight-master/appConfig-default.json`. Para más información sobre la configuración de Presto, consulte [Presto Configuration Options for YARN-Based Clusters](https://prestodb.io/presto-yarn/installation-yarn-configuration-options.html) (Opciones de configuración de Presto para clústeres basados en YARN).

3. Detenga y termine la instancia en ejecución de Presto actualmente:

    `sudo slider stop presto1 --force``sudo slider destroy presto1 --force`

4. Inicie una nueva instancia de Presto con la personalización:

    `sudo slider create presto1 --template /var/lib/presto/presto-hdinsight-master/appConfig-default.json --resources /var/lib/presto/presto-hdinsight-master/resources-default.json`

5. Espere a que la nueva instancia esté lista. Tenga en cuenta la dirección del coordinador de Presto:

    `sudo slider registry --name presto1 --getexp presto`

## <a name="generate-benchmark-data-for-hdinsight-clusters-that-run-presto"></a>Generación de datos de pruebas comparativas para clústeres de HDInsight que ejecutan Presto

TPC-DS es el estándar del sector para medir el rendimiento de muchos sistemas de ayuda a la toma de decisiones, incluidos los sistemas de macrodatos. Puede usar Presto para generar datos y evaluar cómo se compara con sus propios datos de prueba comparativa de HDInsight. Para más información, consulte [tpcds-hdinsight](https://github.com/hdinsight/tpcds-datagen-as-hive-query/blob/master/README.md).



## <a name="next-steps"></a>Pasos siguientes
* [Instalación y uso de Hue en clústeres de Hadoop para HDInsight](hdinsight-hadoop-hue-linux.md). Hue es una interfaz de usuario web que facilita el proceso de crear, ejecutar y guardar trabajos de Apache Pig y Hive.

* [Instalar Apache Giraph en clústeres de Hadoop de HDInsight y usar Giraph para procesar gráficos a gran escala](hdinsight-hadoop-giraph-install-linux.md). Use la personalización del clúster para instalar Giraph en clústeres de HDInsight basados en Hadoop. Con Giraph, puede realizar el procesamiento de grafos mediante Hadoop. También puede utilizarse con Azure HDInsight.

[hdinsight-install-r]: hdinsight-hadoop-r-scripts-linux.md
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster-linux.md
