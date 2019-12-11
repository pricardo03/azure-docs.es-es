---
title: 'Registro de auditoría en Azure Database for PostgreSQL: servidor único'
description: Conceptos para el registro de auditoría de pgAudit en Azure Database for PostgreSQL con un único servidor.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 10/14/2019
ms.openlocfilehash: 4a41e5eda3ca2bd92d78a81d73c1ad4c859e25a3
ms.sourcegitcommit: 6bb98654e97d213c549b23ebb161bda4468a1997
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/03/2019
ms.locfileid: "74764566"
---
# <a name="audit-logging-in-azure-database-for-postgresql---single-server"></a>Registro de auditoría en Azure Database for PostgreSQL con un único servidor

El registro de auditoría de actividades de base de datos en Azure Database for PostgreSQL con un único servidor está disponible a través de la extensión de auditoría de PostgreSQL: [pgAudit](https://www.pgaudit.org/). pgAudit proporciona un registro de auditoría de objeto o sesión detallado.

> [!NOTE]
> pgAudit se encuentra en versión preliminar en Azure Database for PostgreSQL.
> Esta extensión se puede habilitar en servidores optimizados para memoria y de uso general.

Si desea obtener registros de nivel de recurso de Azure para operaciones como el escalado de procesos y de almacenamiento, consulte el [registro de actividad de Azure](../azure-monitor/platform/activity-logs-overview.md).

## <a name="usage-considerations"></a>Consideraciones de uso
De forma predeterminada, las instrucciones de registro de pgAudit se emiten junto con las instrucciones de registro normales mediante el uso de la utilidad de registro estándar de Postgres. En Azure Database for PostgreSQL, estos archivos. log se pueden descargar mediante Azure Portal o la CLI. El almacenamiento máximo de la colección de archivos es de 1 GB y cada archivo está disponible durante un máximo de siete días (el valor predeterminado es tres días). Este servicio es una opción de almacenamiento a corto plazo.

Como alternativa, puede configurar todos los registros que se emitirán al servicio de registro de diagnóstico de Azure Monitor. Si habilita el registro de diagnóstico de Azure Monitor, los registros se enviarán automáticamente (en formato JSON) a Azure Storage, Event Hubs o los registros de Azure Monitor, en función de su elección.

Al habilitar pgAudit, se genera un gran volumen de registro en un servidor, lo que afecta al rendimiento y al almacenamiento de registros. Es recomendable usar el servicio del registro de diagnóstico de Azure, que ofrece opciones de almacenamiento a más largo plazo, así como características de análisis y alertas. Se recomienda desactivar el registro estándar para reducir el impacto sobre el rendimiento del registro adicional:

   1. Establezca el parámetro `logging_collector` en OFF. 
   2. Reinicie el servidor para aplicar este cambio.

Para aprender a configurar el registro en Azure Storage, Event hubs o los registros de Azure Monitor, visite la sección de registros de diagnóstico del [artículo de registros de servidor](concepts-server-logs.md).

## <a name="installing-pgaudit"></a>Instalación de pgAudit

Para instalar pgAudit, tendrá que incluirlo en las bibliotecas de carga previa compartidas del servidor. Si cambia el parámetro `shared_preload_libraries` de Postgres deberá reiniciar el servidor para que tenga efecto. Puede cambiar los parámetros mediante [Azure Portal](howto-configure-server-parameters-using-portal.md), la [CLI de Azure](howto-configure-server-parameters-using-cli.md) o [API REST](/rest/api/postgresql/configurations/createorupdate).

Mediante [Azure Portal](https://portal.azure.com):

   1. Seleccione su servidor de Azure Database for PostgreSQL.
   2. En la barra lateral, seleccione **Parámetros del servidor**.
   3. Busque el parámetro `shared_preload_libraries`.
   4. Seleccione **pgaudit**.
   5. Reinicie el servidor para aplicar el cambio.

   6. Conéctese al servidor mediante un cliente (como psql) y habilite la extensión pgAudit.
      ```SQL
      CREATE EXTENSION pgaudit;
      ```

> [!TIP]
> Si ve un error, confirme que ha reiniciado el servidor después de guardar `shared_preload_libraries`.

## <a name="pgaudit-settings"></a>Configuración de pgAudit

pgAudit le permite configurar el registro de auditoría de objeto o sesión. [El registro de auditoría de sesión](https://github.com/pgaudit/pgaudit/blob/master/README.md#session-audit-logging) emite registros detallados de las instrucciones ejecutadas. [El registro de auditoría de objetos](https://github.com/pgaudit/pgaudit/blob/master/README.md#object-audit-logging) es una auditoría con un ámbito limitado a relaciones específicas. Puede optar por configurar uno o ambos tipos de registro. 

> [!NOTE]
> La configuración de pgAudit se especifica globalmente y no se puede en el nivel de base de datos o rol.

Una vez que haya [instalado pgAudit](#installing-pgaudit), puede configurar sus parámetros para iniciar el registro. La [documentación de pgAudit](https://github.com/pgaudit/pgaudit/blob/master/README.md#settings) proporciona la definición de cada parámetro. Pruebe primero los parámetros y confirme que obtiene el comportamiento esperado.

> [!NOTE]
> Al establecer `pgaudit.log_client` en ON, se redirigirán los registros a un proceso de cliente (como psql), en lugar de escribirse en el archivo. Por lo general, esta configuración se debe dejar deshabilitada.

> [!NOTE]
> `pgaudit.log_level` solo se habilita cuando `pgaudit.log_client` está activado. Además, en Azure Portal, actualmente hay un error con `pgaudit.log_level`: se muestra un cuadro combinado, lo que implica que se pueden seleccionar varios niveles. Sin embargo, solo se debe seleccionar un nivel. 

> [!NOTE]
> En Azure Database for PostgreSQL, `pgaudit.log` no se puede establecer con un acceso directo de signo `-` (menos) como se describe en la documentación de pgAudit. Todas las clases de instrucción necesarias (lectura, escritura, etc.) deben especificarse individualmente.

### <a name="audit-log-format"></a>Formato de los registros de auditoría
Cada entrada de auditoría se indica mediante `AUDIT:` cerca del principio de la línea de registro. El formato del resto de la entrada se detalla en la [documentación de pgAudit](https://github.com/pgaudit/pgaudit/blob/master/README.md#format).

Si necesita algún otro campo para satisfacer sus requisitos de auditoría, use el parámetro `log_line_prefix` de Postgres. `log_line_prefix` es una cadena que se genera al principio de cada línea de registro de Postgres. Por ejemplo, la siguiente opción de `log_line_prefix` proporciona la marca de tiempo, el nombre de usuario, el nombre de la base de datos y el identificador de proceso siguiente:

```
t=%m u=%u db=%d pid=[%p]:
```

Para obtener más información acerca de `log_line_prefix`, visite la [documentación de PostgreSQL](https://www.postgresql.org/docs/current/runtime-config-logging.html#GUC-LOG-LINE-PREFIX).

### <a name="getting-started"></a>Introducción
Para empezar a trabajar rápidamente, establezca `pgaudit.log` en `WRITE` y abra los registros para revisar la salida. 


## <a name="next-steps"></a>Pasos siguientes
- [Obtener información sobre el registro en Azure Database for PostgreSQL](concepts-server-logs.md)
- Aprenda cómo establecer parámetros mediante [Azure Portal](howto-configure-server-parameters-using-portal.md), la [CLI de Azure](howto-configure-server-parameters-using-cli.md) o [API REST](/rest/api/postgresql/configurations/createorupdate).
