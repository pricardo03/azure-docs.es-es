---
title: Crear y administrar Azure Cosmos DB mediante PowerShell
description: Use Azure Powershell para administrar sus cuentas de Azure Cosmos DB, las bases de datos, contenedores y rendimiento.
author: markjbrown
ms.service: cosmos-db
ms.topic: samples
ms.date: 05/23/2019
ms.author: mjbrown
ms.custom: seodec18
ms.openlocfilehash: f720b678f2c7a6e564ef3e8fa9ae071b004ed1a6
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2019
ms.locfileid: "66243389"
---
# <a name="manage-azure-cosmos-db-sql-api-resources-using-powershell"></a>Administrar los recursos de Azure Cosmos DB SQL API con PowerShell

En la guía siguiente se describe cómo usar PowerShell para el script y automatizar la administración de Azure Cosmos DB, incluida la cuenta, base de datos, contenedor y rendimiento. Administración de Azure Cosmos DB es con el proveedor de recursos directamente mediante el cmdlet AzResource, pero no a través de cmdlets específicos de Azure Cosmos DB. Para ver todas las propiedades que se pueden administrar con PowerShell para el proveedor de recursos de Azure Cosmos DB, consulte [esquema del proveedor de recursos de Azure Cosmos DB](/azure/templates/microsoft.documentdb/allversions)

