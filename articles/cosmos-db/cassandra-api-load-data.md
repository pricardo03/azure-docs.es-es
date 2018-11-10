---
title: Carga de datos de ejemplo en una tabla de Cassandra API de Azure Cosmos DB utilizando una aplicación Java | Microsoft Docs
description: En este artículo se explica cómo cargar datos de usuario de ejemplo en una tabla de la cuenta Cassandra API de Azure Cosmos DB utilizando una aplicación Java.
services: cosmos-db
author: kanshiG
ms.service: cosmos-db
ms.component: cosmosdb-cassandra
ms.topic: tutorial
ms.date: 09/24/2018
ms.author: govindk
ms.reviewer: sngun
ms.openlocfilehash: e116dbf1d49fed1a47b830f9a57cd77a33b7ea9c
ms.sourcegitcommit: ae45eacd213bc008e144b2df1b1d73b1acbbaa4c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/01/2018
ms.locfileid: "50740727"
---
# <a name="tutorial-load-sample-data-into-an-azure-cosmos-db-cassandra-api-table"></a>Tutorial: Carga de datos de ejemplo en una tabla de Cassandra API de Azure Cosmos DB

En este tutorial se explica cómo cargar datos de usuario de ejemplo en una tabla de la cuenta Cassandra API de Azure Cosmos DB mediante una aplicación Java. La aplicación Java emplea el [controlador de Java](https://github.com/datastax/java-driver) para cargar datos del usuario, como el identificador, el nombre o la ciudad del usuario. 

En este tutorial se describen las tareas siguientes:

> [!div class="checklist"]
> * Carga de datos en la tabla de Cassandra
> * Ejecución de la aplicación

## <a name="prerequisites"></a>Requisitos previos

* Este artículo pertenece a un tutorial de varias partes. Antes de empezar con este documento, no olvide [crear la cuenta, el espacio de claves y la tabla de Cassandra API](create-cassandra-api-account-java.md).   

## <a name="load-data-into-the-table"></a>Carga de datos en la tabla

Siga estos pasos para cargar datos en la tabla de Cassandra API:

1. Abra el archivo "UserRepository.java", que se encuentra en la carpeta "src\main\java\com\azure\cosmosdb\cassandra", y anexe el código para insertar los campos user_id, user_name y user_bcity en la tabla:

   ```java
   /**
   * Insert a row into user table
   *
   * @param id   user_id
   * @param name user_name
   * @param city user_bcity
   */
   public void insertUser(PreparedStatement statement, int id, String name, String city) {
        BoundStatement boundStatement = new BoundStatement(statement);
        session.execute(boundStatement.bind(id, name, city));
   }

   /**
   * Create a PrepareStatement to insert a row to user table
   *
   * @return PreparedStatement
   */
   public PreparedStatement prepareInsertStatement() {
      final String insertStatement = "INSERT INTO  uprofile.user (user_id, user_name , user_bcity) VALUES (?,?,?)";
   return session.prepare(insertStatement);
   }
   ```
 
2. Abra el archivo "UserProfile.java", que se encuentra en la carpeta "src\main\java\com\azure\cosmosdb\cassandra". Esta clase contiene el método principal que llama a los métodos createKeyspace y createTable definidos anteriormente. Ahora, anexe el código siguiente para insertar algunos datos de ejemplo en la tabla de Cassandra API.

   ```java
   //Insert rows into user table
   PreparedStatement preparedStatement = repository.prepareInsertStatement();
     repository.insertUser(preparedStatement, 1, "JohnH", "Seattle");
     repository.insertUser(preparedStatement, 2, "EricK", "Spokane");
     repository.insertUser(preparedStatement, 3, "MatthewP", "Tacoma");
     repository.insertUser(preparedStatement, 4, "DavidA", "Renton");
     repository.insertUser(preparedStatement, 5, "PeterS", "Everett");
   ```

## <a name="run-the-app"></a>Ejecución de la aplicación

Abra el símbolo del sistema o la ventana de terminal y cambie la ruta de acceso a la carpeta en la que ha creado el proyecto. Ejecute el comando "mvn clean install" para generar el archivo cosmosdb-cassandra-examples.jar dentro de la carpeta de destino y ejecute la aplicación. 

```bash
cd "cassandra-demo"

mvn clean install

java -cp target/cosmosdb-cassandra-examples.jar com.azure.cosmosdb.cassandra.examples.UserProfile
```

Ya puede abrir el Explorador de datos en Azure Portal para confirmar que la información del usuario se agrega a la tabla.
    
## <a name="next-steps"></a>Pasos siguientes

En este tutorial ha aprendido a cargar datos de ejemplo a la cuenta de Cassandra API de Azure Cosmos DB. Puede pasar al siguiente artículo:

> [!div class="nextstepaction"]
> [Consulta de los datos de la cuenta de Cassandra API](cassandra-api-query-data.md)
