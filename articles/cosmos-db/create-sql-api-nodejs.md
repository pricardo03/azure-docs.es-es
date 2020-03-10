---
title: 'Inicio rápido: Uso de Node.js para realizar consultas desde una cuenta de SQL API de Azure Cosmos DB'
description: Uso de Node.js para crear una aplicación que se conecte a una cuenta de Azure Cosmos DB SQL API y consulte datos.
author: deborahc
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: nodejs
ms.topic: quickstart
ms.date: 02/26/2020
ms.author: dech
ms.openlocfilehash: c36f31ef30b6386677c517b1d7e643f9eacea093
ms.sourcegitcommit: f915d8b43a3cefe532062ca7d7dbbf569d2583d8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/05/2020
ms.locfileid: "78303297"
---
# <a name="quickstart-use-nodejs-to-connect-and-query-data-from-azure-cosmos-db-sql-api-account"></a>Inicio rápido: Uso de Node.js para conectarse y consultar datos en una cuenta de Azure Cosmos DB SQL API

> [!div class="op_single_selector"]
> * [.NET V3](create-sql-api-dotnet.md)
> * [.NET V4](create-sql-api-dotnet-V4.md)
> * [Java](create-sql-api-java.md)
> * [Node.js](create-sql-api-nodejs.md)
> * [Python](create-sql-api-python.md)
> * [Xamarin](create-sql-api-xamarin-dotnet.md)

En este inicio rápido se crea y administra una cuenta de Azure Cosmos DB para SQL API desde Azure Portal, y se usa una aplicación de Node.js clonada desde GitHub. Azure Cosmos DB es un servicio de base de datos multimodelo que permite crear y consultar rápidamente bases de datos de documentos, tablas, claves-valores y grafos con funcionalidades de distribución global y escala horizontal.

## <a name="prerequisites"></a>Prerrequisitos

