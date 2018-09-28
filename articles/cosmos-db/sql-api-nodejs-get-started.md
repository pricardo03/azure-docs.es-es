---
title: Tutorial de Node.js para la API de SQL para Azure Cosmos DB | Microsoft Docs
description: Un tutorial de Node.js en el que se muestra cómo conectar con Azure Cosmos DB y realizar consultas en él mediante SQL API
services: cosmos-db
author: deborahc
editor: monicar
ms.service: cosmos-db
ms.component: cosmosdb-sql
ms.devlang: nodejs
ms.topic: tutorial
ms.date: 09/24/2018
ms.author: dech
ms.openlocfilehash: affa302c7fd2a0cb05a6d599050e72c75ef74479
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46969071"
---
# <a name="nodejs-tutorial-create-a-nodejs-console-application-with-javascript-sdk-to-manage-azure-cosmos-db-sql-api-data"></a>Tutorial de Node.js: crear una aplicación de consola de Node.js con el SDK de JavaScript para administrar los datos de API de Azure Cosmos DB SQL

> [!div class="op_single_selector"]
> * [.NET](sql-api-get-started.md)
> * [.NET Core](sql-api-dotnetcore-get-started.md)
> * [Java](sql-api-java-get-started.md)
> * [Async Java](sql-api-async-java-get-started.md)
> * [Node.js](sql-api-nodejs-get-started.md)
> 


Le damos la bienvenida al tutorial de Node.js para el SDK de JavaScript de Azure Cosmos DB. Después de seguir este tutorial, tendrá una aplicación de consola que crea recursos de Azure Cosmos DB y los consulta.

Describiremos:

* Creación de una cuenta de Azure Cosmos DB y conexión a ella
* Configuración de la aplicación
* Creación de una base de datos
* Creación de un contenedor
* Adición de elementos JSON al contenedor
* Consulta en el contenedor
* Sustitución de un elemento
* Eliminación de un elemento
* Eliminación de la base de datos

