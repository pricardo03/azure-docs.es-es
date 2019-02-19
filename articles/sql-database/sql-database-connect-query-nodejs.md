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
ms.date: 02/12/2019
ms.openlocfilehash: 49fe9f51026c4cb096fd8248b53d2e5b5b574923
ms.sourcegitcommit: de81b3fe220562a25c1aa74ff3aa9bdc214ddd65
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2019
ms.locfileid: "56236029"
---
# <a name="quickstart-use-nodejs-to-query-an-azure-sql-database"></a>Inicio rápido: Uso de Node.js para consultar una base de datos SQL de Azure

En este artículo se muestra cómo usar [Node.js](https://nodejs.org) para conectarse a una instancia de Azure SQL Database. Luego puede usar instrucciones de T-SQL para consultar los datos.

## <a name="prerequisites"></a>Requisitos previos

Para completar este ejemplo, asegúrese de que cumple los siguientes requisitos previos:

- Una base de datos SQL de Azure. Puede utilizar uno de estos inicios rápidos para crear y configurar una base de datos en Azure SQL Database:

  || Base de datos única | Instancia administrada |
  |:--- |:--- |:---|
  | Crear| [Portal](sql-database-single-database-get-started.md) | [Portal](sql-database-managed-instance-get-started.md) |
  || [CLI](scripts/sql-database-create-and-configure-database-cli.md) | [CLI](https://medium.com/azure-sqldb-managed-instance/working-with-sql-managed-instance-using-azure-cli-611795fe0b44) |
  || [PowerShell](scripts/sql-database-create-and-configure-database-powershell.md) | [PowerShell](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2018/06/27/quick-start-script-create-azure-sql-managed-instance-using-powershell/) |
  | Configuración | [Regla de firewall de IP en el nivel de servidor](sql-database-server-level-firewall-rule.md)| [Conectividad desde una máquina virtual](sql-database-managed-instance-configure-vm.md)|
  |||[Conectividad desde el sitio](sql-database-managed-instance-configure-p2s.md)
  |Carga de datos|Adventure Works cargado por inicio rápido|[Restauración de World Wide Importers](sql-database-managed-instance-get-started-restore.md)
  |||Restauración o importación de Adventure Works a partir del archivo [BACPAC](sql-database-import.md) desde [GitHub](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/adventure-works)|
  |||

  > [!IMPORTANT]
  > Los scripts de este artículo se escriben para utilizar la base de datos Adventure Works. Con una instancia administrada, debe importar la base de datos Adventure Works a una base de datos de la instancia o modificar los scripts de este artículo para utilizar la base de datos Wide World Importers.


- Software relacionado con Node.js para el sistema operativo:

  - **MacOS**, instale Homebrew y Node.js y, después, instale el controlador ODBC y SQLCMD. Consulte [pasos 1.2 y 1.3](https://www.microsoft.com/sql-server/developer-get-started/node/mac/).
  
  - **Ubuntu**, instale Node.js y, después, instale el controlador ODBC y SQLCMD. Consulte [pasos 1.2 y 1.3](https://www.microsoft.com/sql-server/developer-get-started/node/ubuntu/).
  
  - **Windows**, instale Chocolatey y Node.js y, después, instale el controlador ODBC y SQLCMD. Consulte [pasos 1.2 y 1.3](https://www.microsoft.com/sql-server/developer-get-started/node/windows/).

## <a name="get-sql-server-connection-information"></a>Obtención de información de conexión de SQL Server

Obtención de la información de conexión necesaria para conectarse a Azure SQL Database. En los procedimientos siguientes, necesitará el nombre completo del servidor o nombre de host, el nombre de la base de datos y la información de inicio de sesión.

1. Inicie sesión en el [Azure Portal](https://portal.azure.com/).

2. Vaya a las páginas **SQL Database** o **Instancias administradas de SQL**.

3. En la página **Información general**, revise el nombre completo del servidor junto a **Nombre del servidor** para una única base de datos o el nombre completo del servidor junto a **Host** para una instancia administrada. Para copiar el nombre del servidor o nombre de host, mantenga el cursor sobre él y seleccione el icono **Copiar**. 

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
