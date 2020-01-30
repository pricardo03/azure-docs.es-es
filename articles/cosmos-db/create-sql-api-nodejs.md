---
title: 'Inicio rápido: Uso de Node.js para consultar en una cuenta de Azure Cosmos DB SQL API'
description: Uso de Node.js para crear una aplicación que se conecte a una cuenta de Azure Cosmos DB SQL API y consulte datos.
author: deborahc
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: nodejs
ms.topic: quickstart
ms.date: 11/19/2019
ms.author: dech
ms.openlocfilehash: 8df78df27ffb7e8bb8fc88567bd0b3d37be20488
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2020
ms.locfileid: "76719507"
---
# <a name="quickstart-use-nodejs-to-connect-and-query-data-from-azure-cosmos-db-sql-api-account"></a>Inicio rápido: Uso de Node.js para conectarse y consultar datos en una cuenta de Azure Cosmos DB SQL API

> [!div class="op_single_selector"]
> * [.NET V3](create-sql-api-dotnet.md)
> * [.NET V4](create-sql-api-dotnet-V4.md)
> * [Java](create-sql-api-java.md)
> * [Node.js](create-sql-api-nodejs.md)
> * [Python](create-sql-api-python.md)
> * [Xamarin](create-sql-api-xamarin-dotnet.md)