¿No tiene tiempo? ¡No se preocupe! La solución completa está disponible en [GitHub](https://github.com/Azure-Samples/azure-cosmos-db-sql-api-nodejs-getting-started ). Consulte [Obtención de la solución completa](#GetSolution) para obtener instrucciones rápidas.

Después de haber completado el tutorial de Node.js, use los botones de votos en la parte superior e inferior de esta página para enviar comentarios. Si quiere que nos pongamos en contacto directamente con usted, puede incluir su dirección de correo electrónico en los comentarios.

Comencemos.

## <a name="prerequisites-for-the-nodejs-tutorial"></a>Requisitos previos para el tutorial de Node.js

Asegúrese de que dispone de lo siguiente:

* Una cuenta de Azure activa. Si no tiene una, puede registrarse para obtener una [prueba gratuita de Azure](https://azure.microsoft.com/pricing/free-trial/). 

  [!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

* [Node.js](https://nodejs.org/) versión v6.0.0 o superior.

## <a name="step-1-create-an-azure-cosmos-db-account"></a>Paso 1: Creación de una cuenta de Azure Cosmos DB

Vamos a crear una cuenta de Azure Cosmos DB. Si ya tiene una cuenta que desea usar, puede ir directamente a [Configuración de la aplicación de Node.js](#SetupNode). Si usa el emulador de Azure Cosmos DB, siga los pasos que se indican en [Emulador de Azure Cosmos DB](local-emulator.md) para configurar el emulador y vaya directamente a [Configuración de la aplicación de Node.js](#SetupNode). 

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a id="SetupNode"></a>Paso 2: Configuración de la aplicación de Node.js

1. Abra su terminal favorito.
2. Busque la carpeta o el directorio donde desea guardar la aplicación de Node.js.
3. Cree dos archivos de JavaScript vacíos con los siguientes comandos:
   * Windows:
     * ```fsutil file createnew app.js 0```
     * ```fsutil file createnew config.js 0```
   * Linux/OS X:
     * ```touch app.js```
     * ```touch config.js```
4. Instale el módulo @azure/cosmos mediante npm. Use el comando siguiente:
   * ```npm install @azure/cosmos --save```

Estupendo. Ahora que terminamos la configuración, comencemos a escribir algo de código.

## <a id="Config"></a>Paso 3: Configuración de las opciones de la aplicación

Abra ```config.js``` en el editor de texto que prefiera.

Después, copie y pegue el siguiente fragmento de código y defina las propiedades ```config.endpoint``` y ```config.primaryKey``` para el identificador URI del punto de conexión de Azure Cosmos DB y la clave principal. Ambas opciones de configuración se encuentran en [Azure Portal](https://portal.azure.com).

![Tutorial de Node.js: captura de pantalla de Azure Portal que muestra una cuenta de Azure Cosmos DB, con el centro ACTIVO resaltado, el botón CLAVES resaltado en la hoja de la cuenta de Azure Cosmos DB y los valores de URI, CLAVE PRINCIPAL y CLAVE SECUNDARIA resaltados en la hoja Claves (base de datos de nodos)][keys]

```nodejs
// ADD THIS PART TO YOUR CODE
var config = {}

config.endpoint = "~your Azure Cosmos DB endpoint uri here~";
config.primaryKey = "~your primary key here~";
``` 

Copie y pegue los datos de ```database```, ```container``` y ```items``` en el objeto ```config``` siguiente, donde se definen las propiedades de ```config.endpoint``` y ```config.primaryKey```. Si ya dispone de los datos que desea almacenar en la base de datos, puede usar la [herramienta de migración de datos](import-data.md) de Azure Cosmos DB, en lugar de definir los datos aquí.

```nodejs
var config = {}

config.endpoint = "~your Azure Cosmos DB account endpoint uri here~";
config.primaryKey = "~your primary key here~";

config.database = {
    "id": "FamilyDatabase"
};

config.container = {
    "id": "FamilyContainer"
};

config.items = {
    "Andersen": {
        "id": "Anderson.1",
        "lastName": "Andersen",
        "parents": [{
            "firstName": "Thomas"
        }, {
                "firstName": "Mary Kay"
            }],
        "children": [{
            "firstName": "Henriette Thaulow",
            "gender": "female",
            "grade": 5,
            "pets": [{
                "givenName": "Fluffy"
            }]
        }],
        "address": {
            "state": "WA",
            "county": "King",
            "city": "Seattle"
        }
    },
    "Wakefield": {
        "id": "Wakefield.7",
        "parents": [{
            "familyName": "Wakefield",
            "firstName": "Robin"
        }, {
                "familyName": "Miller",
                "firstName": "Ben"
            }],
        "children": [{
            "familyName": "Merriam",
            "firstName": "Jesse",
            "gender": "female",
            "grade": 8,
            "pets": [{
                "givenName": "Goofy"
            }, {
                    "givenName": "Shadow"
                }]
        }, {
                "familyName": "Miller",
                "firstName": "Lisa",
                "gender": "female",
                "grade": 1
            }],
        "address": {
            "state": "NY",
            "county": "Manhattan",
            "city": "NY"
        },
        "isRegistered": false
    }
};
```
Tenga en cuenta que, si está familiarizado con la versión anterior del SDK de JavaScript, puede estar habituado a ver los términos "colección" y "documento". Dado que Azure Cosmos DB admite [varios modelos de API](https://docs.microsoft.com/azure/cosmos-db/introduction#key-capabilities), todas las versiones del SDK de JavaScript posteriores a la 2.0 usan los términos genéricos "contenedor" y "elemento". Un contenedor puede ser una colección, un grafo o una tabla. Un elemento puede ser un documento, un vértice o borde, o una fila, y es el contenido que hay en un contenedor. 

Por último, exporte el objeto ```config```, con el fin de que pueda hacer referencia a él en el archivo ```app.js```.
```nodejs
        },
        "isRegistered": false
    }
};

// ADD THIS PART TO YOUR CODE
module.exports = config;
```
## <a id="Connect"></a>Paso 4: Conexión a una cuenta de Azure Cosmos DB

Abra el archivo ```app.js``` vacío en el editor de texto. Copie y pegue el código siguiente para importar el módulo ```@azure/cosmos``` y el módulo ```config``` que acaba de crear.

```nodejs
// ADD THIS PART TO YOUR CODE
const CosmosClient = require('@azure/cosmos').CosmosClient;

const config = require('./config');
const url = require('url');
```

Copie y pegue el código para usar las propiedades ```config.endpoint``` y ```config.primaryKey``` que guardó anteriormente para crear un nuevo CosmosClient.

```nodejs
const url = require('url');

// ADD THIS PART TO YOUR CODE
const endpoint = config.endpoint;
const masterKey = config.primaryKey;

const client = new CosmosClient({ endpoint: endpoint, auth: { masterKey: masterKey } });
```

Ahora que tiene el código necesario para inicializar el cliente de Azure Cosmos DB, se explicará cómo usar los recursos de este servicio.

## <a name="step-5-create-a-database"></a>Paso 5: Creación de una base de datos

Copie y pegue el código siguiente para establecer el estado HTTP en No encontrado, el identificador de la base de datos y el identificador del contenedor. Dichos identificadores son la forma en que el cliente de Azure Cosmos DB encuentra la base de datos y el contenedor correctos.

```nodejs
const client = new CosmosClient({ endpoint: endpoint, auth: { masterKey: masterKey } });

// ADD THIS PART TO YOUR CODE
const HttpStatusCodes = { NOTFOUND: 404 };

const databaseId = config.database.id;
const containerId = config.container.id;
```

Para crear una [base de datos](sql-api-resources.md#databases) se pueden usar las funciones [createIfNotExists](/javascript/api/%40azure/cosmos/databases) o [create](/javascript/api/%40azure/cosmos/databases) de la clase **Databases**. Una base de datos es el contenedor lógico de elementos con particiones en contenedores. 

Copie y pegue las funciones **createDatabase** y **readDatabase** en el archivo app.js debajo de la definición de ```databaseId``` y ```containerId```. La función **createDatabase** creará una base de datos con el identificador ```FamilyDatabase```, que se especifica desde el objeto ```config``` si aún no existe. La función **readDatabase** leerá la definición de la base de datos para asegurarse de que la base de datos existe.

```nodejs
/**
 * Create the database if it does not exist
 */
async function createDatabase() {
    const { database } = await client.databases.createIfNotExists({ id: databaseId });
    console.log(`Created database:\n${database.id}\n`);
}

/**
 * Read the database definition
 */
async function readDatabase() {
    const { body: databaseDefinition } = await client.database(databaseId).read();
    console.log(`Reading database:\n${databaseDefinition.id}\n`);
}
```

Copie y pegue el siguiente código debajo de donde definió las funciones **createDatabase** y **readDatabase** para agregar la función auxiliar **exit** que imprimirá el mensaje de salida. 

```nodejs
// ADD THIS PART TO YOUR CODE
function exit(message) {
    console.log(message);
    console.log('Press any key to exit');
    process.stdin.setRawMode(true);
    process.stdin.resume();
    process.stdin.on('data', process.exit.bind(process, 0));
};
```
Copie y pegue el siguiente código debajo de donde definió la función **exit** para llamar a las funciones **createDatabase** y **readDatabase**.

```nodejs
createDatabase()
    .then(() => readDatabase())
    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error \${JSON.stringify(error)}`) });
```

En este momento, el código de ```app.js``` debería ser como este:

```nodejs
const CosmosClient = require('@azure/cosmos').CosmosClient;

const config = require('./config');
const url = require('url');

const endpoint = config.endpoint;
const masterKey = config.primaryKey;

const client = new CosmosClient({ endpoint: endpoint, auth: { masterKey: masterKey } });

const HttpStatusCodes = { NOTFOUND: 404 };

const databaseId = config.database.id;
const containerId = config.container.id;


/**
 * Create the database if it does not exist
 */
async function createDatabase() {
    const { database } = await client.databases.createIfNotExists({ id: databaseId });
    console.log(`Created database:\n${database.id}\n`);
}

/**
 * Read the database definition
 */
async function readDatabase() {
    const { body: databaseDefinition } = await client.database(databaseId).read();
    console.log(`Reading database:\n${databaseDefinition.id}\n`);
}

/**
 * Exit the app with a prompt
 * @param {message} message - The message to display
 */
function exit(message) {
    console.log(message);
    console.log('Press any key to exit');
    process.stdin.setRawMode(true);
    process.stdin.resume();
    process.stdin.on('data', process.exit.bind(process, 0));
}

createDatabase()
    .then(() => readDatabase())
    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });
```

En el terminal, busque el archivo ```app.js``` y ejecute el comando: 
```bash 
node app.js
```

Felicidades. Ha creado correctamente una base de datos de Azure Cosmos DB.

## <a id="CreateContainer"></a>Paso 6: Creación de un contenedor

> [!WARNING]
> Al llamar a la función **createContainer** se creará un contenedor nuevo, lo que afecta a los precios. Para más información, visite nuestra [página de precios](https://azure.microsoft.com/pricing/details/cosmos-db/).

Para crear un contenedor se pueden usar las funciones [createIfNotExists](/javascript/api/%40azure/cosmos/containers) o [create](/javascript/api/%40azure/cosmos/containers) de la clase **Containers**. 

Un contenedor se compone de elementos (que en el caso de SQL API son documentos JSON) y un registro de aplicaciones JavaScript asociado.

Copie y pegue las funciones **createContainer** y **readContainer** debajo de la función **readDatabase** en el archivo app.js. La función **createContainer** creará un contenedor con ```containerId```, que se especifica desde el objeto ```config``` si aún no existe. La función **readContainer** leerá la definición del contenedor para comprobar que el contenedor existe.

```nodejs
/**
 * Create the container if it does not exist
 */
async function createContainer() {
    const { container } = await client.database(databaseId).containers.createIfNotExists({ id: containerId });
    console.log(`Created container:\n${config.container.id}\n`);
}

/**
 * Read the container definition
 */
async function readContainer() {
    const { body: containerDefinition } = await client.database(databaseId).container(containerId).read();
    console.log(`Reading container:\n${containerDefinition.id}\n`);
}
```

Copie y pegue el código debajo de la llamada a **readDatabase** para ejecutar las funciones **createContainer** y **readContainer**.
```nodejs
createDatabase()
    .then(() => readDatabase())

    // ADD THIS PART TO YOUR CODE
    .then(() => createContainer())
    .then(() => readContainer())
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });
```

En este momento, el código de ```app.js``` debería ser como este:

```nodejs
const CosmosClient = require('@azure/cosmos').CosmosClient;

const config = require('./config');
const url = require('url');

const endpoint = config.endpoint;
const masterKey = config.primaryKey;

const client = new CosmosClient({ endpoint: endpoint, auth: { masterKey: masterKey } });

const HttpStatusCodes = { NOTFOUND: 404 };

const databaseId = config.database.id;
const containerId = config.container.id;

/**
 * Create the database if it does not exist
 */
async function createDatabase() {
    const { database } = await client.databases.createIfNotExists({ id: databaseId });
    console.log(`Created database:\n${database.id}\n`);
}

/**
 * Read the database definition
 */
async function readDatabase() {
    const { body: databaseDefinition } = await client.database(databaseId).read();
    console.log(`Reading database:\n${databaseDefinition.id}\n`);
}

/**
 * Create the container if it does not exist
 */
async function createContainer() {
    const { container } = await client.database(databaseId).containers.createIfNotExists({ id: containerId });
    console.log(`Created container:\n${config.container.id}\n`);
}

/**
 * Read the container definition
 */
async function readContainer() {
    const { body: containerDefinition } = await client.database(databaseId).container(containerId).read();
    console.log(`Reading container:\n${containerDefinition.id}\n`);
}

/**
 * Exit the app with a prompt
 * @param {message} message - The message to display
 */
function exit(message) {
    console.log(message);
    console.log('Press any key to exit');
    process.stdin.setRawMode(true);
    process.stdin.resume();
    process.stdin.on('data', process.exit.bind(process, 0));
}

createDatabase()
    .then(() => readDatabase())
    .then(() => createContainer())
    .then(() => readContainer())
    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });
