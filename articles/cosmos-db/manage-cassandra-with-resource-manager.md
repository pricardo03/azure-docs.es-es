---
title: Plantillas de Azure Resource Manager para API de Cassandra de Azure Cosmos DB
description: Usar plantillas de Azure Resource Manager para crear y configurar la API de Cassandra de Azure Cosmos DB.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/06/2019
ms.author: mjbrown
ms.openlocfilehash: db754adbe60acfa155400910c47de556db793eef
ms.sourcegitcommit: e9a46b4d22113655181a3e219d16397367e8492d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/21/2019
ms.locfileid: "65968900"
---
# <a name="manage-azure-cosmos-db-cassandra-api-resources-using-azure-resource-manager-templates"></a>Administrar los recursos de la API Cassandra de Azure Cosmos DB mediante plantillas de Azure Resource Manager

## Crear cuenta de Azure Cosmos, espacio de claves y tabla <a id="create-resource"></a>

Crear recursos de Azure Cosmos DB mediante una plantilla de Azure Resource Manager. Esta plantilla creará una cuenta de Azure Cosmos para API de Cassandra con dos tablas que compartan el rendimiento de 400 RU/s en el nivel de espacio de claves. Copie la plantilla e implementar tal y como se muestra a continuación o visite [Galería de inicio rápido de Azure](https://azure.microsoft.com/resources/templates/101-cosmosdb-cassandra/) e implementar desde el portal de Azure. También puede descargar la plantilla en el equipo local o crear una nueva plantilla y especificar la ruta de acceso local con el `--template-file` parámetro.

[!code-json[create-cosmos-Cassandra](~/quickstart-templates/101-cosmosdb-cassandra/azuredeploy.json)]

## <a name="deploy-with-azure-cli"></a>Implementación con la CLI de Azure

Para implementar la plantilla de Resource Manager mediante la CLI de Azure, **copia** el script y seleccione **Pruébelo** para abrir Azure Cloud shell. Para pegar el script, haga clic en el shell y, a continuación, seleccione **pegar**:

```azurecli-interactive

read -p 'Enter the Resource Group name: ' resourceGroupName
read -p 'Enter the location (i.e. westus2): ' location
read -p 'Enter the account name: ' accountName
read -p 'Enter the primary region (i.e. westus2): ' primaryRegion
read -p 'Enter the secondary region (i.e. eastus2): ' secondaryRegion
read -p 'Enter the keyset name: ' keysetName
read -p 'Enter the first table name: ' table1Name
read -p 'Enter the second table name: ' table2Name

az group create --name $resourceGroupName --location $location
az group deployment create --resource-group $resourceGroupName \
   --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-cosmosdb-cassandra/azuredeploy.json \
   --parameters accountName=$accountName primaryRegion=$primaryRegion secondaryRegion=$secondaryRegion keysetName=$keysetName \
   table1Name=$table1Name table2Name=$table2Name

az cosmosdb show --resource-group $resourceGroupName --name accountName --output tsv
```

El `az cosmosdb show` comando muestra la cuenta de Azure Cosmos recién creada después de que se haya aprovisionado. Si decide usar una versión de CLI de Azure instalada localmente en lugar de usar CloudShell, consulte [interfaz de línea de comandos (CLI) de Azure](/cli/azure/) artículo.

## Actualización de procesamiento (RU/s) en un espacio de claves <a id="keyspace-ru-update"></a>

La siguiente plantilla actualizará el rendimiento de un espacio de claves. Copie la plantilla e implementar tal y como se muestra a continuación o visite [Galería de inicio rápido de Azure](https://azure.microsoft.com/resources/templates/101-cosmosdb-cassandra-keyspace-ru-update/) e implementar desde el portal de Azure. También puede descargar la plantilla en el equipo local o crear una nueva plantilla y especificar la ruta de acceso local con el `--template-file` parámetro.

[!code-json[cosmosdb-cassandra-keyspace-ru-update](~/quickstart-templates/101-cosmosdb-cassandra-keyspace-ru-update/azuredeploy.json)]

### <a name="deploy-keyspace-template-via-azure-cli"></a>Implementar plantilla de espacio de claves mediante la CLI de Azure

Para implementar la plantilla de Resource Manager mediante la CLI de Azure, seleccione **Pruébelo** para abrir Azure Cloud shell. Para pegar el script, haga clic en el shell y, a continuación, seleccione **pegar**:

```azurecli-interactive
read -p 'Enter the Resource Group name: ' resourceGroupName
read -p 'Enter the account name: ' accountName
read -p 'Enter the keyspace name: ' keyspaceName
read -p 'Enter the new throughput: ' throughput

az group deployment create --resource-group $resourceGroupName \
   --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-cosmosdb-cassandra-keyspace-ru-update/azuredeploy.json \
   --parameters accountName=$accountName keyspaceName=$keyspaceName throughput=$throughput
```

## Actualización de procesamiento (RU/s) en una tabla <a id="table-ru-update"></a>

La siguiente plantilla actualizará el rendimiento de una tabla. Copie la plantilla e implementar tal y como se muestra a continuación o visite [Galería de inicio rápido de Azure](https://azure.microsoft.com/resources/templates/101-cosmosdb-cassandra-table-ru-update/) e implementar desde el portal de Azure. También puede descargar la plantilla en el equipo local o crear una nueva plantilla y especificar la ruta de acceso local con el `--template-file` parámetro.

[!code-json[cosmosdb-cassandra-table-ru-update](~/quickstart-templates/101-cosmosdb-cassandra-table-ru-update/azuredeploy.json)]

### <a name="deploy-table-template-via-azure-cli"></a>Implementar la plantilla de tabla mediante la CLI de Azure

Para implementar la plantilla de Resource Manager mediante la CLI de Azure, seleccione **Pruébelo** para abrir Azure Cloud shell. Para pegar el script, haga clic en el shell y, a continuación, seleccione **pegar**:

```azurecli-interactive
read -p 'Enter the Resource Group name: ' resourceGroupName
read -p 'Enter the account name: ' accountName
read -p 'Enter the keyspace name: ' keyspaceName
read -p 'Enter the table name: ' tableName
read -p 'Enter the new throughput: ' throughput

az group deployment create --resource-group $resourceGroupName \
   --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-cosmosdb-cassandra-table-ru-update/azuredeploy.json \
   --parameters accountName=$accountName keyspaceName=$keyspaceName tableName=$tableName throughput=$throughput
```

## <a name="next-steps"></a>Pasos siguientes

Estos son algunos recursos adicionales:

- [Documentación de Azure Resource Manager](/azure/azure-resource-manager/)
- [Esquema de proveedor de recursos de Azure Cosmos DB](/azure/templates/microsoft.documentdb/allversions)
- [Plantillas de Azure Cosmos DB Quickstart](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.DocumentDB&pageNumber=1&sort=Popular)
- [Solucionar errores comunes de implementación de Azure Resource Manager](../azure-resource-manager/resource-manager-common-deployment-errors.md)