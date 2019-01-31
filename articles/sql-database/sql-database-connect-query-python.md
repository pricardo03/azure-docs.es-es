---
title: Uso de Python para consultar Azure SQL Database | Microsoft Docs
description: En este tema se explica cómo usar Python para crear un programa que se conecta a Azure SQL Database y realiza consultas mediante instrucciones Transact-SQL.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: python
ms.topic: quickstart
author: CarlRabeler
ms.author: carlrab
ms.reviewer: ''
manager: craigg
ms.date: 01/28/2019
ms.openlocfilehash: b611eb02203c872e3497b5b7c12acddd9eab14c0
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2019
ms.locfileid: "55188395"
---
# <a name="quickstart-use-python-to-query-an-azure-sql-database"></a>Guía de inicio rápido: Uso de Python para consultar una base de datos SQL de Azure

 Esta guía de inicio rápido muestra cómo utilizar [Python](https://python.org) para conectarse a una base de datos SQL de Azure y utilizar instrucciones Transact-SQL para consultar los datos. Para obtener más detalles del SDK, consulte la documentación de [referencia](https://docs.microsoft.com/python/api/overview/azure/sql), el [repositorio de GitHub pyodbc](https://github.com/mkleehammer/pyodbc/wiki/) y un [ejemplo de pyodbc](https://github.com/mkleehammer/pyodbc/wiki/Getting-started).

## <a name="prerequisites"></a>Requisitos previos

Para completar esta guía de inicio rápido, asegúrese de que dispone de lo siguiente:

[!INCLUDE [prerequisites-create-db](../../includes/sql-database-connect-query-prerequisites-create-db-includes.md)]
  
- Python y el software relacionado para el sistema operativo:
  
  - **MacOS**: instale Homebrew y Python, instale el controlador ODBC y SQLCMD y, a continuación, instale el controlador Python para SQL Server. Consulte los pasos 1.2, 1.3 y 2.1 en [Create Python apps using SQL Server on macOS](https://www.microsoft.com/sql-server/developer-get-started/python/mac/) (Creación de aplicaciones de Python con SQL Server en macOS). Para más información, vea [Instalación de Microsoft ODBC Driver for SQL Server en Linux y macOS](https://docs.microsoft.com/sql/connect/odbc/linux-mac/installing-the-microsoft-odbc-driver-for-sql-server).
    
  - **Ubuntu**: instale Python y otros paquetes necesarios con `sudo apt-get install python python-pip gcc g++ build-essential`. Descargue e instale el controlador ODBC, SQLCMD y el controlador de Python para SQL Server. Para obtener instrucciones, vea [Configurar el entorno de desarrollo para el desarrollo de Python pyodbc](/sql/connect/python/pyodbc/step-1-configure-development-environment-for-pyodbc-python-development#linux).
    
  - **Windows**: instale el controlador ODBC, SQLCMD y el controlador de Python para SQL Server. Para obtener instrucciones, vea [Configurar el entorno de desarrollo para el desarrollo de Python pyodbc](/sql/connect/python/pyodbc/step-1-configure-development-environment-for-pyodbc-python-development#windows).

## <a name="get-sql-server-connection-information"></a>Obtención de información de conexión de SQL Server

[!INCLUDE [prerequisites-server-connection-info](../../includes/sql-database-connect-query-prerequisites-server-connection-info-includes.md)]
    
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

- [Diseño de su primera base de datos SQL de Azure](sql-database-design-first-database.md)
- [Controladores de Microsoft para Python para SQL Server](https://docs.microsoft.com/sql/connect/python/python-driver-for-sql-server/)
- [Centro para desarrolladores de Python](https://azure.microsoft.com/develop/python/?v=17.23h)

