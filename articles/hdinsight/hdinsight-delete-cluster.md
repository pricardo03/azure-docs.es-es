---
title: Eliminación de un clúster de Azure HDInsight
description: Información sobre las distintas formas de eliminar un clúster de HDInsight.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 03/22/2018
ms.author: hrasheed
ms.custom: H1Hack27Feb2017,hdinsightactive
ms.openlocfilehash: 707112f755e6846622d2fde9de466551276925b8
ms.sourcegitcommit: 345b96d564256bcd3115910e93220c4e4cf827b3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2018
ms.locfileid: "52494369"
---
# <a name="delete-an-hdinsight-cluster-using-your-browser-powershell-or-the-azure-classic-cli"></a>Eliminación de un clúster de HDInsight con el explorador, PowerShell o la CLI clásica de Azure

La facturación del clúster de HDInsight se inicia una vez creado el clúster y solo se detiene cuando se elimina. Se facturan por minuto realizando una prorrata, por lo que siempre debe eliminar aquellos que ya no se estén utilizando. En este documento, aprenderá a eliminar un clúster mediante [Azure Portal](https://portal.azure.com), [Azure PowerShell](https://docs.microsoft.com/powershell/azure/) y la CLI de Azure clásica.

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

## <a name="azure-classic-cli"></a>CLI de Azure clásica

[!INCLUDE [classic-cli-warning](../../includes/requires-classic-cli.md)]

Desde un símbolo del sistema, utilice el siguiente comando para eliminar el clúster:

    azure hdinsight cluster delete CLUSTERNAME

Reemplace **CLUSTERNAME** por el nombre del clúster de HDInsight.
