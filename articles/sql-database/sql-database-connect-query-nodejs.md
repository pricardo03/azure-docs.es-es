---
title: Uso de Node.js para consultar Azure SQL Database | Microsoft Docs
description: Cómo usar Node.js para crear un programa que se conecte a Azure SQL Database y realice consultas en él mediante instrucciones T-SQL.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.devlang: nodejs
ms.topic: quickstart
author: CarlRabeler
ms.author: carlrab
ms.reviewer: v-masebo
manager: craigg
ms.date: 11/26/2018
ms.openlocfilehash: c6e518f644e8fc43db58ef481312f44b2071a0dc
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2019
ms.locfileid: "55194311"
---
# <a name="quickstart-use-nodejs-to-query-an-azure-sql-database"></a>Inicio rápido: Uso de Node.js para consultar una base de datos SQL de Azure

En este artículo se muestra cómo usar [Node.js](https://nodejs.org) para conectarse a una instancia de Azure SQL Database. Luego puede usar instrucciones de T-SQL para consultar los datos.

## <a name="prerequisites"></a>Requisitos previos

Para completar este ejemplo, asegúrese de que cumple los siguientes requisitos previos:

[!INCLUDE [prerequisites-create-db](../../includes/sql-database-connect-query-prerequisites-create-db-includes.md)]

- Software relacionado con Node.js para el sistema operativo:

  - **MacOS**, instale Homebrew y Node.js y, después, instale el controlador ODBC y SQLCMD. Consulte [pasos 1.2 y 1.3](https://www.microsoft.com/sql-server/developer-get-started/node/mac/).
  
  - **Ubuntu**, instale Node.js y, después, instale el controlador ODBC y SQLCMD. Consulte [pasos 1.2 y 1.3](https://www.microsoft.com/sql-server/developer-get-started/node/ubuntu/).
  
  - **Windows**, instale Chocolatey y Node.js y, después, instale el controlador ODBC y SQLCMD. Consulte [pasos 1.2 y 1.3](https://www.microsoft.com/sql-server/developer-get-started/node/windows/).

## <a name="get-database-connection"></a>Obtención de la conexión de la base de datos

[!INCLUDE [prerequisites-server-connection-info](../../includes/sql-database-connect-query-prerequisites-server-connection-info-includes.md)]

> [!IMPORTANT]
> Debe tener una regla de firewall activa para la dirección IP pública del equipo en el que sigue este tutorial. Si se encuentra en un equipo diferente o tiene una dirección IP pública diferente, cree una [regla de firewall de nivel de servidor mediante Azure Portal](sql-database-get-started-portal-firewall.md).

## <a name="create-the-project"></a>Creación del proyecto

Abra un símbolo del sistema y cree una carpeta denominada *sqltest*. Navegue hasta la carpeta que ha creado y ejecute el siguiente comando:

  ```bash
  npm init -y
  npm install tedious
  npm install async
  ```

## <a name="add-code-to-query-database"></a>Adición de código a una base de datos de consulta

1. En el editor de texto, cree un archivo, *sqltest.py*.

1. Reemplace su contenido por el código siguiente. Agregue los valores apropiados para el servidor, la base de datos, el usuario y la contraseña.

    ```js
    var Connection = require('tedious').Connection;
    var Request = require('tedious').Request;

    // Create connection to database
    var config =
    {
        userName: 'your_username', // update me
        password: 'your_password', // update me
        server: 'your_server.database.windows.net', // update me
        options:
        {
            database: 'your_database', //update me
            encrypt: true
        }
    }
    var connection = new Connection(config);

    // Attempt to connect and execute queries if connection goes through
    connection.on('connect', function(err)
        {
            if (err)
            {
                console.log(err)
            }
            else
            {
                queryDatabase()
            }
        }
    );

    function queryDatabase()
    {
        console.log('Reading rows from the Table...');

        // Read all rows from table
        var request = new Request(
            "SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName FROM [SalesLT].[ProductCategory] pc "
                + "JOIN [SalesLT].[Product] p ON pc.productcategoryid = p.productcategoryid",
            function(err, rowCount, rows)
            {
                console.log(rowCount + ' row(s) returned');
                process.exit();
            }
        );

        request.on('row', function(columns) {
            columns.forEach(function(column) {
                console.log("%s\t%s", column.metadata.colName, column.value);
            });
        });
        connection.execSql(request);
    }
    ```

> [!NOTE]
> El ejemplo de código usa la base de datos de ejemplo para SQL Azure **AdventureWorksLT**.

## <a name="run-the-code"></a>Ejecución del código

1. En el símbolo del sistema, ejecute el programa.

    ```bash
    node sqltest.js
    ```

1. Compruebe que se han devuelto las 20 primeras filas y cierre la ventana de la aplicación.

## <a name="next-steps"></a>Pasos siguientes

- [Controlador de Microsoft Node.js para SQL Server](/sql/connect/node-js/node-js-driver-for-sql-server)

- Conéctese y realice consultas en Windows, Linux y macOS con [.NET Core](sql-database-connect-query-dotnet-core.md), [Visual Studio Code](sql-database-connect-query-vscode.md) o [SSMS](sql-database-connect-query-ssms.md) (solo Windows)

- [Introducción a .NET Core en Windows, Linux y macOS mediante la línea de comandos](/dotnet/core/tutorials/using-with-xplat-cli)

- Diseño de la primera instancia de Azure SQL Database mediante [.NET](sql-database-design-first-database-csharp.md) o [SSMS](sql-database-design-first-database.md)
