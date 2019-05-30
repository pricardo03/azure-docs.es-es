---
title: Aprenda a administrar cuentas de base de datos en Azure Cosmos DB
description: Aprenda a administrar cuentas de base de datos en Azure Cosmos DB
author: markjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 05/23/2019
ms.author: mjbrown
ms.openlocfilehash: 07d177987db1dea261520e8ee2543d871d552acb
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2019
ms.locfileid: "66240888"
---
# <a name="manage-an-azure-cosmos-account"></a>Administración de una cuenta de Azure Cosmos

En este artículo se describe cómo administrar varias tareas en una cuenta de Azure Cosmos mediante Azure Portal, Azure PowerShell, la CLI de Azure y plantillas de Azure Resource Manager.

## <a name="create-an-account"></a>Crear una cuenta

### <a id="create-database-account-via-portal"></a>Azure Portal

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

### <a id="create-database-account-via-cli"></a>Azure CLI

```azurecli-interactive
# Create an account
$resourceGroupName = 'myResourceGroup'
$accountName = 'myaccountname' # must be lower case.

az cosmosdb create \
   --name $accountName \
   --resource-group $resourceGroupName \
   --kind GlobalDocumentDB \
   --default-consistency-level Session \
   --locations WestUS=0 EastUS=1 \
   --enable-multiple-write-locations true
```

### <a id="create-database-account-via-ps"></a>Azure PowerShell.
```azurepowershell-interactive
# Create an Azure Cosmos Account for Core (SQL) API
$resourceGroupName = "myResourceGroup"
$location = "West US"
$accountName = "mycosmosaccount" # must be lower case.

$locations = @(
    @{ "locationName"="West US"; "failoverPriority"=0 },
    @{ "locationName"="East US"; "failoverPriority"=1 }
)

$consistencyPolicy = @{
    "defaultConsistencyLevel"="BoundedStaleness";
    "maxIntervalInSeconds"=300;
    "maxStalenessPrefix"=100000
}

$CosmosDBProperties = @{
    "databaseAccountOfferType"="Standard";
    "locations"=$locations;
    "consistencyPolicy"=$consistencyPolicy;
    "enableMultipleWriteLocations"="true"
}

New-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName -Location $location `
    -Name $accountName -PropertyObject $CosmosDBProperties
```

### <a id="create-database-account-via-arm-template"></a>Plantilla de Azure Resource Manager

Esta plantilla de Azure Resource Manager creará una cuenta de Azure Cosmos DB para cualquier API compatible configurada con dos regiones y opciones para seleccionar nivel de coherencia, conmutación automática por error y arquitectura multimaestro. Para implementar esta plantilla, haga clic en Implementar en Azure en la página Léame, [Creación de una cuenta de Azure Cosmos DB](https://github.com/Azure/azure-quickstart-templates/tree/master/101-cosmosdb-create-multi-region-account)

## <a name="addremove-regions-from-your-database-account"></a>Incorporación o eliminación de regiones de una cuenta de base de datos

### <a id="add-remove-regions-via-portal"></a>Azure Portal

1. Inicie sesión en el [portal de Azure](https://portal.azure.com). 

1. Vaya a la cuenta de Azure Cosmos y abra el menú **Replicar datos globalmente**.

1. Para agregar regiones, seleccione los hexágonos del mapa con la etiqueta **+** correspondiente a la región (o regiones) deseada. Otra forma de agregar una región es seleccionar la opción **+ Agregar región** y elegir una región en el menú desplegable.

1. Para quitar regiones, desactive una o varias regiones del mapa; para ello, seleccione los hexágonos azules con marcas de verificación. O seleccione el icono de "Papelera" (🗑) junto a la región en el lado derecho.

1. Seleccione **Aceptar** para guardar los cambios.

   ![Menú Agregar o eliminar regiones](./media/how-to-manage-database-account/add-region.png)

En el modo de escritura de región individual, no puede quitar la región de escritura. Debe realizar la conmutación por error a otra región antes de poder eliminar la región de escritura actual.

En el modo de escritura de varias regiones, puede agregar o quitar cualquier región, siempre que tenga al menos una región.

### <a id="add-remove-regions-via-cli"></a>Azure CLI

```azurecli-interactive
$resourceGroupName = 'myResourceGroup'
$accountName = 'myaccountname'

