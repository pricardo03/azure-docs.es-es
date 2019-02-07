---
title: Uso de Ruby para consultar Azure SQL Database | Microsoft Docs
description: En este tema se muestra cómo usar Ruby para crear un programa que se conecta a Azure SQL Database y realiza consultas mediante instrucciones Transact-SQL.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: ruby
ms.topic: quickstart
author: CarlRabeler
ms.author: carlrab
ms.reviewer: ''
manager: craigg
ms.date: 01/02/2019
ms.openlocfilehash: 30174051c0d26595c65a79724cf3d08465a4cab7
ms.sourcegitcommit: ba035bfe9fab85dd1e6134a98af1ad7cf6891033
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2019
ms.locfileid: "55566988"
---
# <a name="quickstart-use-ruby-to-query-an-azure-sql-database"></a>Inicio rápido: Uso de Ruby para consultar una base de datos SQL de Azure

Esta guía de inicio rápido muestra cómo se usa [Ruby](https://www.ruby-lang.org) para conectarse a una instancia de Azure SQL Database y consultar datos con instrucciones Transact-SQL.

## <a name="prerequisites"></a>Requisitos previos

Para completar este tutorial de inicio rápido, debe cumplir los siguientes requisitos previos:

[!INCLUDE [prerequisites-create-db](../../includes/sql-database-connect-query-prerequisites-create-db-includes.md)]
  
- Ruby y el software relacionado para el sistema operativo:
  
  - **MacOS**: instale Homebrew, rbenv y ruby-build, Ruby, FreeTDS y TinyTDS. Consulte los pasos 1.2, 1.3, 1.4, 1.5 y 2.1 del artículo sobre la [creación de aplicaciones de Ruby con SQL Server en macOS](https://www.microsoft.com/sql-server/developer-get-started/ruby/mac/).
  
  - **Ubuntu**: instale los requisitos previos de Ruby, rbenv y ruby-build, Ruby, FreeTDS y TinyTDS. Consulte los pasos 1.2, 1.3, 1.4, 1.5 y 2.1 en [Create Ruby apps using SQL Server on Ubuntu](https://www.microsoft.com/sql-server/developer-get-started/ruby/ubuntu/) (Creación de aplicaciones de Ruby con SQL Server en Ubuntu).
  
  - **Windows**: instale Ruby, Ruby Devkit y TinyTDS. Consulte [Configurar el entorno de desarrollo para el desarrollo Ruby](/sql/connect/ruby/step-1-configure-development-environment-for-ruby-development).

## <a name="get-sql-server-connection-information"></a>Obtención de información de conexión de SQL Server

[!INCLUDE [prerequisites-server-connection-info](../../includes/sql-database-connect-query-prerequisites-server-connection-info-includes.md)]

## <a name="create-code-to-query-your-sql-database"></a>Creación del código para consultar la base de datos SQL

1. En el editor de texto o de código, cree un archivo denominado *sqltest.rb*.
   
1. Agregue el siguiente código. Sustituya los valores de la instancia de Azure SQL Database por `<server>`, `<database>`, `<username>` y `<password>`.
   
   >[!IMPORTANT]
   >El código de este ejemplo usa los datos de AdventureWorksLT de ejemplo, que puede elegir como origen al crear la base de datos. Si la base de datos tiene datos distintos, use las tablas de su propia base de datos en la consulta SELECT. 
   
   ```ruby
   require 'tiny_tds'
   server = '<server>.database.windows.net'
   database = '<database>'
   username = '<username>'
   password = '<password>'
   client = TinyTds::Client.new username: username, password: password, 
       host: server, port: 1433, database: database, azure: true
   
   puts "Reading data from table"
   tsql = "SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName
           FROM [SalesLT].[ProductCategory] pc
           JOIN [SalesLT].[Product] p
           ON pc.productcategoryid = p.productcategoryid"
   result = client.execute(tsql)
   result.each do |row|
       puts row
   end
   ```

## <a name="run-the-code"></a>Ejecución del código

1. En el símbolo del sistema, ejecute el siguiente comando:

   ```bash
   ruby sqltest.rb
   ```
   
1. Compruebe que se devuelven las 20 primeras filas de categoría o productos de la base de datos. 

## <a name="next-steps"></a>Pasos siguientes
- [Diseño de su primera base de datos SQL de Azure](sql-database-design-first-database.md).
- [Repositorio de GitHub para TinyTDS](https://github.com/rails-sqlserver/tiny_tds).
- [Informe de los problemas y realización de preguntas sobre TinyTDS](https://github.com/rails-sqlserver/tiny_tds/issues).
- [Controlador de Ruby para SQL Server](https://docs.microsoft.com/sql/connect/ruby/ruby-driver-for-sql-server/).
