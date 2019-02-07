---
title: 'Ejemplo de script de la CLI de Azure: calcular el tamaño del contenedor de blobs | Microsoft Docs'
description: Calcule el tamaño de un contenedor en Azure Blob Storage sumando el tamaño de los blobs del contenedor.
services: storage
documentationcenter: na
author: tamram
manager: timlt
editor: tysonn
ms.assetid: ''
ms.custom: mvc
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: azurecli
ms.topic: sample
ms.date: 06/28/2017
ms.author: tamram
ms.openlocfilehash: 3cb1e35617a58fcde7968ab45d437d865c91f983
ms.sourcegitcommit: a65b424bdfa019a42f36f1ce7eee9844e493f293
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/04/2019
ms.locfileid: "55696946"
---
# <a name="calculate-the-size-of-a-blob-storage-container"></a>Cálculo del tamaño de un contenedor de Blob Storage

Este script calcula el tamaño de un contenedor en Azure Blob Storage sumando el tamaño de los blobs de dicho contenedor.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

> [!IMPORTANT]
> Este script de la CLI proporciona un tamaño estimado del contenedor y no debe usarse para cálculos de facturación.

## <a name="sample-script"></a>Script de ejemplo

[!code-azurecli[main](../../../cli_scripts/storage/calculate-container-size/calculate-container-size.sh?highlight=2-3 "Calculate container size")]

## <a name="clean-up-deployment"></a>Limpieza de la implementación 

Ejecute el siguiente comando para quitar el grupo de recursos, el contenedor y todos los recursos relacionados.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Explicación del script

Este script usa los siguientes comandos para calcular el tamaño del contenedor de Blob Storage. Cada elemento de la tabla incluye vínculos a la documentación específica del comando.

| Get-Help | Notas |
|---|---|
| [az group create](/cli/azure/group) | Crea un grupo de recursos en el que se almacenan todos los recursos. |
| [az storage blob upload](/cli/azure/storage/account) | Carga archivos locales en un contenedor de Azure Blob Storage. |
| [az storage blob list](/cli/azure/storage/account/keys) | Enumera los blobs de un contenedor de Azure Blob Storage. |

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre la CLI de Azure, consulte la [documentación de la CLI de Azure](/cli/azure).

Puede encontrar ejemplos de script adicionales de la CLI de almacenamiento en los [ejemplos de la CLI de Azure para Azure Blob Storage](../blobs/storage-samples-blobs-cli.md).