```

En el terminal, busque el archivo ```app.js``` y ejecute el comando: 

```bash 
node app.js
```

Felicidades. Ha creado correctamente un contenedor de Azure Cosmos DB.

## <a id="CreateItem"></a>Paso7: Creación de un elemento

Para crear un elemento es preciso usar la función [create](/javascript/api/%40azure/cosmos/items) de la clase **Items**. Cuando se usa SQL API, los elementos se proyectan como documentos, que son contenido JSON (arbitrario) definido por el usuario. Ahora puede insertar un elemento en Azure Cosmos DB.

Copie y pegue la función **createFamilyItem** debajo de la función **readContainer**. La función **createFamilyItem** crea los elementos que contienen los datos JSON guardados en el objeto ```config```. Vamos a asegurarnos de que un elemento con el mismo identificador no existe antes de crearlo.

```nodejs
/**
 * Create family item if it does not exist
 */
async function createFamilyItem(itemBody) {
    try {
        // read the item to see if it exists
        const { item } = await client.database(databaseId).container(containerId).item(itemBody.id).read();
        console.log(`Item with family id ${itemBody.id} already exists\n`);
    }
    catch (error) {
        // create the family item if it does not exist
        if (error.code === HttpStatusCodes.NOTFOUND) {
            const { item } = await client.database(databaseId).container(containerId).items.create(itemBody);
            console.log(`Created family item with id:\n${itemBody.id}\n`);
        } else {
            throw error;
        }
    }
};
```

Copie y pegue el código de debajo de la llamada a **readContainer** para ejecutar la función **createFamilyItem**.
```nodejs
createDatabase()
    .then(() => readDatabase())
    .then(() => createContainer())
    .then(() => readContainer())

    // ADD THIS PART TO YOUR CODE
    .then(() => createFamilyItem(config.items.Andersen))
    .then(() => createFamilyItem(config.items.Wakefield))
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });
```

En el terminal, busque el archivo ```app.js``` y ejecute el comando: 

```bash 
node app.js
```

Felicidades. Ha creado correctamente un elemento de Azure Cosmos DB.


## <a id="Query"></a>Paso 8: Consulta de los recursos de Azure Cosmos DB
Azure Cosmos DB admite [consultas enriquecidas](sql-api-sql-query.md) en los documentos JSON que se almacenan en los contenedores. El código de ejemplo siguiente muestra una consulta que se puede ejecutar en los documentos de un contenedor.

Copie y pegue la función **queryContainer** debajo de la función **createFamilyItem** en el archivo app.js. Azure Cosmos DB admite consultas del tipo SQL tal y como se muestra a continuación. Para más información sobre cómo crear consultas complejas, consulte [Query Playground](https://www.documentdb.com/sql/demo) y la [documentación sobre consultas](sql-api-sql-query.md).

```nodejs
/**
 * Query the container using SQL
 */
