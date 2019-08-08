---
title: Administración de recursos de Azure Cosmos DB mediante la CLI de Azure
description: Use la CLI de Azure para administrar la cuenta, la base de datos y los contenedores de Azure Cosmos DB.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 08/05/2019
ms.author: mjbrown
ms.openlocfilehash: 9ec049311fc158b13bba45deb2974d7cdd531f90
ms.sourcegitcommit: c8a102b9f76f355556b03b62f3c79dc5e3bae305
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2019
ms.locfileid: "68815040"
---
# <a name="manage-azure-cosmos-resources-using-azure-cli"></a>Administración de recursos de Azure Cosmos mediante la CLI de Azure

En la siguiente guía, se describen los comandos comunes para automatizar la administración de las cuentas, las bases de datos y los contenedores de Azure Cosmos DB mediante la CLI de Azure. Hay páginas de referencia para todos los comandos de CLI de Azure Cosmos DB disponibles en la [referencia de la CLI de Azure](https://docs.microsoft.com/cli/azure/cosmosdb). Para ver más ejemplos, consulte [Ejemplos de la CLI de Azure para Azure Cosmos DB](cli-samples.md), incluidos ejemplos sobre cómo crear y administrar cuentas, bases de datos y contenedores de Cosmos DB para MongoDB, Gremlin, Cassandra y Table API.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Si decide instalar y usar la CLI localmente, para este tema es preciso que ejecute la CLI de Azure versión 2.0 o posterior. Ejecute `az --version` para encontrar la versión. Si necesita instalarla o actualizarla, vea [Instalación de la CLI de Azure](/cli/azure/install-azure-cli).

## <a name="create-an-azure-cosmos-db-account"></a>Creación de una cuenta de Azure Cosmos DB

Para crear una cuenta de Azure Cosmos DB con SQL API, coherencia de sesión en las regiones Este de EE. UU. y Oeste de EE. UU., ejecute el siguiente comando:

```azurecli-interactive
az cosmosdb create \
   --name mycosmosdbaccount # must be lowercase and < 31 characters \
   --resource-group myResourceGroup \
   --kind GlobalDocumentDB \
   --default-consistency-level Session \
   --locations regionName=EastUS failoverPriority=0 isZoneRedundant=False \
   --locations regionName=WestUS failoverPriority=1 isZoneRedundant=False \
   --enable-multiple-write-locations false
```

> [!IMPORTANT]
> El nombre de la cuenta de Azure Cosmos debe estar en minúscula.

## <a name="create-a-database"></a>Creación de una base de datos

Para crear una base de datos de Cosmos DB, ejecute el siguiente comando:

```azurecli-interactive
az cosmosdb database create \
   --name mycosmosdbaccount \
   --db-name myDatabase \
   --resource-group myResourceGroup
```

## <a name="create-a-container"></a>Crear un contenedor

Para crear un contenedor de Cosmos DB con 400 RU/s y una clave de partición, ejecute el siguiente comando:

```azurecli-interactive
# Create a container
az cosmosdb collection create \
   --collection-name myContainer \
   --name mycosmosdbaccount \
   --db-name myDatabase \
   --resource-group myResourceGroup \
   --partition-key-path /myPartitionKey \
   --throughput 400
```

## <a name="change-the-throughput-of-a-container"></a>Cambio del rendimiento de un contenedor

Para cambiar el rendimiento de un contenedor de Cosmos DB de 1000 RU/s, ejecute el siguiente comando:

```azurecli-interactive
# Update container throughput
az cosmosdb collection update \
   --collection-name myContainer \
   --name mycosmosdbaccount \
   --db-name myDatabase \
   --resource-group myResourceGroup \
   --throughput 1000
```

## <a name="list-account-keys"></a>Enumeración de claves de cuenta

Para obtener las claves de la cuenta de Cosmos, ejecute el siguiente comando:

```azurecli-interactive
# List account keys
az cosmosdb keys list \
   --name  mycosmosdbaccount \
   --resource-group myResourceGroup
```

## <a name="list-connection-strings"></a>Enumeración de cadenas de conexión

Para obtener las cadenas de conexión de la cuenta de Cosmos, ejecute el siguiente comando:

```azurecli-interactive
# List connection strings
az cosmosdb list-connection-strings \
   --name mycosmosdbaccount \
   --resource-group myResourceGroup
```

## <a name="regenerate-account-key"></a>Regeneración de la clave de cuenta

Para regenerar una nueva clave principal de la cuenta de Cosmos, ejecute el siguiente comando:

```azurecli-interactive
# Regenerate account key
az cosmosdb regenerate-key \
   --name mycosmosdbaccount \
   --resource-group myResourceGroup \
   --key-kind primary
```

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre la CLI de Azure, consulte:

- [Instalación de la CLI de Azure](/cli/azure/install-azure-cli)
- [Referencia de CLI de Azure](https://docs.microsoft.com/cli/azure/cosmosdb)
- [Ejemplos adicionales de la CLI de Azure para Azure Cosmos DB](cli-samples.md)
