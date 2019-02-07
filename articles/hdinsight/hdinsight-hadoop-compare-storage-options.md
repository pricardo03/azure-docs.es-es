---
title: Comparación de opciones de almacenamiento para los clústeres de Azure HDInsight
description: Proporciona información general sobre los tipos de almacenamiento y cómo funcionan con Azure HDInsight.
services: hdinsight,storage
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 02/04/2019
ms.openlocfilehash: 7f113587dfabd66461a9bcfbde18a0178c6608f0
ms.sourcegitcommit: 3aa0fbfdde618656d66edf7e469e543c2aa29a57
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/05/2019
ms.locfileid: "55733552"
---
# <a name="compare-storage-options-for-use-with-azure-hdinsight-clusters"></a>Comparación de opciones de almacenamiento para los clústeres de Azure HDInsight

Los usuarios de Azure HDInsight pueden elegir entre varias opciones de almacenamiento diferentes al crear clústeres de HDInsight:

* Azure Data Lake Storage Gen2
* Azure Storage
* Azure Data Lake Storage Gen1

En este artículo se proporciona información general sobre los diferentes tipos de almacenamiento y sus características exclusivas.

## <a name="use-azure-data-lake-storage-gen2-with-apache-hadoop-in-azure-hdinsight"></a>Uso de Azure Data Lake Storage Gen2 con Apache Hadoop en Azure HDInsight

Para obtener más información sobre Data Lake Storage Gen2, consulte [Introducción a Azure Data Lake Storage Gen2](../storage/blobs/data-lake-storage-introduction.md).

Azure Data Lake Storage Gen2 adopta las características principales de Azure Data Lake Storage Gen1, como un sistema de archivos compatible con Hadoop, Azure Active Directory y listas de control de acceso (ACL) basadas en POSIX, y las integra en Azure Blob Storage. Esta combinación permite aprovechar las ventajas de rendimiento de Azure Data Lake Storage Gen1, al mismo tiempo que se usan la administración del ciclo de vida de los datos y las capas de Blob Storage.

### <a name="core-functionality-of-azure-data-lake-storage-gen2"></a>Funcionalidad básica de Azure Data Lake Storage Gen2

* Acceso compatible con Hadoop: Azure Data Lake Storage Gen2 permite administrar datos y acceder a ellos igual que lo haría con un sistema de archivos distribuido de Hadoop (HDFS). El controlador del sistema de archivos de Azure Blob (ABFS) está disponible en todos los entornos de Apache Hadoop, incluidos Azure HDInsight y Azure Databricks, para obtener acceso a los datos almacenados en Data Lake Storage Gen2.

* Superconjunto de permisos POSIX: el modelo de seguridad de Data Lake Gen2 es compatible con los permisos de ACL y POSIX junto con granularidad adicional específica de Data Lake Storage Gen2. Se puede realizar la configuración mediante herramientas de administración o marcos, como Apache Hive y Apache Spark.

* Rentabilidad: Data Lake Storage Gen2 ofrece transacciones y capacidad de almacenamiento de bajo costo. Características como el ciclo de vida de Azure Blob Storage ayudan a reducir los costos mediante el ajuste de las tarifas de facturación a medida que los datos avanzan en su ciclo de vida.

* Funciona con aplicaciones, marcos y herramientas de Blob Storage: Data Lake Storage Gen2 sigue funcionando con una amplia gama de herramientas, marcos y aplicaciones que existen actualmente para Blob Storage.

* Controlador optimizado: el controlador ABFS está optimizado específicamente para el análisis de macrodatos. Las API de REST correspondientes se exponen a través del punto de conexión dfs, dfs.core.windows.net.

### <a name="whats-new-about-azure-data-lake-storage-gen-2"></a>Novedades de Azure Data Lake Storage Gen2

#### <a name="managed-identities-for-secure-file-access"></a>Identidades administradas para proteger el acceso a archivos