async function queryContainer() {
    console.log(`Querying container:\n${config.container.id}`);

    // query to return all children in a family
    const querySpec = {
        query: "SELECT VALUE r.children FROM root r WHERE r.lastName = @lastName",
        parameters: [
            {
                name: "@lastName",
                value: "Andersen"
            }
        ]
    };

    const { result: results } = await client.database(databaseId).container(containerId).items.query(querySpec).toArray();
    for (var queryResult of results) {
        let resultString = JSON.stringify(queryResult);
        console.log(`\tQuery returned ${resultString}\n`);
    }
};
```

Copie y pegue el código de debajo de las llamadas en **createFamilyItem** para ejecutar la función **queryContainer**.

```nodejs
createDatabase()
    .then(() => readDatabase())
    .then(() => createContainer())
    .then(() => readContainer())
    .then(() => createFamilyItem(config.items.Andersen))
    .then(() => createFamilyItem(config.items.Wakefield))

    // ADD THIS PART TO YOUR CODE
    .then(() => queryContainer())
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });
```

En el terminal, busque el archivo ```app.js``` y ejecute el comando:

```bash 
node app.js
```

Felicidades. Ha consultado correctamente elementos de Azure Cosmos DB.

## <a id="ReplaceItem"></a>Paso 9: Reemplazo de un elemento
Azure Cosmos DB admite el reemplazo del contenido de los elementos.

Copie y pegue la función **replaceFamilyItem** debajo de la función **queryContainer** en el archivo app.js. Observe que hemos cambiado la propiedad "grade" de un elemento secundario del valor anterior de 5 a 6.

```nodejs
// ADD THIS PART TO YOUR CODE
/**
 * Replace the item by ID.
 */
