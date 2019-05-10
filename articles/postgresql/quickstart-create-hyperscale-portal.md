---
title: Inicio rápido de Azure Database for PostgreSQL Hiperescala (Citus) (versión preliminar)
description: Inicio rápido para crear y consultar tablas distribuidas en Azure Database for PostgreSQL Hiperescala (Citus) (versión preliminar).
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.custom: mvc
ms.topic: quickstart
ms.date: 05/06/2019
ms.openlocfilehash: 30de4da43569abf4d7bd668fd0fa481ecac23f4d
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65080033"
---
# <a name="quickstart-create-an-azure-database-for-postgresql---hyperscale-citus-preview-in-the-azure-portal"></a>Inicio rápido: Creación de una instancia de Azure Database for PostgreSQL Hiperescala (Citus) (versión preliminar) en Azure Portal

Azure Database for PostgreSQL es un servicio administrado que usa para ejecutar, administrar y escalar bases de datos de PostgreSQL de alta disponibilidad en la nube. Este inicio rápido muestra cómo crear un servidor de Azure Database for PostgreSQL Hiperescala (Citus) (versión preliminar) utilizando Azure Portal. Explorará los datos distribuidos: tablas de particionamiento entre nodos, ingesta de datos de ejemplo y ejecución de consultas que se ejecutan en varios nodos.

