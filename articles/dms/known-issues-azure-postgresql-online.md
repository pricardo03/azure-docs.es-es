---
title: 'Artículo sobre problemas conocidos y limitaciones de migración con las migraciones en línea de PostgreSQL a Azure Database for PostgreSQL: servidor único | Microsoft Docs'
description: Información acerca de problemas conocidos y limitaciones de migración con las migraciones en línea de PostgreSQL a Azure Database for PostgreSQL.
services: database-migration
author: HJToland3
ms.author: jtoland
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 10/03/2019
ms.openlocfilehash: 891e8a261e092de0ffcef3941dd48f01942a8030
ms.sourcegitcommit: 4f3f502447ca8ea9b932b8b7402ce557f21ebe5a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/02/2019
ms.locfileid: "71802586"
---
# <a name="known-issuesmigration-limitations-with-online-migrations-from-postgresql-to-azure-db-for-postgresql-single-server"></a>Problemas conocidos y limitaciones de migración con las migraciones en línea de PostgreSQL a Azure DB for PostgreSQL: servidor único

Los problemas conocidos y las limitaciones relacionadas con las migraciones en línea de PostgreSQL a Azure Database for PostgreSQL (servidor único) se describen en las siguientes secciones.

## <a name="online-migration-configuration"></a>Configuración de la migración en línea

- El servidor PostgreSQL de origen debe ejecutar las versiones 9.5.11, 9.6.7, 10.3 o posteriores. Para más información, consulte el artículo acerca de las [versiones de base de datos admitidas de PostgreSQL](../postgresql/concepts-supported-versions.md).
- Solo se admiten migraciones de la misma versión. Por ejemplo, no se admite la migración de PostgreSQL 9.5.11 a Azure Database for PostgreSQL 9.6.7.

    > [!NOTE]
    > Para PostgreSQL versión 10, DMS solo admite en la actualidad la migración de la versión 10.3 a Azure Database for PostgreSQL. Tenemos previsto admitir versiones más recientes de PostgreSQL muy pronto.

- Para habilitar la replicación lógica en el archivo de origen **PostgreSQL postgresql.conf**, establezca los parámetros siguientes:
  - **wal_level** = logical
  - **max_replication_slots** = [núm. máx. de bases de datos para la migración]; si quiere migrar cuatro bases de datos, establezca el valor 4
  - **max_wal_senders** = [número de bases de datos que se ejecutan simultáneamente]; el valor recomendado es 10
- Agregue la dirección IP del agente DMS al archivo pg_hba.conf de PostgreSQL de origen.
  1. Anote la dirección IP de DMS cuando termine de aprovisionar una instancia de DMS.
  2. Agregue la dirección IP al archivo pg_hba.conf, tal como se muestran:

        host    all     172.16.136.18/10    md5    host    replication postgres    172.16.136.18/10    md5

- El usuario debe tener el permiso de superusuario en el servidor que hospeda la base de datos de origen
- Aparte de tener ENUM en el esquema de la base de datos de origen, los esquemas de las bases de datos de origen y de destino deben coincidir.
- El esquema de Azure Database for PostgreSQL (servidor único) de destino no debe tener claves externas. Utilice la consulta siguiente para eliminar las claves externas:

    ```
                                SELECT Queries.tablename
           ,concat('alter table ', Queries.tablename, ' ', STRING_AGG(concat('DROP CONSTRAINT ', Queries.foreignkey), ',')) as DropQuery
                ,concat('alter table ', Queries.tablename, ' ', 
                                                STRING_AGG(concat('ADD CONSTRAINT ', Queries.foreignkey, ' FOREIGN KEY (', column_name, ')', 'REFERENCES ', foreign_table_name, '(', foreign_column_name, ')' ), ',')) as AddQuery
        FROM
        (SELECT
        tc.table_schema, 
        tc.constraint_name as foreignkey, 
        tc.table_name as tableName, 
        kcu.column_name, 
        ccu.table_schema AS foreign_table_schema,
        ccu.table_name AS foreign_table_name,
        ccu.column_name AS foreign_column_name 
    FROM 
        information_schema.table_constraints AS tc 
        JOIN information_schema.key_column_usage AS kcu
          ON tc.constraint_name = kcu.constraint_name
          AND tc.table_schema = kcu.table_schema
        JOIN information_schema.constraint_column_usage AS ccu
          ON ccu.constraint_name = tc.constraint_name
          AND ccu.table_schema = tc.table_schema
    WHERE constraint_type = 'FOREIGN KEY') Queries
      GROUP BY Queries.tablename;
    
    ```

    Ejecute la clave externa que desea eliminar (que es la segunda columna) en el resultado de la consulta.

