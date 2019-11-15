---
title: Inicio rápido de Fivetran
description: Empiece a trabajar rápidamente con Fivetran y Azure SQL Data Warehouse.
services: sql-data-warehouse
author: mlee3gsd
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: integration
ms.date: 10/12/2018
ms.author: martinle
ms.reviewer: igorstan
ms.custom: seo-lt-2019
ms.openlocfilehash: 250df3e106ae65cafc84a412c155e3a27c535c79
ms.sourcegitcommit: 609d4bdb0467fd0af40e14a86eb40b9d03669ea1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2019
ms.locfileid: "73686122"
---
# <a name="get-started-quickly-with-fivetran-and-sql-data-warehouse"></a>Empiece a trabajar rápidamente con Fivetran y SQL Data Warehouse

En esta guía de inicio rápido se describe cómo configurar un nuevo usuario de Fivetran para trabajar con Azure SQL Data Warehouse. En este artículo se supone que ya tiene una instancia de SQL Data Warehouse.

## <a name="set-up-a-connection"></a>Configuración de una conexión

1. Busque el nombre completo del servidor y la base de datos que usarán para conectarse a SQL Data Warehouse.
    
    Si necesita ayuda para encontrar esta información, consulte [Conexión a Azure SQL Data Warehouse](sql-data-warehouse-connect-overview.md).

2. En el Asistente para instalación, elija si desea conectarse a la base de datos directamente o mediante un túnel SSH.

   Si decide conectarse directamente a la base de datos, deberá crear una regla de firewall para permitir el acceso. Este es el método más sencillo y seguro.

   Si elige conectarse mediante un túnel SSH, Fivetran se conecta a un servidor independiente de la red. El servidor proporciona un túnel SSH a la base de datos. Debe usar este método si la base de datos está en una subred inaccesible dentro de una red virtual.

3. Agregue la dirección IP **52.0.2.4** al firewall de nivel de servidor para permitir conexiones entrantes a su instancia de SQL Data Warehouse desde Fivetran.

   Para más información, consulte [Creación de una regla de firewall de nivel de servidor](create-data-warehouse-portal.md#create-a-server-level-firewall-rule).

## <a name="set-up-user-credentials"></a>Configuración de las credenciales de usuario

1. Conéctese a su instancia de Azure SQL Data Warehouse mediante SQL Server Management Studio o la herramienta que prefiera. Inicie sesión como usuario administrador del servidor. A continuación, ejecute los siguientes comandos SQL para crear un usuario de Fivetran:
    - En la base de datos maestra: 
    
      ```
      CREATE LOGIN fivetran WITH PASSWORD = '<password>'; 
      ```

    - En la base de datos de SQL Data Warehouse:

      ```
      CREATE USER fivetran_user_without_login without login;
      CREATE USER fivetran FOR LOGIN fivetran;
      GRANT IMPERSONATE on USER::fivetran_user_without_login to fivetran;
      ```

2. Conceda al usuario de Fivetran los siguientes permisos para el almacenamiento:

    ```
    GRANT CONTROL to fivetran;
    ```

    Se requiere el permiso de CONTROL para crear credenciales con ámbito de base de datos que se usan cuando un usuario carga archivos desde Azure Blob Storage mediante PolyBase.

3. Agregue una clase de recurso adecuado al usuario de Fivetran. La clase de recurso que use dependerá de la memoria necesaria para crear un índice de almacén de columnas. Por ejemplo, las integraciones con productos como Marketo y Salesforce requieren una clase de recurso superior debido a la gran cantidad de columnas y el mayor volumen de datos que usan los productos. Una clase de recurso superior requiere más memoria para crear índices de almacén de columnas.

    Se recomienda usar clases de recursos estáticos. Puede comenzar con la clase de recurso `staticrc20`. La clase de recurso `staticrc20` asigna 200 MB para cada usuario, independientemente del nivel de rendimiento que se use. Si se produce un error de indexación de almacén de columnas en el nivel de clase de recurso inicial, aumente la clase de recurso.

    ```
    EXEC sp_addrolemember '<resource_class_name>', 'fivetran';
    ```

    Para obtener más información, lea sobre [límites de memoria y simultaneidad]memory-concurrency-limits.md) y [clases de recursos](sql-data-warehouse-memory-optimizations-for-columnstore-compression.md#ways-to-allocate-more-memory).


## <a name="sign-in-to-fivetran"></a>Inicio de sesión en Fivetran

Para iniciar sesión en Fivetran, escriba las credenciales que usa para acceder a SQL Data Warehouse: 

* Host (el nombre del servidor)
* Puerto
* Base de datos
* Usuario (el nombre de usuario debe ser **fivetran\@_server_name_** , donde *server_name* es parte del URI de host de Azure: ***server_name*.database.windows.net**).
* Password.
