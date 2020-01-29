---
title: 'Apache Hadoop y el almacenamiento de transferencia segura: Azure HDInsight'
description: Aprenda a crear clústeres de HDInsight con cuentas de almacenamiento de Azure habilitadas para transferencia segura.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 01/22/2020
ms.openlocfilehash: a8176cc07296b7de7b6aba5356485280ef5ebde1
ms.sourcegitcommit: 87781a4207c25c4831421c7309c03fce5fb5793f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/23/2020
ms.locfileid: "76548822"
---
# <a name="create-apache-hadoop-cluster-with-secure-transfer-storage-accounts-in-azure-hdinsight"></a>Creación de un clúster de Apache Hadoop con cuentas de almacenamiento de transferencia segura en Azure HDInsight

La característica [Se requiere transferencia segura](../storage/common/storage-require-secure-transfer.md) mejora la seguridad de su cuenta de Azure Storage al aplicar todas las solicitudes a su cuenta mediante una conexión segura. Esta característica y el esquema wasbs solo son compatibles con la versión de clúster de HDInsight 3.6 o posterior.

**Habilitar la transferencia segura del almacenamiento después de crear un clúster puede producir errores al usar la cuenta de almacenamiento y no se recomienda. Es mejor crear un clúster con la propiedad habilitada.**

## <a name="prerequisites"></a>Prerequisites

Antes de empezar este artículo, debe tener lo siguiente:

* Suscripción de Azure: para crear una cuenta de evaluación gratuita durante un mes, vaya a [azure.microsoft.com/es-es/free](https://azure.microsoft.com/free).
* Una cuenta de Azure Storage habilitada para transferencia segura. Para obtener instrucciones, consulte [Crear una cuenta de almacenamiento](../storage/common/storage-account-create.md) y [Requerir transferencia segura](../storage/common/storage-require-secure-transfer.md). 
* Un contenedor de blobs en la cuenta de almacenamiento.

## <a name="create-cluster"></a>Crear clúster

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

En esta sección, se crea un clúster de Hadoop en HDInsight mediante una [plantilla de Azure Resource Manager](../azure-resource-manager/templates/deploy-powershell.md). La plantilla se encuentra en [GitHub](https://azure.microsoft.com/resources/templates/101-hdinsight-linux-with-existing-default-storage-account/). No es necesario tener experiencia en el uso de la plantilla de Resource Manager para seguir este artículo. Para conocer otros métodos de creación de clústeres y las propiedades que se usan en este artículo, consulte el artículo de [Creación de clústeres en HDInsight](hdinsight-hadoop-provision-linux-clusters.md).

1. Haga clic en la imagen siguiente para iniciar sesión en Azure y abrir la plantilla de Resource Manager en Azure Portal.

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-linux-with-existing-default-storage-account%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-hadoop-create-linux-clusters-with-secure-transfer-storage/hdi-deploy-to-azure1.png" alt="Deploy to Azure button for new cluster"></a>

2. Siga las instrucciones para crear el clúster con las especificaciones siguientes:

    * Especifique la versión 3.6 de HDInsight. Se requiere la versión 3.6 o posterior.
    * Especifique una cuenta de almacenamiento habilitada para transferencia.
    * Escriba el nombre corto para la cuenta de almacenamiento.
    * La cuenta de almacenamiento y el contenedor de blobs deben crearse con antelación.

      Para obtener las instrucciones, consulte [Crear un clúster](hadoop/apache-hadoop-linux-tutorial-get-started.md#create-cluster).

Si se usa la acción de scrpt para proporcionar sus propios archivos de configuración, debe usar wasbs en la siguiente configuración:

* fs.defaultFS (sitio principal)
* spark.eventLog.dir
* spark.history.fs.logDirectory

## <a name="add-additional-storage-accounts"></a>Adición de cuentas de almacenamiento adicionales

Existen varias opciones para agregar cuentas de almacenamiento adicionales habilitadas para transferencia segura:

* Modificar la plantilla de Azure Resource Manager de la última sección.
* Crear un clúster mediante [Azure Portal](https://portal.azure.com) y especificar la cuenta de almacenamiento vinculada.
* Use la acción de script para agregar cuentas de almacenamiento adicionales habilitadas para transferencia segura a un clúster de HDInsight existente. Para más información, consulte [Adición de más cuentas de almacenamiento a HDInsight](hdinsight-hadoop-add-storage.md).

## <a name="next-steps"></a>Pasos siguientes

En este artículo aprendió a crear un clúster de HDInsight y a habilitar la transferencia segura para las cuentas de almacenamiento.

Para más información sobre el análisis de datos con HDInsight, consulte los siguientes artículos:

* Para más información sobre el uso de [Apache Hive](https://hive.apache.org/) con HDInsight, como la ejecución de consultas de Hive desde Visual Studio, consulte este artículo sobre el [uso de Apache Hive con HDInsight](hadoop/hdinsight-use-hive.md).
* Para más información sobre [MapReduce de Apache Hadoop](https://hadoop.apache.org/docs/current/hadoop-mapreduce-client/hadoop-mapreduce-client-core/MapReduceTutorial.html), un mecanismo para escribir programas que procesan datos en Hadoop, consulte este artículo sobre el [uso de Apache Hadoop MapReduce con HDInsight](hadoop/hdinsight-use-mapreduce.md).
* Para más información sobre el uso de las herramientas de HDInsight para Visual Studio para analizar datos en HDInsight, consulte [Introducción al uso de herramientas de Apache Hadoop en Visual Studio para HDInsight para ejecutar una consulta de Hive](hadoop/apache-hadoop-visual-studio-tools-get-started.md).

Para aprender más sobre cómo HDInsight almacena datos o cómo insertar datos en HDInsight, consulte los artículos siguientes:

* Para más información sobre cómo HDInsight usa Azure Storage, consulte [Uso de Azure Storage con HDInsight](hdinsight-hadoop-use-blob-storage.md).
* Para más información sobre cómo cargar datos en HDInsight, consulte [Carga de datos en HDInsight](hdinsight-upload-data.md).

Para aprender más sobre la creación o administración de un clúster de HDInsight, consulte los siguientes artículos:

* Para más información sobre cómo administrar el clúster de HDInsight basado en Linux, consulte [Administración de clústeres de HDInsight con la interfaz de usuario web de Apache Ambari](hdinsight-hadoop-manage-ambari.md).
* Para más información acerca de las opciones que se pueden seleccionar al crear un clúster de HDInsight, consulte [Creación de clústeres de Hadoop basados en Linux en HDInsight](hdinsight-hadoop-provision-linux-clusters.md).
* Si está familiarizado con Linux y Apache Hadoop, pero quiere conocer información específica sobre Hadoop en HDInsight, consulte [Información sobre el uso de HDInsight en Linux](hdinsight-hadoop-linux-information.md). Este artículo proporciona información como:

  * Direcciones URL para los servicios hospedados en el clúster, por ejemplo, [Apache Ambari](https://ambari.apache.org/) y [WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat)
  * La ubicación de los archivos y ejemplos de [Apache Hadoop](https://hadoop.apache.org/) en el sistema de archivos local
  * El uso de Azure Storage (WASB) en lugar de [Apache Hadoop HDFS](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsUserGuide.html) como el almacén de datos predeterminado