- El esquema de Azure Database for PostgreSQL (servidor único) de destino no debe tener ningún desencadenador. Use el siguiente comando para deshabilitar los desencadenadores en la base de datos:

     ```
    SELECT Concat('DROP TRIGGER ', Trigger_Name, ';') FROM  information_schema.TRIGGERS WHERE TRIGGER_SCHEMA = 'your_schema';
     ```

## <a name="datatype-limitations"></a>Limitaciones del tipo de datos

- **Limitación**: si hay un tipo de datos ENUM en la base de datos PostgreSQL de origen, se producirá un error en la migración durante la sincronización continua.

    **Solución alternativa**: modificar el tipo de datos ENUM a "character varying" en Azure Database for PostgreSQL.

- **Limitación**: si no hay ninguna clave principal en las tablas, se producirá un error en la sincronización continua.

    **Solución alternativa**: establecer temporalmente una clave principal para la tabla para que continúe la migración. Puede quitar la clave principal una vez completada la migración de datos.

- **Limitación**: No está permitido migrar el tipo de datos JSONB.

## <a name="lob-limitations"></a>Limitaciones de LOB

Las columnas de objetos grandes (LOB) son columnas que pueden alcanzar un tamaño considerable. Para PostgreSQL, los ejemplos de tipos de datos LOB incluyen XML, JSON, IMAGE, TEXT, etc.

- **Limitación**: si se usan tipos de datos de LOB como claves principales, se producirá un error en la migración.

    **Solución alternativa**: reemplazar la clave principal por otros tipos de datos o columnas que no sean de LOB.

- **Limitación**: Si la longitud de columna de objetos grandes (LOB) es mayor que 32 KB, es posible que se trunquen los datos en el destino. Puede comprobar la longitud de columna de LOB mediante esta consulta:

    ```
    SELECT max(length(cast(body as text))) as body FROM customer_mail
    ```

    **Solución alternativa**: si tiene un objeto LOB mayor de 32 KB, póngase en contacto con el equipo de ingeniería en [Ask Azure Database Migrations](mailto:AskAzureDatabaseMigrations@service.microsoft.com).

- **Limitación**: si hay columnas LOB en la tabla y no hay ningún conjunto de claves principales para la tabla, es posible que los datos no se migren para esta tabla.

    **Solución alternativa**: establecer temporalmente una clave principal para la tabla para que continúe la migración. Puede quitar la clave principal una vez completada la migración de datos.

## <a name="postgresql10-workaround"></a>Solución alternativa de PostgreSQL10

PostgreSQL 10.x realiza varios cambios en nombres de la carpeta pg_xlog y, por tanto, la migración no se ejecuta según lo previsto. Si va a migrar desde PostgreSQL 10.x a Azure Database for PostgreSQL 10.3, ejecute el siguiente script en la base de datos de PostgreSQL de origen para crear una función de contenedor en torno a funciones pg_xlog.

