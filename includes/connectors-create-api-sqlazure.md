---
title: archivo de inclusión
description: archivo de inclusión
services: logic-apps
author: ecfan
ms.service: logic-apps
ms.topic: include
ms.date: 05/15/2018
ms.author: estfan
ms.custom: include file
ms.openlocfilehash: da03c5247b8ebe0a3305b08a05d661264497663f
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60541150"
---
* Si está usando Azure SQL Database, siga los pasos que se indican en [Conexión a una instancia de Azure SQL Database](#connect-azure-sql-db). 

* Si usa SQL Server, siga los pasos que se indican en [Conexión con SQL Server](#connect-sql-server).

<a name="connect-azure-sql-db"></a>

### <a name="connect-to-azure-sql-database"></a>Conexión a una instancia de Azure SQL Database

1. Cuando el desencadenador o la acción de SQL le pidan la información de conexión, siga estos pasos:

   1. Cree un nombre para la conexión.

   2. Seleccione su servidor SQL Server y elija la base de datos. 

      La lista de bases de datos aparece solo después de seleccionar el servidor SQL Server.
 
   3. Proporcione el nombre de usuario y la contraseña del servidor.

      Puede encontrar esta información en Azure Portal en las propiedades de SQL Database o en la cadena de conexión: 
      
      "User ID=<*suNombreDeUsuario*>"
      <br>
      "Password=<*suContraseña*>"

   En este ejemplo se muestra la información de conexión para un desencadenador, pero estos pasos funcionan también para las acciones.

   ![Creación de conexión de Azure SQL Database](./media/connectors-create-api-sqlazure/azure-sql-database-create-connection.png)
   <br>
   Los asteriscos (*) indican los valores obligatorios.

   | Propiedad | Value | Detalles | 
   |----------|-------|---------| 
   | Nombre de la conexión | <*my-sql-connection*> | El nombre de la conexión | 
   | Nombre del servidor SQL Server | <*my-sql-server*> | El nombre del servidor SQL server |
   | Nombre de la instancia de SQL Database | <*my-sql-database*>  | El nombre de la instancia de SQL Database | 
   | Nombre de usuario | <*my-sql-username*> | El nombre de usuario para acceder a la base de datos |
   | Contraseña | <*my-sql-password*> | La contraseña para acceder a la base de datos | 
   |||| 

2. Cuando termine, seleccione **Crear**.

3. Después de crear la conexión, continúe con [Incorporación del desencadenador de SQL](#add-sql-trigger) o [Incorporación de acción de SQL](#add-sql-action).

<a name="connect-sql-server"></a>

### <a name="connect-to-sql-server"></a>Conexión con SQL Server

Para poder seleccionar la puerta de enlace, asegúrese de que ya ha [configurado la puerta de enlace de datos](https://docs.microsoft.com/azure/logic-apps/logic-apps-gateway-connection). De este modo, la puerta de enlace aparecerá en la lista correspondiente al crear la conexión.

1. Cuando el desencadenador o la acción de SQL le pidan la información de conexión, siga estos pasos:

   1. En el desencadenador o en la acción, seleccione **Conectar mediante puerta de enlace de datos local** para que aparezcan las opciones de SQL Server.

   2. Cree un nombre para la conexión.

   3. Proporcione la dirección de su servidor SQL Server y, a continuación, indique el nombre de la base de datos.
   
      Puede encontrar esta información en la cadena de conexión: 
      
      * "Server=<*direcciónDeSuServidor*>"
      * "Database=<*nombreDeSuBasededatos*>"

   4. Proporcione el nombre de usuario y la contraseña del servidor.

      Puede encontrar esta información en la cadena de conexión: 
      
      * "User ID=<*suNombreDeUsuario*>"
      * "Password=<*suContraseña*>"

   5. Si el servidor SQL Server utiliza la autenticación básica o de Windows, seleccione el tipo de autenticación.

   6. Seleccione el nombre de la puerta de enlace de datos local que creó anteriormente.
   
      Si la puerta de enlace no aparece en la lista, compruebe que la [configuró](https://docs.microsoft.com/azure/logic-apps/logic-apps-gateway-connection) correctamente.

   En este ejemplo se muestra la información de conexión para un desencadenador, pero estos pasos funcionan también para las acciones.

   ![Creación de una conexión a SQL Server](./media/connectors-create-api-sqlazure/sql-server-create-connection.png)
   <br>
   Los asteriscos (*) indican los valores obligatorios.

   | Propiedad | Value | Detalles | 
   |----------|-------|---------| 
   | Conectar mediante puerta de enlace local | Seleccione esta opción en primer lugar para la configuración de SQL Server. | | 
   | Nombre de la conexión | <*my-sql-connection*> | El nombre de la conexión | 
   | Nombre del servidor SQL Server | <*my-sql-server*> | El nombre del servidor SQL server |
   | Nombre de la instancia de SQL Database | <*my-sql-database*>  | El nombre de la instancia de SQL Database |
   | Nombre de usuario | <*my-sql-username*> | El nombre de usuario para acceder a la base de datos |
   | Contraseña | <*my-sql-password*> | La contraseña para acceder a la base de datos | 
   | Tipo de autenticación | Windows o básica | Opcional: El tipo de autenticación utilizado por SQL server | 
   | Puertas de enlace | <*my-data-gateway*> | El nombre de la puerta de enlace de datos local | 
   |||| 

2. Cuando termine, seleccione **Crear**. 

3. Después de crear la conexión, continúe con [Incorporación del desencadenador de SQL](#add-sql-trigger) o [Incorporación de acción de SQL](#add-sql-action).
