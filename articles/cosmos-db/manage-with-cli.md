---
title: Administración de recursos de Azure Cosmos DB mediante la CLI de Azure
description: Use la CLI de Azure para administrar la cuenta, la base de datos y los contenedores de Azure Cosmos DB.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/23/2019
ms.author: mjbrown
ms.openlocfilehash: 144515fef9da714ab80f15bb39757ed2283c6dd0
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2019
ms.locfileid: "66243372"
---
# <a name="manage-azure-cosmos-resources-using-azure-cli"></a>Administración de recursos de Azure Cosmos mediante la CLI de Azure

En la guía siguiente se describe los comandos comunes para automatizar la administración de cuentas de Azure Cosmos DB, las bases de datos y contenedores mediante la CLI de Azure. Hay páginas de referencia para todos los comandos de CLI de Azure Cosmos DB disponibles en la [referencia de la CLI de Azure](https://docs.microsoft.com/cli/azure/cosmosdb). Para ver más ejemplos, consulte [Ejemplos de la CLI de Azure para Azure Cosmos DB](cli-samples.md), incluidos ejemplos sobre cómo crear y administrar cuentas, bases de datos y contenedores de Cosmos DB para MongoDB, Gremlin, Cassandra y Table API.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Si decide instalar y usar la CLI localmente, para este tema es preciso que ejecute la CLI de Azure versión 2.0 o posterior. Ejecute `az --version` para encontrar la versión. Si necesita instalarla o actualizarla, vea [Instalación de la CLI de Azure](/cli/azure/install-azure-cli).

## <a name="create-an-azure-cosmos-db-account"></a>Creación de una cuenta de Azure Cosmos DB

Para crear una cuenta de Azure Cosmos DB con SQL API, la coherencia de sesión en las regiones este de EE.UU. y oeste de Estados Unidos, ejecute el siguiente comando:

```azurecli-interactive
az cosmosdb create \
   --name mycosmosdbaccount \
   --resource-group myResourceGroup \
   --kind GlobalDocumentDB \
   --default-consistency-level Session \
   --locations EastUS=0 WestUS=1 \
   --enable-multiple-write-locations false
```

> [!IMPORTANT]
> El nombre de cuenta de Cosmos Azure debe estar en minúscula.

## <a name="create-a-database"></a>Creación de una base de datos

Para crear una base de datos de Cosmos DB, ejecute el siguiente comando:

```azurecli-interactive
az cosmosdb database create \
   --name mycosmosdbaccount \
   --db-name myDatabase \
   --resource-group myResourceGroup
```

## <a name="create-a-container"></a>Crear un contenedor

Para crear un contenedor de Cosmos DB con RU/s de 400 y una clave de partición, ejecute el siguiente comando:

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

Para cambiar el rendimiento de un contenedor de Cosmos DB a 1000 RU/s, ejecute el siguiente comando:

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

Para obtener las claves de su cuenta de Cosmos, ejecute el siguiente comando:

```azurecli-interactive
# List account keys
az cosmosdb list-keys \
   --name  mycosmosdbaccount \
   --resource-group myResourceGroup
```

## <a name="list-connection-strings"></a>Enumeración de cadenas de conexión

Para obtener las cadenas de conexión para la cuenta de Cosmos, ejecute el siguiente comando:

```azurecli-interactive
# List connection strings
az cosmosdb list-connection-strings \
   --name mycosmosdbaccount \
   --resource-group myResourceGroup
```

## <a name="regenerate-account-key"></a>Regeneración de la clave de cuenta

Para volver a generar una nueva clave principal para la cuenta de Cosmos, ejecute el siguiente comando:

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
