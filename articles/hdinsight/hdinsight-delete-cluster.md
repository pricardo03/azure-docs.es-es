---
title: Eliminación de un clúster de Azure HDInsight
description: Información sobre las distintas formas de eliminar un clúster de HDInsight.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 04/17/2019
ms.author: hrasheed
ms.custom: H1Hack27Feb2017,hdinsightactive
ms.openlocfilehash: eca7b4f8bd7e91bc8dcb9bcc49ed3b981010aaee
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "62097206"
---
# <a name="delete-an-hdinsight-cluster-using-your-browser-powershell-or-the-azure-cli"></a>Eliminación de un clúster de HDInsight con el explorador, PowerShell o la CLI de Azure

La facturación del clúster de HDInsight se inicia una vez creado el clúster y solo se detiene cuando se elimina. Se facturan por minuto realizando una prorrata, por lo que siempre debe eliminar aquellos que ya no se estén utilizando. En este documento, obtendrá información sobre cómo eliminar un clúster mediante el [portal Azure](https://portal.azure.com), [módulo Azure PowerShell Az](https://docs.microsoft.com/powershell/azure/overview)y el [CLI de Azure](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest).

> [!IMPORTANT]  
> Al eliminar un clúster de HDInsight, no se eliminan las cuentas de Azure Storage o Data Lake Storage asociadas a este. Puede volver a usar los datos almacenados en esos servicios en el futuro.

## <a name="azure-portal"></a>Azure Portal

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).

2. En el menú izquierdo, vaya a **todos los servicios** > **Analytics** > **clústeres de HDInsight** y seleccione el clúster.

3. En la vista predeterminada, seleccione la **eliminar** icono. Siga las indicaciones para eliminar el clúster.
   
    ![eliminar icono](./media/hdinsight-delete-cluster/deletecluster.png)

## <a name="azure-powershell-az-module"></a>Módulo de Azure PowerShell Az

Reemplace `CLUSTERNAME` con el nombre del clúster de HDInsight en el código siguiente. Desde un símbolo del sistema de PowerShell, escriba el siguiente comando para eliminar el clúster:

```powershell
Remove-AzHDInsightCluster -ClusterName CLUSTERNAME
```

## <a name="azure-cli"></a>Azure CLI

Reemplace `CLUSTERNAME` con el nombre del clúster de HDInsight, y `RESOURCEGROUP` con el nombre del grupo de recursos en el código siguiente.  Desde un símbolo del sistema, escriba lo siguiente para eliminar el clúster:

```azurecli
az hdinsight delete --name CLUSTERNAME --resource-group RESOURCEGROUP
```
