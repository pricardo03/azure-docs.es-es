---
title: 'Tutorial Diseño de un panel en tiempo real con Azure Database for PostgreSQL: hiperescala (Citus) (versión preliminar)'
description: En este tutorial se muestra cómo crear, rellenar y consultar tablas distribuidas en hiperescala (Citus) (versión preliminar) de Azure Database for PostgreSQL.
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.custom: mvc
ms.topic: tutorial
ms.date: 05/06/2019
ms.openlocfilehash: 7324ab1d7aa6e42100c9c6760c17b0ea6445f21d
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65079460"
---
# <a name="tutorial-design-a-real-time-analytics-dashboard-by-using-azure-database-for-postgresql--hyperscale-citus-preview"></a>Tutorial: Diseño de un panel de análisis en tiempo real on Azure Database for PostgreSQL: hiperescala (Citus) (versión preliminar)

En este tutorial, usa Azure Database for PostgreSQL: hiperescala (Citus) (versión preliminar) para obtener información sobre lo siguiente:

> [!div class="checklist"]
> * Creación de un grupo de servidores de hiperescala (Citus)
> * Uso de la utilidad psql para crear un esquema
> * Particiones de tablas entre nodos
> * Generación de datos de ejemplo
> * Realización de acumulaciones
> * Consulta de datos sin procesar y agregados
> * Expiración de los datos

## <a name="prerequisites"></a>Requisitos previos