Si no tiene una suscripción a Azure, cree una cuenta [gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="sign-in-to-the-azure-portal"></a>Inicio de sesión en Azure Portal

Inicie sesión en el [Azure Portal](https://portal.azure.com).

## <a name="create-an-azure-database-for-postgresql"></a>Creación de una instancia de Azure Database for PostgreSQL

Para crear un servidor de Azure Database for PostgreSQL, siga estos pasos:
1. Haga clic en **Crear un recurso** de la esquina superior izquierda de Azure Portal.
2. En la página **Nuevo**, seleccione **Bases de datos** y, en la página **Bases de datos**, seleccione **Azure Database for PostgreSQL**.
3. Para la opción de implementación, haga clic en el botón **Crear** en **Grupo de servidores de hiperescala (Citus): VERSIÓN PRELIMINAR.**
4. Rellene el formulario del nuevo servidor con la siguiente información:
   - Grupo de recursos: haga clic en el vínculo **Crear nuevo** que está debajo del cuadro de texto para este campo. Escriba un nombre como **myresourcegroup**.
   - Nombre del grupo de servidores: escriba un nombre exclusivo para el nuevo grupo de servidores, que también se usará para un subdominio de servidor.
   - Nombre de usuario administrador: escriba un nombre de usuario exclusivo, que se usará más adelante para conectarse con la base de datos.
   - Contraseña: tiene que tener al menos ocho caracteres de las tres categorías siguientes: letras mayúsculas del alfabeto inglés, letras minúsculas del alfabeto inglés, números (0-9) y caracteres no alfanuméricos (!, $, #, %, etc.)
   - Ubicación: use la ubicación más cercana a los usuarios para que puedan acceder de la forma más rápida posible a los datos.

   > [!IMPORTANT]
   > Se requiere el inicio de sesión y la contraseña de administrador de servidor que especifique aquí para iniciar sesión en el servidor y a sus bases de datos más adelante en esta guía de inicio rápido. Recuerde o grabe esta información para su uso posterior.

5. Haga clic en **Configurar grupo de servidores**. No modifique los valores de esa sección y haga clic en **Guardar**.
6. Haga clic en **Review + create** (Revisar y crear) y luego en **Crear** para aprovisionar el servidor. El aprovisionamiento tarda unos minutos.
7. La página irá a la supervisión de la implementación. Cuando el estado activo cambia de **La implementación está en curso** a **Se completó la implementación**, haga clic en el elemento de menú **Salidas** que se encuentra a la izquierda de la página.
8. La página de resultados incluirá un nombre de host de coordinación junto a un botón para copiar el valor en el Portapapeles. Anote esta información para usarla más adelante.

## <a name="configure-a-server-level-firewall-rule"></a>Configuración de una regla de firewall de nivel de servidor

El servicio Azure Database for PostgreSQL Hiperescala (Citus) (versión preliminar) usa un firewall en el nivel de servidor. De manera predeterminada, el firewall impide que todas las herramientas y aplicaciones externas se conecten al nodo de coordinación y a cualquier base de datos interna. Tenemos que agregar una regla para abrir el firewall en un intervalo específico de direcciones IP.

1. Desde la sección **Salidas** donde anteriormente copió el nombre de host del nodo de coordinación, haga clic en Atrás para volver al elemento de menú **Información general**.

2. El nombre del grupo de escalado de la implementación tendrá el prefijo "sg-". Lo encontrará en la lista de recursos, haga clic en él.

3. Haga clic en **Firewall** en **Seguridad** del menú de la izquierda.

4. Haga clic en el vínculo **+ Agregar regla de firewall para la dirección IP del cliente actual**. Por último, haga clic en el botón **Guardar**.

5. Haga clic en **Save**(Guardar).

   > [!NOTE]
   > El servidor Azure PostgreSQL se comunica a través de puerto 5432. Si intenta conectarse desde una red corporativa, es posible que el firewall de la red no permita el tráfico saliente a través del puerto 5432. En ese caso, no puede conectarse al servidor de Azure SQL Database, salvo que el departamento de TI abra el puerto 5432.
   >

## <a name="connect-to-the-database-using-psql-in-cloud-shell"></a>Conexión a la base de datos mediante psql en Cloud Shell

Ahora vamos a usar la utilidad de línea de comandos [psql](https://www.postgresql.org/docs/current/app-psql.html) para conectarnos al servidor de Azure Database for PostgreSQL.
1. Inicie Azure Cloud Shell desde el icono del terminal en el panel de navegación superior.

   ![Azure Database for PostgreSQL: icono del terminal de Azure Cloud Shell](./media/quickstart-create-hyperscale-portal/psql-cloud-shell.png)

2. Azure Cloud Shell se abrirá en el explorador y podrá escribir comandos de Bash.

   ![Azure Database for PostgreSQL: indicador de Bash de Azure Shell](./media/quickstart-create-hyperscale-portal/psql-bash.png)

3. En el símbolo de sistema de Cloud Shell, conéctese al servidor de Azure Database for PostgreSQL con los comandos psql. El formato siguiente sirve para conectarse a un servidor de Azure Database for PostgreSQL con la utilidad [psql](https://www.postgresql.org/docs/9.6/static/app-psql.html):
   ```bash
   psql --host=<myserver> --username=myadmin --dbname=citus
   ```

   Por ejemplo, el siguiente comando se conecta a la base de datos predeterminada llamada **citus** en el servidor PostgreSQL **mydemoserver.postgres.database.azure.com** con las credenciales de acceso. Escriba la contraseña de administrador del servidor cuando se le solicite.

   ```bash
   psql --host=mydemoserver.postgres.database.azure.com --username=myadmin --dbname=citus
   ```

## <a name="create-and-distribute-tables"></a>Creación y distribución de tablas

Una vez conectado al nodo coordinador de hiperescala mediante psql, puede completar algunas tareas básicas.

Dentro de los servidores Hiperescala hay tres tipos de tablas:

- Tablas particionadas o distribuidas (repartidas para ayudar a escalar para el rendimiento y paralelización)
- Tablas de referencia (se mantienen varias copias)
- Tablas locales (se usan frecuentemente para las tablas de administración interna)

En este inicio rápido nos centraremos principalmente en las tablas distribuidas para familiarizarnos con ellas.

El modelo de datos con el que vamos a trabajar es simple: datos de usuario y eventos desde GitHub. Los eventos incluyen la creación de una bifurcación, confirmaciones git relacionadas con una organización y más.

Una vez que se haya conectado a través de psql vamos a crear nuestras tablas. En la consola de psql ejecute lo siguiente:

```sql
CREATE TABLE github_events
(
    event_id bigint,
    event_type text,
    event_public boolean,
    repo_id bigint,
    payload jsonb,
    repo jsonb,
    user_id bigint,
    org jsonb,
    created_at timestamp
);

CREATE TABLE github_users
(
    user_id bigint,
    url text,
    login text,
    avatar_url text,
    gravatar_id text,
    display_login text
);
```

El campo `payload` de `github_events` tiene un tipo de datos JSONB. JSONB es el tipo de datos JSON en formato binario en Postgres. Esto facilita el almacenamiento de un esquema más flexible en una sola columna.

Postgres puede crear un índice `GIN` en este tipo que indexará cada clave y valor dentro de él. Con un índice, consultar la carga con varias condiciones pasa a ser una tarea rápida y fácil. Sigamos adelante, ahora vamos a crear un par de índices antes de cargar los datos. En psql:

```sql
CREATE INDEX event_type_index ON github_events (event_type);
CREATE INDEX payload_index ON github_events USING GIN (payload jsonb_path_ops);
```

A continuación con esas tablas Postgres en el nodo de coordinación, indicaremos a Hiperescala que las particione entre los roles de trabajo. Para ello, ejecutaremos una consulta para cada tabla especificando la clave en la que se va a particionar. En el ejemplo actual, vamos a particionar la tabla de usuarios y de eventos en `user_id`:

```sql
SELECT create_distributed_table('github_events', 'user_id');
SELECT create_distributed_table('github_users', 'user_id');
```

Ya estamos preparados para cargar los datos. Descargar los dos archivos de ejemplo [users.csv](https://examples.citusdata.com/users.csv) y [events.csv](https://examples.citusdata.com/events.csv). Después de descargar los archivos, conéctese a la base de datos mediante psql, teniendo cuidado de ejecutar psql desde el directorio que contiene los archivos que descargó. Cargue los datos con el comando `\copy`:

```sql
\copy github_events from 'events.csv' WITH CSV
\copy github_users from 'users.csv' WITH CSV
```

## <a name="run-queries"></a>Ejecutar consultas

Ahora llega la parte más divertida: vamos a ejecutar algunas consultas. Comencemos con un sencillo `count (*)` para ver cuántos datos hemos cargado:

```sql
SELECT count(*) from github_events;
```

Esto ha ido muy bien. Volveremos a ese tipo de agregación más tarde, pero ahora vamos a echar un vistazo a otras consultas. Dentro de la columna `payload` de JSONB existe una cantidad importante de datos, pero varían en función del tipo de evento. Los eventos `PushEvent` contienen un tamaño que incluye el número de diferentes confirmaciones para la inserción. Se puede usar para buscar el número total de confirmaciones por hora:

```sql
SELECT date_trunc('hour', created_at) AS hour,
       sum((payload->>'distinct_size')::int) AS num_commits
FROM github_events
WHERE event_type = 'PushEvent'
GROUP BY hour
ORDER BY hour;
```

Hasta ahora, las consultas que hemos realizado solo han utilizado github\_events, pero podemos combinar esta información con github\_users. Como hemos particionado tanto los usuarios como los eventos en el mismo identificador (`user_id`), las filas de ambas tablas con identificadores de usuario que coincidan se [coubicarán](http://docs.citusdata.com/en/stable/sharding/data_modeling.html#colocation) en los mismos nodos de base de datos con lo que pueden combinarse fácilmente.

Si incorporamos `user_id`, Hiperescala puede insertar la ejecución conjunta en particiones para su ejecución en paralelo en nodos de trabajo. Por ejemplo, vamos a buscar los usuarios que hayan creado el mayor número de repositorios:

```sql
SELECT login, count(*)
FROM github_events ge
JOIN github_users gu
ON ge.user_id = gu.user_id
WHERE event_type = 'CreateEvent' AND
      payload @> '{"ref_type": "repository"}'
GROUP BY login
ORDER BY count(*) DESC;
```

## <a name="clean-up-resources"></a>Limpieza de recursos

En los pasos anteriores, creó recursos de Azure en un grupo de servidores. Si no cree que vaya a necesitar estos recursos en el futuro, elimine el grupo de servidores. Presione el botón **Eliminar** en la página **Información general** del grupo de servidores. Cuando aparezca una página emergente en la que se le pida hacerlo, confirme el nombre del grupo de servidores y haga clic en el botón **Eliminar** final.

## <a name="next-steps"></a>Pasos siguientes

En este inicio rápido, aprendió a aprovisionar un grupo de servidores de Hiperescala (Citus). Se conectó a él con psql, creó un esquema y distribuyó datos.

A continuación, siga un tutorial para crear aplicaciones escalables de multiinquilino.
> [!div class="nextstepaction"]
> [Diseño de una base de datos multiinquilino](https://aka.ms/hyperscale-tutorial-multi-tenant)