Azure HDInsight usa identidades administradas para proteger el acceso del clúster a los archivos de Azure Data Lake Storage Gen2. Las identidades administradas son una característica de Azure Active Directory que proporciona servicios de Azure con un conjunto de credenciales administradas automáticamente. Estas credenciales se pueden usar para autenticarse en cualquier servicio que admita la autenticación de Active Directory. El uso de identidades administradas no requiere almacenar las credenciales en archivos de código o de configuración.

Para obtener más información, vea [What is managed identities for Azure resources?](../active-directory/managed-identities-azure-resources/overview.md) (¿Qué son las identidades administradas de Azure?).

#### <a name="azure-blob-filesystem-abfs-driver"></a>Controlador del sistema de archivos de Azure Blob (ABFS)

Las aplicaciones de Apache Hadoop de forma nativa esperan leer y escribir datos desde el almacenamiento en disco local. Un controlador del sistema de archivos de Hadoop como ABFS permite que las aplicaciones de Hadoop funcionen con el almacenamiento en la nube mediante la emulación de las operaciones habituales del sistema de archivos de Hadoop. El controlador convierte los comandos que recibe desde la aplicación en operaciones que la plataforma real de almacenamiento en la nube reconoce.

Anteriormente, el controlador del sistema de archivos de Hadoop podría convertir todas las operaciones del sistema de archivos en llamadas API de REST de Azure Storage en el lado cliente y, a continuación, invocar la API de REST. Sin embargo, esta conversión del lado cliente resultaba en varias llamadas API de REST para una única operación del sistema de archivos, como cambiar el nombre a un archivo. ABFS movió parte de la lógica del sistema de archivos de Hadoop del lado cliente al lado servidor y la API de Azure Data Lake Storage Gen2 ahora se ejecuta en paralelo con la API de Blob. Esta migración mejora el rendimiento porque ahora se pueden ejecutar operaciones comunes del sistema de archivos de Hadoop con una llamada API de REST.

Para más información, vea [Controlador Azure Blob FileSystem (ABFS): un controlador de Azure Storage dedicado para Hadoop](../storage/blobs/data-lake-storage-abfs-driver.md).

#### <a name="azure-data-lake-storage-gen-2-uri-scheme"></a>Esquema de identificador URI de Azure Data Lake Storage Gen 2

Azure Data Lake Storage Gen2 usa un nuevo esquema de identificador URI para acceder a los archivos de Azure Storage desde HDInsight:

`abfs[s]://<FILE_SYSTEM_NAME>@<ACCOUNT_NAME>.dfs.core.windows.net/<PATH>`

El esquema de URI proporciona acceso cifrado SSL (con el prefijo `abfss://`) y acceso sin cifrar (con el prefijo `abfs://`). Se recomienda usar `abfss` siempre que sea posible, incluso cuando se acceda a datos que se encuentren en la misma región de Azure.

`<FILE_SYSTEM_NAME>` identifica la ruta de acceso del sistema de archivos de Data Lake Storage Gen2.

`<ACCOUNT_NAME>` identifica el nombre de la cuenta de Azure Storage. Se necesita el nombre completo de dominio (FQDN).

`<PATH>` es el nombre de la ruta HDFS del archivo o el directorio.

Si no se especifican los valores de `<FILE_SYSTEM_NAME>` y `<ACCOUNT_NAME>`, se utiliza el sistema de archivos predeterminado. Para los archivos del sistema de archivos predeterminado, puede usar una ruta relativa o absoluta. Por ejemplo, se puede hacer referencia al archivo `hadoop-mapreduce-examples.jar` que se incluye con los clústeres de HDInsight mediante alguna de las rutas de acceso siguientes:

```
abfss://myfilesystempath@myaccount.dfs.core.windows.net/example/jars/hadoop-mapreduce-examples.jar
abfss:///example/jars/hadoop-mapreduce-examples.jar /example/jars/hadoop-mapreduce-examples.jar
```

