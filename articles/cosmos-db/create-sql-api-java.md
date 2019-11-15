---
title: 'Uso de Java para crear una base de datos de documentos: Azure Cosmos DB'
description: En este tema se presenta código de ejemplo de Java que se puede usar para conectarse a SQL API de Azure Cosmos DB y realizar consultas
author: SnehaGunda
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: java
ms.topic: quickstart
ms.date: 10/31/2019
ms.author: sngun
ms.custom: seo-java-august2019, seo-java-september2019
ms.openlocfilehash: d5a32780f8598c0843958b99f02cd18aa33bea2e
ms.sourcegitcommit: f4d8f4e48c49bd3bc15ee7e5a77bee3164a5ae1b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2019
ms.locfileid: "73582843"
---
# <a name="quickstart-build-a-java-app-to-manage-azure-cosmos-db-sql-api-data"></a>Inicio rápido: Creación de una aplicación Java para administrar los datos de SQL API de Azure Cosmos DB


> [!div class="op_single_selector"]
> * [.NET V3](create-sql-api-dotnet.md)
> * [.NET V4](create-sql-api-dotnet-V4.md)
> * [Java](create-sql-api-java.md)
> * [Node.js](create-sql-api-nodejs.md)
> * [Python](create-sql-api-python.md)
> * [Xamarin](create-sql-api-xamarin-dotnet.md)

Este inicio rápido muestra cómo usar una aplicación Java para crear y administrar una base de datos de documentos desde su cuenta de SQL API de Azure Cosmos DB. En primer lugar, creará una cuenta de SQL API de Azure Cosmos DB mediante Azure Portal, una aplicación Java con SQL Java SDK y, después, agregará recursos a la cuenta de Cosmos DB mediante la aplicación Java. Las instrucciones que se indican en este tutorial rápido se pueden seguir en cualquier sistema operativo que sea capaz de ejecutar Java. Después de completar esta guía de inicio rápido, ya sabrá cómo crear y modificar bases de datos de Cosmos DB y contenedores en la interfaz de usuario o mediante programación, lo que prefiera.

