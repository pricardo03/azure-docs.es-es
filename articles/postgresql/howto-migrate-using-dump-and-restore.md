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
ms.date: 07/19/2018
ms.openlocfilehash: 94d196ceecc0b63b9f0b0fe94f71363dc2086c30
ms.sourcegitcommit: 248c2a76b0ab8c3b883326422e33c61bd2735c6c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2018
ms.locfileid: "39213657"
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

> [!IMPORTANT]
> Copie los archivos de copia de seguridad en un blob o almacén de Azure y realice la restauración desde allí, lo que debería ser mucho más rápido que realizar la restauración a través de Internet.
> 

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

## <a name="next-steps"></a>Pasos siguientes
- Para migrar una base de datos de PostgreSQL mediante exportación e importación, vea [Migración de una base de datos de PostgreSQL mediante exportación e importación](howto-migrate-using-export-and-import.md).
- Para obtener más información sobre cómo migrar bases de datos a Azure Database for PostgreSQL, vea la [Guía de migración de base de datos](http://aka.ms/datamigration).
