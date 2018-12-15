---
title: Administración de recursos de Azure Cosmos DB mediante la CLI de Azure
description: Use la CLI de Azure para administrar la cuenta, la base de datos y los contenedores de Azure Cosmos DB.
services: cosmos-db
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/23/2018
ms.author: mjbrown
ms.openlocfilehash: d79cae16c109b407840b8565eb80fb87bbed6432
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/04/2018
ms.locfileid: "52877554"
---
# <a name="manage-azure-cosmos-db-resources-using-azure-cli"></a>Administración de recursos de Azure Cosmos DB mediante la CLI de Azure

En la guía siguiente, se describen los comandos para automatizar la administración de las cuentas, las bases de datos y los contenedores de Azure Cosmos DB mediante la CLI de Azure. También incluye comandos para escalar el rendimiento del contenedor. Hay páginas de referencia para todos los comandos de CLI de Azure Cosmos DB disponibles en la [referencia de la CLI de Azure](https://docs.microsoft.com/cli/azure/cosmosdb). Para ver más ejemplos, consulte [Ejemplos de la CLI de Azure para Azure Cosmos DB](cli-samples.md), incluidos ejemplos sobre cómo crear y administrar cuentas, bases de datos y contenedores de Cosmos DB para MongoDB, Gremlin, Cassandra y Table API.

Este script de ejemplo de la CLI crea una cuenta, base de datos y contenedor de SQL API para Azure Cosmos DB.  

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Si decide instalar y usar la CLI localmente, para este tema es preciso que ejecute la CLI de Azure versión 2.0 o posterior. Ejecute `az --version` para encontrar la versión. Si necesita instalarla o actualizarla, vea [Instalación de la CLI de Azure](/cli/azure/install-azure-cli).

## <a name="create-an-azure-cosmos-db-account"></a>Creación de una cuenta de Azure Cosmos DB

Para crear una cuenta de Azure Cosmos DB con SQL API, coherencia de sesión y la arquitectura multimaestro habilitadas en las regiones Este de EE. UU. y Oeste de EE. UU., abra la CLI de Azure o Cloud Shell y ejecute el siguiente comando:

```azurecli-interactive
az cosmosdb create \
   –-name "myCosmosDbAccount" \
   --resource-group "myResourceGroup" \
   --kind GlobalDocumentDB \
   --default-consistency-level "Session" \
   --locations "EastUS=0" "WestUS=1" \
   --enable-multiple-write-locations true \
```

## <a name="create-a-database"></a>Creación de una base de datos

Para crear una base de datos de Cosmos DB, abra la CLI de Azure o Cloud Shell y ejecute el siguiente comando:

```azurecli-interactive
az cosmosdb database create \
   --name "myCosmosDbAccount" \
   --db-name "myDatabase" \
   --resource-group "myResourceGroup"
```

## <a name="create-a-container"></a>Crear un contenedor

Para crear un contenedor de Cosmos DB con 1000 RU/s y una clave de partición, abra la CLI de Azure o Cloud Shell y ejecute el siguiente comando:

```azurecli-interactive
# Create a container
az cosmosdb collection create \
   --collection-name "myContainer" \
   --name "myCosmosDbAccount" \
   --db-name "myDatabase" \
   --resource-group "myResourceGroup" \
   --partition-key-path = "/myPartitionKey" \
   --throughput 1000
```

## <a name="change-the-throughput-of-a-container"></a>Cambio del rendimiento de un contenedor

Para cambiar el rendimiento de un contenedor de Cosmos DB de 400 RU/s, abra la CLI de Azure o Cloud Shell y ejecute el siguiente comando:

```azurecli-interactive
# Update container throughput
az cosmosdb collection update \
   --collection-name "myContainer" \
   --name "myCosmosDbAccount" \
   --db-name "myDatabase" \
   --resource-group "myResourceGroup" \
   --throughput 400
```

## <a name="list-account-keys"></a>Enumeración de claves de cuenta

Cuando se crea una cuenta de Azure Cosmos DB, el servicio genera dos claves de acceso maestras que se pueden usar para la autenticación cuando se tiene acceso a la cuenta de Azure Cosmos DB. Al proporcionar dos claves de acceso, Azure Cosmos DB permite regenerar las claves sin interrupción en la cuenta de Azure Cosmos DB. También están disponibles las claves de solo lectura para autenticar las operaciones de solo lectura. Hay dos claves de lectura y escritura (principal y secundaria) y dos claves de solo lectura (principal y secundaria). Puede obtener las claves de la cuenta mediante el siguiente comando:

```azurecli-interactive
# List account keys
az cosmosdb list-keys \
   --name "myCosmosDbAccount"\
   --resource-group "myResourceGroup"
```

## <a name="list-connection-strings"></a>Enumeración de cadenas de conexión

La cadena de conexión para conectar la aplicación a la cuenta de Cosmos DB se puede recuperar con el comando siguiente.

```azurecli-interactive
# List connection strings
az cosmosdb list-connection-strings \
   --name "myCosmosDbAccount"\
   --resource-group "myResourceGroup"
```

## <a name="regenerate-account-key"></a>Regeneración de la clave de cuenta

Cambie periódicamente las claves de acceso de la cuenta de Azure Cosmos DB para mantener sus conexiones más seguras. Se asignan dos claves de acceso para que pueda mantener las conexiones con la cuenta de Azure Cosmos DB, de modo que puede usar una clave de acceso mientras regenera la otra.

```azurecli-interactive
# Regenerate account key
az cosmosdb regenerate-key \
   --name "myCosmosDbAccount"\
   --resource-group "myResourceGroup" \
   --key-kind primary
```

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre la CLI de Azure, consulte:

- [Instalación de la CLI de Azure](/cli/azure/install-azure-cli)
- [Referencia de CLI de Azure](https://docs.microsoft.com/cli/azure/cosmosdb)
- [Ejemplos adicionales de la CLI de Azure para Azure Cosmos DB](cli-samples.md)