## <a name="prerequisites"></a>Requisitos previos

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)] 
[!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

Además: 

* [Kit de desarrollo de Java (JDK), versión 8](https://aka.ms/azure-jdks)
    * Asegúrese de establecer la variable de entorno JAVA_HOME para que apunte a la carpeta donde está instalado el JDK.
* [Descargar](https://maven.apache.org/download.cgi) e [instalar](https://maven.apache.org/install.html) un archivo binario de [Maven](https://maven.apache.org/)
    * En Ubuntu, puede ejecutar `apt-get install maven` para instalar Maven.
* [Git](https://www.git-scm.com/)
    * En Ubuntu, puede ejecutar `sudo apt-get install git` para instalar Git.

## <a name="create-a-database-account"></a>Creación de una cuenta de base de datos

Para poder crear una base de datos de documentos, debe crear una cuenta de SQL API con Azure Cosmos DB.

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a name="add-a-container"></a>Agregar un contenedor

[!INCLUDE [cosmos-db-create-collection](../../includes/cosmos-db-create-collection.md)]

<a id="add-sample-data"></a>
## <a name="add-sample-data"></a>Adición de datos de ejemplo

[!INCLUDE [cosmos-db-create-sql-api-add-sample-data](../../includes/cosmos-db-create-sql-api-add-sample-data.md)]

## <a name="query-your-data"></a>Consulta de los datos

[!INCLUDE [cosmos-db-create-sql-api-query-data](../../includes/cosmos-db-create-sql-api-query-data.md)]

## <a name="clone-the-sample-application"></a>Clonación de la aplicación de ejemplo

Ahora vamos a empezar a trabajar con el código. Vamos a clonar una aplicación de SQL API desde GitHub, establecer la cadena de conexión y ejecutarla. Verá lo fácil que es trabajar con datos mediante programación. 

1. Ejecute el comando siguiente para clonar el repositorio de ejemplo. Este comando crea una copia de la aplicación de ejemplo en el equipo.

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-java-getting-started.git
    ```

## <a name="review-the-code"></a>Revisión del código

Este paso es opcional. Si está interesado en aprender cómo se crean los recursos de base de datos en el código, puede revisar los siguientes fragmentos de código. En caso contrario, puede ir directamente a [Ejecutar la aplicación](#run-the-app). 

* Inicialización de `CosmosClient`. `CosmosClient` proporciona una representación lógica del cliente para el servicio de base de datos de Azure Cosmos. Este cliente se usa para configurar y ejecutar solicitudes en el servicio.
    
    [!code-java[](~/azure-cosmosdb-java-v4-getting-started/src/main/java/com/azure/cosmos/sample/sync/SyncMain.java?name=CreateSyncClient)]

* Creación de CosmosDatabase.

    [!code-java[](~/azure-cosmosdb-java-v4-getting-started/src/main/java/com/azure/cosmos/sample/sync/SyncMain.java?name=CreateDatabaseIfNotExists)]

* Creación de CosmosContainer.

    [!code-java[](~/azure-cosmosdb-java-v4-getting-started/src/main/java/com/azure/cosmos/sample/sync/SyncMain.java?name=CreateContainerIfNotExists)]

* Creación de elementos mediante el método `createItem`.

    [!code-java[](~/azure-cosmosdb-java-v4-getting-started/src/main/java/com/azure/cosmos/sample/sync/SyncMain.java?name=CreateItem)]
   
* Las lecturas puntuales se realizan mediante los métodos `getItem` y `read`.

    [!code-java[](~/azure-cosmosdb-java-v4-getting-started/src/main/java/com/azure/cosmos/sample/sync/SyncMain.java?name=ReadItem)]

* Las consultas SQL a través de JSON se realizan con el método `queryItems`.

    [!code-java[](~/azure-cosmosdb-java-v4-getting-started/src/main/java/com/azure/cosmos/sample/sync/SyncMain.java?name=QueryItems)]

## <a name="run-the-app"></a>Ejecución de la aplicación

Ahora vuelva a Azure Portal para obtener la información de la cadena de conexión e iniciar la aplicación con la información del punto de conexión. Esto permite que la aplicación se comunique con la base de datos hospedada.


1. En la ventana de terminal de GIT, `cd` a la carpeta del código de ejemplo.

    ```bash
    cd azure-cosmos-java-getting-started
    ```

2. En la ventana del terminal de GIT, use el comando siguiente para instalar los paquetes Java necesarios.

    ```bash
    mvn package
    ```

3. En la ventana del terminal de GIT, use el siguiente comando para iniciar la aplicación Java (reemplace YOUR_COSMOS_DB_HOSTNAME por el valor del identificador URI del portal, y reemplace YOUR_COSMOS_DB_MASTER_KEY por la clave principal del portal)

    ```bash
    mvn exec:java -DACCOUNT_HOST=YOUR_COSMOS_DB_HOSTNAME -DACCOUNT_KEY=YOUR_COSMOS_DB_MASTER_KEY

    ```

    La ventana del terminal muestra una notificación de que se ha creado la base de datos FamilyDB. 
    
4. La aplicación crea la base de datos con el nombre `AzureSampleFamilyDB`.
5. La aplicación crea un contenedor con el nombre `FamilyContainer`.
6. La aplicación realizará las lecturas puntuales con los identificadores de objeto y el valor de clave de partición (que es lastName en nuestro ejemplo). 
7. La aplicación consultará los elementos para recuperar todas las familias con el apellido en ("Andersen", "Wakefield", "Johnson").

7. La aplicación no elimina los recursos creados. Vuelva al portal para [limpiar los recursos](#clean-up-resources).  desde su cuenta para que no incurrir en gastos.

## <a name="review-slas-in-the-azure-portal"></a>Revisión de los SLA en Azure Portal

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Limpieza de recursos

[!INCLUDE [cosmosdb-delete-resource-group](../../includes/cosmos-db-delete-resource-group.md)]

## <a name="next-steps"></a>Pasos siguientes

En esta guía de inicio rápido, ha aprendido a crear una cuenta de Azure Cosmos, una base de datos de documentos y un contenedor mediante Data Explorer, así como a ejecutar una aplicación para que haga lo mismo mediante programación. Ahora puede importar datos adicionales en el contenedor de Azure Cosmos. 

> [!div class="nextstepaction"]
> [Importación de datos a Azure Cosmos DB](import-data.md)
