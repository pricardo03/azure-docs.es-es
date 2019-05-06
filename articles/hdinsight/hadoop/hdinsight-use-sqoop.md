---
title: Ejecutar trabajos de Apache Sqoop con Azure HDInsight (Apache Hadoop)
description: Aprenda a utilizar Azure PowerShell desde una estación de trabajo para ejecutar la importación y la exportación en Sqoop entre un clúster de Hadoop y una base de datos de Azure SQL.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 04/12/2019
ms.openlocfilehash: 6764d8d812789c9f54fa59e10b2a3e416e583a9c
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "62129406"
---
# <a name="use-apache-sqoop-with-hadoop-in-hdinsight"></a>Uso de Apache Sqoop con Hadoop en HDInsight
[!INCLUDE [sqoop-selector](../../../includes/hdinsight-selector-use-sqoop.md)]

Aprenda a usar Apache Sqoop en HDInsight para importar y exportar datos entre un clúster de HDInsight y Azure SQL database.

Aunque Apache Hadoop es una opción natural para procesar datos no estructurados y semiestructurados, como registros y archivos, es posible que también sea necesario para procesar datos estructurados que se almacenan en bases de datos relacionales.

[Apache Sqoop](https://sqoop.apache.org/docs/1.99.7/user.html) es una herramienta diseñada para transferir datos entre clústeres de Hadoop y bases de datos relacionales. Puede usarla para importar datos desde un sistema de administración de bases de datos relacionales (RDBMS) como SQL Server, MySQL u Oracle en el sistema de archivos distribuidos Hadoop (HDFS), transformar los datos de Hadoop con MapReduce o Apache Hive y, a continuación, exportar los datos en un RDBMS. En este artículo, se usa una base de datos de SQL Server para la base de datos relacional.

> [!IMPORTANT]  
> En este artículo se configura un entorno de prueba para realizar la transferencia de datos. A continuación, elija un método de transferencia de datos para este entorno de uno de los métodos en la sección [trabajos de Sqoop ejecutar](#run-sqoop-jobs), más adelante.

¿Para las versiones de Sqoop que se admiten en clústeres de HDInsight, consulte [cuáles son las novedades en las versiones de clústeres proporcionadas por HDInsight?](../hdinsight-component-versioning.md)

## <a name="understand-the-scenario"></a>Descripción del escenario

El clúster de HDInsight incluye algunos datos de ejemplo. Utilice los dos ejemplos siguientes:

* Un archivo de registro Log4j de Apache, que se encuentra en `/example/data/sample.log`. Los registros siguientes se extraen del archivo:

```text
2012-02-03 18:35:34 SampleClass6 [INFO] everything normal for id 577725851
2012-02-03 18:35:34 SampleClass4 [FATAL] system problem at id 1991281254
2012-02-03 18:35:34 SampleClass3 [DEBUG] detail for id 1304807656
...
```

* Una tabla de Hive denominada `hivesampletable`, que hace referencia al archivo de datos ubicado en `/hive/warehouse/hivesampletable`. La tabla contiene algunos datos del dispositivo móvil.
  
  | Campo | Tipo de datos |
  | --- | --- |
  | clientid |string |
  | querytime |string |
  | market |string |
  | deviceplatform |string |
  | devicemake |string |
  | devicemodel |string |
  | state |string |
  | country |string |
  | querydwelltime |double |
  | sessionid |bigint |
  | sessionpagevieworder |bigint |

En este artículo, usará estos dos conjuntos de datos para probar Sqoop para importar y exportar.

## <a name="create-cluster-and-sql-database"></a>Configurar el entorno de prueba
El clúster, SQL database y otros objetos se crean a través del portal de Azure mediante una plantilla de Azure Resource Manager. La plantilla se puede encontrar en [plantillas de inicio rápido de Azure](https://azure.microsoft.com/resources/templates/101-hdinsight-linux-with-sql-database/). La plantilla de Resource Manager llama a un paquete de bacpac para implementar los esquemas de tabla en una base de datos SQL.  El paquete bacpac se encuentra en un contenedor de blobs público, https://hditutorialdata.blob.core.windows.net/usesqoop/SqoopTutorial-2016-2-23-11-2.bacpac. Si desea usar un contenedor privado para los archivos bacpac, utilice los siguientes valores en la plantilla:

```json
"storageKeyType": "Primary",
"storageKey": "<TheAzureStorageAccountKey>",
```

> [!NOTE]  
> La importación mediante Azure Portal solo admite la importación de un archivo BACPAC desde Azure Blob Storage.

1. Seleccione la imagen siguiente para abrir la plantilla de Resource Manager en Azure portal.

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-linux-with-sql-database%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-use-sqoop/deploy-to-azure.png" alt="Deploy to Azure"></a>

2. Especifique las siguientes propiedades:

    |Campo |Valor |
    |---|---|
    |Subscription |En la lista desplegable, seleccione la suscripción de Azure.|
    |Grupos de recursos |Seleccione el grupo de recursos en la lista desplegable o crear uno nuevo|
    |Location |Seleccione una región en la lista desplegable.|
    |Cluster Name |Escriba el nombre del clúster de Hadoop. Use solo una letra minúscula.|
    |Cluster Login User Name (Nombre de usuario de inicio de sesión del clúster) |Mantenga el valor rellenado previamente `admin`.|
    |Cluster Login Password (Contraseña de inicio de sesión del clúster) |Escriba una contraseña.|
    |Nombre de usuario SSH |Mantenga el valor rellenado previamente `sshuser`.|
    |SSH contraseña |Escriba una contraseña.|
    |Inicio de sesión de administrador de SQL |Mantenga el valor rellenado previamente `sqluser`.|
    |Contraseña de administrador de SQL |Escriba una contraseña.|
    |_artifacts ubicación | Use el valor predeterminado a menos que desee utilizar su propio archivo de bacpac en una ubicación diferente.|
    |Token de Sas de ubicación _artifacts |déjelo en blanco.|
    |Nombre de archivo Bacpac |Use el valor predeterminado a menos que desee utilizar su propio archivo de bacpac.|
    |Location |Use el valor predeterminado.|

    El nombre del servidor SQL de Azure será `<ClusterName>dbserver`. El nombre de la base de datos será `<ClusterName>db`. El nombre de la cuenta de almacenamiento predeterminada será `e6qhezrh2pdqu`.

3. Seleccione **Acepto los términos y condiciones indicados anteriormente**.

4. Seleccione **Comprar**. Verá un icono nuevo llamado Envío de implementación para la implementación de plantilla. Tarda aproximadamente 20 minutos en crear un clúster y la base de datos SQL.

## <a name="run-sqoop-jobs"></a>Ejecución de trabajos de Sqoop

HDInsight puede ejecutar trabajos de Sqoop mediante una variedad de métodos. Use la tabla siguiente para decidir cuál es el método adecuado para usted luego siga el vínculo para ver un tutorial.

| **Utilícelo** si desea... | ...un shell **interactivo** | ...procesamiento por**lotes** | ...desde este **sistema operativo de cliente** |
|:--- |:---:|:---:|:--- |:--- |
| [SSH](apache-hadoop-use-sqoop-mac-linux.md) |? |? |Linux, Unix, Mac OS X o Windows |
| [.NET SDK para Hadoop](apache-hadoop-use-sqoop-dotnet-sdk.md) |&nbsp; |?  |Windows (por ahora) |
| [Azure PowerShell](apache-hadoop-use-sqoop-powershell.md) |&nbsp; |? | Windows |

## <a name="limitations"></a>Limitaciones

* Exportación masiva: con HDInsight basado en Linux, el conector Sqoop que se utiliza para exportar datos a Microsoft SQL Server o Azure SQL Database no es compatible actualmente con las inserciones masivas.
* Procesamiento por lotes: con HDInsight basado en Linux, cuando se usa `-batch` al realizar inserciones, Sqoop realiza varias inserciones en lugar de procesar por lotes las operaciones de inserción.

## <a name="next-steps"></a>Pasos siguientes
Ahora ya ha aprendido a usar Sqoop. Para obtener más información, consulte:

* [Uso de Apache Hive con HDInsight](../hdinsight-use-hive.md)
* [Uso de Apache Pig con HDInsight](../hdinsight-use-pig.md)
* [Carga de datos en HDInsight](../hdinsight-upload-data.md): busque otros métodos para cargar datos en HDInsight o Azure Blob Storage.