async function replaceFamilyItem(itemBody) {
    console.log(`Replacing item:\n${itemBody.id}\n`);
    // Change property 'grade'
    itemBody.children[0].grade = 6;
    const { item } = await client.database(databaseId).container(containerId).item(itemBody.id).replace(itemBody);
};
```
Copie y pegue el código de debajo de la llamada en **queryContainer** para ejecutar la función **replaceFamilyItem**. Además, vuelva a agregar el código para llamar a **queryContainer** para comprobar que el elemento ha cambiado correctamente.

```nodejs
createDatabase()
    .then(() => readDatabase())
    .then(() => createContainer())
    .then(() => readContainer())
    .then(() => createFamilyItem(config.items.Andersen))
    .then(() => createFamilyItem(config.items.Wakefield))
    .then(() => queryContainer())

    // ADD THIS PART TO YOUR CODE
    .then(() => replaceFamilyItem(config.items.Andersen))
    .then(() => queryContainer())
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });
```
En el terminal, busque el archivo ```app.js``` y ejecute el comando:

```bash 
node app.js
```

Felicidades. Ha sustituido correctamente un elemento de Azure Cosmos DB.

## <a id="DeleteItem"></a>Paso 10: Eliminación de un elemento

Azure Cosmos DB admite la eliminación de elementos JSON.

Copie y pegue la función **deleteFamilyItem** debajo de la función **replaceFamilyItem**.

```nodejs
/**
 * Delete the item by ID.
 */
