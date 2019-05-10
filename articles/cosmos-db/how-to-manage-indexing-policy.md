---
title: Administración de directivas de indexación en Azure Cosmos DB
description: Aprenda a administrar directivas de indexación en Azure Cosmos DB
author: ThomasWeiss
ms.service: cosmos-db
ms.topic: sample
ms.date: 05/06/2019
ms.author: thweiss
ms.openlocfilehash: 48d67c765a8a76a6058592f59eb61770e2f23df5
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65068671"
---
# <a name="manage-indexing-policies-in-azure-cosmos-db"></a>Administración de directivas de indexación en Azure Cosmos DB

En Azure Cosmos DB, para indexar los datos se usan las [directivas de indexación](index-policy.md) que se definen para cada contenedor. La directiva de indexación predeterminada para los contenedores recién creados exige que se usen índices de intervalo para todas las cadenas o números, e índices espaciales para todos los objetos GeoJSON del tipo Point. Dicha directiva se puede invalidar:

- desde Azure Portal
- mediante la CLI de Azure
- mediante uno de los SDK

Las [actualizaciones de las directivas de indexación](index-policy.md#modifying-the-indexing-policy) desencadenan transformaciones de índices. También se puede realizar un seguimiento del progreso de esta transformación desde los SDK.

## <a name="use-the-azure-portal"></a>Uso de Azure Portal

Los contenedores de Azure Cosmos almacenan su directiva de indexación en forma de documento JSON que Azure Portal permite editar directamente.

1. Inicie sesión en el [Azure Portal](https://portal.azure.com/).

1. Cree una cuenta de Azure Cosmos DB o seleccione una ya existente.

1. Abra el panel **Data Explorer** y seleccione el contenedor en el que quiere trabajar.

1. Haga clic en **Scale & Settings** (Escala y configuración).

1. Modifique el documento JSON de la directiva de indexación (vea los ejemplos [a continuación](#indexing-policy-examples))

1. Cuando haya terminado, haga clic en **Guardar**.

![Administración de la indexación mediante Azure Portal](./media/how-to-manage-indexing-policy/indexing-policy-portal.png)

## <a name="use-the-azure-cli"></a>Uso de la CLI de Azure

El comando [az cosmosdb collection update](/cli/azure/cosmosdb/collection#az-cosmosdb-collection-update) de la CLI de Azure le permite reemplazar la definición de JSON de una directiva de indexación de un contenedor:

```azurecli-interactive
az cosmosdb collection update \
    --resource-group-name $resourceGroupName \
    --name $accountName \
    --db-name $databaseName \
    --collection-name $containerName \
    --indexing-policy "{\"indexingMode\": \"consistent\", \"includedPaths\": [{ \"path\": \"/*\", \"indexes\": [{ \"dataType\": \"String\", \"kind\": \"Range\" }] }], \"excludedPaths\": [{ \"path\": \"/headquarters/employees/?\" } ]}"
```

## <a name="use-the-net-sdk-v2"></a>Uso de .NET SDK V2

El objeto `DocumentCollection` del [SDK de .NET v2](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/) (consulte [este inicio rápido](create-sql-api-dotnet.md) para conocer su uso) expone una propiedad `IndexingPolicy` que permite cambiar `IndexingMode` y agregar o quitar `IncludedPaths` y `ExcludedPaths`.

```csharp
// retrieve the container's details
ResourceResponse<DocumentCollection> containerResponse = await client.ReadDocumentCollectionAsync(UriFactory.CreateDocumentCollectionUri("database", "container"));
// set the indexing mode to Consistent
containerResponse.Resource.IndexingPolicy.IndexingMode = IndexingMode.Consistent;
// add an excluded path
containerResponse.Resource.IndexingPolicy.ExcludedPaths.Add(new ExcludedPath { Path = "/headquarters/employees/?" });
// update the container with our changes
await client.ReplaceDocumentCollectionAsync(containerResponse.Resource);
```

Para realizar un seguimiento del progreso de transformación del índice, utilice un objeto `RequestOptions` que establezca la propiedad `PopulateQuotaInfo` en `true`.

```csharp
// retrieve the container's details
ResourceResponse<DocumentCollection> container = await client.ReadDocumentCollectionAsync(UriFactory.CreateDocumentCollectionUri("database", "container"), new RequestOptions { PopulateQuotaInfo = true });
// retrieve the index transformation progress from the result
long indexTransformationProgress = container.IndexTransformationProgress;
```

## <a name="use-the-java-sdk"></a>Uso del SDK de Java

El objeto `DocumentCollection` del [SDK de Java](https://mvnrepository.com/artifact/com.microsoft.azure/azure-cosmosdb) (para aprender a usarlo, vea [este inicio rápido](create-sql-api-java.md)) expone los métodos `getIndexingPolicy()` y `setIndexingPolicy()`. El objeto `IndexingPolicy` que manipulan permite cambiar el modo de indexación y agregar o quitar las rutas de acceso incluidas y excluidas.

```java
// retrieve the container's details
Observable<ResourceResponse<DocumentCollection>> containerResponse = client.readCollection(String.format("/dbs/%s/colls/%s", "database", "container"), null);
containerResponse.subscribe(result -> {
    DocumentCollection container = result.getResource();
    IndexingPolicy indexingPolicy = container.getIndexingPolicy();
    // set the indexing mode to Consistent
    indexingPolicy.setIndexingMode(IndexingMode.Consistent);
    Collection<ExcludedPath> excludedPaths = new ArrayList<>();
    ExcludedPath excludedPath = new ExcludedPath();
    excludedPath.setPath("/*");
    // add an excluded path
    excludedPaths.add(excludedPath);
    indexingPolicy.setExcludedPaths(excludedPaths);
    // update the container with our changes
    client.replaceCollection(container, null);
});
```

Para realizar un seguimiento del progreso de transformación del índice en un contenedor, utilice un objeto `RequestOptions` que solicite la información de cuota que se debe rellenar y, después, recupere el valor del encabezado de respuesta `x-ms-documentdb-collection-index-transformation-progress`.

```java
// set the RequestOptions object
RequestOptions requestOptions = new RequestOptions();
requestOptions.setPopulateQuotaInfo(true);
// retrieve the container's details
Observable<ResourceResponse<DocumentCollection>> containerResponse = client.readCollection(String.format("/dbs/%s/colls/%s", "database", "container"), requestOptions);
containerResponse.subscribe(result -> {
    // retrieve the index transformation progress from the response headers
    String indexTransformationProgress = result.getResponseHeaders().get("x-ms-documentdb-collection-index-transformation-progress");
});
```

## <a name="use-the-nodejs-sdk"></a>Uso del SDK de Node.js

La interfaz `ContainerDefinition` del [SDK de Node.js](https://www.npmjs.com/package/@azure/cosmos) (consulte [este inicio rápido](create-sql-api-nodejs.md) para conocer su uso) expone una propiedad `indexingPolicy` que permite cambiar `indexingMode` y agregar o eliminar `includedPaths` y `excludedPaths`.

```javascript
// retrieve the container's details
const containerResponse = await client.database('database').container('container').read();
// set the indexing mode to Consistent
containerResponse.body.indexingPolicy.indexingMode = "consistent";
// add an excluded path
containerResponse.body.indexingPolicy.excludedPaths.push({ path: '/headquarters/employees/?' });
// update the container with our changes
const replaceResponse = await client.database('database').container('container').replace(containerResponse.body);
```

Para realizar un seguimiento del progreso de transformación del índice en un contenedor, utilice un objeto `RequestOptions` que establezca la propiedad `populateQuotaInfo` en `true` y después recupere el valor del encabezado de respuesta `x-ms-documentdb-collection-index-transformation-progress`.

```javascript
// retrieve the container's details
const containerResponse = await client.database('database').container('container').read({
    populateQuotaInfo: true
});
// retrieve the index transformation progress from the response headers
const indexTransformationProgress = replaceResponse.headers['x-ms-documentdb-collection-index-transformation-progress'];
```

## <a name="use-the-python-sdk"></a>Uso del SDK de Python

Cuando se usa el [SDK de Python](https://pypi.org/project/azure-cosmos/) (consulte [este inicio rápido](create-sql-api-python.md) para saber cómo se usa), la configuración del contenedor se administra como un diccionario. Desde este diccionario es posible acceder a la directiva de indexación y a todos sus atributos.

```python
containerPath = 'dbs/database/colls/collection'
# retrieve the container's details
container = client.ReadContainer(containerPath)
# set the indexing mode to Consistent
container['indexingPolicy']['indexingMode'] = 'consistent'
# add an excluded path
container['indexingPolicy']['excludedPaths'] = [{"path" : "/headquarters/employees/?"}]
# update the container with our changes
response = client.ReplaceContainer(containerPath, container)
```

## <a name="indexing-policy-examples"></a>Ejemplos de directiva de indexación

Estos son algunos ejemplos de directivas de indexación que se muestran en su formato JSON, que es cómo se exponen en Azure Portal. Los mismos parámetros se pueden establecer a través de la CLI de Azure o cualquier SDK.

### <a name="opt-out-policy-to-selectively-exclude-some-property-paths"></a>Directiva de rechazo que excluye de forma selectiva algunas rutas de acceso de propiedades
```
    {
        "indexingPolicy": "consistent",
        "includedPaths": [
            {
                "path": "/*",
                "indexes": [
                    {
                        "kind": "Range",
                        "dataType": "Number"
                    },
                    {
                        "kind": "Range",
                        "dataType": "String"
                    },
                    {
                        "kind": "Spatial",
                        "dataType": "Point"
                    }
                ]
            }
        ],
        "excludedPaths": [
            {
                "path": "/path/to/single/excluded/property/?"
            },
            {
                "path": "/path/to/root/of/multiple/excluded/properties/*"
            }
        ]
    }
```

### <a name="opt-in-policy-to-selectively-include-some-property-paths"></a>Directiva de participación que incluye de forma selectiva algunas rutas de acceso de propiedades
```
    {
        "indexingPolicy": "consistent",
        "includedPaths": [
            {
                "path": "/path/to/included/property/?",
                "indexes": [
                    {
                        "kind": "Range",
                        "dataType": "Number"
                    }
                ]
            },
            {
                "path": "/path/to/root/of/multiple/included/properties/*",
                "indexes": [
                    {
                        "kind": "Range",
                        "dataType": "Number"
                    }
                ]
            }
        ],
        "excludedPaths": [
            {
                "path": "/*"
            }
        ]
    }
```

Nota: Por lo general se recomienda usar una directiva de indexación de **rechazo** para permitir que Azure Cosmos DB indexe de forma proactiva todas las propiedades nuevas que se puedan agregar a un modelo.

### <a name="using-a-spatial-index-on-a-specific-property-path-only"></a>Uso de un índice espacial en una ruta de acceso de una propiedad concreta solo
```
    {
        "indexingPolicy": "consistent",
        "includedPaths": [
            {
                "path": "/*",
                "indexes": [
                    {
                        "kind": "Range",
                        "dataType": "Number"
                    },
                    {
                        "kind": "Range",
                        "dataType": "String"
                    }
                ]
            },
            {
                "path": "/path/to/geojson/property/?",
                "indexes": [
                    {
                        "kind": "Spatial",
                        "dataType": "Point"
                    }
                ]
            }
        ],
        "excludedPaths": []
    }
```

### <a name="excluding-all-property-paths-but-keeping-indexing-active"></a>Exclusión de las rutas de acceso de todas las propiedades, pero manteniendo la indexación activa

Esta directiva se puede usar en las situaciones en que la [característica Time-to-Live (TTL)](time-to-live.md) está activa, pero no se requiere un índice secundario (para usar Azure Cosmos DB como almacén de clave-valor puro).
```
    {
        "indexingMode": "consistent",
        "includedPaths": [],
        "excludedPaths": [{
            "path": "/*"
        }]
    }
```

### <a name="no-indexing"></a>Sin indexación
```
    {
        "indexingPolicy": "none"
    }
```

## <a name="composite-indexing-policy-examples"></a>Ejemplos de la directiva de índice compuesto

Además de incluir o excluir rutas de acceso para las propiedades individuales, también puede especificar un índice compuesto. Si quiere hacer una consulta que tiene una cláusula `ORDER BY` para varias propiedades, necesita un [índice compuesto](index-policy.md#composite-indexes) en esas propiedades.

### <a name="composite-index-defined-for-name-asc-age-desc"></a>Índice compuesto definido para (name asc, age desc):
```
    {  
        "automatic":true,
        "indexingMode":"Consistent",
        "includedPaths":[  
            {  
                "path":"/*"
            }
        ],
        "excludedPaths":[  

        ],
        "compositeIndexes":[  
            [  
                {  
                    "path":"/name",
                    "order":"ascending"
                },
                {  
                    "path":"/age",
                    "order":"descending"
                }
            ]
        ]
    }
```

Este índice compuesto sería capaz de admitir estas dos consultas:

Consulta 1:
```sql
    SELECT *
    FROM c
    ORDER BY name asc, age desc    
```

Consulta 2:
```sql
    SELECT *
    FROM c
    ORDER BY name desc, age asc
```

### <a name="composite-index-defined-for-name-asc-age-asc-and-name-asc-age-desc"></a>Índice compuesto definido para (name asc, age asc) y (name asc, age desc):

Puede definir varios índices compuestos distintos dentro de la misma directiva de índice. 
```
    {  
        "automatic":true,
        "indexingMode":"Consistent",
        "includedPaths":[  
            {  
                "path":"/*"
            }
        ],
        "excludedPaths":[  

        ],
        "compositeIndexes":[  
            [  
                {  
                    "path":"/name",
                    "order":"ascending"
                },
                {  
                    "path":"/age",
                    "order":"ascending"
                }
            ]
            [  
                {  
                    "path":"/name",
                    "order":"ascending"
                },
                {  
                    "path":"/age",
                    "order":"descending"
                }
            ]
        ]
    }
```

### <a name="composite-index-defined-for-name-asc-age-asc"></a>Índice compuesto definido para (name asc, age asc):

Si quiere, puede especificar el orden. Si no lo especifica, el orden es ascendente.
```
{  
        "automatic":true,
        "indexingMode":"Consistent",
        "includedPaths":[  
            {  
                "path":"/*"
            }
        ],
        "excludedPaths":[  

        ],
        "compositeIndexes":[  
            [  
                {  
                    "path":"/name",
                },
                {  
                    "path":"/age",
                }
            ]
        ]
}
```

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información acerca de la indexación en los siguientes artículos:

- [Introducción a la indexación](index-overview.md)
- [Directiva de indexación](index-policy.md)