Si no tiene una suscripción a Azure, cree una cuenta [gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="sign-in-to-the-azure-portal"></a>Inicio de sesión en Azure Portal

Inicie sesión en el [Azure Portal](https://portal.azure.com).

## <a name="create-an-azure-database-for-postgresql"></a>Creación de una instancia de Azure Database for PostgreSQL

Para crear un servidor de Azure Database for PostgreSQL, siga estos pasos:
1. Haga clic en **Crear un recurso** de la esquina superior izquierda de Azure Portal.
2. En la página **Nuevo**, seleccione **Bases de datos** y, en la página **Bases de datos**, seleccione **Azure Database for PostgreSQL**.
3. Para la opción de implementación, haga clic en el botón **Crear** en **Hyperscale (Citus) server group - PREVIEW** (Grupo de servidores de hiperescala (Citus): VERSIÓN PRELIMINAR).
4. Rellene el formulario del nuevo servidor con la siguiente información:
   - Grupo de recursos: haga clic en el vínculo **Crear nuevo** que está debajo de este cuadro de texto para este campo. Escriba un nombre como **myresourcegroup**.
   - Nombre del grupo de servidores: **mydemoserver** (el nombre de un servidor que se asigna al nombre DNS y que debe ser globalmente único).
   - Nombre de usuario administrador: **myadmin** (se usará más adelante para conectarse a la base de datos).
   - Contraseña: debe contener al menos ocho caracteres de las tres categorías siguientes: letras mayúsculas del alfabeto inglés, letras minúsculas del alfabeto inglés, números (0-9) y caracteres no alfanuméricos (!, $, #, %, etc.)
   - Ubicación: use la ubicación más cercana a los usuarios para que puedan acceder de la forma más rápida posible a los datos.

   > [!IMPORTANT]
   > El inicio de sesión y la contraseña de administrador del servidor que especifique aquí serán necesarios para iniciar sesión más adelante en ese servidor y en las bases de datos que se especificarán en este tutorial. Recuerde o grabe esta información para su uso posterior.

5. Haga clic en **Configure server group** (Configurar grupo de servidores). No modifique los valores de esa sección y haga clic en **Guardar**.
6. Haga clic en **Review + create** (Revisar y crear) y luego en **Crear** para aprovisionar el servidor. El aprovisionamiento tarda unos minutos.
7. La página irá a la supervisión de la implementación. Cuando el estado activo cambia de **La implementación está en curso** a **Se completó la implementación**, haga clic en el elemento de menú **Salidas** que se encuentra a la izquierda de la página.
8. La página de resultados incluirá un nombre de host de coordinación junto a un botón para copiar el valor en el Portapapeles. Anote esta información para usarla más adelante.

## <a name="configure-a-server-level-firewall-rule"></a>Configuración de una regla de firewall de nivel de servidor

El servicio Azure Database for PostgreSQL usa un firewall en el nivel de servidor. De manera predeterminada, el firewall impide que todas las herramientas y aplicaciones externas se conecten al servidor y a cualquier base de datos del servidor. Debemos agregar una regla para abrir el firewall en un intervalo específico de direcciones IP.

1. Desde la sección **Salidas** donde anteriormente copió el nombre de host del nodo de coordinación, haga clic en Atrás para volver al elemento de menú **Información general**.

2. Busque el grupo de escalado correspondiente a la implementación en la lista de recursos y haga clic en él. (Su nombre tendrá el prefijo "sg-".)

3. Haga clic en **Firewall** en **Seguridad** del menú de la izquierda.

4. Haga clic en el vínculo **+ Agregar regla de firewall para la dirección IP del cliente actual**. Por último, haga clic en el botón **Guardar**.

5. Haga clic en **Save**(Guardar).

   > [!NOTE]
   > El servidor Azure PostgreSQL se comunica a través de puerto 5432. Si intenta conectarse desde una red corporativa, es posible que el firewall de la red no permita el tráfico saliente a través del puerto 5432. En ese caso, no puede conectarse al servidor de Azure SQL Database, salvo que el departamento de TI abra el puerto 5432.
   >

## <a name="connect-to-the-database-using-psql-in-cloud-shell"></a>Conexión a la base de datos mediante psql en Cloud Shell

Ahora vamos a usar la utilidad de línea de comandos [psql](https://www.postgresql.org/docs/current/app-psql.html) para conectarnos al servidor de Azure Database for PostgreSQL.
1. Inicie Azure Cloud Shell desde el icono del terminal en el panel de navegación superior.

   ![Azure Database for PostgreSQL: icono del terminal de Azure Cloud Shell](./media/tutorial-design-database-hyperscale-realtime/psql-cloud-shell.png)

2. Azure Cloud Shell se abrirá en el explorador y podrá escribir comandos de Bash.

   ![Azure Database for PostgreSQL: indicador de Bash de Azure Shell](./media/tutorial-design-database-hyperscale-realtime/psql-bash.png)

3. En el símbolo de sistema de Cloud Shell, conéctese al servidor de Azure Database for PostgreSQL con los comandos psql. El formato siguiente sirve para conectarse a un servidor de Azure Database for PostgreSQL con la utilidad [psql](https://www.postgresql.org/docs/9.6/static/app-psql.html):
   ```bash
   psql --host=<myserver> --username=myadmin --dbname=citus
   ```

   Por ejemplo, el siguiente comando se conecta a la base de datos predeterminada llamada **citus** en el servidor PostgreSQL **mydemoserver.postgres.database.azure.com** con las credenciales de acceso. Escriba la contraseña de administrador del servidor cuando se le solicite.

   ```bash
   psql --host=mydemoserver.postgres.database.azure.com --username=myadmin --dbname=citus
   ```

## <a name="use-psql-utility-to-create-a-schema"></a>Uso de la utilidad psql para crear un esquema

Una vez conectado a Azure Database for PostgreSQL: hiperescala (Citus) (versión preliminar) mediante psql, puede completar algunas tareas básicas. Este tutorial lo guiará a través de la ingesta de datos de tráfico provenientes del análisis web y posterior acumulación de los datos para proporcionar paneles en tiempo real basados en esos datos.

Vamos a crear una tabla que consumirá todos los datos de tráfico web sin procesar. Ejecute los comandos siguientes en el terminal psql:

```sql
CREATE TABLE http_request (
  site_id INT,
  ingest_time TIMESTAMPTZ DEFAULT now(),

  url TEXT,
  request_country TEXT,
  ip_address TEXT,

  status_code INT,
  response_time_msec INT
);
```

También vamos a crear una tabla que contendrá los agregados por minuto y una tabla que conserva la posición de la última acumulación. Ejecute también lo siguiente en psql:

```sql
CREATE TABLE http_request_1min (
  site_id INT,
  ingest_time TIMESTAMPTZ, -- which minute this row represents

  error_count INT,
  success_count INT,
  request_count INT,
  average_response_time_msec INT,
  CHECK (request_count = error_count + success_count),
  CHECK (ingest_time = date_trunc('minute', ingest_time))
);

CREATE INDEX http_request_1min_idx ON http_request_1min (site_id, ingest_time);

CREATE TABLE latest_rollup (
  minute timestamptz PRIMARY KEY,

  CHECK (minute = date_trunc('minute', minute))
);
```

Ahora puede ver las tablas recién creadas en la lista de tablas con este comando de psql:

```postgres
\dt
```

## <a name="shard-tables-across-nodes"></a>Particiones de tablas entre nodos

Una implementación de hiperescala almacena filas de tablas en distintos nodos en función del valor de una columna designada por el usuario. Esta "columna de distribución" marca cómo los datos se particionan entre los distintos nodos.

Establezcamos que la columna de distribución sea site\_id, la clave de partición. En psql, ejecute estas funciones:

  ```sql
SELECT create_distributed_table('http_request',      'site_id');
SELECT create_distributed_table('http_request_1min', 'site_id');
```

## <a name="generate-sample-data"></a>Generación de datos de ejemplo

Ahora, el grupo de servidores debería estar listo para ingerir algunos datos. Podemos ejecutar lo siguiente de manera local desde la conexión de `psql` para insertar datos continuamente.

```sql
DO $$
  BEGIN LOOP
    INSERT INTO http_request (
      site_id, ingest_time, url, request_country,
      ip_address, status_code, response_time_msec
    ) VALUES (
      trunc(random()*32), clock_timestamp(),
      concat('http://example.com/', md5(random()::text)),
      ('{China,India,USA,Indonesia}'::text[])[ceil(random()*4)],
      concat(
        trunc(random()*250 + 2), '.',
        trunc(random()*250 + 2), '.',
        trunc(random()*250 + 2), '.',
        trunc(random()*250 + 2)
      )::inet,
      ('{200,404}'::int[])[ceil(random()*2)],
      5+trunc(random()*150)
    );
    PERFORM pg_sleep(random() * 0.25);
  END LOOP;
END $$;
```

La consulta agrega una fila aproximadamente cada 25 centésimas de segundo. Las filas se almacenan en distintos nodos de trabajo según las indicaciones de la columna de distribución, `site_id`.

   > [!NOTE]
   > Deje que se ejecute la consulta de generación de datos y abra una segunda conexión de psql para el resto de los comandos de este tutorial.
   >

## <a name="query"></a>Consultar

La opción de hospedaje de hiperescala permite que varios nodos procesen consultas en paralelo para así acelerar el proceso. Por ejemplo, la base de datos calcula agregados como SUM y COUNT en nodos de trabajo y combina los resultados en una respuesta final.

Esta es una consulta para contar las solicitudes web por minuto junto con algunas estadísticas.
Intente ejecutarla en psql y observe los resultados.

```sql
SELECT
  site_id,
  date_trunc('minute', ingest_time) as minute,
  COUNT(1) AS request_count,
  SUM(CASE WHEN (status_code between 200 and 299) THEN 1 ELSE 0 END) as success_count,
  SUM(CASE WHEN (status_code between 200 and 299) THEN 0 ELSE 1 END) as error_count,
  SUM(response_time_msec) / COUNT(1) AS average_response_time_msec
FROM http_request
WHERE date_trunc('minute', ingest_time) > now() - '5 minutes'::interval
GROUP BY site_id, minute
ORDER BY minute ASC;
```

## <a name="performing-rollups"></a>Realización de acumulaciones

La consulta anterior funciona bien en las primeras etapas, pero su rendimiento disminuirá a medida que aumentan los datos. Incluso con el procesamiento distribuido, resulta más rápido calcular previamente estos datos que volver a calcularlos varias veces.

Es posible garantizar que el panel siga siendo rápido si acumulamos de manera frecuente los datos sin procesar en una tabla de agregado. En este caso, acumularemos en la tabla de agregación de un minuto, pero también se podrían tener agregaciones de 5 minutos, 15 minutos, una hora, etc.

Como ejecutaremos esta acumulación de manera continua, crearemos una función para realizarla. Ejecute estos comandos en psql para crear la función `rollup_http_request`.

```sql
-- initialize to a time long ago
INSERT INTO latest_rollup VALUES ('10-10-1901');

-- function to do the rollup
CREATE OR REPLACE FUNCTION rollup_http_request() RETURNS void AS $$
DECLARE
  curr_rollup_time timestamptz := date_trunc('minute', now());
  last_rollup_time timestamptz := minute from latest_rollup;
BEGIN
  INSERT INTO http_request_1min (
    site_id, ingest_time, request_count,
    success_count, error_count, average_response_time_msec
  ) SELECT
    site_id,
    date_trunc('minute', ingest_time),
    COUNT(1) as request_count,
    SUM(CASE WHEN (status_code between 200 and 299) THEN 1 ELSE 0 END) as success_count,
    SUM(CASE WHEN (status_code between 200 and 299) THEN 0 ELSE 1 END) as error_count,
    SUM(response_time_msec) / COUNT(1) AS average_response_time_msec
  FROM http_request
  -- roll up only data new since last_rollup_time
  WHERE date_trunc('minute', ingest_time) <@
          tstzrange(last_rollup_time, curr_rollup_time, '(]')
  GROUP BY 1, 2;

  -- update the value in latest_rollup so that next time we run the
  -- rollup it will operate on data newer than curr_rollup_time
  UPDATE latest_rollup SET minute = curr_rollup_time;
END;
$$ LANGUAGE plpgsql;
```

Una vez agregada la función, ejecútela para acumular los datos:

```sql
SELECT rollup_http_request();
```

Y, con los datos en un formulario previamente agregado, podemos consultar la tabla de acumulación para obtener el mismo informe que antes. Ejecute lo siguiente:

```sql
SELECT site_id, ingest_time as minute, request_count,
       success_count, error_count, average_response_time_msec
  FROM http_request_1min
 WHERE ingest_time > date_trunc('minute', now()) - '5 minutes'::interval;
 ```

## <a name="expiring-old-data"></a>Expiración de datos antiguos

Las acumulaciones agilizan las consultas, pero de todos modos es necesario que los datos antiguos expiren para evitar costos de almacenamiento ilimitados. Simplemente debe decidir durante cuánto tiempo quiere conservar los datos para cada granularidad y usar consultas estándar para eliminar los datos expirados. En el ejemplo siguiente, decidimos conservar los datos sin procesar durante un día y las agregaciones por minuto, durante un mes:

```sql
DELETE FROM http_request WHERE ingest_time < now() - interval '1 day';
DELETE FROM http_request_1min WHERE ingest_time < now() - interval '1 month';
```

En el entorno de producción, podría encapsular estas consultas en una función y llamarla cada minuto en un trabajo Cron.

## <a name="clean-up-resources"></a>Limpieza de recursos

En los pasos anteriores, creó recursos de Azure en un grupo de recursos. Si no cree que va a necesitar estos recursos en el futuro, elimine el grupo de servidores. Presione el botón *Eliminar* en la página *Información general* del grupo de servidores. Cuando aparezca una página emergente en la que se le pida hacerlo, confirme el nombre del grupo de servidores y haga clic en el botón *Eliminar* final.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, aprendió a aprovisionar un grupo de servidores de hiperescala (Citus). Se conectó a él con psql, creó un esquema y distribuyó datos. Aprendió a consultar datos en formato sin procesar, agregar esos datos de manera habitual, consultar las tablas de agregados y hacer expirar los datos antiguos.

A continuación, conozca los conceptos de hiperescala.
> [!div class="nextstepaction"]
> [Tipos de nodo de hiperescala](https://aka.ms/hyperscale-concepts)