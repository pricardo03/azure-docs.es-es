---
title: Uso de R para consultar Azure SQL Database
titleSuffix: Azure SQL Database Machine Learning Services (preview)
description: En este artículo se muestra cómo usar un script de R para conectarse a Azure SQL Database y realizar consultas mediante instrucciones Transact-SQL.
services: sql-database
ms.service: sql-database
ms.subservice: machine-learning
ms.custom: ''
ms.devlang: python
ms.topic: quickstart
author: garyericson
ms.author: garye
ms.reviewer: davidph, carlrab
manager: cgronlun
ms.date: 04/11/2019
ms.openlocfilehash: 2b1206e3087b0573736174d4eed502653d76f7a5
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/22/2019
ms.locfileid: "60001197"
---
# <a name="quickstart-use-r-to-query-an-azure-sql-database-preview"></a>Inicio rápido: Uso de R para consultar una base de datos de Azure SQL (versión preliminar)

 En esta guía de inicio rápido se muestra cómo se usa [R](https://www.r-project.org/) con Machine Learning Services para conectarse a una base de datos de Azure SQL y para que use instrucciones Transact-SQL para consultar los datos. Machine Learning Services es una característica de Azure SQL Database que se usa para ejecutar scripts de R en bases de datos. Para más información, consulte [Azure SQL Database Machine Learning Services con R (versión preliminar)](sql-database-machine-learning-services-overview.md).

[!INCLUDE[ml-preview-note](../../includes/sql-database-ml-preview-note.md)]

## <a name="prerequisites"></a>Requisitos previos

Para completar esta guía de inicio rápido, asegúrese de que dispone de lo siguiente:

- Una base de datos SQL de Azure. Puede utilizar uno de estos inicios rápidos para crear y configurar una base de datos en Azure SQL Database:

  || Base de datos única | Instancia administrada |
  |:--- |:--- |:---|
  | Crear| [Portal](sql-database-single-database-get-started.md) | [Portal](sql-database-managed-instance-get-started.md) |
  || [CLI](scripts/sql-database-create-and-configure-database-cli.md) | [CLI](https://medium.com/azure-sqldb-managed-instance/working-with-sql-managed-instance-using-azure-cli-611795fe0b44) |
  || [PowerShell](scripts/sql-database-create-and-configure-database-powershell.md) | [PowerShell](scripts/sql-database-create-configure-managed-instance-powershell.md) |
  | Configuración | [Regla de firewall de IP en el nivel de servidor](sql-database-server-level-firewall-rule.md)| [Conectividad desde una máquina virtual](sql-database-managed-instance-configure-vm.md)|
  |||[Conectividad desde el sitio](sql-database-managed-instance-configure-p2s.md)
  |||

- Machine Learning Services (con R) habilitado. Durante la versión preliminar pública, Microsoft le incorporará y habilitará el aprendizaje automático para la base de datos nueva o existente. Siga los pasos que se indican en [Suscríbase a la versión preliminar](sql-database-machine-learning-services-overview.md#signup).

- La versión más reciente de [SQL Server Management Studio (SSMS)](https://docs.microsoft.com/sql/ssms/sql-server-management-studio-ssms). Puede ejecutar los scripts de R con herramientas de consulta o de otro tipo de administración de base de datos, pero en este tutorial usará SSMS.

## <a name="get-sql-server-connection-information"></a>Obtención de información de conexión de SQL Server

Obtención de la información de conexión necesaria para conectarse a Azure SQL Database. En los procedimientos siguientes, necesitará el nombre completo del servidor o nombre de host, el nombre de la base de datos y la información de inicio de sesión.

1. Inicie sesión en el [Azure Portal](https://portal.azure.com/).

2. Vaya a las páginas **SQL Database** o **Instancias administradas de SQL**.

3. En la página **Información general**, revise el nombre completo del servidor junto a **Nombre del servidor** para una única base de datos o el nombre completo del servidor junto a **Host** para una instancia administrada. Para copiar el nombre del servidor o nombre de host, mantenga el cursor sobre él y seleccione el icono **Copiar**.

## <a name="create-code-to-query-your-sql-database"></a>Creación del código para consultar la base de datos SQL

1. Abra **SQL Server Management Studio** y conéctese a la base de datos SQL.

   Si necesita ayuda para conectarse, consulte [Inicio rápido: Uso de SQL Server Management Studio para conectarse a una base de datos de Azure SQL Database y realizar consultas en ella](sql-database-connect-query-ssms.md).

1. Pase el script de R completo al procedimiento almacenado [sp_execute_external_script](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql).

   El script se pasa mediante el argumento `@script`. Todo lo que hay dentro del argumento `@script` debe ser un código de R válido.

    ```sql
    EXECUTE sp_execute_external_script
    @language = N'R'
    , @script = N'OutputDataSet <- InputDataSet;'
    , @input_data_1 = N'SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName FROM [SalesLT].[ProductCategory] pc JOIN [SalesLT].[Product] p ON pc.productcategoryid = p.productcategoryid'
    ```

   > [!NOTE]
   > Si recibe algún error, es posible que la versión preliminar pública de Machine Learning Services (con R) no esté habilitada para la base de datos SQL. Vea los [requisitos previos](#prerequisites) anteriores.

## <a name="run-the-code"></a>Ejecución del código

1. Ejecute el procedimiento almacenado [sp_execute_external_script](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql).

1. Compruebe que se devuelven las 20 primeras filas de categoría o productos en la ventana de **mensajes**.

## <a name="next-steps"></a>Pasos siguientes

- [Diseño de su primera base de datos SQL de Azure](sql-database-design-first-database.md)
- [Machine Learning Services (con R) en Azure SQL Database](sql-database-machine-learning-services-overview.md)
- [Crear y ejecutar scripts de R sencillos en Azure SQL Database Machine Learning Services (versión preliminar)](sql-database-quickstart-r-create-script.md)
- [Escribir funciones de R avanzadas en Azure SQL Database Machine Learning Services (versión preliminar)](sql-database-machine-learning-services-functions.md)
