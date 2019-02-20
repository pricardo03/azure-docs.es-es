---
title: Actualización de las soluciones de análisis de macrodatos de Azure Data Lake Storage Gen1 a Azure Data Lake Storage Gen2
description: Actualización de una solución para usar Azure Data Lake Storage Gen2
services: storage
author: normesta
ms.topic: conceptual
ms.author: normesta
ms.date: 02/07/2019
ms.service: storage
ms.subservice: data-lake-storage-gen2
ms.openlocfilehash: feb656fcdbb98dcab0a3ccf372a05b3f3dca91c4
ms.sourcegitcommit: d1c5b4d9a5ccfa2c9a9f4ae5f078ef8c1c04a3b4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/08/2019
ms.locfileid: "55961387"
---
# <a name="upgrade-your-big-data-analytics-solutions-from-azure-data-lake-storage-gen1-to-azure-data-lake-storage-gen2"></a>Actualización de las soluciones de análisis de macrodatos de Azure Data Lake Storage Gen1 a Azure Data Lake Storage Gen2

Si usa Azure Data Lake Storage Gen1 en sus soluciones de análisis de macrodatos, esta guía lo ayudará a actualizar dichas soluciones para usar Azure Data Lake Storage Gen2. Puede usar este documento para evaluar las dependencias que la solución tiene de Data Lake Storage Gen1. En esta guía también se muestra cómo planear y llevar a cabo la actualización.

Lo ayudaremos a través de las tareas siguientes:

:heavy_check_mark: Evaluación de la preparación para la actualización

:heavy_check_mark: Planeación de la actualización

:heavy_check_mark: Realización de la actualización

## <a name="assess-your-upgrade-readiness"></a>Evaluación de la preparación para la actualización

Nuestro objetivo es que todas las funcionalidades que existen en Data Lake Storage Gen1 estén disponibles en Data Lake Storage Gen2. La exposición de esas funcionalidades, por ejemplo, en SDK, CLI, etc. puede ser distinta entre Data Lake Storage Gen1 y Data Lake Storage Gen2. Las aplicaciones y los servicios que funcionan con Data Lake Storage Gen1 deben poder funcionar de una manera similar con Data Lake Storage Gen2. Por último, debe tener en cuenta que algunas de las funcionalidades no estarán disponibles de inmediato en Data Lake Storage Gen2. En este documento anunciaremos cuando estén disponibles.

Las secciones siguientes lo ayudarán a decidir la mejor manera de realizar la actualización a Data Lake Storage Gen2 y cuándo tendría más sentido hacerlo.

### <a name="data-lake-storage-gen1-solution-components"></a>Componentes de las soluciones de Data Lake Storage Gen1

Es muy probable que, cuando use Data Lake Storage Gen1 en las canalizaciones o soluciones de análisis, sean muchas las tecnologías adicionales que usa para alcanzar una funcionalidad integral global. En este [artículo](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-data-scenarios) describe distintos componentes del flujo de datos que incluyen la ingesta, el procesamiento y el consumo de datos.

Además, hay componentes transversales que aprovisionan, administran y supervisan estos componentes. Cada uno de los componentes funcionan con Data Lake Storage Gen1 mediante el uso de la interfaz que mejor se adapta a ellos. Cuando planea actualizar la solución a Data Lake Storage Gen2, debe tener en cuenta las interfaces que se usan. Deberá actualizar tanto las interfaces de administración como las interfaces de datos, porque cada una de ellas tiene requisitos distintos.

![Componentes de las soluciones de Data Lake Store](./media/data-lake-storage-upgrade/solution-components.png)

En la **Ilustración 1**, se muestra los componentes de la funcionalidad que podría ver en la mayoría de las soluciones de análisis.

En la **Ilustración 2**, se muestra un ejemplo de cómo se implementarán esos componentes mediante tecnologías específicas.

La funcionalidad de almacenamiento que aparece en la **Ilustración 1** la proporciona Data Lake Storage Gen1 (**Ilustración 2**). Observe cómo los distintos componentes del flujo de datos interactúan con Data Lake Storage Gen1 mediante API REST o el SDK de Java. Observe también cómo interactúan los componentes de funcionalidad transversales con Data Lake Storage Gen1. El componente de aprovisionamiento usa plantillas de recursos de Azure, mientras que el componente de supervisión que usa Log Analytics utiliza datos operativos provenientes de Data Lake Storage Gen1.

Para actualizar una solución que usa Data Lake Storage Gen1 para que use Data Lake Storage Gen2, deberá copiar los datos y metadatos, volver a enlazar los flujos de datos y, luego, todos los componentes deberán poder funcionar con Data Lake Storage Gen2.

En las secciones siguientes se proporciona información para ayudarlo a tomar mejores decisiones:

:heavy_check_mark: Funcionalidades de plataforma

:heavy_check_mark: Interfaces de programación

:heavy_check_mark: Ecosistema de Azure

:heavy_check_mark: Ecosistema del asociado

:heavy_check_mark: Información operativa

