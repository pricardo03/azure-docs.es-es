---
title: 'Ejemplo de script de CLI de Azure: creación de Azure Cache for Redis Premium con agrupación en clústeres | Microsoft Docs'
description: 'Ejemplo de script de CLI de Azure: Creación de Azure Cache for Redis de nivel Premium con agrupación en clústeres'
services: cache
documentationcenter: ''
author: yegu-ms
manager: jhubbard
editor: ''
tags: azure-service-management
ms.assetid: 07bcceae-2521-4fe3-b88f-ed833104ddd2
ms.service: cache
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 08/30/2017
ms.author: yegu
ms.openlocfilehash: 18538cf713d2938b0b595c9d4664c39c6ada5231
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "62130456"
---
# <a name="create-a-premium-azure-cache-for-redis-with-clustering"></a>Creación de una instancia de Azure Cache for Redis de nivel Premium con agrupación en clústeres

En este escenario, aprenderá cómo crear Azure Cache for Redis de nivel Premium de 6 GB con la agrupación en clústeres habilitada y dos particiones.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

## <a name="sample-script"></a>Script de ejemplo

[!code-azurecli[main](../../../cli_scripts/redis-cache/create-premium-cache-cluster/create-premium-cache-cluster.sh "Azure Cache for Redis")]

[!INCLUDE [cli-script-clean-up](../../../includes/redis-cli-script-clean-up.md)]

## <a name="script-explanation"></a>Explicación del script

Este script usa los siguientes comandos para crear un grupo de recursos y una instancia de Azure Cache for Redis de nivel Premium con la habilitación de agrupación en clústeres. Cada comando de la tabla crea un vínculo a documentación específica del comando.

| Get-Help | Notas |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group) | Crea un grupo de recursos en el que se almacenan todos los recursos. |
| [az redis create](https://docs.microsoft.com/cli/azure/redis) | Creación de una instancia de Azure Cache for Redis. |


## <a name="next-steps"></a>Pasos siguientes

Para más información sobre la CLI de Azure, consulte la [documentación de la CLI de Azure](https://docs.microsoft.com/cli/azure).

Encontrará más ejemplos de scripts de CLI de Azure Cache for Redis en la [documentación de Azure Cache for Redis](../cli-samples.md).
