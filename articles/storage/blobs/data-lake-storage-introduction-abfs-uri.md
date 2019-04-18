---
title: Uso del URI de Azure Data Lake Storage Gen2
description: Uso del URI de Azure Data Lake Storage Gen2
services: storage
author: jamesbak
ms.topic: conceptual
ms.author: jamesbak
ms.date: 12/06/2018
ms.service: storage
ms.subservice: data-lake-storage-gen2
ms.openlocfilehash: e4f148ac6f66cfbd7d89eff3623418197c2e0a33
ms.sourcegitcommit: c3d1aa5a1d922c172654b50a6a5c8b2a6c71aa91
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59681189"
---
# <a name="use-the-azure-data-lake-storage-gen2-uri"></a>Uso del URI de Azure Data Lake Storage Gen2

El controlador [Hadoop Filesystem](https://www.aosabook.org/en/hdfs.html), que es compatible con Azure Data Lake Storage Gen2, se conoce por su identificador de esquema `abfs` (sistema de archivos de blob de Azure). El controlador ABFS es coherente con otros controladores del sistema de archivos de Hadoop y emplea un formato URI para dirigir archivos y directorios dentro de una cuenta compatible con Data Lake Storage Gen2.

## <a name="uri-syntax"></a>Sintaxis de URI

La sintaxis del URI de Data Lake Storage Gen2 depende de si la cuenta de almacenamiento está configurada para tener Data Lake Storage Gen2 como sistema de archivos predeterminado.

Si la cuenta compatible con Data Lake Storage Gen2 que quiere dirigir **no está** configurada como el sistema de archivos predeterminado durante la creación de la cuenta, la sintaxis abreviada del URI es la siguiente:

<pre>abfs[s]<sup>1</sup>://&lt;file_system&gt;<sup>2</sup>@&lt;account_name&gt;<sup>3</sup>.dfs.core.windows.net/&lt;path&gt;<sup>4</sup>/&lt;file_name&gt;<sup>5</sup></pre>

1. **Identificador de esquema**: el protocolo `abfs` se usa como el identificador de esquema. Tiene la opción de conectarse con o sin una conexión Capa de sockets seguros (SSL). Use `abfss` para conectarse con una conexión Capa de sockets seguros.

2. **Sistema de archivos**: ubicación principal que contiene los archivos y las carpetas. Es la misma que la de los contenedores del servicio de Azure Storage Blob.

3. **Nombre de cuenta**: nombre dado a la cuenta de almacenamiento durante la creación.

4. **Rutas de acceso**: representación de barra diagonal delimitada (`/`) de la estructura del directorio.

5. **Nombre de archivo**: nombre del archivo individual. Este parámetro es opcional si va a dirigirse a un directorio.

Sin embargo, si la cuenta que quiere dirigir está configurada como el sistema de archivos predeterminado durante la creación de la cuenta, la sintaxis abreviada del URI es la siguiente:

<pre>/&lt;path&gt;<sup>1</sup>/&lt;file_name&gt;<sup>2</sup></pre>

1. **Ruta de acceso**: representación de barra diagonal delimitada (`/`) de la estructura del directorio.

2. **Nombre de archivo**: nombre del archivo individual.


## <a name="next-steps"></a>Pasos siguientes

- [Uso de Data Lake Storage Gen2 con clústeres de Azure HDInsight](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-use-data-lake-storage-gen2?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)