```
BEGIN;
CREATE SCHEMA IF NOT EXISTS fnRenames;
CREATE OR REPLACE FUNCTION fnRenames.pg_switch_xlog() RETURNS pg_lsn AS $$ 
   SELECT pg_switch_wal(); $$ LANGUAGE SQL;
CREATE OR REPLACE FUNCTION fnRenames.pg_xlog_replay_pause() RETURNS VOID AS $$ 
   SELECT pg_wal_replay_pause(); $$ LANGUAGE SQL;
CREATE OR REPLACE FUNCTION fnRenames.pg_xlog_replay_resume() RETURNS VOID AS $$ 
   SELECT pg_wal_replay_resume(); $$ LANGUAGE SQL;
CREATE OR REPLACE FUNCTION fnRenames.pg_current_xlog_location() RETURNS pg_lsn AS $$ 
   SELECT pg_current_wal_lsn(); $$ LANGUAGE SQL;
CREATE OR REPLACE FUNCTION fnRenames.pg_is_xlog_replay_paused() RETURNS boolean AS $$ 
   SELECT pg_is_wal_replay_paused(); $$ LANGUAGE SQL;
CREATE OR REPLACE FUNCTION fnRenames.pg_xlogfile_name(lsn pg_lsn) RETURNS TEXT AS $$ 
   SELECT pg_walfile_name(lsn); $$ LANGUAGE SQL;
CREATE OR REPLACE FUNCTION fnRenames.pg_last_xlog_replay_location() RETURNS pg_lsn AS $$ 
   SELECT pg_last_wal_replay_lsn(); $$ LANGUAGE SQL;
CREATE OR REPLACE FUNCTION fnRenames.pg_last_xlog_receive_location() RETURNS pg_lsn AS $$ 
   SELECT pg_last_wal_receive_lsn(); $$ LANGUAGE SQL;
CREATE OR REPLACE FUNCTION fnRenames.pg_current_xlog_flush_location() RETURNS pg_lsn AS $$ 
   SELECT pg_current_wal_flush_lsn(); $$ LANGUAGE SQL;
CREATE OR REPLACE FUNCTION fnRenames.pg_current_xlog_insert_location() RETURNS pg_lsn AS $$ 
   SELECT pg_current_wal_insert_lsn(); $$ LANGUAGE SQL;
CREATE OR REPLACE FUNCTION fnRenames.pg_xlog_location_diff(lsn1 pg_lsn, lsn2 pg_lsn) RETURNS NUMERIC AS $$ 
   SELECT pg_wal_lsn_diff(lsn1, lsn2); $$ LANGUAGE SQL;
CREATE OR REPLACE FUNCTION fnRenames.pg_xlogfile_name_offset(lsn pg_lsn, OUT TEXT, OUT INTEGER) AS $$ 
   SELECT pg_walfile_name_offset(lsn); $$ LANGUAGE SQL;
CREATE OR REPLACE FUNCTION fnRenames.pg_create_logical_replication_slot(slot_name name, plugin name, 
   temporary BOOLEAN DEFAULT FALSE, OUT slot_name name, OUT xlog_position pg_lsn) RETURNS RECORD AS $$ 
   SELECT slot_name::NAME, lsn::pg_lsn FROM pg_catalog.pg_create_logical_replication_slot(slot_name, plugin, 
   temporary); $$ LANGUAGE SQL;
ALTER USER PG_User SET search_path = fnRenames, pg_catalog, "$user", public;

-- DROP SCHEMA fnRenames CASCADE;
-- ALTER USER PG_User SET search_path TO DEFAULT;
COMMIT;
```

## <a name="limitations-when-migrating-online-from-aws-rds-postgresql"></a>Limitaciones al migrar en línea desde AWS RDS PostgreSQL

Al intentar realizar una migración en línea desde AWS RDS PostgreSQL a Azure Database for PostgreSQL, pueden producirse los siguientes errores.

