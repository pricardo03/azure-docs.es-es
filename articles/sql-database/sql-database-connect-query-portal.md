---
title: 'Azure Portal: Consulta de Azure SQL Database mediante el Editor de consultas | Microsoft Docs'
description: Aprenda a conectarse a SQL Database en Azure Portal mediante el Editor de consultas de SQL. A continuación, ejecute instrucciones de Transact-SQL (T-SQL) para consultar y editar los datos.
keywords: conectarse a sql database, azure portal, portal, editor de consultas
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: ''
ms.topic: quickstart
author: AyoOlubeko
ms.author: ayolubek
ms.reviewer: carlrab
manager: craigg
ms.date: 02/12/2019
ms.openlocfilehash: b2109afe6760eb87269a33ed74784149c2bf3e03
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2019
ms.locfileid: "58105801"
---
# <a name="quickstart-use-the-azure-portals-sql-query-editor-to-connect-and-query-data"></a>Inicio rápido: Uso del Editor de consultas SQL de Azure Portal para conectarse a datos y consultarlos

El Editor de consultas SQL es una herramienta del explorador de Azure Portal que proporciona una manera fácil de ejecutar consultas SQL en su base de datos de Azure SQL o almacenamiento de Azure SQL Data Warehouse. En este inicio rápido, usará el editor de consultas para conectarse a una base de datos SQL y ejecutar instrucciones Transact-SQL para consultar, insertar, actualizar y eliminar datos.

## <a name="prerequisites"></a>Requisitos previos

Para completar este tutorial, necesita:

- Una base de datos de Azure SQL. Puede utilizar uno de estos inicios rápidos para crear y configurar una base de datos en Azure SQL Database:

  || Base de datos única |
  |:--- |:--- |
  | Crear| [Portal](sql-database-single-database-get-started.md) | 
  || [CLI](scripts/sql-database-create-and-configure-database-cli.md) | 
  || [PowerShell](scripts/sql-database-create-and-configure-database-powershell.md) | 
  | Configuración | [Regla de firewall de IP en el nivel de servidor](sql-database-server-level-firewall-rule.md)| 
  |||

> [!NOTE]
> Asegúrese de que la opción **Permitir el acceso a servicios de Azure** esté establecida en **Activado** en la configuración de firewall de SQL Server. Esta opción proporciona al Editor de consultas SQL acceso a las bases de datos y los almacenamientos de datos de datos.

## <a name="sign-in-the-azure-portal"></a>Inicio de sesión en Azure Portal

