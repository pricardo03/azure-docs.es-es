---
title: Inicio rápido de Fivetran con Azure SQL Data Warehouse | Microsoft Docs
description: Empiece a trabajar rápidamente con Fivetran y Azure SQL Data Warehouse.
services: sql-data-warehouse
author: hirokib
manager: jrj
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: manage
ms.date: 10/12/2018
ms.author: elbutter
ms.reviewer: craigg
ms.openlocfilehash: 8e738becfe356908af5baffc0ebf225916b2616e
ms.sourcegitcommit: 8e06d67ea248340a83341f920881092fd2a4163c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/16/2018
ms.locfileid: "49355297"
---
# <a name="get-started-quickly-with-fivetran-and-sql-data-warehouse"></a>Empiece a trabajar rápidamente con Fivetran y SQL Data Warehouse

En este inicio rápido se da por supuesto que ya tiene una instancia preexistente de SQL Data Warehouse.

## <a name="setup-connection"></a>Configuración de la conexión

1. Busque su nombre del servidor completo y el nombre de la base de datos para conectarse a Azure SQL Data Warehouse.

   [¿Cómo puedo buscar el nombre del servidor y el nombre de la base de datos desde el portal?](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-connect-overview)

2. En el asistente para la instalación, decida si quiere conectar la base de datos de forma directa o a través de un túnel SSH.

   Si decide realizar la conexión directamente a la base de datos, deberá crear una regla de firewall para permitir el acceso. Este es el método más sencillo y seguro.

   Si decide realizar la conexión a través de un túnel SSH, Fivetran se conectará a un servidor independiente de la red que proporciona un túnel SSH para la base de datos. Este método es necesario si la base de datos está en una subred inaccesible dentro de una red virtual.

3. Agregue la dirección IP "52.0.2.4" en el firewall de nivel de servidor para permitir conexiones entrantes a Azure SQL Data Warehouse desde Fivetran.

   [¿Cómo puedo agregar un firewall de nivel de servidor?](https://docs.microsoft.com/azure/sql-data-warehouse/create-data-warehouse-portal#create-a-server-level-firewall-rule)

## <a name="setup-user-credentials"></a>Configuración de las credenciales de usuario

Conéctese como usuario administrador del servidor a Azure SQL Data Warehouse mediante SQL Server Management Studio o la herramienta que prefiera y ejecute los siguientes comandos SQL para crear un usuario de Fivetran:

En la base de datos maestra: ` CREATE LOGIN fivetran WITH PASSWORD = '<password>'; `

En la base de datos de SQL Data Warehouse:

```
CREATE USER fivetran_user_without_login without login;
CREATE USER fivetran FOR LOGIN fivetran;
GRANT IMPERSONATE on USER::fivetran_user_without_login to fivetran;
```

Una vez creado el usuario de Fivetran, concédale los permisos siguientes a su almacenamiento:

```
GRANT CONTROL to fivetran;
```

Agregue una clase de recursos adecuada para el usuario creado según los requisitos de memoria para la creación de índices de almacén de columnas. Por ejemplo, las integraciones como Marketo y Salesforce necesitan clases de recursos superiores debido al gran número de columnas o mayor volumen de datos, lo que requiere más memoria para crear índices de almacén de columnas.

Se recomienda usar clases de recursos estáticos. Puede comenzar con la clase de recurso `staticrc20`, que asigna 200 MB para el usuario, independientemente del nivel de rendimiento que utilice. Si se produce un error de indexación del almacén de columnas con la clase de recursos actual, se debe aumentar la clase de recursos.

```
EXEC sp_addrolemember '<resource_class_name>', 'fivetran';
```

Para obtener más información, consulte los documentos sobre [límites de memoria y simultaneidad](https://docs.microsoft.com/azure/sql-data-warehouse/memory-and-concurrency-limits#data-warehouse-limits) y [clases de recursos](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-memory-optimizations-for-columnstore-compression#ways-to-allocate-more-memory)

Se necesita el permiso CONTROL para crear las credenciales con ámbito de base de datos que va a utilizar al cargar archivos desde Blob Storage mediante PolyBase.

Escriba las credenciales para tener acceso a Azure SQL Data Warehouse

1. Host (el nombre del servidor)
2. Port
3. Base de datos
4. Usuario (el nombre de usuario debe ser `fivetran@<server_name>`, donde `<server_name>` forma parte del URI del host de Azure: `<server_name>.database.windows.net`)
5. Contraseña