async function deleteFamilyItem(itemBody) {
    await client.database(databaseId).container(containerId).item(itemBody.id).delete(itemBody);
    console.log(`Deleted item:\n${itemBody.id}\n`);
};
```

Copie y pegue el código de debajo de la llamada a la segunda función **queryContainer** para ejecutar la función **deleteFamilyItem**.

```nodejs
createDatabase()
    .then(() => readDatabase())
    .then(() => createContainer())
    .then(() => readContainer())
    .then(() => createFamilyItem(config.items.Andersen))
    .then(() => createFamilyItem(config.items.Wakefield))
    .then(() => queryContainer
    ())
    .then(() => replaceFamilyItem(config.items.Andersen))
    .then(() => queryContainer())

    // ADD THIS PART TO YOUR CODE
    .then(() => deleteFamilyItem(config.items.Andersen))
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });
```

En el terminal, busque el archivo ```app.js``` y ejecute el comando: 

```bash 
node app.js
```

Felicidades. Ha eliminado correctamente un elemento de Azure Cosmos DB.

## <a id="DeleteDatabase"></a>Paso 11: Eliminación de la base de datos

La eliminación de la base de datos creada quitará la base de datos y todos los recursos secundarios (contenedores, elementos, etc.).

Copie y pegue la función **cleanup** debajo de la función **deleteFamilyItem** para quitar la base de datos y todos sus recursos secundarios.

```nodejs
/**
 * Cleanup the database and container on completion
 */
