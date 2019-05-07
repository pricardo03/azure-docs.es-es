---
title: 'Réplicas de lectura en Azure Database for PostgreSQL: servidor único'
description: 'Este artículo describe la característica de réplica de lectura en Azure Database for PostgreSQL: servidor único.'
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 5/6/2019
ms.openlocfilehash: 1d75d01df74a239ba865d9a4e2b216a410e6069c
ms.sourcegitcommit: 0568c7aefd67185fd8e1400aed84c5af4f1597f9
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65067429"
---
# <a name="read-replicas-in-azure-database-for-postgresql---single-server"></a>Réplicas de lectura en Azure Database for PostgreSQL: servidor único

La característica de réplica de lectura permite replicar datos de un servidor Azure Database for PostgreSQL en un servidor de solo lectura. Puede crear réplicas desde el servidor maestro hasta cinco réplicas dentro de la misma región de Azure. Las réplicas se actualizan de manera asincrónica mediante la tecnología de replicación nativa del motor de PostgreSQL.

Las réplicas son nuevos servidores que se administran de forma similar a los servidores de Azure Database for PostgreSQL normales. En cada réplica de lectura, se le cobra por el proceso aprovisionado en núcleos virtuales y el almacenamiento aprovisionado en GB/mes.

Vea cómo [crear y administrar réplicas](howto-read-replicas-portal.md).

## <a name="when-to-use-a-read-replica"></a>Casos en los que utilizar las réplicas de lectura
La finalidad de la característica de réplica de lectura es ayudar a mejorar el rendimiento y la escala de las cargas de trabajo que conllevan un gran número de operaciones de lectura. Las cargas de trabajo de lectura se pueden aislar en las réplicas, mientras que las cargas de trabajo de escritura se pueden dirigir al servidor maestro.

Un caso habitual es que las cargas de trabajo de análisis e inteligencia empresarial utilicen la réplica de lectura como origen de datos para los informes.

Dado que las réplicas son de solo lectura, no reducen directamente las cargas de capacidad de escritura en el servidor maestro. Esta característica no está destinada a cargas de trabajo intensivas de escritura.

Esta característica de réplica de lectura utiliza la replicación asincrónica nativa de PostgreSQL. La característica no está diseñada para escenarios de replicación sincrónica. Habrá un retraso medible entre el servidor maestro y la réplica. Los datos de la réplica se vuelven finalmente coherentes con los datos del servidor maestro. Use esta característica con cargas de trabajo que puedan admitir este retraso.

## <a name="create-a-replica"></a>Creación de una réplica
El servidor maestro debe tener el `azure.replication_support` establecido en **RÉPLICA**. Cuando se cambia este parámetro, es necesario reiniciar el servidor para que el cambio surta efecto. Este parámetro `azure.replication_support` se aplica únicamente a los niveles De uso General y Optimizado para memoria.

Cuando se inicia el flujo de trabajo de creación de la réplica, se crea un servidor Azure Database for PostgreSQL en blanco. El nuevo servidor se rellena con los datos que estaban en el servidor maestro. El tiempo que se tarda en crear la réplica depende de la cantidad de datos en el servidor maestro y del tiempo desde la última copia de seguridad completa semanal. Puede oscilar desde unos minutos hasta varias horas.

La característica de réplica de lectura usa la replicación física de PostgreSQL (replicación no lógica). El modo de funcionamiento predeterminado es la transmisión de la replicación mediante espacios de replicación. Cuando es necesario, se usa el trasvase de registros para actualizarse.

> [!NOTE]
> Si no tiene alertas de almacenamiento en sus servidores, se recomienda que lo haga. La alerta le informa cuando un servidor está alcanzando el límite de almacenamiento, lo que afectará a la replicación.

Aprenda a [crear una réplica de lectura en Azure Portal](howto-read-replicas-portal.md).

## <a name="connect-to-a-replica"></a>Conexión a una réplica
Al crear una réplica, no se heredan las reglas de firewall o el punto de conexión de servicio de red virtual del servidor maestro. Estas reglas se deben configurar de forma independiente para la réplica.

La réplica hereda su cuenta de administrador del servidor maestro. Todas las cuentas de usuario existentes en el servidor se replican en las réplicas de lectura. Solo se puede conectar a una réplica de lectura utilizando las cuentas de usuario disponibles en el servidor maestro.

Puede conectarse a la réplica mediante su nombre de host y una cuenta de usuario válida, igual que haría en un servidor Azure Database for PostgreSQL normal. En un servidor denominado **myreplica** con el nombre de usuario administrador **myadmin**, puede conectarse a la réplica mediante psql:

```
psql -h myreplica.postgres.database.azure.com -U myadmin@myreplica -d postgres
```

Cuando se le solicite, escriba la contraseña de la cuenta de usuario.

## <a name="monitor-replication"></a>Supervisión de la replicación
Azure Database for PostgreSQL proporciona la métrica **Retraso máximo entre réplicas** en Azure Monitor. Esta métrica está disponible únicamente en el servidor maestro. La métrica muestra el retardo en bytes entre el servidor maestro y la réplica con mayor retardo. 

