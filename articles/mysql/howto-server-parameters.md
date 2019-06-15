---
title: Configuración de parámetros del servidor en Azure Database for MySQL
description: En este artículo se explica cómo configurar parámetros del servidor MySQL en Azure Database for MySQL mediante Azure Portal.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 12/06/2018
ms.openlocfilehash: 103e09a0e2b9dd409fa2ddaff1c5311ef9936d22
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "61422170"
---
# <a name="how-to-configure-server-parameters-in-azure-database-for-mysql-by-using-the-azure-portal"></a>Cómo configurar parámetros del servidor en Azure Database for MySQL mediante Azure Portal

Azure Database para MySQL admite la configuración de algunos parámetros del servidor. En este artículo se explica cómo configurar estos parámetros mediante Azure Portal. No es posible ajustar todos los parámetros del servidor.

## <a name="navigate-to-server-parameters-on-azure-portal"></a>Ir a Parámetros del servidor de Azure Portal

1. Inicie sesión en Azure Portal y luego localice su servidor de Azure Database for MySQL.
2. En la sección **CONFIGURACIÓN**, haga clic en **Parámetros del servidor** para abrir la página Parámetros del servidor de Azure Database for MySQL.
![Página Parámetros del servidor de Azure Portal](./media/howto-server-parameters/auzre-portal-server-parameters.png)
3. Localice cualquier configuración que deba ajustar. Revise la columna **Descripción** para entender el propósito y los valores permitidos.
![Menú desplegable de enumeración](./media/howto-server-parameters/3-toggle_parameter.png)
4. Haga clic en **Guardar** para guardar los cambios.
![Guardar o descartar cambios](./media/howto-server-parameters/4-save_parameters.png)
5. Si ha guardado los nuevos valores para los parámetros, siempre puede revertir todos los elementos a los valores predeterminados; para ello, seleccione **Restablecer todos los valores predeterminados**.
![Restablecer todos los valores predeterminados](./media/howto-server-parameters/5-reset_parameters.png)

## <a name="list-of-configurable-server-parameters"></a>Lista de parámetros configurables del servidor

La lista de parámetros del servidor admitidos crece constantemente. Use la pestaña Parámetros del servidor en Azure Portal para obtener la definición y configurar parámetros del servidor basados en los requisitos de la aplicación.

## <a name="non-configurable-server-parameters"></a>Parámetros no configurables del servidor

El grupo de búferes de InnoDB y el número máximo de conexiones no son configurables y están vinculados a su [plan de tarifa](concepts-service-tiers.md).

|**Plan de tarifa**| **Generación de procesos**|**Núcleos virtuales**|**Grupo de búferes InnoDB (MB)**| **Conexiones máximas**|
|---|---|---|---|--|
|Básica| Gen 4| 1| 960| 50|
|Básica| Gen 4| 2| 2560| 100|
|Básica| Gen 5| 1| 960| 50|
|Básica| Gen 5| 2| 2560| 100|
|Uso general| Gen 4| 2| 3584| 300|
|Uso general| Gen 4| 4| 7680| 625|
|Uso general| Gen 4| 8| 15360| 1250|
|Uso general| Gen 4| 16| 31232| 2\.500|
|Uso general| Gen 4| 32| 62976| 5000|
|Uso general| Gen 5| 2| 3584| 300|
|Uso general| Gen 5| 4| 7680| 625|
|Uso general| Gen 5| 8| 15360| 1250|
|Uso general| Gen 5| 16| 31232| 2\.500|
|Uso general| Gen 5| 32| 62976| 5000|
|Uso general| Gen 5| 64| 125952| 10000|
|Memoria optimizada| Gen 5| 2| 7168| 600|
|Memoria optimizada| Gen 5| 4| 15360| 1250|
|Memoria optimizada| Gen 5| 8| 30720| 2\.500|
|Memoria optimizada| Gen 5| 16| 62464| 5000|
|Memoria optimizada| Gen 5| 32| 125952| 10000|

Estos parámetros adicionales del servidor no son configurables en el sistema:

|**Parámetro**|**Valor fijo**|
| :------------------------ | :-------- |
|innodb_file_per_table en el nivel básico|Apagado|
|innodb_flush_log_at_trx_commit|1|
|sync_binlog|1|
|innodb_log_file_size|512 MB|

Los demás parámetros de servidor que no aparecen aquí se establecen en sus valores predeterminados de MySQL integrado para las versiones [5.7](https://dev.mysql.com/doc/refman/5.7/en/innodb-parameters.html) y [5.6](https://dev.mysql.com/doc/refman/5.6/en/innodb-parameters.html).

## <a name="working-with-the-time-zone-parameter"></a>Trabajo con el parámetro de zona horaria

### <a name="populating-the-time-zone-tables"></a>Relleno de las tablas de la zona horaria

Las tablas de la zona horaria del servidor se pueden rellenar mediante una llamada al procedimiento almacenado `az_load_timezone` desde una herramienta como la línea de comandos de MySQL o MySQL Workbench.

> [!NOTE]
> Si ejecuta el comando `az_load_timezone` de MySQL Workbench, es posible que primero tenga que desactivar el modo de actualización segura mediante `SET SQL_SAFE_UPDATES=0;`.

```sql
CALL mysql.az_load_timezone();
```

Para ver los valores de zonas horarias disponibles, ejecute el comando siguiente:

```sql
SELECT name FROM mysql.time_zone_name;
```

### <a name="setting-the-global-level-time-zone"></a>Establecimiento de la zona horaria de nivel global

La zona horaria de nivel global se puede establecer desde la página **Parámetros del servidor** en Azure Portal. En el ejemplo siguiente se establece la zona horaria global en el valor "US/Pacific".

![Establecimiento del parámetro de zona horaria](./media/howto-server-parameters/timezone.png)

### <a name="setting-the-session-level-time-zone"></a>Establecimiento de la zona horaria de nivel de sesión

La zona horaria de nivel de sesión se puede establecer mediante la ejecución del comando `SET time_zone` desde una herramienta como la línea de comandos de MySQL o MySQL Workbench. En el ejemplo siguiente se establece la zona horaria en **US/Pacific**.

```sql
SET time_zone = 'US/Pacific';
```

Consulte [Date and Time Functions](https://dev.mysql.com/doc/refman/5.7/en/date-and-time-functions.html#function_convert-tz) (Funciones de fecha y hora) en la documentación de MySQL.

## <a name="next-steps"></a>Pasos siguientes

- [Bibliotecas de conexiones de Azure Database for MySQL](concepts-connection-libraries.md)