Inicie sesión en el [Azure Portal](https://portal.azure.com/).

## <a name="connect-using-sql-authentication"></a>Conexión con la autenticación de SQL

1. Seleccione **Bases de datos SQL** en el menú izquierdo y, a continuación, elija **mySampleDatabase**.

2. En el menú izquierdo, busque y seleccione **Editor de consultas (versión preliminar)**. Aparece la página **Iniciar sesión**.

    ![buscar editor de consultas](./media/sql-database-connect-query-portal/find-query-editor.PNG)

3. En el menú desplegable **Tipo de autorización**, seleccione **Autenticación de SQL Server** y escriba el identificador de usuario y la contraseña de la cuenta de administrador del servidor usada para crear la base de datos.

    ![iniciar sesión](./media/sql-database-connect-query-portal/login-menu.png) 

4. Seleccione **Aceptar**.


## <a name="connect-using-azure-active-directory"></a>Clientes que usan Azure Active Directory

La configuración de un administrador de Active Directory (AD) le permite usar una identidad única para iniciar sesión en Azure Portal y en la base de datos SQL. Siga los pasos que figuran a continuación para configurar un administrador de AD para su servidor SQL Server.

> [!NOTE]
> * Las cuentas de correo electrónico (por ejemplo, outlook.com, gmail.com, yahoo.com, etc.) aún no se admiten como administradores de AD. Elija un usuario creado de forma nativa en Azure AD o que esté federada en Azure AD.
> * El iniciar sesión del administrador de Azure AD no funciona con cuentas que tengan habilitada la autenticación en 2 fases.

1. Seleccione **Todos los recursos** en el menú izquierdo y, a continuación, elija el servidor SQL server.

2. En el menú **Configuración** del servidor SQL Server, seleccione **Administrador de Active Directory**.

3. En la barra de herramientas de la página de administradores de AD, seleccione **Establecer administrador** y elija el usuario o grupo como el administrador de AD.

    ![seleccione active directory](./media/sql-database-connect-query-portal/select-active-directory.png)

4. En la barra de herramientas de la página de administradores de AD, seleccione **Guardar**.

5. Navegue hasta la base datos **mySampleDatabase** y, en el menú izquierdo, seleccione **Editor de consultas (versión preliminar)**. Aparece la página **Iniciar sesión**. Si es administrador de AD, en el lado derecho, en **Inicio de sesión único de Active Directory**, aparece un mensaje que indica que ha iniciado sesión. 
   
6. Seleccione **Aceptar**.


## <a name="view-data"></a>Visualización de datos

1. Una vez autenticado, pegue la siguiente consulta SQL en el Editor de consultas para consultar los 20 primeros productos por categoría.

   ```sql
    SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName
    FROM SalesLT.ProductCategory pc
    JOIN SalesLT.Product p
    ON pc.productcategoryid = p.productcategoryid;
   ```

2. En la barra de herramientas, seleccione **Ejecutar** y, a continuación, revise los resultados en el panel **Resultados**.

![resultados del editor de consultas](./media/sql-database-connect-query-portal/query-editor-results.png)

## <a name="insert-data"></a>Insertar datos

Ejecute la siguiente instrucción Transact-SQL [INSERT](https://msdn.microsoft.com/library/ms174335.aspx) para agregar un nuevo producto en la tabla `SalesLT.Product`.

1. Reemplace la consulta anterior por esta otra.

   ```sql
   INSERT INTO [SalesLT].[Product]
           ( [Name]
           , [ProductNumber]
           , [Color]
           , [ProductCategoryID]
           , [StandardCost]
           , [ListPrice]
           , [SellStartDate]
           )
     VALUES
           ('myNewProduct'
           ,123456789
           ,'NewColor'
           ,1
           ,100
           ,100
           ,GETDATE() );
   ```


2. Seleccione **Ejecutar** para insertar una nueva fila en la tabla `Product`. El panel **Mensajes** muestra **Consulta realizada correctamente: Filas afectadas: 1**.


## <a name="update-data"></a>Actualización de datos

Ejecute la siguiente instrucción Transact-SQL [UPDATE](https://msdn.microsoft.com/library/ms177523.aspx) para modificar el nuevo producto.

1. Reemplace la consulta anterior por esta otra.

   ```sql
   UPDATE [SalesLT].[Product]
   SET [ListPrice] = 125
   WHERE Name = 'myNewProduct';
   ```

2. Seleccione **Ejecutar** para actualizar la fila especificada en la tabla `Product`. El panel **Mensajes** muestra **Consulta realizada correctamente: Filas afectadas: 1**.

## <a name="delete-data"></a>Eliminación de datos

Ejecute la siguiente instrucción Transact-SQL [DELETE](https://msdn.microsoft.com/library/ms189835.aspx) para quitar el nuevo producto.

1. Reemplace la consulta anterior por esta:

   ```sql
   DELETE FROM [SalesLT].[Product]
   WHERE Name = 'myNewProduct';
   ```

2. Seleccione **Ejecutar** para eliminar la fila especificada en la tabla `Product`. El panel **Mensajes** muestra **Consulta realizada correctamente: Filas afectadas: 1**.


## <a name="query-editor-considerations"></a>Consideraciones acerca del Editor de consultas

Hay algunos aspectos que debe conocer al trabajar con el Editor de consultas.

* No se puede usar el Editor de consultas para consultar bases de datos de SQL Server en una red virtual.

* Al presionar F5, se actualiza la página del Editor de consultas y se perderá cualquier consulta en la que se esté trabajando.

* El editor de consultas no puede conectarse a una base de datos `master`.

* La ejecución de consultas tienen un tiempo de expiración de 5 minutos.

* El Editor de consultas solo admite la proyección cilíndrica de tipos de datos geográficos.

* Las vistas y las tablas de base de datos no son compatibles con IntelliSense. Sin embargo, el editor permite autocompletar los nombres que ya se han escrito.


## <a name="next-steps"></a>Pasos siguientes

Para obtener información sobre las instrucciones Transact-SQL admitidas en las bases de datos de Azure SQL, vea [Resolución de diferencias de Transact-SQL durante la migración a SQL Database](sql-database-transact-sql-information.md).
