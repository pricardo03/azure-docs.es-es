---
title: El controlador Azure Blob Filesystem para Azure Data Lake Storage Gen2 (versión preliminar)
description: El controlador ABFS Hadoop FileSystem
services: storage
keywords: ''
author: jamesbak
manager: jahogg
ms.topic: article
ms.author: jamesbak
ms.date: 06/27/2018
ms.service: storage
ms.component: data-lake-storage-gen2
ms.openlocfilehash: a726779e731be2534e457ba595d93fe51c023601
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/27/2018
ms.locfileid: "37035225"
---
# <a name="the-azure-blob-filesystem-driver-abfs-a-dedicated-azure-storage-driver-for-hadoop"></a>El controlador Azure Blob Filesystem (ABFS): un controlador de Azure Storage dedicado para Hadoop

Uno de los principales métodos de acceso para los datos en Azure Data Lake Storage Gen2 (versión preliminar) es mediante [Hadoop FileSystem](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-common/filesystem/index.html). Azure Data Lake Storage Gen2 presenta un controlador asociado, el controlador Azure Blob File System o `ABFS`. ABFS forma parte de Apache Hadoop y se incluye en muchas de las distribuciones comerciales de Hadoop. Con este controlador, muchas aplicaciones y plataformas pueden acceder a datos de Data Lake Storage Gen2 sin código que haga referencia explícita al servicio Data Lake Storage Gen2.

## <a name="prior-capability-the-windows-azure-storage-blob-driver"></a>Funcionalidad anterior: el controlador Azure Storage Blob para Windows

El controlador Azure Storage Blob para Windows o [controlador WASB](https://hadoop.apache.org/docs/current/hadoop-azure/index.html) ofrecía la compatibilidad original con Azure Storage Blob. Este controlador realizaba la compleja tarea de asignar semántica de sistema de archivos (como requería la interfaz de Hadoop FileSystem) a la de la interfaz de estilo de almacén de objetos expuesta por Azure Blob Storage. Aunque este controlador aún admite este modelo, y proporciona acceso de alto rendimiento a los datos almacenados en blobs, contiene una importante cantidad de código que realiza esta asignación, lo que hace que sea difícil de mantener. Además, algunas operaciones como [FileSystem.rename()](http://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-common/filesystem/filesystem.html#boolean_renamePath_src_Path_d) y [FileSystem.delete()](http://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-common/filesystem/filesystem.html#boolean_deletePath_p_boolean_recursive), cuando se aplican a directorios, necesitan el controlador para realizar un gran número de operaciones (debido a la falta de compatibilidad de los almacenes de objetos con los directorios), lo que con frecuencia conduce a una degradación en el rendimiento.

Por tanto, para superar las deficiencias inherentes al diseño de WASB, el nuevo servicio Azure Data Lake Storage se implementó con compatibilidad con el nuevo controlador ABFS.

## <a name="the-azure-blob-file-system-driver"></a>El controlador Azure Blob File System

La [interfaz de REST de Azure Data Lake Storage](https://docs.microsoft.com/en-us/rest/api/datalakestorage/) está diseñada para admitir la semántica de sistema de archivos a través de Azure Blob Storage. Dado que Hadoop FileSystem también está diseñado para admitir la misma semántica, no hay ningún requisito de una asignación compleja en el controlador. Por tanto, el controlador Azure Blob File System (o ABFS) es una mera corrección de compatibilidad (shim) de cliente para la API REST.

Sin embargo, hay algunas funciones que el controlador debe seguir realizando:

### <a name="uri-scheme-to-reference-data"></a>Esquema de URI para los datos de referencia

En consonancia con otras implementaciones de FileSystem en Hadoop, el controlador ABFS define su propio esquema de URI de modo que los recursos (directorios y archivos) se puedan direccionar claramente. El esquema de URI se documenta en [Use the Azure Data Lake Storage Gen2 URI](./introduction-abfs-uri.md) (Uso del URI de Azure Data Lake Storage Gen2). La estructura del URI es: `abfs[s]://file_system@account_name.dfs.core.windows.net/<path>/<path>/<file_name>`

Mediante el formato de URI anterior, se pueden usar herramientas y plataformas estándar de Hadoop para hacer referencia a estos recursos:

```bash
hdfs dfs -mkdir -p abfs://fileanalysis@myanalytics.dfs.core.windows.net/tutorials/flightdelays/data 
hdfs dfs -put flight_delays.csv abfs://fileanalysis@myanalytics.dfs.core.windows.net/tutorials/flightdelays/data/ 
```

Internamente, el controlador ABFS convierte los recursos especificados en el URI en archivos y directorios y realiza llamadas a la API REST de Azure Data Lake Storage con esas referencias.

### <a name="authentication"></a>Autenticación

El controlador ABFS admite actualmente la autenticación de clave compartida de modo que la aplicación de Hadoop puede acceder de forma segura a los recursos contenidos en Data Lake Storage Gen2. La clave se cifra y almacena en la configuración de Hadoop.

### <a name="configuration"></a>Configuración

Toda la configuración del controlador ABFS se almacena en el archivo de configuración <code>core-site.xml</code>. En las distribuciones de Hadoop con [Ambari](http://ambari.apache.org/), la configuración también puede administrarse mediante el portal web o la API REST de Ambari.

Se puede encontrar información detallada de todas las entradas de configuración admitidas en la [documentación oficial de Hadoop](http://hadoop.apache.org/docs/current/hadoop-azure/index.html).

### <a name="hadoop-documentation"></a>Documentación de Hadoop

El controlador ABFS está completamente documentado en la [documentación oficial de Hadoop](http://hadoop.apache.org/docs/current/hadoop-azure/index.html).

## <a name="next-steps"></a>Pasos siguientes

- [Configuración de clústeres de HDInsight](./quickstart-create-connect-hdi-cluster.md)
- [Creación de un clúster de Azure Databricks](./quickstart-create-databricks-account.md)
- [Use the Azure Data Lake Storage Gen2 URI](./introduction-abfs-uri.md) (Uso del URI de Azure Data Lake Storage Gen2).