# Create an account with 1 region
az cosmosdb create --name $accountName --resource-group $resourceGroupName --locations westus=0

# Add a region
az cosmosdb update --name $accountName --resource-group $resourceGroupName --locations westus=0 eastus=1

# Remove a region
az cosmosdb update --name $accountName --resource-group $resourceGroupName --locations westus=0
```

### <a id="add-remove-regions-via-ps"></a>Azure PowerShell.

```azurepowershell-interactive
# Create an account with 1 region
$resourceGroupName = "myResourceGroup"
$location = "West US"
$accountName = "mycosmosaccount" # must be lower case.

$locations = @( @{ "locationName"="West US"; "failoverPriority"=0 } )
$consistencyPolicy = @{ "defaultConsistencyLevel"="Session" }
$CosmosDBProperties = @{
    "databaseAccountOfferType"="Standard";
    "locations"=$locations;
    "consistencyPolicy"=$consistencyPolicy
}
New-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName -Location $location `
    -Name $accountName -PropertyObject $CosmosDBProperties

# Add a region
$account = Get-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName -Name $accountName

$locations = @( 
    @{ "locationName"="West US"; "failoverPriority"=0 },
    @{ "locationName"="East Us"; "failoverPriority"=1 } 
)

$account.Properties.locations = $locations
$CosmosDBProperties = $account.Properties

Set-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $accountName -PropertyObject $CosmosDBProperties

# Azure Resource Manager does not wait on the resource update
Write-Host "Confirm region added before continuing..."

# Remove a region
$account = Get-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName -Name $accountName

$locations = @( @{ "locationName"="West US"; "failoverPriority"=0 } )

$account.Properties.locations = $locations
$CosmosDBProperties = $account.Properties

Set-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $accountName -PropertyObject $CosmosDBProperties
```

## <a id="configure-multiple-write-regions"></a>Configuración de varias regiones de escritura

### <a id="configure-multiple-write-regions-portal"></a>Azure Portal

Abra la pestaña **Replicar datos globalmente** y seleccione **Habilitar** para habilitar las escrituras en varias regiones. Después de habilitar las escrituras en varias regiones, todas las regiones de lectura que actualmente tiene en la cuenta se convertirán en regiones de lectura y escritura. 

> [!NOTE]
> No puede deshabilitar las escrituras en varias regiones una vez que las habilita. 

![Captura de pantalla de la cuenta de Azure Cosmos que configura la arquitectura multimaestro](./media/how-to-manage-database-account/single-to-multi-master.png)

Póngase en contacto con el alias askcosmosdb@microsoft.com si tiene más preguntas sobre esta característica. 

### <a id="configure-multiple-write-regions-cli"></a>Azure CLI

```azurecli-interactive
$resourceGroupName = 'myResourceGroup'
$accountName = 'myaccountname'
az cosmosdb update --name $accountName --resource-group $resourceGroupName --enable-multiple-write-locations true
```

### <a id="configure-multiple-write-regions-ps"></a>Azure PowerShell.

```azurepowershell-interactive
# Update an Azure Cosmos Account from single to multi-master

$account = Get-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName -Name $accountName

$account.Properties.enableMultipleWriteLocations = "true"
$CosmosDBProperties = $account.Properties

Set-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $accountName -PropertyObject $CosmosDBProperties
```

### <a id="configure-multiple-write-regions-arm"></a>plantilla de Resource Manager