> [!Note]
> El nombre del archivo es `hadoop-examples.jar` en las versiones 2.1 y 1.6 de los clústeres de HDInsight. Al trabajar con archivos fuera de HDInsight, la mayoría de las utilidades no reconocen el formato ABFS y, en su lugar, esperan un formato básico de ruta de acceso, como `example/jars/hadoop-mapreduce-examples.jar`.

Para más información, vea [Uso del URI de Azure Data Lake Storage Gen2](../storage/blobs/data-lake-storage-introduction-abfs-uri.md).

## <a name="use-azure-storage"></a>Uso de Azure Storage

Azure Storage es una solución de almacenamiento sólida y de uso general, que se integra sin problemas con HDInsight. HDInsight puede usar un contenedor de blobs en Azure Storage como el sistema de archivos predeterminado para el clúster. Mediante una interfaz del sistema de archivos distribuido de Hadoop (HDFS), el conjunto completo de componentes de HDInsight puede operar directamente en datos estructurados o no estructurados almacenados como blobs.

> [!WARNING]  
> Hay varias opciones disponibles al crear una cuenta de Azure Storage. En la tabla siguiente se proporciona información sobre qué opciones se admiten en HDInsight:

| Tipo de cuenta de almacenamiento | Servicios admitidos | Niveles de rendimiento admitidos | Niveles de acceso admitidos |
|----------------------|--------------------|-----------------------------|------------------------|
| Uso general v2   | Blob               | Estándar                    | Frecuente, esporádico, archivo*    |
| Uso general v1   | Blob               | Estándar                    | N/D                    |
| Almacenamiento de blobs         | Blob               | Estándar                    | Frecuente, esporádico, archivo*    |

No se recomienda usar el contenedor de blobs predeterminado para almacenar datos empresariales. Conviene eliminar el contenedor de blobs predeterminado después de cada uso para reducir los costos de almacenamiento. El contenedor predeterminado contiene los registros del sistema y de la aplicación. Asegúrese de recuperar los registros antes de eliminar el contenedor.

