---
title: 'Inicio rápido: Uso de Python para consultar Azure SQL Database'
description: En este tema se explica cómo usar Python para crear un programa que se conecta a una base de datos de Azure SQL y realiza consultas mediante instrucciones Transact-SQL.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: seo-python-october2019
ms.devlang: python
ms.topic: quickstart
author: stevestein
ms.author: sstein
ms.reviewer: ''
ms.date: 03/25/2019
ms.openlocfilehash: 32861fa8f8756c4e8d30c055582789d41a92fe7c
ms.sourcegitcommit: 1d0b37e2e32aad35cc012ba36200389e65b75c21
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2019
ms.locfileid: "72331856"
---
# <a name="quickstart-use-python-to-query-an-azure-sql-database"></a>Guía de inicio rápido: Uso de Python para consultar una base de datos de Azure SQL

 Este artículo muestra cómo se usa [Python](https://python.org) para conectarse a una base de datos de Azure SQL y usar instrucciones Transact-SQL para consultar los datos. Para obtener más detalles del SDK, consulte la documentación de [referencia](https://docs.microsoft.com/python/api/overview/azure/sql), el [repositorio de GitHub pyodbc](https://github.com/mkleehammer/pyodbc/wiki/) y un [ejemplo de pyodbc](https://github.com/mkleehammer/pyodbc/wiki/Getting-started).

## <a name="prerequisites"></a>Requisitos previos

Para completar esta guía de inicio rápido, asegúrese de que dispone de lo siguiente:

- Una base de datos de Azure SQL. Puede utilizar uno de estos inicios rápidos para crear y configurar una base de datos en Azure SQL Database:

  || Base de datos única | Instancia administrada |
  |:--- |:--- |:---|
  | Crear| [Portal](sql-database-single-database-get-started.md) | [Portal](sql-database-managed-instance-get-started.md) |
  || [CLI](scripts/sql-database-create-and-configure-database-cli.md) | [CLI](https://medium.com/azure-sqldb-managed-instance/working-with-sql-managed-instance-using-azure-cli-611795fe0b44) |
  || [PowerShell](scripts/sql-database-create-and-configure-database-powershell.md) | [PowerShell](scripts/sql-database-create-configure-managed-instance-powershell.md) |
  | Configuración | [Regla de firewall de IP en el nivel de servidor](sql-database-server-level-firewall-rule.md)| [Conectividad desde una máquina virtual](sql-database-managed-instance-configure-vm.md)|
  |||[Conectividad desde el sitio](sql-database-managed-instance-configure-p2s.md)
  |Carga de datos|Adventure Works cargado por inicio rápido|[Restauración de World Wide Importers](sql-database-managed-instance-get-started-restore.md)
  |||Restauración o importación de Adventure Works a partir del archivo [BACPAC](sql-database-import.md) desde [GitHub](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/adventure-works)|
  |||

  > [!IMPORTANT]
  > Los scripts de este artículo se escriben para utilizar la base de datos Adventure Works. Con una instancia administrada, debe importar la base de datos Adventure Works en una base de datos de instancia o modificar los scripts de este artículo para utilizar la base de datos Wide World Importers.
  
- Python y el software relacionado para el sistema operativo:
  
  - **MacOS**: instale Homebrew y Python, instale el controlador ODBC y SQLCMD y, a continuación, instale el controlador Python para SQL Server. Consulte los pasos 1.2, 1.3 y 2.1 en [Create Python apps using SQL Server on macOS](https://www.microsoft.com/sql-server/developer-get-started/python/mac/) (Creación de aplicaciones de Python con SQL Server en macOS). Para más información, vea [Instalación de Microsoft ODBC Driver for SQL Server en Linux y macOS](https://docs.microsoft.com/sql/connect/odbc/linux-mac/installing-the-microsoft-odbc-driver-for-sql-server).

  - **Ubuntu**: instale Python y otros paquetes necesarios con `sudo apt-get install python python-pip gcc g++ build-essential`. Descargue e instale el controlador ODBC, SQLCMD y el controlador de Python para SQL Server. Para obtener instrucciones, vea [Configurar el entorno de desarrollo para el desarrollo de Python pyodbc](/sql/connect/python/pyodbc/step-1-configure-development-environment-for-pyodbc-python-development#linux).

  - **Windows**: instale el controlador ODBC, SQLCMD y el controlador de Python para SQL Server. Para obtener instrucciones, vea [Configurar el entorno de desarrollo para el desarrollo de Python pyodbc](/sql/connect/python/pyodbc/step-1-configure-development-environment-for-pyodbc-python-development#windows).

## <a name="get-sql-server-connection-information"></a>Obtención de información de conexión de SQL Server

Obtención de la información de conexión necesaria para conectarse a Azure SQL Database. En los procedimientos siguientes, necesitará el nombre completo del servidor o nombre de host, el nombre de la base de datos y la información de inicio de sesión.

1. Inicie sesión en el [Azure Portal](https://portal.azure.com/).

2. Vaya a las páginas **Bases de datos SQL** o **Instancias administradas de SQL**.

3. En la página **Información general**, revise el nombre completo del servidor junto a **Nombre del servidor** para una única base de datos o el nombre completo del servidor junto a **Host** para una instancia administrada. Para copiar el nombre del servidor o nombre de host, mantenga el cursor sobre él y seleccione el icono **Copiar**.

## <a name="create-code-to-query-your-sql-database"></a>Creación del código para consultar la base de datos SQL 

1. En el editor de texto, cree un archivo llamado *sqltest.py*.  
   
1. Agregue el siguiente código. Sustituya sus propios valores de \<servidor>, \<base de datos>, \<nombre de usuario> y \<contraseña>.
   
   >[!IMPORTANT]
   >El código de este ejemplo usa los datos de AdventureWorksLT de ejemplo, que puede elegir como origen al crear la base de datos. Si la base de datos tiene datos distintos, use las tablas de su propia base de datos en la consulta SELECT. 
   
   ```python
   import pyodbc
   server = '<server>.database.windows.net'
   database = '<database>'
   username = '<username>'
   password = '<password>'
   driver= '{ODBC Driver 17 for SQL Server}'
   cnxn = pyodbc.connect('DRIVER='+driver+';SERVER='+server+';PORT=1433;DATABASE='+database+';UID='+username+';PWD='+ password)
   cursor = cnxn.cursor()
   cursor.execute("SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName FROM [SalesLT].[ProductCategory] pc JOIN [SalesLT].[Product] p ON pc.productcategoryid = p.productcategoryid")
   row = cursor.fetchone()
   while row:
       print (str(row[0]) + " " + str(row[1]))
       row = cursor.fetchone()
   ```
   

## <a name="run-the-code"></a>Ejecución del código

1. En el símbolo del sistema, ejecute el siguiente comando:

   ```cmd
   python sqltest.py
   ```

1. Compruebe que se devuelven las 20 primeras filas de categoría o productos y, después, cierre la ventana Comandos.

## <a name="next-steps"></a>Pasos siguientes

- [Diseño de su primera base de datos de Azure SQL](sql-database-design-first-database.md)
- [Controladores de Microsoft para Python para SQL Server](https://docs.microsoft.com/sql/connect/python/python-driver-for-sql-server/)
- [Centro para desarrolladores de Python](https://azure.microsoft.com/develop/python/?v=17.23h)

