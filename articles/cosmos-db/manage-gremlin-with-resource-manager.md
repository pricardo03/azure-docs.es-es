---
title: Plantillas de Azure Resource Manager para API de Gremlin de Azure Cosmos DB
description: Usar plantillas de Azure Resource Manager para crear y configurar la API de Gremlin de Azure Cosmos DB.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/06/2019
ms.author: mjbrown
ms.openlocfilehash: d1928606a22eba180ebd3f1e979362e75edaf2d7
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65077791"
---
# <a name="create-azure-cosmos-db-gremlin-api-resources-from-a-resource-manager-template"></a>Crear recursos de la API de Gremlin de Azure Cosmos DB desde una plantilla de Resource Manager

Obtenga información sobre cómo crear un recurso de API de Gremlin de Azure Cosmos DB mediante una plantilla de Azure Resource Manager. En el ejemplo siguiente se crea una API de Gremlin de Azure Cosmos DB desde un [plantilla de inicio rápido de Azure](https://aka.ms/gremlin-arm-qs). Esta plantilla creará una cuenta de Azure Cosmos para API de Gremlin con dos gráficos que compartan el rendimiento de 400 RU/s en el nivel de base de datos.

Esta es una copia de la plantilla:

[!code-json[create-cosmos-gremlin](~/quickstart-templates/101-cosmosdb-gremlin/azuredeploy.json)]

## <a name="deploy-with-azure-cli"></a>Implementación con la CLI de Azure

Para implementar la plantilla de Resource Manager mediante la CLI de Azure, **copia** el script y seleccione **Pruébelo** para abrir Azure Cloud shell. Para pegar el script, haga clic en el shell y, a continuación, seleccione **pegar**:

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

El `az cosmosdb show` comando muestra la cuenta de Azure Cosmos recién creada después de que se haya aprovisionado. Si decide usar una versión de CLI de Azure instalada localmente en lugar de usar CloudShell, consulte [interfaz de línea de comandos (CLI) de Azure](/cli/azure/) artículo.

En el ejemplo anterior, ha hecho referencia a una plantilla que se almacena en GitHub. También puede descargar la plantilla en el equipo local o crear una nueva plantilla y especificar la ruta de acceso local con el `--template-file` parámetro.

## <a name="next-steps"></a>Pasos siguientes

Estos son algunos recursos adicionales:

- [Documentación de Azure Resource Manager](/azure/azure-resource-manager/)
- [Esquema de proveedor de recursos de Azure Cosmos DB](/azure/templates/microsoft.documentdb/allversions)
- [Plantillas de Azure Cosmos DB Quickstart](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.DocumentDB&pageNumber=1&sort=Popular)
- [Solucionar errores comunes de implementación de Azure Resource Manager](../azure-resource-manager/resource-manager-common-deployment-errors.md)