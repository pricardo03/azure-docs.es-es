---
title: Administración de clústeres de Apache Hadoop en HDInsight con PowerShell (Azure)
description: Vea cómo realizar tareas administrativas para clústeres de Apache Hadoop en HDInsight mediante Azure PowerShell.
services: hdinsight
ms.reviewer: tyfox
author: hrasheed-msft
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 04/17/2019
ms.author: tylerfox
ms.openlocfilehash: 6cf05437d5fc181a9fadae110a44efd88d06a2da
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/22/2019
ms.locfileid: "60011637"
---
# <a name="manage-apache-hadoop-clusters-in-hdinsight-by-using-azure-powershell"></a>Administración de clústeres de Apache Hadoop en HDInsight con Azure PowerShell
[!INCLUDE [selector](../../includes/hdinsight-portal-management-selector.md)]

Se puede usar Azure PowerShell para controlar y automatizar la implementación y la administración de las cargas de trabajo en Azure. En este artículo, obtendrá información sobre cómo administrar [Apache Hadoop](https://hadoop.apache.org/) clústeres en HDInsight de Azure mediante el módulo Azure PowerShell Az. Para obtener la lista de los cmdlets de HDInsight PowerShell, consulte el [Az.HDInsight referencia](https://docs.microsoft.com/powershell/module/az.hdinsight).

## <a name="prerequisites"></a>Requisitos previos

* Una suscripción de Azure. Consulte [Obtención de una versión de evaluación gratuita](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

* PowerShell [módulo Az](https://docs.microsoft.com/powershell/azure/overview) instalado.

## <a name="create-clusters"></a>Creación de clústeres
Consulte [Crear clústeres basados en Linux en HDInsight con Azure PowerShell](hdinsight-hadoop-create-linux-clusters-azure-powershell.md)

## <a name="list-clusters"></a>Enumeración de clústeres
Use el comando siguiente para enumerar todos los clústeres de la suscripción actual:

```powershell
Get-AzHDInsightCluster
```

## <a name="show-cluster"></a>Presentación de clústeres
Use el comando siguiente para mostrar los detalles de un clúster específico de la suscripción actual:

```powershell
Get-AzHDInsightCluster -ClusterName <Cluster Name>
```

## <a name="delete-clusters"></a>Eliminación de clústeres
Use el comando siguiente para eliminar un clúster:

```powershell
Remove-AzHDInsightCluster -ClusterName <Cluster Name>
```

También puede eliminar un clúster quitando el grupo de recursos que contiene el clúster. La eliminación de un grupo de recursos supone la eliminación de todos los recursos del grupo, incluida la cuenta de almacenamiento predeterminada.

```powershell
Remove-AzResourceGroup -Name <Resource Group Name>
```

## <a name="scale-clusters"></a>Escalado de clústeres
La característica de escalado de clústeres permite cambiar la cantidad de nodos de trabajo que usa un clúster que se ejecuta en HDInsight de Azure sin necesidad de volver a crear el clúster.

A continuación se muestra el efecto que tiene cambiar la cantidad de nodos de datos de cada tipo de clúster compatible con HDInsight:

* Apache Hadoop

    Puede aumentar sin ningún problema la cantidad de nodos de trabajo en un clúster de Hadoop que se encuentre en ejecución, sin que afecte a ningún trabajo pendiente o en ejecución. También se pueden enviar trabajos nuevos mientras la operación está en curso. Los errores que puedan surgir en una operación de escalado se enfrentan oportunamente, por lo que el clúster siempre queda en estado funcional.

    Cuando se realiza la reducción vertical de un clúster de Hadoop al disminuir la cantidad de nodos de datos, se reinician algunos de los servicios del clúster. El reinicio de los servicios provoca que todos los trabajos pendientes y en ejecución experimenten un error al completarse la operación de escalado. Sin embargo, puede volver a enviar los trabajos una vez finalizada la operación.
* HBase Apache

    Puede agregar nodos sin problemas al clúster de HBase mientras se encuentra en ejecución, así como eliminarlos. Los servidores regionales se equilibran automáticamente en unos pocos minutos tras completar la operación de escalado. Pero también puede equilibrar manualmente los servidores regionales si inicia sesión en el nodo principal del clúster y luego ejecuta los comandos siguientes desde una ventana del símbolo del sistema:

    ```bash
    pushd %HBASE_HOME%\bin
    hbase shell
    balancer
    ```

* Apache Storm

    Puede agregar o quitar sin problemas nodos de datos de su clúster de Storm mientras se encuentra en ejecución. Sin embargo, después de finalizar correctamente la operación de escalado, deberá volver a equilibrar la topología.

    Esto se puede realizar de dos formas:

  * La interfaz de usuario web de Storm
  * La herramienta de la interfaz de línea de comandos (CLI)

    Consulte la [documentación de Apache Storm](https://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html) para obtener más detalles.

    La interfaz de usuario web de Storm se encuentra disponible en el clúster de HDInsight:

    ![reequilibrio de escalado de storm de HDInsight](./media/hdinsight-administer-use-powershell/hdinsight.portal.scale.cluster.png)

    El siguiente es un ejemplo de cómo usar el comando CLI para volver a equilibrar la topología de Storm:

    ```cli
    ## Reconfigure the topology "mytopology" to use 5 worker processes,
    ## the spout "blue-spout" to use 3 executors, and
    ## the bolt "yellow-bolt" to use 10 executors
    $ storm rebalance mytopology -n 5 -e blue-spout=3 -e yellow-bolt=10
    ```

Para cambiar el tamaño del clúster de Hadoop con Azure PowerShell, ejecute el siguiente comando desde un equipo cliente:

```powershell
Set-AzHDInsightClusterSize -ClusterName <Cluster Name> -TargetInstanceCount <NewSize>
```


## <a name="grantrevoke-access"></a>Concesión o revocación del acceso
Los clústeres de HDInsight tienen los siguientes servicios web HTTP (todos estos servicios tienen extremos RESTful):

* ODBC
* JDBC
* Ambari
* Oozie
* Templeton

De manera predeterminada, estos servicios se conceden para el acceso. Puede revocar/conceder el acceso. Para revocar:

```powershell
Revoke-AzHDInsightHttpServicesAccess -ClusterName <Cluster Name>
```

Para conceder:

```powershell
$clusterName = "<HDInsight Cluster Name>"

# Credential option 1
$hadoopUserName = "admin"
$hadoopUserPassword = "<Enter the Password>"
$hadoopUserPW = ConvertTo-SecureString -String $hadoopUserPassword -AsPlainText -Force
$credential = New-Object System.Management.Automation.PSCredential($hadoopUserName,$hadoopUserPW)

# Credential option 2
#$credential = Get-Credential -Message "Enter the HTTP username and password:" -UserName "admin"

Grant-AzHDInsightHttpServicesAccess -ClusterName $clusterName -HttpCredential $credential
```

> [!NOTE]  
> Al conceder o revocar el acceso, se restablece el nombre de usuario y la contraseña del clúster.

También es posible conceder y revocar el acceso a través del portal. Consulte [administrar Apache Hadoop clústeres en HDInsight mediante Azure portal](hdinsight-administer-use-portal-linux.md).

## <a name="update-http-user-credentials"></a>Actualización de las credenciales de usuario HTTP
Es el mismo procedimiento que la concesión o la revocación del acceso HTTP. Si se ha concedido al clúster el acceso HTTP, primero debe revocarlo.  Y después conceda el acceso con nuevas credenciales de usuario HTTP.

## <a name="find-the-default-storage-account"></a>Búsqueda de la cuenta de almacenamiento predeterminada
El siguiente script de PowerShell muestra cómo obtener el nombre de la cuenta de almacenamiento predeterminada y la información relacionada:

> [!IMPORTANT]  
> Los valores de `DefaultStorageAccount`, y `DefaultStorageContainer` no se devuelven desde [Get AzHDInsightCluster](https://docs.microsoft.com/powershell/module/az.hdinsight/get-azhdinsightcluster) cuando [transferencia segura](../storage/common/storage-require-secure-transfer.md) está habilitado en la cuenta de almacenamiento.


```powershell
#Connect-AzAccount
$clusterName = "<HDInsight Cluster Name>"

$clusterInfo = Get-AzHDInsightCluster -ClusterName $clusterName
$storageInfo = $clusterInfo.DefaultStorageAccount.split('.')
$defaultStoreageType = $storageInfo[1]
$defaultStorageName = $storageInfo[0]

echo "Default Storage account name: $defaultStorageName"
echo "Default Storage account type: $defaultStoreageType"

if ($defaultStoreageType -eq "blob")
{
    $defaultBlobContainerName = $cluster.DefaultStorageContainer
    $defaultStorageAccountKey = (Get-AzStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName)[0].Value
    $defaultStorageAccountContext = New-AzStorageContext -StorageAccountName $defaultStorageAccountName -StorageAccountKey $defaultStorageAccountKey

    echo "Default Blob container name: $defaultBlobContainerName"
    echo "Default Storage account key: $defaultStorageAccountKey"
}
```


## <a name="find-the-resource-group"></a>Búsqueda del grupo de recursos
En el modo de Resource Manager, cada clúster de HDInsight pertenece a un grupo de recursos de Azure.  Para buscar el grupo de recursos:

```powershell
$clusterName = "<HDInsight Cluster Name>"

$cluster = Get-AzHDInsightCluster -ClusterName $clusterName
$resourceGroupName = $cluster.ResourceGroup
```


## <a name="submit-jobs"></a>Envío de trabajos
**Para enviar trabajos de MapReduce**

Vea [Ejecución de los ejemplos de MapReduce incluidos en HDInsight](hadoop/apache-hadoop-run-samples-linux.md).

**Para enviar trabajos de Apache Hive**

Consulte [Ejecución de consultas de Apache Hive con PowerShell](hadoop/apache-hadoop-use-hive-powershell.md).

**Para enviar trabajos de Apache Pig**

Consulte [Ejecución de trabajos de Apache Pig con PowerShell](hadoop/apache-hadoop-use-pig-powershell.md).

**Para enviar trabajos de Apache Sqoop**

Consulte [Uso de Apache Sqoop con HDInsight](hadoop/hdinsight-use-sqoop.md).

**Para enviar trabajos de Apache Oozie**

Consulte [Uso de Apache Oozie con Apache Hadoop para definir y ejecutar un flujo de trabajo en HDInsight](hdinsight-use-oozie-linux-mac.md).

## <a name="upload-data-to-azure-blob-storage"></a>Carga de archivos de datos al almacenamiento de blobs de Azure

Consulte [cargar datos en HDInsight](hdinsight-upload-data.md).

## <a name="see-also"></a>Vea también

* [Documentación de referencia de los cmdlets de HDInsight](https://msdn.microsoft.com/library/azure/dn479228.aspx)
* [Administración de clústeres de Apache Hadoop en HDInsight mediante Azure Portal](hdinsight-administer-use-portal-linux.md)
* [Administrar HDInsight mediante una interfaz de línea de comandos](hdinsight-administer-use-command-line.md)
* [Creación de clústeres de HDInsight](hdinsight-hadoop-provision-linux-clusters.md)
* [Envío de trabajos de Apache Hadoop mediante programación](hadoop/submit-apache-hadoop-jobs-programmatically.md)
* [Introducción a Azure HDInsight](hadoop/apache-hadoop-linux-tutorial-get-started.md)