- **Error**: El valor predeterminado de la columna "{column}" de la tabla "{table}" de la base de datos "{database}" es diferente en los servidores de origen y destino. Es "{valor en origen}" en el origen y "{valor en destino}" en el destino.

  **Limitación**: este error se produce cuando el valor predeterminado de un esquema de columna es diferente entre las bases de datos de origen y de destino.
  **Solución alternativa**: asegúrese de que el esquema en el destino coincide con el esquema en el origen. Para obtener detalles sobre la migración de esquemas, consulte la [documentación sobre la migración en línea de Azure PostgreSQL](https://docs.microsoft.com/azure/dms/tutorial-postgresql-azure-postgresql-online#migrate-the-sample-schema).

- **Error**: La base de datos de destino "{database}" tiene "{número de tablas}" tablas mientras que la base de datos de origen "{database}" tiene "{número de tablas}" tablas. El número de tablas en las bases de datos de origen y de destino deben coincidir.

  **Limitación**: este error se produce cuando el número de tablas es diferente entre las bases de datos de origen y de destino.
  **Solución alternativa**: asegúrese de que el esquema en el destino coincide con el esquema en el origen. Para obtener detalles sobre la migración de esquemas, consulte la [documentación sobre la migración en línea de Azure PostgreSQL](https://docs.microsoft.com/azure/dms/tutorial-postgresql-azure-postgresql-online#migrate-the-sample-schema).

- **Error:** la base de datos de origen {database} está vacía.

  **Limitación**: este error se produce cuando la base de datos de origen está vacía. La causa más probable es que ha seleccionado una base de datos incorrecta como origen.
  **Solución alternativa**: vuelva a comprobar la base de datos de origen que ha seleccionado para la migración y, a continuación, inténtelo de nuevo.

- **Error:** la base de datos de destino {database} está vacía. Migre el esquema.

  **Limitación**: este error se produce cuando no hay ningún esquema en la base de datos de destino. Asegúrese de que el esquema en el destino coincide con el esquema en el origen.
  **Solución alternativa**: asegúrese de que el esquema en el destino coincide con el esquema en el origen. Para obtener detalles sobre la migración de esquemas, consulte la [documentación sobre la migración en línea de Azure PostgreSQL](https://docs.microsoft.com/azure/dms/tutorial-postgresql-azure-postgresql-online#migrate-the-sample-schema).

## <a name="other-limitations"></a>Otras limitaciones

- El nombre de la base de datos no puede incluir ningún punto y coma (;).
- No se admiten cadenas de contraseña que tenga llaves de apertura ni de cierre ({}). Esta limitación se aplica tanto a conectarse a PostgreSQL de origen como a Azure Database for PostgreSQL de destino.
- Una tabla capturada debe tener una clave principal. Si una tabla no tiene una clave principal, el resultado de las operaciones de registro DELETE y UPDATE será impredecible.
- Se omitirá la actualización de un segmento de clave principal. En estos casos, el destino identificará la aplicación de dicha actualización como una actualización que no actualizó ninguna fila y se escribirá un registro en la tabla de excepciones.
- La migración de varias tablas con el mismo nombre pero con un caso diferente (por ejemplo, table1, TABLE1 y Table1) puede provocar un comportamiento impredecible y, por tanto, no se admite.
- Cambiar el procesamiento de los DLL de la tabla [CREATE | ALTER | DROP] se admite, a menos que se guardan en un bloque de cuerpo de función o procedimiento interno o en otras construcciones anidadas. Por ejemplo, el cambio siguiente no se capturará:

    ```
    CREATE OR REPLACE FUNCTION pg.create_distributors1() RETURNS void
    LANGUAGE plpgsql
    AS $$
    BEGIN
    create table pg.distributors1(did serial PRIMARY KEY,name varchar(40)
    NOT NULL);
    END;
    $$;
    ```

- No se admite el procesamiento de cambios (sincronización continua) de operaciones TRUNCATE. No se admite la migración de tablas con particiones. Cuando se detecta una tabla con particiones, ocurre lo siguiente:

  - La base de datos proporcionará una lista de tablas primarias y secundarias.
  - La tabla se creará en el destino como una tabla normal con las mismas propiedades que las tablas seleccionadas.
  - Si la tabla primaria de la base de datos de origen tiene el mismo valor de clave principal que sus tablas secundarias, se generará un error de "clave duplicada".

- En DMS, el límite de bases de datos para migrar en una única actividad de migración es de cuatro.