- Una cuenta de Azure con una suscripción activa. [cree una de forma gratuita](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio). O bien, [pruebe gratis Azure Cosmos DB](https://azure.microsoft.com/try/cosmosdb/) sin una suscripción de Azure. También puede usar el [emulador de Azure Cosmos DB](https://aka.ms/cosmosdb-emulator) con el identificador URI `https://localhost:8081` y la clave `C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==`.
- [Node.js 6.0.0+](https://nodejs.org/).
- [Git](https://www.git-scm.com/downloads).

## <a name="create-a-database"></a>Crear una base de datos

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a name="add-a-container"></a>Agregar un contenedor

Ahora puede usar la herramienta Explorador de datos en Azure Portal para crear una base de datos y un contenedor. 

1. Seleccione **Data Explorer** > **Nuevo contenedor**. 
    
    El área **Agregar contenedor** se muestra en el extremo derecho, pero es posible que haya que desplazarse hacia la derecha para verlo.

    ![Explorador de datos de Azure Portal, panel Agregar contenedor](./media/create-sql-api-nodejs/azure-cosmosdb-data-explorer.png)

2. En la página **Agregar contenedor**, especifique la configuración del nuevo contenedor.

    |Configuración|Valor sugerido|Descripción
    |---|---|---|
    |**Identificador de base de datos**|Tareas|Escriba *Tareas* como nombre de la nueva base de datos. Los nombres de base de datos tienen que tener entre 1 y 255 caracteres y no pueden contener `/, \\, #, ?` o espacios finales. Seleccione la opción **Provision database throughput** (Aprovisionar rendimiento de la base de datos) que le permite compartir el rendimiento aprovisionado de la base de datos entre todos los contenedores de esta. Esta opción también le ayudará a reducir los costos. |
    |**Rendimiento**|400|Deje el rendimiento en 400 unidades de solicitud por segundo (RU/s). Si quiere reducir la latencia, puede escalar verticalmente el rendimiento más adelante.| 
    |**ID de contenedor**|Elementos|Escriba *Elementos* como nombre del nuevo contenedor. Los identificadores de contenedor tienen los mismos requisitos de caracteres que los nombres de las bases de datos.|
    |**Clave de partición**| /categoría| El ejemplo que se describe en este artículo usa */category* como clave de partición.|
    
    Además de la configuración anterior, puede agregar opcionalmente **claves únicas** para el contenedor. En este ejemplo vamos a dejar el campo en blanco. Las claves únicas proporcionan a los desarrolladores la capacidad de agregar una capa de integridad de datos a la base de datos. Mediante la creación de una directiva de clave única al crear un contenedor, se garantiza la unicidad de uno o varios valores por clave de partición. Para más información, consulte el artículo [Claves únicas en Azure Cosmos DB](unique-keys.md).
    
    Seleccione **Aceptar**. El Explorador de datos muestra la nueva base de datos y el contenedor.

## <a name="add-sample-data"></a>Adición de datos de ejemplo

[!INCLUDE [cosmos-db-create-sql-api-add-sample-data](../../includes/cosmos-db-create-sql-api-add-sample-data.md)]

## <a name="query-your-data"></a>Consulta de los datos

[!INCLUDE [cosmos-db-create-sql-api-query-data](../../includes/cosmos-db-create-sql-api-query-data.md)]

## <a name="clone-the-sample-application"></a>Clonación de la aplicación de ejemplo

Ahora vamos a clonar una aplicación en Node.js desde GitHub, establecer la cadena de conexión y ejecutarla.

1. Ejecute el comando siguiente para clonar el repositorio de ejemplo. Este comando crea una copia de la aplicación de ejemplo en el equipo.

   ```bash
   git clone https://github.com/Azure-Samples/azure-cosmos-db-sql-api-nodejs-getting-started.git
   ```

## <a name="review-the-code"></a>Revisión del código

Este paso es opcional. Si está interesado en aprender cómo se crean los recursos de base de datos de Azure Cosmos en el código, puede revisar los siguientes fragmentos de código. En caso contrario, puede ir directamente a [Actualización de la cadena de conexión](#update-your-connection-string).

Si conoce la versión anterior del SDK de SQL para JavaScript, es posible que esté acostumbrado a ver los términos _colección_ y _documento_. Dado que Azure Cosmos DB admite [varios modelos de API](introduction.md), la [versión 2.0+ del SDK de JavaScript](https://www.npmjs.com/package/@azure/cosmos) usa los términos genéricos _container_ (contenedor), que puede ser una colección, un grafo o una tabla, e _item_ (elemento) para describir el contenido del contenedor.

Los fragmentos de código siguientes se han tomado del archivo _app.js_.

- El objeto `CosmosClient` no se ha inicializado.

  ```javascript
  const client = new CosmosClient({ endpoint, key });
  ```

- Seleccione la base de datos "Tasks".

  ```javascript
  const database = await client.databases(databaseId);
  ```

- Seleccione el contenedor o la colección "Items".

  ```javascript
  const container = await client.databases(containerId);
  ```

- Seleccione todos los elementos del contenedor "Items".

  ```javascript
  // query to return all items
  const querySpec = {
    query: "SELECT * from c"
  };

  const { resources: results } = await container.items
    .query(querySpec)
    .fetchAll();

  return results;
  ```

- Creación de un elemento

  ```javascript
  const { resource: createdItem } = await container.items.create(newItem);
  ```

- Actualización de un elemento

  ```javascript
  const { id, category } = createdItem;

  createdItem.isComplete = true;
  const { resource: itemToUpdate } = await container
    .item(id, category)
    .replace(itemToUpdate);

  return result;
  ```

- Eliminación de un elemento

  ```javascript
  const { resource: result } = await this.container.item(id, category).delete();
  ```

> [!NOTE]
> En los métodos "update" y "delete", el elemento debe seleccionarse de la base de datos mediante una llamada a `container.item()`. Los dos parámetros pasados son el identificador del elemento y la clave de partición del elemento. En este caso, la clave de partición es el valor del campo "category".

## <a name="update-your-connection-string"></a>Actualización de la cadena de conexión

Ahora vuelva a Azure Portal para obtener los detalles de la cadena de conexión de su cuenta de Azure Cosmos. Copie la cadena de conexión en la aplicación para que pueda conectarse a su base de datos.

1. En la cuenta de Azure Cosmos DB, en [Azure Portal](https://portal.azure.com/), seleccione **Claves** en el panel de navegación izquierdo y, después, **Claves de lectura y escritura**. Use los botones de copia del lado derecho de la pantalla para copiar el URI y la clave principal en el archivo _app.js_ en el paso siguiente.

   ![Visualización y copia de una clave de acceso en Azure Portal, hoja Claves](./media/create-sql-api-dotnet/keys.png)

2. Abra el archivo _config.js_.

3. Copie el valor del identificador URI del portal (mediante el botón de copia) y establézcalo como valor de la clave de punto de conexión en _config.js_.

   `endpoint: "<Your Azure Cosmos account URI>"`

4. Después, copie el valor de CLAVE PRINCIPAL del portal y establézcalo como el valor de `config.key` en _config.js_. Ya ha actualizado la aplicación con toda la información que necesita para comunicarse con Azure Cosmos DB.

   `key: "<Your Azure Cosmos account key>"`

## <a name="run-the-app"></a>Ejecución la aplicación

1. Ejecute `npm install` en un terminal para instalar los módulos de NPM necesarios.

2. Ejecute `node app.js` en un terminal para iniciar la aplicación de nodo.

Ya puede volver al Explorador de datos, modificar y trabajar con estos nuevos datos.

## <a name="review-slas-in-the-azure-portal"></a>Revisión de los SLA en Azure Portal

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Limpieza de recursos

[!INCLUDE [cosmosdb-delete-resource-group](../../includes/cosmos-db-delete-resource-group.md)]

## <a name="next-steps"></a>Pasos siguientes

En este inicio rápido, ha aprendido a crear una cuenta de Azure Cosmos DB, a crear un contenedor mediante el Explorador de datos y a ejecutar una aplicación de Node.js. Ahora puede importar datos adicionales en la cuenta de Azure Cosmos DB.

> [!div class="nextstepaction"]
> [Importación de datos a Azure Cosmos DB](import-data.md)
