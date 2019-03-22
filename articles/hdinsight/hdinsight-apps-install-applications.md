---
title: Instalación de aplicaciones de terceros en Azure HDInsight
description: Aprenda a instalar aplicaciones de Hadoop de terceros en Azure HDInsight.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 03/01/2019
ms.author: hrasheed
ms.openlocfilehash: 810f9bb81d367cfe70e59d62d81a9e129cf80e6b
ms.sourcegitcommit: 12d67f9e4956bb30e7ca55209dd15d51a692d4f6
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2019
ms.locfileid: "58225990"
---
# <a name="install-third-party-apache-hadoop-applications-on-azure-hdinsight"></a>Instalación de aplicaciones de Apache Hadoop de terceros en Azure HDInsight

Aprenda a instalar una aplicación de [Apache Hadoop](https://hadoop.apache.org/) de terceros en Azure HDInsight. Para obtener instrucciones sobre cómo instalar su propia aplicación, consulte [Instalación de aplicaciones de HDInsight personalizadas](hdinsight-apps-install-custom-applications.md).

Una aplicación de HDInsight es una aplicación que los usuarios pueden instalar en un clúster de HDInsight. Estas aplicaciones puede desarrollarlas Microsoft, fabricantes de software independientes (ISV) o el propio usuario.  

En la lista siguiente se muestran las aplicaciones publicadas:

|Application |Tipos de clúster | DESCRIPCIÓN |
|---|---|---|
|AtScale Intelligence Platform |Hadoop |AtScale activa su clúster de HDInsight en un servidor OLAP de escalabilidad horizontal, lo que le permite consultar miles de millones de filas de datos de forma interactiva mediante las herramientas de BI que ya conoce, el propietario y prefieras: desde Microsoft Excel, Power BI, Tableau Software hasta QlikView. |
|CDAP 4.2, 4.3 para HDInsight |HBase |CDAP es la primera plataforma de integración unificada para macrodatos que acelera el tiempo para el valor de Hadoop y permite a TI a proporcionar datos de autoservicio. Código abierto y extensible, CDAP elimina las barreras para la innovación. Requisitos: 4 nodos de región, min D3 v2. |
|Datameer |Hadoop |Autoservicio plataforma escalable Datameer para preparar, explorar y que rigen los datos para análisis acelera la conversión de datos complejos de varios orígenes en valiosa información empresarial, ya que ofrece información rápida, acertada a escala empresarial. |
|Dataiku DSS en HDInsight |Hadoop, Spark |Dataiku DSS en una plataforma de ciencia de datos empresariales que permite a los científicos de datos y analistas de datos colaborar para diseñar y ejecutar servicios y productos nuevos datos de forma más eficaz, convirtiendo los datos sin procesar en predicciones de gran impacto. |
|WANdisco Fusion HDI App - 2.12.3, 2.12.1, punto 2.11.2 |Hadoop, Spark, HBase, Storm, Kafka |Mantener datos consistentes en un entorno distribuido es un desafío de las operaciones de datos masivos. WANdisco Fusion, una plataforma de software empresarial, soluciona este problema mediante la habilitación de la coherencia de datos no estructurados en cualquier entorno. |
|H2O SparklingWater para HDInsight |Spark |H2O Sparkling Water es compatible con los siguientes algoritmos distribuidos: GLM, Naïve Bayes, bosque aleatorio de distribuidas, máquina Aumentar degradado, redes neuronales profundas, aprendizaje profundo, K-means, PCA, modelos de rango bajo generalizado, detección de anomalías, Autoencoders. |
|Striim para la integración de datos en tiempo real en HDInsight |Hadoop, HBase, Storm, Spark, Kafka |Striim (pronunciado "secuencia") es una-to-end plataforma de inteligencia + de integración de datos de transmisión por secuencias, de manera continua ingesta, procesamiento y análisis de flujos de datos dispares. |
|Jumbune para HDInsight |Hadoop, Spark |En un nivel alto, Jumbune ayuda a las empresas en 1. Aceleración de Tez, motor de MapReduce y Spark había basado Hive, Java, Scala rendimiento de carga de trabajo. 2. Clúster de Hadoop proactivo, supervisión, 3. Establecimiento de administración de calidad de los datos en el sistema de archivos distribuido. |
|Kyligence Enterprise |Hadoop, HBase, Spark |Con tecnología Apache Kylin, Kyligence Enterprise permite a BI sobre Big Data. Como un motor OLAP de enterprise en Hadoop, Kyligence Enterprise mejora la capacidad de analista de negocios para diseñar la arquitectura de BI en Hadoop con almacenamiento de datos estándar del sector y la metodología de BI. |
|Servidor de trabajo de Spark para KNIME Spark ejecutor |Spark |Servidor de trabajo de Spark para KNIME Spark Executor se utiliza para conectar la plataforma de análisis de KNIME a clústeres de HDInsight. |
|Estallido Presto en Azure HDInsight, Presto Estallido (v0.213-e) |Hadoop |Presto es un rápidas y escalable distribuido motor de consultas SQL. Diseñado para la separación de almacenamiento y proceso, Presto es perfecto para consultar datos en Azure Data Lake Storage, Azure Blob Storage, bases de datos SQL y NoSQL y otros orígenes de datos. |
|StreamSets Data Collector para HDInsight en la nube |Hadoop, HBase, Spark, Kafka |StreamSets Data Collector es un motor ligero, eficaz que transmite los datos en tiempo real. Utilizar el recopilador de datos a los datos de ruta y el proceso de las transmisiones de datos. Viene con una licencia de prueba de 30 días. |
|[Trifacta Wrangler Enterprise](https://www.trifacta.com/) |Hadoop, Spark, HBase |Trifacta Wrangler Enterprise para HDInsight es compatible con la comparación para cualquier escala de datos de datos de toda la empresa. El costo de ejecutar Trifacta en Azure es una combinación de los costos de suscripción Trifacta más los costos de infraestructura de Azure para las máquinas virtuales. |
|Plataforma de datos de Unifi 3.1 |Hadoop, HBase, Storm, Spark |La plataforma de datos de Unifi es un conjunto de herramientas de datos de autoservicio diseñadas para capacitar a los empresarios para afrontar los desafíos de datos que aumentar los ingresos incrementales, reducir los costos o complejidad operativa perfectamente integrado. |
|Unraveldata APM |Spark |Aclarar los datos de aplicación para un clúster de HDInsight Spark. |
|Catálogo de datos de flotación |Spark |Flotación cataloga, organiza y controla datos mediante inteligencia artificial a los datos automáticamente etiqueta con términos de negocio. El catálogo alfabetizado empresarial de Waterline es un componente crítico y de éxito para análisis de autoservicio, cumplimiento y gobierno, así como para iniciativas de administración de TI. |

Para las instrucciones proporcionadas en este artículo se usa Azure Portal. También puede exportar la plantilla de Azure Resource Manager desde el portal u obtener una copia de dicha plantilla de proveedores y usar Azure PowerShell y la CLI de Azure clásica para implementarla.  Consulte [Creación de clústeres de Apache Hadoop en HDInsight con plantillas de Resource Manager](hdinsight-hadoop-create-linux-clusters-arm-templates.md).

## <a name="prerequisites"></a>Requisitos previos
Si desea instalar aplicaciones de HDInsight en un clúster de HDInsight existente, debe tener un clúster de HDInsight. Para crearlo, consulte [Crear clúster](hadoop/apache-hadoop-linux-tutorial-get-started.md#create-cluster). También puede instalar aplicaciones de HDInsight al crear un clúster de HDInsight.

## <a name="install-applications-to-existing-clusters"></a>Instalación de aplicaciones en clústeres existentes
En el siguiente procedimiento, se muestra cómo instalar aplicaciones de HDInsight en un clúster de HDInsight existente.

**Instalación de una aplicación HDInsight**

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
2. En el menú izquierdo, vaya a **todos los servicios** > **Analytics** > **clústeres de HDInsight**.
3. Seleccione un clúster de HDInsight en la lista.  Si no tiene ninguno, debe crearlo primero.  Consulte [Crear clúster](hadoop/apache-hadoop-linux-tutorial-get-started.md#create-cluster).
4. En el **configuración** categoría, seleccione **aplicaciones**. Puede ver una lista de las aplicaciones instaladas en la ventana principal. 
   
    ![Menú del portal de aplicaciones de HDInsight](./media/hdinsight-apps-install-applications/hdinsight-apps-portal-menu.png)
5. Seleccione **+ agregar** en el menú. Puede ver una lista de aplicaciones disponibles.  Si **+ agregar** está atenuado, que significa que no hay no es ninguna aplicación para esta versión del clúster de HDInsight.
   
    ![Aplicaciones disponibles para aplicaciones de HDInsight](./media/hdinsight-apps-install-applications/hdinsight-apps-list.png)
6. Seleccione una de las aplicaciones disponibles y, a continuación, siga las instrucciones para aceptar los términos legales.

Puede ver el estado de instalación de las notificaciones de portal (seleccione el icono de campana en la parte superior del portal). Una vez instalada la aplicación, aparece en la lista Aplicaciones instaladas.

## <a name="install-applications-during-cluster-creation"></a>Instalación de aplicaciones durante la creación del clúster
Tiene la opción de instalar aplicaciones de HDInsight al crear un clúster. Durante el proceso, se instalan aplicaciones de HDInsight una vez que se haya creado el clúster y se encuentre en ejecución. Para instalar aplicaciones durante la creación del clúster mediante el portal de Azure, use el **personalizado** opción en lugar del predeterminado **creación rápida** opción.

## <a name="list-installed-hdinsight-apps-and-properties"></a>Lista de las aplicaciones de HDInsight instaladas y sus propiedades
El portal muestra una lista de las aplicaciones de HDInsight instaladas para un clúster y las propiedades de cada una.

**Lista de las aplicaciones de HDInsight y visualización de sus propiedades**

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
2. En el menú izquierdo, vaya a **todos los servicios** > **Analytics** > **clústeres de HDInsight**.
3. Seleccione un clúster de HDInsight en la lista.
4. En el **configuración** categoría, seleccione **aplicaciones**. Puede ver una lista de las aplicaciones instaladas en la ventana principal. 
   
    ![Aplicaciones instaladas en aplicaciones de HDInsight](./media/hdinsight-apps-install-applications/hdinsight-apps-installed-apps-with-apps.png)
5. Seleccione una de las aplicaciones instaladas para mostrar la propiedad. En la propiedad se indican los siguientes elementos:

    |Propiedad | DESCRIPCIÓN |
    |---|---|
    |Nombre de la aplicación |Nombre de la aplicación. |
    |Status |Estado de la aplicación. |
    |Página web |la dirección URL de la aplicación web que ha implementado en el nodo perimetral. La credencial coincide con las credenciales de usuario HTTP que ha configurado para el clúster. |
    |Punto de conexión SSH |puede usar SSH para conectarse al nodo perimetral. Las credenciales de SSH coinciden con las credenciales de usuario SSH que ha configurado para el clúster. Para más información, consulte [Uso de SSH con HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md). |
    |DESCRIPCIÓN | Descripción de la aplicación. |

6. Para eliminar una aplicación, haga clic con el botón derecho en ella y haga clic en **Eliminar** en el menú contextual.

## <a name="connect-to-the-edge-node"></a>Conexión con el nodo perimetral
Puede conectar con el nodo perimetral mediante HTTP y SSH. Encontrará la información de punto de conexión en el [portal](#list-installed-hdinsight-apps-and-properties). Para más información, consulte [Uso de SSH con HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

Las credenciales del punto de conexión HTTP son las credenciales de usuario HTTP que ha configurado para el clúster de HDInsight; las del punto de conexión SSH son las credenciales SSH que ha configurado para el clúster de HDInsight.

## <a name="troubleshoot"></a>Solución de problemas
Consulte [Solución de problemas de instalación](hdinsight-apps-install-custom-applications.md#troubleshoot-the-installation).

## <a name="next-steps"></a>Pasos siguientes
* [Instalación de aplicaciones de HDInsight personalizadas](hdinsight-apps-install-custom-applications.md): aprenda a implementar en HDInsight una aplicación de HDInsight no publicada.
* [Publicación de aplicaciones de HDInsight en Azure Marketplace](hdinsight-apps-publish-applications.md): aprenda a publicar aplicaciones de HDInsight personalizadas en Azure Marketplace.
* [MSDN: Instalación de una aplicación HDInsight](https://msdn.microsoft.com/library/mt706515.aspx): Obtenga información sobre cómo definir las aplicaciones de HDInsight.
* [Personalización de clústeres de HDInsight mediante la acción de scripts (Linux)](hdinsight-hadoop-customize-cluster-linux.md): aprenda a usar acciones de script para instalar otras aplicaciones.
* [Creación de clústeres de Apache Hadoop basados en Linux en HDInsight con plantillas de Azure Resource Manager](hdinsight-hadoop-create-linux-clusters-arm-templates.md): aprenda a llamar a plantillas de Resource Manager para crear clústeres de HDInsight.
* [Use empty edge nodes in HDInsight](hdinsight-apps-use-edge-node.md)(Utilización de nodos perimetrales vacíos en HDInsight): aprenda a usar un nodo perimetral vacío para acceder a los clústeres de HDInsight, probar aplicaciones de este y hospedarlas.

