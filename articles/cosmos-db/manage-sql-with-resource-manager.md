---
title: Creación y administración de Azure Cosmos DB con plantillas de Azure Resource Manager
description: Use las plantillas de Azure Resource Manager para crear y configurar la API de Azure Cosmos DB para SQL (Core)
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/31/2019
ms.author: mjbrown
ms.openlocfilehash: 5babcadee02da0ba3e112f75e8b4d1aed5f3339f
ms.sourcegitcommit: bc7725874a1502aa4c069fc1804f1f249f4fa5f7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/07/2019
ms.locfileid: "73721079"
---
# <a name="manage-azure-cosmos-db-sql-core-api-resources-using-azure-resource-manager-templates"></a>Administrar recursos de API de Azure Cosmos DB SQL (Core) con plantillas de Azure Resource Manager

En este artículo se explica cómo realizar diferentes operaciones para automatizar la administración de las cuentas, las bases de datos y los contenedores de Azure Cosmos DB con plantillas de Azure Resource Manager. Este artículo solo tiene ejemplos de cuentas de API de SQL; para buscar ejemplos de otras cuentas de tipo de API, consulte los artículos sobre el uso de plantillas de Resource Manager con la API de Azure Cosmos DB para [Cassandra](manage-cassandra-with-resource-manager.md), [Gremlin](manage-gremlin-with-resource-manager.md), [MongoDB](manage-mongodb-with-resource-manager.md) y [Table](manage-table-with-resource-manager.md).

Creación y administración de cuentas de Cosmos DB, bases de datos y contenedores de MongoDB, Gremlin, Cassandra y Table API.

## Cree una cuenta, una base de datos y un contenedor de Azure Cosmos <a id="create-resource"></a>

