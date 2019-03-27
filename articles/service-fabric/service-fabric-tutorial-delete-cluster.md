---
title: Eliminación de un clúster de Service Fabric en Azure | Microsoft Docs
description: En este tutorial, aprenderá a eliminar un clúster de Service Fabric hospedado en Azure y todos sus recursos. Puede eliminar el grupo de recursos que contiene el clúster o eliminar los recursos de forma selectiva.
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 09/26/2018
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: 62372b4ce4032e019219b5be8c5a49ea135782c1
ms.sourcegitcommit: 12d67f9e4956bb30e7ca55209dd15d51a692d4f6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2019
ms.locfileid: "58223281"
---
# <a name="tutorial-remove-a-service-fabric-cluster-running-in-azure"></a>Tutorial: Eliminación de un clúster de Service Fabric que se ejecuta en Azure

Este tutorial es la quinta parte de una serie y le muestra cómo eliminar un clúster de Service Fabric que se ejecuta en Azure. Para eliminar completamente un clúster de Service Fabric, también debe eliminar todos los recursos que usa el clúster. Tiene dos opciones: eliminar el grupo de recursos en el que se encuentra el clúster (lo que elimina el recurso del clúster y todos los demás recursos del grupo) o eliminar específicamente el recurso del clúster y sus recursos asociados (pero ningún otro recurso del grupo).

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Eliminar un grupo de recursos y todos sus recursos
> * Eliminar recursos de un grupo de recursos de forma selectiva

En esta serie de tutoriales, se aprende a:
> [!div class="checklist"]
> * Creación de un [clúster de Windows](service-fabric-tutorial-create-vnet-and-windows-cluster.md) en Azure mediante una plantilla
> * [Supervisión de un clúster](service-fabric-tutorial-monitor-cluster.md)
> * [Escalado o reducción horizontal](service-fabric-tutorial-scale-cluster.md)
> * [Actualización del entorno en tiempo de ejecución de un clúster](service-fabric-tutorial-upgrade-cluster.md)
> * Eliminación de un clúster

## <a name="prerequisites"></a>Requisitos previos

Antes de empezar este tutorial:

* Si no tiene ninguna suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
* Instale la [versión 4.1 o superior del módulo de Azure PowerShell](https://docs.microsoft.com/powershell/azure/azurerm/install-azurerm-ps) o la [CLI de Azure](/cli/azure/install-azure-cli).
* Creación de un [clúster de Windows](service-fabric-tutorial-create-vnet-and-windows-cluster.md) seguro en Azure

## <a name="delete-the-resource-group-containing-the-service-fabric-cluster"></a>Eliminación del grupo de recursos que contiene el clúster de Service Fabric
La manera más sencilla de eliminar el clúster y todos los recursos que consume es eliminar el grupo de recursos.

Inicie sesión en Azure y seleccione el identificador de suscripción con el que quiere quitar el clúster.  Para encontrar el identificador de suscripción, inicie sesión en [Azure Portal](https://portal.azure.com). Elimine el grupo de recursos y todos los recursos de clúster mediante el cmdlet [Remove-AzureRMResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) o el comando [az group delete](/cli/azure/group?view=azure-cli-latest).

```powershell
Connect-AzureRmAccount
Set-AzureRmContext -SubscriptionId <guid>
$groupname = "sfclustertutorialgroup"
Remove-AzureRmResourceGroup -Name $groupname -Force
```

```azurecli
az login
az account set --subscription <guid>
ResourceGroupName="sfclustertutorialgroup"
az group delete --name $ResourceGroupName
```

## <a name="selectively-delete-the-cluster-resource-and-the-associated-resources"></a>Eliminación selectiva del recurso de clúster y de los recursos asociados
Si el grupo de recursos tiene únicamente recursos que están relacionados con el clúster de Service Fabric que quiere eliminar, lo más sencillo es eliminar el grupo de recursos completo. Si desea eliminar de forma selectiva los recursos del grupo de recursos y conservar los recursos no asociados con el clúster, siga estos pasos.

Enumere los recursos del grupo de recursos:

```powershell
Connect-AzureRmAccount
Set-AzureRmContext -SubscriptionId <guid>
$groupname = "sfclustertutorialgroup"
Get-AzureRmResource -ResourceGroupName $groupname | ft
```

```azurecli
az login
az account set --subscription <guid>
ResourceGroupName="sfclustertutorialgroup"
az resource list --resource-group $ResourceGroupName
```

Para cada uno de los recursos que quiera eliminar, ejecute el siguiente script:

```powershell
Remove-AzureRmResource -ResourceName "<name of the Resource>" -ResourceType "<Resource Type>" -ResourceGroupName $groupname -Force
```

```azurecli
az resource delete --name "<name of the Resource>" --resource-type "<Resource Type>" --resource-group $ResourceGroupName
```

Para eliminar el recurso de clúster, ejecute el siguiente script:

```powershell
Remove-AzureRmResource -ResourceName "<name of the Resource>" -ResourceType "Microsoft.ServiceFabric/clusters" -ResourceGroupName $groupname -Force
```

```azurecli
az resource delete --name "<name of the Resource>" --resource-type "Microsoft.ServiceFabric/clusters" --resource-group $ResourceGroupName
```

## <a name="next-steps"></a>Pasos siguientes

En este tutorial aprendió lo siguiente:

> [!div class="checklist"]
> * Eliminar un grupo de recursos y todos sus recursos
> * Eliminar recursos de un grupo de recursos de forma selectiva

Ahora que ha completado este tutorial, pruebe lo siguiente:
* Aprenda a inspeccionar y administrar un clúster de Service Fabric con [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md).
* Aprenda a [revisar el sistema operativo Windows](service-fabric-patch-orchestration-application.md) de los nodos de clúster.
* Aprenda a agregar y recopilar eventos para [clústeres de Windows](service-fabric-diagnostics-event-aggregation-wad.md) y a [configurar Log Analytics](service-fabric-diagnostics-oms-setup.md) para supervisar los eventos de clúster.
