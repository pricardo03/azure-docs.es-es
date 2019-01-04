---
title: 'Ejemplo de script de CLI de Azure: creación de Azure Cache for Redis Premium con agrupación en clústeres | Microsoft Docs'
description: 'Ejemplo de script de CLI de Azure: Creación de Azure Cache for Redis de nivel Premium con agrupación en clústeres'
services: azure-cache-for-redis
documentationcenter: ''
author: wesmc7777
manager: cfowler
editor: ''
tags: azure-service-management
ms.assetid: 07bcceae-2521-4fe3-b88f-ed833104ddd2
ms.service: cache-redis
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 08/30/2017
ms.author: wesmc
ms.openlocfilehash: abd69e27cf95a882ae7c299cbe19e2c30c35a918
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/08/2018
ms.locfileid: "53096832"
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
| [az group create](https://docs.microsoft.com/cli/azure/group#az_group_create) | Crea un grupo de recursos en el que se almacenan todos los recursos. |
| [az redis create](https://docs.microsoft.com/cli/azure/redis#az_redis_create) | Creación de una instancia de Azure Cache for Redis. |


## <a name="next-steps"></a>Pasos siguientes

Para más información sobre la CLI de Azure, consulte la [documentación de la CLI de Azure](https://docs.microsoft.com/cli/azure).

Encontrará más ejemplos de scripts de CLI de Azure Cache for Redis en la [documentación de Azure Cache for Redis](../cli-samples.md).