En cada sección, podrá determinar los "indispensables" de la actualización. Una vez que esté seguro de que las funcionalidades están disponibles o de que tenga claro que se aplicaron soluciones alternativas razonables, pase a la sección [Planeamiento de una actualización](#planning-for-an-upgrade) de esta guía.

### <a name="platform-capabilities"></a>Funcionalidades de plataforma

En esta sección se describen las funcionalidades de plataforma de Data Lake Storage Gen1 que actualmente se encuentran disponibles en Data Lake Storage Gen2.

| | Data Lake Storage Gen1 | Data Lake Storage Gen2: objetivo | Data Lake Storage Gen2: estado de disponibilidad  |
|-----------------------|-----------------|----------------------|----------------------------------|
| Organización de datos| Admite datos almacenados como carpetas y archivos | Admite datos almacenados como objetos/blobs, así como también carpetas y archivos: [vínculo](https://docs.microsoft.com/azure/storage/data-lake-storage/namespace) | Admite datos almacenados como carpetas y archivos: *ya disponible* <br><br> Admite datos almacenados como objetos o blobs: *no disponible aún* |
| Espacio de nombres| Jerárquico | Jerárquico |  *Ya disponible*  |
| API  | API de REST a través de HTTPS | API de REST a través de HTTP/HTTPS| *Ya disponible* |
| API de servidor| [WebHDFS-compatible REST API (API de REST compatible con WebHDFS)](https://msdn.microsoft.com/library/azure/mt693424.aspx) | API REST de Azure Blob Service [API REST de Data Lake Storage Gen2](https://docs.microsoft.com/rest/api/storageservices/data-lake-storage-gen2) | API de REST de Data Lake Storage Gen2: *ya disponible* <br><br> API de REST de Azure Blob Service: *no disponible aún*       |
| Cliente de sistema de archivos de Hadoop | Sí ([Azure Data Lake Storage](https://hadoop.apache.org/docs/current/hadoop-azure-datalake/index.html)) | Sí ([ABFS](https://jira.apache.org/jira/browse/HADOOP-15407))  | *Ya disponible*  |  | [Clave compartida](https://docs.microsoft.com/rest/api/storageservices/authorize-with-shared-key), [Identidades de Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-authentication-scenarios), [Firma de acceso compartido (SAS)](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1?toc=%2fazure%2fstorage%2fqueues%2ftoc.json) | *Ya disponible*  |
| Operaciones de datos: autorización  | Listas de control de acceso (ACL) de POSIX a nivel de archivo y carpeta basadas en Identidades de Azure Active Directory  | Listas de control de acceso (ACL) de POSIX a nivel de archivo y carpeta basadas en Identidades de Azure Active Directory [Clave compartida](https://docs.microsoft.com/rest/api/storageservices/authorize-with-shared-key) para el control de acceso basado en rol ([RBAC](https://docs.microsoft.com/azure/storage/common/storage-auth-aad-rbac)) de autorización a nivel de cuenta para acceder a los contenedores | *Ya disponible* |
| Operaciones de datos: registros  | Sí | Solicitudes de registros únicas con una duración específica mediante el uso de la incidencia de soporte técnico Integración de Supervisión de Azure | Solicitudes de registros únicas con una duración específica mediante el uso de la incidencia de soporte técnico: *ya disponible*<br><br> Integración en supervisión de Azure: *no disponible aún* |
| Cifrado de datos en reposo | Transparente, del servidor con claves administradas por el servicio y con claves administradas por el cliente en Azure Key Vault | Transparente, del servidor con claves administradas por el servicio y con claves administradas por el cliente en Azure Key Vault | Claves administradas por el servicio: *ya disponible*<br><br> Claves administradas por el cliente: *ya disponible*  |
| Compatibilidad con redes virtuales (VNet)  | [Uso de la integración de redes virtuales](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-network-security)  | [Uso del punto de conexión de servicio para Azure Storage](https://docs.microsoft.com/azure/storage/common/storage-network-security?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) | *Ya disponible* | Operaciones de administración (por ejemplo, creación de cuentas) | [Control de acceso basado en rol](https://docs.microsoft.com/azure/role-based-access-control/overview) (RBAC) proporcionado por Azure para la administración de cuentas | [Control de acceso basado en rol](https://docs.microsoft.com/azure/role-based-access-control/overview) (RBAC) proporcionado por Azure para la administración de cuentas | *Ya disponible*| SDK para desarrolladores | .NET, Java, Python, Node.js  | .NET, Java, Python, Node.js, C++, Ruby, PHP, Go, Android, iOS| *No disponible aún* | Rendimiento optimizado para cargas de trabajo de análisis paralelas. Gran capacidad de procesamiento e IOPS | Rendimiento optimizado para cargas de trabajo de análisis paralelas. Gran capacidad de procesamiento e IOPS | *Ya disponible* |
| Límites de tamaño | Sin límites para el tamaño de cuenta, de archivo o el número de archivos | Sin límites para el tamaño de cuenta o el número de archivos. Tamaño de archivo limitado a 5 TB. | *Ya disponible*|
| Redundancia geográfica| Almacenamiento con redundancia local (LRS) | Con redundancia local (LRS), con redundancia de zona (ZRS), con redundancia global (GRS), con redundancia global de acceso de lectura (RA-GRS) Consulte [aquí](https://docs.microsoft.com/azure/storage/common/storage-redundancy) para más información| *Ya disponible* |
| Disponibilidad regional | Consulte [aquí](https://azure.microsoft.com/regions/) | Todas las [regiones de Azure](https://azure.microsoft.com/global-infrastructure/regions/)                                                                                                                                                                                                                                                                                                                                       | *Ya disponible*                                                                                                                           |
| Precio                                       | Consulte [Precios](https://azure.microsoft.com/pricing/details/data-lake-store/)                                                                            | Consulte [Precios](https://azure.microsoft.com/pricing/details/storage/data-lake/)                                                                                                                                                                                                                                                                                                                                         |                                                                                                                                           |
| SLA de disponibilidad                            | [Consulte el Acuerdo de Nivel de Servicio](https://azure.microsoft.com/support/legal/sla/data-lake-store/v1_0/)                                                                   | [Consulte el Acuerdo de Nivel de Servicio](https://azure.microsoft.com/support/legal/sla/storage/v1_3/)                                                                                                                                                                                                                                                                                                                                                | *Ya disponible*                                                                                                                           |
| Administración de datos                             | Expiración de archivo                                                                                                                                        | Directivas de ciclo de vida                                                                                                                                                                                                                                                                                                                                                                                                          | *No disponible aún*                                                                                                                       |

### <a name="programming-interfaces"></a>Interfaces de programación

En esta tabla se describe los conjuntos de API que están disponibles para las aplicaciones personalizadas. Para que todo quede un poco más claro, separamos estos conjuntos de API en dos tipos: API de administración y API de sistema de archivos.

Las API de administración lo ayudan a administrar las cuentas, mientras que las API de sistema de archivos lo ayudan a realizar operaciones en los archivos y las carpetas.

|  Conjunto de API                           |  Data Lake Storage Gen1                                                                                                                                                                                                                                                                                                   | Disponibilidad para Data Lake Storage Gen2, con autenticación de clave compartida | Disponibilidad para Data Lake Storage Gen2, con autenticación OAuth                                                                                                  |
|----------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------|
| SDK de .NET: administración                  | [Vínculo](https://docs.microsoft.com/dotnet/api/overview/azure/datalakestore/management?view=azure-dotnet)                                                                                                                                                                                                                 | *No compatible*                                                      | *Ya disponible:* [vínculo](https://docs.microsoft.com/rest/api/storageservices/operations-on-the-account--blob-service-)                                    |
| SDK de .NET: sistema de archivos                  | [Vínculo](https://docs.microsoft.com/dotnet/api/overview/azure/datalakestore/client?view=azure-dotnet)                                                                                                                                                                                                                     | *No disponible aún*                                                | *No disponible aún*                                                                                                                                             |
| SDK de Java: administración                  | [Vínculo](https://docs.microsoft.com/java/api/overview/azure/datalakestore/management)                                                                                                                                                                                                                                     | *No compatible*                                                      | *Ya disponible:* [vínculo](https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob?view=azure-java-stable)                                     |
| SDK de Java: sistema de archivos                  | [Vínculo](https://docs.microsoft.com/java/api/overview/azure/datalake)                                                                                                                                                                                                                                         | *No disponible aún*                                                | *No disponible aún*                                                                                                                                             |
| Node.js: administración                   | [Vínculo](https://www.npmjs.com/package/azure-arm-datalake-store)                                                                                                                                                                                                                                                                | No compatible                                                      | *Ya disponible:* [vínculo](http://azure.github.io/azure-storage-node/)                                                                                            |
| Node.js: sistema de archivos                   | [Vínculo](https://www.npmjs.com/package/azure-arm-datalake-store)                                                                                                                                                                                                                                                                | *No disponible aún*                                                | *No disponible aún*                                                                                                                                             |
| Python: administración                    | [Vínculo](https://docs.microsoft.com/python/api/overview/azure/datalakestore/management?view=azure-python)                                                                                                                                                                                                                 | *No compatible*                                                      | *Ya disponible:* [vínculo](https://docs.microsoft.com/python/api/overview/azure/storage/management?view=azure-python)                                       |
| Python: sistema de archivos                    | [Vínculo](http://azure-datalake-store.readthedocs.io/en/latest/)                                                                                                                                                                                                                                                                 | *No disponible aún*                                                | *No disponible aún*                                                                                                                                             |
| API REST: administración                  | [Vínculo](https://docs.microsoft.com/rest/api/datalakestore/accounts)                                                                                                                                                                                                                                                      | *No compatible*                                                      | *Ya disponible*                                                                                                                                               |
| API REST: sistema de archivos                  | [Vínculo](https://docs.microsoft.com/rest/api/datalakestore/webhdfs-filesystem-apis)                                                                                                                                                                                                                                       | *Ya disponible*                                                    | *Ya disponible:* [vínculo](https://docs.microsoft.com/rest/api/storageservices/data-lake-storage-gen2)                                                      |
| PowerShell: administración y sistema de archivos | [Vínculo](https://docs.microsoft.com/powershell/module/az.datalakestore)                                                                                                                                                                                                                        | Administración: no compatible Sistema de archivos: *no disponible aún*        | Administración: *ya disponible*, [vínculo](https://docs.microsoft.com/powershell/module/az.storage) <br><br>Sistema de archivos: *no disponible aún* |
| CLI: administración                       | [Vínculo](https://docs.microsoft.com/cli/azure/dls/account?view=azure-cli-latest)                                                                                                                                                                                                                                          | *No compatible*                                                      | *Ya disponible:* [vínculo](https://docs.microsoft.com/cli/azure/storage?view=azure-cli-latest)                                                              |
| CLI: sistema de archivos                       | [Vínculo](https://docs.microsoft.com/cli/azure/dls/fs?view=azure-cli-latest)                                                                                                                                                                                                                                               | *No disponible aún*                                                | *No disponible aún*                                                                                                                                             |
| Plantillas de Azure Resource Manager: administración             | [Template1](https://azure.microsoft.com/resources/templates/101-data-lake-store-no-encryption/)  [Template2](https://azure.microsoft.com/resources/templates/101-data-lake-store-encryption-adls/)  [Template3](https://azure.microsoft.com/resources/templates/101-data-lake-store-encryption-key-vault/)  | *No compatible*                                                      | *Ya disponible:* [vínculo](https://docs.microsoft.com/azure/templates/microsoft.storage/2018-07-01/storageaccounts)                                         |

### <a name="azure-ecosystem"></a>Ecosistema de Azure

Cuando se usa Data Lake Storage Gen1, puede usar una variedad de productos y servicios de Microsoft en las canalizaciones completas. Estos servicios y productos funcionan con Data Lake Storage Gen1 ya sea de manera directa o indirecta. En esta tabla se muestra una lista de los servicios que se han modificado para funcionar con Data Lake Storage Gen1 y también los que actualmente son compatibles con Data Lake Storage Gen2.

| **Ámbito**             | **Disponibilidad para Data Lake Storage Gen1**                                                                                                                                    | **Disponibilidad para Data Lake Storage Gen2, con autenticación de clave compartida**                                                                                                           | **Disponibilidad para Data Lake Storage Gen2, con OAuth**                                                                                        |
|----------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------|
| Marco de análisis  | [Apache Hadoop](https://hadoop.apache.org/docs/current/hadoop-azure-datalake/index.html)                                                                                       | *Ya disponible*                                                                                                                                                              | *Ya disponible*                                                                                                                                 |
|                      | [HDInsight ](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-hdinsight-hadoop-use-portal)                                                               | [HDInsight](https://docs.microsoft.com/azure/storage/data-lake-storage/quickstart-create-connect-hdi-cluster) 3.6: *ya disponible* HDInsight 4.0: *no disponible aún*      | HDInsight 3.6 ESP: *ya disponible* <br><br>  HDInsight 4.0 ESP: *no disponible aún*                                                                 |
|                      | Databricks Runtime 3.1 y versiones superiores                                                                                                                                               | [Databricks Runtime 5.1 y versiones superiores](https://docs.azuredatabricks.net/spark/latest/data-sources/azure/azure-datalake-gen2.html#azure-data-lake-storage-gen2): *ya disponible* | [Databricks Runtime 5.1 y versiones superiores](https://docs.azuredatabricks.net/spark/latest/data-sources/azure/azure-datalake-gen2.html#azure-data-lake-storage-gen2): *ya disponible*                                                                                              |
|                      | [SQL Data Warehouse ](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-load-from-azure-data-lake-store)                                            | *No compatible*                                                                                                                                                              | *Ya disponible:* [vínculo](https://docs.microsoft.com/sql/t-sql/statements/create-external-data-source-transact-sql?view=sql-server-2017) |
| Integración de datos     | [Factoría de datos ](https://docs.microsoft.com/azure/data-factory/load-azure-data-lake-store)                                                                                | [Version 2](https://docs.microsoft.com/azure/data-factory/connector-azure-data-lake-storage): *ya disponible* Versión 1: *no compatible*                               | [Versión 2](https://docs.microsoft.com/azure/data-factory/connector-azure-data-lake-storage): *ya disponible* <br><br> Versión 1: *no compatible*  |
|                      | [AdlCopy](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-copy-data-azure-storage-blob)                                                                 | *No compatible*                                                                                                                                                              | *No compatible*                                                                                                                                 |
|                      | [SQL Server Integration Services ](https://docs.microsoft.com/sql/integration-services/connection-manager/azure-data-lake-store-connection-manager?view=sql-server-2017) | *No disponible aún*                                                                                                                                                          | *No disponible aún*                                                                                                                             |
|                      | [Data Catalog](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-with-data-catalog)                                                                       | *No disponible aún*                                                                                                                                                          | *No disponible aún*                                                                                                                             |
|                      | [Logic Apps ](https://docs.microsoft.com/connectors/azuredatalake/)                                                                                                      | *No disponible aún*                                                                                                                                                          | *No disponible aún*                                                                                                                             |
| IoT                  | [Event Hubs: Capture ](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-archive-eventhub-capture)                                                       | *No disponible aún*                                                                                                                                                          | *No disponible aún*                                                                                                                             |
|                      | [Stream Analytics ](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-stream-analytics)                                                                   | *No disponible aún*                                                                                                                                                          | *No disponible aún*                                                                                                                             |
| Consumo          | [PowerBI Desktop  ](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-power-bi)                                                                           | *No disponible aún*                                                                                                                                                          | *No disponible aún*                                                                                                                             |
|                      | [Excel ](https://techcommunity.microsoft.com/t5/Excel-Blog/Announcing-the-Azure-Data-Lake-Store-Connector-in-Excel/ba-p/91677)                                                 | *No disponible aún*                                                                                                                                                          | *No disponible aún*                                                                                                                             |
|                      | [Analysis Services ](https://blogs.msdn.microsoft.com/analysisservices/2017/09/05/using-azure-analysis-services-on-top-of-azure-data-lake-storage/)                            | *No disponible aún*                                                                                                                                                          | *No disponible aún*                                                                                                                             |
| Productividad         | [Azure Portal](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-get-started-portal)                                                                      | *No compatible*                                                                                                                                                              | Administración de cuentas: *ya disponible* <br><br>Operaciones de datos *:* *no disponible aún*                                                                   |
|                      | [Data Lake Tools para Visual Studio ](https://docs.microsoft.com/azure/data-lake-analytics/data-lake-analytics-data-lake-tools-install)                                   | *No disponible aún*                                                                                                                                                          | *No disponible aún*                                                                                                                             |
|                      | [Explorador de Azure Storage ](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-in-storage-explorer)                                                          | *Ya disponible*                                                                                                                                                              | *Ya disponible*                                                                                                                                 |
|                      | [Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=usqlextpublisher.usql-vscode-ext)                                                                     | *No disponible aún*                                                                                                                                                          | *No disponible aún*                                                                                                                             |

### <a name="partner-ecosystem"></a>Ecosistema del asociado

En esta tabla se muestra una lista de los servicios y productos de terceros que se modificaron para funcionar con Data Lake Storage Gen1. También se muestra cuáles son compatibles actualmente con Data Lake Storage Gen2.

| Ámbito            | Asociado  | Producto o servicio  | Disponibilidad para Data Lake Storage Gen1                                                                                                                                               | Disponibilidad para Data Lake Storage Gen2, con autenticación de clave compartida                                                   | Disponibilidad para Data Lake Storage Gen2, con OAuth                                                             |
|---------------------|--------------|----------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------|
| Marco de análisis | Cloudera     | CDH                  | [Vínculo](https://www.cloudera.com/documentation/enterprise/5-11-x/topics/admin_adls_config.html)                                                                                            | *No disponible aún*                                                                                                  | [Vínculo](https://www.cloudera.com/documentation/enterprise/latest/topics/admin_adls2_config.html)                                                                                                  |
|                     | Cloudera     | Altus                | [Vínculo](https://www.cloudera.com/more/news-and-blogs/press-releases/2017-09-27-cloudera-introduces-altus-data-engineering-microsoft-azure-hybrid-multi-cloud-data-analytic-workflows.html) | *No compatible*                                                                                                                  | *No disponible aún*                                                                                                  |
|                     | HortonWorks  | HDP 3.0              | [Vínculo](https://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.6.3/bk_cloud-data-access/content/adls-get-started.html)                                                                       | *No disponible aún*                                                                                                  | *No disponible aún*                                                                                                  |
|                     | Qubole       |                      | [Vínculo](https://www.qubole.com/blog/big-data-analytics-microsoft-azure-data-lake-store-qubole/)                                                                                            | *No disponible aún*                                                                                                  | *No disponible aún*                                                                                                  |
| ETL                 | StreamSets   |                      | [Vínculo](https://streamsets.com/blog/ingest-data-azure)                                                                                                                                     | *No disponible aún*                                                                                                  | *No disponible aún*                                                                                                  |
|                     | Informatica  |                      | [Vínculo](https://kb.informatica.com/proddocs/Product%20Documentation/6/IC_Spring2017_MicrosoftAzureDataLakeStoreV2ConnectorGuide_en.pdf)                                                    | *No disponible aún*                                                                                                  | *No disponible aún*                                                                                                  |
|                     | Attunity     |                      | [Vínculo](https://www.attunity.com/company/press-releases/microsoft-and-attunity-announce-strategic-partnership-for-data-migration/)                                                         | *No disponible aún*                                                                                                  | *No disponible aún*                                                                                                  |
|                     | Alteryx      |                      | [Vínculo](https://help.alteryx.com/2018.2/DataSources/AzureDataLake.htm)                                                                                                                     | *No disponible aún*                                                                                                  | *No disponible aún*                                                                                                  |
|                     | ImanisData   |                      | [Vínculo](https://www.imanisdata.com/expansion-azure-support-azure-data-lake-store-integration/)                                                                                             | *No disponible aún*                                                                                                  | *No disponible aún*                                                                                                  |
|                     | WANdisco     |                      | [Vínculo](https://azure.microsoft.com/blog/globally-replicated-data-lakes-with-livedata-using-wandisco-on-azure/)                                                                      | [Vínculo](https://azure.microsoft.com/blog/globally-replicated-data-lakes-with-livedata-using-wandisco-on-azure/) | [Vínculo](https://azure.microsoft.com/blog/globally-replicated-data-lakes-with-livedata-using-wandisco-on-azure/) |

### <a name="operational-information"></a>Información operativa

Data Lake Storage Gen1 inserta datos e información específica en otros servicios, lo que lo ayuda a operacionalizar las canalizaciones. En esta tabla se muestra la compatibilidad correspondiente en Data Lake Storage Gen2.

| Tipo de datos                                                                                       | Disponibilidad para Data Lake Storage Gen1                                                                 | Disponibilidad para Data Lake Storage Gen2                                                                                               |
|--------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------|
| Datos de facturación: medidores que se envían al equipo de comercio para facturación y que luego queda a disposición de los clientes  | *Ya disponible*                                                                                             | *Ya disponible*                                                                                                                           |
| Registros de actividad                                                                                          | [Vínculo](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-diagnostic-logs#audit-logs)   | Solicitudes de registros únicas con una duración específica mediante el uso de la incidencia de soporte técnico: *ya disponible* Integración de Supervisión de Azure: *no disponible aún* |
| Registros de diagnóstico                                                                                        | [Vínculo](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-diagnostic-logs#request-logs) | Solicitudes de registros únicas con una duración específica mediante el uso de la incidencia de soporte técnico: *ya disponible* Integración de Supervisión de Azure: *no disponible aún* |
| Métricas                                                                                                | *No compatible*                                                                                               | *Ya disponible:* [vínculo](https://docs.microsoft.com/azure/storage/common/storage-metrics-in-azure-monitor)                          |

## <a name="planning-for-an-upgrade"></a>Planeación de una actualización

En esta sección se da por supuesto que se revisó la sección [Evaluación de la preparación para la actualización](#assess-your-upgrade-readiness) de esta guía y que se cumplen todas las dependencias. Si hay funcionalidades que todavía no están disponibles en Data Lake Storage Gen2, continúe solo si conoce las soluciones alternativas correspondientes. En las secciones siguientes se brinda una guía sobre cómo planear la actualización de las canalizaciones. El proceso mismo de la actualización se describirá en la sección [Realización de la actualización](#performing-the-upgrade) de esta guía.

### <a name="upgrade-strategy"></a>Estrategia de actualización

La parte más crítica de la actualización es decidir la estrategia. Esta decisión determinará las opciones disponibles.

En esta tabla se enumeran estrategias muy conocidas que se han usado para migrar bases de datos, clústeres de Hadoop, etc. En la guía, se adoptarán estrategias similares, las que se adaptarán al contexto en cuestión.

| Estrategia                   | Ventajas                                                                                  | Desventajas                                                           | Cuándo se deben usar                                                                                                                                                                                             |
|--------------------------------|-------------------------------------------------------------------------------------------|--------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Migración mediante lift-and-shift                 | Es más sencilla.                                                                                 | Requiere tiempo de inactividad para copiar los datos, mover los trabajos y mover la entrada y la salida                                             | En las soluciones más sencillas, donde no hay varias soluciones que acceden a la misma cuenta de Gen1 y, por tanto, se pueden mover en conjunto de manera rápida y controlada.                                                  |
| Copia única y copia incremental | Disminuya el tiempo de inactividad al realizar la copia en segundo plano mientras el sistema de origen sigue activo. | Requiere tiempo de inactividad para mover la entrada y la salida.                   | Se va a copiar una gran cantidad de datos y el tiempo de inactividad asociado con la migración mediante lift-and-shift no es aceptable. Pueden necesitarse pruebas con una considerable cantidad de datos de producción en el sistema de destino antes de realizar la transición. |
| Adopción en paralelo              | Un menor tiempo de inactividad permite que las aplicaciones realicen la migración según su propio criterio.           | Más elaborada, porque se requiere una sincronización bidireccional entre ambos sistemas. | En el caso de escenarios complejos en que las aplicaciones basadas en Data Lake Storage Gen1 no se puedan trasladar de una sola vez y se deben mover de manera incremental.                                                      |

A continuación, encontrará más detalles sobre los pasos para cada una de las estrategias. Los pasos indican lo que podría hacer con los componentes que participan de la actualización. Esto incluye el sistema de origen general, el sistema de destino general, los orígenes de entrada del sistema de origen, los destinos de salida del sistema de origen y los trabajos que se ejecutan en el sistema de origen.

Estos pasos no pretenden ser prescriptivos. Están diseñados para establecer el marco que indica cómo se piensa en cada estrategia. Proporcionaremos casos prácticos donde vemos implementadas cada una de las estrategias.

#### <a name="lift-and-shift"></a>Migración mediante lift-and-shift

1. Pause el sistema de origen: orígenes de entrada, trabajos, destinos de salida.
2. Copie todos los datos del sistema de origen al sistema de destino.
3. Apunte todos los orígenes de entrada al sistema de destino. Apunte al destino de salida desde el sistema de destino.
4. Mueva, modifique y ejecute todos los trabajos al sistema de destino.
5. Apague el sistema de origen.

#### <a name="copy-once-and-copy-incremental"></a>Copia única y copia incremental

1. Copie los datos del sistema de origen al sistema de destino.
2. Copie los datos incrementales del sistema de origen al sistema de destino a intervalos regulares.
3. Apunte al destino de salida desde el sistema de destino.
4. Mueva, modifique y ejecute todos los trabajos en el sistema de destino.
5. Apunte los orígenes de entrada de manera incremental al sistema de destino según su preferencia.
6. Una vez que todos los orígenes de entrada apunten al sistema de destino
    1. Apague la copia incremental.
    2. Apague el sistema de origen.

#### <a name="parallel-adoption"></a>Adopción en paralelo

1. Configure el sistema de destino.
2. Configure una replicación bidireccional entre el sistema de origen y el sistema de destino.
3. Apunte los orígenes de entrada de manera incremental al sistema de destino.
4. Mueva, modifique y ejecute trabajos de manera incremental al sistema de destino.
5. Apunte a los destinos de salida de manera incremental desde el sistema de destino.
6. Una vez que todos los orígenes de entrada originales, los trabajos y el destino de salida funcionen con el sistema de destino, apague el sistema de origen.

### <a name="data-upgrade"></a>Actualización de datos

La estrategia general que usa para realizar la actualización (que se describe en la sección [Estrategia de actualización](#upgrade-strategy) de esta guía) determinará las herramientas que se pueden usar para la actualización de los datos. Las herramientas que se muestran a continuación son sugerencias que se basan en información actual. 

#### <a name="tools-guidance"></a>Guía sobre las herramientas

| Estrategia                       | Herramientas                                                                                                             | Ventajas                                                                                                                             | Consideraciones                                                                                                                                                                                                                                                                                                                |
|------------------------------------|-----------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Migración mediante lift-and-shift**                 | [Azure Data Factory](https://docs.microsoft.com/azure/data-factory/load-azure-data-lake-storage-gen2-from-gen1) | Servicio en la nube administrado                                                                                                                | Solo copia los datos. Actualmente, no se pueden copiar las ACL.                                                                                                                                                                                                                                                                      |
|                                    | [Distcp](https://hadoop.apache.org/docs/r1.2.1/distcp.html)                                                           | Herramienta conocida proporcionada por Hadoop. Con esta herramienta se pueden copiar los permisos, es decir, las ACL.                                                   | Requiere un clúster que se pueda conectar a Data Lake Storage Gen1 y Gen2 al mismo tiempo.                                                                                                                                                                                   |
| **Copia única y copia incremental** | Azure Data Factory                                                                                                    | Servicio en la nube administrado                                                                                                                | Para admitir la copia incremental en el ADF, los datos se deben organizar en una manera de serie temporal. El intervalo más breve entre las copias incrementales es de [15 minutos](https://docs.microsoft.com/azure/data-factory/how-to-create-tumbling-window-trigger). En intervalos más breves, no funcionará el ADF. Actualmente, no se pueden copiar las ACL. |
| **Adopción en paralelo**              | [WANdisco](http://docs.wandisco.com/bigdata/wdfusion/adls/)                                                           | Admite la replicación coherente. Si se usa un entorno de Hadoop puro conectado a Azure Data Lake Storage, admite la replicación bidireccional. | Si no se usa un entorno de Hadoop puro, puede haber un retraso en la replicación.                                                                                                                                                                                                                                                  |

Tenga en cuenta que hay terceros que pueden controlar la actualización de Data Lake Storage Gen1 a Data Lake Storage Gen2 sin usar las herramientas de copia de datos o metadatos mencionadas (por ejemplo, [Cloudera](http://blog.cloudera.com/blog/2017/08/use-amazon-s3-with-cloudera-bdr/)). Proporciona una experiencia de "tienda única" que lleva a cabo la migración de datos, además de la migración de las cargas de trabajo. Es posible que tenga que realizar una actualización fuera de banda de cualquier herramienta que esté fuera del ecosistema.

#### <a name="considerations"></a>Consideraciones

* En primer lugar, deberá crear manualmente la cuenta de Data Lake Storage Gen2 antes de empezar con la actualización, debido a que la guía actual no incluye ningún proceso automático para crear una cuenta de Gen2 basado en la información de la cuenta de Gen1. Asegúrese de comparar los procesos de creación de cuentas de [Gen1](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-get-started-portal) y [Gen2](https://docs.microsoft.com/azure/storage/data-lake-storage/quickstart-create-account).

* Data Lake Storage Gen2 solo admite archivos con un tamaño de hasta 5 TB. Para realizar la actualización a Data Lake Storage Gen2, es posible que tenga que cambiar el tamaño de los archivos en Data Lake Storage Gen1 para que tengan un tamaño inferior a los 5 TB.

* Si usa una herramienta que no copia ACL o no quiere copiar las ACL, deberá establecer las ACL en el destino de manera manual en el nivel superior correspondiente. Para ello, puede usar el Explorador de Storage. Asegúrese de que esas ACL sean las ACL predeterminadas para que los archivos y las carpetas que copie las hereden.

* En Data Lake Storage Gen1, el nivel más alto en que puede establecer las ACL es en la raíz de la cuenta. Sin embargo, en Data Lake Storage Gen2, el nivel más alto en que puede establecer las ACL es en la carpeta raíz de un sistema de archivos, no toda la cuenta. Por lo tanto, si quiere establecer las ACL predeterminadas en el nivel de la cuenta, deberá duplicarlas en todos los sistemas de archivos de la cuenta de Data Lake Storage Gen2.

* Las restricciones de nomenclatura de archivos son distintas entre ambos sistemas de almacenamiento. Estas diferencias se refieren especialmente a cuando se copia de Data Lake Storage Gen1 a Data Lake Storage Gen2, porque este último tiene restricciones más estrictas.

### <a name="application-upgrade"></a>Actualización de aplicaciones

Cuando necesita compilar aplicaciones en Data Lake Storage Gen1 o Data Lake Storage Gen2, primero debe elegir una interfaz de programación adecuada. Cuando llame a una API en ese interfaz, deberá proporcionar el URI correspondiente y las credenciales adecuadas. La representación de estos tres elementos (la API, el URI y cómo se proporcionan las credenciales) son distintas en Data Lake Storage Gen1 y Data Lake Storage Gen2. Por tanto, como parte de la actualización de las aplicaciones, deberá asignar estas tres construcciones de manera adecuada.

#### <a name="uri-changes"></a>Cambios en el URI

La principal tarea aquí es transformar el URI adl:// que se usaba en las cargas de trabajo existentes en un URI abfss://.

El esquema de URI de Data Lake Storage Gen1 se menciona detalladamente [aquí](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-use-data-lake-store) pero, en términos generales, es *adl://mydatalakestore.azuredatalakestore.net/\<ruta_de_acceso_a_archivo\>.*

El esquema de URI para acceder a los archivos de Data Lake Storage Gen2 se explica detalladamente [aquí](https://docs.microsoft.com/azure/storage/data-lake-storage/use-hdi-cluster) pero, en términos generales, es *abfss://\<NOMBRE_SISTEMA_DE_ARCHIVOS\>\@\<NOMBRE_DE_CUENTA\>.dfs.core.windows.net/\<RUTA_DE_ACCESO\>.*

Deberá recorrer las aplicaciones existentes y asegurarse de haber cambiado los URI como corresponde para que apunten a los de Data Lake Storage Gen2. Además, deberá agregar las credenciales adecuadas. Por último, la manera en que se retiran las aplicaciones originales y se reemplazan por la aplicación nueva deberá estar estrechamente alineada con la estrategia de actualización general.

#### <a name="custom-applications"></a>Aplicaciones personalizadas

En función de la interfaz que la aplicación usa con Data Lake Storage Gen1, deberá modificarla y adaptarla a Data Lake Storage Gen2.

##### <a name="rest-apis"></a>API de REST

Si la aplicación usa API REST de Data Lake Storage, deberá modificar la aplicación para que use las API REST de Data Lake Storage Gen2. Puede encontrar vínculos en la sección *Interfaces de programación*.

##### <a name="sdks"></a>SDK

Tal como se indica en la sección *Evaluación de la preparación para la actualización*, actualmente los SDK no están disponibles. Si quiere migrar las aplicaciones a Data Lake Storage Gen2, le recomendamos que espere a que los SDK compatibles estén disponibles.

##### <a name="powershell"></a>PowerShell

Como se describe en la sección Evaluación de la preparación para la actualización, la compatibilidad con PowerShell no está disponible actualmente en el plano de datos.

Podría reemplazar los commandlets del plano de administración por los correspondientes en Data Lake Storage Gen2. Puede encontrar vínculos en la sección *Interfaces de programación*.

##### <a name="cli"></a>CLI

Como se describe en la sección *Evaluación de la preparación para la actualización*, la compatibilidad con la CLI no está disponible actualmente en el plano de datos.

Podría reemplazar los comandos del plano de administración por los correspondientes en Data Lake Storage Gen2. Puede encontrar vínculos en la sección *Interfaces de programación*.

### <a name="analytics-frameworks-upgrade"></a>Actualización de los marcos de análisis

Si la aplicación crea metadatos sobre la información que se encuentra en el almacén como rutas de acceso a archivos y carpetas explícitas, deberá realizar acciones adicionales después de la actualización de los datos y metadatos del almacén. Esto resulta especialmente cierto en marcos de análisis como Azure HDInsight, Databricks, etc., los que habitualmente crean datos de catálogo en los datos del almacén.

Los marcos de análisis funcionan con los datos y metadatos almacenados en almacenes remotos como Data Lake Storage Gen1 y Gen2. Por tanto, en teoría, los motores pueden ser temporales y activarse solo cuando sea necesario ejecutar trabajos en los datos almacenados.

Sin embargo, para optimizar el rendimiento, los marcos de análisis pueden crear referencias explícitas a los archivos y carpetas almacenados en el almacén remoto y, luego, crear una memoria caché que los contenga. Si el URI de los datos remotos cambia, como por ejemplo, un clúster que antes almacenaba datos en Data Lake Storage Gen1 y que ahora se quiere almacenar en Data Lake Storage Gen2, el URI del mismo contenido copiado será distinto. Por tanto, después de la actualización de los datos y metadatos, también es necesario actualizar o reinicializar las memorias caché de estos motores.

Como parte del proceso de planeación, deberá identificar la aplicación y averiguar cómo se puede reinicializar la información de metadatos para apuntar a los datos que ahora están almacenados en Data Lake Storage Gen2. A continuación, encontramos una guía de los marcos de análisis habitualmente adoptados que lo ayudará con los pasos de la actualización.

#### <a name="azure-databricks"></a>Azure Databricks

Los pasos serán distintos en función de la estrategia de actualización que elija. En la sección actual se da por supuesto que eligió la estrategia de "migración mediante lift-and-shift". Además, se espera que el área de trabajo de Databricks que solía acceder a los datos de una cuenta de Data Lake Storage Gen1 funcione con los datos que se copian a la cuenta de Data Lake Storage Gen2.

En primer lugar, asegúrese de crear la cuenta de Gen2 y, luego, copiar los datos y metadatos de Gen1 a Gen2 con una herramienta adecuada. Tales herramientas se describen en la sección [Actualización de datos](#data-upgrade) de esta guía.

Luego, [actualice](https://docs.azuredatabricks.net/user-guide/clusters/index.html) el clúster de Databricks existente para empezar a usar el entorno de ejecución de Databricks 5.1 o superior, que debe admitir Data Lake Storage Gen2.

A partir de entonces, los pasos se basan en cómo el área de trabajo de Databricks existente accede a los datos en la cuenta de Data Lake Storage Gen1. Para ello, se puede llamar a los URI adl:// [directamente](https://docs.azuredatabricks.net/spark/latest/data-sources/azure/azure-datalake.html#access-azure-data-lake-store-directly) desde los cuadernos o a través de [puntos de montaje](https://docs.azuredatabricks.net/spark/latest/data-sources/azure/azure-datalake.html#mount-azure-data-lake-store-with-dbfs).

Si accede directamente desde cuadernos al proporcionar los URI adl:// completos, deberá pasar por cada cuaderno y cambiar la configuración para acceder al URI de Data Lake Storage Gen2 correspondiente.

En el futuro, deberá volver a configurarla para apuntar a la cuenta de Data Lake Storage Gen2. No se necesitan otros cambios y los cuadernos deberían poder trabajar tal como antes.

Si usa cualquier otra estrategia de actualización, puede variar los pasos mencionados para cumplir con los requisitos.

### <a name="azure-ecosystem-upgrade"></a>Actualización del ecosistema de Azure

Será necesario configurar cada una de las herramientas y los servicios descritos en la sección [Ecosistema de Azure](#azure-ecosystem) de esta guía para que funcionen con Data Lake Storage Gen2.

En primer ligar, asegúrese de que la integración con Data Lake Storage Gen2 se encuentre disponible.

Luego, se deberán cambiar los elementos descritos anteriormente (por ejemplo, el URI y las credenciales). Se puede modificar la instancia existente que funciona con Data Lake Storage Gen1 o puede crear una instancia que debiera funcionar con Data Lake Storage Gen2.

### <a name="partner-ecosystem-upgrade"></a>Actualización del ecosistema de asociados

Trabaje en conjunto con el asociado y proporcione el componente y las herramientas para garantizar que pueden funcionar con Data Lake Storage Gen2. 

## <a name="performing-the-upgrade"></a>Realización de la actualización

### <a name="pre-upgrade"></a>Antes de la actualización

Como parte de esto, tendría que haber revisado las secciones *Evaluación de la preparación para la actualización* y [Planeación de una actualización](#planning-for-an-upgrade) de esta guía y haber recibido toda la información necesaria, además de haber creado un plan para satisfacer sus necesidades. Es probable que tenga una tarea de prueba durante esta fase.

### <a name="in-upgrade"></a>Durante la actualización

Según la estrategia que elija y las complejidades de la solución, esta fase podría ser breve o larga, con varias cargas de trabajo que esperan migrar de manera incremental a Data Lake Storage Gen2. Esta será la parte más crítica de la actualización.

### <a name="post-upgrade"></a>Después de la actualización

Una vez que termine con la operación de transición, los pasos finales implicarán una comprobación exhaustiva. Esto podría incluir, entre otros aspectos, comprobar que los datos se copiaron de manera confiable, que las ACL se establecieron correctamente y que las canalizaciones E2E funcionan como corresponde, etc. Una vez que se realizan las comprobaciones, puede apagar las canalizaciones antiguas, eliminar las cuentas de Data Lake Storage Gen1 de origen y acelerar la implementación de las soluciones basadas en Data Lake Storage Gen2.

## <a name="conclusion"></a>Conclusión

La guía que se brinda en este documento debería haberlo ayudado a actualizar la solución para usar Data Lake Storage Gen2. 

Si tiene más preguntas o tiene algún comentario, indíquelo a continuación o en el [foro de comentarios de Azure](https://feedback.azure.com/forums/327234-data-lake).