async function cleanup() {
    await client.database(databaseId).delete();
}
```

Copie y pegue el código de debajo de la llamada a **deleteFamilyItem** para ejecutar la función **cleanup**.

```nodejs
createDatabase()
    .then(() => readDatabase())
    .then(() => createContainer())
    .then(() => readContainer())
    .then(() => createFamilyItem(config.items.Andersen))
    .then(() => createFamilyItem(config.items.Wakefield))
    .then(() => queryContainer())
    .then(() => replaceFamilyItem(config.items.Andersen))
    .then(() => queryContainer())
    .then(() => deleteFamilyItem(config.items.Andersen))

    // ADD THIS PART TO YOUR CODE
    .then(() => cleanup())
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });
```

## <a id="Run"></a>Paso 12: Ejecute íntegramente la aplicación Node.js

En conjunto, el código debe ser similar a este:
```nodejs
const CosmosClient = require('@azure/cosmos').CosmosClient;

const config = require('./config');
const url = require('url');

const endpoint = config.endpoint;
const masterKey = config.primaryKey;

const HttpStatusCodes = { NOTFOUND: 404 };

const databaseId = config.database.id;
const containerId = config.container.id;

const client = new CosmosClient({ endpoint: endpoint, auth: { masterKey: masterKey } });

/**
 * Create the database if it does not exist
 */
async function createDatabase() {
    const { database } = await client.databases.createIfNotExists({ id: databaseId });
    console.log(`Created database:\n${database.id}\n`);
}

/**
 * Read the database definition
 */
async function readDatabase() {
    const { body: databaseDefinition } = await client.database(databaseId).read();
    console.log(`Reading database:\n${databaseDefinition.id}\n`);
}

/**
 * Create the container if it does not exist
 */
async function createContainer() {
    const { container } = await client.database(databaseId).containers.createIfNotExists({ id: containerId });
    console.log(`Created container:\n${config.container.id}\n`);
}

/**
 * Read the container definition
 */
async function readContainer() {
    const { body: containerDefinition } = await client.database(databaseId).container(containerId).read();
    console.log(`Reading container:\n${containerDefinition.id}\n`);
}

/**
 * Create family item if it does not exist
 */
async function createFamilyItem(itemBody) {
    try {
        // read the item to see if it exists
        const { item } = await client.database(databaseId).container(containerId).item(itemBody.id).read();
        console.log(`Item with family id ${itemBody.id} already exists\n`);
    }
    catch (error) {
        // create the family item if it does not exist
        if (error.code === HttpStatusCodes.NOTFOUND) {
            const { item } = await client.database(databaseId).container(containerId).items.create(itemBody);
            console.log(`Created family item with id:\n${itemBody.id}\n`);
        } else {
            throw error;
        }
    }
};

/**
 * Query the container using SQL
 */
async function queryContainer() {
    console.log(`Querying container:\n${config.container.id}`);

    // query to return all children in a family
    const querySpec = {
        query: "SELECT VALUE r.children FROM root r WHERE r.lastName = @lastName",
        parameters: [
            {
                name: "@lastName",
                value: "Andersen"
            }
        ]
    };

    const { result: results } = await client.database(databaseId).container(containerId).items.query(querySpec).toArray();
    for (var queryResult of results) {
        let resultString = JSON.stringify(queryResult);
        console.log(`\tQuery returned ${resultString}\n`);
    }
};

/**
 * Replace the item by ID.
 */
async function replaceFamilyItem(itemBody) {
    console.log(`Replacing item:\n${itemBody.id}\n`);
    // Change property 'grade'
    itemBody.children[0].grade = 6;
    const { item } = await client.database(databaseId).container(containerId).item(itemBody.id).replace(itemBody);
};

