---
title: 'Azure Portal: Creación de una regla de firewall de SQL Database | Microsoft Docs'
description: Creación de una regla de firewall de nivel de servidor de SQL Database
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: quickstart
author: sachinpMSFT
ms.author: sachinp
ms.reviewer: vanto, carlrab
manager: craigg
ms.date: 12/01/2018
ms.openlocfilehash: cab92539b5019d4807ddefb2b84279c844f53016
ms.sourcegitcommit: 333d4246f62b858e376dcdcda789ecbc0c93cd92
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/01/2018
ms.locfileid: "52721908"
---
# <a name="quickstart-create-a-server-level-firewall-rule-for-your-sql-database-using-the-azure-portal"></a>Guía de inicio rápido: Creación de una regla de firewall de nivel de servidor para SQL Database mediante Azure Portal

Este inicio rápido le guía a través de la creación de una regla de firewall de nivel de servidor para una instancia de Azure SQL Database para que pueda conectarse a ella desde un recurso local.

## <a name="prerequisites"></a>Requisitos previos

En esta guía de inicio rápido se usan los recursos creados en [Creación de una instancia de Azure SQL Database en Azure Portal](sql-database-get-started-portal.md) como punto de partida.

## <a name="sign-in-to-the-azure-portal"></a>Inicio de sesión en Azure Portal

Inicie sesión en el [Azure Portal](https://portal.azure.com/).

## <a name="create-a-server-level-firewall-rule"></a>Crear una regla de firewall de nivel de servidor

El servicio SQL Database crea un firewall en el nivel de servidor. Este firewall evita que herramientas y aplicaciones se conecten al servidor o a las bases de datos del servidor a menos que se cree una regla de firewall para abrir el firewall. Para una conexión desde una dirección IP fuera de Azure, cree una regla de firewall para una dirección IP específica o un intervalo de direcciones. Para más información general acerca de las reglas de firewall, consulte [Creación de una regla de firewall de nivel de servidor para SQL Database mediante Azure Portal](sql-database-firewall-configure.md).

> [!NOTE]
> SQL Database se comunica a través del puerto 1433. Si intenta conectarse desde una red corporativa, es posible que el firewall de la red no permita el tráfico saliente a través del puerto 1433. En ese caso, no puede conectarse al servidor de Azure SQL Database, salvo que el departamento de TI abra el puerto 1433.
>

Siga estos pasos para crear una regla de firewall de nivel de servidor para la dirección IP de su cliente y habilite la conectividad externa mediante el firewall de SQL Database solo para su dirección IP.

1. Una vez completa la implementación del [requisito de la base de datos de Azure SQL](#prerequisites), seleccione **Bases de datos SQL** en el menú izquierdo y, después, elija **mySampleDatabase** en la página **Bases de datos SQL**. Se abre la página de información general de la base de datos, que muestra el nombre completo del servidor (por ejemplo, **mynewserver-20170824.database.windows.net**) y proporciona opciones para otras configuraciones.

2. Copie este nombre de servidor completo para conectarse a su servidor y a sus bases de datos en otras guías de inicio rápido.

   ![nombre del servidor](./media/sql-database-get-started-portal/server-name.png)

3. Seleccione **Establecer el firewall del servidor** en la barra de herramientas. Se abrirá la página **Configuración del firewall** del servidor de SQL Database.

   ![regla de firewall del servidor](./media/sql-database-get-started-portal/server-firewall-rule.png)

4. Elija **Agregar IP de cliente** en la barra de herramientas para agregar la dirección IP actual a la nueva regla de firewall. La regla de firewall puede abrir el puerto 1433 para una única dirección IP o un intervalo de direcciones IP.

   > [!IMPORTANT]
   > De forma predeterminada, el acceso a través del firewall de SQL Database está habilitado para todos los servicios de Azure. Elija **OFF** (Desactivar) en esta página para deshabilitar todos los servicios de Azure.
   >

5. Seleccione **Guardar**. Se crea una regla de firewall de nivel de servidor para el puerto 1433 de la dirección IP actual en el servidor lógico.

6. Cierre la página **Configuración de firewall**.

Mediante SQL Server Management Studio o cualquier otra herramienta que elija, ahora puede conectarse al servidor de SQL Database y a sus bases de datos desde esta dirección IP, usando la cuenta de administrador del servidor creada con anterioridad.

## <a name="clean-up-resources"></a>Limpieza de recursos

Guarde estos recursos si desea volver a la sección [Pasos siguientes](#next-steps) y aprender a conectarse y consultar la base de datos mediante una serie de métodos diferentes. Sin embargo, si desea eliminar los recursos que ha creado en esta guía de inicio rápido, siga estos pasos.


1. En el menú izquierdo de Azure Portal, seleccione **Grupos de recursos** y **myResourceGroup**.
2. En la página del grupo de recursos, seleccione **Eliminar**, escriba **myResourceGroup** en el cuadro de texto y, después, seleccione **Eliminar**.

## <a name="next-steps"></a>Pasos siguientes

- Ahora que tiene una base de datos, puede [conectarse y realizar consultas](sql-database-connect-query.md) con las herramientas o lenguajes que prefiera, incluidas:
  - [Conexión y consulta con SQL Server Management Studio](sql-database-connect-query-ssms.md)
  - [Conexión y consulta de datos con Azure Data Studio](https://docs.microsoft.com/sql/azure-data-studio/quickstart-sql-database?toc=/azure/sql-database/toc.json)
- Para aprender a diseñar su primera base de datos, crear tablas e insertar datos, consulte uno de estos tutoriales:
  - [Diseño de la primera instancia de Azure SQL Database mediante SSMS](sql-database-design-first-database.md)
  - [Diseño de una base de datos de Azure SQL Database y conexión con C# y ADO.NET](sql-database-design-first-database-csharp.md)
