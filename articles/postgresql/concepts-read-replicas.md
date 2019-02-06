---
title: Réplicas de lectura en Azure Database for PostgreSQL
description: En este artículo se describen las réplicas de lectura en Azure Database for PostgreSQL.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 01/23/2019
ms.openlocfilehash: 9270c3290bd7be0bbb79d30aff8becc04dcfc603
ms.sourcegitcommit: 644de9305293600faf9c7dad951bfeee334f0ba3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/25/2019
ms.locfileid: "54904019"
---
# <a name="read-replicas-in-azure-database-for-postgresql"></a>Réplicas de lectura en Azure Database for PostgreSQL

> [!IMPORTANT]
> La característica de réplica de lectura está en versión preliminar pública.

La característica de réplica de lectura le permite replicar datos de un servidor Azure Database for PostgreSQL (maestro) en hasta cinco servidores de solo lectura (réplicas de lectura) dentro de la misma región de Azure. Las réplicas de lectura se actualizan de manera asincrónica mediante la tecnología de replicación nativa del motor de PostgreSQL.

Las réplicas son nuevos servidores que pueden administrarse de manera parecida a como se hace con los servidores Azure Database for PostgreSQL independientes normales. En cada réplica de lectura, se le cobra por el proceso aprovisionado en núcleos virtuales y el almacenamiento aprovisionado en GB/mes.

## <a name="when-to-use-read-replicas"></a>Casos en los que utilizar las réplicas de lectura
La finalidad de la característica de réplica de lectura es ayudar a mejorar el rendimiento y la escala de cargas de trabajo que conllevan un gran número de operaciones de lectura. Por ejemplo, las cargas de trabajo de lectura se podrían aislar en las réplicas, mientras que las cargas de trabajo de escritura se pueden dirigir al servidor maestro.

Un caso habitual es que las cargas de trabajo de análisis e inteligencia empresarial utilicen la réplica de lectura como origen de datos para los informes.

Dado que las réplicas son de solo lectura, no alivian directamente las cargas de capacidad de escritura en el servidor maestro, por lo que esta característica no está pensada para cargas de trabajo que consuman muchas operaciones de escritura.

La característica de réplica de lectura usa la replicación asincrónica de PostgreSQL y, por tanto, no sirve para escenarios de replicación sincrónica. Habrá un retraso medible entre el servidor maestro y la réplica. Los datos de la réplica se vuelven finalmente coherentes con los datos del servidor maestro. Use esta característica con cargas de trabajo que puedan admitir este retraso.

## <a name="creating-a-replica"></a>Creación de una réplica
El servidor maestro debe tener el parámetro **azure.replication_support** establecido en REPLICA (RÉPLICA). Si cambia este parámetro, será necesario reiniciar el servidor para que el cambio surta efecto. (El parámetro **Azure.replication_support** solo se aplica a los niveles De uso general y Optimizado para memoria).

Cuando se inicia el flujo de trabajo de creación de la réplica, se crea un servidor Azure Database for PostgreSQL en blanco. El nuevo servidor se rellena con los datos que estaban en el servidor maestro. El tiempo que se tarda en crear la réplica depende de la cantidad de datos en el servidor maestro y del tiempo desde la última copia de seguridad completa semanal. Suele oscilar entre unos minutos y unas horas.

La característica de réplica de lectura usa la replicación física de PostgreSQL (replicación no lógica). El modo de funcionamiento predeterminado es la transmisión de la replicación mediante ranuras de replicación. Cuando es necesario, se usa el trasvase de registros para actualizarse.

> [!NOTE]
> Si aún no tiene una alerta de almacenamiento configurada en los servidores, se recomienda que lo haga. De esta manera, cuando un servidor se aproxime a su límite de almacenamiento, se le enviará una notificación, ya que la replicación se verá afectada.

[Aprenda a crear una réplica de lectura en Azure Portal](howto-read-replicas-portal.md).

## <a name="connecting-to-a-replica"></a>Conexión a una réplica
Al crear una réplica, no se heredan las reglas de firewall o el punto de conexión de servicio de red virtual del servidor maestro. Estas reglas se deben configurar de forma independiente para la réplica.

La réplica hereda su cuenta de administrador del servidor maestro. Todas las cuentas de usuario existentes en el servidor se replican en las réplicas de lectura. Solo se puede conectar a una réplica de lectura utilizando las cuentas de usuario disponibles en el servidor maestro.

Puede conectarse a la réplica mediante su nombre de host y una cuenta de usuario válida, igual que haría en un servidor Azure Database for PostgreSQL normal. Por ejemplo, si el nombre del servidor es myreplica y el nombre de usuario administrador es myadmin, puede conectarse a él desde psql de la manera siguiente:

```
psql -h myreplica.postgres.database.azure.com -U myadmin@myreplica -d postgres
```
y, cuando se le solicite, escriba la contraseña de la cuenta de usuario.

## <a name="monitoring-replication"></a>Supervisión de la replicación
Hay una métrica **Max Lag across Replicas** (Retardo máximo entre réplicas) disponible en Azure Monitor. Esta métrica está disponible únicamente en el servidor maestro. La métrica muestra el tiempo de retardo entre el servidor maestro y la réplica con mayor retardo. 