Es posible migrar una cuenta desde un maestro único a una arquitectura multimaestro mediante la implementación de la plantilla de Resource Manager que se usa para crear la cuenta y estableciendo `enableMultipleWriteLocations: true`. La plantilla de Azure Resource Manager siguiente es una plantilla mínima que implementará una cuenta de Azure Cosmos DB para SQL API con una sola región y la arquitectura multimaestro habilitadas.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "name": {
            "type": "String"
        },
        "location": {
            "type": "String",
            "defaultValue": "[resourceGroup().location]"
        }
    },
    "resources": [
        {
            "type": "Microsoft.DocumentDb/databaseAccounts",
            "kind": "GlobalDocumentDB",
            "name": "[parameters('name')]",
            "apiVersion": "2015-04-08",
            "location": "[parameters('location')]",
            "tags": {},
            "properties": {
                "databaseAccountOfferType": "Standard",
                "consistencyPolicy": { "defaultConsistencyLevel": "Session" },
                "locations": [
                    {
                        "locationName": "[parameters('location')]",
                        "failoverPriority": 0
                    }
                ],
                "enableMultipleWriteLocations": true
            }
        }
    ]
}
```

## <a id="automatic-failover"></a>Habilitación de la conmutación automática por error de la cuenta de Azure Cosmos DB

La opción de conmutación automática por error permite que Azure Cosmos DB conmute por error en la región con la prioridad de conmutación por error más alta sin acción del usuario si una región deja de estar disponible. Cuando la conmutación automática por error está habilitada, es posible modificar la prioridad de región. La cuenta debe tener dos o más regiones para habilitar la conmutación automática por error.

### <a id="enable-automatic-failover-via-portal"></a>Azure Portal

1. En la cuenta de Azure Cosmos DB, abra el panel **Replicar datos globalmente**.

2. En la parte superior del panel, seleccione **Conmutación automática por error**.

   ![Menú Replicar datos globalmente](./media/how-to-manage-database-account/replicate-data-globally.png)

3. En el panel **Conmutación automática por error**, asegúrese de que **Habilitar la conmutación automática por error** está establecido en **Activado**. 

4. Seleccione **Guardar**.

   ![Menú Conmutación por error automática del portal](./media/how-to-manage-database-account/automatic-failover.png)

### <a id="enable-automatic-failover-via-cli"></a>Azure CLI

```azurecli-interactive
# Enable automatic failover on an existing account
$resourceGroupName = 'myResourceGroup'
$accountName = 'myaccountname'

az cosmosdb update --name $accountName --resource-group $resourceGroupName --enable-automatic-failover true
```

### <a id="enable-automatic-failover-via-ps"></a>Azure PowerShell.

```azurepowershell-interactive
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"

$account = Get-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $accountName

$account.Properties.enableAutomaticFailover="true";
$CosmosDBProperties = $account.Properties;

Set-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $accountName -PropertyObject $CosmosDBProperties
```

## <a name="set-failover-priorities-for-your-azure-cosmos-account"></a>Establecimiento de las prioridades de conmutación por error para la cuenta de Azure Cosmos

Una vez que una cuenta de Cosmos se configura para la conmutación automática por error, es posible modificar la prioridad de la conmutación por error para las regiones.

> [!IMPORTANT]
> No se puede modificar la región de escritura (prioridad de conmutación por error de cero) cuando la cuenta está configurada para la conmutación automática por error. Para cambiar la región de escritura, debe deshabilitar la conmutación automática por error y realizar una conmutación por error manual.

### <a id="set-failover-priorities-via-portal"></a>Azure Portal

1. En la cuenta de Azure Cosmos, abra el panel **Replicar datos globalmente**.

2. En la parte superior del panel, seleccione **Conmutación automática por error**.

   ![Menú Replicar datos globalmente](./media/how-to-manage-database-account/replicate-data-globally.png)

3. En el panel **Conmutación automática por error**, asegúrese de que **Habilitar la conmutación automática por error** está establecido en **Activado**.

4. Para modificar la prioridad de la conmutación por error, arrastre las regiones de lectura mediante los tres puntos del lado izquierdo de la fila que aparecen al mantener el puntero sobre ellos.

5. Seleccione **Guardar**.

   ![Menú Conmutación por error automática del portal](./media/how-to-manage-database-account/automatic-failover.png)

### <a id="set-failover-priorities-via-cli"></a>Azure CLI

```azurecli-interactive
# Assume region order is initially eastus=0 westus=1 southeastasia=2 on account creation
$resourceGroupName = 'myResourceGroup'
$accountName = 'myaccountname'

