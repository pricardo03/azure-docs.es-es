---
title: 'Réplicas de lectura en Azure Database for PostgreSQL: servidor único'
description: 'Este artículo describe la característica de réplica de lectura en Azure Database for PostgreSQL: servidor único.'
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 06/05/2019
ms.openlocfilehash: 75a3c8a9912fe9ace70e411983996167da755128
ms.sourcegitcommit: 4cdd4b65ddbd3261967cdcd6bc4adf46b4b49b01
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/06/2019
ms.locfileid: "66734648"
---
# <a name="read-replicas-in-azure-database-for-postgresql---single-server"></a>Réplicas de lectura en Azure Database for PostgreSQL: servidor único

La característica de réplica de lectura permite replicar datos de un servidor Azure Database for PostgreSQL en un servidor de solo lectura. Puede replicar desde el servidor maestro para hasta cinco réplicas. Las réplicas se actualizan de manera asincrónica mediante la tecnología de replicación nativa del motor de PostgreSQL.

> [!IMPORTANT]
> Puede crear una réplica de lectura en la misma región que el servidor maestro o en cualquier otra región de Azure de su elección. La replicación entre regiones está actualmente en versión preliminar pública.

Las réplicas son nuevos servidores que se administran de forma similar a los servidores de Azure Database for PostgreSQL normales. En cada réplica de lectura, se le cobra por el proceso aprovisionado en núcleos virtuales y el almacenamiento aprovisionado en GB/mes.

Vea cómo [crear y administrar réplicas](howto-read-replicas-portal.md).

## <a name="when-to-use-a-read-replica"></a>Casos en los que utilizar las réplicas de lectura
La finalidad de la característica de réplica de lectura es ayudar a mejorar el rendimiento y la escala de las cargas de trabajo que conllevan un gran número de operaciones de lectura. Las cargas de trabajo de lectura se pueden aislar en las réplicas, mientras que las cargas de trabajo de escritura se pueden dirigir al servidor maestro.

Un caso habitual es que las cargas de trabajo de análisis e inteligencia empresarial utilicen la réplica de lectura como origen de datos para los informes.

Dado que las réplicas son de solo lectura, no reducen directamente las cargas de capacidad de escritura en el servidor maestro. Esta característica no está destinada a cargas de trabajo intensivas de escritura.

Esta característica de réplica de lectura utiliza la replicación asincrónica nativa de PostgreSQL. La característica no está diseñada para escenarios de replicación sincrónica. Habrá un retraso medible entre el servidor maestro y la réplica. Los datos de la réplica se vuelven finalmente coherentes con los datos del servidor maestro. Use esta característica con cargas de trabajo que puedan admitir este retraso.

Las réplicas de lectura pueden mejorar el plan de recuperación ante desastres. Primero deberá tener una réplica en otra región de Azure del servidor maestro. Si se produce un desastre de región, puede detener la replicación en la réplica y redirigir la carga de trabajo a ella. Al detener la replicación permite a la réplica comenzar a Aceptar operaciones de escritura, así como lecturas. Obtenga más información en el [detener la replicación](#stop-replication) sección. 

## <a name="create-a-replica"></a>Creación de una réplica
El servidor maestro debe tener el `azure.replication_support` establecido en **RÉPLICA**. Cuando se cambia este parámetro, es necesario reiniciar el servidor para que el cambio surta efecto. Este parámetro `azure.replication_support` se aplica únicamente a los niveles De uso General y Optimizado para memoria.

Cuando se inicia el flujo de trabajo de creación de la réplica, se crea un servidor Azure Database for PostgreSQL en blanco. El nuevo servidor se rellena con los datos que estaban en el servidor maestro. El tiempo que se tarda en crear la réplica depende de la cantidad de datos en el servidor maestro y del tiempo desde la última copia de seguridad completa semanal. Puede oscilar desde unos minutos hasta varias horas.

Cada réplica está habilitada para el almacenamiento [crecimiento automático](concepts-pricing-tiers.md#storage-auto-grow). La característica de crecimiento permite a la réplica a mantenerse al día con los datos replicados a él y evitar una interrupción en la replicación causada por errores de almacenamiento insuficiente.

La característica de réplica de lectura usa la replicación física de PostgreSQL (replicación no lógica). El modo de funcionamiento predeterminado es la transmisión de la replicación mediante espacios de replicación. Cuando es necesario, se usa el trasvase de registros para actualizarse.

Aprenda a [crear una réplica de lectura en Azure Portal](howto-read-replicas-portal.md).

## <a name="connect-to-a-replica"></a>Conexión a una réplica
Al crear una réplica, no se heredan las reglas de firewall o el punto de conexión de servicio de red virtual del servidor maestro. Estas reglas se deben configurar de forma independiente para la réplica.

La réplica hereda su cuenta de administrador del servidor maestro. Todas las cuentas de usuario existentes en el servidor se replican en las réplicas de lectura. Solo se puede conectar a una réplica de lectura utilizando las cuentas de usuario disponibles en el servidor maestro.

Puede conectarse a la réplica mediante su nombre de host y una cuenta de usuario válida, igual que haría en un servidor Azure Database for PostgreSQL normal. Para un servidor denominado **mi réplica** con el nombre de usuario administrador **myadmin**, puede conectarse a la réplica mediante psql:

```
psql -h myreplica.postgres.database.azure.com -U myadmin@myreplica -d postgres
```

Cuando se le solicite, escriba la contraseña de la cuenta de usuario.

## <a name="monitor-replication"></a>Supervisión de la replicación
Azure Database for PostgreSQL proporciona dos métricas para supervisar la replicación. Las dos métricas son **retraso máximo entre réplicas** y **réplica Lag**. Para obtener información sobre cómo ver estas métricas, vea el **supervisar una réplica** sección de la [lea el artículo de procedimientos de réplica](howto-read-replicas-portal.md).

El **retraso máximo entre réplicas** métrica muestra el intervalo de bytes entre el patrón y la réplica de retraso de la mayoría. Esta métrica está disponible únicamente en el servidor maestro.

El **réplica Lag** métrica muestra el tiempo transcurrido desde la última transacción de reproducir. Si no se produce ninguna transacción en el servidor maestro, la métrica refleja este retardo de tiempo. Esta métrica está disponible para los servidores de réplica solo. Retraso de la réplica se calcula a partir del `pg_stat_wal_receiver` vista:

```SQL
EXTRACT (EPOCH FROM now() - pg_last_xact_replay_timestamp());
```

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

Al detener la replicación, la réplica pierde todos los vínculos a su principal anterior y otras réplicas. No hay ninguna conmutación por error automatizada entre una principal y réplica. 

Aprenda a [detener la replicación en una réplica](howto-read-replicas-portal.md).


## <a name="considerations"></a>Consideraciones

En esta sección se resumen las consideraciones sobre la característica de réplica de lectura.

### <a name="prerequisites"></a>Requisitos previos
El parámetro `azure.replication_support` debe establecerse en **RÉPLICA** en el servidor maestro antes de crear una réplica de lectura. Cuando se cambia este parámetro, es necesario reiniciar el servidor para que el cambio surta efecto. El parámetro `azure.replication_support` se aplica únicamente a los niveles De uso General y Optimizado para memoria.

### <a name="new-replicas"></a>Nuevas réplicas
Las réplicas de lectura se crean como nuevos servidores Azure Database for PostgreSQL. Un servidor no puede volver a convertirse en una réplica. No se puede crear una réplica de otra réplica de lectura.

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
