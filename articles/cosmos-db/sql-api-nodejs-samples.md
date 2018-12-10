---
title: Ejemplos de Node.js para Azure Cosmos DB
description: Busque ejemplos de Node.js en GitHub para tareas comunes en Azure Cosmos DB, incluidas las operaciones CRUD.
services: cosmos-db
author: deborahc
ms.service: cosmos-db
ms.component: cosmosdb-sql
ms.topic: sample
ms.date: 09/24/2018
ms.author: deborahc
ms.openlocfilehash: 21ef8c5464f15c5eca939b103ac67f0f3984d8ea
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/04/2018
ms.locfileid: "52878653"
---
# <a name="azure-cosmos-db-nodejs-examples"></a>Ejemplos de Node.js de Azure Cosmos DB
> [!div class="op_single_selector"]
> * [Ejemplos de .NET](sql-api-dotnet-samples.md)
> * [Ejemplos de Java](sql-api-java-samples.md)
> * [Ejemplos de Java asincrónico](sql-api-async-java-samples.md)
> * [Ejemplos de Node.js](sql-api-nodejs-samples.md)
> * [Ejemplos de Python](sql-api-python-samples.md)
> * [Galería de ejemplos de código de Azure](https://azure.microsoft.com/resources/samples/?sort=0&service=cosmos-db)
> 
> 

En el repositorio de GitHub [azure-cosmos-js](https://github.com/Azure/azure-cosmos-js/tree/master/samples) se incluyen soluciones de ejemplo que realizan operaciones CRUD y otras operaciones comunes en recursos de Azure Cosmos DB. Este artículo ofrece:

* Vínculos a las tareas de cada uno de los archivos de proyecto de ejemplo de Node.js.
* Vínculos al contenido de referencia de la API relacionada.

**Requisitos previos**

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

- Puede [activar los beneficios de suscriptor de Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio): su suscripción a Visual Studio le proporciona crédito todos los meses que puede usar con servicios de Azure de pago.

[!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

También necesita el [SDK de JavaScript](sql-api-sdk-node.md).
   
   > [!NOTE]
   > Cada ejemplo es independiente, es decir, se configura a sí mismo y posteriormente se limpia solo. Por tanto, los ejemplos emiten varias llamadas a [Containers.create](https://docs.microsoft.com/javascript/api/%40azure/cosmos/containers?view=azure-node-latest#create). Cada vez que esto ocurre, se cobra a la suscripción una hora de uso de acuerdo con el nivel de rendimiento del contenedor que se va a crear.
   > 
   > 

## <a name="database-examples"></a>Ejemplos de base de datos
El archivo [app.js](https://github.com/Azure/azure-cosmos-js/blob/master/samples/DatabaseManagement/app.js) del proyecto [DatabaseManagement](https://github.com/Azure/azure-cosmos-js/tree/master/samples/DatabaseManagement) muestra cómo realizar las tareas siguientes.

| Task | Referencia de API |
| --- | --- |
| [Creación de una base de datos si no hay una](https://github.com/Azure/azure-cosmos-js/blob/216672a679ab389e5b341280eeacab1cab3691e4/samples/DatabaseManagement/app.js#L35-L37) |[Databases.createIfNotExists](https://docs.microsoft.com/javascript/api/%40azure/cosmos/databases?view=azure-node-latest#createifnotexists) |
| [Enumeración de las bases de datos de una cuenta](https://github.com/Azure/azure-cosmos-js/blob/216672a679ab389e5b341280eeacab1cab3691e4/samples/DatabaseManagement/app.js#L40-L42) |[Databases.readAll](https://docs.microsoft.com/javascript/api/%40azure/cosmos/databases?view=azure-node-latest#readall) |
| [Lectura de una base de datos por identificador](https://github.com/Azure/azure-cosmos-js/blob/216672a679ab389e5b341280eeacab1cab3691e4/samples/DatabaseManagement/app.js#L40-L42) |[Database.read](https://docs.microsoft.com/javascript/api/%40azure/cosmos/database?view=azure-node-latest#read) |
| [Eliminación de una base de datos](https://github.com/Azure/azure-cosmos-js/blob/216672a679ab389e5b341280eeacab1cab3691e4/samples/DatabaseManagement/app.js#L57-L60) |[Database.delete](https://docs.microsoft.com/javascript/api/%40azure/cosmos/database?view=azure-node-latest#delete) |

## <a name="container-examples"></a>Ejemplos de contenedor
El archivo [app.js](https://github.com/Azure/azure-cosmos-js/blob/master/samples/ContainerManagement/app.js) del proyecto [ContainerManagement](https://github.com/Azure/azure-cosmos-js/tree/master/samples/ItemManagement) muestra cómo realizar las tareas siguientes.

| Task | Referencia de API |
| --- | --- |
| [Creación de un contenedor si no hay uno](https://github.com/Azure/azure-cosmos-js/blob/216672a679ab389e5b341280eeacab1cab3691e4/samples/ContainerManagement/app.js#L36-L37) |[Containers.createIfNotExists](https://docs.microsoft.com/javascript/api/%40azure/cosmos/containers?view=azure-node-latest#createifnotexists) |
| [Enumeración de contenedores de una cuenta](https://github.com/Azure/azure-cosmos-js/blob/216672a679ab389e5b341280eeacab1cab3691e4/samples/ContainerManagement/app.js#L36-L37) |[Containers.readAll](https://docs.microsoft.com/javascript/api/%40azure/cosmos/containers?view=azure-node-latest#readall) |
| [Lectura de una colección por identificador](https://github.com/Azure/azure-cosmos-js/blob/216672a679ab389e5b341280eeacab1cab3691e4/samples/ContainerManagement/app.js#L47-L51) |[Container.read](https://docs.microsoft.com/javascript/api/%40azure/cosmos/container?view=azure-node-latest#read) |
| [Eliminación de un contenedor](https://github.com/Azure/azure-cosmos-js/blob/216672a679ab389e5b341280eeacab1cab3691e4/samples/ContainerManagement/app.js#L54-L55) |[Container.delete](https://docs.microsoft.com/javascript/api/%40azure/cosmos/container?view=azure-node-latest#delete) |

## <a name="item-examples"></a>Ejemplos de elementos
El archivo [app.js](https://github.com/Azure/azure-cosmos-js/blob/master/samples/ItemManagement/app.js) del proyecto [ItemManagement](https://github.com/Azure/azure-cosmos-js/tree/master/samples/ItemManagement) muestra cómo realizar las tareas siguientes.

| Task | Referencia de API |
| --- | --- |
| [Creación de elementos](https://github.com/Azure/azure-cosmos-js/blob/216672a679ab389e5b341280eeacab1cab3691e4/samples/ItemManagement/app.js#L49-L56) |[Items.create](https://docs.microsoft.com/javascript/api/%40azure/cosmos/items?view=azure-node-latest#create) |
| [Lectura de todos los elementos de un contenedor](https://github.com/Azure/azure-cosmos-js/blob/216672a679ab389e5b341280eeacab1cab3691e4/samples/ItemManagement/app.js#L59-L64) |[Items.readAll](https://docs.microsoft.com/javascript/api/%40azure/cosmos/items?view=azure-node-latest#readall) |
| [Lectura de un elemento por identificador](https://github.com/Azure/azure-cosmos-js/blob/216672a679ab389e5b341280eeacab1cab3691e4/samples/ItemManagement/app.js#L59-L64) |[Item.read](https://docs.microsoft.com/javascript/api/%40azure/cosmos/item?view=azure-node-latest#read) |
| [Lectura de un elemento solo si el elemento ha cambiado](https://github.com/Azure/azure-cosmos-js/blob/216672a679ab389e5b341280eeacab1cab3691e4/samples/ItemManagement/app.js#L73-L94) |[Item.read](https://docs.microsoft.com/javascript/api/%40azure/cosmos/item?view=azure-node-latest#read)<br/>[RequestOptions.accessCondition](https://docs.microsoft.com/javascript/api/%40azure/cosmos/requestoptions?view=azure-node-latest#accesscondition) |
| [Consulta de documentos](https://github.com/Azure/azure-cosmos-js/blob/216672a679ab389e5b341280eeacab1cab3691e4/samples/ItemManagement/app.js#L97-L118) |[Items.query](https://docs.microsoft.com/javascript/api/%40azure/cosmos/items?view=azure-node-latest#query) |
| [Reemplazo de un elemento](https://github.com/Azure/azure-cosmos-js/blob/216672a679ab389e5b341280eeacab1cab3691e4/samples/ItemManagement/app.js#L131-L136) |[Item.replace](https://docs.microsoft.com/javascript/api/%40azure/cosmos/item?view=azure-node-latest#replace) |
| [Reemplazo de un artículo con la comprobación de ETag condicional](https://github.com/Azure/azure-cosmos-js/blob/216672a679ab389e5b341280eeacab1cab3691e4/samples/ItemManagement/app.js#L139-L160) |[Item.replace](https://docs.microsoft.com/javascript/api/%40azure/cosmos/item?view=azure-node-latest#replace)<br/>[RequestOptions.accessCondition](https://docs.microsoft.com/javascript/api/%40azure/cosmos/requestoptions?view=azure-node-latest#accesscondition) |
| [Eliminación de un elemento](https://github.com/Azure/azure-cosmos-js/blob/216672a679ab389e5b341280eeacab1cab3691e4/samples/ItemManagement/app.js#L162-L164) |[Item.delete](https://docs.microsoft.com/javascript/api/%40azure/cosmos/item?view=azure-node-latest#delete) |

## <a name="indexing-examples"></a>Ejemplos de indización
El archivo [app.js](https://github.com/Azure/azure-cosmos-js/blob/master/samples/IndexManagement/app.js) del proyecto [IndexManagement](https://github.com/Azure/azure-cosmos-js/tree/master/samples/IndexManagement) muestra cómo realizar las tareas siguientes.

| Task | Referencia de API |
| --- | --- |
| [Indexar manualmente un documento determinado](https://github.com/Azure/azure-cosmos-js/blob/216672a679ab389e5b341280eeacab1cab3691e4/samples/IndexManagement/app.js#L135-L177) |[RequestOptions.indexingDirective: 'include'](https://docs.microsoft.com/javascript/api/%40azure/cosmos/requestoptions?view=azure-node-latest#indexingdirective) |
| [Excluir manualmente un documento determinado del índice](https://github.com/Azure/azure-cosmos-js/blob/216672a679ab389e5b341280eeacab1cab3691e4/samples/IndexManagement/app.js#L90-L131) |[RequestOptions.indexingDirective: 'exclude'](https://docs.microsoft.com/javascript/api/%40azure/cosmos/requestoptions?view=azure-node-latest#indexingdirective) |
| [Uso de la indexación diferida para la importación en bloque o la lectura de contenedores pesados](https://github.com/Azure/azure-cosmos-js/blob/216672a679ab389e5b341280eeacab1cab3691e4/samples/IndexManagement/app.js#L183-L214) |[IndexingMode.Lazy](https://docs.microsoft.com/javascript/api/%40azure/cosmos/indexingmode?view=azure-node-latest) |
| [Exclusión de una ruta de acceso del índice](https://github.com/Azure/azure-cosmos-js/blob/216672a679ab389e5b341280eeacab1cab3691e4/samples/IndexManagement/app.js#L352-L429) |[IndexingPolicy.ExcludedPath](https://docs.microsoft.com/javascript/api/%40azure/cosmos/indexingpolicy?view=azure-node-latest#excludedpaths) |
| [Permitir un análisis en una ruta de acceso de cadena durante una operación de intervalo](https://github.com/Azure/azure-cosmos-js/blob/216672a679ab389e5b341280eeacab1cab3691e4/samples/IndexManagement/app.js#L219-L275) |[FeedOptions.EnableScanInQuery](https://docs.microsoft.com/javascript/api/%40azure/cosmos/feedoptions?view=azure-node-latest#enablescaninquery) |
| [Crear un índice de intervalo en una ruta de acceso de cadena](https://github.com/Azure/azure-cosmos-js/blob/216672a679ab389e5b341280eeacab1cab3691e4/samples/IndexManagement/app.js#L281-L346) |[IndexKind.Range](https://docs.microsoft.com/javascript/api/%40azure/cosmos/indexkind?view=azure-node-latest), [IndexingPolicy](https://docs.microsoft.com/javascript/api/%40azure/cosmos/indexingpolicy?view=azure-node-latest), [Items.query](https://docs.microsoft.com/javascript/api/%40azure/cosmos/items?view=azure-node-latest#query) |
| [Creación de un contenedor con indexPolicy de forma predeterminada y posterior actualización en línea](https://github.com/Azure/azure-cosmos-js/blob/216672a679ab389e5b341280eeacab1cab3691e4/samples/IndexManagement/app.js#L435-L507) |[Containers.create](https://docs.microsoft.com/javascript/api/%40azure/cosmos/containers?view=azure-node-latest#create)


Para más información sobre la indexación, consulte [Directivas de indexación de Azure Cosmos DB](index-policy.md).

## <a name="server-side-programming-examples"></a>Ejemplos de programación en el servidor
El archivo [app.js](https://github.com/Azure/azure-cosmos-js/blob/master/samples/ServerSideScripts/app.js) del proyecto [ServerSideScripts](https://github.com/Azure/azure-cosmos-js/tree/master/samples/ServerSideScripts) muestra cómo realizar las tareas siguientes.

| Task | Referencia de API |
| --- | --- |
| [Crear un procedimiento almacenado](https://github.com/Azure/azure-cosmos-js/blob/216672a679ab389e5b341280eeacab1cab3691e4/samples/ServerSideScripts/JS/upsert.js#L12-L72) |[StoredProcedures.create](https://docs.microsoft.com/javascript/api/%40azure/cosmos/storedprocedures?view=azure-node-latest) |
| [Ejecutar un procedimiento almacenado](https://github.com/Azure/azure-cosmos-js/blob/216672a679ab389e5b341280eeacab1cab3691e4/samples/ServerSideScripts/app.js#L44) |[StoredProcedure.execute](https://docs.microsoft.com/javascript/api/%40azure/cosmos/storedprocedure?view=azure-node-latest#execute) |

Para más información sobre la programación en el servidor, consulte [Programación en el servidor de Azure Cosmos DB: procedimientos almacenados, desencadenadores de base de datos y funciones definidas por el usuario](programming.md).

