---
title: Administración de clústeres de Azure HDInsight mediante la CLI de Azure
description: Obtenga información sobre cómo usar la CLI de Azure para administrar clústeres de Azure HDInsight. Los tipos de clúster incluyen Apache Hadoop, Spark, HBase, Storm, Kafka, Interactive Query y servicios de aprendizaje automático.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive,hdiseo17may2017
ms.date: 12/06/2019
ms.openlocfilehash: 81bc632f1061f0ee73d2295cafa5f7a8472d20ee
ms.sourcegitcommit: 5b9287976617f51d7ff9f8693c30f468b47c2141
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/09/2019
ms.locfileid: "74951809"
---
# <a name="manage-azure-hdinsight-clusters-using-azure-cli"></a>Administración de clústeres de Azure HDInsight mediante la CLI de Azure

[!INCLUDE [selector](../../includes/hdinsight-portal-management-selector.md)]

Más información sobre cómo utilizar la [CLI de Azure](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest) para administrar clústeres de Azure HDInsight. La interfaz de la línea de comandos (CLI) de Azure es la experiencia de línea de comandos multiplataforma de Microsoft para administrar los recursos de Azure.

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

* CLI de Azure. Si no tiene instalada la CLI de Azure, consulte [Instalación de la CLI de Azure](https://docs.microsoft.com/cli/azure/install-azure-cli) los pasos que debe seguir.

* Un clúster de Apache Hadoop en HDInsight. Consulte [Introducción a HDInsight en Linux](hadoop/apache-hadoop-linux-tutorial-get-started.md).

## <a name="connect-to-azure"></a>Conexión a Azure

Inicie sesión en la suscripción de Azure. Si va a usar Azure Cloud Shell, seleccione **Probar** en la esquina superior derecha del bloque de código. En caso contrario, escriba el siguiente comando:

```azurecli-interactive
az login

# If you have multiple subscriptions, set the one to use
# az account set --subscription "SUBSCRIPTIONID"
```

## <a name="list-clusters"></a>Enumeración de clústeres

Utilice [az hdinsight list](https://docs.microsoft.com/cli/azure/hdinsight?view=azure-cli-latest#az-hdinsight-list) para enumerar los clústeres. Para modificar los comandos siguientes, reemplace `RESOURCE_GROUP_NAME` por el nombre del grupo de recursos y, luego, escriba los comandos:

```azurecli-interactive
# List all clusters in the current subscription
az hdinsight list

# List only cluster name and its resource group
az hdinsight list --query "[].{Cluster:name, ResourceGroup:resourceGroup}" --output table

# List all cluster for your resource group
az hdinsight list --resource-group RESOURCE_GROUP_NAME

# List all cluster names for your resource group
az hdinsight list --resource-group RESOURCE_GROUP_NAME --query "[].{clusterName:name}" --output table
```

## <a name="show-cluster"></a>Presentación de clústeres

Utilice [az hdinsight show](https://docs.microsoft.com/cli/azure/hdinsight?view=azure-cli-latest#az-hdinsight-show) para mostrar la información de un clúster especificado. Para modificar el comando siguiente, reemplace `RESOURCE_GROUP_NAME` y `CLUSTER_NAME` por la información pertinente y, después, escriba el comando:

```azurecli-interactive
az hdinsight show --resource-group RESOURCE_GROUP_NAME --name CLUSTER_NAME
```

## <a name="delete-clusters"></a>Eliminación de clústeres

Utilice [az hdinsight delete](https://docs.microsoft.com/cli/azure/hdinsight?view=azure-cli-latest#az-hdinsight-delete) para eliminar un clúster especificado. Para modificar el comando siguiente, reemplace `RESOURCE_GROUP_NAME` y `CLUSTER_NAME` por la información pertinente y, después, escriba el comando:

```azurecli-interactive
az hdinsight delete --resource-group RESOURCE_GROUP_NAME --name CLUSTER_NAME
```

También puede eliminar un clúster eliminando el grupo de recursos que lo contiene. Tenga en cuenta que esto eliminará todos los recursos del grupo, incluida la cuenta de almacenamiento predeterminada.

```azurecli-interactive
az group delete --name RESOURCE_GROUP_NAME
```

## <a name="scale-clusters"></a>Escalado de clústeres

Utilice [az hdinsight resize](https://docs.microsoft.com/cli/azure/hdinsight?view=azure-cli-latest#az-hdinsight-resize) para cambiar el tamaño del clúster de HDInsight al tamaño especificado. Para modificar el comando siguiente, reemplace `RESOURCE_GROUP_NAME` y `CLUSTER_NAME` por la información pertinente: Reemplace `TARGET_INSTANCE_COUNT` por el número deseado de nodos de trabajo para el clúster. Para más información sobre la escalabilidad de los clústeres, consulte [Escalabilidad de clústeres de HDInsight](./hdinsight-scaling-best-practices.md). Escriba el comando:

```azurecli-interactive
az hdinsight resize --resource-group RESOURCE_GROUP_NAME --name CLUSTER_NAME --target-instance-count TARGET_INSTANCE_COUNT
```

## <a name="next-steps"></a>Pasos siguientes

En este artículo, ha aprendido a realizar diferentes tareas administrativas en clústeres de HDInsight. Para obtener más información, consulte los artículos siguientes:

* [Administración de clústeres de Apache Hadoop en HDInsight mediante Azure Portal](hdinsight-administer-use-portal-linux.md)
* [Administración de HDInsight con Azure PowerShell](hdinsight-administer-use-powershell.md)
* [Introducción a Azure HDInsight](hadoop/apache-hadoop-linux-tutorial-get-started.md)
* [Introducción a la CLI de Azure](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli?view=azure-cli-latest)
