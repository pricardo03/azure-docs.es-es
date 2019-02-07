---
title: 'Ejemplo de script de la CLI de Azure: obtención del nombre de host, los puertos y las claves para Azure Cache for Redis | Microsoft Docs'
description: 'Ejemplo de script de la CLI de Azure: obtención del nombre de host, los puertos y las claves para una instancia de Azure Cache for Redis'
services: azure-cache-for-redis
documentationcenter: ''
author: wesmc7777
manager: cfowler
editor: ''
tags: azure-service-management
ms.assetid: 761eb24e-2ba7-418d-8fc3-431153e69a90
ms.service: cache-redis
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 08/30/2017
ms.author: wesmc
ms.openlocfilehash: fd69c3182f026996618fea5e26466e8434ea36f6
ms.sourcegitcommit: 039263ff6271f318b471c4bf3dbc4b72659658ec
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/06/2019
ms.locfileid: "55754462"
---
# <a name="get-the-hostname-ports-and-keys-for-azure-cache-for-redis"></a>Obtención del nombre de host, los puertos y las claves para Azure Cache for Redis

En este escenario se aprende a recuperar el nombre de host, los puertos y las claves que se usan para conectarse a una instancia de Azure Cache for Redis.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

## <a name="sample-script"></a>Script de ejemplo

[!code-azurecli[main](../../../cli_scripts/redis-cache/cache-keys-ports/cache-keys-ports.sh "Azure Cache for Redis")]


## <a name="script-explanation"></a>Explicación del script

Este script usa los comandos siguientes para recuperar el nombre de host, las claves y los puertos de una instancia de Azure Cache for Redis. Cada comando de la tabla crea un vínculo a documentación específica del comando.

| Get-Help | Notas |
|---|---|
| [az redis show](https://docs.microsoft.com/cli/azure/redis) | Recuperación de los detalles de una instancia de Azure Cache for Redis. |
| [az redis list-keys](https://docs.microsoft.com/cli/azure/redis) | Recuperación de las teclas de acceso de una instancia de Azure Cache for Redis. |


## <a name="next-steps"></a>Pasos siguientes

Para más información sobre la CLI de Azure, consulte la [documentación de la CLI de Azure](https://docs.microsoft.com/cli/azure).

Encontrará más ejemplos de scripts de CLI de Azure Cache for Redis en la [documentación de Azure Cache for Redis](../cli-samples.md).
