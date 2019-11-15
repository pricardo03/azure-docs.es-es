---
title: Plantillas de Azure Resource Manager para la API de Gremlin de Azure Cosmos DB
description: Use las plantillas de Azure Resource Manager para crear y configurar la API de Gremlin de Azure Cosmos DB.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 08/05/2019
ms.author: mjbrown
ms.openlocfilehash: f50a6980cb26d6a89e2564b492a8be900ad57736
ms.sourcegitcommit: c62a68ed80289d0daada860b837c31625b0fa0f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2019
ms.locfileid: "73606785"
---
# <a name="manage-azure-cosmos-db-gremlin-api-resources-using-azure-resource-manager-templates"></a>Administrar recursos de la API de Gremlin de Azure Cosmos DB con plantillas de Azure Resource Manager

En este artículo se explica cómo realizar diferentes operaciones para automatizar la administración de las cuentas, las bases de datos y los contenedores de Azure Cosmos DB con plantillas de Azure Resource Manager. Este artículo solo tiene ejemplos de cuentas de API de Gremlin; para buscar ejemplos de otras cuentas de tipo de API, vea los artículos sobre el uso de plantillas de Resource Manager con la API de Azure Cosmos DB para [Cassandra](manage-cassandra-with-resource-manager.md), [SQL](manage-sql-with-resource-manager.md), [MongoDB](manage-mongodb-with-resource-manager.md) y [Table](manage-table-with-resource-manager.md).

## Crear una API de Azure Cosmos DB para la cuenta, la base de datos y la colección de MongoDB <a id="create-resource"></a>