En este inicio rápido se muestra cómo usar una aplicación en Node.js para conectarse a la cuenta [SQL API](sql-api-introduction.md) en Azure Cosmos DB. Luego puede usar consultas SQL de Azure Cosmos DB para consultar y administrar datos. La aplicación en Node.js que se crea en este artículo usa el [SDK de JavaScript de SQL](sql-api-sdk-node.md). En esta guía de inicio rápido se usa la versión 2.0 del [SDK de JavaScript](https://www.npmjs.com/package/@azure/cosmos).

## <a name="prerequisites"></a>Prerequisites

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)] 
[!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

* Además:
    * [Node.js](https://nodejs.org/en/) versión v6.0.0 o superior
    * [Git](https://git-scm.com/)

## <a name="create-a-database"></a>Crear una base de datos 

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a name="add-a-container"></a>Agregar un contenedor

[!INCLUDE [cosmos-db-create-collection](../../includes/cosmos-db-create-collection.md)]

## <a name="add-sample-data"></a>Adición de datos de ejemplo

[!INCLUDE [cosmos-db-create-sql-api-add-sample-data](../../includes/cosmos-db-create-sql-api-add-sample-data.md)]

## <a name="query-your-data"></a>Consulta de los datos

[!INCLUDE [cosmos-db-create-sql-api-query-data](../../includes/cosmos-db-create-sql-api-query-data.md)]

## <a name="clone-the-sample-application"></a>Clonación de la aplicación de ejemplo

Ahora vamos a clonar una aplicación en Node.js desde GitHub, establecer la cadena de conexión y ejecutarla.

1. Abra un símbolo del sistema, cree una carpeta nueva denominada ejemplos de GIT y, después, cierre el símbolo del sistema.

    ```bash
    md "C:\git-samples"
    ```

2. Abra una ventana de terminal de Git, como git bash y utilice el comando `cd` para cambiar a la nueva carpeta para instalar la aplicación de ejemplo.

    ```bash
    cd "C:\git-samples"
    ```

3. Ejecute el comando siguiente para clonar el repositorio de ejemplo. Este comando crea una copia de la aplicación de ejemplo en el equipo.

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-sql-api-nodejs-getting-started.git
    ```

## <a name="review-the-code"></a>Revisión del código

Este paso es opcional. Si está interesado en aprender cómo se crean los recursos de base de datos de Azure Cosmos en el código, puede revisar los siguientes fragmentos de código. En caso contrario, puede ir directamente a [Actualización de la cadena de conexión](#update-your-connection-string). 

Tenga en cuenta que, si está familiarizado con la versión anterior del SDK de JavaScript, puede estar habituado a ver los términos "colección" y "documento". Dado que Azure Cosmos DB admite [varios modelos de API](https://docs.microsoft.com/azure/cosmos-db/introduction), la versión 2.0+ del SDK de JavaScript usa los términos genéricos "container" (contenedor), que puede ser una colección, un gráfico o una tabla e "item" (elemento) para describir el contenido del contenedor.

Los fragmentos de código siguientes se han tomado del archivo **app.js**.

* El objeto `CosmosClient` no se ha inicializado.

    ```javascript
    const client = new CosmosClient({ endpoint, key });
    ```

* Cree una base de datos de Azure Cosmos.

    ```javascript
    const { database } = await client.databases.createIfNotExists({ id: databaseId });
    ```

* Se crea un contenedor (colección) en la base de datos.

    ```javascript
    const { container } = await client.database(databaseId).containers.createIfNotExists({ id: containerId });
    ```

* Se crea un elemento (documento).

    ```javascript
    const { item } = await client.database(databaseId).container(containerId).items.create(itemBody);
    ```

* Se realiza una consulta SQL a través de JSON se realiza en la base de datos de la familia. La consulta devuelve todos los elementos secundarios de la familia "Anderson". 

    ```javascript
      const querySpec = {
        query: 'SELECT VALUE r.children FROM root r WHERE r.lastName = @lastName',
        parameters: [
          {
            name: '@lastName',
            value: 'Andersen'
          }
        ]
      }

      const { resources: results } = await client
        .database(databaseId)
        .container(containerId)
        .items.query(querySpec)
        .fetchAll()
      for (var queryResult of results) {
        let resultString = JSON.stringify(queryResult)
        console.log(`\tQuery returned ${resultString}\n`)
      }
    ```    

## <a name="update-your-connection-string"></a>Actualización de la cadena de conexión

Ahora vuelva a Azure Portal para obtener los detalles de la cadena de conexión de su cuenta de Azure Cosmos. Copie la cadena de conexión en la aplicación para que pueda conectarse a su base de datos.

1. En [Azure Portal](https://portal.azure.com/), en la cuenta de Azure Cosmos, en el panel de navegación izquierdo, haga clic en **Claves** y en **Claves de lectura y escritura**. Deberá usar los botones de copia del lado derecho de la pantalla para copiar el URI y la clave principal en el archivo `config.js` en el paso siguiente.

    ![Visualización y copia de una clave de acceso en Azure Portal, hoja Claves](./media/create-sql-api-dotnet/keys.png)

2. Abra el archivo `config.js`. 

3. Copie el valor del URI del portal (con el botón de copia) y conviértalo en el valor de la clave de punto de conexión en `config.js`. 

    `config.endpoint = "<Your Azure Cosmos account URI>"`

4. Después, copie el valor de la clave principal del portal y conviértalo en el valor de `config.key` en `config.js`. Ya ha actualizado la aplicación con toda la información que necesita para comunicarse con Azure Cosmos DB. 

    `config.key = "<Your Azure Cosmos account key>"`
    
## <a name="run-the-app"></a>Ejecución la aplicación

1. Ejecute `npm install` en un terminal para instalar los módulos de NPM necesarios.

2. Ejecute `node app.js` en un terminal para iniciar la aplicación de nodo.

Ya puede volver al Explorador de datos, modificar y trabajar con estos nuevos datos.

## <a name="review-slas-in-the-azure-portal"></a>Revisión de los SLA en Azure Portal

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Limpieza de recursos

[!INCLUDE [cosmosdb-delete-resource-group](../../includes/cosmos-db-delete-resource-group.md)]

## <a name="next-steps"></a>Pasos siguientes

En este inicio rápido ha aprendido a crear una cuenta de Azure Cosmos, a crear un contenedor mediante el Explorador de datos y a ejecutar una aplicación. Ya puede importar datos adicionales en su base de datos de Azure Cosmos. 

> [!div class="nextstepaction"]
> [Importación de datos a Azure Cosmos DB](import-data.md)