/**
 * Delete the item by ID.
 */
async function deleteFamilyItem(itemBody) {
    await client.database(databaseId).container(containerId).item(itemBody.id).delete(itemBody);
    console.log(`Deleted item:\n${itemBody.id}\n`);
};

/**
 * Cleanup the database and container on completion
 */
async function cleanup() {
    await client.database(databaseId).delete();
}

/**
 * Exit the app with a prompt
 * @param {message} message - The message to display
 */
function exit(message) {
    console.log(message);
    console.log('Press any key to exit');
    process.stdin.setRawMode(true);
    process.stdin.resume();
    process.stdin.on('data', process.exit.bind(process, 0));
}

createDatabase()
    .then(() => readDatabase())
    .then(() => createContainer())
    .then(() => readContainer())
    .then(() => createFamilyItem(config.items.Andersen))
    .then(() => createFamilyItem(config.items.Wakefield))
    .then(() => queryContainer())
    .then(() => replaceFamilyItem(config.items.Andersen))
    .then(() => queryContainer())
    .then(() => deleteFamilyItem(config.items.Andersen))
    .then(() => cleanup())
    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });
```

En el terminal, busque el archivo ```app.js``` y ejecute el comando: 

```bash 
node app.js
```

Ahora debería ver la salida de la aplicación GetStarted. La salida debe coincidir con el texto del ejemplo siguiente.

    Created database:
    FamilyDatabase

    Reading database:
    FamilyDatabase

    Created container:
    FamilyContainer

    Reading container:
    FamilyContainer

    Created family item with id:
    Anderson.1

    Created family item with id:
    Wakefield.7

    Querying container:
    FamilyContainer
            Query returned [{"firstName":"Henriette Thaulow","gender":"female","grade":5,"pets":[{"givenName":"Fluffy"}]}]

    Replacing item:
    Anderson.1

    Querying container:
    FamilyContainer
            Query returned [{"firstName":"Henriette Thaulow","gender":"female","grade":6,"pets":[{"givenName":"Fluffy"}]}]

    Deleted item:
    Anderson.1

    Completed successfully
    Press any key to exit

Felicidades. Ha completado el tutorial de Node.js y tiene la primera aplicación de consola de Azure Cosmos DB.

## <a id="GetSolution"></a>Obtención de la solución completa del tutorial de Node.js

Si no dispuso de tiempo para completar los pasos de este tutorial, o simplemente desea descargar el código, puede obtenerlo de [GitHub](https://github.com/Azure-Samples/azure-cosmos-db-sql-api-nodejs-getting-started ).

Para ejecutar la solución de introducción que contiene todo el código de este artículo, necesitará lo siguiente:

* Una [cuenta de Azure Cosmos DB][create-account].
* La solución [Getting Started](https://github.com/Azure-Samples/azure-cosmos-db-sql-api-nodejs-getting-started) disponible en GitHub.

Instale el módulo **@azure/cosmos** mediante npm. Use el comando siguiente:

* ```npm install @azure/cosmos --save```

Después, en el archivo ```config.js``` , actualice los valores de config.endpoint y config.primaryKey tal como se describe en el [paso 3: Configuración de las opciones de la aplicación](#Config). 

Luego, en el terminal, busque el archivo ```app.js``` y ejecute el comando: 

```bash 
node app.js
```

Ya está, ha terminado. 

## <a name="next-steps"></a>Pasos siguientes
* ¿Desea un ejemplo más complejo de Node.js? Consulte [Creación de una aplicación web de Node.js con Azure Cosmos DB](sql-api-nodejs-application.md).
* Información acerca de cómo [supervisar una cuenta de Azure Cosmos DB](monitor-accounts.md).
* Ejecute las consultas en nuestro conjunto de datos de ejemplo en el [área de consultas](https://www.documentdb.com/sql/demo).

[create-account]: create-sql-api-dotnet.md#create-account
[keys]: media/sql-api-nodejs-get-started/node-js-tutorial-keys.png