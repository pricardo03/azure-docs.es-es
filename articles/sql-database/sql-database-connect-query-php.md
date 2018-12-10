---
title: Uso de PHP para consultar una base de datos Azure SQL | Microsoft Docs
description: Cómo usar PHP para crear un programa que se conecte a una base de datos Azure SQL y realice consultas ella mediante instrucciones T-SQL.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.devlang: php
ms.topic: quickstart
author: CarlRabeler
ms.author: carlrab
ms.reviewer: v-masebo
manager: craigg
ms.date: 11/28/2018
ms.openlocfilehash: be3ac9fab6c89c65ad9673811e108cefe2c80d00
ms.sourcegitcommit: 333d4246f62b858e376dcdcda789ecbc0c93cd92
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/01/2018
ms.locfileid: "52724254"
---
# <a name="quickstart-use-php-to-query-an-azure-sql-database"></a>Guía de inicio rápido: Uso de PHP para consultar una base de datos de Azure SQL Database

En este artículo se muestra cómo usar [PHP](http://php.net/manual/en/intro-whatis.php) para conectarse a una base de datos de Azure SQL. Luego puede usar instrucciones de T-SQL para consultar los datos.

## <a name="prerequisites"></a>Requisitos previos

Para completar este ejemplo, asegúrese de que cumple los siguientes requisitos previos:

[!INCLUDE [prerequisites-create-db](../../includes/sql-database-connect-query-prerequisites-create-db-includes.md)]

- Una [regla de firewall de nivel de servidor](sql-database-get-started-portal-firewall.md) para la dirección IP pública del equipo que usa.

- Software relacionado con PHP instalado para el sistema operativo:

    - **MacOS**, instale Homebrew y PHP, el controlador ODBC y SQLCMD y, a continuación, el controlador PHP para SQL Server. Consulte [pasos 1.2, 1.3 y 2.1](https://www.microsoft.com/sql-server/developer-get-started/php/mac/).

    - **Ubuntu**, instale PHP y otros paquetes necesarios y, a continuación, instale el controlador PHP para SQL Server. Consulte [pasos 1.2 y 2.1](https://www.microsoft.com/sql-server/developer-get-started/php/ubuntu/).

    - **Windows**, instale PHP para IIS Express y Chocolatey, y después instale el controlador ODBC y SQLCMD. Consulte [pasos 1.2 y 1.3](https://www.microsoft.com/sql-server/developer-get-started/php/windows/).

## <a name="get-database-connection"></a>Obtención de la conexión de la base de datos

[!INCLUDE [prerequisites-server-connection-info](../../includes/sql-database-connect-query-prerequisites-server-connection-info-includes.md)]

## <a name="add-code-to-query-database"></a>Adición de código a una base de datos de consulta

1. En el editor de texto, cree un nuevo archivo, *sqltest.php*.  

1. Reemplace su contenido por el código siguiente. Agregue los valores apropiados para el servidor, la base de datos, el usuario y la contraseña.

   ```PHP
   <?php
       $serverName = "your_server.database.windows.net"; // update me
       $connectionOptions = array(
           "Database" => "your_database", // update me
           "Uid" => "your_username", // update me
           "PWD" => "your_password" // update me
       );
       //Establishes the connection
       $conn = sqlsrv_connect($serverName, $connectionOptions);
       $tsql= "SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName
            FROM [SalesLT].[ProductCategory] pc
            JOIN [SalesLT].[Product] p
            ON pc.productcategoryid = p.productcategoryid";
       $getResults= sqlsrv_query($conn, $tsql);
       echo ("Reading data from table" . PHP_EOL);
       if ($getResults == FALSE)
           echo (sqlsrv_errors());
       while ($row = sqlsrv_fetch_array($getResults, SQLSRV_FETCH_ASSOC)) {
        echo ($row['CategoryName'] . " " . $row['ProductName'] . PHP_EOL);
       }
       sqlsrv_free_stmt($getResults);
   ?>
   ```

## <a name="run-the-code"></a>Ejecución del código

1. En el símbolo del sistema, ejecute la aplicación.

   ```bash
   php sqltest.php
   ```

1. Compruebe que se han devuelto las 20 primeras filas y cierre la ventana de la aplicación.

## <a name="next-steps"></a>Pasos siguientes

- [Diseño de su primera base de datos SQL de Azure](sql-database-design-first-database.md)

- [Controladores de Microsoft para PHP para SQL Server](https://github.com/Microsoft/msphpsql/)

- [Informe de los problemas y realización de preguntas](https://github.com/Microsoft/msphpsql/issues)

- [Ejemplo de lógica de reintento: conexión resistente a SQL con PHP](/sql/connect/php/step-4-connect-resiliently-to-sql-with-php)
