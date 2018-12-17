---
title: Volcado y restauración en Azure Database for PostgreSQL
description: Describe cómo extraer una base de datos de PostgreSQL en un archivo de volcado y cómo restaurar desde un archivo creado por pg_dump en Azure Database for PostgreSQL.
services: postgresql
author: rachel-msft
ms.author: raagyema
manager: kfile
editor: jasonwhowell
ms.service: postgresql
ms.topic: conceptual
ms.date: 09/22/2018
ms.openlocfilehash: 2aa59bcf0d56358601b81730abe330a56ca35d02
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/06/2018
ms.locfileid: "52966915"
---
# <a name="migrate-your-postgresql-database-using-dump-and-restore"></a>Migración de una base de datos de PostgreSQL mediante volcado y restauración
Puede usar [pg_dump](https://www.postgresql.org/docs/9.3/static/app-pgdump.html) para extraer una base de datos de PostgreSQL a un archivo de volcado y [pg_restore](https://www.postgresql.org/docs/9.3/static/app-pgrestore.html) para restaurar la base de datos de PostgreSQL desde un archivo de almacenamiento creado por pg_dump.

## <a name="prerequisites"></a>Requisitos previos
Para seguir esta guía, necesitará:
- Un [servidor de Azure Database for PostgreSQL](quickstart-create-server-database-portal.md) con reglas de firewall para permitir el acceso a las bases de datos que hay en él.
- Utilidades de línea de comandos [pg_dump](https://www.postgresql.org/docs/9.6/static/app-pgdump.html) y [pg_restore](https://www.postgresql.org/docs/9.6/static/app-pgrestore.html) instaladas

Siga estos pasos para realizar un volcado y restaurar la base de datos de PostgreSQL:

## <a name="create-a-dump-file-using-pgdump-that-contains-the-data-to-be-loaded"></a>Creación de un archivo de volcado mediante pg_dump con los datos que se van a cargar
Para hacer una copia de seguridad de una base de datos de PostgreSQL existente en el entorno local o en una máquina virtual, ejecute el comando siguiente:
```bash
pg_dump -Fc -v --host=<host> --username=<name> --dbname=<database name> > <database>.dump
```
Por ejemplo, si tiene un servidor local y una base de datos llamada **testdb** en él:
```bash
pg_dump -Fc -v --host=localhost --username=masterlogin --dbname=testdb > testdb.dump
```


## <a name="restore-the-data-into-the-target-azure-database-for-postrgesql-using-pgrestore"></a>Restauración de los datos a la instancia de Azure Database for PostrgeSQL con pg_restore
Después de crear la base de datos de destino, puede usar el comando pg_restore y el parámetro -d, --dbname para restaurar los datos en la base de datos de destino desde el archivo de volcado.
```bash
pg_restore -v --no-owner –-host=<server name> --port=<port> --username=<user@servername> --dbname=<target database name> <database>.dump
```
Al incluir el parámetro --no-owner, todos los objetos creados durante la restauración pasan a ser propiedad del usuario especificado con --username. Para más información, consulte la documentación oficial de PostgreSQL en [pg_restore](https://www.postgresql.org/docs/9.6/static/app-pgrestore.html).

> [!NOTE]
> Si el servidor de PostgreSQL requiere conexiones SSL (que lo sucede de forma predeterminada en los servidores de Azure Database for PostgreSQL), establezca una variable de entorno `PGSSLMODE=require` para que la herramienta pg_restore se conecte con SSL. Sin SSL, el error podría ser `FATAL:  SSL connection is required. Please specify SSL options and retry.`
>
> En la línea de comandos de Windows, ejecute el comando `SET PGSSLMODE=require` antes de ejecutar el comando pg_restore. En Linux o Bash, ejecute el comando `export PGSSLMODE=require` antes de ejecutar el comando pg_restore.
>

En este ejemplo, restaure los datos del archivo de volcado **testdb.dump** en la base de datos **mypgsqldb** en el servidor de destino **mydemoserver.postgres.database.azure.com**. 
```bash
pg_restore -v --no-owner --host=mydemoserver.postgres.database.azure.com --port=5432 --username=mylogin@mydemoserver --dbname=mypgsqldb testdb.dump
```

## <a name="optimizing-the-migration-process"></a>Optimización del proceso de migración

Una manera de migrar la base de datos de PostgreSQL existente al servicio Azure Database for PostgreSQL es realizar una copia de seguridad de la base de datos en el origen y restaurarla en Azure. Para minimizar el tiempo necesario para completar la migración, considere el uso de los parámetros siguientes con los comandos de copia de seguridad y restauración.

> [!NOTE]
> Para obtener información de sintaxis detallada, vea los artículos [pg_dump](https://www.postgresql.org/docs/9.6/static/app-pgdump.html) y [pg_restore](https://www.postgresql.org/docs/9.6/static/app-pgrestore.html).
>

### <a name="for-the-backup"></a>Para la copia de seguridad
- Realice la copia de seguridad con el modificador -Fc para poder realizar la restauración en paralelo a fin de acelerar. Por ejemplo: 

    ```
    pg_dump -h MySourceServerName -U MySourceUserName -Fc -d MySourceDatabaseName > Z:\Data\Backups\MyDatabaseBackup.dump
    ```

### <a name="for-the-restore"></a>Para la restauración
- Se sugiere mover el archivo de copia de seguridad a una máquina virtual de Azure de la misma región que el servidor de Azure Database for PostgreSQL al que se está migrando y ejecutar pg_restore desde esa máquina virtual para reducir la latencia de red. También se recomienda crear la máquina virtual con [redes aceleradas](../virtual-network/create-vm-accelerated-networking-powershell.md) habilitadas.
- Ya debería estar así de forma predeterminada, pero abra el archivo de volcado para comprobar que las instrucciones de creación de índice están después de la inserción de los datos. Si no es así, mueva las instrucciones de creación de índice una vez insertados los datos.
- Restaure con los modificadores -Fc y -j *#* a fin de ejecutar la restauración en paralelo. *#* es el número de núcleos en el servidor de destino. También puede probar con *#* establecido en dos veces el número de núcleos del servidor de destino para ver el impacto. Por ejemplo: 

    ```
    pg_restore -h MyTargetServer.postgres.database.azure.com -U MyAzurePostgreSQLUserName -Fc -j 4 -d MyTargetDatabase Z:\Data\Backups\MyDatabaseBackup.dump
    ```

- También puede editar el archivo de volcado si agrega el comando *set synchronous_commit = off;* al principio y el comando *set synchronous_commit = on;* al final. No activarlo al final, antes de que las aplicaciones cambien los datos, podría provocar la consiguiente pérdida de datos.

No olvide probar y validar estos comandos en un entorno de prueba antes de usarlos en producción.

## <a name="next-steps"></a>Pasos siguientes
- Para migrar una base de datos de PostgreSQL mediante exportación e importación, vea [Migración de una base de datos de PostgreSQL mediante exportación e importación](howto-migrate-using-export-and-import.md).
- Para obtener más información sobre cómo migrar bases de datos a Azure Database for PostgreSQL, vea la [Guía de migración de base de datos](https://aka.ms/datamigration).
