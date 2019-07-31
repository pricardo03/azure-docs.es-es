---
title: Creación de contenedores de Azure Cosmos con una clave de partición de gran tamaño mediante Azure Portal y varios SDK.
description: Aprenda a crear un contenedor en Azure Cosmos DB con una clave de partición de gran tamaño mediante Azure Portal y distintos SDK.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 07/03/2019
ms.author: mjbrown
ms.openlocfilehash: bd1697378e5db0432d181f9f688ccc2468b306e7
ms.sourcegitcommit: d2785f020e134c3680ca1c8500aa2c0211aa1e24
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/04/2019
ms.locfileid: "67566018"
---
# <a name="create-containers-with-large-partition-key"></a>Creación de contenedores con una clave de partición de gran tamaño

Azure Cosmos DB usa el esquema de partición basado en hash para lograr el escalado horizontal de los datos. Todos los contenedores de Azure Cosmos creados antes del 3 de ayo de 2019 usan una función hash que calcula el hash en función de los primeros 100 bytes de la clave de partición. Si hay varias claves de partición con los mismos 100 primeros bytes, el servicio considera que esas particiones lógicas son la misma partición lógica. Esto puede causar problemas, como que la cuota del tamaño de la partición no sea correcta y que se apliquen índices únicos en las distintas claves de partición. Para solucionar este problema, aparecieron las claves de partición de gran tamaño. Azure Cosmos DB ahora admite claves de partición de gran tamaño con valores hasta 2 KB.

Las claves de partición de gran tamaño se admiten mediante el uso de la funcionalidad de una versión mejorada de la función hash, la que puede generar un hash único a partir de claves de partición de gran tamaño de hasta 2 KB. Esta versión de hash también se recomienda para los escenarios con una cardinalidad de clave de partición alta, independientemente del tamaño de la clave de partición. Una cardinalidad de clave de partición se define como el número de particiones lógicas únicas, por ejemplo en el orden de unas 30 000 particiones lógicas en un contenedor. En este artículo se describe cómo crear un contenedor con una clave de partición de gran tamaño mediante Azure Portal y distintos SDK. 

## <a name="create-a-large-partition-key-net-sdk-v2"></a>Creación de una clave de partición de gran tamaño (SDK de .NET v2)

Para crear un contenedor con una clave de partición de gran tamaño mediante el SDK de .NET, especifique la propiedad `PartitionKeyDefinitionVersion.V2`. En el ejemplo siguiente se muestra cómo especificar la propiedad de versión dentro del objeto PartitionKeyDefinition y establecerla en PartitionKeyDefinitionVersion.V2.

```csharp
DocumentCollection collection = await newClient.CreateDocumentCollectionAsync(
database,
     new DocumentCollection
        {
           Id = Guid.NewGuid().ToString(),
           PartitionKey = new PartitionKeyDefinition
           {
             Paths = new Collection<string> {"/longpartitionkey" },
             Version = PartitionKeyDefinitionVersion.V2
           }
         },
      new RequestOptions { OfferThroughput = 400 });
```

## <a name="create-a-large-partition-key-azure-portal"></a>Creación de una clave de partición de gran tamaño (Azure Portal) 

Para crear una clave de partición de gran tamaño, mientras crea un contenedor con Azure Portal, active la opción **My partition key is larger than 100-bytes** (Mi clave de partición tiene más de 100 bytes). De manera predeterminada, todos los contenedores nuevos usan las claves de partición de gran tamaño. Desactive la casilla si no necesita claves de partición de gran tamaño o si tiene aplicaciones que ejecutan una versión de SDK anterior a 1.18.

![Creación de claves de partición de gran tamaño con Azure Portal](./media/large-partition-keys/large-partition-key-with-portal.png)

## <a name="create-a-large-partition-key-powershell"></a>Creación de una clave de partición de gran tamaño (PowerShell)

Para crear un contenedor con una clave de partición grande mediante PowerShell, incluya `"version" = 2` en el objeto `partitionKey`.

```azurepowershell-interactive
# Create a Cosmos SQL API container with large partition key support (version 2)
$resourceGroupName = "myResourceGroup"
$containerName = "mycosmosaccount" + "/sql/" + "myDatabase" + "/" + "myContainer"

# Container with large partition key support (version = 2)
$containerProperties = @{
  "resource"=@{
    "id"=$containerName;
    "partitionKey"=@{
        "paths"=@("/myPartitionKey");
        "kind"="Hash";
        "version" = 2
    };
    "indexingPolicy"=@{
        "indexingMode"="Consistent";
        "includedPaths"= @(@{
            "path"="/*"
        });
        "excludedPaths"= @(@{
            "path"="/myPathToNotIndex/*"
        })
    }
  }
}

New-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts/apis/databases/containers" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $containerName -PropertyObject $containerProperties
```

## <a name="supported-sdk-versions"></a>Versiones de SDK compatibles

Las claves de partición de gran tamaño se admiten con las siguientes versiones mínimas de SDK:

|Tipo de SDK  | Versión mínima   |
|---------|---------|
|.Net     |    1.18     |
|Java Sync     |   2.4.0      |
|Java Async   |  2.5.0        |
| API DE REST | Una versión superior a `2017-05-03` mediante el uso del encabezado de solicitud`x-ms-version`.|

Actualmente, no se pueden usar contenedores con una clave de partición de gran tamaño dentro de Power BI ni de Azure Logic Apps. Puede usar contenedores sin una clave de partición de gran tamaño desde estas aplicaciones.

## <a name="next-steps"></a>Pasos siguientes

* [Creación de particiones en Azure Cosmos DB](partitioning-overview.md)
* [Unidades de solicitud en Azure Cosmos DB](request-units.md)
* [Aprovisionar rendimiento en contenedores y bases de datos](set-throughput.md)
* [Uso de la cuenta de Azure Cosmos](account-overview.md)


