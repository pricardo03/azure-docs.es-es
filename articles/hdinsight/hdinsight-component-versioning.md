---
title: 'Componentes y versiones de Apache Hadoop: Azure HDInsight'
description: Conozca los componentes y las versiones de Apache Hadoop en HDInsight y los niveles de servicio disponibles en esta distribución de nube de Hortonworks Data Platform.
keywords: versiones de Hadoop, componentes del ecosistema de Hadoop, componentes de Hadoop, cómo comprobar la versión de Hadoop
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: conceptual
ms.date: 06/07/2019
ms.openlocfilehash: 72be96e89cd46267d0c0795bd5121de76e48cf6f
ms.sourcegitcommit: 9b80d1e560b02f74d2237489fa1c6eb7eca5ee10
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/01/2019
ms.locfileid: "67484094"
---
# <a name="what-are-the-apache-hadoop-components-and-versions-available-with-hdinsight"></a>¿Cuáles son los componentes y versiones de Apache Hadoop disponibles con HDInsight?

Conozca Enterprise Security Package, así como los componentes y las versiones del ecosistema [Apache Hadoop](https://hadoop.apache.org/) que se incluyen en Microsoft Azure HDInsight. Además, obtenga información sobre cómo comprobar las versiones de componentes de Hadoop en HDInsight.

## <a name="apache-hadoop-components-available-with-different-hdinsight-versions"></a>Componentes de Apache Hadoop disponibles con las distintas versiones de HDInsight

HDInsight de Azure es compatible con varias versiones de clústeres de Hadoop que se pueden implementar en cualquier momento. Cada versión crea una versión específica de la distribución HDP y un conjunto de componentes que están incluidos en esa distribución. A partir del 4 de abril de 2017, la versión de clúster predeterminada que usa Azure HDInsight es la 3.6 y se basa en HDP 2.6.

En la tabla siguiente se enumeran las versiones de componente asociadas a las versiones de clúster de HDInsight: 

> [!NOTE]  
> La versión predeterminada del servicio HDInsight puede cambiar sin previo aviso. Si tiene una dependencia de la versión, especifique la versión de HDInsight al crear clústeres con el SDK de .NET con Azure PowerShell y la CLI de Azure clásica.

| Componente | HDInsight 4.0 | HDInsight 3.6 (predeterminado) | HDInsight 3.5 | HDInsight 3.4 | HDInsight 3.3 | HDInsight 3.2 |
|---------------------------|---------------|-----------------------------|---------------|---------------|---------------|----------------------|
| Hortonworks Data Platform | 3.0 | 2.6 | 2.5 | 2.4 | 2.3 | 2.2 |
| Apache Hadoop y YARN | 3.1.1 | 2.7.3 | 2.7.3 | 2.7.1 | 2.7.1 | 2.6.0 |
| Apache Tez | 0.9.1 | 0.7.0 | 0.7.0 | 0.7.0 | 0.7.0 | 0.5.2 |
| Apache Pig | 0.16.0 | 0.16.0 | 0.16.0 | 0.15.0 | 0.15.0 | 0.14.0 |
| Apache Hive y HCatalog | - | 1.2.1 | 1.2.1 | 1.2.1 | 1.2.1 | 0.14.0 |
| Apache Hive | 3.1.0 | 2.1.0 | - | - | - | - |
| Apache Tez Hive2 | - | 0.8.4 | - | - | - | - |
| Apache Ranger | 1.1.0 | 0.7.0 | 0.6.0 | - | - | - |
| HBase Apache | 2.0.1 | 1.1.2 | 1.1.2 | 1.1.2 | 1.1.1 | 0.98.4 |
| Apache Sqoop | 1.4.7 | 1.4.6 | 1.4.6 | 1.4.6 | 1.4.6 | 1.4.5 |
| Apache Oozie | 4.3.1 | 4.2.0 | 4.2.0 | 4.2.0 | 4.2.0 | 4.1.0 |
| Apache Zookeeper | 3.4.6 | 3.4.6 | 3.4.6 | 3.4.6 | 3.4.6 | 3.4.6 |
| Apache Storm | - | 1.1.0 | 1.0.1 | 0.10.0 | 0.10.0 | 0.9.3 |
| Apache Mahout | - | 0.9.0+ | 0.9.0+ | 0.9.0+ | 0.9.0+ | 0.9.0 |
| Apache Phoenix | 5 | 4.7.0 | 4.7.0 | 4.4.0 | 4.4.0 | 4.2.0 |
| Spark de Apache | 2.3.1, 2.4 | 2.3.0, 2.2.0 y 2.1.0 | 1.6.2, 2.0 | 1.6.0 | 1.5.2 | 1.3.1 (solo Windows) |
| Apache Livy | 0,5 | 0.4, 0.4, 0.3 | 0,3 | 0,3 | 0,2 | - |
| Apache Kafka | 1.1.1, 2.1 | 1.1, 1.0 * (vea la nota siguiente) | 0.10.0 | 0.9.0 | - | - |
| Apache Ambari | 2.7.0 | 2.6.0 | 2.4.0 | 2.2.1 | 2.1.0 | - |
| Apache Zeppelin | 0.8.0 | 0.7.0 | - | - | - | - |
| Mono | 4.2.1 | 4.2.1 | 4.2.1 | 3.2.8 | - | - |

> [!NOTE]
> Debido a consideraciones de rendimiento del sistema, la compatibilidad con la versión 0.10 de Kafka expiró en marzo de 2019.

## <a name="check-for-current-hadoop-component-version-information"></a>Comprobación de la información de la versión de los componentes actuales de Hadoop

Las versiones de los componentes del ecosistema de Hadoop asociados a las versiones de los clústeres de HDInsight pueden cambiar en futuras actualizaciones de HDInsight. Para comprobar los componentes de Hadoop disponibles y comprobar las versiones que se están usando para un clúster, use la API REST de Ambari. El comando **GetComponentInformation** recupera información sobre componentes de servicio. Para más información, consulte la [documentación de Apache Ambari](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).

### <a name="release-notes"></a>Notas de la versión

Consulte [Notas de la versión de HDInsight](hdinsight-release-notes.md) para conocer otras notas de las últimas versiones de HDInsight.

## <a name="supported-hdinsight-versions"></a>Versiones compatibles de HDInsight

Las tablas siguientes enumeran las versiones de HDInsight. Las versiones de HDP que corresponden a cada versión de HDInsight se muestran junto con las fechas de lanzamiento del producto. Si se conocen, también se proporcionan las fechas de expiración y retirada.

### <a name="available-versions"></a>Versiones disponibles

En la tabla siguiente se enumeran las versiones de HDInsight que están disponibles en Azure Portal, así como otros métodos de implementación como PowerShell y SDK de .NET.

| Versión de HDInsight | Versión de HDP | SISTEMA OPERATIVO DE LA MÁQUINA VIRTUAL | Fecha de lanzamiento | Fecha de expiración del soporte técnico | Fecha de retirada | Alta disponibilidad |  Disponibilidad en Azure Portal | 
| --- | --- | --- | --- | --- | --- | --- | --- |
| HDInsight 4.0 |HDP 3.0 |Ubuntu 16.0.4 LTS |24 de septiembre de 2018 | | |Sí |Sí |
| HDInsight 3.6 |HDP 2.6 |Ubuntu 16.0.4 LTS |4 de abril de 2017 | 30 de junio de 2020 |31 de diciembre de 2020 |Sí |Sí |


> [!NOTE]  
> Cuando expira la compatibilidad de una versión, puede dejar de estar disponible en Microsoft Azure Portal. Sin embargo, las versiones del clúster seguirán estando disponibles mediante el parámetro `Version` del comando [New-AzHDInsightCluster](https://docs.microsoft.com/powershell/module/az.hdinsight/new-azhdinsightcluster) de Windows PowerShell y en el SDK de .NET hasta la fecha de retirada de la versión.
>

### <a name="retired-versions"></a>Versiones retiradas

En la tabla siguiente se enumeran las versiones de HDInsight que **no** están disponibles en Azure Portal.

| Versión de HDInsight | Versión de HDP | SISTEMA OPERATIVO DE LA MÁQUINA VIRTUAL | Fecha de lanzamiento | Fecha de expiración del soporte técnico | Fecha de retirada | Alta disponibilidad |  Disponibilidad en Azure Portal | 
| --- | --- | --- | --- | --- | --- | --- | --- |
| HDInsight 3.5 <br> (No pertenece a Spark) |HDP 2.5 |Ubuntu 16.0.4 LTS |30 de septiembre de 2016 |5 de septiembre de 2017 |28 de junio de 2018 |Sí |Sin |
| HDInsight 3.4 |HDP 2.4 |Ubuntu 14.0.4 LTS |29 de marzo de 2016 |29 de diciembre de 2016 |9 de enero de 2018 |Sí |Sin |
| HDInsight 3.3 |HDP 2.3 |Windows Server 2012 R2 |2 de diciembre de 2015 |27 de junio de 2016 |31 de julio de 2018 |Sí |Sin |
| HDInsight 3.3 |HDP 2.3 |Ubuntu 14.0.4 LTS |2 de diciembre de 2015 |27 de junio de 2016 |31 de julio de 2017 |Sí |Sin |
| HDInsight 3.2 |HDP 2.2 |Ubuntu 12.04 LTS o Windows Server 2012 R2 |18 de febrero de 2015 |1 de marzo de 2016 |1 de abril de 2017 |Sí |Sin |
| HDInsight 3.1 |HDP 2,1 |Windows Server 2012 R2 |24 de junio de 2014 |18 de mayo de 2015 |30 de junio de 2016 |Sí |Sin |
| HDInsight 3.0 |HDP 2,0 |Windows Server 2012 R2 |11 de febrero de 2014 |17 de septiembre de 2014 |30 de junio de 2015 |Sí |Sin |
| HDInsight 2.1 |HDP 1,3 |Windows Server 2012 R2 |28 de octubre de 2013 |12 de mayo de 2014 |31 de mayo de 2015 |Sí |Sin |
| HDInsight 1.6 |HDP 1.1 | |28 de octubre de 2013 |26 de abril de 2014 |31 de mayo de 2015 |Sin |Sin |

> [!NOTE]  
> Los clústeres de alta disponibilidad con dos nodos principales se implementan de forma predeterminada para los clústeres de HDInsight 2.1 y versiones posteriores. No están disponibles para los clústeres de HDInsight 1.6.

## <a name="enterprise-security-package-for-hdinsight"></a>Paquete de seguridad de la empresa para HDInsight

Enterprise Security es un paquete opcional que puede agregar en su clúster de HDInsight como parte del flujo de trabajo de creación del clúster. El paquete de seguridad de la empresa admite:

- Integración con Active Directory para autenticación.

    En el pasado, solo se podían crear clústeres de HDInsight con un usuario administrador local y un usuario de SSH local. El usuario administrador local podía acceder a todos los archivos, carpetas, tablas y columnas.  Con el paquete de seguridad de la empresa, puede habilitar el control de acceso basado en rol mediante la integración de clústeres de HDInsight con su propia instancia de Active Directory, incluyendo Active Directory local, Azure Active Directory Domain Services o Active Directory en una máquina virtual de IaaS. El administrador de dominio del clúster puede conceder a los usuarios permiso para utilizar sus propias credenciales corporativas (dominio) de nombre de usuario y contraseña para acceder al clúster. 

    Para más información, consulte:

    - [Introducción a la seguridad de Apache Hadoop con clústeres de HDInsight unidos a un dominio](./domain-joined/apache-domain-joined-introduction.md)
    - [Planeamiento de clústeres de Apache Hadoop unidos a un dominio de Azure en HDInsight](./domain-joined/apache-domain-joined-architecture.md)
    - [Configuración de un entorno de espacio aislado unido a un dominio](./domain-joined/apache-domain-joined-configure.md)
    - [Configurar clústeres de HDInsight unidos a un dominio con Azure Active Directory Domain Services](./domain-joined/apache-domain-joined-configure-using-azure-adds.md)

- Autorización de datos

  - Integración con Apache Ranger para la autorización de Hive, Spark SQL y colas de Yarn.
  - Puede establecer un control de acceso en archivos y carpetas.

    Para más información, consulte:

  - [Configuración de directivas de Apache Hive en HDInsight unido a un dominio](./domain-joined/apache-domain-joined-run-hive.md)

- Vea los registros de auditoría para supervisar accesos y las directivas configuradas. 

### <a name="supported-cluster-types"></a>Tipos de clúster compatibles

Actualmente, solo los siguientes tipos de clúster admiten el paquete de seguridad de la empresa:

- Hadoop (solo HDInsight 3.6)
- Spark
- Interactive Query

### <a name="support-for-azure-data-lake-storage"></a>Compatibilidad con Azure Data Lake Storage

Enterprise Security Package permite usar Azure Data Lake Storage como almacenamiento principal y como almacenamiento complementario.

### <a name="pricing-and-service-level-agreement"></a>Precios y contrato de nivel de servicio

Para obtener información sobre los precios y el Acuerdo de Nivel de Servicio del paquete de seguridad de la empresa, consulte [Precios de HDInsight](https://azure.microsoft.com/pricing/details/hdinsight/).


## <a name="service-level-agreement-for-hdinsight-cluster-versions"></a>Contrato de nivel de servicio para las versiones de clúster de HDInsight

El contrato de nivel de servicio (SLA) de define en términos de _plazo de soporte técnico_. Un plazo de soporte técnico se refiere al período durante el cual la versión del clúster de HDInsight puede recibir soporte técnico por parte del Soporte técnico y el servicio al cliente de Microsoft. Si la versión tiene una _fecha de expiración de soporte técnico_ que ya ha pasado, el clúster de HDInsight está fuera del servicio de soporte técnico. La fecha de expiración del soporte técnico de una versión determinada X de HDInsight (una vez que hay disponible una versión más reciente X+1) se calcula como la fecha más tardía de las dos siguientes:  

* Fórmula 1: agregue 180 días a la fecha en la que se lanzó la versión X del clúster de HDInsight.
* Fórmula 2: agregue 90 días a la fecha en la que la versión del clúster de HDInsight x+1 se encuentra disponible en Azure Portal.

La _fecha de retirada_ es la fecha tras la cual no se puede crear la versión del clúster en HDInsight. A partir del 31 de julio de 2017, no se puede cambiar el tamaño de un clúster de HDInsight después de su fecha de retirada. 

> [!NOTE]  
> Los clústeres de HDInsight en Windows (incluidas las versiones 2.1, 3.0, 3.1, 3.2 y 3.3) se ejecutan en el SO invitado de Azure Familia 4 que usa la versión de 64 bits de Windows Server 2012 R2. El SO invitado de Azure Familia 4 es compatible con las versiones .NET Framework 4.0, 4.5, 4.5.1 y 4.5.2.

## <a name="hortonworks-release-notes-associated-with-hdinsight-versions"></a>Notas de la versión de HortonWorks asociadas con las versiones de HDInsight

En la sección se proporcionan vínculos a las notas de la versión para las distribuciones de Hortonworks Data Platform y los componentes de Apache usados con HDInsight.
* La versión 4.0 del clúster de HDInsight utiliza una distribución de Hadoop basada en [Hortonworks Data Platform 3.0](https://docs.hortonworks.com/HDPDocuments/HDP3/HDP-3.0.0/release-notes/content/relnotes.html).
* La versión 3.6 del clúster de HDInsight utiliza una distribución de Hadoop basada en [Hortonworks Data Platform 2.6](https://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.6.0/bk_release-notes/content/ch_relnotes.html).
* La versión 3.5 del clúster de HDInsight utiliza una distribución de Hadoop basada en [Hortonworks Data Platform 2.5](https://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.5.0/bk_release-notes/content/ch_relnotes_v250.html). El clúster de HDInsight 3.5 es el clúster de Hadoop _predeterminado_ que se crea en Azure Portal.
* La versión 3.4 del clúster de HDInsight utiliza una distribución de Hadoop basada en [Hortonworks Data Platform 2.4](https://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.4.0/bk_HDP_RelNotes/content/ch_relnotes_v240.html).
* La versión 3.3 del clúster de HDInsight utiliza una distribución de Hadoop basada en [Hortonworks Data Platform 2.3](https://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.3.0/bk_HDP_RelNotes/content/ch_relnotes_v230.html).

  * Las [notas de la versión de Apache Storm](https://storm.apache.org/2015/11/05/storm0100-released.html) están disponibles en el sitio web de Apache.
  * Las [notas de la versión de Apache Hive](https://issues.apache.org/jira/secure/ReleaseNote.jspa?version=12332384&styleName=Text&projectId=12310843) están disponibles en el sitio web de Apache.
* La versión 3.2 del clúster de HDInsight utiliza una distribución de Hadoop basada en [Hortonworks Data Platform 2.2][hdp-2-2].

  * Las notas de la versión para los componentes específicos de Apache están disponibles como se indica a continuación: [Hive 0.14](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310843&version=12326450), [Pig 0.14](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310730&version=12326954), [HBase 0.98.4](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310753&version=12326810), [Phoenix 4.2.0](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12315120&version=12327581), [M/R 2.6](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310941&version=12327180), [HDFS 2.6](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310942&version=12327181), [YARN 2.6](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12313722&version=12327197), [Common](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310240&version=12327179), [Tez 0.5.2](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12314426&version=12328742), [Ambari 2.0](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12312020&version=12327486), [Storm 0.9.3](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12314820&version=12327112) y [Oozie 4.1.0](https://issues.apache.org/jira/secure/ReleaseNote.jspa?version=12324960&projectId=12311620).
* La versión 3.1 del clúster de HDInsight utiliza una distribución de Hadoop basada en [Hortonworks Data Platform 2.1.7][hdp-2-1-7]. HDInsight 3.1 clusters created before November, 7, 2014, are based on [Hortonworks Data Platform 2.1.1][hdp-2-1-1].
* La versión 3.0 del clúster de HDInsight utiliza una distribución de Hadoop basada en [Hortonworks Data Platform 2.0][hdp-2-0-8].
* La versión 2.1 del clúster de HDInsight utiliza una distribución de Hadoop basada en [Hortonworks Data Platform 1.3][hdp-1-3-0].
* La versión 1.6 del clúster de HDInsight utiliza una distribución de Hadoop basada en [Hortonworks Data Platform 1.1][hdp-1-1-0].

## <a name="default-node-configuration-and-virtual-machine-sizes-for-clusters"></a>Configuración de nodo predeterminada y tamaños de máquina virtual para clústeres

En las tablas siguientes se indican los tamaños de máquina virtual predeterminados para clústeres de HDInsight.  Este gráfico es necesario para entender qué tamaños de VM se deben usar al crear scripts de PowerShell o la CLI de Azure para implementar clústeres de HDInsight.

> [!IMPORTANT]  
> Si necesita más de 32 nodos de trabajo en un clúster, tiene que seleccionar un tamaño de nodo principal con al menos 8 núcleos y 14 GB de RAM.

* Todas las regiones, excepto Sur de Brasil y Japón Occidental:

|Tipo de clúster|Hadoop|HBase|Interactive Query|Storm|Spark|ML Server|Kafka|
|---|---|---|---|---|---|---|---|
|Principal: tamaño de máquina virtual predeterminado|D12 v2|D12 v2|D13 v2|A3|D12 v2|D12 v2|D3v2|
|Principal: tamaños de máquina virtual recomendados|D3 v2|D3 v2|D13|A4 v2|D12 v2|D12 v2|A2M v2|
||D4 v2|D4 v2|D14|A8 v2|D13 v2|D13 v2|D3 v2|
||D12 v2|D12 v2|E16 v3|A2m v2|D14 v2|D14 v2|D4 v2|
||E4 v3|E4 v3|E32 v3|E4 v3|E4 v3|E4 v3|D12 v2|
|Trabajo: tamaño de máquina virtual predeterminado|D4 v2|D4 v2|D14 v2|D3 v2|D13 v2|D4 v2|4 D12v2 con 2 discos S30 por agente|
|Trabajo: tamaños de máquina virtual recomendados|D3 v2|D3 v2|D13|D3 v2|D4 v2|D4 v2|D13 v2|
||D4 v2|D4 v2|D14|D4 v2|D12 v2|D12 v2|DS12 v2|
||D12 v2|D12 v2|E16 v3|D12 v2|D13 v2|D13 v2|DS13 v2|
||E4 v3|E4 v3|E20 v3|E4 v3|D14 v2|D14 v2|E4 v3|
||||E32 v3||E16 v3|E16 v3|ES4 v3|
||||E64 v3||E20 v3|E20 v3|E8 v3|
||||||E32 v3|E32 v3|ES8 v3|
||||||E64 v3|E64 v3||
|Zookeeper: tamaño de máquina virtual predeterminado||A4 v2|A4 v2|A4 v2||A2 v2|D3v2|
|Zookeeper: tamaños de máquina virtual recomendados||A4 v2||A2 v2|||A2M v2|
|||A8 v2||A4 v2|||D3 v2|
|||A2m v2||A8 v2|||E8 v3|
|Servicio Machine Learning: tamaño de máquina virtual predeterminado||||||D4 v2||
|Servicio Machine Learning: tamaño de máquina virtual recomendado||||||D4 v2||
|||||||D12 v2||
|||||||D13 v2||
|||||||D14 v2||
|||||||E16 v3||
|||||||E20 v3||
|||||||E32 v3||
|||||||E64 v3||

* Solo Sur de Brasil y Japón Occidental (ningún tamaño v2):

  | Tipo de clúster | Hadoop | HBase | Interactive Query |Storm | Spark | Machine Learning Services |
  | --- | --- | --- | --- | --- | --- | --- |
  | Principal: tamaño de máquina virtual predeterminado |D12 |D12  | D13 |A3 |D12 |D12 |
  | Principal: tamaños de máquina virtual recomendados |D3,<br/> D4,<br/> D12 |D3,<br/> D4,<br/> D12  | D13,<br/> D14 |A3,<br/> A4,<br/> A5 |D12,<br/> D13,<br/> D14 |D12,<br/> D13,<br/> D14 |
  | Trabajo: tamaño de máquina virtual predeterminado |D4 |D4  |  D14 |D3 |D13 |D4 |
  | Trabajo: tamaños de máquina virtual recomendados |D3,<br/> D4,<br/> D12 |D3,<br/> D4,<br/> D12  | D13,<br/> D14 |D3,<br/> D4,<br/> D12 |D4,<br/> D12,<br/> D13,<br/> D14 | D4,<br/> D12,<br/> D13,<br/> D14 |
  | Zookeeper: tamaño de máquina virtual predeterminado | |A4 v2 | A4 v2| A4 v2 | | A2 v2|
  | Zookeeper: tamaños de máquina virtual recomendados | |A2,<br/> A3,<br/> A4 | |A2,<br/> A3,<br/> A4 | | |
  | Servicio Machine Learning: tamaños de máquina virtual predeterminados | | | | | |D4 |
  | Servicio Machine Learning: tamaños de máquina virtual recomendados | | | | | |D4,<br/> D12,<br/> D13,<br/> D14 |

> [!NOTE]
> - Nota: Principal se conoce como *Nimbus* para el tipo de clúster de Storm.
> - El trabajo se conoce como *Supervisor* para el tipo de clúster de Storm.
> - El trabajo se conoce como *Región* para el tipo de clúster de HBase.

## <a name="next-steps"></a>Pasos siguientes
- [Configuración de clúster para Apache Hadoop, Spark, etc. en HDInsight](hdinsight-hadoop-provision-linux-clusters.md)
- [Trabajo en Apache Hadoop en HDInsight desde un equipo Windows](hdinsight-hadoop-windows-tools.md)

[hdp-2-2]: https://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.2.9/bk_HDP_RelNotes/content/ch_relnotes_v229.html

[hdp-2-1-7]: https://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.7-Win/bk_releasenotes_HDP-Win/content/ch_relnotes-HDP-2.1.7.html

[hdp-2-1-1]: https://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.1/bk_releasenotes_hdp_2.1/content/ch_relnotes-hdp-2.1.1.html

[hdp-2-0-8]: https://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.0.8.0/bk_releasenotes_hdp_2.0/content/ch_relnotes-hdp2.0.8.0.html

[hdp-1-3-0]: https://docs.hortonworks.com/HDPDocuments/HDP1/HDP-1.3.0/bk_releasenotes_hdp_1.x/content/ch_relnotes-hdp1.3.0_1.html

[hdp-1-1-0]: https://docs.hortonworks.com/HDPDocuments/HDP1/HDP-1.3.0/bk_releasenotes_hdp_1.x/content/ch_relnotes-hdp1.1.1.16_1.html
