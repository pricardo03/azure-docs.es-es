---
title: Implementar una solución distribuida geográficamente de Azure SQL Database| Microsoft Docs
description: Aprenda a configurar su base de datos y su aplicación de Azure SQL Database para realizar conmutación por error a una base de datos replicada y probar una conmutación por error.
services: sql-database
ms.service: sql-database
ms.subservice: high-availability
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: anosov1960
ms.author: sashan
ms.reviewer: mathoma, carlrab
manager: craigg
ms.date: 03/12/2019
ms.openlocfilehash: 6022c016b83ffe1362db4d826a5ee4397afd4128
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60338996"
---
# <a name="tutorial-implement-a-geo-distributed-database"></a>Tutorial: Implementar una base de datos distribuida geográficamente

Configure una base de datos de Azure SQL y una aplicación para realizar conmutación por error a una región remota y probar un plan de conmutación por error. Aprenderá a:

> [!div class="checklist"]
> - Cree un [grupo de conmutación por error](sql-database-auto-failover-group.md).
> - Ejecutar una aplicación de Java para consultar una base de datos de Azure SQL
> - Conmutación por error de prueba

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]
> [!IMPORTANT]
> El módulo de PowerShell de Azure Resource Manager es compatible aún con Azure SQL Database, pero todo el desarrollo futuro es para el módulo Az.Sql. Para estos cmdlets, consulte [AzureRM.Sql](https://docs.microsoft.com/powershell/module/AzureRM.Sql/). Los argumentos para los comandos en el módulo de Az y en los módulos AzureRm son esencialmente idénticos.

Para completar el tutorial, asegúrese de que instaló los elementos siguientes:

- [Azure PowerShell](/powershell/azureps-cmdlets-docs)
- Una base de datos de Azure SQL. Para crear uno, use:
  - [Portal](sql-database-single-database-get-started.md)
  - [CLI](sql-database-cli-samples.md)
  - [PowerShell](sql-database-powershell-samples.md)

  > [!NOTE]
  > El tutorial usa la base de datos de ejemplo de *AdventureWorksLT*.

- Java y Maven, consulte [Build an app using SQL Server](https://www.microsoft.com/sql-server/developer-get-started/)(Compilar una aplicación con SQL Server), resalte **Java**, seleccione su entorno y, después, siga los pasos.

> [!IMPORTANT]
> Asegúrese de configurar las reglas de firewall para usar la dirección IP pública del equipo en el que está realizando los pasos de este tutorial. Las reglas de firewall a nivel de base de datos se replicarán automáticamente al servidor secundario.
>
> Para obtener más información, vea [Create a database-level firewall rule](/sql/relational-databases/system-stored-procedures/sp-set-database-firewall-rule-azure-sql-database) (Creación de una regla de firewall a nivel de base de datos) o, para determinar la dirección IP que usa la regla de firewall a nivel de base de datos para el equipo, vea [Create a server-level firewall](sql-database-server-level-firewall-rule.md) (Crear un firewall de nivel de servidor).  

## <a name="create-a-failover-group"></a>Creación de un grupo de conmutación por error

Con Azure PowerShell, cree [grupos de conmutación por error](sql-database-auto-failover-group.md) entre un servidor SQL Azure existente y uno nuevo en otra región. A continuación, agregue la base de datos de ejemplo al grupo de conmutación por error.

> [!IMPORTANT]
> [!INCLUDE [sample-powershell-install](../../includes/sample-powershell-install-no-ssh.md)]

Para crear un grupo de conmutación por error, ejecute el siguiente script:

   ```powershell
    # Set variables for your server and database
    $adminlogin = "<your admin>"
    $password = "<your password>"
    $myresourcegroupname = "<your resource group name>"
    $mylocation = "<your resource group location>"
    $myservername = "<your existing server name>"
    $mydatabasename = "<your database name>"
    $mydrlocation = "<your disaster recovery location>"
    $mydrservername = "<your disaster recovery server name>"
    $myfailovergroupname = "<your globally unique failover group name>"

    # Create a backup server in the failover region
    New-AzSqlServer -ResourceGroupName $myresourcegroupname `
       -ServerName $mydrservername `
       -Location $mydrlocation `
       -SqlAdministratorCredentials $(New-Object -TypeName System.Management.Automation.PSCredential `
          -ArgumentList $adminlogin, $(ConvertTo-SecureString -String $password -AsPlainText -Force))

    # Create a failover group between the servers
    New-AzSqlDatabaseFailoverGroup `
       –ResourceGroupName $myresourcegroupname `
       -ServerName $myservername `
       -PartnerServerName $mydrservername  `
       –FailoverGroupName $myfailovergroupname `
       –FailoverPolicy Automatic `
       -GracePeriodWithDataLossHours 2

    # Add the database to the failover group
    Get-AzSqlDatabase `
       -ResourceGroupName $myresourcegroupname `
       -ServerName $myservername `
       -DatabaseName $mydatabasename | `
     Add-AzSqlDatabaseToFailoverGroup `
       -ResourceGroupName $myresourcegroupname `
       -ServerName $myservername `
       -FailoverGroupName $myfailovergroupname
   ```

Las opciones de configuración de replicación geográfica también pueden cambiarse en Azure Portal mediante la selección de la base de datos y, después, **Configuración** > **Replicación geográfica**.

![Configuración de la replicación geográfica](./media/sql-database-implement-geo-distributed-database/geo-replication.png)

## <a name="run-the-sample-project"></a>Ejecución del proyecto de ejemplo

1. En la consola, cree un proyecto de Maven con el siguiente comando:

   ```bash
   mvn archetype:generate "-DgroupId=com.sqldbsamples" "-DartifactId=SqlDbSample" "-DarchetypeArtifactId=maven-archetype-quickstart" "-Dversion=1.0.0"
   ```

1. Escriba **Y** y presione **ENTRAR**.

1. Cambie los directorios al nuevo proyecto.

   ```bash
   cd SqlDbSample
   ```

1. Con el editor que prefiera, abra el archivo *pom.xml* de la carpeta del proyecto.

1. Agregue la dependencia Microsoft JDBC Driver para SQL Server mediante la adición de la sección `dependency` siguiente. La dependencia se debe pegar en la sección `dependencies` de mayor tamaño.

   ```xml
   <dependency>
     <groupId>com.microsoft.sqlserver</groupId>
     <artifactId>mssql-jdbc</artifactId>
    <version>6.1.0.jre8</version>
   </dependency>
   ```

1. Especifique la versión de Java mediante la adición de la sección `properties` después de la sección `dependencies`:

   ```xml
   <properties>
     <maven.compiler.source>1.8</maven.compiler.source>
     <maven.compiler.target>1.8</maven.compiler.target>
   </properties>
   ```

1. Admita archivos de manifiesto agregando la sección `build` después de la sección `properties`:

   ```xml
   <build>
     <plugins>
       <plugin>
         <groupId>org.apache.maven.plugins</groupId>
         <artifactId>maven-jar-plugin</artifactId>
         <version>3.0.0</version>
         <configuration>
           <archive>
             <manifest>
               <mainClass>com.sqldbsamples.App</mainClass>
             </manifest>
           </archive>
        </configuration>
       </plugin>
     </plugins>
   </build>
   ```

1. Guarde y cierre el archivo *pom.xml*.

1. Abra el archivo *App.java* ubicado en ..\SqlDbSample\src\main\java\com\sqldbsamples y sustituya el contenido por el código siguiente:

   ```java
   package com.sqldbsamples;

   import java.sql.Connection;
   import java.sql.Statement;
   import java.sql.PreparedStatement;
   import java.sql.ResultSet;
   import java.sql.Timestamp;
   import java.sql.DriverManager;
   import java.util.Date;
   import java.util.concurrent.TimeUnit;

   public class App {

      private static final String FAILOVER_GROUP_NAME = "<your failover group name>";  // add failover group name
  
      private static final String DB_NAME = "<your database>";  // add database name
      private static final String USER = "<your admin>";  // add database user
      private static final String PASSWORD = "<your password>";  // add database password

      private static final String READ_WRITE_URL = String.format("jdbc:" +
         "sqlserver://%s.database.windows.net:1433;database=%s;user=%s;password=%s;encrypt=true;" +
         "hostNameInCertificate=*.database.windows.net;loginTimeout=30;", +
         FAILOVER_GROUP_NAME, DB_NAME, USER, PASSWORD);
      private static final String READ_ONLY_URL = String.format("jdbc:" +
         "sqlserver://%s.secondary.database.windows.net:1433;database=%s;user=%s;password=%s;encrypt=true;" +
         "hostNameInCertificate=*.database.windows.net;loginTimeout=30;", +
         FAILOVER_GROUP_NAME, DB_NAME, USER, PASSWORD);

      public static void main(String[] args) {
         System.out.println("#######################################");
         System.out.println("## GEO DISTRIBUTED DATABASE TUTORIAL ##");
         System.out.println("#######################################");
         System.out.println("");

         int highWaterMark = getHighWaterMarkId();

         try {
            for(int i = 1; i < 1000; i++) {
                //  loop will run for about 1 hour
                System.out.print(i + ": insert on primary " +
                   (insertData((highWaterMark + i))?"successful":"failed"));
                TimeUnit.SECONDS.sleep(1);
                System.out.print(", read from secondary " +
                   (selectData((highWaterMark + i))?"successful":"failed") + "\n");
                TimeUnit.SECONDS.sleep(3);
            }
         } catch(Exception e) {
            e.printStackTrace();
      }
   }

   private static boolean insertData(int id) {
      // Insert data into the product table with a unique product name so we can find the product again
      String sql = "INSERT INTO SalesLT.Product " +
         "(Name, ProductNumber, Color, StandardCost, ListPrice, SellStartDate) VALUES (?,?,?,?,?,?);";

      try (Connection connection = DriverManager.getConnection(READ_WRITE_URL);
              PreparedStatement pstmt = connection.prepareStatement(sql)) {
         pstmt.setString(1, "BrandNewProduct" + id);
         pstmt.setInt(2, 200989 + id + 10000);
         pstmt.setString(3, "Blue");
         pstmt.setDouble(4, 75.00);
         pstmt.setDouble(5, 89.99);
         pstmt.setTimestamp(6, new Timestamp(new Date().getTime()));
         return (1 == pstmt.executeUpdate());
      } catch (Exception e) {
         return false;
      }
   }

   private static boolean selectData(int id) {
      // Query the data previously inserted into the primary database from the geo replicated database
      String sql = "SELECT Name, Color, ListPrice FROM SalesLT.Product WHERE Name = ?";

      try (Connection connection = DriverManager.getConnection(READ_ONLY_URL);
              PreparedStatement pstmt = connection.prepareStatement(sql)) {
         pstmt.setString(1, "BrandNewProduct" + id);
         try (ResultSet resultSet = pstmt.executeQuery()) {
            return resultSet.next();
         }
      } catch (Exception e) {
         return false;
      }
   }

   private static int getHighWaterMarkId() {
      // Query the high water mark id stored in the table to be able to make unique inserts
      String sql = "SELECT MAX(ProductId) FROM SalesLT.Product";
      int result = 1;
      try (Connection connection = DriverManager.getConnection(READ_WRITE_URL);
              Statement stmt = connection.createStatement();
              ResultSet resultSet = stmt.executeQuery(sql)) {
         if (resultSet.next()) {
             result =  resultSet.getInt(1);
            }
         } catch (Exception e) {
          e.printStackTrace();
         }
         return result;
      }
   }
   ```

1. Guarde y cierre el archivo*App.java*.

1. En la consola de comandos, ejecute el siguiente comando:

   ```bash
   mvn package
   ```

1. Inicie la aplicación que se ejecutará durante aproximadamente una hora hasta que se detenga manualmente, lo que le proporciona tiempo para ejecutar la prueba de conmutación por error.

   ```bash
   mvn -q -e exec:java "-Dexec.mainClass=com.sqldbsamples.App"
   ```

   ```output
   #######################################
   ## GEO DISTRIBUTED DATABASE TUTORIAL ##
   #######################################

   1. insert on primary successful, read from secondary successful
   2. insert on primary successful, read from secondary successful
   3. insert on primary successful, read from secondary successful
   ...
   ```

## <a name="test-failover"></a>Conmutación por error de prueba

Ejecute los siguientes scripts para simular una conmutación por error y observar los resultados de la aplicación. Observe cómo se producirán errores en algunas inserciones y selecciones durante la migración de la base de datos.

También puede comprobar el rol del servidor de recuperación ante desastres durante la prueba con el siguiente comando:

   ```powershell
   (Get-AzSqlDatabaseFailoverGroup `
      -FailoverGroupName $myfailovergroupname `
      -ResourceGroupName $myresourcegroupname `
      -ServerName $mydrservername).ReplicationRole
   ```

Para probar una conmutación por error:

1. Inicie la conmutación por error manual del grupo de conmutación por error:

   ```powershell
   Switch-AzSqlDatabaseFailoverGroup `
      -ResourceGroupName $myresourcegroupname `
      -ServerName $mydrservername `
      -FailoverGroupName $myfailovergroupname
   ```

1. Revierta el grupo de conmutación por error al servidor principal:

   ```powershell
   Switch-AzSqlDatabaseFailoverGroup `
      -ResourceGroupName $myresourcegroupname `
      -ServerName $myservername `
      -FailoverGroupName $myfailovergroupname
   ```

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, configuró una base de datos de Azure SQL y una aplicación para realizar la conmutación por error a una región remota y, después, probó el plan de conmutación por error. Ha aprendido a:

> [!div class="checklist"]
> - Crear un grupo de conmutación por error de replicación geográfica
> - Ejecutar una aplicación de Java para consultar una base de datos de Azure SQL
> - Conmutación por error de prueba

Avance al siguiente tutorial sobre cómo migrar mediante DMS.

> [!div class="nextstepaction"]
> [Migración de SQL Server a una instancia administrada de Azure SQL Database mediante DMS](../dms/tutorial-sql-server-to-managed-instance.md)