az cosmosdb failover-priority-change --name $accountName --resource-group $resourceGroupName --failover-policies eastus=0 southeastasia=1 westus=2
```

### <a id="set-failover-priorities-via-ps"></a>Azure PowerShell.

```azurepowershell-interactive
# Assume account currently has regions with priority: West US = 0, East US = 1, Southeast Asia = 2
$resourceGroupName = "myResourceGroup"
$accountName = "myaccountname"

$failoverPolicies = @(
    @{ "locationName"="West US"; "failoverPriority"=0 },
    @{ "locationName"="Southeast Asia"; "failoverPriority"=1 },
    @{ "locationName"="East US"; "failoverPriority"=2 }
)

Invoke-AzResourceAction -Action failoverPriorityChange `
    -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" `
    -ResourceGroupName $resourceGroupName -Name $accountName -Parameters $failoverPolicies
```

## <a id="manual-failover"></a>Realización de una conmutación por error manual en una cuenta de Azure Cosmos

> [!IMPORTANT]
> La cuenta de Azure Cosmos debe estar configurada para la conmutación por error manual para que esta operación se realice correctamente.

El proceso para realizar una conmutación por error manual implica cambiar la región de escritura de la cuenta (prioridad de conmutación por error = 0) a otra región configurada para la cuenta.

> [!NOTE]
> Las cuentas de arquitectura multimaestro no se puede conmutar por error de manera manual. Para las aplicaciones que usan el SDK de Azure Cosmos DB, el SDK detectará cuando una región deja de estar disponible y luego redirige automáticamente a la región más cercana siguiente si se usa una API de hospedaje múltiple en el SDK.

### <a id="enable-manual-failover-via-portal"></a>Azure Portal

1. Vaya a la cuenta de Azure Cosmos y abra el menú **Replicar datos globalmente**.

2. En la parte superior del menú, seleccione **Conmutación por error manual**.

   ![Menú Replicar datos globalmente](./media/how-to-manage-database-account/replicate-data-globally.png)

3. En el menú **Conmutación por error manual**, seleccione la nueva región de escritura. Active la casilla de verificación para indicar que comprende que esta opción cambia la región de escritura.

4. Para desencadenar la conmutación por error, seleccione **Aceptar**.

   ![Menú Conmutación por error del portal](./media/how-to-manage-database-account/manual-failover.png)

### <a id="enable-manual-failover-via-cli"></a>Azure CLI

```azurecli-interactive
# Assume account currently has regions with priority: eastus=0 westus=1
# Change the priority order to trigger a failover of the write region
$resourceGroupName = 'myResourceGroup'
$accountName = 'myaccountname'

az cosmosdb update --name $accountName --resource-group $resourceGroupName --locations westus=0 eastus=1
```

### <a id="enable-manual-failover-via-ps"></a>Azure PowerShell.

```azurepowershell-interactive
# Assume account currently has regions with priority: West US = 0, East US = 1
# Change the priority order to trigger a failover of the write region
$resourceGroupName = "myResourceGroup"
$accountName = "myaccountname"

$account = Get-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $accountName

$locations = @(
    @{ "locationName"="East US"; "failoverPriority"=0 },
    @{ "locationName"="West US"; "failoverPriority"=1 }
)

$account.Properties.locations=$locations;
$CosmosDBProperties = $account.Properties;

Set-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $accountName -PropertyObject $CosmosDBProperties
```

## <a name="next-steps"></a>Pasos siguientes

Para más información y ejemplos sobre cómo administrar la cuenta de Azure Cosmos así como la base de datos y los contenedores, lea estos artículos:

* [Manage Azure Cosmos DB using Azure PowerShell](manage-with-powershell.md) (Administración de Azure Cosmos DB con Azure PowerShell)
* [Manage Azure Cosmos DB using Azure CLI](manage-with-cli.md) (Administración de Azure Cosmos DB con la CLI de Azure)