Cree recursos de Azure Cosmos DB con una plantilla de Azure Resource Manager. Esta plantilla creará una cuenta de Azure Cosmos para la API de Gremlin con dos grafos que compartirán el rendimiento de 400 RU/s en el nivel de base de datos. Copie la plantilla y realice la implementación tal y como se muestra a continuación o visite [Azure Quickstart Gallery](https://azure.microsoft.com/resources/templates/101-cosmosdb-gremlin/) (Galería de inicio rápido de Azure) y realice la implementación desde Azure Portal. También puede descargar la plantilla en el equipo local o crear una nueva plantilla y especificar la ruta de acceso local con el parámetro `--template-file`.

> [!NOTE]
> Los nombres de las cuentas deben estar en minúsculas y deben tener 31 caracteres o menos.

[!code-json[create-cosmos-gremlin](~/quickstart-templates/101-cosmosdb-gremlin/azuredeploy.json)]

## <a name="deploy-with-azure-cli"></a>Implementación con la CLI de Azure

Para implementar la plantilla de Resource Manager mediante la CLI de Azure, **copie** el script y seleccione **Probar** para abrir Azure Cloud Shell. Para pegar el script, haga clic con el botón derecho en el shell y luego seleccione **Pegar**:

```azurecli-interactive

read -p 'Enter the Resource Group name: ' resourceGroupName
read -p 'Enter the location (i.e. westus2): ' location
read -p 'Enter the account name: ' accountName
read -p 'Enter the primary region (i.e. westus2): ' primaryRegion
read -p 'Enter the secondary region (i.e. eastus2): ' secondaryRegion
read -p 'Enter the database name: ' databaseName
read -p 'Enter the first graph name: ' graph1Name
read -p 'Enter the second graph name: ' graph2Name

az group create --name $resourceGroupName --location $location
az group deployment create --resource-group $resourceGroupName \
   --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-cosmosdb-gremlin/azuredeploy.json \
   --parameters accountName=$accountName primaryRegion=$primaryRegion secondaryRegion=$secondaryRegion databaseName=$databaseName \
   graph1Name=$graph1Name graph2Name=$graph2Name

az cosmosdb show --resource-group $resourceGroupName --name accountName --output tsv
```

El comando `az cosmosdb show` muestra la cuenta de Azure Cosmos recién creada después de que se haya aprovisionado. Si decide usar una versión de la CLI de Azure instalada localmente en lugar de usar CloudShell, consulte el artículo [Interfaz de la línea de comandos (CLI) de Azure](/cli/azure/).

## Actualizar el rendimiento (RU/s) de una base de datos <a id="database-ru-update"></a>

La siguiente plantilla actualizará el rendimiento de una base de datos. Copie la plantilla y realice la implementación tal y como se muestra a continuación o visite [Azure Quickstart Gallery](https://azure.microsoft.com/resources/templates/101-cosmosdb-gremlin-database-ru-update/) (Galería de inicio rápido de Azure) y realice la implementación desde Azure Portal. También puede descargar la plantilla en el equipo local o crear una nueva plantilla y especificar la ruta de acceso local con el parámetro `--template-file`.

[!code-json[cosmosdb-gremlin-database-ru-update](~/quickstart-templates/101-cosmosdb-gremlin-database-ru-update/azuredeploy.json)]

### <a name="deploy-database-template-via-azure-cli"></a>Implementar la plantilla de base de datos mediante la CLI de Azure

Para implementar la plantilla de Resource Manager mediante la CLI de Azure, seleccione **Probar** para abrir Azure Cloud Shell. Para pegar el script, haga clic con el botón derecho en el shell y luego seleccione **Pegar**:

```azurecli-interactive
read -p 'Enter the Resource Group name: ' resourceGroupName
read -p 'Enter the account name: ' accountName
read -p 'Enter the database name: ' databaseName
read -p 'Enter the new throughput: ' throughput

az group deployment create --resource-group $resourceGroupName \
   --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-cosmosdb-gremlin-database-ru-update/azuredeploy.json \
   --parameters accountName=$accountName databaseName=$databaseName throughput=$throughput
```

## Actualizar el rendimiento (RU/s) de un grafo <a id="graph-ru-update"></a>

La siguiente plantilla actualizará el rendimiento de un grafo. Copie la plantilla y realice la implementación tal y como se muestra a continuación o visite [Azure Quickstart Gallery](https://azure.microsoft.com/resources/templates/101-cosmosdb-gremlin-graph-ru-update/) (Galería de inicio rápido de Azure) y realice la implementación desde Azure Portal. También puede descargar la plantilla en el equipo local o crear una nueva plantilla y especificar la ruta de acceso local con el parámetro `--template-file`.

[!code-json[cosmosdb-gremlin-graph-ru-update](~/quickstart-templates/101-cosmosdb-gremlin-graph-ru-update/azuredeploy.json)]

### <a name="deploy-graph-template-via-azure-cli"></a>Implementar la plantilla de grafo mediante la CLI de Azure

Para implementar la plantilla de Resource Manager mediante la CLI de Azure, seleccione **Probar** para abrir Azure Cloud Shell. Para pegar el script, haga clic con el botón derecho en el shell y luego seleccione **Pegar**:

```azurecli-interactive
read -p 'Enter the Resource Group name: ' resourceGroupName
read -p 'Enter the account name: ' accountName
read -p 'Enter the database name: ' databaseName
read -p 'Enter the graph name: ' graphName
read -p 'Enter the new throughput: ' throughput

az group deployment create --resource-group $resourceGroupName \
   --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-cosmosdb-gremlin-graph-ru-update/azuredeploy.json \
   --parameters accountName=$accountName databaseName=$databaseName graphName=$graphName throughput=$throughput
```

## <a name="next-steps"></a>Pasos siguientes

Estos son algunos recursos adicionales:

- [Documentación de Azure Resource Manager](/azure/azure-resource-manager/)
- [Esquema del proveedor de recursos de Azure Cosmos DB](/azure/templates/microsoft.documentdb/allversions)
- [Plantillas de inicio rápido de Azure Cosmos DB](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.DocumentDB&pageNumber=1&sort=Popular)
- [Solución de errores comunes de implementación de Azure Resource Manager](../azure-resource-manager/resource-manager-common-deployment-errors.md)