También se proporciona una métrica **Replica Lag** (Retardo de réplica) en Azure Monitor. Esta métrica está disponible únicamente para las réplicas. 

La métrica se calcula a partir de la vista pg_stat_wal_receiver:

```SQL
EXTRACT (EPOCH FROM now() - pg_last_xact_replay_timestamp())
```
Recuerde que la métrica de retardo de réplica muestra el tiempo desde la última transacción reproducida. Si no se produce ninguna transacción en el servidor maestro, la métrica refleja este retardo de tiempo.

Se recomienda establecer una alerta que le informe cuando el retardo de la réplica alcance un valor que no sea aceptable para la carga de trabajo. 

Para más información, consulte el servidor maestro directamente para obtener el retardo de replicación en bytes en todas las réplicas.
En PG 10:
```SQL
select pg_wal_lsn_diff(pg_current_wal_lsn(), stat.replay_lsn) 
AS total_log_delay_in_bytes from pg_stat_replication;
```

En PG 9.6 e inferior:
```SQL
select pg_xlog_location_diff(pg_current_xlog_location(), stat.replay_location) 
AS total_log_delay_in_bytes from pg_stat_replication;
```

> [!NOTE]
> Si se reinicia un servidor maestro o de réplica, el tiempo que tarda en reiniciarse y ponerse al día se reflejará en la métrica de retardo de la réplica.

## <a name="stopping-replication-to-a-replica"></a>Detención de la replicación en una réplica
Puede decidir detener la replicación entre un servidor maestro y una réplica. Como consecuencia, la réplica se reiniciará para quitar su configuración de replicación. Una vez que se ha detenido la replicación entre un servidor maestro y un servidor de réplica, el servidor de réplica se convierte en un servidor independiente. Los datos del servidor independiente son los datos que estaban disponibles en la réplica en el momento en que se inició el comando para detener la replicación. Este servidor independiente no se pone al día con el servidor maestro.

Este servidor no puede volver a convertirse en una réplica.

Asegúrese de que la réplica tenga todos los datos que necesita antes de detener la replicación.

Para [saber cómo detener una réplica, consulte la documentación de procedimientos](howto-read-replicas-portal.md).


## <a name="considerations"></a>Consideraciones

### <a name="preparing-for-replica"></a>Preparación para la réplica
**azure.replication_support** se debe establecer en REPLICA (RÉPLICA) en el servidor maestro antes de crear una réplica. Si cambia este parámetro, será necesario reiniciar el servidor para que el cambio surta efecto. Este parámetro se aplica únicamente a los niveles De uso General y Optimizado para memoria.

### <a name="stopped-replicas"></a>Réplicas detenidas
Si decide detener la replicación entre un servidor maestro y una réplica, la réplica se reiniciará para aplicar estos cambios. Después, no se puede convertir de nuevo en una réplica.

### <a name="replicas-are-new-servers"></a>Las réplicas son servidores nuevos
Las réplicas se crean como nuevos servidores Azure Database for PostgreSQL. Los servidores existentes no pueden convertirse en réplicas.

### <a name="replica-server-configuration"></a>Configuración de los servidores de réplica
Los servidores de réplica se crean utilizando las mismas opciones de configuración de servidor que el servidor maestro, que incluye las siguientes:
- Plan de tarifa
- Generación de procesos
- Núcleos virtuales
- Storage
- Período de retención de copia de seguridad
- Opción de redundancia de copia de seguridad
- Versión del motor de PostgreSQL

Después de crear una réplica, se puede cambiar el plan de tarifa (excepto hacia Basic y desde Basic), la generación de proceso, los núcleos virtuales, el almacenamiento y el período de retención de copia de seguridad de forma independiente del servidor maestro.

> [!IMPORTANT]
> Antes de actualizar la configuración de un servidor maestro con nuevos valores, se debe actualizar la configuración de las réplicas a valores iguales o mayores. De esta forma, se tiene la garantía de que las réplicas están al día con los cambios realizados en el servidor maestro.

En concreto, Postgres requiere que el valor del servidor de réplica del parámetro max_connections sea mayor o igual que el valor del servidor maestro; si no, la réplica no se iniciará. En Azure Database for PostgreSQL, el valor de max_connections se establece en función de la SKU. Para más información, lea el [documento de límites](concepts-limits.md). 

Si intenta realizar una actualización que infrinja esta regla, se producirá un error.


### <a name="deleting-the-master"></a>Eliminación del servidor maestro
Cuando se elimina un servidor maestro, todas sus réplicas de lectura se convierten en servidores independientes. Las réplicas se reiniciarán para reflejar este cambio.

### <a name="other"></a>Otros
- Las réplicas de lectura solo se pueden crear en la misma región de Azure que el servidor maestro.
- No permite crear réplicas de réplicas.

## <a name="next-steps"></a>Pasos siguientes
- [How to create and manage read replicas in the Azure portal](howto-read-replicas-portal.md) (Creación y administración de réplicas de lectura en Azure Portal).
