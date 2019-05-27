---
title: Plantillas de Azure Resource Manager para Azure Cosmos DB Table API
description: Usar plantillas de Azure Resource Manager para crear y configurar Azure Cosmos DB Table API.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/20/2019
ms.author: mjbrown
ms.openlocfilehash: 82e2a436bf6b25b6164d845d234896390a262292
ms.sourcegitcommit: e9a46b4d22113655181a3e219d16397367e8492d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/21/2019
ms.locfileid: "65968825"
---
# <a name="manage-azure-cosmos-db-table-api-resources-using-azure-resource-manager-templates"></a>Administrar los recursos de Azure Cosmos DB Table API con plantillas de Azure Resource Manager

## Crear tabla y la cuenta de Azure Cosmos <a id="create-resource"></a>

Crear recursos de Azure Cosmos DB mediante una plantilla de Azure Resource Manager. Esta plantilla creará una cuenta de Azure Cosmos para Table API con una tabla con un rendimiento de 400 RU/s. Copie la plantilla e implementar tal y como se muestra a continuación o visite [Galería de inicio rápido de Azure](https://azure.microsoft.com/resources/templates/101-cosmosdb-table/) e implementar desde el portal de Azure. También puede descargar la plantilla en el equipo local o crear una nueva plantilla y especificar la ruta de acceso local con el `--template-file` parámetro.

[!code-json[create-cosmos-table](~/quickstart-templates/101-cosmosdb-table/azuredeploy.json)]

### <a name="deploy-via-powershell"></a>Implementar a través de PowerShell

Para implementar la plantilla de Resource Manager mediante PowerShell, **copia** el script y seleccione **Pruébelo** para abrir Azure Cloud shell. Para pegar el script, haga clic en el shell y, a continuación, seleccione **pegar**:

```azurepowershell-interactive

$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
$accountName = Read-Host -Prompt "Enter the account name"
$location = Read-Host -Prompt "Enter the location (i.e. westus2)"
$primaryRegion = Read-Host -Prompt "Enter the primary region (i.e. westus2)"
$secondaryRegion = Read-Host -Prompt "Enter the secondary region (i.e. eastus2)"
$tableName = Read-Host -Prompt "Enter the table name"

New-AzResourceGroup -Name $resourceGroupName -Location $location
New-AzResourceGroupDeployment `
    -ResourceGroupName $resourceGroupName `
    -TemplateUri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-cosmosdb-table/azuredeploy.json" `
    -primaryRegion $primaryRegion `
    -secondaryRegion $secondaryRegion `
    -tableName $tableName

 (Get-AzResource --ResourceType "Microsoft.DocumentDb/databaseAccounts" --ApiVersion "2015-04-08" --ResourceGroupName $resourceGroupName).name
```

Si decide usar una versión instalada localmente de PowerShell en lugar de desde Azure Cloud shell, deberá [instalar](/powershell/azure/install-az-ps) el módulo Azure PowerShell. Ejecute `Get-Module -ListAvailable Az` para encontrar la versión.

### <a name="deploy-via-azure-cli"></a>Implementación a través de la CLI de Azure

Para implementar la plantilla de Resource Manager mediante la CLI de Azure, **copia** el script y seleccione **Pruébelo** para abrir Azure Cloud shell. Para pegar el script, haga clic en el shell y, a continuación, seleccione **pegar**:

```azurecli-interactive
read -p 'Enter the Resource Group name: ' resourceGroupName
read -p 'Enter the location (i.e. westus2): ' location
read -p 'Enter the account name: ' accountName
read -p 'Enter the primary region (i.e. westus2): ' primaryRegion
read -p 'Enter the secondary region (i.e. eastus2): ' secondaryRegion
read -p 'Enter the table name: ' tableName

az group create --name $resourceGroupName --location $location
az group deployment create --resource-group $resourceGroupName \
   --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-cosmosdb-table/azuredeploy.json \
   --parameters accountName=$accountName primaryRegion=$primaryRegion secondaryRegion=$secondaryRegion tableName=$tableName

az cosmosdb show --resource-group $resourceGroupName --name accountName --output tsv
```

El `az cosmosdb show` comando muestra la cuenta de Azure Cosmos recién creada después de que se haya aprovisionado. Si decide usar una versión de CLI de Azure instalada localmente en lugar de usar CloudShell, consulte [interfaz de línea de comandos (CLI) de Azure](/cli/azure/) artículo.

## Actualización de procesamiento (RU/s) en una tabla <a id="table-ru-update"></a>

La siguiente plantilla actualizará el rendimiento de una tabla. Copie la plantilla e implementar tal y como se muestra a continuación o visite [Galería de inicio rápido de Azure](https://azure.microsoft.com/resources/templates/101-cosmosdb-table-ru-update/) e implementar desde el portal de Azure. También puede descargar la plantilla en el equipo local o crear una nueva plantilla y especificar la ruta de acceso local con el `--template-file` parámetro.

[!code-json[cosmosdb-table-ru-update](~/quickstart-templates/101-cosmosdb-table-ru-update/azuredeploy.json)]

### <a name="deploy-table-throughput-via-powershell"></a>Implementar el rendimiento de la tabla a través de PowerShell

Para implementar la plantilla de Resource Manager mediante PowerShell, **copia** el script y seleccione **Pruébelo** para abrir Azure Cloud shell. Para pegar el script, haga clic en el shell y, a continuación, seleccione **pegar**:

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
$accountName = Read-Host -Prompt "Enter the account name"
$tableName = Read-Host -Prompt "Enter the table name"
$throughput = Read-Host -Prompt "Enter new throughput for table"

New-AzResourceGroupDeployment `
    -ResourceGroupName $resourceGroupName `
    -TemplateUri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-cosmosdb-table-ru-update/azuredeploy.json" `
    -accountName $accountName `
    -tableName $tableName `
    -throughput $throughput
```

### <a name="deploy-table-template-via-azure-cli"></a>Implementar la plantilla de tabla mediante la CLI de Azure

Para implementar la plantilla de Resource Manager mediante la CLI de Azure, seleccione **Pruébelo** para abrir Azure Cloud shell. Para pegar el script, haga clic en el shell y, a continuación, seleccione **pegar**:

```azurecli-interactive
read -p 'Enter the Resource Group name: ' resourceGroupName
read -p 'Enter the account name: ' accountName
read -p 'Enter the table name: ' tableName
read -p 'Enter the new throughput: ' throughput

az group deployment create --resource-group $resourceGroupName \
   --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-cosmosdb-table-ru-update/azuredeploy.json \
   --parameters accountName=$accountName tableName=$tableName throughput=$throughput
```

## <a name="next-steps"></a>Pasos siguientes

Estos son algunos recursos adicionales:

- [Documentación de Azure Resource Manager](/azure/azure-resource-manager/)
- [Esquema de proveedor de recursos de Azure Cosmos DB](/azure/templates/microsoft.documentdb/allversions)
- [Plantillas de Azure Cosmos DB Quickstart](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.DocumentDB&pageNumber=1&sort=Popular)
- [Solucionar errores comunes de implementación de Azure Resource Manager](../azure-resource-manager/resource-manager-common-deployment-errors.md)