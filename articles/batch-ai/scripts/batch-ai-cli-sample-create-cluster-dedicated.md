---
title: 'Ejemplo de script de la CLI de Azure: creación de un clúster de Batch AI especializado | Microsoft Docs'
description: 'Ejemplo de script de la CLI de Azure: creación y administración de un clúster de Batch AI de nodos (máquinas virtuales) especializados'
services: batch-ai
documentationcenter: ''
author: dlepow
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: batch-ai
ms.devlang: azurecli
ms.topic: sample
ms.tgt-pltfrm: multiple
ms.workload: na
ms.date: 07/26/2018
ms.author: danlep
ms.openlocfilehash: 10f3444f81dfaeac4331f0b7798ade7eefbd29fb
ms.sourcegitcommit: ebd06cee3e78674ba9e6764ddc889fc5948060c4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/07/2018
ms.locfileid: "44058192"
---
# <a name="cli-example-create-and-manage-a-batch-ai-cluster-of-dedicated-nodes"></a>Ejemplo de la CLI: creación y administración de un clúster de Batch AI de nodos especializados

Este script muestra algunos de los comandos disponibles en la CLI de Azure para crear y administrar un clúster de Batch AI formado por nodos (máquinas virtuales) especializados.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Si decide instalar y usar la CLI en un entorno local, para esta guía de inicio rápido es preciso que ejecute la versión 2.0.38 de la CLI de Azure o una posterior. Ejecute `az --version` para encontrar la versión. Si necesita instalarla o actualizarla, consulte [Instalación de la CLI de Azure 2.0](/cli/azure/install-azure-cli). 

## <a name="example-script"></a>Script de ejemplo

[!code-azurecli-interactive[main](../../../cli_scripts/batch-ai/create-cluster/create-cluster-dedicated.sh "Create Batch AI cluster - dedicated nodes")]

## <a name="clean-up-deployment"></a>Limpieza de la implementación

Ejecute el siguiente comando para quitar los grupos de recursos y todos los recursos asociados a ellos.

```azurecli-interactive
# Remove resource group for the cluster.
az group delete --name myResourceGroup

# Remove resource group for the auto-storage account.
az group delete --name batchaiautostorage
```

## <a name="script-explanation"></a>Explicación del script

Este script usa los siguientes comandos. Cada comando de la tabla crea un vínculo a documentación específica del comando.

| Get-Help | Notas |
|---|---|
| [az group create](/cli/azure/group#az-group-create) | Crea un grupo de recursos en el que se almacenan todos los recursos. |
| [az batchai workspace create](/cli/azure/batchai/workspace#az-batchai-workspace-create) | Crea un área de trabajo de Batch AI. |
| [az batchai cluster create](/cli/azure/batchai/cluster#az-batchai-cluster-create) | Crea un clúster de Batch AI. |
| [az batchai cluster show](/cli/azure/batchai/cluster#az-batchai-cluster-show) | Muestra información acerca de un clúster de Batch AI. |
| [az batchai cluster node list](/cli/azure/batchai/cluster/node#az-batchai-cluster-show) | Muestra los nodos de un clúster de Batch AI. |
| [az batchai cluster resize](/cli/azure/batchai/cluster#az-batchai-cluster-resize) | Cambia el tamaño del clúster de Batch AI.  |
| [az group delete](/cli/azure/group#az-group-delete) | Elimina un grupo de recursos, incluidos todos los recursos anidados. |

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre la CLI de Azure, consulte la [documentación de la CLI de Azure](https://docs.microsoft.com/cli/azure).