No se permite usar un contenedor de blobs como el sistema de archivos predeterminado entre varios clústeres.
 
 > [!NOTE]  
 > El nivel de acceso de archivo es un nivel sin conexión que tiene una latencia de recuperación de varias horas y no se recomienda para su uso con HDInsight. Para más información, consulte [Nivel de acceso de archivo](../storage/blobs/storage-blob-storage-tiers.md#archive-access-tier).

### <a name="hdinsight-storage-architecture"></a>Arquitectura de almacenamiento de HDInsight
El diagrama siguiente proporciona una panorámica de la arquitectura de almacenamiento de HDInsight disponible al utilizar Azure Storage:

![Los clústeres de Hadoop usan la API de HDFS para acceder y almacenar datos estructurados y no estructurados en Blob Storage.](./media/hdinsight-hadoop-compare-storage-options/HDI.WASB.Arch.png "Arquitectura de almacenamiento para HDInsight")

HDInsight brinda acceso al sistema de archivos distribuidos que se adjunta localmente a los nodos de ejecución. Se puede acceder a este sistema de archivos usando el URI completo, por ejemplo:

    hdfs://<namenodehost>/<path>

Además, HDInsight le permite acceder a los datos almacenados en Azure Storage. La sintaxis es:

    wasb[s]://<containername>@<accountname>.blob.core.windows.net/<path>

A la hora de usar una cuenta de Azure Storage con clústeres de HDInsight, es necesario tener en cuenta algunas cosas.

* **Contenedores de las cuentas de almacenamiento que se conectan a un clúster:** dado que el nombre y la clave de la cuenta se asocian al clúster durante la creación, tiene acceso total a los blobs de dichos contenedores.

* **Contenedores públicos o blobs públicos de las cuentas de almacenamiento que NO se conectan a un clúster:** tiene permiso de solo lectura de los blobs de los contenedores.
  
  > [!NOTE]  
  > Los contenedores públicos le permiten obtener una lista de todos los blobs disponibles del contenedor en cuestión y obtener sus metadatos. Los blobs públicos le permiten acceder a los blobs solo si conoce la URL exacta. Para más información, consulte el artículo sobre la [Administración del acceso a los contenedores y los blobs](../storage/blobs/storage-manage-access-to-resources.md).

* **Contenedores privados de las cuentas de almacenamiento que NO se conectan a un clúster:** no puede tener acceso a los blobs de los contenedores a menos que defina la cuenta de almacenamiento al enviar los trabajos de WebHCat. Esto se explica posteriormente en este artículo.

Las cuentas de almacenamiento definidas en el proceso de creación y sus claves se almacenan en %HADOOP_HOME%/conf/core-site.xml en los nodos de clúster. El comportamiento predeterminado de HDInsight es usar las cuentas de almacenamiento definidas en el archivo core-site.xml. Puede modificar esta configuración mediante [Apache Ambari](./hdinsight-hadoop-manage-ambari.md).

Varios trabajos de WebHCat, incluidos Apache Hive, MapReduce, streaming de Apache Hadoop y Apache Pig, pueden llevar una descripción de cuentas de almacenamiento y metadatos con ellos. (Actualmente esto funciona para Pig con cuentas de almacenamiento pero no para metadatos). Para obtener más información, consulte [Uso de un clúster de HDInsight con cuentas de almacenamiento y tiendas de metadatos alternativas](https://social.technet.microsoft.com/wiki/contents/articles/23256.using-an-hdinsight-cluster-with-alternate-storage-accounts-and-metastores.aspx).

Los blobs se pueden usar para datos estructurados y no estructurados. Los contenedores de blobs almacenan los datos como pares de clave-valor y no hay jerarquía de directorios. No obstante, el carácter de barra diagonal ( / ) se puede usar en el nombre de la clave para que parezca que el archivo está almacenado dentro de una estructura de directorios. Por ejemplo, la clave de un blob puede ser `input/log1.txt`. No hay directorios `input`, pero dada la presencia del carácter de barra diagonal en el nombre de la clave, parece la ruta de un archivo.

### <a id="benefits"></a>Ventajas de Azure Storage
El costo de rendimiento implícito por no tener ubicados juntos los recursos de almacenamiento y clústeres de proceso se ve mitigado por el modo en que los clústeres de proceso se crean cerca de los recursos de la cuenta de almacenamiento dentro de la región de Azure, donde la red de alta velocidad consigue que los nodos de proceso sean eficientes en el acceso a los datos de Azure Storage.

Hay varias ventajas asociadas al almacenamiento de datos en Azure Storage en lugar de en HDFS:

* **Uso compartido y reutilización de datos**: los datos de HDFS se ubican dentro del clúster de proceso. Solamente las aplicaciones que tengan acceso al clúster de cálculo podrán usar los datos usando las API HDFS. Se puede acceder a los datos de Azure Storage mediante las API de HDFS o las API REST de Blob Storage. Por lo tanto, se puede usar un conjunto mayor de aplicaciones (incluyendo otros clústeres de HDInsight) y herramientas para producir y consumir los datos.
* **Archivado de datos**: almacenar los datos en Azure Storage permite que los clústeres de HDInsight usados para el cálculo se eliminen de forma segura sin perder datos del usuario.
* **Costo del almacenamiento de datos:** almacenar datos en DFS a largo plazo es más caro que almacenarlos en Azure Storage, ya que el costo de un clúster de proceso es superior al de Azure Storage. Además, como no hay que volver a cargar los datos para cada generación de clúster de proceso, también se ahorra en costos de carga de datos.
* **Escalabilidad horizontal elástica**: aunque HDFS proporciona un sistema de archivos escalable en horizontal, la escala se determina en función del número de nodos que cree para su clúster. Cambiar la escala puede ser un proceso más complicado que basarse en las funcionalidades de escalado elástico que se obtienen automáticamente en Azure Storage.
* **Replicación geográfica:** su almacenamiento de Azure Storage se puede replicar geográficamente. Aunque esto le aporta recuperación geográfica y redundancia de datos, una conmutación por error en la ubicación replicada geográficamente afecta gravemente a su rendimiento y puede incurrir en costes adicionales. Por lo tanto, nuestra recomendación es que elija la replicación geográfica de forma inteligente y únicamente si merece la pena pagar el coste adicional por el valor de los datos.

Determinados trabajos y paquetes de MapReduce podrían crear resultados intermedios que realmente no desea almacenar en Azure Storage. En tal caso, puede optar por almacenar los datos en el HDFS local. De hecho, HDInsight usa DFS para varios de estos resultados intermedios en los trabajos de Hive y otros procesos.

> [!NOTE]  
> La mayoría de los comandos HDFS (por ejemplo, `ls`, `copyFromLocal` y `mkdir`) siguen funcionando según lo previsto. Únicamente los comandos específicos de la implementación nativa de HDFS (a la que nos referiremos como DFS), como `fschk` y `dfsadmin`, muestran comportamientos diferentes en Azure Storage.

## <a name="use-azure-data-lake-storage-gen1"></a>Uso de Azure Data Lake Storage Gen1

### <a name="overview"></a>Información general

Para obtener más información sobre Azure Data Lake Storage Gen1, consulte [Información general de Azure Data Lake Storage Gen1](../data-lake-store/data-lake-store-overview.md).

Azure Data Lake Storage Gen1 es un repositorio a gran escala de nivel empresarial para cargas de trabajo de análisis de macrodatos. Azure Data Lake permite capturar datos de cualquier tamaño, tipo y velocidad de ingesta en un único lugar para realizar análisis exploratorios y operativos.

Se puede acceder a Data Lake Storage Gen1 desde Hadoop (disponible con un clúster de HDInsight) mediante las API REST compatibles con WebHDFS. Está diseñado para habilitar el análisis de los datos almacenados y está optimizado para el rendimiento en escenarios de análisis de datos. De forma inmediata, incluye todas las capacidades de nivel empresarial –seguridad, facilidad de administración, escalabilidad, confiabilidad y disponibilidad– esenciales para los casos de uso empresariales reales.

![Azure Data Lake Storage](./media/hdinsight-hadoop-compare-storage-options/data-lake-store-concept.png "Arquitectura de Almacenamiento para HDInsight")

Entre las capacidades clave de Data Lake Storage Gen1, se incluyen las siguientes.

#### <a name="built-for-hadoop"></a>Creado para Hadoop

Data Lake Storage Gen1 es un sistema de archivos de Apache Hadoop compatible con el Sistema de archivos distribuido de Hadoop (HDFS) que funciona con el ecosistema de Hadoop.  Las aplicaciones o los servicios de HDInsight existentes que usen la API de WebHDFS se pueden integrar fácilmente con Data Lake Storage Gen1. Además, Data Lake Storage Gen1 expone una interfaz de REST compatible con WebHDFS para aplicaciones.

Los datos almacenados en Data Lake Storage Gen1 se pueden analizar fácilmente mediante marcos analíticos de Hadoop como MapReduce o Hive. Los clústeres de Microsoft Azure HDInsight se pueden aprovisionar y configurar para que accedan directamente a datos almacenados en Data Lake Storage Gen1.

#### <a name="unlimited-storage-petabyte-files"></a>Almacenamiento ilimitado, archivos de petabytes de tamaño

Data Lake Storage Gen1 proporciona almacenamiento ilimitado y es adecuado para almacenar diversos datos para análisis. No se impone ningún límite al tamaño de cuenta, el tamaño de archivo o la cantidad de datos que se pueden almacenar en un Data Lake. El tamaño de los archivos individuales puede oscilar entre kilobytes y petabytes, por lo que es una buena opción para almacenar cualquier tipo de datos. Los datos se almacenan de forma duradera mediante la realización de varias copias y no hay ningún límite al período de tiempo durante el que se pueden almacenar los datos en Data Lake.

#### <a name="performance-tuned-for-big-data-analytics"></a>Rendimiento optimizado para el análisis de macrodatos

Data Lake Storage Gen1 se creó para ejecutar sistemas de análisis a gran escala que requieren un procesamiento masivo con el fin de consultar y analizar grandes cantidades de datos. Data Lake distribuye partes de un archivo entre varios servidores de almacenamiento individuales. Esto mejora el rendimiento de lectura cuando se lee el archivo en paralelo para realizar análisis de datos.

#### <a name="enterprise-ready-highly-available-and-secure"></a>Preparada para la empresa: durable y con una elevada disponibilidad

Data Lake Storage Gen1 proporciona la confiabilidad y disponibilidad estándar del sector. Los recursos de datos se almacenan de forma duradera realizando copias redundantes para protegerse ante los errores inesperados. Las empresas pueden usar Data Lake Storage Gen1 en sus soluciones como parte importante de su plataforma de datos existente.

Además, Data Lake Storage Gen1 también proporciona seguridad de nivel empresarial para los datos almacenados. Para obtener más información, consulte [Securing data in Azure Data Lake Storage Gen1](#DataLakeStoreSecurity) (Protección de los datos almacenados en Azure Data Lake Storage Gen1).

#### <a name="all-data"></a>Todos los datos

Data Lake Storage Gen1 puede almacenar cualquier dato en su formato nativo, tal cual, sin necesidad de transformarlo antes. Asimismo, Data Lake Storage Gen1 no requiere la definición de un esquema antes de que se carguen los datos, sino que deja que cada marco analítico interprete los datos y defina un esquema en el momento del análisis. La capacidad de almacenar archivos de formatos y tamaños arbitrarios hace posible que Data Lake Storage Gen1 administre datos estructurados, semiestructurados y no estructurados.

Los contenedores de datos de Data Lake Storage Gen1 son básicamente carpetas y archivos. Se opera en los datos almacenados mediante los SDK, Azure Portal y Azure Powershell. Siempre que ponga los datos en el almacén mediante estas interfaces y los contenedores adecuados, puede almacenar cualquier tipo de datos. Data Lake Storage Gen1 no realiza ningún control especial de datos según el tipo de datos que almacene.

### <a name="DataLakeStoreSecurity"></a>Protección de datos en Data Lake Storage Gen1
Data Lake Storage Gen1 usa Azure Active Directory para la autenticación y listas de control de acceso (ACL) para administrar el acceso a los datos.

| Característica | DESCRIPCIÓN |
| --- | --- |
| Authentication |Data Lake Storage Gen1 se integra con Azure Active Directory (AAD) para la administración de identidades y accesos para todos los datos almacenados en Data Lake Storage Gen1. Como resultado de la integración, Data Lake Storage Gen1 se beneficia de todas las características de AAD, lo que incluye la autenticación multifactor, el acceso condicional, el control de acceso basado en roles, la supervisión del uso de aplicaciones, la supervisión y las alertas de seguridad, etc. Igualmente, Data Lake Storage Gen1 es compatible con el protocolo OAuth 2.0 para la autenticación en la interfaz de REST. Consulte [autenticación de Data Lake Storage Gen1](../data-lake-store/data-lakes-store-authentication-using-azure-active-directory.md).|
| Control de acceso |Data Lake Storage Gen1 proporciona control de acceso gracias a la compatibilidad con los permisos de estilo POSIX que expone el protocolo WebHDFS. Las listas de control de acceso se pueden habilitar en la carpeta raíz, en subcarpetas y en archivos individuales. Para obtener más información sobre cómo funcionan las ACL en el contexto de Data Lake Storage Gen1, consulte [Control de acceso en Data Lake Storage Gen1](../data-lake-store/data-lake-store-access-control.md). |
| Cifrado |Data Lake Storage Gen1 también proporciona el cifrado de los datos que se almacenan en la cuenta. Puede especificar la configuración de cifrado mientras crea una cuenta de Data Lake Storage Gen1. Puede elegir si cifrar o no los datos. Para obtener más información, consulte el artículo de [Cifrado de datos en Data Lake Storage Gen1](../data-lake-store/data-lake-store-encryption.md). Para obtener instrucciones sobre cómo proporcionar una configuración relacionada con el cifrado, consulte [Introducción a Azure Data Lake Storage Gen1 con Azure Portal](../data-lake-store/data-lake-store-get-started-portal.md). |

¿Quiere obtener más información acerca cómo proteger los datos en Data Lake Storage Gen1? siga estos vínculos.

* Para obtener instrucciones sobre cómo proteger datos en el Data Lake Storage Gen1, consulte [Securing data in Azure Data Lake Storage Gen1](../data-lake-store/data-lake-store-secure-data.md) (Proteger los datos en Azure Data Lake Storage Gen1).

### <a name="applications-compatible-with-data-lake-storage-gen1"></a>Aplicaciones compatibles con Data Lake Storage Gen1
Data Lake Storage Gen1 es compatible con la mayor parte de los componentes de código abierto del ecosistema de Hadoop. También se integra perfectamente con otros servicios de Azure.  Para obtener más información acerca de cómo se puede usar Data Lake Storage Gen1 con componentes de código abierto y con otros servicios de Azure, siga estos vínculos.

* Consulte [Aplicaciones y servicios compatibles con Azure Data Lake Storage Gen1](../data-lake-store/data-lake-store-compatible-oss-other-applications.md) para obtener una lista de aplicaciones de código abierto interoperables con Data Lake Storage Gen1.
* Consulte la página sobre la [integración con otros servicios de Azure](../data-lake-store/data-lake-store-integrate-with-other-services.md) para saber cómo usar Data Lake Storage Gen1 con otros servicios de Azure, y así poder obtener una gama más amplia de escenarios.
* Consulte [Scenarios for using Data Lake Storage Gen1](../data-lake-store/data-lake-store-data-scenarios.md) (Escenarios para usar Data Lake Storage Gen1) para obtener información acerca de cómo usar Data Lake Storage Gen1 en escenarios como la ingesta de datos, el procesamiento de datos, la descarga de datos y la visualización de datos.

### <a name="what-is-data-lake-storage-gen1-file-system-adl"></a>¿Qué es el sistema de archivos de Data Lake Storage Gen1 (adl://)?
En entornos de Hadoop (disponibles con el clúster de HDInsight), se puede acceder a Data Lake Storage Gen1 a través del nuevo sistema de archivos, AzureDataLakeFilesystem (adl://). Las aplicaciones y los servicios que usan adl:// pueden aprovechar aún más la optimización del rendimiento que no está actualmente disponible en WebHDFS. Como resultado, Data Lake Storage Gen1 ofrece la posibilidad de elegir entre disponer del mejor rendimiento con la opción recomendada que usa adl:// o mantener el código existente usando la API de WebHDFS directamente. Azure HDInsight aprovecha completamente AzureDataLakeFilesystem para proporcionar el mejor rendimiento en Data Lake Storage Gen1.

Puede obtener acceso a los datos en Data Lake Storage Gen1 mediante `adl://<data_lake_storage_gen1_name>.azuredatalakestore.net`. Para obtener más información sobre cómo acceder a los datos en Data Lake Storage Gen1, consulte [View properties of the stored data](../data-lake-store/data-lake-store-get-started-portal.md#properties) (Ver las propiedades de los datos almacenados).



## <a name="next-steps"></a>Pasos siguientes

* [Introducción a Azure Data Lake Storage Gen2](../storage/blobs/data-lake-storage-introduction.md).
* [Introducción a Almacenamiento de Azure](../storage/common/storage-introduction.md)