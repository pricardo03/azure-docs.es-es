---
title: Artículo sobre problemas conocidos y limitaciones de migración con las migraciones en línea a Azure Database for MySQL | Microsoft Docs
description: Información acerca de problemas conocidos y limitaciones de migración con las migraciones en línea a Azure Database for MySQL.
services: database-migration
author: HJToland3
ms.author: jtoland
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 03/12/2019
ms.openlocfilehash: 0641545c10d7f59cb1874659eae9c7e7bf65932e
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "60532266"
---
# <a name="known-issuesmigration-limitations-with-online-migrations-to-azure-db-for-mysql"></a>Problemas conocidos y limitaciones de migración con las migraciones en línea a Azure DB for MySQL

Los problemas conocidos y las limitaciones relacionadas con las migraciones en línea de MySQL a Azure Database for MySQL se describen en las siguientes secciones. 

## <a name="online-migration-configuration"></a>Configuración de la migración en línea
- La versión del servidor MySQL Server de origen debe ser la 5.6.35, 5.7.18 o una posterior.
- Azure Database for MySQL es compatible con:
    - MySQL Community Edition
    - Motor de InnoDB
- Migración de la misma versión. No se admite la migración de MySQL 5.6 a Azure Database for MySQL 5.7.
- Habilite el registro binario en my.ini (Windows) o my.cnf (Unix).
    - Establezca Server_id en cualquier número mayor o igual a 1, por ejemplo, Server_id = 1 (solo para MySQL 5.6).
    - Establezca log-bin = \<ruta de acceso> (solo para MySQL 5.6)
    - Establezca binlog_format = row.
    - Expire_logs_days = 5 (recomendado - solo para MySQL 5.6).
- El usuario debe tener el rol ReplicationAdmin.
- Las intercalaciones definidas para la base de datos de MySQL de origen son las mismas que las que están definidas en Azure Database for MySQL de destino.
- El esquema debe coincidir entre la base de datos de MySQL de origen y la base de datos de destino de Azure Database for MySQL.
- El esquema de Azure Database for MySQL de destino no debe tener claves externas. Utilice la consulta siguiente para eliminar las claves externas:
    ```
    SET group_concat_max_len = 8192;
    SELECT SchemaName, GROUP_CONCAT(DropQuery SEPARATOR ';\n') as DropQuery, GROUP_CONCAT(AddQuery SEPARATOR ';\n') as AddQuery
    FROM
    (SELECT 
    KCU.REFERENCED_TABLE_SCHEMA as SchemaName, KCU.TABLE_NAME, KCU.COLUMN_NAME,
        CONCAT('ALTER TABLE ', KCU.TABLE_NAME, ' DROP FOREIGN KEY ', KCU.CONSTRAINT_NAME) AS DropQuery,
        CONCAT('ALTER TABLE ', KCU.TABLE_NAME, ' ADD CONSTRAINT ', KCU.CONSTRAINT_NAME, ' FOREIGN KEY (`', KCU.COLUMN_NAME, '`) REFERENCES `', KCU.REFERENCED_TABLE_NAME, '` (`', KCU.REFERENCED_COLUMN_NAME, '`) ON UPDATE ',RC.UPDATE_RULE, ' ON DELETE ',RC.DELETE_RULE) AS AddQuery
        FROM INFORMATION_SCHEMA.KEY_COLUMN_USAGE KCU, information_schema.REFERENTIAL_CONSTRAINTS RC
        WHERE
          KCU.CONSTRAINT_NAME = RC.CONSTRAINT_NAME
          AND KCU.REFERENCED_TABLE_SCHEMA = RC.UNIQUE_CONSTRAINT_SCHEMA
      AND KCU.REFERENCED_TABLE_SCHEMA = ['schema_name']) Queries
      GROUP BY SchemaName;
    ```

    Ejecute la clave externa que desea eliminar (que es la segunda columna) en el resultado de la consulta.
- El esquema de Azure Database for MySQL de destino no debe tener ningún desencadenador. Para eliminar los desencadenadores en la base de datos de destino:
    ```
    SELECT Concat('DROP TRIGGER ', Trigger_Name, ';') FROM  information_schema.TRIGGERS WHERE TRIGGER_SCHEMA = 'your_schema';
    ```

## <a name="datatype-limitations"></a>Limitaciones del tipo de datos
- **Limitación**: si hay un tipo de datos JSON en la base de datos MySQL de origen, se producirá un error en la migración durante la sincronización continua.

    **Solución alternativa**: modificar el tipo de datos JSON a un texto medio o largo en la base de datos MySQL de origen.

- **Limitación**: si no hay ninguna clave principal en las tablas, se producirá un error en la sincronización continua.
 
    **Solución alternativa**: establecer temporalmente una clave principal para la tabla para que continúe la migración. Puede quitar la clave principal una vez completada la migración de datos.

## <a name="lob-limitations"></a>Limitaciones de LOB
Las columnas de objetos grandes (LOB) son columnas que pueden alcanzar un tamaño considerable. Para MySQL, Medium text, Longtext, Blob, Mediumblob, Longblob, etc. son algunos de los tipos de datos de LOB.

- **Limitación**: si se usan tipos de datos de LOB como claves principales, se producirá un error en la migración.

    **Solución alternativa**: reemplazar la clave principal por otros tipos de datos o columnas que no sean de LOB.

- **Limitación**: Si la longitud de columna de objetos grandes (LOB) es mayor que 32 KB, es posible que se trunquen los datos en el destino. Puede comprobar la longitud de columna de LOB mediante esta consulta:
    ```
    SELECT max(length(description)) as LEN from catalog;
    ```

    **Solución alternativa**: si tiene un objeto LOB mayor de 32 KB, póngase en contacto con el equipo de ingeniería en [Ask Azure Database Migrations](mailto:AskAzureDatabaseMigrations@service.microsoft.com). 

## <a name="other-limitations"></a>Otras limitaciones
- No se admite una cadena de contraseña que tenga llaves de apertura y cierre {  } al principio y al final de la cadena de contraseña. Esta limitación se aplica tanto a conectarse a MySQL de origen como a Azure Database for MySQL de destino.
- No se admiten los DDL siguientes:
    - Todos los DDL de la partición
    - Eliminación de una tabla
    - Cambio de nombre de una tabla
- No está admitido el uso de la instrucción *alter table <nombre_tabla> add column <nombre_columna>* para agregar columnas al principio o en medio de una tabla. La instrucción *alter table <nombre_tabla> add column <nombre_columna>* agrega la columna al final de la tabla.
- No se admiten índices creados solo en parte de la columna de datos. La siguiente instrucción es un ejemplo que crea un índice utilizando solo parte de la columna de datos:
    ``` 
    CREATE INDEX partial_name ON customer (name(10));
    ```

- En DMS, el límite de bases de datos para migrar en una única actividad de migración es de cuatro.
