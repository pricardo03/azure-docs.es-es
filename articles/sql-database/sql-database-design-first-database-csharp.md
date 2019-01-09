---
title: 'Diseño de la primera base de datos de Azure SQL Database: C# | Microsoft Docs'
description: Aprenda a diseñar la primera base de datos de SQL Azure Dabatase y a conectarse a ella con un programa de C# mediante ADO.NET.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.topic: tutorial
author: MightyPen
ms.author: genemi
ms.reviewer: carlrab
manager: craigg-msft
ms.date: 12/10/2018
ms.openlocfilehash: cf180f6e2970ac4435602f1cceeb98a4dd9e8724
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/21/2018
ms.locfileid: "53727173"
---
# <a name="tutorial-design-an-azure-sql-database-and-connect-with-cx23-and-adonet"></a>Tutorial: Diseño de una base de datos de SQL Azure Database y conexión con C&#x23; y ADO.NET

Azure SQL Database es una base de datos como servicio (DBaaS) relacional en Microsoft Cloud (Azure). En este tutorial se aprenderá a usar Azure Portal y ADO.NET con Visual Studio para:

> [!div class="checklist"]
> * Crear una base de datos en Azure Portal
> * Establecer una regla de firewall de nivel de servidor en Azure Portal
> * Conectarse a la base de datos con ADO.NET y Visual Studio
> * Crear tablas con ADO.NET
> * Insertar, actualizar y eliminar datos con ADO.NET
> * Consultar datos con ADO.NET

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Una instalación de [Visual Studio 2017](https://www.visualstudio.com/downloads/)

<!-- The following included .md, sql-database-tutorial-portal-create-firewall-connection-1.md, is long.
And it starts with a ## H2.
-->

[!INCLUDE [sql-database-tutorial-portal-create-firewall-connection-1](../../includes/sql-database-tutorial-portal-create-firewall-connection-1.md)]

<!-- The following included .md, sql-database-csharp-adonet-create-query-2.md, is long.
And it starts with a ## H2.
-->

[!INCLUDE [sql-database-csharp-adonet-create-query-2](../../includes/sql-database-csharp-adonet-create-query-2.md)]

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, aprendió tareas básicas de las bases de datos como crear una base de datos y tablas, conectarse a la base de datos, cargar datos y ejecutar consultas. Ha aprendido a:

> [!div class="checklist"]
> * Creación de una base de datos
> * Configurar una regla de firewall
> * Conectarse a la base de datos con [Visual Studio y C#](sql-database-connect-query-dotnet-visual-studio.md)
> * Cree las tablas.
> * Insertar, actualizar, eliminar y consultar los datos

Prosiga con el tutorial siguiente para aprender sobre la migración de datos.

> [!div class="nextstepaction"]
> [Migración de SQL Server a Azure SQL Database sin conexión mediante DMS](../dms/tutorial-sql-server-to-azure-sql.md)