Azure Database for PostgreSQL también proporciona la métrica **Retraso entre réplicas** en Azure Monitor. Esta métrica está disponible únicamente para las réplicas. 

La métrica se calcula a partir de la vista `pg_stat_wal_receiver`:

```SQL
EXTRACT (EPOCH FROM now() - pg_last_xact_replay_timestamp())
```

La métrica de retardo de réplica muestra el tiempo desde la última transacción reproducida. Si no se produce ninguna transacción en el servidor maestro, la métrica refleja este retardo de tiempo.

Establezca una alerta que le informe cuando el retardo de la réplica alcance un valor que no sea aceptable para la carga de trabajo. 

Para más información, consulte el servidor maestro directamente para obtener el retardo de replicación en bytes en todas las réplicas.

En la versión 10 de PostgreSQL:

```SQL
select pg_wal_lsn_diff(pg_current_wal_lsn(), stat.replay_lsn) 
AS total_log_delay_in_bytes from pg_stat_replication;
```

En la versión 9.6 y versiones anteriores de PostgreSQL:

```SQL
select pg_xlog_location_diff(pg_current_xlog_location(), stat.replay_location) 
AS total_log_delay_in_bytes from pg_stat_replication;
```

> [!NOTE]
> Si se reinicia un servidor maestro o una réplica de lectura, el tiempo que tarda en reiniciarse y ponerse al día se reflejará en la métrica de retardo de la réplica.

## <a name="stop-replication"></a>Detención replicación
Puede decidir detener la replicación entre un servidor maestro y una réplica. La acción de detención reinicia la réplica para quitar su configuración de replicación. Una vez que se ha detenido la replicación entre un servidor maestro y una réplica de lectura, la réplica se convierte en un servidor independiente. Los datos del servidor independiente son los datos que estaban disponibles en la réplica en el momento en que se inició el comando para detener la replicación. El servidor independiente no alcanza al servidor maestro.

> [!IMPORTANT]
> Este servidor independiente no puede volver a convertirse en una réplica.
> Asegúrese de que la réplica tenga todos los datos que necesita antes de detener la replicación en una réplica de lectura.

Aprenda a [detener la replicación en una réplica](howto-read-replicas-portal.md).


## <a name="considerations"></a>Consideraciones

En esta sección se resumen las consideraciones sobre la característica de réplica de lectura.

### <a name="prerequisites"></a>Requisitos previos
El parámetro `azure.replication_support` debe establecerse en **RÉPLICA** en el servidor maestro antes de crear una réplica de lectura. Cuando se cambia este parámetro, es necesario reiniciar el servidor para que el cambio surta efecto. El parámetro `azure.replication_support` se aplica únicamente a los niveles De uso General y Optimizado para memoria.

### <a name="new-replicas"></a>Nuevas réplicas
Las réplicas de lectura se crean como nuevos servidores Azure Database for PostgreSQL. Un servidor no puede volver a convertirse en una réplica. Las réplicas de lectura solo se pueden crear en la misma región de Azure que el servidor maestro. No se puede crear una réplica de otra réplica de lectura.

### <a name="replica-configuration"></a>Configuración de réplicas
Las réplicas se crean con la misma configuración de servidor que el servidor maestro. Después de crear una réplica, se pueden cambiar varias configuraciones independientemente del servidor maestro: generación de proceso, núcleos virtuales, almacenamiento y período de retención de copia de seguridad. El plan de tarifa también se puede cambiar de forma independiente, excepto si es con origen o destino en el nivel Básico.

> [!IMPORTANT]
> Antes de actualizar la configuración de un servidor maestro con nuevos valores, actualice la configuración de las réplicas a valores iguales o mayores. Esta acción garantiza que la réplica puede hacer frente a los cambios realizados en el servidor maestro.

PostgreSQL requiere que el valor del parámetro `max_connections` en la réplica de lectura sea mayor o igual que el valor principal; en caso contrario, no se iniciará la réplica. En Azure Database for PostgreSQL, el valor del parámetro `max_connections` se basa en la SKU. Para obtener más información, consulte el artículo de [límites de Azure Database for PostgreSQL](concepts-limits.md). 

Si trata de actualizar los valores del servidor, pero no cumple los límites, recibirá un error.

### <a name="stopped-replicas"></a>Réplicas detenidas
Si decide detener la replicación entre un servidor maestro y una réplica de lectura, esta se reiniciará para aplicar el cambio. La réplica detenida se convierte en un servidor independiente que acepta las lecturas y escrituras. Este servidor independiente no puede volver a convertirse en una réplica.

### <a name="deleted-master-and-standalone-servers"></a>Servidores maestro e independiente eliminados
Cuando se elimina un servidor maestro, todas sus réplicas de lectura se convierten en servidores independientes. Las réplicas se reiniciarán para reflejar este cambio.

## <a name="next-steps"></a>Pasos siguientes
Aprenda a [crear y administrar réplicas de lectura en Azure Portal](howto-read-replicas-portal.md).