Cree recursos de Azure Cosmos DB con una plantilla de Azure Resource Manager. Esta plantilla creará una cuenta de Azure Cosmos con dos contenedores que comparten un rendimiento de 400 RU/s en el nivel de base de datos y un contenedor único con un rendimiento dedicado de 400 RU/s. Copie la plantilla y realice la implementación tal y como se muestra a continuación o visite [Azure Quickstart Gallery](https://azure.microsoft.com/resources/templates/101-cosmosdb-sql/) (Galería de inicio rápido de Azure) y realice la implementación desde Azure Portal. También puede descargar la plantilla en el equipo local o crear una nueva plantilla y especificar la ruta de acceso local con el parámetro `--template-file`.

> [!NOTE]
>
> - No puede agregar ni quitar ubicaciones en una cuenta de Azure Cosmos a la vez que modifica otras propiedades. Estas acciones deben realizarse como operaciones independientes.
> - Los nombres de las cuentas deben estar en minúsculas y deben tener 44 caracteres o menos.
> - Para actualizar las RU/s, vuelva a enviar la plantilla con los valores de propiedad de rendimiento actualizados.

[!code-json[create-cosmosdb-sql](~/quickstart-templates/101-cosmosdb-sql/azuredeploy.json)]

> [!NOTE]
> Para crear un contenedor con una clave de partición de gran tamaño, incluya la propiedad `"version":2` en el objeto `partitionKey` de la plantilla anterior.

### <a name="deploy-via-powershell"></a>Implementación mediante PowerShell

Para implementar la plantilla de Resource Manager mediante PowerShell, **copie** el script y seleccione **Probar** para abrir Azure Cloud Shell. Para pegar el script, haga clic con el botón derecho en el shell y luego seleccione **Pegar**:

```azurepowershell-interactive

$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
$accountName = Read-Host -Prompt "Enter the account name"
$location = Read-Host -Prompt "Enter the location (i.e. westus2)"
$primaryRegion = Read-Host -Prompt "Enter the primary region (i.e. westus2)"
$secondaryRegion = Read-Host -Prompt "Enter the secondary region (i.e. eastus2)"
$databaseName = Read-Host -Prompt "Enter the database name"
$sharedThroughput = Read-Host -Prompt "Enter the shared database throughput (i.e. 400)"
$sharedContainer1Name = Read-Host -Prompt "Enter the first shared container name"
$sharedContainer2Name = Read-Host -Prompt "Enter the second shared container name"
$dedicatedContainer1Name = Read-Host -Prompt "Enter the dedicated container name"
$dedicatedThroughput = Read-Host -Prompt "Enter the dedicated container throughput (i.e. 400)"

New-AzResourceGroup -Name $resourceGroupName -Location $location
New-AzResourceGroupDeployment `
    -ResourceGroupName $resourceGroupName `
    -TemplateUri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-cosmosdb-sql/azuredeploy.json" `
    -accountName $accountName `
    -location $location `
    -primaryRegion $primaryRegion `
    -secondaryRegion $secondaryRegion `
    -databaseName $databaseName `
    -sharedThroughput $ $sharedThroughput `
    -sharedContainer1Name $sharedContainer1Name `
    -sharedContainer2Name $sharedContainer2Name `
    -dedicatedContainer1Name $dedicatedContainer1Name `
    -dedicatedThroughput $dedicatedThroughput

 (Get-AzResource --ResourceType "Microsoft.DocumentDb/databaseAccounts" --ApiVersion "2019-08-01" --ResourceGroupName $resourceGroupName).name
```

Si decide usar una versión instalada localmente de PowerShell de forma local en lugar de en el shell de Azure Cloud, debe [instalar](/powershell/azure/install-az-ps) el módulo de Azure PowerShell. Ejecute `Get-Module -ListAvailable Az` para encontrar la versión.

### <a name="deploy-via-azure-cli"></a>Implementación mediante la CLI de Azure

Para implementar la plantilla de Resource Manager mediante la CLI de Azure, seleccione **Probar** para abrir Azure Cloud Shell. Para pegar el script, haga clic con el botón derecho en el shell y luego seleccione **Pegar**:

```azurecli-interactive
read -p 'Enter the Resource Group name: ' resourceGroupName
read -p 'Enter the location (i.e. westus2): ' location
read -p 'Enter the account name: ' accountName
read -p 'Enter the primary region (i.e. westus2): ' primaryRegion
read -p 'Enter the secondary region (i.e. eastus2): ' secondaryRegion
read -p 'Enter the database name: ' databaseName
read -p 'Enter the shared database throughput: sharedThroughput
read -p 'Enter the first shared container name: ' sharedContainer1Name
read -p 'Enter the second shared container name: ' sharedContainer2Name
read -p 'Enter the dedicated container name: ' dedicatedContainer1Name
read -p 'Enter the dedicated container throughput: dedicatedThroughput

az group create --name $resourceGroupName --location $location
az group deployment create --resource-group $resourceGroupName \
   --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-cosmosdb-sql/azuredeploy.json \
   --parameters accountName=$accountName \
   primaryRegion=$primaryRegion \
   secondaryRegion=$secondaryRegion \
   databaseName=$databaseName \
   sharedThroughput=$sharedThroughput \
   sharedContainer1Name=$sharedContainer1Name \
   sharedContainer2Name=$sharedContainer2Name \
   dedicatedContainer1Name=$dedicatedContainer1Name \
   dedicatedThroughput=$dedicatedThroughput

az cosmosdb show --resource-group $resourceGroupName --name accountName --output tsv
```

El comando `az cosmosdb show` muestra la cuenta de Azure Cosmos recién creada después de que se haya aprovisionado. Si decide usar una versión de la CLI de Azure instalada localmente en lugar de usar CloudShell, consulte el artículo [Interfaz de la línea de comandos (CLI) de Azure](/cli/azure/).

## Creación de un contenedor de Azure Cosmos DB con funcionalidad del lado servidor <a id="create-sproc"></a>

Cree un contenedor de Azure Cosmos DB con un procedimiento almacenado, un desencadenador y una función definida por el usuario mediante una plantilla de Azure Resource Manager. Copie la plantilla y realice la implementación tal y como se muestra a continuación o visite [Azure Quickstart Gallery](https://azure.microsoft.com/resources/templates/101-cosmosdb-sql-container-sprocs/) (Galería de inicio rápido de Azure) y realice la implementación desde Azure Portal. También puede descargar la plantilla en el equipo local o crear una nueva plantilla y especificar la ruta de acceso local con el parámetro `--template-file`.

[!code-json[create-cosmosdb-sql-sprocs](~/quickstart-templates/101-cosmosdb-sql-container-sprocs/azuredeploy.json)]

### <a name="deploy-stored-procedure-template-via-powershell"></a>Implementación de la plantilla de procedimiento almacenado a través de PowerShell

Para implementar la plantilla de Resource Manager mediante PowerShell, **copie** el script y seleccione **Probar** para abrir Azure Cloud Shell. Para pegar el script, haga clic con el botón derecho en el shell y luego seleccione **Pegar**:

```azurepowershell-interactive

$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
$accountName = Read-Host -Prompt "Enter the account name"
$location = Read-Host -Prompt "Enter the location (i.e. westus2)"
$primaryRegion = Read-Host -Prompt "Enter the primary region (i.e. westus2)"
$secondaryRegion = Read-Host -Prompt "Enter the secondary region (i.e. eastus2)"
$databaseName = Read-Host -Prompt "Enter the database name"
$containerName = Read-Host -Prompt "Enter the container name"

New-AzResourceGroup -Name $resourceGroupName -Location $location
New-AzResourceGroupDeployment `
    -ResourceGroupName $resourceGroupName `
    -TemplateUri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-cosmosdb-sql-container-sprocs/azuredeploy.json" `
    -accountName $accountName `
    -location $location `
    -primaryRegion $primaryRegion `
    -secondaryRegion $secondaryRegion `
    -databaseName $databaseName `
    -containerName $containerName

 (Get-AzResource --ResourceType "Microsoft.DocumentDb/databaseAccounts" --ApiVersion "2019-08-01" --ResourceGroupName $resourceGroupName).name
```

Si decide usar una versión instalada localmente de PowerShell de forma local en lugar de en el shell de Azure Cloud, debe [instalar](/powershell/azure/install-az-ps) el módulo de Azure PowerShell. Ejecute `Get-Module -ListAvailable Az` para encontrar la versión.

### <a name="deploy-stored-procedure-template-via-azure-cli"></a>Implementación de la plantilla de procedimiento almacenado a través de la CLI de Azure

Para implementar la plantilla de Resource Manager mediante la CLI de Azure, seleccione **Probar** para abrir Azure Cloud Shell. Para pegar el script, haga clic con el botón derecho en el shell y luego seleccione **Pegar**:

```azurecli-interactive
read -p 'Enter the Resource Group name: ' resourceGroupName
read -p 'Enter the location (i.e. westus2): ' location
read -p 'Enter the account name: ' accountName
read -p 'Enter the primary region (i.e. westus2): ' primaryRegion
read -p 'Enter the secondary region (i.e. eastus2): ' secondaryRegion
read -p 'Enter the database name: ' databaseName
read -p 'Enter the container name: ' containerName

az group create --name $resourceGroupName --location $location
az group deployment create --resource-group $resourceGroupName \
   --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-cosmosdb-sql-container-sprocs/azuredeploy.json \
   --parameters accountName=$accountName primaryRegion=$primaryRegion secondaryRegion=$secondaryRegion databaseName=$databaseName \
   containerName=$containerName

az cosmosdb show --resource-group $resourceGroupName --name accountName --output tsv
```

## <a name="next-steps"></a>Pasos siguientes

Estos son algunos recursos adicionales:

- [Documentación de Azure Resource Manager](/azure/azure-resource-manager/)
- [Esquema del proveedor de recursos de Azure Cosmos DB](/azure/templates/microsoft.documentdb/allversions)
- [Plantillas de inicio rápido de Azure Cosmos DB](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.DocumentDB&pageNumber=1&sort=Popular)
- [Solución de errores comunes de implementación de Azure Resource Manager](../azure-resource-manager/resource-manager-common-deployment-errors.md)
