---
title: Plantillas de Azure Resource Manager para la API de MongoDB de Azure Cosmos DB
description: Usar plantillas de Azure Resource Manager para crear y configurar la API de Azure Cosmos DB para MongoDB.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/20/2019
ms.author: mjbrown
ms.openlocfilehash: 99f1e41107c277c8b3f1b21f81952d5d5cadaa29
ms.sourcegitcommit: e9a46b4d22113655181a3e219d16397367e8492d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/21/2019
ms.locfileid: "65968871"
---
# <a name="manage-azure-cosmos-db-mongodb-api-resources-using-azure-resource-manager-templates"></a>Administrar los recursos de la API de MongoDB de Azure Cosmos DB mediante plantillas de Azure Resource Manager

## Creación de API de Azure Cosmos DB para MongoDB cuenta, base de datos y colección <a id="create-resource"></a>

Crear recursos de Azure Cosmos DB mediante una plantilla de Azure Resource Manager. Esta plantilla creará una cuenta de Azure Cosmos para API de MongoDB con dos colecciones que compartan el rendimiento de 400 RU/s en el nivel de base de datos. Copie la plantilla e implementar tal y como se muestra a continuación o visite [Galería de inicio rápido de Azure](https://azure.microsoft.com/resources/templates/101-cosmosdb-mongodb/) e implementar desde el portal de Azure. También puede descargar la plantilla en el equipo local o crear una nueva plantilla y especificar la ruta de acceso local con el `--template-file` parámetro.

[!code-json[create-cosmos-mongo](~/quickstart-templates/101-cosmosdb-mongodb/azuredeploy.json)]

### <a name="deploy-via-azure-cli"></a>Implementación a través de la CLI de Azure

Para implementar la plantilla de Resource Manager mediante la CLI de Azure, **copia** el script y seleccione **Pruébelo** para abrir Azure Cloud shell. Para pegar el script, haga clic en el shell y, a continuación, seleccione **pegar**:

```azurecli-interactive

read -p 'Enter the Resource Group name: ' resourceGroupName
read -p 'Enter the location (i.e. westus2): ' location
read -p 'Enter the account name: ' accountName
read -p 'Enter the primary region (i.e. westus2): ' primaryRegion
read -p 'Enter the secondary region (i.e. eastus2): ' secondaryRegion
read -p 'Enter the database name: ' databaseName
read -p 'Enter the first collection name: ' collection1Name
read -p 'Enter the second collection name: ' collection2Name

az group create --name $resourceGroupName --location $location
az group deployment create --resource-group $resourceGroupName \
  --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-cosmosdb-mongodb/azuredeploy.json \
  --parameters accountName=$accountName primaryRegion=$primaryRegion secondaryRegion=$secondaryRegion \
  databaseName=$databaseName collection1Name=$collection1Name collection2Name=$collection2Name

az cosmosdb show --resource-group $resourceGroupName --name accountName --output tsv
```

El `az cosmosdb show` comando muestra la cuenta de Azure Cosmos recién creada después de que se haya aprovisionado. Si decide usar una versión de CLI de Azure instalada localmente en lugar de usar CloudShell, consulte [interfaz de línea de comandos (CLI) de Azure](/cli/azure/) artículo.

## Actualización de procesamiento (RU/s) en una base de datos <a id="database-ru-update"></a>

La siguiente plantilla actualizará el rendimiento de una base de datos. Copie la plantilla e implementar tal y como se muestra a continuación o visite [Galería de inicio rápido de Azure](https://azure.microsoft.com/resources/templates/101-cosmosdb-mongodb-database-ru-update/) e implementar desde el portal de Azure. También puede descargar la plantilla en el equipo local o crear una nueva plantilla y especificar la ruta de acceso local con el `--template-file` parámetro.

[!code-json[cosmosdb-mongodb-database-ru-update](~/quickstart-templates/101-cosmosdb-mongodb-database-ru-update/azuredeploy.json)]

### <a name="deploy-database-template-via-azure-cli"></a>Implementar la plantilla de base de datos mediante la CLI de Azure

Para implementar la plantilla de Resource Manager mediante la CLI de Azure, seleccione **Pruébelo** para abrir Azure Cloud shell. Para pegar el script, haga clic en el shell y, a continuación, seleccione **pegar**:

```azurecli-interactive
read -p 'Enter the Resource Group name: ' resourceGroupName
read -p 'Enter the account name: ' accountName
read -p 'Enter the database name: ' databaseName
read -p 'Enter the new throughput: ' throughput

az group deployment create --resource-group $resourceGroupName \
   --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-cosmosdb-mongodb-database-ru-update/azuredeploy.json \
   --parameters accountName=$accountName databaseName=$databaseName throughput=$throughput
```

## Actualización de procesamiento (RU/s) en una colección <a id="collection-ru-update"></a>

La siguiente plantilla actualizará el rendimiento de la colección. Copie la plantilla e implementar tal y como se muestra a continuación o visite [Galería de inicio rápido de Azure](https://azure.microsoft.com/resources/templates/101-cosmosdb-mongodb-collection-ru-update/) e implementar desde el portal de Azure. También puede descargar la plantilla en el equipo local o crear una nueva plantilla y especificar la ruta de acceso local con el `--template-file` parámetro.

[!code-json[cosmosdb-mongodb-collection-ru-update](~/quickstart-templates/101-cosmosdb-mongodb-collection-ru-update/azuredeploy.json)]

### <a name="deploy-collection-template-via-azure-cli"></a>Implementar la plantilla de la colección mediante la CLI de Azure

Para implementar la plantilla de Resource Manager mediante la CLI de Azure, seleccione **Pruébelo** para abrir Azure Cloud shell. Para pegar el script, haga clic en el shell y, a continuación, seleccione **pegar**:

```azurecli-interactive
read -p 'Enter the Resource Group name: ' resourceGroupName
read -p 'Enter the account name: ' accountName
read -p 'Enter the database name: ' databaseName
read -p 'Enter the collection name: ' collectionName
read -p 'Enter the new throughput: ' throughput

az group deployment create --resource-group $resourceGroupName \
   --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-cosmosdb-mongodb-collection-ru-update/azuredeploy.json \
   --parameters accountName=$accountName databaseName=$databaseName collectionName=$collectionName throughput=$throughput
```

## <a name="next-steps"></a>Pasos siguientes

Estos son algunos recursos adicionales:

- [Documentación de Azure Resource Manager](/azure/azure-resource-manager/)
- [Esquema de proveedor de recursos de Azure Cosmos DB](/azure/templates/microsoft.documentdb/allversions)
- [Plantillas de Azure Cosmos DB Quickstart](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.DocumentDB&pageNumber=1&sort=Popular)
- [Solucionar errores comunes de implementación de Azure Resource Manager](../azure-resource-manager/resource-manager-common-deployment-errors.md)