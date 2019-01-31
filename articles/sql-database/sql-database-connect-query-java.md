---
title: Uso de Java para consultar Azure SQL Database | Microsoft Docs
description: En este tema se muestra cómo usar Java para crear un programa que se conecta a Azure SQL Database y realiza consultas mediante instrucciones T-SQL.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.devlang: java
ms.topic: quickstart
author: ajlam
ms.author: andrela
ms.reviewer: v-masebo
manager: craigg
ms.date: 11/20/2018
ms.openlocfilehash: 6b748ebdbdc443c5fa44c82323774cd12c82ba38
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2019
ms.locfileid: "55197507"
---
# <a name="quickstart-use-java-to-query-an-azure-sql-database"></a>Inicio rápido: Uso de Java para consultar una base de datos SQL de Azure

En este artículo se muestra cómo usar [Java](/sql/connect/jdbc/microsoft-jdbc-driver-for-sql-server) para conectarse a una instancia de Azure SQL Database. Luego puede usar instrucciones de T-SQL para consultar los datos.

## <a name="prerequisites"></a>Requisitos previos

Para completar este ejemplo, asegúrese de que cumple los siguientes requisitos previos:

[!INCLUDE [prerequisites-create-db](../../includes/sql-database-connect-query-prerequisites-create-db-includes.md)]

- Java y software relacionado instalado para el sistema operativo:

  - **MacOS**: instalación de Homebrew y Java y, a continuación, instalación de Maven. Consulte [pasos 1.2 y 1.3](https://www.microsoft.com/sql-server/developer-get-started/java/mac/).

  - **Ubuntu**: instalación de Java, instalación del Kit de desarrollo de Java e instalación de Maven. Consulte [pasos 1.2, 1.3 y 1.4](https://www.microsoft.com/sql-server/developer-get-started/java/ubuntu/).

  - **Windows**: instalación de Java y, a continuación, instalación de Maven. Consulte [pasos 1.2 y 1.3](https://www.microsoft.com/sql-server/developer-get-started/java/windows/).

## <a name="get-database-connection"></a>Obtención de la conexión de la base de datos

[!INCLUDE [prerequisites-server-connection-info](../../includes/sql-database-connect-query-prerequisites-server-connection-info-includes.md)]

## <a name="create-the-project"></a>Creación del proyecto

1. Desde el símbolo del sistema, cree un nuevo proyecto de Maven llamado *sqltest*.

    ```bash
    mvn archetype:generate "-DgroupId=com.sqldbsamples" "-DartifactId=sqltest" "-DarchetypeArtifactId=maven-archetype-quickstart" "-Dversion=1.0.0" --batch-mode
    ```

1. Cambie a la carpeta *sqltest* y abra *pom.xml* con el editor de texto. Agregue **Microsoft JDBC Driver para SQL Server** a las dependencias del proyecto mediante el código siguiente.

    ```xml
    <dependency>
        <groupId>com.microsoft.sqlserver</groupId>
        <artifactId>mssql-jdbc</artifactId>
        <version>7.0.0.jre8</version>
    </dependency>
    ```

1. También en *pom.xml*, agregue las siguientes propiedades al proyecto. Si no tiene una sección de propiedades, puede agregarla después de las dependencias.

   ```xml
   <properties>
       <maven.compiler.source>1.8</maven.compiler.source>
       <maven.compiler.target>1.8</maven.compiler.target>
   </properties>
   ```

1. Guarde y cierre el archivo *pom.xml*.

## <a name="add-code-to-query-database"></a>Adición de código a una base de datos de consulta

1. Ya debe tener un archivo denominado *App.java* en el proyecto Maven ubicado en:

   *..\sqltest\src\main\java\com\sqldbsamples\App.java*

1. Abra el archivo y reemplace el contenido por el código siguiente. Agregue los valores apropiados para el servidor, la base de datos, el usuario y la contraseña.

    ```java
    package com.sqldbsamples;

    import java.sql.Connection;
    import java.sql.Statement;
    import java.sql.PreparedStatement;
    import java.sql.ResultSet;
    import java.sql.DriverManager;

    public class App {

        public static void main(String[] args) {

            // Connect to database
            String hostName = "your_server.database.windows.net"; // update me
            String dbName = "your_database"; // update me
            String user = "your_username"; // update me
            String password = "your_password"; // update me
            String url = String.format("jdbc:sqlserver://%s:1433;database=%s;user=%s;password=%s;encrypt=true;"
                + "hostNameInCertificate=*.database.windows.net;loginTimeout=30;", hostName, dbName, user, password);
            Connection connection = null;

            try {
                connection = DriverManager.getConnection(url);
                String schema = connection.getSchema();
                System.out.println("Successful connection - Schema: " + schema);

                System.out.println("Query data example:");
                System.out.println("=========================================");

                // Create and execute a SELECT SQL statement.
                String selectSql = "SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName "
                    + "FROM [SalesLT].[ProductCategory] pc "  
                    + "JOIN [SalesLT].[Product] p ON pc.productcategoryid = p.productcategoryid";

                try (Statement statement = connection.createStatement();
                ResultSet resultSet = statement.executeQuery(selectSql)) {

                    // Print results from select statement
                    System.out.println("Top 20 categories:");
                    while (resultSet.next())
                    {
                        System.out.println(resultSet.getString(1) + " "
                            + resultSet.getString(2));
                    }
                    connection.close();
                }
            }
            catch (Exception e) {
                e.printStackTrace();
            }
        }
    }
    ```

   > [!NOTE]
   > El ejemplo de código usa la base de datos de ejemplo para SQL Azure **AdventureWorksLT**.

## <a name="run-the-code"></a>Ejecución del código

1. En el símbolo del sistema, ejecute la aplicación.

    ```bash
    mvn package -DskipTests
    mvn -q exec:java "-Dexec.mainClass=com.sqldbsamples.App"
    ```

1. Compruebe que se han devuelto las 20 primeras filas y cierre la ventana de la aplicación.

## <a name="next-steps"></a>Pasos siguientes

- [Diseño de su primera base de datos SQL de Azure](sql-database-design-first-database.md)  

- [Controlador JDBC de Microsoft para SQL Server](https://github.com/microsoft/mssql-jdbc)  

- [Informe de los problemas y realización de preguntas](https://github.com/microsoft/mssql-jdbc/issues)  
