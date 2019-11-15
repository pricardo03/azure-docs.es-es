---
title: Conexión con VSTS
description: Realice consultas en Azure SQL Data Warehouse con Visual Studio.
services: sql-data-warehouse
author: kevinvngo
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: development
ms.date: 08/15/2019
ms.author: kevin
ms.reviewer: igorstan
ms.custom: seo-lt-2019
ms.openlocfilehash: e2d37b2d71f605077903197d25b5da2803e34ad3
ms.sourcegitcommit: 609d4bdb0467fd0af40e14a86eb40b9d03669ea1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2019
ms.locfileid: "73685579"
---
# <a name="connect-to-sql-data-warehouse-with-visual-studio-and-ssdt"></a>Conexión a SQL Data Warehouse con Visual Studio y SSDT
> [!div class="op_single_selector"]
> * [Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md)
> * [Azure Machine Learning](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
> * [Visual Studio](sql-data-warehouse-query-visual-studio.md)
> * [sqlcmd](sql-data-warehouse-get-started-connect-sqlcmd.md) 
> * [SSMS](sql-data-warehouse-query-ssms.md)
> 
> 

Utilice Visual Studio para realizar consultas en Azure SQL Data Warehouse en unos minutos. Este método usa la extensión SQL Server Data Tools (SSDT) de Visual Studio 2019. 

## <a name="prerequisites"></a>Requisitos previos
Para utilizar este tutorial, necesitará:

* Una cuenta de SQL Data Warehouse existente. Para crear una, consulte [Creación de una instancia de Azure SQL Data Warehouse][Create a SQL Data Warehouse].
* SSDT para Visual Studio. Si tiene Visual Studio, probablemente ya tenga este componente. Para ver instrucciones y opciones de instalación, consulte [Instalación de Visual Studio 2015 y SSDT para Almacenamiento de datos SQL][Installing Visual Studio and SSDT].
* El nombre del servidor SQL completo. Para encontrarlo, consulte [Connect to Azure SQL Data Warehouse][Connect to SQL Data Warehouse](Conexión a Almacenamiento de datos SQL).

## <a name="1-connect-to-your-sql-data-warehouse"></a>1. Conexión a la instancia de SQL Data Warehouse
1. Abra Visual Studio 2019.
2. Abra el Explorador de objetos de SQL Server. Para ello, seleccione **Ver** > **Explorador de objetos de SQL Server**.
   
    ![Explorador de objetos de SQL Server][1]
3. Haga clic en el botón **Agregar SQL Server** .
   
    ![Agregar SQL Server][2]
4. Rellene los campos en la ventana Conectar al servidor.
   
    ![Conectar al servidor][3]
   
   * **Nombre del servidor**. Escriba el **nombre del servidor** definido anteriormente.
   * **Autenticación**. Seleccione **Autenticación de SQL Server** o **Autenticación integrada de Active Directory**.
   * **Nombre de usuario** y **Contraseña**. Escriba el nombre de usuario y la contraseña si la autenticación de SQL Server se seleccionó anteriormente.
   * Haga clic en **Conectar**.
5. Para explorar, expanda su Azure SQL Server. Puede ver las bases de datos asociadas al servidor. Expanda AdventureWorksDW para ver las tablas de la base de datos de ejemplo.
   
    ![Explorar AdventureWorksDW][4]

## <a name="2-run-a-sample-query"></a>2. Ejecución de una consulta de ejemplo
Ahora que se ha establecido una conexión a la base de datos, pasemos a escribir una consulta.

1. Haga clic con el botón derecho en la base de datos en el Explorador de objetos de SQL Server.
2. Seleccione **Nueva consulta**. Se abre una nueva ventana de consulta.
   
    ![Nueva consulta][5]
3. Copie esta consulta TSQL en la ventana de consulta:
   
    ```sql
    SELECT COUNT(*) FROM dbo.FactInternetSales;
    ```
4. Ejecute la consulta. Para hacerlo, haga clic en la flecha verde o use la combinación de teclas `CTRL`+`SHIFT`+`E`.
   
    ![Ejecutar consulta][6]
5. Consulte los resultados de la consulta. En este ejemplo, la tabla FactInternetSales tiene 60398 filas.
   
    ![Resultados de la consulta][7]

## <a name="next-steps"></a>Pasos siguientes
Ahora que puede conectarse y realizar consultas, pruebe a [visualizar los datos con PowerBI][visualizing the data with PowerBI].

Para configurar el entorno para la autenticación de Azure Active Directory, consulte [Authentication to Azure SQL Data Warehouse][Authenticate to SQL Data Warehouse](Autenticación en Almacenamiento de datos SQL de Azure).

<!--Arcticles-->
[Connect to SQL Data Warehouse]: sql-data-warehouse-connect-overview.md
[Create a SQL Data Warehouse]: sql-data-warehouse-get-started-provision.md
[Installing Visual Studio and SSDT]: sql-data-warehouse-install-visual-studio.md
[Authenticate to SQL Data Warehouse]: sql-data-warehouse-authentication.md
[visualizing the data with PowerBI]: sql-data-warehouse-get-started-visualize-with-power-bi.md  

<!--Other-->
[Azure portal]: https://portal.azure.com

<!--Image references-->

[1]: media/sql-data-warehouse-query-visual-studio/open-ssdt.png
[2]: media/sql-data-warehouse-query-visual-studio/add-server.png
[3]: media/sql-data-warehouse-query-visual-studio/connection-dialog.png
[4]: media/sql-data-warehouse-query-visual-studio/explore-sample.png
[5]: media/sql-data-warehouse-query-visual-studio/new-query2.png
[6]: media/sql-data-warehouse-query-visual-studio/run-query.png
[7]: media/sql-data-warehouse-query-visual-studio/query-results.png