Para la administración multiplataforma de Azure Cosmos DB, puede usar [CLI de Azure](manage-with-cli.md), el [API de REST][rp-rest-api], o el [portal Azure](create-sql-api-dotnet.md#create-account).

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="getting-started"></a>Introducción

Siga las instrucciones de [cómo instalar y configurar Azure PowerShell] [ powershell-install-configure] para instalar e iniciar sesión su cuenta de Azure en Powershell.

* Si desea ejecutar los siguientes comandos sin necesidad de confirmación del usuario, anexe la marca `-Force` al comando.
* Todos los siguientes comandos son sincrónicos.

## <a name="azure-cosmos-accounts"></a>Azure Cosmos Accounts

Las siguientes secciones muestran cómo administrar la cuenta de Azure Cosmos, incluidos:

* [Crear una cuenta de Azure Cosmos](#create-account)
* [Actualizar una cuenta de Azure Cosmos](#update-account)
* [Obtener una cuenta de Azure Cosmos](#get-account)
* [Eliminación de una cuenta de Azure Cosmos](#delete-account)
* [Actualizar las etiquetas para una cuenta de Azure Cosmos](#update-tags)
* [Enumeración de claves para una cuenta de Azure Cosmos](#list-keys)
* [Regenerar claves para una cuenta de Azure Cosmos](#regenerate-keys)
* [Lista de cadenas de conexión para una cuenta de Azure Cosmos](#list-connection-strings)
* [Modificar la prioridad de conmutación por error para una cuenta de Azure Cosmos](#modify-failover-priority)

### <a id="create-account"></a> Crear una cuenta de Azure Cosmos

Este comando le permite crear una cuenta de base de datos de Azure Cosmos DB. Configure la nueva cuenta de base de datos como de región única o de [varias regiones][distribute-data-globally] con una determinada [directiva de coherencia](consistency-levels.md).

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

* `$accountName` El nombre de la cuenta de Azure Cosmos. Debe estar en minúsculas, acepta alfanuméricos y '-' carácter y entre 3 y 50 caracteres.
* `$location` La ubicación de la cuenta de Azure Cosmos.
* `$locations` Las regiones de réplica para la cuenta de base de datos. Debe haber una región de escritura por cuenta de base de datos con un valor de prioridad de conmutación por error de 0.
* `$consistencyPolicy` El nivel de coherencia predeterminado de la cuenta de Azure Cosmos. Para más información, consulte [Niveles de coherencia en Azure Cosmos DB](consistency-levels.md).
* `$CosmosDBProperties` Los valores de propiedad se pasan al proveedor Cosmos DB Azure Resource Manager para aprovisionar la cuenta.

Puntos de conexión de servicio de cuentas pueden configurarse con Firewall de IP, así como la red Virtual de Azure Cosmos. Para obtener información sobre cómo configurar el Firewall para Azure Cosmos DB, consulte [configurar Firewall de IP](how-to-configure-firewall.md).  Para obtener más información sobre cómo habilitar los puntos de conexión de servicio para Azure Cosmos DB, consulte [configurar acceso desde redes virtuales](how-to-configure-vnet-service-endpoint.md) .

### <a id="get-account"></a> Obtiene las propiedades de una cuenta de Azure Cosmos

Este comando le permite obtener las propiedades de una cuenta existente de Azure Cosmos.

```azurepowershell-interactive
# Get the properties of an Azure Cosmos Account

$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"

Get-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $accountName | Select-Object Properties
```

### <a id="update-account"></a> Actualizar una cuenta de Azure Cosmos

Este comando permite actualizar las propiedades de la cuenta de base de datos de Azure Cosmos DB. Las propiedades que se pueden actualizar incluyen lo siguiente:

* Agregar o quitar regiones
* Cambiar la directiva de coherencia predeterminada
* Cambio de la directiva de conmutación por error
* Cambiar el filtro de intervalo de IP
* Cambiar las configuraciones de red Virtual
* Habilitación de varios maestros

> [!NOTE]
> Este comando le permite agregar y quitar regiones, pero no le permite modificar las prioridades de conmutación por error. Para modificar las prioridades de conmutación por error, consulte [modificar la prioridad de conmutación por error para una cuenta de Azure Cosmos](#modify-failover-priority).

```azurepowershell-interactive
# Update an Azure Cosmos Account and set Consistency level to Session

$resourceGroupName = "myResourceGroup"
$accountName = "myaccountname"

$account = Get-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName -Name $accountName

$consistencyPolicy = @{ "defaultConsistencyLevel"="Session" }

$account.Properties.consistencyPolicy = $consistencyPolicy
$CosmosDBProperties = $account.Properties

Set-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $accountName -PropertyObject $CosmosDBProperties
```

### <a id="delete-account"></a> Eliminar una cuenta de Azure Cosmos

Este comando permite eliminar una cuenta existente de Azure Cosmos.

```azurepowershell-interactive
# Delete an Azure Cosmos Account
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"

Remove-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $accountName
```

### <a id="update-tags"></a> Actualizar las etiquetas de una cuenta de Azure Cosmos

El ejemplo siguiente describe cómo establecer [etiquetas de recursos de Azure] [ azure-resource-tags] para una cuenta de Azure Cosmos.

> [!NOTE]
> Este comando se puede combinar con los comandos de creación o actualización anexando la marca `-Tags` con el parámetro correspondiente.

```azurepowershell-interactive
# Update tags for an Azure Cosmos Account

$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"

$tags = @{
    "dept" = "Finance";
    "environment" = "Production"
}

Set-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $accountName -Tags $tags
```

### <a id="list-keys"></a> Enumerar claves de cuenta

Cuando se crea una cuenta de Azure Cosmos DB, el servicio genera dos claves de acceso maestras que se pueden usar para la autenticación cuando se tiene acceso a la cuenta de Azure Cosmos DB. Al proporcionar dos claves de acceso, Azure Cosmos DB permite regenerar las claves sin interrupción en la cuenta de Azure Cosmos DB. También están disponibles las claves de solo lectura para autenticar las operaciones de solo lectura. Hay dos claves de lectura y escritura (principal y secundaria) y dos claves de solo lectura (principal y secundaria).

```azurepowershell-interactive
# List keys for an Azure Cosmos Account

$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"

$keys = Invoke-AzResourceAction -Action listKeys `
    -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" `
    -ResourceGroupName $resourceGroupName -Name $accountName

Select-Object $keys
```

### <a id="list-connection-strings"></a> Enumeración de cadenas de conexión

En el caso de las cuentas de MongoDB, se puede recuperar la cadena de conexión para conectar la aplicación de MongoDB a la cuenta de la base de datos con el comando siguiente.

```azurepowershell-interactive
# List connection strings for an Azure Cosmos Account

$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"

$keys = Invoke-AzResourceAction -Action listConnectionStrings `
    -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" `
    -ResourceGroupName $resourceGroupName -Name $accountName

Select-Object $keys
```

### <a id="regenerate-keys"></a> Regenerar claves de cuenta

Se deben regenerar periódicamente las claves de acceso a una cuenta de Azure Cosmos para ayudar a proteger las conexiones. Una claves de acceso principal y secundaria se asignan a la cuenta. Esto permite a los clientes a mantener el acceso mientras se regenera la otra. Hay cuatro tipos de claves para una cuenta de Azure Cosmos (principal, secundaria, PrimaryReadonly y SecondaryReadonly)

```azurepowershell-interactive
# Regenerate the primary key for an Azure Cosmos Account

$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"

$keyKind = @{ "keyKind"="Primary" }

$keys = Invoke-AzResourceAction -Action regenerateKey `
    -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" `
    -ResourceGroupName $resourceGroupName -Name $accountName -Parameters $keyKind

Select-Object $keys
```

### <a id="modify-failover-priority"></a> Modificar la prioridad de conmutación por error

Para las cuentas de base de datos de varias regiones, puede cambiar la prioridad de conmutación por error de las distintas regiones que existe la cuenta de base de datos de Azure Cosmos DB. Para obtener más información sobre la conmutación por error en la cuenta de base de datos de Azure Cosmos DB, consulte [Distribución de datos global con Azure Cosmos DB][distribute-data-globally].

Para el ejemplo siguiente, se supone que la cuenta tiene una prioridad de conmutación por error actual de westus = 0 y eastus = 1. En este ejemplo a continuación va a invertir las regiones.

> [!CAUTION]
> Esta operación desencadenará una conmutación por error manual de su cuenta para la nueva región con un valor de failoverPriority de cero.

```azurepowershell-interactive
# Change the failover priority for an Azure Cosmos Account

$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"

$failoverPolicies = @(
    @{ "locationName"="East US"; "failoverPriority"=0 },
    @{ "locationName"="West US"; "failoverPriority"=1 }
)

Invoke-AzResourceAction -Action failoverPriorityChange `
    -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" `
    -ResourceGroupName $resourceGroupName -Name $accountName -Parameters $failoverPolicies
```

## <a name="azure-cosmos-database"></a>Azure Cosmos Database

Las siguientes secciones muestran cómo administrar la base de datos de Azure Cosmos, incluidos:

* [Crear una base de datos de Azure Cosmos](#create-db)
* [Crear una base de datos de Azure Cosmos con rendimiento compartido](#create-db-ru)
* [Enumerar todas las bases de datos de Azure Cosmos en una cuenta](#get-all-db)
* [Obtener una sola base de datos de Azure Cosmos](#get-db)
* [Eliminar una base de datos de Azure Cosmos](#delete-db)

### <a id="create-db"></a>Crear una base de datos de Azure Cosmos

```azurepowershell-interactive
# Create an Azure Cosmos database
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "database1"
$resourceName = $accountName + "/sql/" + $databaseName

$DataBaseProperties = @{
    "resource"=@{"id"=$databaseName}
} 
New-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts/apis/databases" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $resourceName -PropertyObject $DataBaseProperties
```

### <a id="create-db-ru"></a>Crear una base de datos de Azure Cosmos con rendimiento compartido

```azurepowershell-interactive
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "database2"
$resourceName = $accountName + "/sql/" + $databaseName

$DataBaseProperties = @{
    "resource"=@{ "id"=$databaseName };
    "options"=@{ "Throughput"="400" }
}

New-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts/apis/databases" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $resourceName -PropertyObject $DataBaseProperties
```

### <a id="get-all-db"></a>Obtener todas las bases de datos de Azure Cosmos en una cuenta

```azurepowershell-interactive
# Get all databases in an Azure Cosmos account
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$resourceName = $accountName + "/sql/"

Get-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts/apis/databases" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $resourceName  | Select-Object Properties
```

### <a id="get-db"></a>Obtener una sola base de datos de Azure Cosmos

```azurepowershell-interactive
# Get a single database in an Azure Cosmos account
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "database1"
$resourceName = $accountName + "/sql/" + $databaseName

Get-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts/apis/databases" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $resourceName | Select-Object Properties
```

### <a id="delete-db"></a>Eliminar una base de datos de Azure Cosmos

```azurepowershell-interactive
# Delete a database in an Azure Cosmos account
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "database1"
$resourceName = $accountName + "/sql/" + $databaseName
Remove-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts/apis/databases" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName -Name $resourceName
```

## <a name="azure-cosmos-container"></a>Azure Cosmos Container

Las secciones siguientes muestran cómo administrar el contenedor de Azure Cosmos, incluidos:

* [Crear un contenedor de Azure Cosmos](#create-container)
* [Crear un contenedor de Azure Cosmos con rendimiento compartido](#create-container-ru)
* [Crear un contenedor de Cosmos Azure con la indización personalizada](#create-container-custom-index)
* [Crear un contenedor de Azure Cosmos con desactiva la indexación](#create-container-no-index)
* [Crear un contenedor de Azure Cosmos con clave única y TTL](#create-container-unique-key-ttl)
* [Crear un contenedor de Cosmos Azure con la resolución de conflictos](#create-container-lww)
* [Mostrar todos los contenedores de Cosmos Azure en una base de datos](#list-all-container)
* [Obtener un único contenedor de Cosmos Azure en una base de datos](#get-container)
* [Eliminar un contenedor de Azure Cosmos](#delete-container)

### <a id="create-container"></a>Crear un contenedor de Azure Cosmos

```azurepowershell-interactive
# Create an Azure Cosmos container with default indexes and throughput at 400 RU 
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "database1"
$containerName = "container1"
$resourceName = $accountName + "/sql/" + $databaseName + "/" + $containerName

$ContainerProperties = @{
    "resource"=@{
        "id"=$containerName; 
        "partitionKey"=@{
            "paths"=@("/myPartitionKey"); 
            "kind"="Hash"
        }
    }; 
    "options"=@{ "Throughput"="400" }
} 
New-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts/apis/databases/containers" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $resourceName -PropertyObject $ContainerProperties
```

### <a id="create-container-ru"></a>Crear un contenedor de Azure Cosmos con rendimiento compartido

```azurepowershell-interactive
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "database1"
$containerName = "container2"
$resourceName = $accountName + "/sql/" + $databaseName + "/" + $containerName

$ContainerProperties = @{
    "resource"=@{
        "id"=$containerName; 
        "partitionKey"=@{
            "paths"=@("/myPartitionKey"); 
            "kind"="Hash"
        }
    }; 
    "options"=@{}
} 
New-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts/apis/databases/containers" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $resourceName -PropertyObject $ContainerProperties 
```

### <a id="create-container-custom-index"></a>Crear un contenedor de Cosmos Azure con la directiva de índice personalizado

```azurepowershell-interactive
# Create a container with a custom indexing policy
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "database1"
$containerName = "container3"
$resourceName = $accountName + "/sql/" + $databaseName + "/" + $containerName

$ContainerProperties = @{
    "resource"=@{
        "id"=$containerName; 
        "partitionKey"=@{
            "paths"=@("/myPartitionKey"); 
            "kind"="Hash"
        }; 
        "indexingPolicy"=@{
            "indexingMode"="Consistent"; 
            "includedPaths"= @(@{
                "path"="/*";
                "indexes"= @(@{
                        "kind"="Range";
                        "dataType"="number";
                        "precision"=-1
                    },
                    @{
                        "kind"="Range";
                        "dataType"="string";
                        "precision"=-1
                    }
                )
            });
            "excludedPaths"= @(@{
                "path"="/myPathToNotIndex/*"
            })
        }
    };
    "options"=@{ "Throughput"="400" }
} 

New-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts/apis/databases/containers" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $resourceName -PropertyObject $ContainerProperties
```

### <a id="create-container-no-index"></a>Crear un contenedor de Azure Cosmos con desactiva la indexación

```azurepowershell-interactive
# Create an Azure Cosmos container with no indexing 
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "database1"
$containerName = "container4"
$resourceName = $accountName + "/sql/" + $databaseName + "/" + $containerName

$ContainerProperties = @{
    "resource"=@{
        "id"=$containerName; 
        "partitionKey"=@{
            "paths"=@("/myPartitionKey"); 
            "kind"="Hash"
        }; 
        "indexingPolicy"=@{
            "indexingMode"="none"
        }
    };
    "options"=@{ "Throughput"="400" }
} 

New-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts/apis/databases/containers" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $resourceName -PropertyObject $ContainerProperties
```

### <a id="create-container-unique-key-ttl"></a>Crear un contenedor de Azure Cosmos con directiva de clave única y TTL

```azurepowershell-interactive
# Create a container with a unique key policy and TTL
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "database1"
$containerName = "container5"
$resourceName = $accountName + "/sql/" + $databaseName + "/" + $containerName

$ContainerProperties = @{
    "resource"=@{
        "id"=$containerName; 
        "partitionKey"=@{
            "paths"=@("/myPartitionKey"); 
            "kind"="Hash"
        }; 
        "indexingPolicy"=@{
            "indexingMode"="Consistent"; 
            "includedPaths"= @(@{
                "path"="/*";
                "indexes"= @(@{
                        "kind"="Range";
                        "dataType"="number";
                        "precision"=-1
                    },
                    @{
                        "kind"="Range";
                        "dataType"="string";
                        "precision"=-1
                    }
                )
            });
            "excludedPaths"= @()
        };
        "uniqueKeyPolicy"= @{
            "uniqueKeys"= @(@{
                "paths"= @(
                    "/myUniqueKey1";
                    "/myUniqueKey2"
                )
            })
        };
        "defaultTtl"= 100;
    }; 
    "options"=@{ "Throughput"="400" }
} 

New-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts/apis/databases/containers" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $resourceName -PropertyObject $ContainerProperties
```

### <a id="create-container-lww"></a>Crear un contenedor de Cosmos Azure con la resolución de conflictos

Para crear una directiva de resolución de conflictos para utilizar un procedimiento almacenado, establezca `"mode"="custom"` y establecer la ruta de acceso de resolución como el nombre del procedimiento almacenado, `"conflictResolutionPath"="myResolverStoredProcedure"`. Para todos los conflictos de escritura a la ConflictsFeed y controlar por separado, establezca `"mode"="custom"` y `"conflictResolutionPath"=""`

```azurepowershell-interactive
# Create container with last-writer-wins conflict resolution policy 
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "database1"
$containerName = "container6"
$resourceName = $accountName + "/sql/" + $databaseName + "/" + $containerName

$ContainerProperties = @{
    "resource"=@{
        "id"=$containerName;
        "partitionKey"=@{
            "paths"=@("/myPartitionKey"); 
            "kind"="Hash"
        }; 
        "conflictResolutionPolicy"=@{
            "mode"="lastWriterWins"; 
            "conflictResolutionPath"="/myResolutionPath"
        }
    }; 
    "options"=@{ "Throughput"="400" }
} 
New-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts/apis/databases/containers" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $resourceName -PropertyObject $ContainerProperties
```

### <a id="list-all-container"></a>Mostrar todos los contenedores de Cosmos Azure en una base de datos

```azurepowershell-interactive
# List all Azure Cosmos containers in a database 
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "database1"
$resourceName = $accountName + "/sql/" + $databaseName

Get-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts/apis/databases/containers" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $resourceName | Select-Object Properties
```

### <a id="get-container"></a>Obtener un único contenedor de Cosmos Azure en una base de datos

```azurepowershell-interactive
# Get a single Azure Cosmos container in a database
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "database1"
$containerName = "container5"
$resourceName = $accountName + "/sql/" + $databaseName + "/" + $containerName

Get-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts/apis/databases/containers" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $resourceName | Select-Object Properties
```

### <a id="delete-container"></a>Eliminar un contenedor de Azure Cosmos

```azurepowershell-interactive
# Delete an Azure Cosmos container
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "database1"
$containerName = "container1"
$resourceName = $accountName + "/sql/" + $databaseName + "/" + $containerName

Remove-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts/apis/databases/containers" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName -Name $resourceName
```

## <a name="next-steps"></a>Pasos siguientes

* [Todos los ejemplos de PowerShell](powershell-samples.md)
* [Cómo administrar la cuenta de Azure Cosmos](how-to-manage-database-account.md)
* [Crear un contenedor de Azure Cosmos](how-to-create-container.md)
* [Configurar el período de vida en Azure Cosmos DB](how-to-time-to-live.md)

<!--Reference style links - using these makes the source content way more readable than using inline links-->

[powershell-install-configure]: https://docs.microsoft.com/azure/powershell-install-configure
[scaling-globally]: distribute-data-globally.md#EnableGlobalDistribution
[distribute-data-globally]: distribute-data-globally.md
[azure-resource-groups]: https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups
[azure-resource-tags]: https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags
[rp-rest-api]: https://docs.microsoft.com/rest/api/cosmos-db-resource-provider/