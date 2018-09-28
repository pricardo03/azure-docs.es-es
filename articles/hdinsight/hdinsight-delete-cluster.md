---
title: Eliminación de un clúster de Azure HDInsight
description: Información sobre las distintas formas de eliminar un clúster de HDInsight.
services: hdinsight
author: jasonwhowell
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 03/22/2018
ms.author: jasonh
ms.custom: H1Hack27Feb2017,hdinsightactive
ms.openlocfilehash: 39404ff74552b11e982cf5968c0eb131ea642e27
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46979461"
---
# <a name="delete-an-hdinsight-cluster-using-your-browser-powershell-or-the-azure-classic-cli"></a>Eliminación de un clúster de HDInsight con el explorador, PowerShell o la CLI clásica de Azure

La facturación del clúster de HDInsight se inicia una vez creado el clúster y solo se detiene cuando se elimina. Se facturan por minuto realizando una prorrata, por lo que siempre debe eliminar aquellos que ya no se estén utilizando. En este documento, aprenderá a eliminar un clúster mediante Azure Portal, Azure PowerShell y la CLI clásica de Azure.

> [!IMPORTANT]
> Al eliminar un clúster de HDInsight, no se eliminan las cuentas de Azure Storage o Data Lake Store asociadas a este. Puede volver a usar los datos almacenados en esos servicios en el futuro.

## <a name="azure-portal"></a>Azure Portal

1. Inicie sesión en [Azure Portal](https://portal.azure.com) y seleccione su clúster de HDInsight. Si este no está anclado al panel, puede buscarlo por su nombre utilizando el campo de búsqueda.
   
    ![búsqueda del portal](./media/hdinsight-delete-cluster/navbar.png)

2. En la configuración del clúster, seleccione el icono **Eliminar**. Cuando se le pida, seleccione **Sí** para eliminar el clúster.
   
    ![eliminar icono](./media/hdinsight-delete-cluster/deletecluster.png)

## <a name="azure-powershell"></a>Azure PowerShell

Desde un símbolo del sistema de PowerShell, utilice el siguiente comando para eliminar el clúster:

    Remove-AzureRmHDInsightCluster -ClusterName CLUSTERNAME

Reemplace **CLUSTERNAME** por el nombre del clúster de HDInsight.

## <a name="azure-classic-cli"></a>CLI clásica de Azure

[!INCLUDE [classic-cli-warning](../../includes/requires-classic-cli.md)]

Desde un símbolo del sistema, utilice el siguiente comando para eliminar el clúster:

    azure hdinsight cluster delete CLUSTERNAME

Reemplace **CLUSTERNAME** por el nombre del clúster de HDInsight.
