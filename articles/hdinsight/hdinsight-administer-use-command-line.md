---
title: Administración de clústeres de HDInsight de Azure mediante la CLI de Azure
description: Obtenga información sobre cómo usar la CLI de Azure para administrar clústeres de HDInsight de Azure. Tipos de clúster incluyen Apache Hadoop, Spark, HBase, Storm, Kafka, Interactive Query y servicios de aprendizaje automático.
ms.reviewer: jasonh
author: tylerfox
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: conceptual
ms.date: 05/13/2019
ms.author: tyfox
ms.openlocfilehash: 7c12831c43762ddc776e8d5701f002be97992cbc
ms.sourcegitcommit: 4c2b9bc9cc704652cc77f33a870c4ec2d0579451
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/17/2019
ms.locfileid: "65859959"
---
# <a name="manage-azure-hdinsight-clusters-using-azure-cli"></a>Administración de clústeres de HDInsight de Azure mediante la CLI de Azure

[!INCLUDE [selector](../../includes/hdinsight-portal-management-selector.md)]

Aprenda a usar [CLI de Azure](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest) para administrar clústeres de HDInsight de Azure. La interfaz de la línea de comandos (CLI) de Azure es la experiencia de línea de comandos multiplataforma de Microsoft para administrar los recursos de Azure.

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

* CLI de Azure. Si no ha instalado la CLI de Azure, consulte [instalar la CLI de Azure](https://docs.microsoft.com/cli/azure/install-azure-cli) para conocer los pasos.

* Un clúster de Apache Hadoop en HDInsight. Consulte [empezar a trabajar con HDInsight en Linux](hadoop/apache-hadoop-linux-tutorial-get-started.md).

## <a name="connect-to-azure"></a>Conéctese a Azure

Inicie sesión en la suscripción de Azure. Si va a usar Azure Cloud Shell, simplemente seleccione **Pruébelo** en la esquina superior derecha del bloque de código. En caso contrario, escriba el siguiente comando:

```azurecli-interactive
az login

# If you have multiple subscriptions, set the one to use
# az account set --subscription "SUBSCRIPTIONID"
```

## <a name="list-clusters"></a>Enumeración de clústeres

Use [lista de az hdinsight](https://docs.microsoft.com/cli/azure/hdinsight?view=azure-cli-latest#az-hdinsight-list) a la lista de clústeres. Editar los comandos siguientes, reemplazando `RESOURCE_GROUP_NAME` con el nombre del grupo de recursos, a continuación, escriba los comandos:

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

Use [show de az hdinsight](https://docs.microsoft.com/cli/azure/hdinsight?view=azure-cli-latest#az-hdinsight-show) para mostrar la información de un clúster especificado. Modifique el comando siguiente reemplazando `RESOURCE_GROUP_NAME`, y `CLUSTER_NAME` con la información pertinente, a continuación, escriba el comando:

```azurecli-interactive
az hdinsight show --resource-group RESOURCE_GROUP_NAME --name CLUSTER_NAME
```

## <a name="delete-clusters"></a>Eliminación de clústeres

Use [az hdinsight elimina](https://docs.microsoft.com/cli/azure/hdinsight?view=azure-cli-latest#az-hdinsight-delete) para eliminar un clúster especificado. Modifique el comando siguiente reemplazando `RESOURCE_GROUP_NAME`, y `CLUSTER_NAME` con la información pertinente, a continuación, escriba el comando:

```azurecli-interactive
az hdinsight delete --resource-group RESOURCE_GROUP_NAME --name CLUSTER_NAME
```

También puede eliminar un clúster eliminando el grupo de recursos que lo contiene. Tenga en cuenta que esta acción eliminará todos los recursos del grupo, incluida la cuenta de almacenamiento predeterminada.

```azurecli-interactive
az group delete --name RESOURCE_GROUP_NAME
```

## <a name="scale-clusters"></a>Escalado de clústeres

Use [cambiar el tamaño de az hdinsight](https://docs.microsoft.com/cli/azure/hdinsight?view=azure-cli-latest#az-hdinsight-resize) para cambiar el tamaño del clúster de HDInsight especificado al tamaño especificado. Modifique el comando siguiente reemplazando `RESOURCE_GROUP_NAME`, y `CLUSTER_NAME` con la información pertinente. Reemplace `TARGET_INSTANCE_COUNT` con el número deseado de nodos de trabajo para el clúster. Para obtener más información sobre el escalado de clústeres, consulte [de clústeres de HDInsight de escala](./hdinsight-scaling-best-practices.md). Escriba el comando:

```azurecli-interactive
az hdinsight delete --resource-group RESOURCE_GROUP_NAME --name CLUSTER_NAME --target-instance-count TARGET_INSTANCE_COUNT
```

## <a name="next-steps"></a>Pasos siguientes

En este artículo, ha aprendido a realizar diferentes tareas administrativas en clústeres de HDInsight. Para obtener más información, consulte los artículos siguientes:

* [Administración de clústeres de Apache Hadoop en HDInsight mediante Azure Portal](hdinsight-administer-use-portal-linux.md)
* [Administración de HDInsight mediante Azure PowerShell](hdinsight-administer-use-powershell.md)
* [Introducción a Azure HDInsight](hadoop/apache-hadoop-linux-tutorial-get-started.md)
* [Introducción a la CLI de Azure](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli?view=azure-cli-latest)
