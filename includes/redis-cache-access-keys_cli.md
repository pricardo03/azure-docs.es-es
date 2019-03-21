---
title: archivo de inclusión
description: archivo de inclusión
services: redis-cache
author: wesmc7777
ms.service: cache
ms.topic: include
ms.date: 03/28/2018
ms.author: wesmc
ms.custom: include file
ms.openlocfilehash: 778151e401624398b70101a242e4cf0be8e0a1b3
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "58114895"
---
### <a name="retrieve-host-name-ports-and-access-keys-using-azure-cli"></a>Recuperación del nombre de host, puertos y claves de acceso mediante la CLI de Azure

Para recuperar el nombre de host y puertos mediante la CLI de Azure se puede llamar a [show de az redis](https://docs.microsoft.com/cli/azure/redis#az_redis_show)y para recuperar las claves que se puede llamar a [az redis enumerar claves](https://docs.microsoft.com/cli/azure/redis#az_redis_list_keys). El siguiente script llama a estos dos comandos y devuelve el nombre de host, puertos y claves en la consola.

```azurecli
#/bin/bash

# Retrieve the hostname, ports, and keys for contosoCache located in contosoGroup

# Retrieve the hostname and ports for an Azure Cache for Redis instance
redis=($(az redis show --name contosoCache --resource-group contosoGroup --query [hostName,enableNonSslPort,port,sslPort] --output tsv))

# Retrieve the keys for an Azure Cache for Redis instance
keys=($(az redis list-keys --name contosoCache --resource-group contosoGroup --query [primaryKey,secondaryKey] --output tsv))

# Display the retrieved hostname, keys, and ports
echo "Hostname:" ${redis[0]}
echo "Non SSL Port:" ${redis[2]}
echo "Non SSL Port Enabled:" ${redis[1]}
echo "SSL Port:" ${redis[3]}
echo "Primary Key:" ${keys[0]}
echo "Secondary Key:" ${keys[1]}
```

Para obtener más información acerca de esta secuencia de comandos, consulte [obtener el nombre de host, puertos y claves de caché de Azure para Redis](../articles/azure-cache-for-redis/scripts/cache-keys-ports.md). Para obtener más información sobre la CLI de Azure consulte [instalar la CLI de Azure](https://docs.microsoft.com/cli/azure/install-azure-cli) y [empezar a trabajar con la CLI de Azure](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli).
