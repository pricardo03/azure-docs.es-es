---
title: Consulta de datos de una cuenta de Cassandra API de Azure Cosmos DB
description: En este artículo se explica cómo consultar datos de usuario de la cuenta Cassandra API de Azure Cosmos DB utilizando una aplicación Java.
services: cosmos-db
ms.service: cosmos-db
author: kanshiG
ms.author: govindk
ms.reviewer: sngun
ms.component: cosmosdb-cassandra
ms.topic: tutorial
ms.date: 09/24/2018
ms.openlocfilehash: a06e7e6159953bfeffa966759d29b91bbcbafd37
ms.sourcegitcommit: ae45eacd213bc008e144b2df1b1d73b1acbbaa4c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/01/2018
ms.locfileid: "50739216"
---
# <a name="tutorial-query-data-from-an-azure-cosmos-db-cassandra-api-account"></a>Tutorial: Consulta de datos de una cuenta de Cassandra API de Azure Cosmos DB

En este tutorial se explica cómo consultar datos de usuario de la cuenta Cassandra API de Azure Cosmos DB utilizando una aplicación Java. La aplicación Java emplea el [controlador de Java](https://github.com/datastax/java-driver) y consulta datos del usuario, como el identificador, el nombre o la ciudad del usuario. 

En este tutorial se describen las tareas siguientes:

> [!div class="checklist"]
> * Consulta de datos de tabla de Cassandra
> * Ejecución de la aplicación

## <a name="prerequisites"></a>Requisitos previos

* Este artículo pertenece a un tutorial de varias partes. Antes de empezar, asegúrese de completar los pasos anteriores para [crear la cuenta de Cassandra API], el espacio de claves, la tabla y [cargar datos de ejemplo en la tabla](cassandra-api-load-data.md). 

## <a name="query-data"></a>Datos de consulta

Use los pasos siguientes para consultar los datos de su cuenta de Cassandra API:

1. Abra el archivo `UserRepository.java` en la carpeta `src\main\java\com\azure\cosmosdb\cassandra`. Anexe el bloque de código siguiente. Este código proporciona tres métodos: 

   * Para consultar todos los usuarios de la base de datos
   * Para consultar un usuario específico, filtrado por identificador de usuario
   * Para eliminar una tabla.

   ```java
   /**
   * Select all rows from user table
   */
   public void selectAllUsers() {

     final String query = "SELECT * FROM uprofile.user";
     List<Row> rows = session.execute(query).all();

     for (Row row : rows) {
        LOGGER.info("Obtained row: {} | {} | {} ", row.getInt("user_id"), row.getString("user_name"), row.getString("user_bcity"));
     }
   }

   /**
   * Select a row from user table
   *
   * @param id user_id
   */
   public void selectUser(int id) {
      final String query = "SELECT * FROM uprofile.user where user_id = 3";
      Row row = session.execute(query).one();

      LOGGER.info("Obtained row: {} | {} | {} ", row.getInt("user_id"), row.getString("user_name"), row.getString("user_bcity"));
   }

   /**
   * Delete user table.
   */
   public void deleteTable() {
     final String query = "DROP TABLE IF EXISTS uprofile.user";
     session.execute(query);
   }
   ```

2. Abra el archivo `UserProfile.java` en la carpeta `src\main\java\com\azure\cosmosdb\cassandra`. Esta clase contiene el método principal que llama a los métodos de inserción de datos createKeyspace y createTable definidos anteriormente. Ahora, anexe el código siguiente que consulta todos los usuarios o un usuario específico:

   ```java
   LOGGER.info("Select all users");
   repository.selectAllUsers();

   LOGGER.info("Select a user by id (3)");
   repository.selectUser(3);

   LOGGER.info("Delete the users profile table");
   repository.deleteTable();
   ```

## <a name="run-the-java-app"></a>Ejecución de la aplicación Java
1. Abra un símbolo del sistema o una ventana de terminal. Pegue el bloque de código siguiente. 

   Este código cambia el directorio (cd) a la ruta de acceso de la carpeta donde creó el proyecto. A continuación, se ejecuta el comando `mvn clean install` para generar el archivo `cosmosdb-cassandra-examples.jar` en la carpeta de destino. Finalmente, se ejecuta la aplicación de Java.

   ```bash
   cd "cassandra-demo"
   
   mvn clean install
   
   java -cp target/cosmosdb-cassandra-examples.jar com.azure.cosmosdb.cassandra.examples.UserProfile
   ```

2. Ahora, en Azure Portal, abra el **Explorador de datos** y confirme que se ha eliminado la tabla de usuario.

## <a name="next-steps"></a>Pasos siguientes

* En este tutorial ha aprendido a consultar datos de la cuenta de Cassandra API de Azure Cosmos DB. Avance al siguiente artículo:

> [!div class="nextstepaction"]
> [Migrar datos a la cuenta de Cassandra API](cassandra-import-data.md)


